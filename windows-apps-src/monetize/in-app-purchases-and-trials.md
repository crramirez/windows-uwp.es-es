---
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Obtenga información sobre las tareas y los conceptos básicos necesarios para implementar y probar la funcionalidad de pruebas y compras desde la aplicación en aplicaciones para UWP.
title: Pruebas y compras desde la aplicación
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, compras desde la aplicación, IAPs, complementos, pruebas, consumibles, duraderos, suscripción
ms.localizationpriority: medium
ms.openlocfilehash: fea4b222d604c2da1b97d692658abd8561ab1501
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942814"
---
# <a name="in-app-purchases-and-trials"></a>Pruebas y compras desde la aplicación

Windows SDK proporciona API que puedes usar para implementar las siguientes características con el fin de ganar más dinero con tu aplicación de la Plataforma universal de Windows (UWP):

* **In-app purchases** &nbsp; Compras &nbsp; desde la aplicación Tanto si su aplicación es gratuita como si no, puede vender contenido o nueva funcionalidad de la aplicación (como desbloquear el siguiente nivel de un juego) desde la parte derecha dentro de la aplicación.

* **Trial functionality** &nbsp; Funcionalidad &nbsp; de prueba Si [configura la aplicación como una evaluación gratuita en el centro de Partners](../publish/set-app-pricing-and-availability.md#free-trial), puede persuadir a los clientes para que compren la versión completa de la aplicación excluyendo o limitando algunas características durante el período de prueba. Asimismo, puedes habilitar características tales como banners o marcas de agua que solo se muestren durante la prueba antes de que el cliente compre la aplicación.

Este artículo proporciona una introducción de cómo funcionan las compras desde la aplicación y las pruebas en aplicaciones para UWP.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Elegir qué espacio de nombres usar

Existen dos espacios de nombres diferentes que puedes usar para agregar compras desde la aplicación y la funcionalidad de prueba a las aplicaciones para UWP, según la versión de Windows 10 a la que se destinen tus aplicaciones. Aunque las API de estos espacios de nombres tienen los mismos objetivos, se han diseñado de forma bastante diferente y el código no es compatible entre las dos API.

* **[Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store)** &nbsp; &nbsp; A partir de Windows 10, versión 1607, las aplicaciones pueden usar la API en este espacio de nombres para implementar compras y pruebas desde la aplicación. Se recomienda usar los miembros de este espacio de nombres si el proyecto de la aplicación tiene como destino la **edición de aniversario de Windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio. Este espacio de nombres admite los tipos de complementos más recientes, como los complementos consumibles administrados por el almacén, y está diseñado para ser compatible con los tipos de productos y características futuros admitidos por el centro de Partners y el almacén. Para obtener más información acerca de este espacio de nombres, consulta la sección [Pruebas y compras desde la aplicación con el espacio de nombres Windows.Services.Store](#api_intro) de este artículo.

* **[Windows. ApplicationModel. Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store)** &nbsp; &nbsp; Todas las versiones de Windows 10 también admiten una API anterior para las compras desde la aplicación y las pruebas en este espacio de nombres. Para obtener información sobre el espacio de nombres **Windows. applicationmodel. Store** , consulte [compras desde la aplicación y pruebas mediante el espacio de nombres Windows. applicationmodel. Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> El espacio de nombres **Windows. ApplicationModel. Store** ya no se actualiza con nuevas características y se recomienda usar el espacio de nombres **Windows. Services. Store** en su lugar, si es posible, para la aplicación. El espacio de nombres **Windows. ApplicationModel. Store** no se admite en las aplicaciones de escritorio de Windows que usan el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop) o en aplicaciones o juegos que usan un espacio aislado de desarrollo en el centro de Partners (por ejemplo, este es el caso de cualquier juego que se integre con Xbox Live).

<span id="concepts" />

## <a name="basic-concepts"></a>Conceptos básicos

Por lo general, todos los elementos que se ofrecen en la Tienda se denominan *producto*. La mayoría de los desarrolladores solo trabajan con los siguientes tipos de productos: *aplicaciones* y *Complementos*.

Un complemento es un producto o característica que se pone a disposición de los clientes en el contexto de la aplicación: por ejemplo, la moneda que se va a usar en una aplicación o un juego, nuevas asignaciones o armas para un juego, la capacidad de usar la aplicación sin anuncios o contenido digital como música o vídeos para aplicaciones que tienen la capacidad de ofrecer ese tipo de contenido. Cada aplicación y complemento tiene una licencia asociada que indica si el usuario tiene derecho a usar la aplicación o el complemento. Si el usuario tiene derecho a usar la aplicación o el complemento como una versión de prueba, la licencia también proporciona información adicional sobre la versión de prueba.

Para ofrecer un complemento a los clientes de la aplicación, debe [definir el complemento para la aplicación en el centro de Partners](../publish/add-on-submissions.md) para que el almacén lo sepa. A continuación, la aplicación puede usar las API en los espacios de nombres **Windows.Services.Store** o **Windows.ApplicationModel.Store** para vender el complemento al usuario como una compra desde la aplicación.

Las aplicaciones para UWP pueden ofrecer los siguientes tipos de complementos.

| Tipo de complemento |  Descripción  |
|---------|-------------------|
| Duradero  |  Un complemento que se mantiene durante el tiempo que se [especifica en el centro de Partners](../publish/enter-iap-properties.md). <p/><p/>De manera predeterminada, los complementos durables nunca expiran, por lo que solo se pueden adquirir una vez. Si especificas una duración particular para el complemento, el usuario puede volver a comprar el complemento después de su expiración. |
| Consumible administrado por el desarrollador  |  Un complemento que se puede comprar, usar y volver a adquirir una vez que se ha consumido. Usted es responsable de realizar un seguimiento del equilibrio del usuario de los elementos que representa el complemento.<p/><p/>Cuando el usuario consume algún elemento que está asociado con el complemento, usted es responsable de mantener el saldo del usuario y de notificar la compra del complemento como se ha completado en la tienda después de que el usuario haya consumido todos los elementos. El usuario no puede comprar el complemento otra vez hasta que la aplicación notifique que la compra del complemento anterior se ha completado. <p/><p/>Por ejemplo, si el complemento representa 100 monedas en un juego y el usuario consume 10 monedas, la aplicación o el servicio debe mantener el nuevo saldo restante de 90 monedas para el usuario. Cuando el usuario haya consumido las 100 monedas, la aplicación debe notificar el complemento como completado y, a continuación, el usuario puede volver a comprar el complemento de 100 monedas.    |
| Consumible administrado por la Tienda  |  Un complemento que se puede comprar, usar y volver a comprar en cualquier momento. El almacén realiza un seguimiento del equilibrio del usuario de los elementos que representa el complemento.<p/><p/>Cuando el usuario consume elementos que están asociados con el complemento, usted es responsable de notificar esos elementos como entregados al almacén y el almacén actualiza el saldo del usuario. El usuario puede adquirir el complemento tantas veces como desee (no es necesario usar los elementos primero). La aplicación puede consultar el saldo actual del usuario en cualquier momento. <p/><p/> Por ejemplo, si el complemento representa una cantidad inicial de 100 monedas en un juego y el usuario consume 50 monedas, la aplicación notifica a la tienda que se han cumplido las 50 unidades del complemento y el almacén actualiza el resto del saldo. Si el usuario vuelve a comprar el complemento para adquirir 100 más monedas, ahora tendrá 150 monedas en total. <p/><p/>**Nota:** &nbsp; &nbsp; Para usar los consumibles administrados por el almacenamiento, la aplicación debe tener como destino la **edición de aniversario de Windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio, y debe usar el espacio de nombres **Windows. Services. Store** en lugar del espacio de nombres **Windows. ApplicationModel. Store** .  |
| Subscription | Un complemento duradero en el que el cliente se sigue cobrando a intervalos recurrentes para seguir usando el complemento. El cliente puede cancelar la suscripción en cualquier momento para evitar cargos adicionales. <p/><p/>**Nota:** &nbsp; &nbsp; Para usar los complementos de suscripción, la aplicación debe tener como destino la **edición de aniversario de Windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio, y debe usar el espacio de nombres **Windows. Services. Store** en lugar del espacio de nombres **Windows. ApplicationModel. Store** .  |

<span />

> [!NOTE]
> Otros tipos de complementos, como los complementos duraderos con paquetes (también conocidos como contenido descargable o DLC), solo están disponibles para un conjunto restringido de desarrolladores y no se incluyen en esta documentación.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Pruebas y compras desde la aplicación con el espacio de nombres Windows.Services.Store

En esta sección se proporciona información general sobre las tareas y conceptos importantes del espacio de nombres [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) . Este espacio de nombres solo está disponible para las aplicaciones que tienen como destino la **edición de aniversario de Windows 10 (10,0; Compilación 14393)** o una versión posterior en Visual Studio (esto corresponde a Windows 10, versión 1607). Se recomienda que las aplicaciones usen el espacio de nombres **Windows. Services. Store** en lugar del espacio de nombres [Windows. ApplicationModel. Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) si es posible. Para obtener información sobre el espacio de nombres **Windows. ApplicationModel. Store** , consulte [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**En esta sección**

* [Vídeo](#video)
* [Introducción a la clase StoreContext](#get-started-storecontext)
* [Implementar compras desde la aplicación](#implement-iap)
* [Implementar la funcionalidad de prueba](#implement-trial)
* [Probar la implementación de pruebas o compras desde la aplicación](#testing)
* [Recibos de las compras desde la aplicación](#receipts)
* [Uso de la clase StoreContext con el puente de escritorio](#desktop)
* [Productos, SKU y disponibilidades](#products-skus)
* [Id. de la Tienda](#store-ids)

<span id="video" />

### <a name="video"></a>Vídeo

Vea el vídeo siguiente para obtener información general sobre cómo implementar compras desde la aplicación en la aplicación con el espacio de nombres [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) .
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Introducción a la clase StoreContext

El punto de entrada principal al espacio de nombres **Windows.Services.Store** es la clase [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Esta clase proporciona métodos que puedes usar para obtener información acerca de la aplicación actual y sus complementos disponibles, obtener información de licencia de la aplicación actual o sus complementos, comprar una aplicación o un complemento para el usuario actual y realizar otras tareas. Para obtener un objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext), realiza una de las siguientes acciones:

* En una aplicación de un solo usuario (es decir, una aplicación que se ejecuta solo en el contexto del usuario que inició la aplicación), use el método estático [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) para obtener un objeto **StoreContext** que puede usar para tener acceso a los datos relacionados con Microsoft Store del usuario. La mayoría de las aplicaciones para la Plataforma universal de Windows (UWP) son aplicaciones de usuario único.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* En una [aplicación de varios usuarios](../xbox-apps/multi-user-applications.md), use el método estático [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) para obtener un objeto **StoreContext** que pueda usar para acceder a datos relacionados con Microsoft Store para un usuario específico que haya iniciado sesión con su cuenta de Microsoft mientras usa la aplicación. En el siguiente ejemplo se obtiene un objeto **StoreContext** para el primer usuario disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Las aplicaciones de escritorio de Windows que usan el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop) deben realizar pasos adicionales para configurar el objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) antes de poder utilizar este objeto. Para más información, consulte [esta sección](#desktop).

Cuando tengas un objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext), puedes empezar a llamar a métodos de este objeto para obtener información de productos de la Tienda para la aplicación actual y sus complementos, recuperar información de licencias relativa a la aplicación actual y sus complementos, comprar una aplicación o un complemento para el usuario actual y realizar otras tareas. Para obtener más información sobre las tareas comunes que puedes realizar con este objeto, consulta los siguientes artículos:

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de una suscripción para la aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)

Para obtener una aplicación de muestra que indica cómo usar **StoreContext** y otros tipos del espacio de nombres **Windows.Services.Store**, consulta la [muestra de Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implementar compras desde la aplicación

Para ofrecer una compra desde la aplicación a los clientes mediante el espacio de nombres **Windows.Services.Store**:

1. Si la aplicación ofrece complementos que los clientes pueden comprar, [cree envíos de complementos para la aplicación en el centro de Partners ](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Escribe código en tu aplicación para [recuperar información de producto para la aplicación o un complemento que ofrece tu aplicación](get-product-info-for-apps-and-add-ons.md) y, después, [determina si la licencia está activa](get-license-info-for-apps-and-add-ons.md) (es decir, si el usuario tiene licencia para usar la aplicación o complemento). Si la licencia no está activa, muestra al usuario una interfaz que ofrezca la aplicación o el complemento a la venta como una compra desde la aplicación.

3. Si el usuario decide comprar la aplicación o el complemento, usa el método adecuado para comprar el producto:

    * Si el usuario compra la aplicación o un complemento durable, sigue las instrucciones de [Habilitar compras desde la aplicación de aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Si el usuario compra un complemento consumible, sigue las instrucciones que se indican en [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md).
    * Si el usuario está comprando un complemento de suscripción, siga las instrucciones de [Habilitar complementos de suscripción para la aplicación](enable-subscription-add-ons-for-your-app.md).

4. Prueba la implementación siguiendo la [guía de prueba](#testing) de este artículo.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implementar la funcionalidad de prueba

Para excluir o limitar características en una versión de prueba de la aplicación mediante el espacio de nombres **Windows.Services.Store**:

1. [Configure la aplicación como una evaluación gratuita en el centro de Partners](../publish/set-app-pricing-and-availability.md#free-trial).

2. Escribe código en tu aplicación para [recuperar información de producto para la aplicación o un complemento que ofrece tu aplicación](get-product-info-for-apps-and-add-ons.md) y, después, [determina si la licencia asociada a la aplicación es de prueba](get-license-info-for-apps-and-add-ons.md).

3. Excluye o limita ciertas funciones de la aplicación si es una versión de prueba y, después, habilita las características cuando el usuario adquiera una licencia completa. Para obtener más información y un ejemplo de código, consulta [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md).

4. Prueba la implementación siguiendo la [guía de prueba](#testing) de este artículo.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Probar la implementación de pruebas o compras desde la aplicación

Si la aplicación usa las API del espacio de nombres **Windows. Services. Store** para implementar la funcionalidad de compra o de prueba desde la aplicación, debe publicar la aplicación en la tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para realizar pruebas. Siga este proceso para probar el código:

1. Si la aplicación aún no se ha publicado y está disponible en la tienda, asegúrese de que la aplicación cumple los requisitos mínimos del kit para la [certificación de aplicaciones de Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) , [envíe la aplicación](https://docs.microsoft.com/windows/uwp/publish/app-submissions) en el centro de Partners y asegúrese de que la aplicación pasa el proceso de certificación. Puede [configurar la aplicación para que no se pueda detectar en el almacén mientras la](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) prueba. Tenga en cuenta la configuración adecuada de los [vuelos de paquetes](../publish/package-flights.md). Es posible que no se puedan descargar los vuelos de paquetes configurados incorrectamente.

2. Después, asegúrate de haber seguido estos pasos:

    * Escribe código en la aplicación que usa la clase [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) y otros tipos relacionados en el espacio de nombres **Windows.Services.Store** para implementar [compras desde la aplicación](#implement-iap) o una [funcionalidad de prueba](#implement-trial).
    * Si su aplicación ofrece un complemento que los clientes pueden comprar, [cree un envío de complementos para la aplicación en el centro de Partners](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Si quiere excluir o limitar algunas características de una versión de prueba de la aplicación, [Configure la aplicación como una evaluación gratuita en el centro de Partners](../publish/set-app-pricing-and-availability.md#free-trial).

3. Abre tu proyecto en Visual Studio, haz clic en el **menú Proyecto**, selecciona **Tienda** y, después, haz clic en **Asociar aplicación con la Tienda**. Siga las instrucciones del Asistente para asociar el proyecto de la aplicación a la aplicación en la cuenta del centro de partners que desee usar para las pruebas.
    > [!NOTE]
    > Si no asocia el proyecto a una aplicación de la tienda, los métodos de [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) establecen la propiedad **ExtendedError** de los valores devueltos en el valor de código de error 0x803F6107. Este valor indica que la Tienda no tiene ningún conocimiento de la aplicación.
4. Si aún no lo has hecho, instala la aplicación de la Tienda que especificaste en el paso anterior, ejecuta la aplicación una vez y ciérrala a continuación. Esta acción garantiza que una licencia válida de la aplicación está instalada en el dispositivo de desarrollo.

5. En Visual Studio, empieza a ejecutar o depurar el proyecto. El código debe recuperar datos de aplicación y del complemento de la aplicación de la Tienda que asociaste al proyecto local. Si un mensaje te pide que reinstales la aplicación, sigue las instrucciones y, después, ejecuta o depura el proyecto.
    > [!NOTE]
    > Después de completar estos pasos, puede seguir actualizando el código de la aplicación y, a continuación, depurar el proyecto actualizado en el equipo de desarrollo sin enviar nuevos paquetes de aplicaciones al almacén. Solo debes descargar la versión de la aplicación de la Tienda en el equipo de desarrollo una vez para obtener la licencia local que se usará para las pruebas. Solo necesitas enviar nuevos paquetes de la aplicación a la Tienda si después de completar las pruebas quieres que las características de compras o pruebas desde la aplicación de tu aplicación estén disponibles para tus clientes.

Si su aplicación usa el espacio de nombres **Windows. ApplicationModel. Store** , puede usar la clase [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) en la aplicación para simular la información de licencia durante las pruebas antes de enviar la aplicación a la tienda. Para obtener más información, consulte Introducción [a las clases CurrentApp y CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** no proporciona ninguna clase que puedas usar para simular la información de licencia durante las pruebas. Si usa el espacio de nombres **Windows. Services. Store** para implementar las compras o las pruebas desde la aplicación, debe publicar la aplicación en la tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas como se describió anteriormente.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Recibos de las compras desde la aplicación

El espacio de nombres **Windows.Services.Store** no proporciona ninguna API que puedas usar para recuperar un recibo de transacción de las compras que culminen correctamente en el código de tu aplicación. Se trata de una experiencia distinta de la de las aplicaciones que usan el espacio de nombres **Windows.ApplicationModel.Store**, que pueden [usar la API del lado cliente para obtener un recibo de transacción](use-receipts-to-verify-product-purchases.md).

Si implementa compras desde la aplicación con el espacio de nombres **Windows. Services. Store** y desea validar si un determinado cliente ha adquirido una aplicación o un complemento, puede usar el [método consulta de productos](query-for-products.md) de la API de REST de la [colección de Microsoft Store](view-and-grant-products-from-a-service.md). Los datos que este método devuelve confirman si el cliente en cuestión tiene derecho a un producto determinado y proporciona datos de la transacción en la que el usuario compró el producto. La API de colección Microsoft Store usa la autenticación de Azure AD para recuperar esta información.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Uso de la clase StoreContext con el puente de escritorio

Las aplicaciones de escritorio que usan el [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop) pueden utilizar la clase [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) para implementar compras desde la aplicación y periodos de pruebas. Sin embargo, si tienes una aplicación de escritorio Win32 o una con un identificador de ventana (HWND) asociado al marco de representación (por ejemplo, una aplicación WPF), la aplicación debe configurar el objeto **StoreContext** para especificar qué ventana de aplicación es la ventana propietaria en los cuadros de diálogo modales que el objeto muestra.

Muchos miembros de **StoreContext** (y los miembros de otros tipos relacionados a los que se accede a través del objeto **StoreContext**) muestran un cuadro de diálogo modal al usuario de las operaciones relacionadas con la Tienda, como comprar un producto. Si una aplicación de escritorio no configura el objeto **StoreContext** para que especifique la ventana propietaria de los cuadros de diálogo modales, este objeto devolverá datos incorrectos o errores.

Para configurar un objeto **StoreContext** en una aplicación de escritorio que usa el Puente de escritorio, sigue estos pasos.

1. Realiza una de las siguientes acciones para permitir que la aplicación acceda a la interfaz [IInitializeWithWindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow):

    * Si tu aplicación está escrita en un lenguaje administrado, como C# o Visual Basic, declara la interfaz **IInitializeWithWindow** en el código de la aplicación con el atributo [ComImport](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) como se muestra en el siguiente ejemplo de C#. En este ejemplo se da por hecho que el archivo de código tiene una instrucción **using** para el espacio de nombres **System.Runtime.InteropServices**.

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * Si la aplicación está escrita en C++, agrega una referencia al archivo de encabezado shobjidl.h en el código. Este archivo de encabezado contiene la declaración de la interfaz **IInitializeWithWindow**.

2. Obtén un objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) con el método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) (o [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) si tu aplicación es [multiusuario](../xbox-apps/multi-user-applications.md)) tal como se describe anteriormente en este artículo y conviértelo en un objeto [IInitializeWithWindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow). Después, llama al método [IInitializeWithWindow.Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) y pasa el identificador de la ventana que quieras que sea la propietaria de los cuadros de diálogo modales que muestren los métodos **StoreContext**. El siguiente ejemplo de C# muestra cómo pasar el identificador de la ventana principal de la aplicación al método.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Productos, SKU y disponibilidades

Cada producto de la Tienda tiene al menos una *SKU*, y cada SKU tiene al menos una *disponibilidad*. Estos conceptos se abstraen de la mayoría de los desarrolladores del centro de Partners, y la mayoría de los desarrolladores nunca definirán SKU ni disponibilidad para sus aplicaciones o complementos. Sin embargo, dado que el modelo de objetos para los productos de la tienda en el espacio de nombres **Windows. Services. Store** incluye SKU y disponibilidad, una comprensión básica de estos conceptos puede ser útil para algunos escenarios.

| Object |  Descripción  |
|---------|-------------------|
| Producto  |  Un *producto* hace referencia a cualquier tipo de producto que esté disponible en el almacén, incluida una aplicación o un complemento. <p/><p/> Cada producto de la tienda tiene un objeto [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) correspondiente. Esta clase proporciona propiedades que puedes usar para acceder a datos, como el Id. de la Tienda del producto, las imágenes y los vídeos de descripción de la Tienda y la información de precios. También proporciona métodos que puedes usar para comprar el producto. |
| SKU |  Una *SKU* es una versión específica de un producto con su propia descripción, precio y otros detalles de productos únicos. Cada aplicación o complemento tiene una SKU predeterminada. La única vez que la mayoría de los desarrolladores tendrán varias SKU para una aplicación es si publican una versión completa de su aplicación y una versión de prueba (en el catálogo de la Tienda, cada una de estas versiones es una SKU diferente de la misma aplicación). <p/><p/> Algunos publicadores tienen la capacidad de definir sus propias SKU. Por ejemplo, un publicador de juegos importante puede lanzar un juego con una SKU que muestre sangre verde en mercados que no permitan sangre roja y otra SKU que muestre sangre roja en el resto de los mercados. Como alternativa, un publicador que venda contenido de vídeo digital podría publicar dos SKU para un vídeo, una SKU para la versión de alta definición y una SKU diferente para la versión de definición estándar. <p/><p/> Cada SKU del almacén tiene un objeto [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) correspondiente. Cada [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) tiene una propiedad [SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) que puede usar para tener acceso a las SKU del producto. |
| Disponibilidad  |  Una *disponibilidad* es una versión específica de una SKU con su propia información de precios única. Cada SKU tiene una disponibilidad predeterminada. Algunos publicadores tienen la capacidad de definir sus propias disponibilidades para presentar opciones de precios diferentes para una SKU determinada. <p/><p/> Cada disponibilidad en el almacén tiene un objeto [StoreAvailability](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) correspondiente. Cada [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) tiene una propiedad [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) que puede usar para tener acceso a las disponibilidades de la SKU. Para la mayoría de los desarrolladores, cada SKU tiene la disponibilidad predeterminada única.  |

<span id="store_ids" />

### <a name="store-ids"></a>Id. de la Tienda

Cada aplicación, complemento u otro producto de la tienda tiene un **identificador de almacén** asociado (también se denomina a veces identificador de *almacén de productos*). Muchas API requieren el identificador de almacén para realizar una operación en una aplicación o un complemento.

El identificador de la Tienda de cualquier producto de la Tienda es una cadena de 12 caracteres alfanuméricos, como ```9NBLGGH4R315```. Hay varias maneras de obtener el identificador de la tienda de un producto en la tienda:

* En el caso de una aplicación, puede obtener el identificador de la tienda en la [Página identidad](../publish/view-app-identity-details.md) de la aplicación en el centro de Partners.
* En el caso de un complemento, puede obtener el identificador de la tienda en la página de información general del complemento en el centro de Partners.
* Para cualquier producto, también puede obtener el identificador de almacén mediante programación con la propiedad [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) del objeto [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) que representa el producto.

En el caso de los productos con SKU y disponibilidad, las SKU y las disponibilidades también tienen sus propios identificadores de almacén con distintos formatos.

| Object |  Formato de identificador de la Tienda  |
|---------|-------------------|
| SKU |  El identificador de almacén de una SKU tiene el formato ```<product Store ID>/xxxx``` , donde ```xxxx``` es una cadena alfanumérica de 4 caracteres que identifica una SKU para el producto. Por ejemplo, ```9NBLGGH4R315/000N```. Este identificador lo devuelve la propiedad [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) de un objeto [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) y, en ocasiones, se denomina *identificador de la Tienda de la SKU*. |
| Disponibilidad  |  El identificador de almacén para una disponibilidad tiene el formato ```<product Store ID>/xxxx/yyyyyyyyyyyy``` , donde ```xxxx``` es una cadena alfanumérica de 4 caracteres que identifica una SKU para el producto y ```yyyyyyyyyyyy``` es una cadena alfanumérica de 12 caracteres que identifica una disponibilidad para la SKU. Por ejemplo, ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Este identificador lo devuelve la propiedad [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid) de un objeto [StoreAvailability](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) y, en ocasiones, se denomina *identificador de la Tienda de disponibilidad*.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Cómo usar identificadores de producto para complementos en el código

Si desea que un complemento esté disponible para los clientes en el contexto de la aplicación, debe [escribir un identificador de producto único](../publish/set-your-add-on-product-id.md#product-id) para el complemento al [crear el envío del complemento](../publish/add-on-submissions.md) en el centro de Partners. Puede usar este identificador de producto para hacer referencia al complemento en el código, aunque los escenarios específicos en los que puede usar el identificador de producto dependen del espacio de nombres que use para las compras desde la aplicación en la aplicación.

> [!NOTE]
> El identificador de producto que especifique en el centro de partners para un complemento es diferente del [identificador de almacén](#store-ids)del complemento. El identificador de almacén se genera por el centro de Partners.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Aplicaciones que usan el espacio de nombres Windows. Services. Store

Si su aplicación usa el espacio de nombres **Windows. Services. Store** , puede usar el identificador de producto para identificar fácilmente el [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) que representa el complemento o el [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) que representa la licencia del complemento. El identificador de producto se expone mediante las propiedades [StoreProduct. InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) y [StoreLicense. InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken) .

> [!NOTE]
> Aunque el identificador de producto es una manera útil de identificar un complemento en el código, la mayoría de las operaciones en el espacio de nombres **Windows. Services. Store** usan el [identificador de almacén](#store-ids) de un complemento en lugar del ID. del producto. Por ejemplo, para recuperar mediante programación uno o más complementos conocidos para una aplicación, pase los identificadores de almacén (en lugar de los identificadores de producto) de los complementos al método [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync) . Del mismo modo, para notificar un complemento consumible como completado, pase el identificador de almacén del complemento (en lugar del ID. de producto) al método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) .

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Aplicaciones que usan el espacio de nombres Windows. ApplicationModel. Store

Si su aplicación usa el espacio de nombres **Windows. ApplicationModel. Store** , deberá usar el identificador de producto que asigna a un complemento en el centro de partners para la mayoría de las operaciones. Por ejemplo:

* Use el ID. de producto para identificar el [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) que representa el complemento o el [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) que representa la licencia del complemento. El identificador de producto se expone mediante las propiedades [ProductListing. ProductID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) y [ProductLicense. ProductID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId) .

* Especifique el identificador del producto cuando compre el complemento para el usuario mediante el método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) . Para obtener más información, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md).

* Especifique el identificador de producto al notificar el complemento consumible como completado mediante el método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) . Para obtener más información, consulte [Habilitar compras consumibles de productos en la aplicación](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Temas relacionados

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de una suscripción para la aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Códigos de error para las operaciones de Store](error-codes-for-store-operations.md)
* [Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
