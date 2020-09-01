---
title: Creación de una aplicación universal de Windows de varias instancias
description: En este tema se describe cómo escribir aplicaciones para UWP que admitan la creación de instancias múltiples.
keywords: UWP de varias instancias
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e19425222459bb3bd56a5fe406ec76c0b7fb6b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164779"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Creación de una aplicación universal de Windows de varias instancias

En este tema se describe cómo crear aplicaciones de Plataforma universal de Windows de instancias múltiples (UWP).

Desde Windows 10, versión 1803 (10,0; Compilación 17134) en adelante, la aplicación UWP puede participar para admitir varias instancias. Si se está ejecutando una instancia de una aplicación para UWP de varias instancias y llega una solicitud de activación posterior, la plataforma no activará la instancia existente. En su lugar, creará una instancia nueva, que se ejecutará en un proceso independiente.

> [!IMPORTANT]
> La creación de instancias múltiples se admite para las aplicaciones de JavaScript, pero no para la redirección de varias instancias. Dado que la redirección de varias instancias no se admite para las aplicaciones de JavaScript, la clase [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) no es útil para estas aplicaciones.

## <a name="opt-in-to-multi-instance-behavior"></a>Participación en el comportamiento de varias instancias

Si va a crear una nueva aplicación de varias instancias, puede instalar las [plantillas de proyecto de aplicación de varias instancias. vsix](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.MultiInstanceApps), disponibles en el [Visual Studio Marketplace](https://marketplace.visualstudio.com/). Una vez instaladas las plantillas, estarán disponibles en el cuadro de diálogo **nuevo proyecto** en **Visual C# > Windows universal** (u **otros lenguajes > Visual C++ > Windows universal**).

Se instalan dos plantillas: **aplicación UWP de varias instancias**, que proporciona la plantilla para crear una aplicación de varias instancias y una **aplicación UWP de redirección de varias instancias**, que proporciona lógica adicional en la que puede crear una nueva instancia o activar de forma selectiva una instancia que ya se ha iniciado. Por ejemplo, quizás solo desea una instancia de cada vez editando el mismo documento, de modo que la instancia que tiene ese archivo se abre en primer plano en lugar de iniciar una nueva instancia.

Ambas plantillas se agregan `SupportsMultipleInstances` al `package.appxmanifest` archivo. Tenga en cuenta el prefijo de espacio `desktop4` de nombres y `iot2` : solo los proyectos que tienen como destino los proyectos de escritorio o Internet de las cosas (IOT) admiten la creación de instancias múltiples.

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

## <a name="multi-instance-activation-redirection"></a>Redirección de activación de varias instancias

 La compatibilidad con varias instancias para aplicaciones UWP va más allá, simplemente haciendo posible iniciar varias instancias de la aplicación. Permite la personalización en los casos en los que desea seleccionar si se inicia una nueva instancia de la aplicación o si se activa una instancia que ya se está ejecutando. Por ejemplo, si se inicia la aplicación para editar un archivo que ya se está editando en otra instancia, es posible que desee redirigir la activación a esa instancia en lugar de abrir otra instancia que ya está editando el archivo.

Para verlo en acción, vea este vídeo sobre cómo crear aplicaciones para UWP de varias instancias.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

La plantilla de **aplicación para UWP de redirección de varias instancias** agrega `SupportsMultipleInstances` al archivo package. appxmanifest tal y como se mostró anteriormente, y también agrega un **Program.CS** (o **Program. cpp**, si usa la versión de C++ de la plantilla) al proyecto que contiene una `Main()` función. La lógica para redirigir la activación se lleva a cabo en la `Main` función. La plantilla para **Program.CS** se muestra a continuación.

La propiedad [**AppInstance. RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) representa la instancia preferida proporcionada por el shell para esta solicitud de activación, si hay alguna (o `null` si no hay ninguna). Si el Shell proporciona una preferencia, puede redirigir la activación a esa instancia o puede omitirla si lo desea.

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

`Main()` es lo primero que se ejecuta. Se ejecuta antes de [**iniciarse**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) y de [**activarse**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Esto le permite determinar si se debe activar o no otra instancia antes de que se ejecute cualquier otro código de inicialización de la aplicación.

El código anterior determina si se activa una instancia existente o nueva de la aplicación. Una clave se usa para determinar si hay una instancia de existente que desee activar. Por ejemplo, si se puede iniciar la aplicación para [controlar la activación de archivos](./handle-file-activation.md), puede usar el nombre de archivo como una clave. A continuación, puede comprobar si una instancia de la aplicación ya está registrada con esa clave y activarla en lugar de abrir una nueva instancia. Esta es la idea detrás del código: `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Si se encuentra una instancia registrada con la clave, esa instancia se activa. Si no se encuentra la clave, la instancia actual (la instancia que se está ejecutando actualmente `Main` ) crea su objeto de aplicación y empieza a ejecutarse.

## <a name="background-tasks-and-multi-instancing"></a>Tareas en segundo plano y múltiples instancias

- Las tareas en segundo plano fuera de proceso admiten la creación de instancias múltiples. Normalmente, cada nuevo desencadenador genera una nueva instancia de la tarea en segundo plano (aunque técnicamente se pueden ejecutar varias tareas en segundo plano en el mismo proceso de host). No obstante, se crea una instancia diferente de la tarea en segundo plano.
- Las tareas en segundo plano en proceso no admiten la creación de instancias múltiples.
- Las tareas de audio de fondo no admiten la creación de instancias múltiples.
- Cuando una aplicación registra una tarea en segundo plano, normalmente primero comprueba si la tarea ya está registrada y, a continuación, la elimina y vuelve a registrarla, o no hace nada para mantener el registro existente. Todavía es el comportamiento típico de las aplicaciones de varias instancias. Sin embargo, una aplicación de varias instancias puede optar por registrar un nombre de tarea en segundo plano diferente en cada instancia. Esto producirá varios registros para el mismo desencadenador y se activarán varias instancias de la tarea en segundo plano cuando se active el desencadenador.
- App-Services inicia una instancia independiente de la tarea en segundo plano del servicio de aplicación para cada conexión. Esto permanece inalterado en las aplicaciones de varias instancias, es decir, cada instancia de una aplicación de varias instancias obtendrá su propia instancia de la tarea en segundo plano de App-Service. 

## <a name="additional-considerations"></a>Consideraciones adicionales

- La creación de instancias múltiples es compatible con las aplicaciones UWP destinadas a proyectos de escritorio y Internet de las cosas (IoT).
- Para evitar las condiciones de carrera y los problemas de contención, las aplicaciones de varias instancias deben tomar medidas para particionar o sincronizar el acceso a la configuración, el almacenamiento local de la aplicación y cualquier otro recurso (por ejemplo, archivos de usuario, un almacén de datos, etc.) que se pueda compartir entre varias instancias. Están disponibles mecanismos de sincronización estándar como exclusiones mutuas, semáforos, eventos, etc.
- Si la aplicación tiene `SupportsMultipleInstances` en su archivo package. appxmanifest, no es necesario declarar sus extensiones `SupportsMultipleInstances` . 
- Si agrega `SupportsMultipleInstances` a cualquier otra extensión, aparte de las tareas en segundo plano o de App-Services, y la aplicación que hospeda la extensión no también `SupportsMultipleInstances` se declara en el archivo package. appxmanifest, se genera un error de esquema.
- Las aplicaciones pueden usar la declaración de [**ResourceGroup**](./declare-background-tasks-in-the-application-manifest.md) en su manifiesto para agrupar varias tareas en segundo plano en el mismo host. Esto entra en conflicto con la creación de instancias múltiples, donde cada activación entra en un host independiente. Por lo tanto, una aplicación no puede declarar `SupportsMultipleInstances` y `ResourceGroup` en su manifiesto.

## <a name="sample"></a>Ejemplo

Vea [el ejemplo de varias instancias](https://github.com/Microsoft/AppModelSamples/tree/master/Samples/BananaEdit) para obtener un ejemplo de redirección de activación de varias instancias.

## <a name="see-also"></a>Vea también

[AppInstance. FindOrRegisterInstanceForKey](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_) 
 [AppInstance. GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs) 
 [AppInstance. RedirectActivationTo](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo) 
 [Controlar la activación](./activate-an-app.md) de la aplicación