---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "Obtén información sobre cómo habilitar las pruebas y compras desde la aplicación en las aplicaciones para UWP."
title: "Pruebas y compras desde la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# Pruebas y compras desde la aplicación

Windows SDK proporciona API que puedes usar para agregar las funcionalidades de pruebas y compras desde la aplicación a tu aplicación para la Plataforma universal de Windows (UWP) con el fin de rentabilizar la aplicación y agregar nuevas funcionalidades. Estas API también proporcionan acceso a la información de licencia de la aplicación.

Para estos escenarios, Windows 10 ofrece dos API diferentes:

* Todas las versiones de Windows 10 admiten una API para las compras desde la aplicación y la información de licencia en el espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx).

* A partir de Windows 10, versión 1607, hay una API alternativa para las compras desde la aplicación y la información de licencia en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).  

Aunque las API de estos espacios de nombres tienen los mismos objetivos, se han diseñado de forma bastante diferente y el código no es compatible entre las dos API. Si la aplicación está destinada a Windows 10, versión 1607 o posterior, te recomendamos que uses el espacio de nombres **Windows.Services.Store**. Este espacio de nombres admite los tipos de complemento más recientes, como los complementos de consumibles administrados por la Tienda, y está diseñado para ser compatible con futuros tipos de productos y características compatibles con el Centro de desarrollo de Windows y la Tienda. El espacio de nombres **Windows.Services.Store** también presenta un rendimiento mejorado.

En este artículo se presentan las compras desde la aplicación para aplicaciones para UWP y se proporciona una visión general del espacio de nombres **Windows.Services.Store**, que está disponible a partir de Windows 10, versión 1607. Para obtener información sobre el uso de los miembros en el espacio de nombres **Windows.ApplicationModel.Store**, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).


## Información general sobre las compras desde la aplicación en aplicaciones para UWP

En esta sección se describen los conceptos básicos del funcionamiento de las pruebas y las compras desde la aplicación con las aplicaciones para UWP en la Tienda. La mayoría de estos conceptos se aplican a ambos espacios de nombres **Windows.Services.Store** y **Windows.ApplicationModel.Store**.

Por lo general, todos los elementos que se ofrecen en la Tienda se denominan *producto*. La mayoría de los desarrolladores trabajan con los siguientes tipos de productos: *aplicaciones* y *complementos* (también conocidos como productos desde la aplicación o IAP). Un complemento hace referencia a un producto o una característica que se pone a disposición de los clientes en el contexto de la aplicación. Un complemento puede representar cualquier funcionalidad que la aplicación ofrezca a los clientes: por ejemplo, la moneda que se usará en una aplicación o un juego, los nuevos mapas o armas para un juego, la posibilidad de usar la aplicación sin anuncios o el contenido digital, como música o vídeos, para las aplicaciones capaces de ofrecer ese tipo de contenido.

Cada aplicación y complemento tiene una licencia asociada que indica si el usuario tiene derecho a usar la aplicación o el complemento. Si el usuario tiene derecho a usar la aplicación o el complemento como una versión de prueba, la licencia también proporciona información adicional sobre la versión de prueba.

Para ofrecer un complemento a los clientes en la aplicación, empieza por [definir los complementos para la aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). A continuación, escribe código en tu aplicación para determinar si el usuario tiene una licencia para usar la característica que representa el complemento y ofrece el complemento para su venta al usuario como una compra desde la aplicación, si aún no tiene una licencia para usarlo. Para obtener ejemplos que muestren las tareas relacionadas con el espacio de nombres **Windows.Services.Store** en las aplicaciones destinadas a Windows 10, versión 1607 o posterior, consulta los siguientes artículos:

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)

