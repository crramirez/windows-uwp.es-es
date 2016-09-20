---
author: TylerMSFT
title: "Administrar la reanudación de la aplicación"
description: "Aprende a actualizar el contenido mostrado cuando el sistema reanuda la aplicación."
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.sourcegitcommit: e6957dd44cdf6d474ae247ee0e9ba62bf17251da
ms.openlocfilehash: dd3d75c7f3dfe325324e1fe31c039cd207b68d0b

---

# Administrar la reanudación de la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Reanudar**](https://msdn.microsoft.com/library/windows/apps/br242339)

Aprende a actualizar el contenido mostrado cuando el sistema reanuda la aplicación. El ejemplo de este tema registra un controlador de eventos para el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).


            **Guía básica:** Relación de este tema con los demás. Consulta:

-   [Guía básica para crear aplicaciones de Windows Runtime con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [Guía básica para crear aplicaciones de Windows en tiempo de ejecución con C++](https://msdn.microsoft.com/library/windows/apps/hh700360)

## Registrar el controlador de eventos de reanudación

Haz el registro para controlar el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), que indica que el usuario abandonó la aplicación y después volvió.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
>    }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Resuming, AddressOf App_Resuming
>    End Sub
>
> End Class
> ```
> ```cpp
> MainPage::MainPage()
> {
>     InitializeComponent();
>     Application::Current->Resuming +=
>         ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
> }
> ```

## [!div class="tabbedCodeSnippets"]

Actualizar el contenido mostrado tras la suspensión

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data
> }
> ```

> Cuando la aplicación controla el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), tiene la oportunidad de actualizar el contenido que muestra.

## [!div class="tabbedCodeSnippets"]



            **Nota** Dado que el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) no se genera desde el subproceso de la interfaz de usuario, deberá usarse un distribuidor para acceder al subproceso de la interfaz de usuario e inyectar una actualización en ella, si eso es lo que se quiere hacer en el controlador. Comentarios El sistema suspende la aplicación cuando el usuario cambia a otra aplicación o al escritorio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera.

El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano

> No obstante, es posible que la aplicación haya estado suspendida durante un período de tiempo largo. Por ello, debe actualizar el contenido mostrado que puede haber cambiado mientras la aplicación estaba suspendida, como fuentes de noticias o la ubicación del usuario. Si la aplicación no tiene contenido mostrado que se deba actualizar, no es necesario que controle el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339). 
            **Nota** Si la aplicación está conectada al depurador de Visual Studio, puedes enviarle un evento **Resume**.

> Asegúrate de que la **barra de herramientas Ubicación de depuración** es visible y haz clic en el menú desplegable junto al icono **Suspender**. A continuación, elige **Reanudar**. 
            **Nota** En las aplicaciones de la Tienda de WindowsPhone, el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) siempre va seguido del método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), aunque la aplicación esté suspendida por el momento y el usuario la reinicie desde un icono principal o una lista de aplicaciones.

## Las aplicaciones pueden omitir la inicialización si ya hay contenido establecido en la ventana actual.

* [Puedes comprobar la propiedad [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar si la aplicación se ha iniciado desde un icono principal o secundario y, según esta información, decidir si quieres presentar una experiencia de aplicación nueva o reanudar la existente.](activate-an-app.md)
* [Temas relacionados](suspend-an-app.md)
* [Controlar la activación de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Controlar la suspensión de la aplicación](app-lifecycle.md)



<!--HONumber=Jun16_HO5-->


