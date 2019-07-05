---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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


# Autenticación y seguridad
{: #basic_authentication}

La infraestructura de seguridad de MobileFirst se basa en el protocolo [OAuth 2.0](http://oauth.net/). De acuerdo con este protocolo, un recurso puede ser protegido por un **ámbito** que define los permisos requeridos para acceder al recurso. Para acceder a un recurso protegido, el cliente debe proporcionar una **señal de acceso** coincidente que encapsula el ámbito de la autorización que se le otorga al cliente.

El protocolo OAuth separa los roles del servidor de autorización y el servidor de recursos en el que se aloja el recurso.

* El servidor de autorización gestiona la autorización de cliente y la generación del ámbito.
* El servidor de recursos utiliza el servidor de autorización para validar la señal de acceso que proporciona el cliente y para asegurar que coincide con el ámbito de protección del recurso solicitado.

La infraestructura de seguridad se genera en torno al servidor de autorización que implementa el protocolo OAuth y expone los puntos finales de OAuth con los que el cliente interactúa para obtener señales de acceso. La infraestructura de seguridad proporciona los bloques de creación para implementar una lógica de autorización personalizada sobre el servidor de autorización y el protocolo OAuth subyacente. De forma predeterminada, el servidor de MobileFirst funciona también como **servidor de autorización**. Sin embargo, puede configurar un dispositivo de IBM WebSphere DataPower para que actúe como servidor de autorización e interactúe con el servidor de MobileFirst.

A continuación, la aplicación cliente puede utilizar estas señales para acceder a recursos en un **servidor de recursos**, que puede ser el propio servidor de MobileFirst o un servidor externo. El servidor de recursos verifica la validez de la señal para asegurarse de que se le puede otorgar acceso al recurso solicitado al cliente. La separación entre el servidor de recursos y el servidor de autorización impone la seguridad en los recursos que se ejecutan fuera del servidor de MobileFirst.

Los desarrolladores de aplicación protegen acceso a los recursos definiendo el ámbito necesario para cada recurso protegido e implementando comprobaciones de seguridad y manejadores de desafíos. La infraestructura de seguridad de lado del servidor y la API del lado del cliente manejan el intercambio del mensaje OAuth y la interacción con el servidor de autorización de manera transparente, y de esta forma permiten a los desarrolladores centrarse solo en la lógica de autorización.

## Entidades de autorización
{: #acs_authorization_entitiesty}

### Señales de acceso
{: #acs_access_tokens}

Una señal de acceso de MobileFirst es una entidad firmada digitalmente que describe los permisos de autorización de un cliente. Una vez que se otorga la solicitud de autorización del cliente para un ámbito específico y que el cliente está autenticado, el punto final de la señal del servidor de autorización envía al cliente una respuesta HTTP que contiene la señal de acceso solicitada.

#### Estructura de la señal de acceso

La señal de renovación de MobileFirst contiene la siguiente información:

* **ID de cliente**: un identificador único del cliente.
* **Ámbito**: el ámbito al que se otorgó la señal (ver ámbitos de OAuth). Este ámbito no incluye un ámbito de aplicación obligatorio.
* **Hora de vencimiento de la señal**: la hora a partir de la cual la señal se vuelve inválida (caduca), en segundo.

#### Vencimiento de la señal
{: #token-expiration}

La señal de acceso otorgada continua siendo válida hasta que transcurre la hora de vencimiento. La hora de vencimiento de la señal de acceso se establece en la hora de vencimiento más corta de entre las horas de vencimiento de todas las comprobaciones de seguridad del ámbito. Sin embargo, si el período de tiempo hasta la hora de vencimiento más corta es más largo que el período máximo de vencimiento de la señal de la aplicación, la hora de vencimiento de la señal se establece en la hora actual más el período de vencimiento máximo. El período de vencimiento de señal máximo predeterminado (duración de validación) es 3.600 segundos (1 hora), pero puede configurarse estableciendo el valor de la propiedad ``maxTokenExpiration``.

##### Configuración del período de vencimiento de señal de acceso máximo
{: #acs_config-max-access-tokens}

Configure el período de vencimiento de señal de acceso máximo de la aplicación utilizando uno de los métodos alternativos siguientes:

* Utilización de la consola de operaciones MobileFirst
    1. Seleccione el separador **[su aplicación]** → **Seguridad**.
    2. En la sección **Configuración de señal**, establezca el valor del campo **Período de vencimiento de señal máximo (segundos)** en su valor preferido, y pulse **Guardar**. Puede repetir este procedimiento, en cualquier momento, para modificar el período de vencimiento de señal máximo, o seleccionar **Restaurar valores predeterminados** para restaurar el valor predeterminado.

* Edición del archivo de configuración de la aplicación

    1. Desde una CLI, vaya a la carpeta raíz del proyecto y ejecute el mandato siguiente.
      ```bash
      mfpdev app pull
      ```
    2. Abra el archivo de configuración, que se encuentra en la carpeta `[project-folder]\mobilefirst`.
    3. Edite el archivo definiendo la propiedad `maxTokenExpiration` y establezca el valor al período de vencimiento de señal de acceso máximo, en segundos:
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. Despliegue el archivo JSON de configuración actualizando ejecutando el mandato: ``mfpdev app push``.

##### Estructura de la respuesta de la señal de acceso
{: #acs_access-tokens-structure}

Una respuesta HTTP correcta a una solicitud de señal de acceso contiene un objeto JSON con la señal de acceso y datos adicionales. A continuación, se muestra un ejemplo de una respuesta de señal de acceso válida del servidor de autorización:

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

El objeto JSON de respuesta de señal tiene los siguientes objetos de propiedad:

* **token_type**: el tipo de señal siempre es "*Bearer*", de acuerdo con la [especificación OAuth 2.0 Bearer Token Usage](https://tools.ietf.org/html/rfc6750).
* **expires_in**: la hora de vencimiento de la señal de acceso en segundos.
* **access_token**: la señal de acceso generada (las señales de acceso reales son más largas que las que se muestran en el ejemplo).
* **scope**: el ámbito solicitado.

La información **expires_in** y **scope** también se encuentra en la misma señal (**access_token**).

**Nota**: La estructura de una respuesta de señal de acceso válida es relevante si utiliza la clase de nivel bajo `WLAuthorizationManager` y gestiona la interacción OAuth entre el cliente y la autorización y los servidores de recurso usted mismo, o si utiliza un cliente confidencial. Si utiliza la clase de nivel alto `WLResourceRequest`, que encapsula el flujo OAuth para acceder a recursos protegidos, la infraestructura de seguridad maneja el proceso de las respuestas de señal de acceso en su lugar. Consulte [API de seguridad de cliente](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) y [Clientes confidenciales](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).

### Señales de renovación
{: #acs_refresh_tokens}

Una Señal de renovación es un tipo de señal especial, que se puede utilizar para obtener una señal de acceso nueva cuando caduque la señal de acceso. Para solicitar una nueva señal de acceso, se puede presentar una señal de renovación válida. Las señales de renovación son señales de larga duración y seguirán siendo válidas durante un periodo de tiempo más largo en comparación con las señales de acceso.

Una aplicación debe utilizar con cuidado la señal de renovación porque puede permitir a un usuario seguir siempre autenticado. Las aplicaciones de redes sociales, las aplicaciones de comercio electrónico, las aplicaciones de navegación de catálogo de productos y tales aplicaciones de utilidad, donde el proveedor de aplicaciones no autentica a los usuarios regularmente, pueden utilizar las señales de renovación. Las aplicaciones que obligan la autenticación de usuario frecuentemente deben evitar el uso de las señales de renovación.
Señal de renovación de MobileFirst

Una señal de renovación de MobileFirst es una entidad firmada digitalmente como señal de acceso que describe los permisos de autorización de un cliente. La señal de renovación se puede utilizar para obtener una nueva señal de acceso del mismo ámbito. Una vez que se otorga la solicitud de autorización del cliente para un ámbito específico y que el cliente está autenticado, el punto final de la señal del servidor de autorización envía al cliente una respuesta HTTP que contiene la señal de acceso y la señal de renovación solicitadas. Cuando caduca la señal de acceso, el cliente envía una señal de renovación al punto final de señal del servidor de autorización para obtener un nuevo conjunto de señales de acceso y de señales de renovación.

#### Estructura de la señal de renovación
{: #structure_refresh_tokens}

De forma similar a la señal de acceso de MobileFirst, la señal de renovación de MobileFirst contiene la información siguiente:

* **ID de cliente**: un identificador único del cliente.
* **Ámbito**: el ámbito al que se otorgó la señal (ver ámbitos de OAuth). Este ámbito no incluye un ámbito de aplicación obligatorio.
* **Hora de vencimiento de la señal**: la hora a partir de la cual la señal se vuelve inválida (caduca), en segundo.

##### Vencimiento de la señal
{: #str-token-expiration}

El periodo de vencimiento de la señal para la señal de renovación es mayor que el periodo de vencimiento de la señal de acceso típico. La señal de renovación una vez otorgada continúa siendo válida hasta que transcurre la hora de vencimiento. Dentro de este periodo de validez, un cliente puede utilizar la señal de renovación para obtener un conjunto nuevo de señales de acceso y de señales de renovación. La señal de renovación tiene un periodo de vencimiento fijo de 30 días. Cada vez que el cliente recibe un conjunto nuevo de señales de acceso y de señales de renovación correctamente, se restablece el vencimiento de la señal de renovación, dando así al cliente una experiencia de una señal que no vence nunca. Las reglas de vencimiento de la señal de acceso permanecen igual que lo explicado en la sección **Señal de acceso**.

##### Habilitación de la característica de señal de renovación
{: #acs_enable-refresh-token}

La característica de señal de renovación se puede habilitar utilizando las propiedades siguientes en el lado del cliente y del servidor, respectivamente.

Propiedad del lado del cliente (Android):
*Nombre de archivo*: mfpclient.properties
*Nombre de propiedad*: wlEnableRefreshToken
*Valor de propiedad*: true
Por ejemplo,
*wlEnableRefreshToken*=true

Propiedad del lado del cliente (iOS):
*Nombre de archivo*: mfpclient.plist
*Nombre de propiedad*: wlEnableRefreshToken
*Valor de propiedad*: true
Por ejemplo,
*wlEnableRefreshToken*=true

Propiedad del lado del servidor:
*Nombre de archivo*: server.xml
*Nombre de propiedad*: mfp.security.refreshtoken.enabled.apps
*Valor de propiedad*: id de paquete de aplicación separado por ‘;’

Por ejemplo:

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
Utilice ID de paquetes distintos para distintas plataformas.

##### Estructura de respuesta de señal de renovación
{: #refresh-token-response-structure}

A continuación, se muestra un ejemplo de una respuesta de señal de renovación válida del servidor de autorizaciones:

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

La respuesta de la señal de renovación tiene un objeto de propiedad adicional
`refresh_token`, además de los otros objetos de propiedad que se describen como parte de la estructura de la respuesta de la señal de acceso.

**Nota**: Las señales de renovación tienen una duración mayor que las señales de acceso. Por lo tanto, la característica de señal de renovación se debe utilizar con cuidado. Las aplicaciones donde la autenticación de usuario periódica no es necesaria son candidatos ideales para utilizar la característica de señal de renovación.

MobileFirst da soporte a la característica de señal de renovación en iOS a partir de CD Update 3.

#### Comprobaciones de seguridad
{: #acs_securitychecks}

Una comprobación de seguridad es una entidad de lado del servidor que implementa la lógica de seguridad para proteger los recursos de aplicación de lado de servidor. Un ejemplo simple de una comprobación de seguridad es una comprobación de seguridad de inicio de sesión de usuario que recibe las credenciales de un usuario y verifica las credenciales en relación con un registro de usuario. Otro ejemplo es la comprobación de seguridad de autenticidad de aplicación predefinida de MobileFirst, que valida la autenticidad de la aplicación móvil y, por tanto, protege contra los intentos ilegales de acceder a los recursos de la aplicación. También puede utilizarse la misma comprobación de seguridad para proteger varios recursos.

Una comprobación de seguridad normalmente emite desafíos de seguridad que requieren que el cliente responda de forma específica para pasar la comprobación. Este reconocimiento se produce como parte del flujo de adquisición de señal de acceso de OAuth. El cliente utiliza los **manejadores de desafíos** para manejar los desafíos de las comprobaciones de seguridad.

##### Comprobaciones de seguridad incorporadas
{: #builtin-sec-checks}

Están disponibles las siguientes comprobaciones de seguridad predefinidas:

* Autenticidad de aplicación
* Inicio de sesión único (SSO) basado en LTPA
* Direct Update

#### Entidad del manejador de desafío
{: #challengehandler_entity}

Al intentar acceder a un recurso protegido, es posible que el cliente se enfrente a un desafío. Un desafío es una pregunta, una prueba de seguridad o una solicitud del servidor para asegurar que el cliente tiene permiso para acceder al recurso. Más frecuentemente, el desafío es una solicitud de credenciales como, por ejemplo, un nombre de usuario y una contraseña.

Un manejador de desafíos es una entidad del lado del cliente que implementa la lógica de seguridad del lado del cliente y la interacción de usuario relacionada.

**Importante**: Una vez recibido el desafío, no puede ignorarse. Debe responder o cancelarlo. Ignorar un desafío puede provocar comportamientos inesperados.

### Ámbitos
{: #scopes}

Puede proteger recursos, como los adaptadores, de acceso no autorizado asignando un ámbito al recurso.

Un ámbito se define como una serie de uno o más elementos de ámbito separados por espacios ("scopeElement1 scopeElement2..."), o nulo para aplicar el ámbito predeterminado (RegisteredClient). La infraestructura de seguridad de MobileFirst requiere una señal de acceso para cualquier recurso de adaptador, incluso si al recurso no se le asigna un ámbito, a menos que se inhabilite la protección de recursos para el recurso.

#### Elementos de ámbito
{: #scopeelements}

Un elemento de ámbito puede ser cualquiera de los siguientes,

* El nombre de una comprobación de seguridad.
* Una palabra clave arbitraria `access-restricted` o `deletePrivilege` que defina el nivel de seguridad necesario para el recurso. Más adelante, la palabra clave luego se correlaciona con una comprobación de seguridad.

#### Correlación de ámbito
{: #scopemapping}

De forma predeterminada, los **elementos de ámbito** que escribe en el **ámbito** se correlacionan con una
**comprobación de seguridad** con el mismo nombre. Por ejemplo, si escribe una comprobación de seguridad denominada `PinCodeAttempts`, puede utilizar un elemento de ámbito con el mismo nombre en su ámbito.

La correlación de ámbito permite correlacionar elementos de ámbito con comprobaciones de seguridad. Cuando el cliente pide un elemento de ámbito, esta configuración define qué comprobaciones de seguridad es necesario aplicar. Por ejemplo, puede correlacionar el elemento de ámbito `access-restricted` con la comprobación de seguridad `PinCodeAttempts`.

La correlación de ámbito es útil si desea proteger un recurso de una manera diferente en función de la aplicación a la que intenta acceder. También puede correlacionar un ámbito a una lista de comprobaciones de seguridad.

Por ejemplo: scope = `access-restricted deletePrivilege`

* En la app A
    * `access-restricted` se correlaciona con `PinCodeAttempts`.
    * `deletePrivilege` se correlaciona con una cadena vacía.
* En la app B
    * `access-restricted` se correlaciona con `PinCodeAttempts`.
    * `deletePrivilege` se correlaciona con `UserLogin`.

Para correlacionar su elemento de ámbito con una cadena vacía, no seleccione ninguna comprobación de seguridad en el menú emergente **Añadir nueva correlación de elemento de ámbito**.

![Correlación de ámbito](/images/scope_mapping.png "Pantalla Añadir nueva correlación de elemento de ámbito")

También puede editar manualmente el archivo JSON de configuración de la aplicación con la configuración necesaria y volver a enviar por push los cambios a un servidor de MobileFirst.

1. Desde una **ventana de línea de mandatos**, navegue a la carpeta de raíz de proyecto y ejecute `mfpdev app pull`.
2. Abra el archivo de configuración, que se encuentra en la carpeta `[project-folder]\mobilefirst`.
3. Edite el archivo definiendo una propiedad `scopeElementMapping`. En esta propiedad, defina los pares de datos que se componen cada uno del nombre del elemento de ámbito seleccionado y una serie de cero o más comprobaciones de seguridad separadas por espacios con las que se correlacionan los elementos. Por ejemplo:

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. Despliegue el archivo JSON de configuración actualizado ejecutando el mandato siguiente,
  ```bash
  mfpdev app push
  ```

También puede enviar configuraciones actualizadas a servidores remotos. Consulte la guía de aprendizaje Utilización de la CLI de MobileFirst para gestionar los artefactos de MobileFirst.
{: note}

### Protección de recursos
{: #protecting-resources}

En el modelo OAuth, un recurso protegido es un recurso que requiere una señal de acceso. Puede utilizar la infraestructura de seguridad de MobileFirst para proteger los recursos alojados en una instancia del servidor de MobileFirst y los recursos de un servidor externo. Proteja un recurso asignándole un ámbito que defina los permisos requeridos para adquirir una señal de acceso.

Puede proteger los recursos de varias maneras:

#### Ámbito de aplicación obligatorio
{: #mandatoryappscope}

A nivel de aplicación, puede definir un ámbito que se aplicará a todos los recursos utilizados en la aplicación. La infraestructura de seguridad ejecuta estas comprobaciones(si existen) además de las comprobaciones de seguridad del ámbito de recurso solicitado.

**Nota**:
   * El ámbito de aplicación obligatorio no se aplica al acceder a un recurso desprotegido.
   * La señal de acceso que se otorgada para el ámbito de recursos no contiene el ámbito de aplicación obligatorio.

En la Consola de operaciones de MobileFirst, seleccione la aplicación en la sección **Aplicaciones** de la barra lateral de navegación y, a continuación, seleccione el separador **Seguridad**. En **Ámbito de aplicación obligatorio**, seleccione **Añadir a ámbito**.

![Ámbito de aplicación obligatorio](/images/mandatory-application-scope.png "Pantalla Configurar ámbito de aplicación obligatorio")

También puede editar manualmente el archivo JSON de configuración de la aplicación con la configuración necesaria y volver a enviar por push los cambios a un servidor de MobileFirst.

1. Desde una **ventana de línea de mandatos**, navegue a la carpeta de raíz de proyecto y ejecute `mfpdev app pull`.
2. Abra el archivo de configuración, que se encuentra en la carpeta **project-folder\mobilefirst**.
3. Edite el archivo definiendo la propiedad `mandatoryScope` y estableciendo el valor de propiedad a una serie de ámbitos que contenga una lista separada por espacios de los elementos de ámbito seleccionados. Por ejemplo:

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. Despliegue el archivo JSON de configuración actualizando ejecutando el mandato: mfpdev app push.

También puede enviar configuraciones actualizadas a servidores remotos.

#### Protección de recursos de adaptador
{: #protectadapterres}

En el adaptador puede especificar el ámbito de protección para el método Java o un procedimiento de recurso de JavaScript, o para toda una clase de recursos Java. Un ámbito se define como una serie de uno o más elementos de ámbito separados por espacios ("scopeElement1 scopeElement2..."), o nulo para aplicar el ámbito predeterminado. Para obtener más información sobre la protección de los recursos de adaptador, consulte [Protección de adaptadores](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters).

### Inhabilitación de la protección de recurso
{: #disablingresprotection}

Puede inhabilitar la protección de recursos de MobileFirst predeterminada para un recurso de adaptador Java o JavaScript específico o para toda una clase Java, tal y como se indica en las siguientes secciones Java y JavaScript. Cuando la protección de recursos está inhabilitada, la infraestructura de seguridad de MobileFirst no requiere una señal para acceder al recurso.

#### Inhabilitación de la protección OAuth de recurso Java
{: #disablejavaresoauthprotection}

Para inhabilitar una protección OAuth para el método o clase de recurso Java, añada la anotación `@OAuthSecurity` a la declaración de clase o recurso y establezca el valor del elemento `enabled` en `false`:

```
@OAuthSecurity(enabled = false)
```

El valor predeterminado del elemento `enabled` de la anotación es `true`. Cuando el elemento `enabled` se establezca en `false`, el elemento `scope` se ignora y el recurso o clase de recurso queda desprotegido.

**Nota**: Cuando asigne un ámbito a un método de una clase desprotegida, el método se protege pese a la anotación de clase, a menos que establezca el elemento `enabled` de la anotación de recurso en `false`.

Revise los ejemplos siguientes:

El código siguiente inhabilita la protección de recurso de un método `helloUser`:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

El código siguiente inhabilita la protección de un recurso para la clase `MyUnprotectedResources`:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### Inhabilitación de la protección OAuth de recurso JavaScript
{: #disablejavascriptresoauthprotection}

Para inhabilitar por completo la protección OAuth para un recurso de adaptador de JavaScript (procedimiento), en el archivo **adapter.xml**, establezca el atributo `secured` del elemento <procedure> en `false`:

```javascript
<procedure name="procedureName" secured="false">
```

Cuando el atributo `secured` se establece en `false`, el atributo `scope` se ignora y el recurso queda desprotegido.

Revise el ejemplo siguiente:

El código siguiente inhabilita la protección de recurso para un procedimiento `userName`:

```javascript
<procedure name="userName" secured="false">
```

### Definición de recursos no protegidos
{: #defunprotectedresources}

Un recurso desprotegido es un recurso que no requiere una señal de acceso. La infraestructura de seguridad de MobileFirst no gestiona el acceso a recursos no protegidos, y no valida ni comprueba la identidad de los clientes que acceden a estos recursos. Por lo tanto, no se da soporte a las funciones como Direct Update, bloqueo del acceso a un dispositivo o la inhabilitación remota de una aplicación en los recursos desprotegidos.

### Protección de recursos externos
{: #protecextresources}

Para proteger recursos externos, añada un filtro de recursos con un módulo de validación de una señal de acceso a un servidor de recurso externo. El módulo de validación de señales utiliza el punto final de introspección del servidor de autorización de la infraestructura de seguridad para validar las señales de acceso de MobileFirst antes de que el cliente OAuth obtenga acceso a los recursos. Puede utilizar la API REST de MobileFirst para el tiempo de ejecución MobileFirst para crear su propio módulo de validación de señales de acceso para cualquier servidor externo. Como alternativa, puede utilizar una de las extensiones de MobileFirst proporcionadas para proteger los recursos externos Java.
