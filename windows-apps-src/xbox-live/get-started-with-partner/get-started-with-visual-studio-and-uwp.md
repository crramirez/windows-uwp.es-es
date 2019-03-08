---
title: Introducción a Visual Studio para juegos UWP
description: Obtenga información sobre cómo configurar un proyecto de Visual Studio para habilitar Xbox Live para un juego para UWP
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 314d5e937bb8680dc26b7dfdfa15ff90f8f26c81
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651410"
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>Introducción al uso de Visual Studio para juegos UWP

## <a name="requirements"></a>Requisitos

1. Inscripción en el  **[programa para desarrolladores de centro de partners](https://developer.microsoft.com/store/register)**.
2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**  con el **herramientas de desarrollo de aplicaciones de Windows Universal**. La versión mínima necesaria para aplicaciones UWP es Visual Studio 2015 Update 3. Se recomienda que utilice la versión más reciente de Visual Studio para desarrolladores y actualizaciones de seguridad. 
4. **[SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** o una versión posterior.

> [!IMPORTANT]
> Visual Studio 2017 es necesaria si usa Windows 10 SDK versión 10.0.15063.0 (también conocida como Creators Update) o una versión posterior.

## <a name="create-a-new-product-in-partner-center"></a>Crear un nuevo producto en el centro de partners

Cada título de Xbox Live debe tener un producto que creó en [centro de partners](https://partner.microsoft.com/dashboard) antes de poder iniciar sesión y realizar llamadas de servicio de Xbox Live. Consulte [crear un título en UDC](create-a-new-title.md) para obtener más información.

## <a name="configuring-your-development-device"></a>Configurar el dispositivo de desarrollo

Los siguientes pasos de instalación preliminar son necesarios en el dispositivo, para que pueda correctamente el inicio de sesión con Xbox Live y llamar a los diversos servicios de Xbox Live.

### <a name="set-your-sandbox"></a>Establecer el espacio aislado

Los espacios aislados ofrecen una manera de mantener su [configuración del servicio de Xbox Live](../xbox-live-service-configuration.md) aislada de minorista hasta que esté listo para liberar su título. Algunos datos que se recopilan son específicos a un espacio aislado. Por ejemplo supongamos que su título define un estado llamado *Headshots*, y acumular un número de Headshots en una cuenta de usuario durante las pruebas de su título. Este valor sería específico para el espacio aislado de desarrollo de su título y, si ha cambiado a reproducir la versión comercial de su título, el headshots no llevaría a través.

Consulte la [espacios aislados de Xbox Live](../xbox-live-sandboxes.md) artículo para obtener más información y ver cómo establecer el espacio aislado.

### <a name="sign-in-with-a-test-account"></a>Inicie sesión con una cuenta de prueba

Para iniciar sesión en su espacio aislado de desarrollo, debe crear una cuenta de prueba o aprovisionar una cuenta Microsoft (MSA) normal para el acceso a su espacio aislado. Esto proporciona seguridad mejorada para los títulos en desarrollo, así como algunas otras ventajas.

Para obtener más información acerca de las cuentas de prueba y cómo crearla, vea [Xbox Live Test Accounts](../xbox-live-test-accounts.md)

## <a name="visual-studio-project-setup"></a>Configuración del proyecto de Visual Studio

### <a name="1-open-a-uwp-project"></a>1. Abra un proyecto de UWP
Si no dispone de un proyecto existente de UWP, puede crearla haciendo lo siguiente:

1. En Visual Studio, **archivo** > **nueva** > **proyecto**.
2. En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C#**   >  **Windows** > **Universal** nodo en el panel izquierdo y haga clic en **aplicación vacía (Windows Universal)** en el panel derecho.
3. En la parte inferior del cuadro de diálogo, asigne un nombre al proyecto y especifique la ubicación del proyecto.
4. Especificar la versión de destino y la versión mínima de Windows 10 SDK. Consulte [elegir una versión de UWP](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) para obtener más información.

![crear el proyecto en Visual Studio](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> Xbox Live API (XSAPI) requiere una versión mínima 10.0.10586.0 o superior.

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. Agregue referencias a Xbox Live API (XSAPI) en el proyecto

La API de servicios de Xbox incluye versiones para UWP y XDK y de C++ y WinRT y ha su espacio de nombres estructurados como **Microsoft.Xbox.Live.SDK.*. UWP** y **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** está dirigido a desarrolladores que crean un juego para UWP, que puede ejecutarse en PC, la Xbox One o de Windows Phone.
2. **XboxOneXDK** es para ID@Xbox y administra los desarrolladores que usan el XDK una Xbox.
3. El SDK de C++ puede usarse para motores de juegos de C++, mientras que el SDK de WinRT es para los motores de juegos escritos en C++, C#, o JavaScript.
4. Al usar WinRT con un motor de C++, debe usar C++ / c++ / CX que utiliza sombreros (^). C++ es la API que se recomienda que se usará para los motores de juegos de C++.  

> [!TIP]
> Puede leer más sobre la ejecución de UWP en Xbox One en [Introducción al desarrollo de aplicaciones para UWP en Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Para usar la API de Xbox Live desde el proyecto, puede agregar referencias a los archivos binarios mediante paquetes de NuGet o agregando el origen de la API. Agregar paquetes de NuGet agiliza la compilación mientras que agregar el origen hace que la depuración sea más sencilla. En este artículo le guiará a través del uso de paquetes de NuGet. Si desea usar el código fuente, consulte [compilar la Xbox Live las API de origen en su proyecto de UWP](add-xbox-live-apis-source-to-a-uwp-project.md). Puede agregar el paquete de NuGet del SDK de Xbox Live:

1. En Visual Studio, vaya a **herramientas** > **Administrador de paquetes de NuGet** > **administrar paquetes de NuGet para la solución...** .
2. En el Administrador de paquetes de NuGet, haga clic en **examinar** y escriba **Xbox.Live.SDK** en el cuadro de búsqueda.
3. Seleccione la versión de Xbox Live SDK que desea usar en la lista de la izquierda. En este caso, usamos el paquete Microsoft.Xbox.Live.SDK.WinRT.UWP.
3. En el lado derecho de la ventana, active la casilla situada junto a su proyecto y haga clic en **instalar**.

![Agregar XBL a través de NuGet](../images/getting_started/vs-add-nuget-xbl.gif)


> [!IMPORTANT]
> Para `Microsoft.Xbox.Live.SDK.Cpp.*` en función de los proyectos, asegúrese de incluir el encabezado `#include <xsapi\services.h>` en código fuente del proyecto.

### <a name="3-optional-using-connected-storage-andor-secure-sockets"></a>3. (Opcional) Uso de almacenamiento conectados o Sockets seguros
Según la versión del SDK de Windows que está usando, es posible que deba instalar contenido adicional o agregar manualmente referencias a un proyecto con el fin de usar Xbox Live [almacenamiento conectado](../storage-platform/connected-storage/connected-storage-technical-overview.md) o Sockets seguros. Si desea usar la característica de almacenamiento conectado, deberá tener acceso a la `Windows.Gaming.XboxLive.Storage` espacio de nombres. Si desea usar Sockets seguros, deberá tener acceso a `Windows.Networking.XboxLive`.

#### <a name="windows-10-sdk-version-10016299-or-higher"></a>SDK de Windows 10 versión 10.0.16299 o versiones posteriores
Si ha especificado el SDK de Windows 10 10.0.16299 o versiones posteriores, a continuación, podrá tener acceso al espacio de nombres de almacenamiento conectados sin tener que realizar ningún trabajo adicional. Para obtener acceso a Secure Sockets, deberá agregar una referencia a **extensiones de escritorio de Windows para UWP**. Puede hacerlo:

1. En el **el Explorador de soluciones**, haga clic con el botón derecho en el **referencias** nodo y seleccionar **Agregar referencia...**
2. En el lado izquierdo de la **Administrador de referencias** cuadro de diálogo, seleccione **Windows Universal** > **extensiones**.
3. En la lista que aparece, busque **extensiones de escritorio de Windows para UWP** y Active la casilla situada junto a la versión que coincida con el SDK de Windows 10.
4. Haga clic en **Aceptar**.

![Agregar nueva referencia en Visual Studio](../images/getting_started/get-started-vs-add-ref.png)

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Versión del SDK de Windows 10 10.0.15063 o inferior
Si desea usar el almacenamiento conectado o Sockets seguros, deberá instalar el SDK de extensiones de Xbox Live plataformas antes de poder agregar referencias al proyecto. Puede hacerlo:

1. Descargue y extraiga el [extensiones de SDK de plataforma de Xbox Live](https://aka.ms/xblextsdk).
2. Una vez extraídos, ejecute el archivo MSI incluido que coincida con la versión de SDK de Windows 10 que está usando.

Después de haber instalado el SDK de extensiones de Xbox Live Platform, deberá agregar una referencia a él en Visual Studio. Puede hacerlo:

1. En el **el Explorador de soluciones**, haga clic con el botón derecho en el **referencias** nodo y seleccionar **Agregar referencia...**
2. En el lado izquierdo de la **Administrador de referencias** cuadro de diálogo, seleccione **Windows Universal** > **extensiones**.
3. En la lista que aparece, busque **extensiones de escritorio de Windows para UWP** y Active la casilla situada junto a la versión que coincida con el SDK de Windows 10.
4. Haga clic en **Aceptar**.

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. Asociar el proyecto de Visual Studio con la aplicación para UWP

Para su juego poder iniciar sesión, debe estar asociada con el producto que creó en el centro de partners. Puede asociar su juego en Visual Studio mediante el Asistente para la asociación de Store. En Visual Studio, haga lo siguiente:

1.  A la derecha, haga clic en el proyecto principal (el proyecto de inicio), haga clic en **Store** > **asociar aplicación con el Store...**
2.  Inicie sesión con la **cuenta de desarrollador de Windows** utilizado para crear la aplicación si se le pida y siga las indicaciones.

> [!TIP]
> Consulte [empaquetar aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/) para obtener más información sobre la preparación de su juego para Windows Store.

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. Agregar capacidades de Internet a su proyecto de Visual Studio
El proyecto UWP deberá especificar las capacidades de internet para comunicarse con Xbox Live. Se pueden establecer estas propiedades:

1. Haga doble clic en el **package.appxmanifest** archivo en Visual Studio para abrir el **Diseñador de manifiestos**.
2. Haga clic en el **capacidades** pestaña y compruebe **Internet (cliente)**.

![Agregar nueva referencia en Visual Studio](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. Asociar el proyecto de Visual Studio con el título de Xbox Live habilitado

Para comunicarse con el servicio Xbox Live, deberá agregar un archivo de configuración de servicio al proyecto. Esto puede hacerse fácilmente:

1. En su proyecto de inicio, haga clic y seleccione **agregar** > **nuevo elemento**.
2. Seleccione el **archivo de texto** escriba y asígnele el nombre **xboxservices.config**.
3. Haga clic con el botón derecho en el archivo, seleccione **propiedades** y asegúrese de que:
    1. **Acción de compilación** está establecido en **contenido**, y  
    2. **Copiar en el directorio de salida** está establecido en **copiar siempre**.
5.  Editar el archivo de configuración con la plantilla siguiente, reemplazando el **TitleId** y **PrimaryServiceConfigId** con los valores aplicables a su título. Puede obtener los valores correctos de la página de Xbox Live raíz en el centro de partners. El **PrimaryServiceConfigId** aparece en el centro de partners como **¿SCID**.

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID"
    }
```

Por ejemplo:

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

> [!TIP]
> Todos los valores dentro de xboxservices.config distinguen mayúsculas de minúsculas. Consulte [configuración del servicio](../xbox-live-service-configuration.md) para obtener más información sobre cómo obtener el TitleID y PrimaryServiceConfigId.

### <a name="7-optional-add-multiplayer-capabilities"></a>7. (Opcional) Agregar capacidades de varios jugadores

Si tiene previsto agregar participan varios jugadores admite en su título y desea implementar la capacidad para reproductores para invitar a otros usuarios para un juego multijugador, será preciso agregar otro campo a su archivo AppXManifest. Consulte [configurar su AppXManifest para varios jugadores](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md) para obtener más información.

## <a name="learn-more"></a>Más información

El [ejemplos del SDK de Xbox Live](https://github.com/Microsoft/xbox-live-samples) son una buena forma de ver cómo se usan las API de Xbox Live e incluye las API disponibles para los desarrolladores en el ID@Xbox programa. Para usar los ejemplos, deberá cambiar su espacio aislado a XDKS.1.
