---
author: TylerMSFT
title: Crear una aplicación universal de Windows de instancias múltiples
description: En este tema, se describe cómo escribir aplicaciones para UWP que admiten instancias múltiples.
keywords: uwp de instancias múltiples
ms.author: twhitney
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd4e0ced4de2419858424a88f5fa5ce66f5b4286
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4620611"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Crear una aplicación universal de Windows de instancias múltiples

En este tema, se describe cómo crear aplicaciones para la Plataforma universal de Windows (UWP) de instancias múltiples.

Desde Windows 10, versión 1803 (10.0; Compilación 17134) en adelante, tu aplicación para UWP puede optar por admitir varias instancias. Si se está ejecutando una instancia de una aplicación para UWP de varias instancias y llega una solicitud de activación posterior, la plataforma no activará la instancia existente. En su lugar, creará una instancia nueva, que se ejecuta en un proceso independiente.

> [!IMPORTANT]
> Instancias múltiples se admiten para aplicaciones de JavaScript, pero no es el redireccionamiento de instancias múltiples. Dado que no se admite el redireccionamiento de instancias múltiples para aplicaciones de JavaScript, la clase [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) no es útil para dichas aplicaciones.

## <a name="opt-in-to-multi-instance-behavior"></a>Participar en el comportamiento de instancias múltiples

