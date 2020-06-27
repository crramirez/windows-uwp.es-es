---
title: Configurar el proyecto de juego
description: El primer paso para desarrollar el juego consiste en configurar un proyecto en Microsoft Visual Studio. Después de configurar un proyecto específicamente para el desarrollo de juegos, puede volver a usarlo como un tipo de plantilla.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, juegos, configuración, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: c0c8d82752d9b73a3e3e7ffcec3ced1515564072
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409594"
---
# <a name="set-up-the-game-project"></a>Configurar el proyecto de juego

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

El primer paso para desarrollar el juego es crear un proyecto en Microsoft Visual Studio. Después de configurar un proyecto específicamente para el desarrollo de juegos, puede volver a usarlo como un tipo de plantilla.

## <a name="objectives"></a>Objetivos

* Cree un nuevo proyecto en Visual Studio mediante una plantilla de proyecto.
* Comprenda el punto de entrada y la inicialización del juego examinando el archivo de código fuente de la clase **App** .
* Fíjese en el bucle del juego.
* Revise el archivo **Package. appxmanifest** del proyecto.

## <a name="create-a-new-project-in-visual-studio"></a>Creación de un proyecto en Visual Studio

> [!NOTE]
> Para más información sobre cómo instalar Visual Studio para la implementación de C++/WinRT &mdash;incluida la instalación y el uso de la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación)&mdash;, consulta [Compatibilidad de Visual Studio para C++/WinRT](/windows/uwp/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

En primer lugar, instale (o actualice a) la versión más reciente de la extensión de Visual Studio (VSIX) de C++/WinRT. Vea la nota anterior. A continuación, en Visual Studio, cree un nuevo proyecto basado en la plantilla de proyecto de la **aplicación principal (C++/WinRT)** . Elija como destino la versión más reciente disponible de manera general (es decir, no en versión preliminar) de Windows SDK.

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>Revise la clase **App** para entender **IFrameworkViewSource** y **IFrameworkView** .

En el proyecto de aplicación principal, abra el archivo de código fuente `App.cpp` . En, hay la implementación de la clase **App** , que representa la aplicación y su ciclo de vida. En este caso, por supuesto, sabemos que la aplicación es un juego. Pero nos referiremos a ella como una *aplicación* para hablar más en general sobre cómo se inicializa una aplicación plataforma universal de Windows (UWP).

### <a name="the-wwinmain-function"></a>Función wWinMain

La función **wWinMain** es el punto de entrada de la aplicación. Este es el aspecto de **wWinMain** (desde `App.cpp` ).

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

Creamos una instancia de la clase **App** (que es la única instancia de la **aplicación** que se creó) y la pasamos al método [**CoreApplication. Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) estático. Tenga en cuenta que **CoreApplication. Run** espera una interfaz [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Por lo tanto, la clase de **aplicación** debe implementar esa interfaz.

En las dos secciones siguientes de este tema se describen las interfaces [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) y [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Esas interfaces (así como **CoreApplication. Run**) representan una forma para que la aplicación suministre Windows con un *proveedor de vistas*. Windows usa ese proveedor de vistas para conectar la aplicación con el shell de Windows para que pueda controlar los eventos de ciclo de vida de la aplicación.

### <a name="the-iframeworkviewsource-interface"></a>La interfaz IFrameworkViewSource

La clase de **aplicación** implementa [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource), como puede ver en la siguiente lista.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

Un objeto que implementa **IFrameworkViewSource** es un objeto *de generador de proveedores de vistas* . El trabajo de ese objeto es fabricar y devolver un objeto de *proveedor de vista* .

**IFrameworkViewSource** tiene el método único [**IFrameworkViewSource:: CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview). Windows llama a esa función en el objeto que se pasa a **CoreApplication. Run**. Como puede ver más arriba, se devuelve la implementación **App:: CreateView** de ese método `*this` . En otras palabras, el objeto de **aplicación** se devuelve a sí mismo. Puesto que **IFrameworkViewSource:: CreateView** tiene un tipo de valor devuelto de [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource), se sigue que la clase de **aplicación** también tiene que implementar *esa* interfaz. Y puede ver que en la lista anterior.

### <a name="the-iframeworkview-interface"></a>Interfaz IFrameworkView

Un objeto que implementa [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) es un objeto *de proveedor de vista* . Y ahora hemos proporcionado Windows con ese proveedor de vistas. Es el mismo objeto de **aplicación** que hemos creado en **wWinMain**. Por lo tanto, la clase **App** actúa como *generador de proveedores de vistas* y como *proveedor de vistas*.

Ahora Windows puede llamar a las implementaciones de la clase de **aplicación** de los métodos de **IFrameworkView**. En las implementaciones de esos métodos, la aplicación tiene la oportunidad de realizar tareas como la inicialización, para empezar a cargar los recursos necesarios, para conectar los controladores de eventos adecuados y para recibir el [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) que la aplicación usará para mostrar su salida.

Las implementaciones de los métodos de **IFrameworkView** se llaman en el orden que se muestra a continuación.

- [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**Cargar**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- Se genera el evento [**CoreApplicationView:: Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) . Por lo tanto, si se ha registrado (opcionalmente) para controlar ese evento, se llama al controlador **onactivated** en este momento.
- [**Realizar**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**Anular**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

Y este es el esqueleto de la clase **App** (en `App.cpp` ), que muestra las firmas de esos métodos.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::CoreCoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::CoreCoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

Esto fue solo una introducción a **IFrameworkView**. En [definir el marco de trabajo de la aplicación para UWP del juego](tutorial--building-the-games-uwp-app-framework.md), veremos mucho más detalles sobre estos métodos y cómo implementarlos.

### <a name="tidy-up-the-project"></a>Preparar el proyecto

El proyecto de aplicación principal que creó a partir de la plantilla de proyecto contiene una funcionalidad que debemos poner al día en este momento. Después, podemos usar el proyecto para volver a crear el juego de la galería de disparos (**Simple3DGameDX**). Realice los cambios siguientes en la clase **App** en `App.cpp` .

- Elimine sus miembros de datos.
- Elimine **OnPointerPressed**, **OnPointerMoved**y **AddVisual**
- Elimine el código de **SetWindow**.

El proyecto se compilará y ejecutará, pero solo mostrará un color sólido en el área de cliente.

## <a name="the-game-loop"></a>El bucle del juego

Para hacerse una idea de lo que es un bucle de juego, busque en el código fuente del juego de ejemplo [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que descargó.

La clase **App** tiene un miembro de datos, denominado *m_main*, de tipo **GameMain**. Y ese miembro se usa en **App:: Run** de este modo.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Puede encontrar **GameMain:: Run** en `GameMain.cpp` . Es el bucle principal del juego y este es un esquema muy aproximado que muestra las características más importantes.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Y esta es una breve descripción de lo que hace este bucle de juego principal.

Si la ventana del juego no está cerrada, envíe todos los eventos, actualice el temporizador y, a continuación, represente y presente los resultados de la canalización de gráficos. Hay mucho más que decir sobre esos problemas y lo haremos en los temas [definir el marco de trabajo de la aplicación para UWP del juego, el](tutorial--building-the-games-uwp-app-framework.md) [marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md)y la representación del [marco II: representación de juegos](tutorial-game-rendering.md). Pero esta es la estructura de código básica de un juego DirectX de UWP.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Revisar y actualizar el archivo package. appxmanifest

El archivo **Package. appxmanifest** contiene metadatos sobre un proyecto de UWP. Estos metadatos se usan para empaquetar y iniciar el juego, y para enviarlos al Microsoft Store. El archivo también contiene información importante que el sistema del reproductor usa para proporcionar acceso a los recursos del sistema que el juego tiene que ejecutar.

Para iniciar el **Diseñador de manifiestos** , haga doble clic en el archivo **Package. appxmanifest** en **Explorador de soluciones**.

![captura de pantalla del editor de manifiestos de Package. appx.](images/simple-dx-game-setup-app-manifest.png)

Para más información sobre el archivo **package.appxmanifest** y el empaquetado, consulta el [Diseñador de manifiestos](/previous-versions/br230259(v=vs.140)). Por ahora, eche un vistazo a la pestaña **capacidades** y examine las opciones proporcionadas.

![captura de pantalla con las funciones predeterminadas de una aplicación Direct3D.](images/simple-dx-game-setup-capabilities.png)

Si no selecciona las funcionalidades que usa el juego, como el acceso a **Internet** para el panel global de puntuación alta, no podrá tener acceso a las características y los recursos correspondientes. Cuando cree un nuevo juego, asegúrese de seleccionar las capacidades que necesiten las API a las que llama el juego.

Ahora veamos el resto de los archivos que se incorporan con la plantilla de **aplicación DirectX 11 (Windows universal)** .

## <a name="review-other-important-libraries-and-source-code-files"></a>Revisar otras bibliotecas importantes y archivos de código fuente

Si piensa crear un tipo de plantilla de proyecto de juego para sí mismo, de modo que pueda volver a usarlo como punto de partida para futuros proyectos, querrá copiar `GameMain.h` y `GameMain.cpp` salir del proyecto de [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que descargó y agregarlos al nuevo proyecto de aplicación principal. Estudie esos archivos, obtenga información sobre lo que hacen y quite cualquier elemento específico de **Simple3DGameDX**. También comente cualquier cosa que dependa del código que todavía no ha copiado. Solo a modo de ejemplo, `GameMain.h` depende de `GameRenderer.h` . Podrá quitar la marca de comentario cuando copie más archivos de **Simple3DGameDX**.

Esta es una breve encuesta de algunos de los archivos de **Simple3DGameDX** que le resultará útil incluir en la plantilla, si está realizando una. En cualquier caso, son igualmente importantes para entender cómo funciona **Simple3DGameDX** .

|Archivo de origen|Carpeta de archivos|Descripción|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources. h/. cpp|Sectores públicos|Define la clase **DeviceResources** , que controla todos los [recursos de dispositivo](tutorial--assembling-the-rendering-pipeline.md#resource)de DirectX. También define la interfaz **IDeviceNotify** , que se usa para notificar a la aplicación que el dispositivo de adaptador de gráficos se ha perdido o se ha vuelto a crear.|
|DirectXSample.h|Sectores públicos|Implementa funciones auxiliares, como **ConvertDipsToPixels**. **ConvertDipsToPixels** convierte una longitud en píxeles independientes del dispositivo (DIP) a una longitud en píxeles físicos.|
|GameTimer. h/. cpp|Sectores públicos|Define un temporizador de alta resolución útil para aplicaciones de representación de juego o interactivas.|
|GameRenderer.h/.cpp|Representación|Define la clase **GameRenderer** , que implementa una canalización de representación básica.|
|GameHud. h/. cpp|Representación|Define una clase para representar una pantalla emergente (HUD) para el juego, mediante Direct2D y DirectWrite.|
|VertexShader. HLSL y VertexShaderFlat. HLSL|Sombreadores|Contiene el código de lenguaje HLSL (High-Level Shader Language) para los sombreadores de vértices básicos.|
|U. HLSL y PixelShaderFlat. HLSL|Sombreadores|Contiene el código de lenguaje HLSL (High-Level Shader Language) para los sombreadores de píxeles básicos.|
|ConstantBuffers.hlsli|Sombreadores|Contiene las definiciones de la estructura de datos para los búferes de constantes y estructuras de sombreador que se usan para pasar matrices de modelo-vista-proyección (MVP) y datos por vértice al sombreador de vértices.|
|pch.h/.cpp|N/D|Contiene Common C++/WinRT, Windows y DirectX includes.| 

### <a name="next-steps"></a>Pasos siguientes

En este punto, hemos mostrado cómo crear un nuevo proyecto de UWP para un juego de DirectX, hemos examinado algunas de las partes que contenían y hemos empezado a pensar en cómo convertir ese proyecto en un tipo de plantilla reutilizable para juegos. También hemos analizado algunas de las partes importantes del juego de ejemplo **Simple3DGameDX** .

En la sección siguiente se [define el marco de trabajo de la aplicación para UWP del juego](tutorial--building-the-games-uwp-app-framework.md). Aquí veremos con más detalle cómo funciona **Simple3DGameDX** .
