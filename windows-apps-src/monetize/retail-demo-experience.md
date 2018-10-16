---
author: joannaleecy
title: Agregar características de demostración (RDX) comercial a la aplicación
description: Preparar la aplicación para el modo de demostración comercial, lo que ayuda a presentar la aplicación en la superficie de ventas comercial.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, aplicación de demostración comercial
ms.localizationpriority: medium
ms.openlocfilehash: 152c775c1b69bfd82d8969aed7e638f98646bdd7
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4684540"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Agregar características de demostración (RDX) comercial a la aplicación

Incluir un modo de demostración comercial en la aplicación de Windows, para que los clientes que probar equipos y dispositivos en la superficie de ventas pueden ir directamente.

Cuando los clientes están en un establecimiento comercial, esperan poder probar demostraciones de equipos y dispositivos. A menudo, al gastar una divertirse de su tiempo con aplicaciones a través de la [experiencia de demostración comercial (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Puedes configurar la aplicación para proporcionar experiencias diferentes mientras que en los modos _normal_ o _comercial_ . Por ejemplo, si la aplicación se inicia un proceso de instalación, es posible que omita en modo comercial y rellenar previamente la aplicación con la configuración predeterminada y los datos de muestra, por lo que pueden ir directamente.

Desde la perspectiva de nuestros clientes, hay solo una aplicación. Para ayudar a los clientes a distinguir entre los dos modos, te recomendamos que mientras la aplicación está en modo comercial, muestra la palabra "Comercial" un lugar destacado en la barra de título o en una ubicación adecuada.

Además de los requisitos de Microsoft Store para las aplicaciones, las aplicaciones con reconocimiento RDX también deben ser compatibles con el programa de instalación RDX, limpieza y los procesos de actualización para garantizar que los clientes tengan una experiencia positiva coherente en la tienda minorista.

## <a name="design-principles"></a>Principios de diseño

* **Mostrar el mejor**. Usa la experiencia de demostración comercial para exhibir por qué es genial la aplicación. Esto es probable que la primera vez que el cliente vea la aplicación, por lo tanto, mostrar la mejor parte!

* **Mostrarlo rápido**. Los clientes pueden impacientarse. Por eso, cuanto más rápido puedan experimentar el valor real de la aplicación, mejor.

* **Simplificar**. La experiencia de demostración comercial es un argumento de venta para el valor de la aplicación.

* **Centrarse en la experiencia**. Permite que el usuario tenga tiempo para asimilar el contenido. Si bien es importante llevarlo a la mejor parte rápido, diseñar pausas adecuadas puede ayudarlo a disfrutar completamente de la experiencia.

## <a name="technical-requirements"></a>Requisitos técnicos

Dado que las aplicaciones con reconocimiento RDX están diseñadas para mostrar lo mejor de la aplicación a los clientes minoristas, debe cumplir los requisitos técnicos y ajustarse a las normativas de privacidad de la Microsoft Store tiene para todas las aplicaciones de experiencia de demostración comercial.

Esto puede usarse como una lista de comprobación que te ayudarán a prepararse para el proceso de validación y para proporcionar mayor claridad en el proceso de pruebas. Ten en cuenta que estos requisitos deben mantenerse no solo para el proceso de validación, sino para toda la duración de la aplicación de experiencia de demostración comercial, siempre que la aplicación se siga ejecutando en los dispositivos de demostración comercial.

### <a name="critical-requirements"></a>Requisitos de críticos

Las aplicaciones con reconocimiento de RDX que no cumplan con estos requisitos críticos se quitará todos los dispositivos de demostración comercial tan pronto como sea posible.

* **No solicites la información de identificación personal (PII)**. Esto incluye información de inicio de sesión, la información de cuenta de Microsoft o contacto detalles.

* **Experiencia sin errores**. La aplicación debe ejecutarse sin errores. Además, no deben mostrarse mensajes emergentes o notificaciones de error a los clientes que usan los dispositivos de demostración comercial. Errores negativo en la aplicación propia, tu marca, marca del dispositivo, del fabricante del dispositivo y marca de Microsoft.

* **Las aplicaciones de pago deben tener un modo de prueba**. La aplicación debe ser gratuita o incluir un [modo de prueba](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Los clientes no esperan tener que pagar por una experiencia en una tienda comercial.

### <a name="high-priority-requirements"></a>Requisitos de prioridad alta

Las aplicaciones con reconocimiento de RDX que no cumplan con estos requisitos de prioridad alta que se investigar para encontrar una corrección inmediata. Si no se encuentra ninguna corrección inmediata, es posible que la aplicación se quite de todos los dispositivos de demostración comercial.

* **Memorable experiencia sin conexión**. La aplicación debe mostrar una estupenda experiencia sin conexión ya aproximadamente el 50% de los dispositivos están sin conexión en los establecimientos minoristas. Esto sirve para garantizar que los clientes que interactúen con la aplicación sin conexión también puedan tener una experiencia positiva y significativa.

* **Experiencia de contenido actualizado**. La aplicación nunca se puede solicitar actualizaciones cuando en línea. Si se necesitan actualizaciones, se deben realizar de forma silenciosa.

* **Sin comunicaciones anónimas**. Dado que un cliente que usa un dispositivo de demostración comercial es un usuario anónimo, no debe ser capaz de mensaje o el recurso compartido de contenido desde el dispositivo.

* **Proporcionar experiencias coherentes con el proceso de limpieza**. Todos los clientes deben tener la misma experiencia cuando usan un dispositivo de demostración comercial. La aplicación debe usar el [proceso de limpieza](#clean-up-process) para regresar al mismo estado predeterminado después de cada uso. No queremos que el cliente siguiente vea la actividad del último cliente. Esto incluye marcadores, logros y desbloqueos.

* **Contenido adecuado según la edad**. Todo el contenido de aplicación debe ser un apto para adolescentes o clasificación inferior. Para obtener más información, consulte [obtener la aplicación clasificación de IARC](https://www.globalratings.com/for-developers.aspx) y [clasificaciones de ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Requisitos de prioridad Media

Es posible que el equipo de la tienda comercial de Windows se ponga en contacto directamente con los desarrolladores para analizar cómo solucionar estos problemas.

* **Capacidad para ejecutarse correctamente en una gama de dispositivos**. Las aplicaciones deben ejecutarse correctamente en todos los dispositivos, incluidos los dispositivos con especificaciones de gama baja. Si la aplicación está instalada en los dispositivos que no cumplen con las especificaciones mínimas, la aplicación debe notificar claramente al usuario acerca de este. Los requisitos mínimos del dispositivo deben notificarse para que la aplicación siempre pueda ejecutarse con un alto rendimiento.

* **Cumplir los requisitos de tamaño de aplicación de la tienda comercial**. El tamaño de la aplicación debe ser inferior a 800MB. Ponte en contacto con el equipo de la tienda comercial de Windows directamente para comentarlo con ellos si tu aplicación compatible con RDX no cumple los requisitos de tamaño.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: Preparar el código para el modo de demostración

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
La propiedad [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) en la clase de utilidad [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) , que forma parte del espacio de nombres [Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) en el SDK de Windows 10, se usa como un indicador booleano para especificar qué ruta de acceso de código que la aplicación se ejecuta en - lo normal _ _modo o el modo _comercial_ .

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>RetailInfo.Properties

Cuando [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) devuelve "true", puedes consultar un conjunto de propiedades sobre el dispositivo mediante el uso de [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) para crear una experiencia de demostración comercial más personalizada. Estas propiedades incluyen [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory), etcétera.

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>Proceso de limpieza

Limpieza comienza dos minutos después de que un comprador deja de interactuar con el dispositivo. Se reproduce la demostración comercial, y Windows comienza el restablecimiento de los datos de ejemplo en los contactos, fotos y otras aplicaciones. Según el dispositivo, esto puede tardar entre 1-5 minutos totalmente Restablecer todo lo normal. Esto garantiza que todos los clientes de la tienda minorista pueden acercarse a un dispositivo y tener la misma experiencia cuando se interactúa con el dispositivo.

Paso 1: limpieza
* Se cierran todas las aplicaciones de Win32 y de la Store.
* Se eliminan todos los archivos que se encuentran en carpetas conocidas como __Imágenes__, __Vídeos__, __Música__, __Documentos__, __Imágenes guardadas__, __Álbum de cámara__, __Escritorio__ y __Descargas__.
* Se eliminan los estados de itinerancia estructurados y no estructurados.
* Se eliminan los estados locales estructurados.

Paso 2: instalación
* Para dispositivos sin conexión: las carpetas permanecen vacías
* Para dispositivos en línea: los activos de demostración comercial pueden insertarse en el dispositivo de Microsoft Store

### <a name="store-data-across-user-sessions"></a>Almacenar datos de sesiones de usuario

Para almacenar datos de sesiones de usuario, puedes almacenar información en __ApplicationData.Current.TemporaryFolder__ como el proceso de limpieza predeterminado no elimina automáticamente los datos de esta carpeta. Ten en cuenta que la información almacenada mediante *LocalState* se elimina durante el proceso de limpieza.

### <a name="customize-the-cleanup-process"></a>Personalizar el proceso de limpieza

Para personalizar el proceso de limpieza, implementar la `Microsoft-RetailDemo-Cleanup` servicio de aplicaciones en la aplicación.

Los escenarios donde se necesita una lógica de limpieza personalizada incluyen la ejecución de una configuración amplia, descarga y almacenamiento en caché de datos o que no se *LocalState* los datos se eliminarán.

Paso 1: Declarar el servicio de _Microsoft-RetailDemo-Cleanup_ en el manifiesto de la aplicación.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

Paso 2: Implementar la lógica de limpieza personalizada en la función de mayúsculas _AppdataCleanup_ utilizando la plantilla de ejemplo siguiente.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>Vínculos relacionados

* [Almacenar y recuperar datos de la aplicación](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Cómo crear y consumir un servicio de aplicaciones](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localización de contenido de la aplicación](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Experiencia de demostración comercial (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
