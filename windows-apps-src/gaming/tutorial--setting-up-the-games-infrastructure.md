---
title: Configurar el proyecto de juego
description: El primer paso para ensamblar el juego es configurar un proyecto en Microsoft Visual Studio de tal forma que se reduzca al mínimo la cantidad de trabajo necesaria en la infraestructura de código.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, el programa de instalación, directx
ms.localizationpriority: medium
ms.openlocfilehash: ca91926ec374015eeb88be6d89d3e1741d8b9c6d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367683"
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

Una plantilla de Visual Studio es una colección de configuraciones y archivos de código para un tipo específico de aplicación basándose en el lenguaje y la tecnología elegidos. En Microsoft Visual Studio 2017, encontrará una serie de plantillas que puede facilitar considerablemente desarrollo de aplicaciones de juegos y gráficos. Si no usas una plantilla, debes desarrollar tú mismo casi todo el marco básico de generación de gráficos, lo que puede suponer una ardua tarea para un desarrollador de juegos novel.

La plantilla usada para este tutorial es la titulada **DirectX 11 App (Universal Windows)** . 

Pasos para crear un proyecto de juego DirectX 11 en Visual Studio:
1.  Seleccione **archivo...** &gt; **Nuevo** &gt; **proyecto...**
2.  En el panel izquierdo, seleccione **instalado** &gt; **plantillas** &gt; **Visual C++** &gt; **Universal de Windows**
3.  En el panel central, selecciona **DirectX 11 App (Universal Windows)**
4.  Da a tu proyecto un nombre y haz clic en **Aceptar**.

![captura de pantalla que muestra cómo seleccionar la plantilla directx11 para crear un nuevo proyecto de juego](images/simple-dx-game-setup-new-project.png)

Esta plantilla te proporciona el marco básico para una aplicación para UWP usando DirectX con C++. Haz clic en F5 para compilarla y ejecutarla. Fíjate en esa pantalla azul polvo. La plantilla crea varios archivos de código que contienen la funcionalidad básica de una aplicación para UWP con DirectX y C++.

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>Revisa el punto de entrada principal de la aplicación entendiendo IFrameworkView

La clase **App** se hereda de la clase **IFrameworkView**.

### <a name="inspect-apph"></a>Inspecciona **App.h**.

