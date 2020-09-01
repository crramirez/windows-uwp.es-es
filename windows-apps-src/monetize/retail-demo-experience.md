---
title: Agregar características de prueba comercial (RDX) a la aplicación
description: Prepare la aplicación para el modo de demostración comercial, ayudando a exhibir su aplicación en el piso de ventas minoristas.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: aplicación de demostración comercial de Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 39f1cb7439c02f215824c6c632fb2e2fc6afdb39
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164539"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Agregar características de prueba comercial (RDX) a la aplicación

Incluye un modo de prueba comercial en la aplicación de Windows para que los clientes que prueben equipos PC y dispositivos en la zona de ventas puedan comenzar de inmediato.

Cuando los clientes están en una tienda, esperan poder probar demostraciones de equipos y dispositivos. A menudo, emplean un fragmento considerable de su tiempo jugando con las aplicaciones a través de la [experiencia de demostración comercial (RDX)](/windows-hardware/customize/desktop/retail-demo-experience).

Puede configurar la aplicación para proporcionar diferentes experiencias en los modos _normal_ o _comercial_ . Por ejemplo, si la aplicación se inicia con un proceso de instalación, puede omitirla en modo comercial y rellenar previamente la aplicación con los datos de ejemplo y la configuración predeterminada para que puedan saltarse de inmediato.

Desde la perspectiva de nuestros clientes, solo hay una aplicación. Para ayudar a los clientes a distinguir entre los dos modos, se recomienda que, mientras la aplicación esté en modo comercial, se muestre la palabra "Retail" de forma destacada en la barra de título o en una ubicación adecuada.

Además de los requisitos de Microsoft Store para las aplicaciones, las aplicaciones compatibles con RDX también deben ser compatibles con los procesos de instalación, limpieza y actualización de RDX para asegurarse de que los clientes tienen una experiencia sistemáticamente positiva en la tienda.

## <a name="design-principles"></a>Principios de diseño

* **Mostrar el mejor**. Utilice la experiencia de demostración comercial para mostrar el motivo de la aplicación. Es probable que sea la primera vez que el cliente vea la aplicación, por lo que debe mostrarle la mejor opción.

* **Mostrarlo rápidamente**. Los clientes pueden impacientarse. Por eso, cuanto más rápido puedan experimentar el valor real de la aplicación, mejor.

* **Conserve la historia en simple**. La experiencia de demostración comercial es un paso de montacargas para el valor de la aplicación.

* **Céntrese en la experiencia**. Permite que el usuario tenga tiempo para asimilar el contenido. Si bien es importante llevarlo a la mejor parte rápido, diseñar pausas adecuadas puede ayudarlo a disfrutar completamente de la experiencia.

## <a name="technical-requirements"></a>Requisitos técnicos

Dado que las aplicaciones compatibles con RDX están pensadas para exhibir lo mejor de su aplicación a los clientes minoristas, deben cumplir los requisitos técnicos y cumplir los reglamentos de privacidad que el Microsoft Store tiene para todas las aplicaciones de la experiencia de demostración comercial.

Se puede usar como una lista de comprobación para ayudarle a preparar el proceso de validación y para proporcionar claridad en el proceso de prueba. Ten en cuenta que estos requisitos deben mantenerse no solo para el proceso de validación, sino para toda la duración de la aplicación de experiencia de demostración comercial, siempre que la aplicación se siga ejecutando en los dispositivos de demostración comercial.

### <a name="critical-requirements"></a>Requisitos críticos

Las aplicaciones compatibles con RDX que no cumplan estos requisitos críticos se quitarán de todos los dispositivos de demostración de venta directa lo antes posible.

* **No pida información de identificación personal (PII)**. Esto incluye la información de inicio de sesión, la información de cuenta de Microsoft o los detalles de contacto.

* **Experiencia sin errores**. La aplicación debe ejecutarse sin errores. Además, no deben mostrarse mensajes emergentes o notificaciones de error a los clientes que usan los dispositivos de demostración comercial. Los errores reflejan negativamente en la propia aplicación, en la marca, en la marca del dispositivo, en la marca de manufacturer's del dispositivo y en la marca de Microsoft.

* Las **aplicaciones de pago deben tener un modo de prueba**. La aplicación debe ser gratuita o incluir un [modo de prueba](./exclude-or-limit-features-in-a-trial-version-of-your-app.md). Los clientes no esperan tener que pagar por una experiencia en una tienda comercial.

### <a name="high-priority-requirements"></a>Requisitos de alta prioridad

Las aplicaciones compatibles con RDX que no cumplen estos requisitos de prioridad alta deben investigarse de inmediato para una corrección. Si no se encuentra ninguna corrección inmediata, es posible que la aplicación se quite de todos los dispositivos de demostración comercial.

* **Experiencia sin conexión memorable**. La aplicación debe demostrar una excelente experiencia sin conexión, ya que el 50% de los dispositivos están sin conexión en las ubicaciones comerciales. Esto sirve para garantizar que los clientes que interactúen con la aplicación sin conexión también puedan tener una experiencia positiva y significativa.

* **Experiencia de contenido actualizada**. La aplicación nunca debe solicitar actualizaciones cuando está en línea. Si se necesitan actualizaciones, deben realizarse de forma silenciosa.

* **Sin comunicación anónima**. Dado que un cliente que usa un dispositivo de demostración comercial es un usuario anónimo, no debe ser capaz de enviar o compartir contenido desde el dispositivo.

