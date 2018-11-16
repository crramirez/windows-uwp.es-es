---
author: mijacobs
Description: Learn how to store and retrieve local, roaming, and temporary app data.
title: Almacenar y recuperar la configuración y otros datos de aplicación
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.author: mijacobs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2af9aec9742a950f7bd35794ff4d10763e6c2c5c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995229"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Almacenar y recuperar la configuración y otros datos de la aplicación

Los *datos de aplicación* son datos mutables específicos de una aplicación concreta. Incluyen el estado de tiempo de ejecución, las preferencias del usuario y otras opciones de configuración. Los datos de la aplicación son diferentes de los *datos de usuario*, los datos que el usuario crea y administra al usar una aplicación. Entre los datos de usuario se incluyen archivos multimedia o documentos, correos electrónicos o transcripciones de comunicaciones, o registros de bases de datos que contienen contenido creado por el usuario. Los datos de usuario pueden ser útiles o significativos a más de una aplicación. Con frecuencia, se trata de datos que el usuario quiere manipular o transmitir como entidad independiente de la propia aplicación (como, por ejemplo, un documento).

**Nota importante sobre los datos de la aplicación:** La duración de los datos de la aplicación está vinculada a la duración de la aplicación. Si una aplicación se quita, todos sus datos se perderán en consecuencia. No uses datos de la aplicación para almacenar datos del usuario o cualquier elemento que se considere valioso o irreemplazable. Recomendamos almacenar este tipo de información en las bibliotecas del usuario o en Microsoft OneDrive. Los datos de la aplicación son perfectos para almacenar la configuración, las preferencias de usuario específicas de la aplicación y los favoritos.

## <a name="types-of-app-data"></a>Tipos de datos de aplicación


Hay dos tipos de datos de aplicación: configuración y archivos.

-   **Configuración**

    Configura para almacenar las preferencias del usuario y la información del estado de la aplicación. La API de datos de la aplicación te permite crear fácilmente y recuperar la configuración (te mostramos algunos ejemplos más adelante en este artículo).

    Estos son los tipos de datos que puedes usar para la configuración de la aplicación:

    -   **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
    -   **Booleano**
    -   **Char16**, **String**
    -   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588): es una configuración de un conjunto de aplicaciones relacionadas que se debe serializar y deserializar de forma atómica. Usa configuraciones compuestas para procesar fácilmente las actualizaciones atómicas de configuraciones interdependientes. El sistema se encarga de garantizar la integridad de las configuraciones compuestas durante el acceso simultáneo y la itinerancia. Las configuraciones compuestas se optimizan para pequeñas cantidades de datos, y el rendimiento puede ser deficiente si se usan para grandes conjuntos de datos.
-   **Archivos**

    Usa archivos para almacenar datos binarios o para permitir tus propios tipos serializados personalizados.

## <a name="storing-app-data-in-the-app-data-stores"></a>Almacenar datos de aplicación en los almacenes de datos de la aplicación


Cuando se instala una aplicación, el sistema le ofrece sus propios almacenes de datos por usuario para configuración y archivos. No tiene que saber dónde y cómo existen estos datos, porque el sistema es responsable de administrar el almacenamiento físico, garantizando que los datos se mantienen aislados de otras aplicaciones y otros usuarios. También conserva el contenido de estos almacenes de datos cuando el usuario instala una actualización de la aplicación y quita el contenido de estos almacenes de datos de forma total y limpia al desinstalar la aplicación.

En su almacén de datos de aplicación, cada aplicación tiene directorios raíz definidos por el sistema: uno para archivos locales, uno para archivos móviles y otro para archivos temporales. La aplicación puede agregar nuevos archivos y contenedores a cada uno de estos directorios raíz.

## <a name="local-app-data"></a>Datos de aplicaciones locales


Los datos de aplicaciones locales deben usarse para la información que deba conservarse de una sesión de la aplicación a otra y que no sea apropiada, para datos de la aplicación móviles. Aquí también deben almacenarse los datos que no sean aplicables a otros dispositivos. No hay restricciones generales en cuanto al tamaño de los datos locales almacenados. Usa el almacén de datos de aplicación locales para los datos que no tenga sentido mover y para grandes conjuntos de datos.

