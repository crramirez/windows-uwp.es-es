---
author: mcleanbyron Description: Si permites que los clientes usen la aplicación de forma gratuita durante un período de prueba, puedes animarles a actualizar a la versión completa de tu aplicación excluyendo o limitando algunas características durante el período de prueba.
título: Excluye o limita características en una versión de prueba ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056 keywords: prueba gratuita keywords: período de prueba gratuita keywords: ejemplo de código de la prueba gratuita keywords: muestra de código de la prueba gratuita
---

# Excluye o limita características en una versión de prueba


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Si permites que los clientes puedan usar la aplicación gratis durante un período de prueba, puedes animarles a actualizar a la versión completa de tu aplicación excluyendo o limitando algunas características durante el período de prueba. Determina las funciones que quieres restringir antes de empezar a codificar y luego asegúrate de que tu aplicación solo permita que funcionen una vez comprada la licencia completa. Asimismo, también puedes habilitar características tales como banners o marcas de agua, para que solo se muestren durante la prueba, antes de que el cliente compre la aplicación.

Veamos cómo puedes agregar esto a tu aplicación.

## Requisitos previos

Una aplicación de Windows en la que se puedan agregar características que los clientes pueden comprar.

## Paso 1: Elige las características que deseas habilitar o deshabilitar durante el período de prueba

El estado de licencia de la aplicación se almacena como propiedad de la clase [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157). Por lo general, tendrás que incluir las funciones que dependen del estado de la licencia en un bloque condicional, tal y como describimos en el siguiente paso. Cuando uses estas características, asegúrate de implementarlas de manera que funcionen en todos los estados de la licencia.

Además, debes decidir cómo quieres tratar los cambios en la licencia de la aplicación cuando se está ejecutando la aplicación. La aplicación de prueba puede tener todas las características, pero también puede incorporar anuncios publicitarios que la versión de pago no tiene. O bien, la aplicación de prueba puede tener determinadas características deshabilitadas o mostrar mensajes regulares que indiquen al usuario que compre la aplicación.

Piensa en el tipo de aplicación que estás haciendo y cuál sería la mejor estrategia de prueba o expiración. Para la versión de prueba de un juego, una buena estrategia es limitar la cantidad de contenido del juego que puede utilizar el usuario. Para la versión de prueba de una utilidad, te recomendamos que establezcas una fecha de caducidad o que limites las características que podría usar un posible comprador.

Para la mayoría de las aplicaciones que no sean juegos, definir una fecha de expiración funciona bien, porque los usuarios pueden familiarizarse con la aplicación completa. Estos son algunos escenarios de expiración habituales y las opciones para tratarlos.

-   **La licencia de prueba expira cuando se está ejecutando la aplicación**

    Si la prueba expira mientras se está ejecutando la aplicación, esta puede:

    -   No hacer nada.
    -   Mostrar un mensaje al cliente.
    -   Cerrarse.
    -   Pedir al cliente que compre la aplicación.

    El procedimiento recomendado es mostrar un mensaje pidiendo que se compre la aplicación y, si el cliente la compra, permitirle usarla con todas las características habilitadas. Si el usuario decide no comprar la aplicación, ciérrela o recuérdeles que compren la aplicación a intervalos regulares.

-   **La licencia de prueba expira antes de que se inicie la aplicación**

    Si la prueba expira antes de que el usuario inicie la aplicación, esta no se iniciará. En lugar de ello, aparecerá un cuadro de diálogo y los usuarios tendrán la posibilidad de comprar tu aplicación en la Tienda Windows.

-   **El cliente compra la aplicación mientras se está ejecutando**

    Si el cliente compra la aplicación mientras la está ejecutando, estas son algunas de las acciones que la aplicación puede realizar.

    -   No hacer nada y dejar que el cliente siga en el modo de prueba hasta que reinicie la aplicación.
    -   Darle las gracias por la compra o mostrar un mensaje.
    -   Habilitar de forma silenciosa las características que están disponibles con una licencia completa (o deshabilitar los avisos exclusivos de la prueba).

Si quieres detectar el cambio de licencia y realizar alguna acción en la aplicación, debes agregar un controlador de eventos como se describe en el siguiente paso.
## Paso 2: Inicializar la información de licencia

Cuando se esté inicializando la aplicación, obtén el objeto [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) para la aplicación, tal y como se muestra en este ejemplo. Se supone que **licenseInformation** es una variable global o un campo de tipo **LicenseInformation**.

Inicializa la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) o [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) para obtener acceso a la información de licencia de la aplicación.

```CSharp
void initializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;
      
}
```