Para obtener ejemplos que muestren las tareas relacionadas mediante el espacio de nombres **Windows.ApplicationModel.Store**, consulta [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

Todos los desarrolladores pueden crear los siguientes tipos de complementos.

| Tipo de complemento |  Descripción  |
|---------|-------------------|
| Durable  |  Un complemento que persiste durante el ciclo de vida especificado en el [panel del Centro de desarrollo de Windows](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties). <p/><p/>De manera predeterminada, los complementos durables nunca expiran, por lo que solo se pueden adquirir una vez. Si especificas una duración particular para el complemento, el usuario puede volver a comprar el complemento después de su expiración.  |
| Consumible administrado por el desarrollador  |  Un complemento que se puede comprar, usar y volver a comprar. Este tipo de complemento suele usarse para la moneda en la aplicación. <p/><p/>Para este tipo de consumible, eres responsable de realizar un seguimiento del saldo del usuario de los elementos que representa el complemento, así como de notificar la compra del complemento cuando se complete en la Tienda y una vez que el usuario haya consumido todos los elementos. El usuario no puede comprar el complemento otra vez hasta que la aplicación notifique que la compra del complemento anterior se ha completado. <p/><p/>Por ejemplo, si el complemento representa 100 monedas en un juego y el usuario consume 10 monedas, la aplicación o el servicio debe mantener el nuevo saldo restante de 90 monedas para el usuario. Cuando el usuario haya consumido las 100 monedas, la aplicación debe notificar el complemento como completado y, a continuación, el usuario puede volver a comprar el complemento de 100 monedas.    |
| Consumible administrado por la Tienda  |  Un complemento que se puede comprar, usar y volver a comprar. Este tipo de complemento suele usarse para la moneda en la aplicación.<p/><p/>Para este tipo de consumible, la Tienda realiza un seguimiento del saldo del usuario de los elementos que representa el complemento. Cuando el usuario consume algún elemento, debes notificar dichos elementos como completados en la Tienda, tras lo cual la Tienda actualiza el saldo del usuario. La aplicación puede consultar el saldo actual del usuario en cualquier momento. Cuando el usuario haya consumido todos los elementos, el usuario puede volver a comprar el complemento.  <p/><p/> Por ejemplo, si el complemento representa una cantidad inicial de 100 monedas en un juego y el usuario consume 10 monedas, la aplicación notifica a la Tienda que se han completado 10 unidades del complemento, y la Tienda actualiza el saldo restante. Cuando el usuario ha consumido las 100 monedas, puede volver a comprar el complemento de 100 monedas. <p/><p/> **Los consumibles administrados por la Tienda están disponibles a partir de Windows 10, versión 1607. La capacidad para crear un consumible administrado por la Tienda en el panel del Centro de desarrollo de Windows estará disponible próximamente.**  |

<span />

>**Nota**&nbsp;&nbsp;Otros tipos de complementos, como los complementos durables con paquetes (también conocidos como contenido descargable o DLC) solo están disponibles para un conjunto de desarrolladores restringido y no se tratan en esta documentación.

<span id="api_intro" />
## Introducción al espacio de nombres Windows.Services.Store

El punto de entrada principal al espacio de nombres **Windows.Services.Store** es la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Esta clase proporciona métodos que puedes usar para obtener información sobre la aplicación actual y sus complementos disponibles, comprar una aplicación o un complemento para el usuario actual, obtener información de licencia de la aplicación actual o sus complementos y otras tareas. Para obtener un objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), realiza una de las siguientes acciones:

* En una aplicación de usuario único (es decir, una aplicación que se ejecuta solamente en el contexto del usuario que la inició), usa el método [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) para obtener un objeto **StoreContext** que puedas usar para acceder a los datos relacionados con la Tienda Windows del usuario, así como administrarlos. La mayoría de las aplicaciones para la Plataforma universal de Windows (UWP) son aplicaciones de usuario único.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* En una aplicación multiusuario, usa el método [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) para obtener un objeto **StoreContext** que puedas usar para acceder a los datos relacionados con la Tienda Windows de un usuario específico que haya iniciado sesión con su cuenta de Microsoft al usar la aplicación, así como administrar dichos datos. Para obtener más información sobre las aplicaciones multiusuario, consulta [Introducción a las aplicaciones multiusuario](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications). En el siguiente ejemplo se obtiene un objeto **StoreContext** para el primer usuario disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