### <a name="retrieve-the-local-app-data-store"></a>Recuperar el almacén de datos de la aplicación local

Para poder leer o escribir datos de aplicaciones locales, debes recuperar el almacén de datos de la aplicación local. Para recuperar el almacén de datos locales de la aplicación, usa la propiedad [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) para conseguir la configuración local de la aplicación como objeto [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599). Usa la propiedad [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) para obtener los archivos en un objeto [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). Usa la propiedad [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825) para obtener la carpeta del almacén de datos locales de la aplicación en la que puedes guardar los archivos que non se incluyen en la copia de seguridad ni en la restauración.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Crear y recuperar una configuración local simple

Para crear o escribir una configuración, usa la propiedad [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) para obtener acceso a la configuración en el contenedor `localSettings` que obtuvimos en el paso anterior. En este ejemplo se crea una configuración llamada `exampleSetting`.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Para recuperar la configuración, usa la misma propiedad [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) que usaste para crear la configuración. En este ejemplo se muestra cómo recuperar la configuración que acabamos de crear.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Crear y recuperar un valor compuesto local

Para crear o escribir un valor compuesto, crea un objeto [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588). En este ejemplo se crea una configuración compuesta llamada `exampleCompositeSetting` y se la agrega al contenedor `localSettings`.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

En este ejemplo se muestra cómo recuperar el valor compuesto que acabamos de crear.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>Crear y leer un archivo local

