---
author: joannaleecy
title: Crear una aplicación de experiencia de demostración comercial
description: Crea una aplicación de experiencia de demostración comercial (RDX); es decir, una única aplicación que puede iniciarse en modo de demostración comercial y en modo normal.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, aplicación de demostración comercial
ms.localizationpriority: medium
ms.openlocfilehash: 19a22e09484943d63988cef6bb6a7e7c09e016dd
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691021"
---
#  <a name="create-a-retail-demo-experience-rdx-app"></a>Crear una aplicación de experiencia de demostración comercial (RDX)

Cuando los clientes entran en una tienda o un establecimiento comercial, esperan encontrar los equipos y los teléfonos móviles más recientes en exposición y estos dispositivos que se exhiben se conocen como dispositivos de demostración comercial.
Los dispositivos de demostración comercial y el contenido que se instala en ellos son, en gran medida, responsables de la experiencia del cliente en las tiendas porque los clientes suelen dedicar una parte importante de su tiempo a divertirse con estos dispositivos.

Las aplicaciones que se instalan en estos equipos y teléfonos móviles de las tiendas minoristas deben ser aplicaciones aptas para la experiencia de demostración comercial (RDX). En este artículo se proporciona información general sobre cómo diseñar y desarrollar una versión de demostración comercial de una aplicación para instalarla en equipos y dispositivos móviles comerciales en una tienda minorista.

Una aplicación de experiencia de demostración comercial viene en una única compilación que puede iniciarse en uno de dos modos diferentes: _normal_ o _comercial_.
Desde la perspectiva de nuestros clientes, hay una sola aplicación y para ayudarlos a distinguir entre las dos versiones, se recomienda que la aplicación que se ejecuta en el modo comercial muestre la palabra "Comercial" en un lugar destacado en la barra de título o en una ubicación adecuada.

Además de los requisitos de la Store para las aplicaciones, las aplicaciones de RDX también deben ser totalmente compatibles con el sistema de configuración, limpieza y actualización de los dispositivos de demostración comercial para garantizar que los clientes tengan una experiencia positiva coherente en la tienda minorista.

## <a name="design-principles"></a>Principios de diseño

### <a name="show-your-best"></a>Mostrar lo mejor que se tiene

Usa la experiencia de demostración comercial para exhibir por qué tu aplicación es genial.  Es probable que esta sea la primera vez que el cliente vea la aplicación y, por lo tanto, debes lucirte.

### <a name="show-it-fast"></a>Mostrarlo rápido

Los clientes pueden impacientarse. Por eso, cuanto más rápido puedan experimentar el valor real de la aplicación, mejor.

### <a name="keep-the-story-simple"></a>Simplificar

Recuerda que la experiencia de demostración comercial es un argumento de venta para el valor de la aplicación.

### <a name="focus-on-the-experience"></a>Centrarse en la experiencia

Permite que el usuario tenga tiempo para asimilar el contenido.  Si bien es importante llevarlo a la mejor parte rápido, diseñar pausas adecuadas puede ayudarlo a disfrutar completamente de la experiencia.

## <a name="technical-requirements"></a>Requisitos técnicos

Dado que las aplicaciones de experiencia de demostración comercial están diseñadas para mostrar lo mejor de la aplicación a los clientes minoristas, es fundamental cumplir los siguientes requisitos técnicos y ajustarse a las normativas de privacidad que la Store tiene para todas las aplicaciones de experiencia de demostración comercial.
Esto también puede usarse como una lista de comprobación que te ayudará a prepararte para el proceso de validación y para proporcionar mayor claridad en el proceso de pruebas. Ten en cuenta que estos requisitos deben mantenerse no solo para el proceso de validación, sino para toda la duración de la aplicación de experiencia de demostración comercial, siempre que la aplicación se siga ejecutando en los dispositivos de demostración comercial.

### <a name="critical-level-requirements"></a>Requisitos de nivel crítico

Las aplicaciones de RDX que no cumplan con estos requisitos críticos se quitarán de todos los dispositivos de demostración comercial tan pronto como sea posible.