Cuando tengas un objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), puedes empezar a llamar a métodos para comprar una aplicación o un complemento para el usuario actual y realizar otras tareas. Para obtener más información, consulta los artículos siguientes:

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)

Para obtener una muestra completa que demuestre cómo implementar pruebas y compras desde la aplicación con el espacio de nombres **Windows.Services.Store**, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span />
### Modelo de objetos de productos, SKU y disponibilidades

Cada producto de la Tienda tiene al menos una *SKU* y cada SKU tiene al menos una *disponibilidad*. Estos conceptos se abstraen de la mayoría de los desarrolladores en el panel del Centro de desarrollo de Windows, y la mayoría de los desarrolladores nunca definirán las SKU ni las disponibilidades de sus aplicaciones o complementos. Sin embargo, dado que el modelo de objetos de los productos de la Tienda en el espacio de nombres **Windows.Services.Store** incluye las SKU y las disponibilidades, un conocimiento básico sobre estos conceptos puede resultar útil.

| Tipo de objeto |  Descripción  |
|---------|-------------------|
| Producto  |  La clase [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) representa cualquier tipo de producto que esté disponible en la Tienda, incluido un complemento o una aplicación. Esta clase proporciona propiedades que puedes usar para acceder a datos, como el Id. de la Tienda del producto, las imágenes y los vídeos de descripción de la Tienda y la información de precios. También proporciona métodos que puedes usar para comprar el producto. |
| SKU |  La clase [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) representa una *SKU* de un producto. Una SKU es una versión específica de un producto con su descripción, precio y otros detalles únicos del producto. Cada aplicación o complemento tiene una SKU predeterminada. La única vez que la mayoría de los desarrolladores tendrán varias SKU para una aplicación es si publican una versión completa de su aplicación y una versión de prueba (en el catálogo de la Tienda, cada una de estas versiones es una SKU diferente de la misma aplicación). <p/><p/> Algunos publicadores tienen la capacidad de definir sus propias SKU. Por ejemplo, un publicador de juegos importante puede lanzar un juego con una SKU que muestre sangre verde en mercados que no permitan sangre roja y otra SKU que muestre sangre roja en el resto de los mercados. Como alternativa, un publicador que venda contenido de vídeo digital podría publicar dos SKU para un vídeo, una SKU para la versión de alta definición y una SKU diferente para la versión de definición estándar. <p/><p/> Cada producto tiene una propiedad [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) que puedes usar para acceder a las SKU. |
| Disponibilidad  |  La clase [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) representa una *disponibilidad* para una SKU. Una disponibilidad es una versión específica de una SKU con su propia información de precios única. Cada SKU tiene una disponibilidad predeterminada. Algunos publicadores tienen la capacidad de definir sus propias disponibilidades para presentar opciones de precios diferentes para una SKU determinada. <p/><p/> Cada SKU tiene una propiedad [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) que puedes usar para acceder a las disponibilidades. Para la mayoría de los desarrolladores, cada SKU tiene la disponibilidad predeterminada única.  |

<span id="store_ids" />
### Id. de la Tienda

Cada aplicación y cada complemento de la Tienda tienen un **Id. de la Tienda** asociado. Muchas de las API del espacio de nombres **Windows.Services.Store** requieren el Id. de la Tienda para realizar una operación en una aplicación o un complemento. Los productos, las SKU y las disponibilidades tienen distintos formatos de Id. de la Tienda.

