---
author: TylerMSFT
title: Guardar y cargar la configuración en una aplicación para UWP
description: Obtén información acerca de cómo guardar y cargar la configuración de la aplicación en aplicaciones de la Plataforma universal de Windows.
ms.author: twhitney
ms.date: 05/07/2018
ms.topic: article
keywords: introducción, uwp, windows 10, pista de aprendizaje, configuración, guardar configuración, cargar configuración
ms.localizationpriority: medium
ms.openlocfilehash: 18b11ea100915f8b6ff52db5223284da6f24a1d4
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5542143"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>Guardar y cargar la configuración en una aplicación para UWP

En este tema se explica lo que necesitas saber para empezar a cargar, y guardar, la configuración en una aplicación de la Plataforma universal de Windows (UWP). Se presentan las API principales y se proporcionan vínculos que te ayudarán a obtener más información.

Usa la configuración para recordar los aspecto personalizables por el usuario de la aplicación. Por ejemplo, un lector de noticias podría usar la configuración de la aplicación para guardar qué fuentes de noticias se mostrarán y qué fuente se usará para leer artículos.

Examinaremos el código para guardar y cargar la configuración de la aplicación, incluida la configuración local y de itinerancia.

## <a name="what-do-you-need-to-know"></a>Qué debes saber

Usa la configuración de la aplicación para guardar datos de configuración como las preferencias del usuario y el estado de la aplicación.  La configuración específica del dispositivo se almacena localmente. La configuración aplicable a cualquier dispositivo en el que esté instalada tu aplicación se almacena en el almacén de datos de itinerancia. La configuración es móvil entre dispositivos en los que el usuario inicia sesión con la misma cuenta de Microsoft y tiene la misma versión de la aplicación instalada.

Se pueden usar los siguientes tipos de datos con la configuración: enteros, dobles, controles flotantes, caracteres, cadenas, puntos, fechas y horas, etc. También puedes almacenar instancias de la clase [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) que es útil cuando hay varias opciones de configuración que deben tratarse como una unidad. Por ejemplo, un tamaño de fuente y un tamaño de punto para mostrar texto en el panel de lectura de la aplicación se deben guardar y restaurar como una sola unidad. Esto impide que una configuración no se sincronice con la otra debido a retrasos en el roaming de una configuración antes de la otra.

Estas son las API principales que necesitas saber sobre cómo guardar o cargar la configuración de la aplicación:

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) obtiene el contenedor de configuraciones de la aplicación desde el almacén de datos de aplicación locales. La configuración del almacén aquí que no es adecuada para roaming entre dispositivos porque representan estado específico de este dispositivo o es demasiado grande.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) obtiene el contenedor de configuraciones de la aplicación desde el almacén de datos de aplicación de roaming. Estos datos se transfieren entre dispositivos.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) es un contenedor que representa la configuración de la aplicación como pares clave/valor. Usa esta clase para crear y recuperar valores de configuración.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) representa varias opciones de configuración de aplicación que se deben serializar como unidad. Esto es útil cuando una opción de configuración no se debe actualizar independientemente de la otra.

## <a name="save-app-settings"></a>Guardar configuración de la aplicación

Para esta introducción, nos centraremos en dos escenarios sencillos: guardar y cargar una configuración de aplicación localmente, y realizar el roaming de una configuración de tamaño de fuente o fuente compuesta entre dispositivos.

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

Para guardar una configuración en el dispositivo local, obtén primer un **ApplicationDataContainer** para el almacén de datos de configuración local con `Windows.Storage.ApplicationData.Current.LocalSettings`. Los pares de diccionario clave/valor que asignas a esta instancia se guardan en el almacén de datos de configuración de dispositivo local.

Guarda una configuración de roaming con una trama similar. Obtén primero un **ApplicationDataContainer** para el almacén de datos de configuración de roaming con `Windows.Storage.ApplicationData.Current.RoamingSettings`. A continuación, asigna pares clave/valor a esta instancia.  Esos pares clave/valor se pasarán automáticamente de un dispositivo a otro.

En el fragmento de código anterior, un **ApplicationDataCompositeValue** almacena varios pares clave/valor. Los valores compuestos son útiles cuando tienes varias opciones de configuración que no deberían dejar de sincronizarse entre sí. Cuando guardes un **ApplicationDataCompositeValue**, los valores se guardan y se cargan como unidad o de forma atómica. De esta manera, la configuración relacionada no se deja de sincronizar porque se pasan como unidad en lugar de en forma individual.

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

Para cargar una configuración desde el dispositivo local, obtén primero una instancia de **ApplicationDataContainer** para el almacén de datos de configuración local con `Windows.Storage.ApplicationData.Current.LocalSettings`. A continuación, úsala para recuperar pares de clave y valor.

Para cargar una configuración de roaming, sigue una trama similar. Obtén primero una instancia de **ApplicationDataContainer** desde el almacén de datos de configuración de roaming con `Windows.Storage.ApplicationData.Current.RoamingSettings`. Obtén acceso a los pares de clave y valor desde esa instancia. Si los datos no se han pasado aún al dispositivo desde el que está obteniendo acceso a la configuración, obtendrás un valor de **ApplicationDataContainer ** nulo. Ese es el motivo por el que hay una comprobación `if (composite != null)` en el código de ejemplo anterior.

## <a name="useful-apis-and-docs"></a>API y documentos de utilidad

Este es un resumen rápido de las API y otra documentación útiles que te ayudarán a comenzar a guardar y cargar configuración de aplicación.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
| [ApplicationData.LocalSettings](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.temporaryfolder) | Obtiene el contenedor de configuración de la aplicación del almacén de datos local de la aplicación. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | Obtiene el contenedor de configuración de la aplicación del almacén de datos de roaming de la aplicación. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | Contenedor de configuración de aplicación que admite la creación, la eliminación, la enumeración y el recorrido de la jerarquía de contenedores. |
| [Windows.UI.ApplicationSettings Namespace](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Proporciona clases que usarás para definir la configuración de la aplicación que aparece en el panel de configuración del shell de Windows. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Directrices para la configuración de una aplicación](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | Describe los procedimientos recomendados para crear y mostrar la configuración de la aplicación. |
| [Almacenar y recuperar la configuración y otros datos de aplicación](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | Tutorial para guardar y recuperar la configuración, incluida la configuración de roaming. |

## <a name="useful-code-samples"></a>Muestras de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Ejemplo de los datos de la aplicación](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | Escenarios 2 y 4 Enfoque en la configuración |