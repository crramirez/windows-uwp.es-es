---
author: joannaleecy
title: Configurar el proyecto de juego
description: El primer paso para ensamblar el juego es configurar un proyecto en Microsoft Visual Studio de tal forma que se reduzca al mínimo la cantidad de trabajo necesaria en la infraestructura de código.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, el programa de instalación, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9100e80e0b4ac436ae872698e94fe29e5c8cab46
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7105449"
---
# <a name="set-up-the-game-project"></a>Configurar el proyecto de juego

Este artículo analiza cómo configurar un juego de DirectX para UWP sencillo con las plantillas de Visual Studio. El primer paso para ensamblar el juego es configurar un proyecto en Microsoft Visual Studio de tal forma que se reduzca al mínimo la cantidad de trabajo necesaria en la infraestructura de código. Aprende a ahorrar tiempo de configuración al usar la plantilla adecuada y configurar el proyecto específicamente para desarrollar un juego.

## <a name="objectives"></a>Objetivos

* Configura un proyecto de juego Direct3D en Visual Studio usando una plantilla.
* Comprende el punto de entrada principal del juego examinando los archivos de origen de la **Aplicación**.
* Revisa el archivo **package.appxmanifest** del proyecto
* Averigua qué herramientas y asistencia de desarrollo de juegos están incluidos con el proyecto

## <a name="how-to-set-up-the-game-project"></a>Cómo configurar el proyecto de juego

Si no estás familiarizado con el desarrollo de la Plataforma Universal de Windows (UWP), te recomendamos el uso de plantillas de Visual Studio para configurar la estructura de código básica.

>[!Note]
>Este artículo es parte de una serie de tutoriales basados en un juego de muestra. Puedes obtener el código más reciente en [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

### <a name="use-directx-template-to-create-a-project"></a>Usa la plantilla de DirectX para crear un proyecto

Una plantilla de Visual Studio es una colección de configuraciones y archivos de código para un tipo específico de aplicación basándose en el lenguaje y la tecnología elegidos. En Microsoft Studio2017 Visual, encontrarás varias plantillas que pueden facilitar enormemente el desarrollo de aplicaciones de juegos y gráficos. Si no usas una plantilla, debes desarrollar tú mismo casi todo el marco básico de generación de gráficos, lo que puede suponer una ardua tarea para un desarrollador de juegos novel.

La plantilla usada para este tutorial es la titulada **DirectX 11 App (Universal Windows)**. 

Pasos para crear un proyecto de juego DirectX 11 en Visual Studio:
1.  Selecciona **Archivo...** &gt; **Nuevo**  &gt; **Proyecto...**
2.  En el panel izquierdo, selecciona **Instalado** &gt; **Plantillas** &gt; **Visual C++** &gt; **Windows Universal**
3.  En el panel central, selecciona **DirectX 11 App (Universal Windows)**
4.  Da a tu proyecto un nombre y haz clic en **Aceptar**.

![captura de pantalla que muestra cómo seleccionar la plantilla directx11 para crear un nuevo proyecto de juego](images/simple-dx-game-setup-new-project.png)

Esta plantilla te proporciona el marco básico para una aplicación para UWP usando DirectX con C++. Haz clic en F5 para compilarla y ejecutarla. Fíjate en esa pantalla azul polvo. La plantilla crea varios archivos de código que contienen la funcionalidad básica de una aplicación para UWP con DirectX y C++.

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>Revisa el punto de entrada principal de la aplicación entendiendo IFrameworkView

La clase **App** se hereda de la clase **IFrameworkView**.

### <a name="inspect-apph"></a>Inspecciona **App.h**.

Observemos rápidamente estos 5 métodos en **App.h** &mdash; [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) y [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) al implementar la interfaz [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) que define un proveedor de vista. Estos métodos los ejecuta el singleton de la aplicación que se crea al lanzar el juego, y carga todos los recursos de la aplicación, además de conectar los controladores de eventos apropiados.

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        ...
    };
```

### <a name="inspect-appcpp"></a>Inspecciona **App.cpp**

Este es el método **principal** en el archivo de origen **App.cpp**:

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

En este método, crea una instancia del proveedor de vista Direct3D a partir de la fábrica del proveedor de vista (**Direct3DApplicationSource**, definido en **App.h**) y la pasa al singleton de la aplicación llamando a ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). Esto significa que el punto de partida de tu juego vivirá en el cuerpo de la implementación del método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) y, en este caso, es el **App::Run**. 

Desplázate hasta encontrar el método **App:: Run** en **App.cpp**. Aquí te mostramos el código:

```cpp
//This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Lo que hace este método: si la ventana de tu juego no está cerrada, distribuirá todos los eventos, actualizará el temporizador y representará los resultados de tu canalización de gráficos. Hablaremos sobre esto en mayor detalle en [Definir el marco de la aplicación para UWP](tutorial--building-the-games-uwp-app-framework.md), [Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md) y [Marco de representación II: Representación de juego](tutorial-game-rendering.md). Llegados a este punto, deberías tener una idea de la estructura de código básica de un juego DirectX de UWP.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Revisa y actualiza el archivo package.appxmanifest


