---
author: TylerMSFT
title: Administrar la reanudación de la aplicación
description: Aprende a actualizar el contenido mostrado cuando el sistema reanuda la aplicación.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 717c819aaa732cf8d29e0a701a1fec81485f48ac
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "6051466"
---
# <a name="handle-app-resume"></a>Controlar la reanudación de aplicaciones

**API importantes**

- [**Reanudar**](https://msdn.microsoft.com/library/windows/apps/br242339)

Aprende dónde actualizar la interfaz de usuario cuando el sistema reanuda la aplicación. El ejemplo de este tema registra un controlador de eventos para el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

## <a name="register-the-resuming-event-handler"></a>Registrar el controlador de eventos de reanudación

Haz el registro para controlar el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), que indica que el usuario abandonó la aplicación y después volvió.

```csharp
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```

```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Resuming({ this, &MainPage::App_Resuming });
}
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming +=
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>Actualizar el contenido mostrado y volver adquirir recursos

El sistema suspende la aplicación unos segundos después de que el usuario cambie a otra aplicación o al escritorio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto en el que estaba. Para el usuario, parece como si la aplicación se haya estado ejecutando en segundo plano.

Cuando la aplicación controla el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) la aplicación se puede suspender durante horas o días. Se debe actualizar todo el contenido que pueda haber quedado obsoleto mientras la aplicación estaba suspendida, por ejemplo, fuentes de noticias o la ubicación del usuario.

También es un buen momento para restaurar todos los recursos exclusivos que liberaste cuando se suspendió la aplicación, como los identificadores de archivos, cámaras, dispositivos de E/S, dispositivos externos y recursos de red.

```csharp
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.

    End Sub
>
End Class
```

```cppwinrt
void MainPage::App_Resuming(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::Foundation::IInspectable const& /* e */)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

> [!NOTE]
> Dado que no se genera el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) desde el subproceso de interfaz de usuario, debe usar un distribuidor en el controlador para distribuir todas las llamadas a la interfaz de usuario.

## <a name="remarks"></a>Observaciones

Cuando la aplicación está conectada al depurador de Visual Studio, no se suspenderá. Sin embargo, se puede suspender desde el depurador y luego volver a enviarle un evento **Resume**, para que puedas depurar el código. Asegúrate de que la **barra de herramientas Ubicación de depuración** es visible y haz clic en el menú desplegable junto al icono **Suspender**. A continuación, elige **Reanudar**.

En las aplicaciones de la Tienda de WindowsPhone, el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) siempre va seguido del evento [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), aunque la aplicación esté suspendida en el momento y el usuario la reinicie desde un icono principal o una lista de aplicaciones. Las aplicaciones pueden omitir la inicialización si ya hay contenido establecido en la ventana actual. Puedes comprobar la propiedad [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar si la aplicación se ha iniciado desde un icono principal o secundario y, según esta información, decidir si quieres presentar una experiencia de aplicación nueva o reanudar la existente.

## <a name="related-topics"></a>Temas relacionados

* [Ciclo de vida de la aplicación](app-lifecycle.md)
* [Controlar la activación de aplicaciones](activate-an-app.md)
* [Administrar la suspensión de aplicaciones](suspend-an-app.md)
