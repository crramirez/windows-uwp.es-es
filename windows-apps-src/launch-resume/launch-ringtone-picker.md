---
title: ms-tonepicker scheme
description: En este tema se describe el esquema de URI ms-tonepicker y cómo usarlo para mostrar un selector de tono para seleccionar un tono, guardar un tono y obtener el nombre descriptivo de un tono.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: a57ba895ae77eec3b2c5df2a7ae70864c6f701a1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167879"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>Elegir y guardar los tonos con el esquema de URI ms-tonepicker

En este tema se describe cómo usar el esquema de URI **ms-tonepicker:**. Este esquema de URI puede usarse para:
- Determinar si el selector de tono está disponible en el dispositivo.
- Mostrar el selector de tono para obtener una lista de tonos disponibles, sonidos del sistema, tonos de SMS y sonidos de alarma; y obtener un token de tono que represente el sonido seleccionado por el usuario.
- Mostrar el protector de tono, que toma un token de archivo de sonido como entrada y lo guarda en el dispositivo. A continuación, los tonos guardados están disponibles mediante el selector de tono. Los usuarios también pueden asignar al tono un nombre descriptivo.
- Convertir un token de tono a su nombre descriptivo.

## <a name="ms-tonepicker-uri-scheme-reference"></a>Referencia del esquema de URI ms-tonepicker:

Este esquema de URI no pasa argumentos a través de la cadena de esquema de URI, sino que pasa argumentos a través de una clase [ValueSet](/uwp/api/windows.foundation.collections.valueset). Todas las cadenas distinguen mayúsculas de minúsculas.

En las siguientes secciones se indican los argumentos que se deben pasar para realizar la tarea especificada.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>Tarea: Determinar si el selector de tono está disponible en el dispositivo
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>Tarea: Mostrar el selector de tono

Los argumentos que puedes pasar para mostrar el selector de tono son los siguientes:

| Parámetro | Tipo | Obligatorio | Valores posibles | Descripción |
|-----------|------|----------|-------|-------------|
| Acción | string | sí | "PickRingtone" | Abre el selector de tono. |
| CurrentToneFilePath | string | no | Un token de tono existente. | Tono para mostrar como el tono actual en el selector de tono. Si no se establece este valor, el primer tono de la lista se selecciona de manera predeterminada.<br>No se trata, estrictamente hablando, de una ruta de archivo. Puedes obtener un valor adecuado para `CurrenttoneFilePath` del valor `ToneToken` que devuelve el selector de tono.  |
| TypeFilter | string | no | "Tonos", "Notificaciones", "Alarmas", "Ninguno" | Selecciona los tonos que se van a agregar al selector. Si no se especifica ningún filtro, se muestran todos los tonos. |

Valores que se devuelven en [LaunchUriResults.Result](/uwp/api/windows.system.launchuriresult.result):

| Valores devueltos | Tipo | Valores posibles | Descripción |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0: correcto. <br>1: cancelado <br>7: parámetros no válidos. <br>8: ningún tono coincide con los criterios de filtro. <br>255: la acción especificada no está implementada. | Resultado de la operación del selector. |
| ToneToken | string | Token del tono seleccionado. <br>La cadena está vacía si el usuario selecciona **default** en el selector. | Este token puede usarse en una carga de notificación del sistema, o puede asignarse como tono de un contacto o tono de SMS. El parámetro se devuelve en la clase ValueSet solamente si **Result** es 0. |
| DisplayName | string | Nombre descriptivo del tono especificado. | Cadena que puede mostrarse al usuario para representar el tono seleccionado. El parámetro se devuelve en la clase ValueSet solamente si **Result** es 0. |


**Ejemplo: Abrir el selector de tono para que el usuario pueda seleccionar un tono**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>Tarea: Mostrar el protector de tono

Los argumentos que puedes pasar para mostrar el protector de tono son los siguientes:

| Parámetro | Tipo | Obligatorio | Valores posibles | Descripción |
|-----------|------|----------|-------|-------------|
| Acción | string | sí | "SaveRingtone" | Abre el selector para guardar un tono. |
| ToneFileSharingToken | string | sí | Token de uso compartido de archivos [SharedStorageAccessManager](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager) para el archivo de tonos que se va a guardar. | Guarda un archivo de sonido específico como un tono. Los tipos de contenido admitidos para el archivo son audio MPEG y audio x-ms-wma. |
| DisplayName | string | no | Nombre descriptivo del tono especificado. | Establece el nombre para mostrar que se usará al guardar el tono especificado. |

Valores que se devuelven en [LaunchUriResults.Result](/uwp/api/windows.system.launchuriresult.result):

| Valores devueltos | Tipo | Valores posibles | Descripción |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0: correcto.<br>1: cancelado por el usuario.<br>2: archivo no válido.<br>3: tipo de contenido de archivo no válido.<br>4: el archivo supera el tamaño de tono máximo (1 MB en Windows 10).<br>5: el archivo supera la duración máxima de 40 segundos.<br>6: el archivo está protegido por administración de derechos digitales.<br>7: parámetros no válidos. | Resultado de la operación del selector. |

**Ejemplo: Guardar un archivo de música local como un tono**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>Tarea: Convertir un token de tono a su nombre descriptivo

Los argumentos que puedes pasar para obtener el nombre descriptivo de un tono son los siguientes:

| Parámetro | Tipo | Obligatorio | Valores posibles | Descripción |
|-----------|------|----------|-------|-------------|
| Acción | string | sí | "GetToneName" | Indica que quieres obtener el nombre descriptivo de un tono. |
| ToneToken | string | sí | Token de tono | Token de tono desde el que se obtiene un nombre para mostrar. |

Valores que se devuelven en [LaunchUriResults.Result](/uwp/api/windows.system.launchuriresult.result):

| Valor devuelto | Tipo | Valores posibles | Descripción |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0: la operación del selector se realizó correctamente.<br>7: parámetro incorrecto (por ejemplo, no se proporcionó ningún valor ToneToken).<br>9: error al leer el nombre del token especificado.<br>10: no se encuentra el token de tono especificado. | Resultado de la operación del selector.
| DisplayName | string | Nombre descriptivo del tono. | Devuelve el nombre para mostrar del tono seleccionado. Este parámetro solo se devuelve en la clase ValueSet si **Result** es 0. |

**Ejemplo: Recuperar un token de tono de Contact.RingToneToken y mostrar su nombre descriptivo en la tarjeta de contacto.**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```