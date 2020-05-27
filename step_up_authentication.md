---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: mobile foundation security, authentication, using challenge handlers

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}

# Step up authentication
{: #step_up_authentication}

Resources can be protected by several security checks. In such a scenario, the {{site.data.keyword.mobilefoundation_short}} Server sends all the relevant challenges simultaneously to the application.
{: shortdesc}

A security check can also depend on another security check. Therefore, it is important to be able to control when the challenges are sent.

For example, this tutorial describes an application that has two resources that are protected by a username and password, where the second resource also requires an extra PIN code.

## Referencing a Security Check
{: #referencing-a-security-check}

Create two security checks: `StepUpPinCode` and `StepUpUserLogin`.

In this example, `StepUpPinCode` depends on `StepUpUserLogin`. The user should be asked to enter a PIN code only after a successful login to `StepUpUserLogin`. For this purpose, `StepUpPinCode` must be able to reference the `StepUpUserLogin` class.

The {{site.data.keyword.mobilefoundation_short}} framework provides an annotation to inject a reference.

In your `StepUpPinCode` class, at the class level, add:

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

Both security check implementations need to be bundled inside the same adapter.
{: important}

To resolve this reference, the framework looks up for a security check with the appropriate class, and injects its reference into the dependent security check.

If there is more than one security check of the same class, the annotation has an optional name parameter, which you can use to specify the unique name of the referred check.

## State machine
{: #state-machine}

All classes that extend `CredentialsValidationSecurityCheck` (which includes both `StepUpPinCode` and `StepUpUserLogin`) inherit a simple state machine. At any moment, the security check can be in one of these states:
- `STATE_ATTEMPTING`: A challenge has been sent and the security check is waiting for the client response. The attempt count is maintained during this state.
- `STATE_SUCCESS`: The credentials have been validated.
- `STATE_BLOCKED`: The maximum number of attempts has been reached and the check is in locked state.

The current state can be obtained by using the inherited `getState()` method.

In `StepUpUserLogin`, add a convenience method to check whether the user is logged-in. This method is used later in the tutorial.

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## The Authorize Method
{: #the-authorize-method}

The `SecurityCheck` interface defines a method that is called `authorize`. This method is responsible for implementing the main logic of the security check, such as sending a challenge or validating the request.

The class `CredentialsValidationSecurityCheck`, which `StepUpPinCode` extends, already includes an implementation for this method. However, in this case, the goal is to check the state of StepUpUserLogin before starting the default behavior of the `authorize` method.

To do so, **override** the `authorize` method:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

This implementation checks the current state of the `StepUpUserLogin` reference:
- If the state is `STATE_SUCCESS` (the user is logged in), the normal flow of the security check continues.
- If `StepUpUserLogin` is in any other state, nothing is done: no challenge is sent, neither success nor failure.

Assuming the resource is protected by **both** `StepUpPinCode` and `StepUpUserLogin`, this flow makes sure that the user is logged in before being prompted for the secondary credential (PIN code). The client never receives both challenges at the same time, even though both security checks are activated.

Alternatively, if the resource is protected **only** by `StepUpPinCode` (the framework activates only this security check), you can change the `authorize` implementation to trigger `StepUpUserLogin` manually:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        //If StepUpUserLogin is successful, continue the normal processing of StepUpPinCode
        super.authorize(scope, credentials, request, response);
    } else {
        //In any other case, process StepUpUserLogin instead.
        userLogin.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

## Retrieve current user
{: #retrieve-current-user}

In the `StepUpPinCode` security check, you are interested in knowing the current user’s ID so that you can look up this user’s PIN code in some database.

In the `StepUpUserLogin` security check, add the following method to obtain the current user from the **authorization context**:

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

In `StepUpPinCode`, you can then use the `userLogin.getUser()` method to get the current user from the `StepUpUserLogin` security check, and check the valid PIN code for this specific user:

```java
@Override
protected boolean validateCredentials(Map<String, Object> credentials) {
    //Get the correct PIN code from the database
    User user = userManager.getUser(userLogin.getUser().getId());

    if(credentials!=null && credentials.containsKey(PINCODE_FIELD)){
        String pinCode = credentials.get(PINCODE_FIELD).toString();

        if(pinCode.equals(user.getPinCode())){
            errorMsg = null;
            return true;
        }
        else{
            errorMsg = "Wrong credentials. Hint: " + user.getPinCode();
        }
    }
    return false;
}
```
{: codeblock}

## Challenge Handlers
{: #challenge-handlers}

On the client side, there are no special APIs to handle multiple steps. Rather, each challenge handler handles its own challenge. In this example, you must register two separate challenge handlers: one to handle challenges from `StepUpUserLogin` and one to handle challenges from `StepUpPincode`.