Agrega un controlador de eventos para recibir notificaciones cuando cambie la licencia mientras se esté ejecutando la aplicación. La licencia de la aplicación puede cambiar si expira el período de prueba o si el cliente compra la aplicación a través de una Tienda, por ejemplo.

```CSharp
void InitializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // Register for the license state change event.
     licenseInformation.LicenseChanged += new LicenseChangedEventHandler(licenseChangedEventHandler);

}

// ...

void licenseChangedEventHandler()
{
    ReloadLicense(); // code is in next steps
}
```

## Paso 3: Codificar las características en un bloque condicional

Cuando se genere el evento de cambio de licencia, la aplicación debe llamar a la API de licencia para determinar si el estado de prueba se ha modificado. En el código de este paso se muestra cómo estructurar el controlador para este evento. En este punto, si el usuario compró la aplicación, se recomienda proporcionar información al usuario sobre los cambios de estado de licencia. Es posible que necesites pedirle al usuario que reinicie la aplicación, si así la has codificado. Esta transición debe ser lo más sencilla y fácil posible.

En este ejemplo se muestra cómo evaluar el estado de licencia de una aplicación para que puedas habilitar o deshabilitar una característica de la aplicación adecuadamente.

```CSharp
void ReloadLicense()
{
    if (licenseInformation.IsActive)
    {
         if (licenseInformation.IsTrial)
         {
             // Show the features that are available during trial only.
         }
         else
         {
             // Show the features that are available only with a full license.
         }
     }
     else
     {
         // A license is inactive only when there' s an error.
     }
}
```

## Paso 4: Obtener la fecha de expiración de la prueba de una aplicación

Incluye el código para determinar la fecha de vencimiento de la prueba de la aplicación.

El código de este ejemplo define una función para obtener la fecha de expiración de la licencia de prueba de la aplicación. Si la licencia sigue siendo válida, muestra la fecha de expiración con el número de días que quedan para que expire la prueba.

```CSharp
void DisplayTrialVersionExpirationTime()
{
    if (licenseInformation.IsActive)
    {
        if (licenseInformation.IsTrial)
        {
            var longDateFormat = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longdate");
                                                
            // Display the expiration date using the DateTimeFormatter. 
            // For example, longDateFormat.Format(licenseInformation.ExpirationDate)

            var daysRemaining = (licenseInformation.ExpirationDate - DateTime.Now).Days;

            // Let the user know the number of days remaining before the feature expires
        }
        else
        {
            // ...
        }
    }
    else
    {
       // ...
    }
}
```

## Paso 5: Probar las características con llamadas simuladas a la API de licencia

Ahora, prueba la aplicación con llamadas simuladas al servidor de licencias. En JavaScript, C#, Visual Basic o Visual C++, reemplaza las referencias de [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) por [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) en el código de inicialización de la aplicación.

[
              **CurrentAppSimulator**
            ](https://msdn.microsoft.com/library/windows/apps/hh779766) obtiene información de licencia específica de la versión de prueba de un archivo XML llamado "WindowsStoreProxy.xml", que se encuentra en %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Si esta ruta de acceso y este archivo no existen, deberás crearlos durante la instalación o durante el tiempo de ejecución. Si intentas acceder a la propiedad [**CurrentAppSimulator.LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/hh779768) sin el archivo WindowsStoreProxy.xml en esa ubicación específica, obtendrás un mensaje de error.

En este ejemplo se muestra cómo agregar el código a la aplicación para probarla en diferentes estados de licencia.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

Puedes editar WindowsStoreProxy.xml para cambiar las fechas de expiración simuladas para la aplicación y para sus características. Prueba todas las configuraciones de licencia y expiración posibles para asegurarte de que todo funcione correctamente.

## Paso 6: Reemplaza los métodos API de licencia simulados con los API reales.

Después de probar tu aplicación con el servidor de licencias simulado, y antes de enviar la aplicación a una Tienda para su certificación, reemplaza [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) por [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765), tal y como se muestra en la siguiente muestra de código.

**Importante**  La aplicación debe usar el objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) cuando la envíes a una Tienda, o no logrará la certificación.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    //   licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Paso 7: Describir cómo funciona la prueba gratuita a tus clientes

Explica cómo se comportará la aplicación durante el período de prueba gratuito y después de él, para que no les sorprenda el comportamiento de la aplicación.

Para obtener más información sobre cómo describir tu aplicación, consulta [Crear descripciones de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt148529).

## Temas relacionados

* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [Establecer los precios y la disponibilidad de las aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 






<!--HONumber=May16_HO2-->