* No debe solicitarse información de identificación personal (PII)

    No se permite que la aplicación solicite a los clientes información de identificación personal.  Esto incluye toda la información de cuenta de Microsoft, los detalles de los contactos, etc.

* Experiencia sin errores

    La aplicación debe ejecutarse sin errores. Además, no deben mostrarse mensajes emergentes o notificaciones de error a los clientes que usan los dispositivos de demostración comercial. Esto es importante porque queremos presentar a los clientes lo mejor que tenemos y lo mejor no debe contener errores.
    Otro motivo es que los errores tienen un impacto negativo en la aplicación en sí, así como en tu marca, el dispositivo en el que se ejecuta la aplicación, la marca del fabricante del dispositivo y, también, la marca de Microsoft.

* Las aplicaciones de pago deben tener un modo de prueba

    Para que las aplicaciones se instalen en dispositivos de demostración comercial, la aplicación debe ser gratuita o tener un modo de prueba establecido.  Los clientes no esperan tener que pagar por una experiencia en una tienda comercial. Para obtener más información, consulta [Excluir o limitar las características de una versión de prueba](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)

### <a name="high-priority-requirements"></a>Requisitos de prioridad alta

Las aplicaciones de RDX que no cumplan con estos requisitos de prioridad alta se deben investigar para encontrar una corrección inmediata. Si no se encuentra ninguna corrección inmediata, es posible que la aplicación se quite de todos los dispositivos de demostración comercial.

* Experiencia sin conexión memorable

    La aplicación de experiencia de demostración comercial debe mostrar una estupenda experiencia sin conexión, ya que aproximadamente el 50% de los dispositivos están sin conexión en los establecimientos minoristas. Esto sirve para garantizar que los clientes que interactúen con la aplicación sin conexión también puedan tener una experiencia positiva y significativa.

* Experiencia de contenido actualizado

    Para ofrecer una magnífica experiencia, la aplicación siempre debe estar actualizada y nunca se les debe pedir a los clientes que actualicen la aplicación cuando está en línea.

* Sin comunicaciones anónimas

    Dado que un cliente que usa un dispositivo de demostración comercial es un usuario anónimo, se debe impedir que envíe contenido del dispositivo por mensaje o que lo comparta.