Si vas a crear una nueva aplicación de instancias múltiples, puedes instalar el **proyecto de aplicación de instancias múltiples Templates.VSIX**, disponible en [Visual Studio Marketplace ](https://aka.ms/E2nzbv). Después de instalar las plantillas, estarán disponibles en el cuadro de diálogo **Nuevo proyecto** en **Visual C# > Windows Universal** (o **Otros lenguajes > Visual C++ > Windows Universal**).

Se instalan dos plantillas: **Aplicaciones para UWP de varias instancias**, que proporciona la plantilla para crear una aplicación de instancias múltiples y **Aplicación para UWP de redireccionamiento de instancias múltiples**, que proporciona lógica adicional que se puede generar para iniciar una nueva instancia o para activar de forma selectiva una instancia que ya se ha iniciado. Por ejemplo, quizás solo deseas que un mismo documento solo pueda editarlo una instancia al mismo tiempo, por lo que llevas al primer plano la instancia que tiene abierto el archivo en lugar de iniciar una nueva instancia.

Ambas plantillas agregan `SupportsMultipleInstances` a la `package.appxmanifest` archivo. Ten en cuenta el prefijo de espacio de nombres `desktop4` y `iot2`: sólo los proyectos destinados al escritorio o los proyectos de Internet de las cosas (IoT) admiten instancias múltiples.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>Redireccionamiento de activación de instancias múltiples

 La compatibilidad de instancias múltiples para aplicaciones para UWP es mucho más que simplemente poder iniciar varias instancias de la aplicación. Permite la personalización en los casos que desees seleccionar si se inicia una nueva instancia de la aplicación o se activa una instancia que ya se está ejecutando. Por ejemplo, si la aplicación se inicia para editar un archivo que ya se está editando en otra instancia, puedes redirigir la activación a esa instancia en lugar de abrir otra instancia que ya está editando el archivo.

Para ver esto en acción, mira este vídeo acerca de cómo crear aplicaciones para UWP de instancias múltiples.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

La plantilla **Aplicación para UWP de varias instancias** agrega `SupportsMultipleInstances`al archivo package.appxmanifest, como se ha mostrado anteriormente, y también agrega un **Program.cs** (o **Program.cpp**, si estás usando la versión C++ de la plantilla) al proyecto que contiene una función `Main()`. La lógica para redirigir la activación se incluye en la función `Main`. A continuación se muestra la plantilla para **Program.cs** .

La propiedad [**AppInstance.RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) representa la instancia preferida siempre shell para esta solicitud de activación, si existe una (o `null` si hay uno). Si el shell proporciona una preferencia, a continuación, puede puede redirigir la activación a esa instancia, o puedes omitirla si eliges.

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` es lo primero que se ejecuta. Se ejecuta antes de [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) y [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Esto permite determinar si hay que activar esta u otra instancia antes de que se ejecute cualquier otro código de inicialización de la aplicación.

El código anterior determina si se activa una instancia existente o nueva de la aplicación. Se usa una clave se usa para determinar si hay una instancia que deseas activar. Por ejemplo, si la aplicación se puede iniciar con [Administrar la activación de archivos](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/handle-file-activation), podrías usar el nombre de archivo como clave. A continuación, puedes comprobar si una instancia de la aplicación ya está registrada con esa clave y activarla en lugar de abrir una nueva instancia. Esta es la idea que hay detrás del código: `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Si se encuentra una instancia registrada con la clave, esa instancia se activa. Si no se encuentra la clave, la instancia actual (la instancia que actualmente está ejecutando `Main`) crea su objeto de aplicación y comienza a ejecutarse.

## <a name="background-tasks-and-multi-instancing"></a>Tareas en segundo plano e instancias múltiples

- Las tareas en segundo plano fuera del proceso admiten instancias múltiples. Por lo general, cada desencadenador nuevo da como resultado una nueva instancia de la tarea en segundo plano (aunque técnicamente hablando, se pueden ejecutar varias tareas en segundo plano en el mismo proceso de host). No obstante, se crea una instancia diferente de la tarea en segundo plano.
- Las tareas en segundo plano en el proceso no admiten instancias múltiples.
- Las tareas de audio en segundo plano no admiten instancias múltiples.
- Cuando una aplicación registra una tarea en segundo plano, por lo general primero comprueba si la tarea ya está registrada y, a continuación, la elimina y vuelve a registrarla o o no hace nada para mantener el registro existente. Este sigue siendo el comportamiento típico en las aplicaciones de instancias múltiples. Sin embargo, una aplicación de instancias múltiples puede optar por registrar un nombre de tarea en segundo plano diferente por cada instancia. Esto da lugar a varios registros del mismo desencadenador, por lo que se activarán varias instancias de la tarea en segundo plano cuando se active el desencadenador.
- Los servicios de la aplicación inician una instancia diferente de la tarea en segundo plano del servicio de la aplicación para cada conexión. Esto también es igual en las aplicaciones de instancias múltiples, es decir, cada instancia de una aplicación de instancias múltiples obtendrá su propia instancia de la tarea en segundo plano del servicio de la aplicación. 

## <a name="additional-considerations"></a>Consideraciones adicionales

- Las instancias múltiples son compatibles con aplicaciones para UWP destinadas a proyectos de Internet de las cosas (IoT) y de escritorio.
- Para evitar problemas de condiciones de carrera y contención, la aplicación de instancias múltiples debe tomar medidas para sincronizar o particionar el acceso a la configuración, el almacenamiento local de la aplicación y cualquier otro recurso (por ejemplo, archivos de usuario, un almacén de datos etc.) que puede compartirse entre varias instancias. Hay disponibles mecanismos de sincronización estándar, como exclusiones mutuas, semáforos, eventos, etc.
- Si la aplicación tiene `SupportsMultipleInstances` en su archivo Package.appxmanifest, sus extensiones no tienen que declarar `SupportsMultipleInstances`. 
- Si agregas `SupportsMultipleInstances` a cualquier otra extensión, aparte de las tareas en segundo plano tareas o los servicios de la aplicación, y la aplicación que hospeda la extensión no declara también `SupportsMultipleInstances` en su archivo Package.appxmanifest, se genera un error del esquema.
- Las aplicaciones pueden usar la declaración de [**ResourceGroup**](https://docs.microsoft.com/windows/uwp/launch-resume/declare-background-tasks-in-the-application-manifest) en su manifiesto para agrupar varias tareas en segundo plano en el mismo host. Esto entra en conflicto con las instancias múltiples, dónde cada activación va a un host independiente. Por lo tanto, una aplicación no puede declarar `SupportsMultipleInstances` y `ResourceGroup` en su manifiesto.

## <a name="sample"></a>Muestra

Consulta la [muestra de varias instancias](https://aka.ms/Kcrqst) para obtener un ejemplo del redireccionamiento de activación de instancias múltiples.

## <a name="see-also"></a>Puedes ver también

[AppInstance.FindOrRegisterInstanceForKey](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_)
[AppInstance.GetActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs)
[AppInstance.RedirectActivationTo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo)
[Administrar la activación de aplicaciones](https://docs.microsoft.com/windows/uwp/launch-resume/activate-an-app)