| Tipo de objeto |  Formato de Id. de la Tienda  |
|---------|-------------------|
| Producto  |  El Id. de la Tienda de cualquier producto de la Tienda es una cadena de 12 caracteres alfanuméricos, como ```9NBLGGH4R315```. Este Id. de la Tienda está disponible en la página de panel del Centro de desarrollo de Windows para la aplicación o el complemento, y lo devuelve la propiedad [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) del objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). A veces, este id. se denomina *Id. de la Tienda del producto*. |
| SKU |  Para una SKU, el Id. de la Tienda tiene el formato ```<product Store ID>/xxxx```, donde ```xxxx``` es una cadena de 4 caracteres alfanuméricos que identifica una SKU del producto. Por ejemplo, ```9NBLGGH4R315/000N```. Este id. lo devuelve la propiedad [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) de un objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) y, en ocasiones, se denomina *Id. de la Tienda de la SKU*. |
| Disponibilidad  |  Para una disponibilidad, el Id. de la Tienda tiene el formato ```<product Store ID>/xxxx/yyyyyyyyyyyy```, donde ```xxxx``` es una cadena de 4 caracteres alfanuméricos que identifica una SKU del producto e ```yyyyyyyyyyyy``` es una cadena de 12 caracteres alfanuméricos que identifica una disponibilidad de la SKU. Por ejemplo, ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Este id. lo devuelve la propiedad [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) de un objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) y, en ocasiones, se denomina *Id. de disponibilidad de la Tienda*.  |

<span id="testing" />
### Probar las aplicaciones que usan el espacio de nombres Windows.Services.Store

El espacio de nombres **Windows.Services.Store** no proporciona una clase que puedas usar para simular la información de licencia durante las pruebas. En su lugar, debes publicar una aplicación en la Tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas. Se trata de una experiencia diferente de las aplicaciones que usan el espacio de nombres **Windows.ApplicationModel.Store**, ya que estas aplicaciones pueden usar la clase [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para simular la información de licencia durante las pruebas.

Si la aplicación usa las API en el espacio de nombres **Windows.Services.Store** para acceder a la información de tu aplicación y sus complementos, sigue este proceso para probar el código:

1. Si la aplicación ya está publicada y disponible en la Tienda y quieres actualizar esta aplicación para usar algunas API del espacio de nombres **Windows.Services.Store**, estás listo para empezar. Si quieres ofrecer complementos para la aplicación, asegúrate de [definir los complementos para la aplicación](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) en el panel del Centro de desarrollo.

  Si aún no tienes una aplicación que esté publicada y disponible en la Tienda, crea una aplicación básica que cumpla con los requisitos mínimos del [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) y [envía esta aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) al panel del Centro de desarrollo de Windows. Si quieres ofrecer complementos para la aplicación, asegúrate de [definir los complementos para la aplicación](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). De manera opcional, puedes [ocultar la aplicación en la Tienda](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) durante las pruebas.

2. Escribe código en la aplicación que use uno de los métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en el espacio de nombres **Windows.Services.Store** para realizar tareas como [obtener los complementos disponibles para la aplicación actual](get-product-info-for-apps-and-add-ons.md), [comprar una aplicación o un complemento](enable-in-app-purchases-of-apps-and-add-ons.md) u [obtener la información de licencia para tu aplicación](get-license-info-for-apps-and-add-ons.md). Consulta la sección de temas relacionados a continuación para ver otros ejemplos.

3. En Visual Studio, haz clic en el **menú Proyecto**, apunta a **Tienda** y, después, haz clic en **Asociar aplicación con la Tienda**. Sigue las instrucciones del Asistente para asociar el proyecto de aplicación con la aplicación de tu cuenta del Centro de desarrollo de Windows que quieres usar para las pruebas.

  >**Nota**&nbsp;&nbsp;Si no asocias el proyecto con una aplicación de la Tienda, los métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) establecen la propiedad **ExtendedError** de sus valores de retorno en el valor del código de error 0x803F6107. Este valor indica que la Tienda no tiene ningún conocimiento de la aplicación.

4. Si aún no lo has hecho, instala la aplicación de la Tienda que especificaste en el paso anterior, ejecuta la aplicación una vez y ciérrala a continuación. Esta acción garantiza que una licencia válida de la aplicación está instalada en el dispositivo de desarrollo.

5. En Visual Studio, empieza a ejecutar o depurar el proyecto. El código debe recuperar datos de aplicación y del complemento de la aplicación de la Tienda que asociaste al proyecto local. Si se te pide que reinstales la aplicación, sigue las instrucciones y, después, ejecuta o depura el proyecto.

## Temas relacionados

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Compras desde la aplicación y pruebas con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