La plantilla no solo consiste en archivos de código. El archivo **Package.appxmanifest** contiene metadatos de tu proyecto que sirven para empaquetar y lanzar el juego, así como para enviarlo a Microsoft Store. También contiene información importante que el sistema del jugador usa para proporcionar acceso a los recursos del sistema que el programa necesita para ejecutarse.

Ejecuta el **Diseñador de manifiestos** haciendo doble clic en el archivo **Package.appxmanifest** en el **Explorador de soluciones**.

![captura de pantalla del editor del manifiesto package.appx.](images/simple-dx-game-setup-app-manifest.png)

Para más información sobre el archivo **package.appxmanifest** y el empaquetado, consulta el [Diseñador de manifiestos](https://msdn.microsoft.com/library/windows/apps/br230259.aspx). Por ahora, echa un vistazo a la pestaña **Capacidades** y mira las opciones proporcionadas.

![captura de pantalla con las capacidades predeterminadas de una aplicación direct3d.](images/simple-dx-game-setup-capabilities.png)

Si no seleccionas las funcionalidades que usa tu juego, como el acceso a **Internet** para los juegos locales guardados, o Internet para ver las puntuaciones más altas globales, no podrás tener acceso a los recursos o características correspondientes. Cuando crees un nuevo juego, asegúrate de que seleccionas las funcionalidades que necesitará el juego para ejecutarse.

Ahora, veamos el resto de archivos que vienen con la plantilla **DirectX11 App (Universal Windows)**.

## <a name="review-the-included-libraries-and-headers"></a>Revisa las bibliotecas y los encabezados incluidos

Hay varios archivos que aún no hemos visto. Estos archivos proporcionan herramientas y soporte adicionales comunes a escenarios de desarrollo de juegos Direct3D. Para la lista completa de archivos que se incluyen con el proyecto de juego básico de DirectX, consulta [Plantillas de proyectos de juegos DirectX](user-interface.md#template-structure).

| Archivo de código fuente de plantilla         | Carpeta de archivos            | descripción |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | Común                 | Define un objeto de clase que controla todos los [recursos de dispositivo](tutorial--assembling-the-rendering-pipeline.md#resource) DirectX. También incluye una interfaz para la aplicación que posee DeviceResources para que se le notifique cuando se pierde o se crea el dispositivo.                                                |
| DirectXHelper.h              | Común                 | Implementa métodos, incluyendo **DX::ThrowIfFailed**, **ReadDataAsync** y **ConvertDipsToPixels. **DX::ThrowIfFailed** convierte los valores de errores HRESULT devueltos por las API de DirectX Win32 en excepciones de Windows Runtime. Usa este método para colocar un punto de interrupción para depurar errores de DirectX. Para obtener más información, consulta [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed). **ReadDataAsync** lee desde un archivo binario de forma asincrónica. **ConvertDipsToPixels** convierte una longitud en píxeles independientes del dispositivo (DIP) a una longitud en píxeles físicos. |
| StepTimer.h                  | Común                 | Define un temporizador de alta resolución útil para aplicaciones de representación de juego o interactivas.   |
| Sample3DSceneRenderer.h/.cpp | contenido                | Define un objeto de clase para crear una instancia de una canalización de representación básica. Crea una implementación de representación básica que conecta una cadena de intercambio Direct3D y un adaptador gráfico a tu UWP con DirectX.   |
| SampleFPSTextRenderer.h/.cpp | Contenido                | Define un objeto de clase para representar el valor actual de fotogramas por segundo (FPS) en la parte inferior derecha de la pantalla con Direct2D y DirectWrite.  |
| SamplePixelShader.hlsl       | Contenido                | Contiene el código de lenguaje de sombreado de alto nivel (HLSL) para un sombreador de píxeles muy básico.                                            |
| SampleVertexShader.hlsl      | Contenido                | Contiene el código de lenguaje de sombreado de alto nivel (HLSL) para un sombreador de vértices muy básico.                                           |
| ShaderStructures.h           | Contenido                | Contiene las estructuras de sombreador que pueden usarse para enviar las matrices de MVP y los datos por vértice al sombreador de vértices.  |
| pch.h/.cpp                   | Principal                   | Contiene todos los archivos de inclusión del sistema Windows para las API usadas por una aplicación Direct3D, incluidas las API de DirectX11.| 

### <a name="next-steps"></a>Pasos siguientes

En este punto, has aprendido a crear un proyecto de juego de DirectX de UWP mediante la plantilla **DirectX 11 App (Universal Windows)** y has recibido información sobre algunos componentes y archivos proporcionados por el proyecto.

La siguiente sección es [Definir el marco de UWP del juego](tutorial--building-the-games-uwp-app-framework.md). Examinaremos cómo usa y amplía este juego muchos de los conceptos y componentes proporcionados por la plantilla.

 

 




