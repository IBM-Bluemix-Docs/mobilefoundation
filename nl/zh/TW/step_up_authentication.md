---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, authentication, using challenge handlers

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# 增強鑑別
{: #step_up_authentication}

可以由數個安全檢查保護資源。在這類情境中，MobileFirst Server 會同時將所有相關的盤查傳送至應用程式。

安全檢查也可以相依於另一個安全檢查。因此，重要的是可以控制何時傳送盤查。

例如，本指導教學所說明的應用程式具有兩個受使用者名稱及密碼保護的資源，其中，第二個資源也需要其他 PIN 碼。

## 參照安全檢查
{: #referencing-a-security-check}

建立兩個安全檢查：`StepUpPinCode` 及 `StepUpUserLogin`。

在此範例中，`StepUpPinCode` 相依於 `StepUpUserLogin`。只有在成功登入 `StepUpUserLogin` 之後，才應該要求使用者輸入 PIN 碼。基於此目的，`StepUpPinCode` 必須可以參照 `StepUpUserLogin` 類別。

MobileFirst 架構提供要注入參考資料的註釋。

在 `StepUpPinCode` 類別中，於類別層次中新增：

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

**重要事項**：需要在相同的配接器內組合兩個安全檢查實作。{.important}

為了解析此參考資料，此架構會查閱適當類別的安全檢查，並將其參考資料注入至相依安全檢查。

如果同一個類別有多個安全檢查，則註釋具有選用名稱參數，可用來指定所參照檢查的唯一名稱。

## 狀態機器
{: #state-machine}

所有擴充 `CredentialsValidationSecurityCheck` 的類別（包括 `StepUpPinCode` 及 `StepUpUserLogin`）都繼承簡單狀態機器。在任何給定的時間，安全檢查都可能處於下列其中一種狀態：

* `STATE_ATTEMPTING`：已傳送盤查，而且安全檢查正在等待用戶端回應。在此狀態期間，會維護嘗試計數。
* `STATE_SUCCESS`：已順利驗證認證。
* `STATE_BLOCKED`：已達嘗試次數上限，而且檢查處於已鎖定狀態。

使用繼承的 `getState()` 方法，可以取得現行狀態。

在 `StepUpUserLogin` 中，新增便利方法來檢查使用者目前是否已登入。稍後在本指導教學中會使用此方法。

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## authorize 方法
{: #the-authorize-method}

`SecurityCheck` 介面定義稱為 `authorize` 的方法。此方法負責實作安全檢查的主要邏輯，例如，傳送盤查或驗證要求。

`StepUpPinCode` 所擴充的類別 `CredentialsValidationSecurityCheck` 已包括此方法的實作。不過，在此情況下，目標是先檢查 StepUpUserLogin 的狀態，再啟動 `authorize` 方法的預設行為。

若要這樣做，會**置換** `authorize` 方法：

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

此實作會檢查 `StepUpUserLogin` 參考資料的現行狀態：

* 如果狀態為 `STATE_SUCCESS`（使用者已登入），則會繼續安全檢查的正常流程。
* 如果 `StepUpUserLogin` 處於任何其他狀態，則不會執行任何動作：不論成功還是失敗，都不會傳送任何盤查。

假設資源受 `StepUpPinCode` 及 `StepUpUserLogin` **兩者**所保護，則此流程先確保使用者已登入，系統再提示輸入次要認證（PIN 碼）。用戶端永遠不會同時接收到兩個盤查，即使啟動兩個安全檢查也是一樣。

或者，如果資源**僅**受 `StepUpPinCode` 所保護（此架構只會啟動這個安全檢查），則您可以變更 `authorize` 實作以手動觸發 `StepUpUserLogin`：

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

## 擷取現行使用者
{: #retrieve-current-user}

在 `StepUpPinCode` 安全檢查中，您會想要知道現行使用者的 ID，以在某個資料庫中查閱此使用者的 PIN 碼。

在 `StepUpUserLogin` 安全檢查中，新增下列方法，以從**授權環境定義**取得現行使用者：

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

在 `StepUpPinCode` 中，您接著可以使用 `userLogin.getUser()` 方法從 `StepUpUserLogin` 安全檢查取得現行使用者，以及檢查此特定使用者的有效 PIN 碼：

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

## 盤查處理程式
{: #challenge-handlers}

在用戶端上，沒有特殊 API 可以處理多個步驟。而是每個盤查處理程式都會處理它自己的盤查。在此範例中，您必須登錄兩個不同的盤查處理程式：一個處理來自 `StepUpUserLogin` 的盤查，另一個則處理來自 `StepUpPincode` 的盤查。