Para crear y actualizar un archivo en el almacén de datos locales de la aplicación, usa las API de archivo, como [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) y [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). En este ejemplo se crea un archivo llamado `dataFile.txt` en el contenedor `localFolder` y se escriben la fecha y la hora actuales en el archivo. El valor **ReplaceExisting** de la enumeración [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indica que se reemplace el archivo si ya existe.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Para abrir y leer un archivo en el almacén de datos locales de la aplicación, usa las API de archivo, como [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) y [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). En este ejemplo se abre el archivo `dataFile.txt` creado en el paso anterior y se lee la fecha que aparece en él. Para obtener información detallada sobre la carga de recursos de archivos de varias ubicaciones, consulta [Cómo cargar recursos de archivos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>Datos de movilidad


Si usas datos móviles en la aplicación, los usuarios pueden mantener los datos de aplicación sincronizados en varios dispositivos fácilmente. Si un usuario instala la aplicación en varios dispositivos, el sistema operativo mantiene los datos de aplicación sincronizados, lo que reduce el trabajo de configuración que el usuario debe realizar para la aplicación en el segundo dispositivo. El uso de datos móviles también permite a los usuarios continuar con una tarea, como crear una lista, justo donde la dejaron, incluso en otro dispositivo. El sistema operativo replica los datos móviles en la nube cuando se actualizan y sincroniza los datos con los otros dispositivos en los que la aplicación está instalada.

El sistema operativo limita el tamaño de los datos de itinerancia de cada aplicación. Consulta [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625). Si la aplicación alcanza el límite, no se replicará ningún dato de la aplicación en la nube hasta que la cantidad total de datos de itinerancia de la aplicación vuelva a ser inferior a ese límite. Por esta razón, es recomendable usar datos móviles únicamente para preferencias de usuario, vínculos y archivos de datos pequeños.

Los datos móviles de una aplicación están disponibles en la nube siempre que el usuario obtenga acceso a los mismos desde algún dispositivo en el intervalo de tiempo requerido. Si transcurre más tiempo sin que el usuario ejecute la aplicación, se quitarán estos datos móviles de la nube. Si un usuario desinstala una aplicación, sus datos móviles no se quitarán automáticamente de la nube, sino que se conservarán. Si el usuario reinstala la aplicación en este intervalo de tiempo, se sincronizarán los datos de itinerancia desde la nube.

### <a name="roaming-data-dos-and-donts"></a>Qué hacer y qué no con los datos de itinerancia

-   Usaa la itinerancia para las preferencias y personalizaciones, vínculos y pequeños archivos de datos. Por ejemplo, usa los datos móviles para mantener la preferencia de color de fondo de un usuario en todos sus dispositivos.
-   Usa los datos móviles para que los usuarios puedan continuar con una tarea en diferentes dispositivos. Por ejemplo, incluye en los datos móviles de la aplicación información del tipo del contenido del borrador de un correo electrónico o de la última página vista en una aplicación de lectura.
-   Controla el evento [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) actualizando los datos de la aplicación. Este evento se produce cuando los datos de la aplicación acaban de terminar de sincronizarse desde la nube.
-   La movilidad hace referencia al contenido más que a los datos sin procesar. Por ejemplo, incluye en los datos de itinerancia una URL en lugar del contenido de un artículo en línea.
-   Para las configuraciones importantes en las que el tiempo es fundamental, usa el parámetro *HighPriority* asociado a [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624).
-   No incluyas como datos móviles aquellos datos de la aplicación que sean específicos de un dispositivo. Alguna información solo es relevante de forma local como, por ejemplo, el nombre de una ruta de acceso a un recurso de archivos locales. Si decides incluir información local como datos móviles, asegúrate de que la aplicación puede recuperarse si la información no es válida en el dispositivo secundario.
-   No incluyas como datos móviles grandes conjuntos de datos de la aplicación. La cantidad de datos de la aplicación que se pueden definir como datos de itinerancia es limitada. Usa la propiedad [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) para obtener la cantidad máxima. Si una aplicación alcanza este límite, no se podrán transferir datos móviles hasta que el tamaño del almacén de datos de la aplicación deje de superar el límite. Al diseñar la aplicación, busca la forma de restringir los datos de mayor tamaño de manera que no se exceda el límite. Por ejemplo, si se necesitan 10 KB para guardar cada estado de un juego, la aplicación podría permitir que el usuario almacene solo 10 juegos.
-   No definas datos que usan sincronización instantánea como datos móviles. Windows no garantiza una sincronización instantánea; la itinerancia podría verse demorada de forma significativa si el usuario no dispone de conexión o está conectado a una red con una latencia elevada. Asegúrate de que tu UI no depende de la sincronización instantánea.
-   No uses la itinerancia de datos que cambian con frecuencia. Por ejemplo, si tu aplicación realiza el seguimiento de información que cambia con frecuencia, como la posición en segundos de una canción, no almacenes esto como datos móviles de la aplicación. En su lugar, elige una representación menos frecuente que siga proporcionando al usuario una buena experiencia, como la canción que se está reproduciendo en ese momento.

### <a name="roaming-pre-requisites"></a>Requisitos previos para el uso de los datos móviles

Todos los usuarios pueden beneficiarse de los datos de las aplicaciones de itinerancia si usan una cuenta Microsoft para iniciar sesión en su dispositivo. Sin embargo, tanto los usuarios como los administradores de directivas de grupo pueden desactivar los datos móviles de aplicaciones de un dispositivo en cualquier momento. Si un usuario decide no usar una cuenta Microsoft o deshabilita la funcionalidad de datos de itinerancia, seguirá pudiendo usar la aplicación, pero los datos de la aplicación serán locales para cada dispositivo.

Los datos almacenados en [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) solo se transferirán si un usuario ha marcado el dispositivo como "de confianza". Si un dispositivo no es de confianza, los datos guardados en este almacén no se transferirán.

### <a name="conflict-resolution"></a>Resolución de conflictos

Los datos móviles de una aplicación no están diseñados para ser usados en más de un dispositivo al mismo tiempo. Si surge un conflicto durante la sincronización porque se modificó algún dato en los dos dispositivos, el sistema siempre dará validez al último valor introducido. De este modo se garantiza que la aplicación usará la información más actualizada. Si la unidad de datos es una opción de configuración compuesta, la resolución de conflictos se seguirá realizando en el nivel de la opción de configuración unitaria, lo que significa que se realizará la sincronización del compuesto con el cambio más reciente.

### <a name="when-to-write-data"></a>Cuándo escribir los datos

En función de la vida útil prevista de la opción de configuración, los datos deben escribirse en diferentes momentos. Los datos de la aplicación que cambian con poca frecuencia o lentamente deben escribirse inmediatamente. Sin embargo, los datos de la aplicación que cambian con frecuencia solo deben escribirse periódicamente a intervalos regulares (por ejemplo, cada cinco minutos), y cuando se suspende la aplicación. Por ejemplo, una aplicación de música podría escribir la opción de configuración "canción actual" siempre que se empiece a reproducir una canción nueva; sin embargo, la posición real de la canción solo debe escribirse durante el estado de suspensión.

### <a name="excessive-usage-protection"></a>Protección contra uso excesivo

El sistema dispone de varios mecanismos de protección para evitar el uso inapropiado de los recursos. Si los datos de la aplicación no se transfieren como es de esperar, es probable que el dispositivo esté temporalmente restringido. Normalmente, esperar un tiempo resuelve esta situación automáticamente y no es necesario realizar ninguna acción.

### <a name="versioning"></a>Control de versiones

Los datos de la aplicación pueden usar el control de versiones para actualizar de una estructura de datos a otra. El número de versión es diferente de la versión de la aplicación y se puede establecer a voluntad. Aunque no es obligatorio, es muy recomendable usar solo números de versión incrementales, porque se podría producir una situación no deseada (incluida la pérdida de datos) al transferir a un número de versión de los datos inferior que represente datos más recientes.

Los datos de la aplicación solo se transfieren entre aplicaciones que tengan el mismo número de versión. Por ejemplo, los dispositivos con la versión2 transferirán los datos entre sí, y los dispositivos con la versión3 harán lo mismo, pero no se realizará una transferencia entre los dispositivos de la versión2 y los de la versión3. Si instalas una aplicación que usó varios números de versión en otros dispositivos, la aplicación instalada recientemente sincronizará los datos de la aplicación asociados al número de versión más alto.

### <a name="testing-and-tools"></a>Pruebas y herramientas

Los desarrolladores pueden bloquear su dispositivo para desencadenar una sincronización del perfil móvil de datos de la aplicación. Si parece que los datos de la aplicación no se transfieren en un determinado plazo de tiempo, comprueba los siguientes elementos y asegúrate de lo siguiente:

-   Los datos de itinerancia no superan el tamaño máximo (consulta [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) para obtener más información).
-   Tus archivos están cerrados y correctamente publicados.
-   Hay al menos dos dispositivos que están ejecutando la misma versión de la aplicación.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>Registrarse para recibir notificaciones cuando cambian los datos de itinerancia

Para usar datos móviles de aplicaciones, debes registrarte para los cambios de datos móviles y recuperar los contenedores de datos móviles, para que puedas leer y crear configuración.

1.  Regístrate para recibir notificaciones cuando cambian los datos de itinerancia.

    El evento [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) te notifica cuándo cambian los datos de itinerancia. En este ejemplo se establece `DataChangeHandler` como el controlador para cambios de datos de itinerancia.

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  Obtén los contenedores para los archivos y la configuración de la aplicación.

    Usa la propiedad [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) para obtener la configuración y la propiedad [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) para obtener los archivos.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Crear y recuperar la configuración de itinerancia

Usa la propiedad [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) para obtener acceso a la configuración en el contenedor `roamingSettings` que obtuvimos en la sección anterior. En este ejemplo se crea una configuración sencilla denominada `exampleSetting` y un valor compuesto llamado `composite`.

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

En este ejemplo se recupera la configuración que acabamos de crear.

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>Crear y recuperar archivos móviles

Para crear y actualizar un archivo en el almacén de datos de itinerancia de la aplicación, usa las API de archivo, como [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) y [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). En este ejemplo se crea un archivo llamado `dataFile.txt` en el contenedor `roamingFolder` y se escriben la fecha y la hora actuales en el archivo. El valor **ReplaceExisting** de la enumeración [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indica que se reemplace el archivo si ya existe.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Para abrir y leer un archivo en el almacén de datos de itinerancia de la aplicación, usa las API de archivo, como [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) y [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). En este ejemplo se abre el archivo `dataFile.txt` creado en la sección anterior y se lee la fecha que aparece en él. Para obtener información detallada sobre la carga de recursos de archivos de varias ubicaciones, consulta [Cómo cargar recursos de archivos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>Datos de aplicaciones temporales


El almacén de datos de aplicaciones temporales funciona como una memoria caché. Sus archivos no son móviles y se pueden quitar en cualquier momento. La tarea Mantenimiento del sistema puede eliminar automáticamente los datos almacenados en esta ubicación en cualquier momento. El usuario también puede borrar archivos del almacén de datos temporales con el Liberador de espacio en disco. Los datos de aplicaciones temporales se pueden usar para almacenar información temporal durante una sesión de la aplicación. No hay garantía de que estos datos persistan más allá del final de una sesión de la aplicación, porque el sistema podría reclamar el espacio usado si fuera necesario. La ubicación está disponible mediante la propiedad [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629).

### <a name="retrieve-the-temporary-data-container"></a>Recuperar el contenedor de datos temporales

Usa la propiedad [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) para obtener los archivos. En los siguientes pasos se usa la variable `temporaryFolder` de este paso.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Crear y leer archivos temporales

Para crear y actualizar un archivo en el almacén de datos temporales de la aplicación, usa las API de archivo, como [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) y [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). En este ejemplo se crea un archivo llamado `dataFile.txt` en el contenedor `temporaryFolder` y se escriben la fecha y la hora actuales en el archivo. El valor **ReplaceExisting** de la enumeración [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indica que se reemplace el archivo si ya existe.


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Para abrir y leer un archivo en el almacén de datos temporales de la aplicación, usa las API de archivo, como [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) y [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). En este ejemplo se abre el archivo `dataFile.txt` creado en el paso anterior y se lee la fecha que aparece en él. Para obtener información detallada sobre la carga de recursos de archivos de varias ubicaciones, consulta [Cómo cargar recursos de archivos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>Organizar los datos de aplicación con contenedores


Para ayudar a organizar tus archivos y configuración de datos de la aplicación, crea contenedores (representados por objetos [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599)) en lugar de trabajar directamente con directorios. Puedes agregar contenedores a los almacenes de datos de aplicaciones locales, de itinerancia y temporales. Los contenedores se pueden anidar hasta en 32 niveles de profundidad.

Para crear un contenedor de configuraciones, llama al método [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611). En este ejemplo se crea un contenedor de configuraciones local denominado `exampleContainer` y se agrega una configuración llamada `exampleSetting`. El valor **Siempre** de la enumeración [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616) indica que el contenedor se crea si no existe ya.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>Eliminar los contenedores y la configuración de aplicación


Para eliminar una configuración sencilla que tu aplicación ya no necesita, usa el método [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608). En este ejemplo se elimina la configuración local `exampleSetting` que hemos creado anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Para eliminar una configuración compuesta, usa el método [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597). En este ejemplo se elimina la configuración compuesta local `exampleCompositeSetting` que creamos en el ejemplo anterior.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Para eliminar un contenedor, llama al método [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612). Este ejemplo elimina el contenedor de configuraciones `exampleContainer` local que creamos anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Control de versiones de los datos de aplicación


Opcionalmente, puedes crear versiones de los datos de aplicación para la aplicación. Esto te permitiría crear una versión futura de la aplicación que cambie el formato de sus datos de aplicación sin provocar problemas de compatibilidad con la versión anterior de la aplicación. La aplicación comprueba la versión de los datos de la aplicación en el almacén de datos y, si la versión es anterior a la que la aplicación espera, la aplicación debe actualizar los datos de la aplicación al nuevo formato y actualizar la versión. Para más información, consulta la propiedad [**Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630) y el método [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429).

## <a name="related-articles"></a>Artículos relacionados

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)
