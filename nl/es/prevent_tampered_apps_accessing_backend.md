---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, restrict backend access, tampered apps

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

# Impedir que las apps manipuladas accedan al programa de fondo
{: #prevent_tampered_apps_accessing_backend}

La autenticidad de la aplicación ayuda a comprobar la validez de la aplicación antes de habilitar cualquier servicio, impidiendo así que la aplicación manipulada acceda a los servicios de fondo.
{: shortdesc}

Para proteger adecuadamente la aplicación, habilite la comprobación de seguridad de autenticidad de aplicación predefinida de MobileFirst (``appAuthenticity``). Cuando esté habilitada, la verificación valida la autenticidad de la aplicación antes de proporcionarle un servicio. Las aplicaciones en un entorno de producción deberían tener habilitada esta característica.

Para habilitar la autenticidad de aplicación, puede seguir las instrucciones en pantalla en la **consola de operaciones de MobileFirst → [su aplicación] → Autenticidad**, o revisar la información siguiente.

<dl>
  <dt>Disponibilidad</dt>
  <dd>La autenticidad de aplicación está disponible en todas las plataformas soportadas (iOS, Android, Windows 8.1 Universal, Windows 10 UWP) en las aplicaciones nativas y en Cordova.</dd>
  <dt>Limitaciones</dt>
  <dd>La autenticidad de aplicación no da soporte a **Bitcode** en iOS. Por lo tanto, antes de utilizar la autenticidad de aplicación, inhabilite Bitcode en las propiedades de proyecto de Xcode.</dd>
</dl>

## Flujo de la autenticidad de aplicación
{: #appauthenticityflow}

La comprobación de seguridad de autenticidad de aplicación se ejecuta durante el registro de la aplicación en MobileFirst Server, que se produce la primera vez que una instancia de la aplicación intenta conectarse al servidor. De forma predeterminada, la verificación de autenticidad no se ejecuta de nuevo.

Una vez que se ha habilitado la autenticidad de app, si el cliente debe realizar cambios en la aplicación, la versión de la aplicación se debe actualizar.

Consulte [Configuración de la autenticidad de aplicación](#configappauthenticity) para saber cómo personalizar este comportamiento.

### Habilitación de la autenticidad de aplicación
{: #enableappauthenticity}

Para que la autenticidad de aplicación se habilite en su aplicación:

1. Abra la Consola de operaciones de MobileFirst en su navegador favorito.
2. Seleccione la aplicación de la barra lateral de navegación y pulse el elemento de menú **Autenticidad**.
3. Conmute el botón **Activado/desactivado** en el recuadro **Estado**.

![Habilitación de la autenticidad de aplicación](/images/enable_application_authenticity.png)

El servidor de MobileFirst valida la autenticidad de aplicación en el primer intento de conectarse con el servidor. Para aplicar también esta validación a los recursos protegidos, añada la comprobación de seguridad appAuthenticity al ámbito de protección.

### Inhabilitación de la autenticidad de aplicación
{: #disableappauthenticity}

Es posible que algunas modificaciones en la aplicación durante el desarrollo causen fallos en la validación de la autenticidad. Por consiguiente, se recomienda inhabilitar la autenticidad de aplicación durante el proceso de desarrollo. Las aplicaciones en un entorno de producción deberían tener habilitada esta característica.

Para inhabilitar la autenticidad de aplicación, vuelva a conmutar el botón **Activado/desactivado** en el recuadro **Estado**.

## Configuración de la autenticidad de aplicación
{: #configappauthenticity}

De forma predeterminada, la autenticidad de aplicación se comprueba solo durante el registro del cliente. Sin embargo, igual que con cualquier otra comprobación de seguridad, puede decidir si desea proteger la aplicación o los recursos con la comprobación de seguridad ``appAuthenticity`` en la consola, siguiendo las instrucciones de Protección de recursos.

Puede configurar la comprobación de seguridad de autenticidad de aplicación predefinida con las propiedades siguientes:

* ``expirationSec``: Tiene como valor predeterminado 3600 segundos / 1 hora. Defina la duración hasta que la señal de autenticidad caduca.

Después de que se haya completado la verificación de autenticidad, no se vuelve a producir hasta que se caduca la señal en función del valor definido.

Para configurar la propiedad ``expirationSec``:

1. Cargue la consola de operaciones de MobileFirst, vaya a **[su aplicación] → Seguridad → Configuraciones de comprobación de seguridad** y pulse **Nueva**.
2. Busque el elemento de ámbito ``appAuthenticity``.
3. Defina un nuevo valor en segundos.

    ![Configuración del vencimiento en número de segundos](/images/configuring_expirationSec.png)

## Build Time Secret (BTS)
{: #buildtimesecret}

BTS (Build Time Secret) es una **herramienta opcional para mejorar la validación de autenticidad** solo para las aplicaciones de iOS. La herramienta inyecta la aplicación con un secreto determinado en el momento de la compilación, que se utiliza más adelante en el proceso de validación de autenticidad.

La herramienta BTS se puede descargar desde la **Consola de operaciones de MobileFirst → Centro de descargas**.

Para utilizar la herramienta BTS en Xcode:

1. En el separador **Fases de compilación** pulse el botón **+** y cree una nueva **Fase de script de ejecución**.
2. Copie la vía de acceso de la herramienta BTS y péguela en la nueva "Fase de script de ejecución" que ha creado.
3. Arrastre la fase de script de ejecución sobre la **Fase de compilación de orígenes**.

La herramienta se debe utilizar cuando se cree una versión de producción de la aplicación.

## Resolución de problemas de autenticidad de aplicación
{: #troubleshooting-app-authenticity}

### Restablecer
{: #trblreset}

El algoritmo de la autenticidad de aplicación utiliza los datos y metadatos de aplicación para la validación. El primer dispositivo que se conecta al servidor tras habilitar la autenticidad de aplicación proporciona una "huella dactilar" de la aplicación, que contiene algunos de estos datos.

Es posible restablecer esta huella y proporcionar nuevos datos al algoritmo. Esto puede resultar de utilidad durante el desarrollo (por ejemplo, después de modificar la aplicación en Xcode). Para restablecer la huella, utilice el mandato **reset** en la CLI mfpadm.

Después de restablecer la huella, la comprobación de seguridad appAuthenticity continua trabajando como lo hacía al principio (esto será completamente transparente para el usuario).

### Tipos de validación
{: #trblvalidationtypes}

Mobile First Platform Foundation proporciona autenticidad de app estática y dinámica para las aplicaciones. Estos tipos de validación difieren en el algoritmo y los atributos que se utilizan para generar los elementos de provisión de autenticación de app. De forma predeterminada, cuando la autenticidad de aplicación está habilitada, utiliza el algoritmo de validación dinámico. Ambos tipos de validación garantizan la seguridad de la aplicación. La autenticidad de app dinámica utiliza validaciones y comprobaciones estrictas para la autenticidad de app. Para la autenticidad de app estática, utilizamos un algoritmo un poco más flexible, que no utilizará todas las comprobaciones de validación como se utiliza con la autenticidad de app dinámica.

La autenticidad de app dinámica se puede configurar desde la consola de MobileFirst. El algoritmo interno se ocupa de generar los datos de autenticidad de app según las opciones que se han elegido en la consola. Para la autenticidad estática de app, se necesita utilizar la CLI mfpadm.

Para habilitar la autenticidad estática de app y alternar entre tipos de validación, utilice la CLI mfpadm:

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE puede ser dinámico o estático.