* Ofrecer una experiencia uniforme mediante el proceso de limpieza

    Todos los clientes deben tener la misma experiencia cuando usan un dispositivo de demostración comercial. La aplicación debe usar un [proceso de limpieza](#clean-up-process) para regresar al mismo estado predeterminado después de cada uso, ya que no queremos que el cliente siguiente vea la actividad del último cliente.  Esto incluye marcadores, logros y desbloqueos.

* Contenido adecuado según la edad

    Todo el contenido de una aplicación de demostración comercial debe ser apto para adolescentes o una clasificación inferior. Para obtener más información, consulta cómo [obtener la clasificación de IARC para tu aplicación](https://www.globalratings.com/for-developers.aspx) y las [clasificaciones de ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Requisitos de prioridad media

Es posible que el equipo de la tienda comercial de Windows se ponga en contacto directamente con los desarrolladores para analizar cómo solucionar estos problemas.

* Capacidad para ejecutarse correctamente en una gama de dispositivos

    Las aplicaciones de experiencia de demostración comercial deben ejecutarse correctamente en todos los dispositivos, incluidos los dispositivos con especificaciones de gama baja. Si la aplicación de experiencia de demostración comercial se instala en dispositivos que no cumplen con las especificaciones mínimas para ejecutar la aplicación, esta debe notificar claramente al usuario al respecto. Los requisitos mínimos del dispositivo deben notificarse para que la aplicación siempre pueda ejecutarse con un alto rendimiento.

* Cumplir los requisitos de tamaño de aplicación de la tienda comercial

    El tamaño de la aplicación debe ser inferior a 800MB. Si tu aplicación de experiencia de demostración comercial no cumple con los requisitos de tamaño, ponte en contacto directamente con el equipo de la tienda comercial de Windows para comentarlo con ellos.

## <a name="preparing-codebase-for-retail-demo-mode-development"></a>Preparar el código base para el desarrollo del modo de demostración comercial

La propiedad [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) de la clase de utilidad [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo), que forma parte del espacio de nombres [Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) en el SDK de Windows 10, se usa como un indicador booleano para especificar en qué ruta de acceso de código se ejecuta la aplicación: el modo _normal_ o el modo _comercial_.

Cuando [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) devuelve "true", puedes consultar un conjunto de propiedades sobre el dispositivo mediante el uso de [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) para crear una experiencia de demostración comercial más personalizada. Estas propiedades incluyen [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory), etcétera.


## <a name="clean-up-process"></a>Proceso de limpieza

El proceso de limpieza se usa para restablecer automáticamente los dispositivos de demostración comercial a la configuración predeterminada original cuando no hay interacción con el dispositivo durante un tiempo fijo. Esto sirve para garantizar que todos los usuarios de la tienda minorista puedan usar un dispositivo y tengan exactamente la misma experiencia predeterminada prevista cuando interactúen con el dispositivo. Al desarrollar una aplicación de experiencia de demostración comercial, es importante comprender cuándo y cómo se desencadena el proceso de limpieza, así como qué sucede durante el proceso de limpieza predeterminado y cómo personalizar este proceso en función de los requisitos de la experiencia de demostración comercial prevista.

### <a name="when-does-clean-up-begin"></a>¿Cuándo se inicia la limpieza?

La secuencia de limpieza comienza después de una determinada cantidad de tiempo de inactividad del dispositivo. El tiempo de inactividad inicia el recuento cuando no hay ninguna entrada de la función táctil, el mouse y el teclado en el dispositivo.

#### <a name="desktoppc"></a>Escritorio o PC

Después de 120 segundos de tiempo de inactividad, el vídeo de la aplicación de exposición del tiempo de inactividad empezará a reproducirse en el dispositivo. Cinco segundos después, se iniciará el proceso de limpieza.

#### <a name="phone"></a>Teléfono

Después de 60 segundos de tiempo de inactividad, el vídeo de la aplicación de exposición del tiempo de inactividad empezará a reproducirse en el dispositivo y se iniciará el proceso de limpieza de inmediato.

### <a name="what-happens-during-a-default-clean-up-process"></a>¿Qué sucede durante un proceso de limpieza predeterminado?

#### <a name="step-1-clean-up"></a>Paso 1: Limpiar
* Se cierran todas las aplicaciones de Win32 y de la Store.
* Se eliminan todos los archivos que se encuentran en carpetas conocidas como __Imágenes__, __Vídeos__, __Música__, __Documentos__, __Imágenes guardadas__, __Álbum de cámara__, __Escritorio__ y __Descargas__.
* Se eliminan los estados de itinerancia estructurados y no estructurados.
* Se eliminan los estados locales estructurados.

#### <a name="step-2-set-up"></a>Paso 2: Configurar
* Para dispositivos sin conexión: las carpetas permanecen vacías
* Para dispositivos en línea: los activos de demostración comercial pueden insertarse en el dispositivo de Microsoft Store

### <a name="how-to-store-data-across-user-sessions"></a>¿Cómo se almacenan los datos de las sesiones de usuario?

Si quieres almacenar datos de las sesiones de usuario, puedes almacenar información en __ApplicationData.Current.TemporaryFolder__, ya que el proceso de limpieza predeterminado no elimina automáticamente los datos de esta carpeta. Ten en cuenta que la información almacenada mediante *LocalState* se elimina durante el proceso de limpieza.

### <a name="how-to-customize-the-clean-up-process"></a>¿Cómo se personaliza el proceso de limpieza?

Si quieres personalizar el proceso de limpieza, tienes que implementar el servicio de aplicaciones `Microsoft-RetailDemo-Cleanup` en la aplicación.

Los escenarios en los que se necesita una lógica de limpieza personalizada incluyen la ejecución de una instalación costosa, la descarga y el almacenamiento de datos en caché o que no se deseen eliminar los datos de *LocalState*.

Paso 1: Declarar el servicio _Microsoft-RetailDemo-Cleanup_ en el manifiesto de la aplicación.
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

Paso 2: Implementar la lógica de limpieza personalizada en la función "case" _AppdataCleanup_ mediante el uso de la plantilla de ejemplo siguiente.
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


 

 
