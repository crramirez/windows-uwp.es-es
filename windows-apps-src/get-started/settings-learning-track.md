---
title: Guardar y cargar la configuración en una aplicación para UWP
description: Obtenga información acerca de cómo guardar y cargar la configuración de la aplicación en las aplicaciones de la Plataforma universal de Windows.
ms.date: 05/07/2018
ms.topic: article
keywords: introducción, uwp, windows 10, pista de aprendizaje, configuración, guardar configuración, cargar configuración
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 490dd8f0f3841fae089626ec9c283d54cc0d8cd9
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66370495"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>Guardar y cargar la configuración en una aplicación para UWP

En este tema se explica lo que necesita saber para comenzar a cargar y guardar configuraciones en una aplicación de la Plataforma universal de Windows (UWP). Se presentan las API principales y se proporcionan vínculos que le ayudarán a obtener más información.

Use la configuración para recordar los aspectos personalizables de su aplicación. Por ejemplo, un lector de noticias podría usar la configuración de la aplicación para guardar qué fuentes de noticias se mostrarán y qué fuente se usará para leer artículos.

Veremos el código para guardar y cargar la configuración de la aplicación, incluida la configuración local y de itinerancia.

## <a name="what-do-you-need-to-know"></a>Qué debe saber

Use la configuración de la aplicación para guardar datos de configuración como las preferencias del usuario y el estado de la aplicación.  La configuración específica del dispositivo se almacena localmente. Las configuraciones que se aplican en cualquier dispositivo en el que esté instalada su aplicación se almacenan en el almacén de datos de itinerancia. La configuración es móvil entre dispositivos en los que el usuario ha iniciado sesión con la misma cuenta de Microsoft y tiene instalada la misma versión de la aplicación.

Se pueden usar los siguientes tipos de datos con la configuración: enteros, dobles, controles flotantes, caracteres, cadenas, puntos, fechas y horas, etc. También puede almacenar instancias de la clase [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) que es útil cuando hay varias opciones de configuración que deben tratarse como una unidad. Por ejemplo, un tamaño de fuente y un tamaño de punto para mostrar texto en el panel de lectura de su aplicación se deben guardar y restaurar como una sola unidad. Esto impide que una configuración pierda la sincronización con la otra debido a retrasos en la itinerancia de una configuración antes de la otra.

Estas son las principales API que debe conocer para guardar o cargar la configuración de la aplicación:

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) obtiene el contenedor de configuración de la aplicación del almacén de datos de la aplicación local. La configuración de Store aquí que no es adecuada para itinerancia entre dispositivos porque representan estado específico de este dispositivo o es demasiado grande.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) obtiene el contenedor de configuración de la aplicación del almacén de datos de la aplicación de itinerancia. Estos datos se transfieren entre dispositivos.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer)es un contenedor que representa la configuración de la aplicación como pares clave/valor. Use esta clase para crear y recuperar valores de configuración.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) representa varias opciones de configuración de aplicación que se deben serializar como unidad. Esto es útil cuando configuración no se debe actualizar independientemente de la otra.

## <a name="save-app-settings"></a>Guardar configuración de la aplicación

Para esta introducción, nos centraremos en dos escenarios sencillos: guardar y cargar una configuración de aplicación localmente, y realizar la itinerancia de una configuración de tamaño de fuente o fuente compuesta entre dispositivos.

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

Para guardar una configuración en el dispositivo local, obtenga primero un **ApplicationDataContainer** para el almacén de datos de configuración local con `Windows.Storage.ApplicationData.Current.LocalSettings`. Los pares de diccionario clave/valor que asigna a esta instancia se guardan en el almacén de datos de configuración de dispositivo local.

Guarda una configuración de itinerancia con una trama similar. Obtenga primero un **ApplicationDataContainer** para el almacén de datos de configuración de itinerancia con `Windows.Storage.ApplicationData.Current.RoamingSettings`. A continuación, asigne pares clave/valor a esta instancia.  Esos pares clave/valor se pasarán automáticamente de un dispositivo a otro.

En el fragmento de código anterior, un **ApplicationDataCompositeValue** almacena varios pares clave/valor. Los valores compuestos son útiles cuando tiene varias opciones de configuración que no deberían dejar de sincronizarse entre sí. Cuando guarde un **ApplicationDataCompositeValue**, los valores se guardan y se cargan como unidad o de forma atómica. De esta manera, las configuraciones relacionadas no se dejan de sincronizar porque se desplazan como una unidad en lugar de en forma individual.

## <a name="load-app-settings"></a>Cargar configuración de aplicaciones

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

Para cargar una configuración desde el dispositivo local, obtenga primero una instancia de **ApplicationDataContainer** para el almacén de datos de configuración local con `Windows.Storage.ApplicationData.Current.LocalSettings`. A continuación, úsela para recuperar pares de clave y valor.

Para cargar una configuración de itinerancia, siga una trama similar. Obtenga primero una instancia de **ApplicationDataContainer** desde el almacén de datos de configuración de itinerancia con `Windows.Storage.ApplicationData.Current.RoamingSettings`. Obtenga acceso a los pares de clave y valor desde esa instancia. Si los datos no se han pasado aún al dispositivo desde el que está obteniendo acceso a la configuración, obtendrá un valor de **ApplicationDataContainer**  nulo. Ese es el motivo por el que hay una comprobación `if (composite != null)` en el código de ejemplo anterior.

## <a name="useful-apis-and-docs"></a>API y documentos útiles

Este es un resumen rápido de las API y otra documentación útil para ayudarlo a comenzar a guardar y cargar la configuración de la aplicación.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
| [ApplicationData.LocalSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) | Obtiene el contenedor de configuración de la aplicación del almacén de datos de la aplicación local. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | Obtiene el contenedor de configuración de la aplicación del almacén de datos de la aplicación de itinerancia. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | Un contenedor para la configuración de la aplicación que soporte crear, eliminar, enumerar y el recorrido de la jerarquía de contenedores. |
| [Windows.UI.ApplicationSettings Namespace](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Proporciona clases que utilizará para definir la configuración de la aplicación que aparece en el panel de configuración del shell de Windows. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Instrucciones para la configuración de una aplicación](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | Describe los procedimientos recomendados para crear y mostrar la configuración de la aplicación. |
| [Almacenar y recuperar la configuración y otros datos de aplicación](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | Recorrido para guardar y recuperar la configuración, incluida la configuración de itinerancia. |

## <a name="useful-code-samples"></a>Ejemplos de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Application data sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) (ejemplo de datos de aplicaciones) | Los escenarios 2-4 se centran en la configuración |
