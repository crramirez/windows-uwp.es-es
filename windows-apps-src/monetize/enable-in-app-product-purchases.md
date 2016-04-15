---
Description: Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.
title: Habilitar compras de productos desde la aplicación
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: oferta desde la aplicación
keywords: compra desde la aplicación
keywords: producto desde la aplicación
keywords: cómo ofrecer soporte técnico en la aplicación
keywords: muestra de código de compra desde la aplicación
keywords: muestra de código de oferta desde la aplicación
---

# Habilitar compras de productos desde la aplicación

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.

**Nota** Los productos desde la aplicación no pueden ofrecerse durante la versión de prueba de una aplicación. Los clientes que usan una versión de prueba de la aplicación solamente pueden comprar un producto desde la aplicación si compran una versión completa de la misma.

## Requisitos previos

-   Una aplicación de Windows en la que se puedan agregar características que los clientes pueden comprar.
-   Cuando codificas y pruebas nuevos productos desde la aplicación por primera vez, debes usar el objeto [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) en lugar del objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). De esta manera, puedes comprobar la lógica de la licencia con llamadas simuladas al servidor de licencias, en lugar de llamar al servidor activo. Para ello, debes personalizar el archivo llamado "WindowsStoreProxy.xml" en %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. El simulador de Microsoft Visual Studio crea este archivo la primera vez que ejecutas la aplicación, aunque también puedes cargar un archivo personalizado en tiempo de ejecución. Para obtener más información, consulta [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).
-   Este tema también hace referencia a los ejemplos de código que se proporcionan en la [muestra de la Tienda](http://go.microsoft.com/fwlink/p/?LinkID=627610). Esta muestra es ideal para conseguir experiencia práctica con las diferentes opciones de monetización que se proporcionan para las aplicaciones para la Plataforma universal de Windows (UWP).

## Paso 1: Inicializa la información de licencia para la aplicación

Cuando se esté iniciando la aplicación, obtén el objeto [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) de la aplicación mediante la inicialización de la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) o [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), para habilitar las compras de un producto desde la aplicación.

```CSharp
void AppInit()
{
    // some app initialization functions 

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Paso 2: Agrega las ofertas desde la aplicación

Para cada función que quieras tener disponible a través de un producto desde la aplicación, crea una oferta y agrégala a la aplicación.

**Importante** Debes agregar todos los productos desde la aplicación que quieras presentar a tus clientes antes de enviarlos a la Tienda. Si deseas agregar nuevos productos en la aplicación más adelante, tendrás que actualizar la aplicación y enviar una nueva versión.

1.  **Crea un token de oferta desde la aplicación**

    En la aplicación, puedes identificar cada producto desde la aplicación con un token. Este token es una cadena que debes definir y usar en la aplicación y en la Tienda para identificar un producto concreto desde la aplicación. Debes darle un nombre único y significativo (en la aplicación) para que puedas identificar rápidamente la función correcta que representa mientras la estás codificando. Estos son algunos ejemplos de nombres:

    -   "SpaceMissionLevel4"
    -   "ContosoCloudSave"
    -   "RainbowThemePack".

2.  **Codifica la característica en un bloqueo condicional**

    Debes establecer el código de cada función asociada a un producto desde la aplicación en un bloqueo condicional; así podrás comprobar si el cliente tiene una licencia para usar esa función.

    En este ejemplo se muestra cómo puedes codificar una función de producto llamada **featureName** en un bloqueo condicional específico de una licencia. La cadena **featureName**, es el token que identifica este producto de manera única en la aplicación. Se usa también para identificarlo en la Tienda.

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive) 
        {
            // the customer can access this feature
        } 
        else
        {
            // the customer can' t access this feature
        }
    ```

3.  **Agrega la interfaz de usuario de compra para esta característica**

    La aplicación también debe proporcionar una manera para que los clientes puedan comprar el producto o la función incluida en el producto desde la aplicación. No pueden obtenerlos a través de la Tienda de la misma manera que obtuvieron la aplicación completa.

    A continuación te mostramos cómo comprobar si el cliente ya tiene un producto desde la aplicación y, si no la tiene, cómo mostrar el cuadro de diálogo de compra para que puedan comprarla. Reemplaza el comentario "mostrar el cuadro de diálogo de compra" por el código personalizado del cuadro de diálogo de compra (como, por ejemplo, una página con un alegre botón que diga: “¡Compra esta aplicación!” ).

    ```    CSharp
    void BuyFeature1() {
            if (!licenseInformation.ProductLicenses["featureName"].IsActive)
            {
                try
                    {
                    // The customer doesn't own this feature, so 
                    // show the purchase dialog.
                                    
                    await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);
                    //Check the license state to determine if the in-app purchase was successful.
                }
                catch (Exception)
                {
                    // The in-app purchase was not completed because 
                    // an error occurred.
                }
            } 
            else
            {
                // The customer already owns this feature.
            }
        }
    ```

## Paso 3: Cambia el código de prueba para las llamadas finales

Este es un paso fácil: cambia cada referencia a [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) por [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) en el código de la aplicación. Ya no es necesario que proporciones el archivo WindowsStoreProxy.xml, por lo tanto, elimínalo de la ruta de acceso de la aplicación (aunque es posible que quieras guardarlo como referencia cuando configures la oferta desde la aplicación en el próximo paso).

## Paso 4: Configura la oferta del producto desde la aplicación en la Tienda

En el panel del Centro de desarrollo, define el identificador de producto, el tipo, el precio y otras propiedades del producto desde la aplicación. Asegúrate de que la configuración es idéntica a la configuración que estableciste en WindowsStoreProxy.xml durante las pruebas. Para más información, consulta [IAP submissions](https://msdn.microsoft.com/library/windows/apps/mt148551).

## Comentarios

Si te interesa que los clientes cuenten con varios productos consumibles desde la aplicación (elementos que se puedan comprar, usar y después volver a comprar si se desea), consulta el tema [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md).

Si necesitas usar recibos para comprobar que el usuario ha realizado una compra desde la aplicación, asegúrate de revisar [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md).

## Temas relacionados


* [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)
* [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md)
* [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md)
* [Muestra de la Tienda (muestra pruebas y compras desde la aplicación)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
 

 






<!--HONumber=Mar16_HO1-->


