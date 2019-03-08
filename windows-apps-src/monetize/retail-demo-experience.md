---
title: Agregar características de demostración (RDX) de venta directa a la aplicación
description: Preparación de la aplicación para el modo de demostración de minoristas, ayudando a exhibir su aplicación en el piso de ventas minoristas.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, aplicación de demostración comercial
ms.localizationpriority: medium
ms.openlocfilehash: b66435dd7c94762874461b48e19e9a60224f287b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596760"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Agregar características de demostración (RDX) de venta directa a la aplicación

Incluir un modo de demostración de venta directa en la aplicación de Windows para que los clientes que prueben equipos y dispositivos en el piso de ventas pueden comenzar de inmediato.

Cuando los clientes están en una tienda minorista, esperan poder probar las demostraciones de equipos y dispositivos. A menudo dedican un fragmento considerable de su tiempo a jugar con las aplicaciones a través de la [venta por menor experiencia de demostración (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Puede configurar la aplicación para proporcionar experiencias diferentes de _normal_ o _retail_ modos. Por ejemplo, si la aplicación se inicia con un proceso de instalación, puede omitir en el modo de venta directa y rellenar previamente la aplicación con la configuración predeterminada y los datos de ejemplo, por lo que puede comenzar de inmediato.

Desde la perspectiva de nuestros clientes, hay solo una aplicación. Para ayudar a los clientes distinguir entre los dos modos, se recomienda que mientras la aplicación está en modo de venta directa, muestra la palabra "Comercial" forma destacada en la barra de título o en una ubicación adecuada.

Además de los requisitos de Microsoft Store para aplicaciones, aplicaciones basadas en RDX también deben ser compatibles con el programa de instalación RDX, limpieza y procesos de actualización para asegurarse de que los clientes tengan una experiencia positiva en la tienda.

## <a name="design-principles"></a>Principios de diseño

* **Mostrar la mejor**. Usar la experiencia de demostración de venta directa a showcase por qué la aplicación es impresionante. Esto es probable que la primera vez que sus clientes podrán ver su aplicación, por lo que muestran lo mejor!

* **Mostrar rápido**. Los clientes pueden impacientarse. Por eso, cuanto más rápido puedan experimentar el valor real de la aplicación, mejor.

* **Simplificar la historia**. La experiencia de demostración de venta directa es una propuesta de elevador para el valor de la aplicación.

* **Centrarse en la experiencia**. Permite que el usuario tenga tiempo para asimilar el contenido. Si bien es importante llevarlo a la mejor parte rápido, diseñar pausas adecuadas puede ayudarlo a disfrutar completamente de la experiencia.

## <a name="technical-requirements"></a>Requisitos técnicos

Como aplicaciones dependientes del RDX están diseñadas para presentar mejor de su aplicación a los clientes por menor, deben cumplir los requisitos técnicos y cumplir las normas de privacidad que tiene la Microsoft Store para todas las aplicaciones de experiencia de demostración de venta directa.

Esto puede usarse como una lista de comprobación para ayudarle a prepararse para el proceso de validación y para proporcionar mayor claridad en el proceso de pruebas. Ten en cuenta que estos requisitos deben mantenerse no solo para el proceso de validación, sino para toda la duración de la aplicación de experiencia de demostración comercial, siempre que la aplicación se siga ejecutando en los dispositivos de demostración comercial.

### <a name="critical-requirements"></a>Requisitos críticos

Tan pronto como sea posible RDX compatible con aplicaciones que no cumplen estos requisitos críticos se quitará de todos los dispositivos de demostración de venta directa.

* **No pedir información personal identificable (PII)**. Esto incluye información de inicio de sesión, información de cuenta Microsoft o póngase en contacto con los detalles.

* **Experiencia sin errores**. La aplicación debe ejecutarse sin errores. Además, no deben mostrarse mensajes emergentes o notificaciones de error a los clientes que usan los dispositivos de demostración comercial. Errores reflejan negativamente en la aplicación propia, su marca, marca del dispositivo, marca del fabricante del dispositivo y marca de Microsoft.

* **Aplicaciones de pago deben tener un modo de prueba**. La aplicación o bien debe una segunda oportunidad o incluir un [modo de prueba](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Los clientes no esperan tener que pagar por una experiencia en una tienda comercial.

### <a name="high-priority-requirements"></a>Requisitos de alta prioridad

Deben investigarse inmediatamente por una corrección de aplicaciones RDX dependientes que no cumplen estos requisitos de alta prioridad. Si no se encuentra ninguna corrección inmediata, es posible que la aplicación se quite de todos los dispositivos de demostración comercial.

* **Experiencia sin conexión fácil de recordar**. La aplicación necesita para demostrar una gran experiencia sin conexión como aproximadamente el 50% de los dispositivos están sin conexión en establecimientos comerciales. Esto sirve para garantizar que los clientes que interactúen con la aplicación sin conexión también puedan tener una experiencia positiva y significativa.

* **Actualiza la experiencia de contenido**. La aplicación nunca debe ser prompt actualizaciones al estar en línea. Si las actualizaciones son necesarias, debe realizarse en modo silencioso.

* **No hay comunicación anónima**. Dado que un cliente que usa un dispositivo comercial de demostración es un usuario anónimo, no debe ser capaz de contenido de mensaje o un recurso compartido desde el dispositivo.

* **Entregue experiencias coherentes mediante el proceso de limpieza**. Todos los clientes deben tener la misma experiencia cuando usan un dispositivo de demostración comercial. La aplicación debe usar [proceso de limpieza](#cleanup-process) para volver al estado predeterminado después de cada uso. No queremos que el cliente siguiente para ver lo que el cliente última quedan atrás. Esto incluye marcadores, logros y desbloqueos.

* **Antigüedad del contenido correspondiente**. Todo el contenido de aplicación debe ser asignada una adolescente o categoría de clasificación inferior. Para obtener más información, consulte [publique su aplicación clasificados por IARC](https://www.globalratings.com/for-developers.aspx) y [clasificaciones ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Requisitos de prioridad Media

Es posible que el equipo de la tienda comercial de Windows se ponga en contacto directamente con los desarrolladores para analizar cómo solucionar estos problemas.

* **Capacidad de ejecutar correctamente a través de una variedad de dispositivos**. Las aplicaciones deben ejecutar también en todos los dispositivos, incluidos los dispositivos con las especificaciones de bajo perfil. Si la aplicación se instala en dispositivos que no cumple las especificaciones mínimas, la aplicación necesita claramente informar al usuario sobre esto. Los requisitos mínimos del dispositivo deben notificarse para que la aplicación siempre pueda ejecutarse con un alto rendimiento.

* **Cumplir requisitos de tamaño de aplicación de tienda minorista**. El tamaño de la aplicación debe ser inferior a 800 MB. Póngase en contacto con el equipo de Windows Retail Store directamente para obtener más información sobre si la aplicación compatible con RDX no cumple los requisitos de tamaño.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: Preparar el código para el modo de demostración

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
El [ **IsDemoModeEnabled** ](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) propiedad en el [ **RetailInfo** ](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) clase de utilidad, que forma parte de la [ Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) espacio de nombres en el SDK de Windows 10, se usa como un valor booleano que indica para especificar qué ruta de acceso de código que se ejecuta la aplicación - la _normal_ modo o la _retail_ modo.

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

Limpieza comienza a dos minutos después de un comprador deja de interactuar con el dispositivo. Reproduce la demostración de venta directa, y Windows iniciará al restablecer los datos de ejemplo en los contactos, fotografías y otras aplicaciones. En función del dispositivo, esta operación puede tardar entre 1 y 5 minutos totalmente Restablecer todo a la normalidad. Esto garantiza que todos los clientes en la tienda pueden acercarse a un dispositivo y tener la misma experiencia al interactuar con el dispositivo.

Paso 1: Limpieza
* Se cierran todas las aplicaciones de Win32 y de la Tienda.
* Se eliminan todos los archivos que se encuentran en carpetas conocidas como __Imágenes__, __Vídeos__, __Música__, __Documentos__, __Imágenes guardadas__, __Álbum de cámara__, __Escritorio__ y __Descargas__.
* Se eliminan los estados de itinerancia estructurados y no estructurados.
* Se eliminan los estados locales estructurados.

Paso 2:  Instalación
* Para dispositivos sin conexión: Carpetas permanecen vacías
* Para dispositivos en línea: Activos de demostración de venta directa se pueden insertar en el dispositivo desde la Microsoft Store

### <a name="store-data-across-user-sessions"></a>Store datos entre sesiones de usuario

Para almacenar datos en las sesiones de usuario, puede almacenar información en __ApplicationData.Current.TemporaryFolder__ como valor predeterminado el proceso de limpieza no eliminar automáticamente los datos en esta carpeta. Tenga en cuenta esa información almacenada mediante *LocalState* se elimina durante el proceso de limpieza.

### <a name="customize-the-cleanup-process"></a>Personalizar el proceso de limpieza

Para personalizar el proceso de limpieza, implemente el `Microsoft-RetailDemo-Cleanup` servicio de aplicación en la aplicación.

Escenarios donde es necesaria una lógica de limpieza personalizada incluye la ejecución de una configuración amplia, descargar y almacenar datos en caché o no que deseen *LocalState* datos va a eliminar.

Paso 1: Declare el _RetailDemo-Microsoft-limpieza_ servicio en el manifiesto de aplicación.
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

Paso 2: Implementar la lógica de limpieza personalizado en el _AppdataCleanup_ función case mediante la plantilla de ejemplo siguiente.
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

* [Store y recuperar datos de la aplicación](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Cómo crear y consumir un servicio de aplicación](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localizar el contenido de la aplicación](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Experiencia de demostración de venta directa (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