* **Ofrezca experiencias coherentes mediante el proceso de limpieza**. Todos los clientes deben tener la misma experiencia cuando usan un dispositivo de demostración comercial. La aplicación debe usar el [proceso de limpieza](#cleanup-process) para volver al mismo estado predeterminado después de cada uso. No queremos que el siguiente cliente vea lo que dejó el último cliente. Esto incluye marcadores, logros y desbloqueos.

* **Contenido adecuado de edad**. Todo el contenido de la aplicación debe tener asignado un adolescente o una categoría de clasificación inferior. Para obtener más información, consulte [obtención de la aplicación clasificada por IARC](https://www.globalratings.com/for-developers.aspx) y [ESRB ratings](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Requisitos de prioridad media

Es posible que el equipo de la tienda comercial de Windows se ponga en contacto directamente con los desarrolladores para analizar cómo solucionar estos problemas.

* **Capacidad para ejecutarse correctamente en una variedad de dispositivos**. Las aplicaciones deben ejecutarse correctamente en todos los dispositivos, incluidos los dispositivos con especificaciones de low-end. Si la aplicación está instalada en dispositivos que no cumplen las especificaciones mínimas, la aplicación debe informar claramente al usuario sobre esto. Los requisitos mínimos del dispositivo deben notificarse para que la aplicación siempre pueda ejecutarse con un alto rendimiento.

* **Cumplir los requisitos de tamaño**de la aplicación de la tienda minorista. El tamaño de la aplicación debe ser inferior a 800 MB. Póngase en contacto directamente con el equipo de la tienda Windows para obtener más información si su aplicación compatible con RDX no cumple los requisitos de tamaño.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: preparación del código para el modo de demostración

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
La propiedad [**IsDemoModeEnabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) de la clase de utilidad [**RetailInfo**](/uwp/api/Windows.System.Profile.RetailInfo) , que forma parte de la [Windows.SysTEM. ](/uwp/api/windows.system.profile) Espacio de nombres de perfil en el SDK de Windows 10, se usa como un indicador booleano para especificar la ruta de acceso del código en la que se ejecuta la aplicación: el modo _normal_ o el modo _comercial_ .

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo. Properties

Cuando [**IsDemoModeEnabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) devuelve true, puede consultar un conjunto de propiedades sobre el dispositivo mediante [**RetailInfo. Properties**](/uwp/api/windows.system.profile.retailinfo.properties) para crear una experiencia de demostración comercial más personalizada. Estas propiedades incluyen [**ManufacturerName**](/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](/uwp/api/windows.system.profile.knownretailinfoproperties.memory), etcétera.

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

```cpp
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

La limpieza comienza dos minutos después de que un comprador deja de interactuar con el dispositivo. La demostración comercial se reproduce y Windows comienza a restablecer los datos de ejemplo de los contactos, las fotografías y otras aplicaciones. En función del dispositivo, esto podría tardar entre 1-5 minutos en restablecer todo de nuevo a normal. Esto garantiza que todos los clientes de la tienda de venta directa pueden hacer frente a un dispositivo y tener la misma experiencia al interactuar con el dispositivo.

Paso 1: limpieza
* Se cierran todas las aplicaciones de Win32 y de la Tienda.
* Se eliminan todos los archivos que se encuentran en carpetas conocidas como __Imágenes__, __Vídeos__, __Música__, __Documentos__, __Imágenes guardadas__, __Álbum de cámara__, __Escritorio__ y __Descargas__.
* Se eliminan los estados de itinerancia estructurados y no estructurados.
* Se eliminan los estados locales estructurados.

Paso 2: configuración
* Para dispositivos sin conexión: las carpetas permanecen vacías
* En el caso de los dispositivos en línea: los recursos de demostración comercial se pueden insertar en el dispositivo desde el Microsoft Store

### <a name="store-data-across-user-sessions"></a>Almacenar datos entre sesiones de usuario

Para almacenar datos en las sesiones de usuario, puede almacenar información en __ApplicationData. Current. TemporaryFolder__ , ya que el proceso de limpieza predeterminado no elimina automáticamente los datos de esta carpeta. Tenga en cuenta que la información almacenada mediante *LocalState* se elimina durante el proceso de limpieza.

### <a name="customize-the-cleanup-process"></a>Personalizar el proceso de limpieza

Para personalizar el proceso de limpieza, implemente `Microsoft-RetailDemo-Cleanup` App Service en la aplicación.

Los escenarios en los que se necesita una lógica de limpieza personalizada incluyen la ejecución de una amplia configuración, la descarga y el almacenamiento en caché de los datos, o no la eliminación de los datos de *LocalState* .

Paso 1: declarar el servicio _Microsoft-RetailDemo-Cleanup_ en el manifiesto de la aplicación.
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

Paso 2: implemente la lógica de limpieza personalizada en la función _AppdataCleanup_ Case mediante la siguiente plantilla de ejemplo.
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

* [Almacenar y recuperar datos de la aplicación](../design/app-settings/store-and-retrieve-app-data.md)
* [Cómo crear y consumir un servicio de aplicaciones](../launch-resume/how-to-create-and-consume-an-app-service.md)
* [Localización de contenido de la aplicación](../design/globalizing/globalizing-portal.md)
* [Experiencia de demostración comercial (RDX)](/windows-hardware/customize/desktop/retail-demo-experience)