---
author: Xansky
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Obtén información sobre cómo habilitar las pruebas y compras desde la aplicación en las aplicaciones para UWP.
title: Pruebas y compras desde la aplicación
ms.author: mhopkins
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, compras desde la aplicación, IAP, complementos, pruebas, consumible, duradero, suscripción
ms.localizationpriority: medium
ms.openlocfilehash: b116e4794f024ecc5cf44d73e22c09ab8ee33ff4
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5398776"
---
# <a name="in-app-purchases-and-trials"></a>Pruebas y compras desde la aplicación

Windows SDK proporciona API que puedes usar para implementar las siguientes características con el fin de ganar más dinero con tu aplicación de la Plataforma universal de Windows (UWP):

* **Compras desde la aplicación**&nbsp;&nbsp;Independientemente de que la aplicación sea gratuita o no, puedes vender contenido o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación.

* **Funcionalidad de prueba**&nbsp;&nbsp;Si configuras la aplicación como una [prueba gratuita en el panel del Centro de desarrollo de Windows](../publish/set-app-pricing-and-availability.md#free-trial), puedes animar a los clientes a comprar la versión completa de tu aplicación excluyendo o limitando ciertas características durante el período de prueba. Asimismo, puedes habilitar características tales como banners o marcas de agua que solo se muestren durante la prueba antes de que el cliente compre la aplicación.

Este artículo proporciona una introducción de cómo funcionan las compras desde la aplicación y las pruebas en aplicaciones para UWP.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Elegir qué espacio de nombres usar

Existen dos espacios de nombres diferentes que puedes usar para agregar compras desde la aplicación y la funcionalidad de prueba a las aplicaciones para UWP, según la versión de Windows 10 a la que se destinen tus aplicaciones. Aunque las API de estos espacios de nombres tienen los mismos objetivos, se han diseñado de forma bastante diferente y el código no es compatible entre las dos API.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;A partir de la versión 1607 de Windows 10, las aplicaciones pueden usar la API de este espacio de nombres para implementar compras desde la aplicación y pruebas. Si el proyecto de la aplicación se destina a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio, te recomendamos que uses los miembros de este espacio de nombres. Este espacio de nombres admite los tipos de complemento más recientes, como los complementos de consumibles administrados por la Tienda, y está diseñado para ser compatible con futuros tipos de productos y características compatibles con el Centro de desarrollo de Windows y la Tienda. Para obtener más información acerca de este espacio de nombres, consulta la sección [Pruebas y compras desde la aplicación con el espacio de nombres Windows.Services.Store](#api_intro) de este artículo.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;Todas las versiones de Windows 10 también admiten una API anterior para las compras desde la aplicación y las pruebas en este espacio de nombres. Para obtener información sobre el espacio de nombres **Windows.ApplicationModel.Store**, consulta [Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> El espacio de nombres **Windows.ApplicationModel.Store** ya no se actualiza con las nuevas características por lo que te recomendamos que uses el espacio de nombres **Windows.Services.Store** en su lugar si es posible para tu aplicación. El espacio de nombres **Windows.ApplicationModel.Store** no se admite en aplicaciones de escritorio de Windows que usan el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop) o en aplicaciones o juegos que usan un espacio aislado de desarrollo en el Centro de desarrollo (por ejemplo, este es el caso de los juegos que se integran con Xbox Live).

<span id="concepts" />

## <a name="basic-concepts"></a>Conceptos básicos

Por lo general, todos los elementos que se ofrecen en la Tienda se denominan *producto*. La mayoría de los desarrolladores solo trabajan con los siguientes tipos de productos: *aplicaciones* y *complementos*.

Un complemento es un producto o una característica que pones a disposición de tus clientes en el contexto de tu aplicación: por ejemplo, la moneda que se usará en una aplicación o un juego, los nuevos mapas o armas para un juego, la posibilidad de usar la aplicación sin anuncios o el contenido digital, como música o vídeos, para las aplicaciones capaces de ofrecer ese tipo de contenido. Cada aplicación y complemento tiene una licencia asociada que indica si el usuario tiene derecho a usar la aplicación o el complemento. Si el usuario tiene derecho a usar la aplicación o el complemento como una versión de prueba, la licencia también proporciona información adicional sobre la versión de prueba.

Para ofrecer un complemento a los clientes en la aplicación, debes [definir el complemento de la aplicación en el panel del Centro de desarrollo](../publish/add-on-submissions.md) para comunicarlo a la Tienda. A continuación, la aplicación puede usar las API en los espacios de nombres **Windows.Services.Store** o **Windows.ApplicationModel.Store** para vender el complemento al usuario como una compra desde la aplicación.

Las aplicaciones para UWP pueden ofrecer los siguientes tipos de complementos.

| Tipo de complemento |  Descripción  |
|---------|-------------------|
| Durable  |  Un complemento que persiste durante el ciclo de vida especificado en el [panel del Centro de desarrollo de Windows](../publish/enter-iap-properties.md). <p/><p/>De manera predeterminada, los complementos durables nunca expiran, por lo que solo se pueden adquirir una vez. Si especificas una duración particular para el complemento, el usuario puede volver a comprar el complemento después de su expiración. |
| Consumible administrado por el desarrollador  |  Un complemento que se puede comprar, usar y volver a comprar después de que se consume. Es responsable de realizar un seguimiento del saldo del usuario de los elementos que representa el complemento.<p/><p/>Cuando el usuario consume elementos asociados con el complemento, eres responsable del mantenimiento del saldo del usuario y de informar de la compra del complemento cuando se complete en la Store y una vez que el usuario haya consumido todos los elementos. El usuario no puede comprar el complemento otra vez hasta que la aplicación notifique que la compra del complemento anterior se ha completado. <p/><p/>Por ejemplo, si el complemento representa 100 monedas en un juego y el usuario consume 10 monedas, la aplicación o el servicio debe mantener el nuevo saldo restante de 90 monedas para el usuario. Cuando el usuario haya consumido las 100 monedas, la aplicación debe notificar el complemento como completado y, a continuación, el usuario puede volver a comprar el complemento de 100 monedas.    |
| Consumible administrado por la Store  |  Un complemento que se puede comprar, usar y volver a comprar en cualquier momento. La Store realiza un seguimiento del saldo del usuario de los elementos que representa el complemento.<p/><p/>Cuando el usuario consume algún elemento asociado al complemento, debes notificar dichos elementos como completados en la Store, tras lo cual la Store actualiza el saldo del usuario. Los usuarios pueden adquirir el complemento tantas veces como quieran (no necesitan consumir los elementos antes). La aplicación puede consultar el saldo actual del usuario en cualquier momento. <p/><p/> Por ejemplo, si el complemento representa una cantidad inicial de 100 monedas en un juego y el usuario consume 50 monedas, la aplicación notifica a la Store que se han completado 50 unidades del complemento, y la Store actualiza el saldo restante. Si el usuario vuelve a comprar el complemento para adquirir 100 monedas más, ahora tendrá 150 monedas totales. <p/><p/>**Nota**&nbsp;&nbsp;Para usar consumibles administrados por Microsoft Store, la aplicación debe destinarse a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio, y debe usar el espacio de nombres **Windows.Services.Store** en lugar de **Windows.ApplicationModel.Store**.  |
| Suscripción | Un complemento duradero donde al cliente se le continúa cobrando en intervalos periódicos para poder seguir usando el complemento. El cliente puede cancelar la suscripción en cualquier momento para evitar más cargos. <p/><p/>**Nota**&nbsp;&nbsp;Para usar complementos de suscripción, la aplicación debe destinarse a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio, y debe usar el espacio de nombres **Windows.Services.Store** en lugar de **Windows.ApplicationModel.Store**.  |

<span />

> [!NOTE]
> Otros tipos de complementos, como los complementos durables con paquetes (también conocidos como contenido descargable o DLC) solo están disponibles para un conjunto de desarrolladores restringido y no se tratan en esta documentación.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Pruebas y compras desde la aplicación con el espacio de nombres Windows.Services.Store

Esta sección proporciona una visión general de los conceptos y las tareas importantes relacionados con el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Este espacio de nombres solo está disponible para aplicaciones destinadas a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio (esto corresponde a Windows 10, versión 1607). Recomendamos que las aplicaciones usen el espacio de nombres **Windows.Services.Store** en lugar de [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) en la medida de lo posible. Para obtener información sobre el espacio de nombres **Windows.ApplicationModel.Store**, consulta [este artículo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

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

Vea el vídeo siguiente para obtener información general sobre cómo implementar las compras desde la aplicación en tu aplicación mediante el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Introducción a la clase StoreContext

El punto de entrada principal al espacio de nombres **Windows.Services.Store** es la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Esta clase proporciona métodos que puedes usar para obtener información acerca de la aplicación actual y sus complementos disponibles, obtener información de licencia de la aplicación actual o sus complementos, comprar una aplicación o un complemento para el usuario actual y realizar otras tareas. Para obtener un objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), realiza una de las siguientes acciones:

* En una aplicación de usuario único (es decir, una aplicación que se ejecuta solamente en el contexto del usuario que la inició), usa el método estático [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) para obtener un objeto **StoreContext** que puedes usar para acceder a los datos relacionados con Microsoft Store del usuario. La mayoría de las aplicaciones para la Plataforma universal de Windows (UWP) son aplicaciones de usuario único.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* En una [aplicación multiusuario](../xbox-apps/multi-user-applications.md), usa el método estático [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) para obtener un objeto **StoreContext** que puedes usar para acceder a los datos relacionados con Microsoft Store de un usuario específico que ha iniciado sesión con su cuenta Microsoft al usar la aplicación. En el siguiente ejemplo se obtiene un objeto **StoreContext** para el primer usuario disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Para las aplicaciones de escritorio de Windows que usan el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop) se han de realizar pasos adicionales para configurar el objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) antes de poder usar dicho objeto. Para obtener más información, consulta [esta sección](#desktop).

Cuando tengas un objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), puedes empezar a llamar a métodos de este objeto para obtener información de productos de la Tienda para la aplicación actual y sus complementos, recuperar información de licencias relativa a la aplicación actual y sus complementos, comprar una aplicación o un complemento para el usuario actual y realizar otras tareas. Para obtener más información sobre las tareas comunes que puedes realizar con este objeto, consulta los siguientes artículos:

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de una suscripción para tu aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)