Echemos un vistazo rápidamente a los métodos de 5 **App.h** &mdash; [ **inicializar**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [ **SetWindow** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [ **Carga**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load), [ **ejecutar**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run), y [ **desinicializar** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) al implementar el [ **IFrameworkView** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.run) interfaz que define un proveedor de vistas. Estos métodos los ejecuta el singleton de la aplicación que se crea al lanzar el juego, y carga todos los recursos de la aplicación, además de conectar los controladores de eventos apropiados.

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

Este método crea una instancia del proveedor de la vista de Direct3D desde la fábrica de proveedor de vistas (**Direct3DApplicationSource**, definido en `App.h`) y lo pasa a singleton de la aplicación mediante una llamada a [  **CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run). Los métodos de la vista del marco de trabajo (que es el **aplicación** clase en este ejemplo) se llaman en el orden **inicializar**-**SetWindow** - **Carga**-**OnActivated**-**ejecutar**-**desinicializar**. Una llamada a **CoreApplication::Run** inicia ese lifycycle. El bucle principal de su juego reside en el cuerpo de la implementación de la [ **IFrameworkView::Run** método](/uwp/api/windows.applicationmodel.core.iframeworkview.run), y en este caso, tiene la **App::Run**.

Desplázate hasta encontrar el método **App:: Run** en **App.cpp**. Este es el código.

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

¿Qué hace este método: Si no se cierra la ventana para su juego, envía todos los eventos, actualiza el temporizador, a continuación, procesa y presenta los resultados de la canalización de gráficos. Hablaremos sobre esto más detalladamente en [definir el marco de aplicación para UWP](tutorial--building-the-games-uwp-app-framework.md), [marco de representación lo hago?: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md), y [II del marco de representación: Representación de juegos](tutorial-game-rendering.md). Llegados a este punto, deberías tener una idea de la estructura de código básica de un juego DirectX de UWP.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Revisa y actualiza el archivo package.appxmanifest


La plantilla no solo consiste en archivos de código. El archivo **Package.appxmanifest** contiene metadatos de tu proyecto que sirven para empaquetar y lanzar el juego, así como para enviarlo a Microsoft Store. También contiene información importante que el sistema del jugador usa para proporcionar acceso a los recursos del sistema que el programa necesita para ejecutarse.

Ejecuta el **Diseñador de manifiestos** haciendo doble clic en el archivo **Package.appxmanifest** en el **Explorador de soluciones**.

![captura de pantalla del editor del manifiesto package.appx.](images/simple-dx-game-setup-app-manifest.png)

Para más información sobre el archivo **package.appxmanifest** y el empaquetado, consulta el [Diseñador de manifiestos](https://docs.microsoft.com/previous-versions/br230259(v=vs.140)). Por ahora, echa un vistazo a la pestaña **Capacidades** y mira las opciones proporcionadas.

![captura de pantalla con las capacidades predeterminadas de una aplicación direct3d.](images/simple-dx-game-setup-capabilities.png)

Si no seleccionas las funcionalidades que usa tu juego, como el acceso a **Internet** para los juegos locales guardados, o Internet para ver las puntuaciones más altas globales, no podrás tener acceso a los recursos o características correspondientes. Cuando crees un nuevo juego, asegúrate de que seleccionas las funcionalidades que necesitará el juego para ejecutarse.

Ahora, veamos el resto de archivos que vienen con la plantilla **DirectX 11 App (Universal Windows)** .

## <a name="review-the-included-libraries-and-headers"></a>Revisa las bibliotecas y los encabezados incluidos

Hay varios archivos que aún no hemos visto. Estos archivos proporcionan herramientas y soporte adicionales comunes a escenarios de desarrollo de juegos Direct3D. Para la lista completa de archivos que se incluyen con el proyecto de juego básico de DirectX, consulta [Plantillas de proyectos de juegos DirectX](user-interface.md#template-structure).

| Archivo de código fuente de plantilla         | Carpeta de archivos            | Descripción |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | Común                 | Define un objeto de clase que controla todos los [recursos de dispositivo](tutorial--assembling-the-rendering-pipeline.md#resource) DirectX. También incluye una interfaz para la aplicación que posee DeviceResources para que se le notifique cuando se pierde o se crea el dispositivo.                                                |
| DirectXHelper.h              | Común                 | Implementa métodos, incluyendo **DX::ThrowIfFailed**, **ReadDataAsync** y **ConvertDipsToPixels. **DX::ThrowIfFailed** convierte los valores de errores HRESULT devueltos por las API de DirectX Win32 en excepciones de Windows Runtime. Usa este método para colocar un punto de interrupción para depurar errores de DirectX. Para obtener más información, consulta [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed). **ReadDataAsync** lee desde un archivo binario de forma asincrónica. **ConvertDipsToPixels** convierte una longitud en píxeles independientes del dispositivo (DIP) a una longitud en píxeles físicos. |
| StepTimer.h                  | Común                 | Define un temporizador de alta resolución útil para aplicaciones de representación de juego o interactivas.   |
| Sample3DSceneRenderer.h/.cpp | Contenido                | Define un objeto de clase para crear una instancia de una canalización de representación básica. Crea una implementación de representación básica que conecta una cadena de intercambio Direct3D y un adaptador gráfico a tu UWP con DirectX.   |
| SampleFPSTextRenderer.h/.cpp | Contenido                | Define un objeto de clase para representar el valor actual de fotogramas por segundo (FPS) en la parte inferior derecha de la pantalla con Direct2D y DirectWrite.  |
| SamplePixelShader.hlsl       | Contenido                | Contiene el código de lenguaje de sombreado de alto nivel (HLSL) para un sombreador de píxeles muy básico.                                            |
| SampleVertexShader.hlsl      | Contenido                | Contiene el código de lenguaje de sombreado de alto nivel (HLSL) para un sombreador de vértices muy básico.                                           |
| ShaderStructures.h           | Contenido                | Contiene las estructuras de sombreador que pueden usarse para enviar las matrices de MVP y los datos por vértice al sombreador de vértices.  |
| pch.h/.cpp                   | Principal                   | Contiene todos los archivos de inclusión del sistema Windows para las API usadas por una aplicación Direct3D, incluidas las API de DirectX 11.| 

### <a name="next-steps"></a>Pasos siguientes

En este punto, has aprendido a crear un proyecto de juego de DirectX de UWP mediante la plantilla **DirectX 11 App (Universal Windows)** y has recibido información sobre algunos componentes y archivos proporcionados por el proyecto.

La siguiente sección es [Definir el marco de UWP del juego](tutorial--building-the-games-uwp-app-framework.md). Examinaremos cómo usa y amplía este juego muchos de los conceptos y componentes proporcionados por la plantilla.

 

 