Para obtener una aplicación de muestra que indica cómo usar **StoreContext** y otros tipos del espacio de nombres **Windows.Services.Store**, consulta la [muestra de Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implementar compras desde la aplicación

Para ofrecer una compra desde la aplicación a los clientes mediante el espacio de nombres **Windows.Services.Store**:

1. Si tu aplicación ofrece complementos que los clientes pueden comprar, [crea envíos de complementos de la aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Escribe código en tu aplicación para [recuperar información de producto para la aplicación o un complemento que ofrece tu aplicación](get-product-info-for-apps-and-add-ons.md) y, después, [determina si la licencia está activa](get-license-info-for-apps-and-add-ons.md) (es decir, si el usuario tiene licencia para usar la aplicación o complemento). Si la licencia no está activa, muestra al usuario una interfaz que ofrezca la aplicación o el complemento a la venta como una compra desde la aplicación.

3. Si el usuario decide comprar la aplicación o el complemento, usa el método adecuado para comprar el producto:

    * Si el usuario compra la aplicación o un complemento durable, sigue las instrucciones de [Habilitar compras desde la aplicación de aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Si el usuario compra un complemento consumible, sigue las instrucciones que se indican en [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md).
    * Si el usuario compra un complemento de la suscripción, sigue las instrucciones de [Habilitar complementos de una suscripción para tu aplicación](enable-subscription-add-ons-for-your-app.md).

4. Prueba la implementación siguiendo la [guía de prueba](#testing) de este artículo.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implementar la funcionalidad de prueba

Para excluir o limitar características en una versión de prueba de la aplicación mediante el espacio de nombres **Windows.Services.Store**:

1. [Configura la aplicación como una prueba gratuita en el panel del Centro de desarrollo de Windows](../publish/set-app-pricing-and-availability.md#free-trial).

2. Escribe código en tu aplicación para [recuperar información de producto para la aplicación o un complemento que ofrece tu aplicación](get-product-info-for-apps-and-add-ons.md) y, después, [determina si la licencia asociada a la aplicación es de prueba](get-license-info-for-apps-and-add-ons.md).

3. Excluye o limita ciertas funciones de la aplicación si es una versión de prueba y, después, habilita las características cuando el usuario adquiera una licencia completa. Para obtener más información y un ejemplo de código, consulta [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md).

4. Prueba la implementación siguiendo la [guía de prueba](#testing) de este artículo.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Probar la implementación de pruebas o compras desde la aplicación

Si la aplicación usa las API en el espacio de nombres **Windows.Services.Store** para implementar la compra desde la aplicación o la funcionalidad de prueba, debes publicar tu aplicación en Microsoft Store y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas. Sigue este proceso para probar el código:

1. Si la aplicación aún no está publicada ni disponible en Microsoft Store, asegúrate de que tu aplicación cumpla con los requisitos mínimos del [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit), [envía la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) al Panel del Centro de desarrollo de Windows y asegúrate de que la aplicación supera el proceso de certificación. Puedes [configurar la aplicación para que no se pueda descubrir en la Tienda](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) mientras la pruebas. Ten en cuenta la configuración correcta de [paquetes piloto](../publish/package-flights.md). Incorrectamente paquete configurado puede ser piloto no podrán descargarse.

2. Después, asegúrate de haber seguido estos pasos:

    * Escribe código en la aplicación que usa la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) y otros tipos relacionados en el espacio de nombres **Windows.Services.Store** para implementar [compras desde la aplicación](#implement-iap) o una [funcionalidad de prueba](#implement-trial).
    * Si tu aplicación ofrece un complemento que los clientes pueden comprar, [crea un envío de complemento para la aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Si quieres excluir o limitar ciertas funciones en una versión de prueba de la aplicación, [configura tu aplicación como una prueba gratuita en el panel del Centro de desarrollo de Windows](../publish/set-app-pricing-and-availability.md#free-trial).

3. Abre tu proyecto en Visual Studio, haz clic en el **menú Proyecto**, selecciona **Tienda** y, después, haz clic en **Asociar aplicación con la Tienda**. Sigue las instrucciones del Asistente para asociar el proyecto de aplicación con la aplicación de tu cuenta del Centro de desarrollo de Windows que quieres usar para las pruebas.
    > [!NOTE]
    > Si no asocias el proyecto con una aplicación de la Tienda, los métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) establecen la propiedad **ExtendedError** de sus valores devueltos en el valor del código de error 0x803F6107. Este valor indica que la Tienda no tiene ningún conocimiento de la aplicación.
4. Si aún no lo has hecho, instala la aplicación de la Tienda que especificaste en el paso anterior, ejecuta la aplicación una vez y ciérrala a continuación. Esta acción garantiza que una licencia válida de la aplicación está instalada en el dispositivo de desarrollo.

5. En Visual Studio, empieza a ejecutar o depurar el proyecto. El código debe recuperar datos de aplicación y del complemento de la aplicación de la Tienda que asociaste al proyecto local. Si un mensaje te pide que reinstales la aplicación, sigue las instrucciones y, después, ejecuta o depura el proyecto.
    > [!NOTE]
    > Después de completar estos pasos, puedes seguir con la actualización del código de la aplicación y, después, depurar el proyecto actualizado en el equipo de desarrollo sin enviar nuevos paquetes de aplicación a la Tienda. Solo debes descargar la versión de la aplicación de la Tienda en el equipo de desarrollo una vez para obtener la licencia local que se usará para las pruebas. Solo necesitas enviar nuevos paquetes de la aplicación a la Tienda si después de completar las pruebas quieres que las características de compras o pruebas desde la aplicación de tu aplicación estén disponibles para tus clientes.

Si la aplicación usa el espacio de nombres **Windows.ApplicationModel.Store**, puedes usar la clase [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) de tu aplicación para simular la información de licencia durante las pruebas antes de enviar la aplicación a la Tienda. Para obtener más información, consulta [Introducción a las clases CurrentApp y CurrentAppSimulator] (in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> El espacio de nombres **Windows.Services.Store** no proporciona ninguna clase que puedas usar para simular la información de licencia durante las pruebas. Si usas el espacio de nombres **Windows.Services.Store** para implementar las pruebas o compras desde la aplicación, debes publicar tu aplicación en la Tienda y descargar la aplicación en el dispositivo de desarrollo para usar su licencia para las pruebas como se ha descrito anteriormente.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Recibos de las compras desde la aplicación

El espacio de nombres **Windows.Services.Store** no proporciona ninguna API que puedas usar para recuperar un recibo de transacción de las compras que culminen correctamente en el código de tu aplicación. Se trata de una experiencia distinta de la de las aplicaciones que usan el espacio de nombres **Windows.ApplicationModel.Store**, que pueden [usar la API del lado cliente para obtener un recibo de transacción](use-receipts-to-verify-product-purchases.md).

Si implementas compras desde la aplicación con el espacio de nombres **Windows.Services.Store** y quieres comprobar si un cliente en particular ha comprado una aplicación o un complemento, puedes usar el [método de consulta de productos](query-for-products.md) en la [API de REST de colecciones de Microsoft Store](view-and-grant-products-from-a-service.md). Los datos que este método devuelve confirman si el cliente en cuestión tiene derecho a un producto determinado y proporciona datos de la transacción en la que el usuario compró el producto. La API de colecciones de Microsoft Store usa la autenticación de Azure AD para recuperar esta información.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Uso de la clase StoreContext con el puente de escritorio

Las aplicaciones de escritorio que usan el [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop) pueden utilizar la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para implementar compras desde la aplicación y periodos de pruebas. Sin embargo, si tienes una aplicación de escritorio Win32 o una con un identificador de ventana (HWND) asociado al marco de representación (por ejemplo, una aplicación WPF), la aplicación debe configurar el objeto **StoreContext** para especificar qué ventana de aplicación es la ventana propietaria en los cuadros de diálogo modales que el objeto muestra.

Muchos miembros de **StoreContext** (y los miembros de otros tipos relacionados a los que se accede a través del objeto **StoreContext**) muestran un cuadro de diálogo modal al usuario de las operaciones relacionadas con la Tienda, como comprar un producto. Si una aplicación de escritorio no configura el objeto **StoreContext** para que especifique la ventana propietaria de los cuadros de diálogo modales, este objeto devolverá datos incorrectos o errores.

Para configurar un objeto **StoreContext** en una aplicación de escritorio que usa el Puente de escritorio, sigue estos pasos.

1. Realiza una de las siguientes acciones para permitir que la aplicación acceda a la interfaz [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx):

    * Si tu aplicación está escrita en un lenguaje administrado, como C# o Visual Basic, declara la interfaz **IInitializeWithWindow** en el código de la aplicación con el atributo [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) como se muestra en el siguiente ejemplo de C#. En este ejemplo se da por hecho que el archivo de código tiene una instrucción **using** para el espacio de nombres **System.Runtime.InteropServices**.

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

2. Obtén un objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) con el método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) (o [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) si tu aplicación es [multiusuario](../xbox-apps/multi-user-applications.md)) tal como se describe anteriormente en este artículo y conviértelo en un objeto [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Después, llama al método [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) y pasa el identificador de la ventana que quieras que sea la propietaria de los cuadros de diálogo modales que muestren los métodos **StoreContext**. El siguiente ejemplo de C# muestra cómo pasar el identificador de la ventana principal de la aplicación al método.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Productos, SKU y disponibilidades

Cada producto de la Tienda tiene al menos una *SKU*, y cada SKU tiene al menos una *disponibilidad*. Estos conceptos se abstraen de la mayoría de los desarrolladores en el panel del Centro de desarrollo de Windows, y la mayoría de los desarrolladores nunca definirán las SKU ni las disponibilidades de sus aplicaciones o complementos. Sin embargo, dado que el modelo de objetos de los productos de la Tienda en el espacio de nombres **Windows.Services.Store** incluye las SKU y las disponibilidades, puede resultar útil disponer de un conocimiento básico de estos conceptos para algunos escenarios.

| Objeto |  Descripción  |
|---------|-------------------|
| Producto  |  Un *producto* hace referencia a cualquier tipo de producto que esté disponible en la Tienda, incluido un complemento o una aplicación. <p/><p/> Cada producto de la Tienda tiene un objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) correspondiente. Esta clase proporciona propiedades que puedes usar para acceder a datos, como el Id. de la Tienda del producto, las imágenes y los vídeos de descripción de la Tienda y la información de precios. También proporciona métodos que puedes usar para comprar el producto. |
| SKU |  Una *SKU* es una versión específica de un producto con su descripción, precio y otros detalles únicos del producto. Cada aplicación o complemento tiene una SKU predeterminada. La única vez que la mayoría de los desarrolladores tendrán varias SKU para una aplicación es si publican una versión completa de su aplicación y una versión de prueba (en el catálogo de la Tienda, cada una de estas versiones es una SKU diferente de la misma aplicación). <p/><p/> Algunos publicadores tienen la capacidad de definir sus propias SKU. Por ejemplo, un publicador de juegos importante puede lanzar un juego con una SKU que muestre sangre verde en mercados que no permitan sangre roja y otra SKU que muestre sangre roja en el resto de los mercados. Como alternativa, un publicador que venda contenido de vídeo digital podría publicar dos SKU para un vídeo, una SKU para la versión de alta definición y una SKU diferente para la versión de definición estándar. <p/><p/> Cada SKU de la Tienda tiene un objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) correspondiente. Cada [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) tiene una propiedad [Skus](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) que puedes usar para acceder a las SKU para el producto. |
| Disponibilidad  |  Una *disponibilidad* es una versión específica de una SKU con su propia información de precios única. Cada SKU tiene una disponibilidad predeterminada. Algunos publicadores tienen la capacidad de definir sus propias disponibilidades para presentar opciones de precios diferentes para una SKU determinada. <p/><p/> Cada disponibilidad de la Tienda tiene un objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) correspondiente. Cada [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) tiene una propiedad [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) que puedes usar para acceder a las disponibilidades para la SKU. Para la mayoría de los desarrolladores, cada SKU tiene la disponibilidad predeterminada única.  |

<span id="store_ids" />

### <a name="store-ids"></a>Id. de la Tienda

Cada aplicación, complemento u otro producto de la tienda tiene un **Id. de la Tienda** asociado (esto se denomina también a veces *identificador de la Tienda del producto*). Muchas API requieren el Id. de la Tienda para realizar una operación en una aplicación o un complemento.

El identificador de la Tienda de cualquier producto de la Tienda es una cadena de 12 caracteres alfanuméricos, como ```9NBLGGH4R315```. Hay varias maneras diferentes de obtener el id. de la tienda para un producto de la Tienda:

* Para una aplicación, puedes obtener el Id. de la Tienda en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo.
* Para un complemento, puedes obtener el id. de la Tienda en la página de introducción del complemento en el panel.
* Para cualquier producto, también puedes obtener el id. de la tienda mediante programación usando la propiedad [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) del objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) objeto que representa el producto.

Para los productos con SKU y disponibilidades, la SKU y las disponibilidades también tienen sus propios id. de tienda con diferentes formatos.

| Objeto |  Formato de identificador de la Tienda  |
|---------|-------------------|
| SKU |  Para una SKU, el id. de la Tienda tiene el formato ```<product Store ID>/xxxx```, donde ```xxxx``` es una cadena de 4 caracteres alfanuméricos que identifica una SKU del producto. Por ejemplo, ```9NBLGGH4R315/000N```. Este identificador lo devuelve la propiedad [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) de un objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) y, en ocasiones, se denomina *identificador de la Tienda de la SKU*. |
| Disponibilidad  |  Para una disponibilidad, el identificador de la Tienda tiene el formato ```<product Store ID>/xxxx/yyyyyyyyyyyy```, donde ```xxxx``` es una cadena de 4 caracteres alfanuméricos que identifica una SKU del producto e ```yyyyyyyyyyyy``` es una cadena de 12 caracteres alfanuméricos que identifica una disponibilidad de la SKU. Por ejemplo, ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Este identificador lo devuelve la propiedad [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid) de un objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) y, en ocasiones, se denomina *identificador de la Tienda de disponibilidad*.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Cómo usar los identificadores de producto para complementos en tu código

Si quieres que un complemento esté disponible para los clientes en el contexto de tu aplicación, debes [introducir un id. de producto único](../publish/set-your-add-on-product-id.md#product-id) para el complemento al [crear el envío de complemento](../publish/add-on-submissions.md) en el panel del Centro de desarrollo. Puedes usar este id. del producto para hacer referencia al complemento en tu código, aunque los escenarios específicos en los que puedes usar el identificador de producto dependen de qué espacio de nombres usas para las compras desde la aplicación en tu aplicación.

> [!NOTE]
> El id. del producto que especificas en el panel del Centro de desarrollo de un complemento es diferente del [id. de la Tienda ](#store-ids) del complemento. El id. de la Tienda lo genera el Centro de desarrollo.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Aplicaciones que usan el espacio de nombres Windows.Services.Store

Si la aplicación usa el espacio de nombres **Windows.Services.Store**, puedes usar el id. del producto para identificar fácilmente el [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) que representa el complemento o la [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) que representa licencia la licencia del complemento. El id. del producto se expone por las propiedades [StoreProduct.InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) y [StoreLicense.InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken).

> [!NOTE]
> Aunque el id. del producto es útil para identificar un complemento en el código, la mayoría de las operaciones del espacio de nombres **Windows.Services.Store** usan el [Id. de la tienda](#store-ids) de un complemento en lugar del id. del producto. Por ejemplo, para recuperar mediante programación uno o más complementos conocidos de una aplicación, pasa los id. de la Tienda (en lugar de los id. del producto) de los complementos al método [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Del mismo modo, para notificar un complemento consumible como completado, pasa el id. de la Tienda del complemento (en lugar del id. del producto) al método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Aplicaciones que usan el espacio de nombres Windows.ApplicationModel.Store

Si la aplicación usa el espacio de nombres **Windows.ApplicationModel.Store**, tendrás que usar el id. del producto que asignas a un complemento en el panel del Centro de desarrollo para la mayoría de las operaciones. Por ejemplo:

* Usa el id. del producto para identificar la [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) que representa el complemento o la [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) que representa la licencia de tu complemento. El id. del producto se expone por las propiedades [ProductListing.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) y [ProductLicense.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId).

* Especifica el id. del producto al comprar el complemento para el usuario mediante el método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync). Para obtener más información, consulta [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md).

* Especifica el id. del producto al notificar el complemento consumible como completado mediante el método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync). Para obtener más información, consulta [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Temas relacionados

* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de una suscripción para tu aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
* [Códigos de error para las operaciones de Microsoft Store](error-codes-for-store-operations.md)
* [Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
