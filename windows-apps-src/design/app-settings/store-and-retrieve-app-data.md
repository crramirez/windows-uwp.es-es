---
Description: Obtenga información sobre cómo almacenar y recuperar datos de aplicaciones locales, móviles y temporales.
title: Almacenar y recuperar la configuración y otros datos de la aplicación
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690367"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Almacenar y recuperar la configuración y otros datos de la aplicación

Los datos de la *aplicación* son datos mutables creados y administrados por una aplicación específica. Incluye el estado de tiempo de ejecución, la configuración de la aplicación, las preferencias del usuario, el contenido de referencia (como las definiciones de diccionario en una aplicación de diccionario) y otros valores de configuración. Los datos de la aplicación son diferentes de los *datos del usuario*, los datos que el usuario crea y administra cuando se usa una aplicación. Los datos de usuario incluyen archivos de documento o multimedia, transcripciones de comunicaciones o correo electrónico, o registros de bases de datos que contienen contenido creado por el usuario. Los datos de usuario pueden ser útiles o significativos para más de una aplicación. A menudo, se trata de datos que el usuario desea manipular o transmitir como entidad independiente de la propia aplicación, como un documento.

**Nota importante acerca de los datos de la aplicación:** La duración de los datos de la aplicación está ligada a la duración de la aplicación. Si se quita la aplicación, todos los datos de la aplicación se perderán como consecuencia. No use los datos de la aplicación para almacenar los datos de usuario o cualquier cosa que los usuarios puedan percibir como valiosos e irremplazables. Se recomienda usar las bibliotecas del usuario y Microsoft OneDrive para almacenar este tipo de información. Los datos de la aplicación son ideales para almacenar las preferencias de usuario, la configuración y los favoritos específicos de la aplicación.

## <a name="types-of-app-data"></a>Tipos de datos de la aplicación

Hay dos tipos de datos de la aplicación: configuración y archivos.

### <a name="settings"></a>Configuración

Use la configuración para almacenar las preferencias de usuario y la información de estado de la aplicación. La API de datos de la aplicación le permite crear y recuperar fácilmente la configuración (le mostraremos algunos ejemplos más adelante en este artículo).

Estos son los tipos de datos que puede usar para la configuración de la aplicación:

- **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
- **Boolean**
- **Char16**, **cadena**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime), [ **TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - Para C#/.net, use: [**System. DateTimeOffset**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0), [**System. TimeSpan.** ](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)
- **GUID**, [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**size**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size), [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**ApplicationDataCompositeValue**](/uwp/api/Windows.Storage.ApplicationDataCompositeValue): un conjunto de opciones de configuración de aplicaciones relacionadas que se deben serializar y deserializar de forma atómica. Use la configuración compuesta para controlar fácilmente las actualizaciones atómicas de la configuración interdependiente. El sistema garantiza la integridad de la configuración compuesta durante el acceso simultáneo y la itinerancia. La configuración compuesta está optimizada para pequeñas cantidades de datos y el rendimiento puede ser deficiente si se usan para conjuntos de datos grandes.

### <a name="files"></a>Archivos

Use archivos para almacenar datos binarios o para habilitar sus propios tipos serializados personalizados.

## <a name="storing-app-data-in-the-app-data-stores"></a>Almacenar datos de aplicaciones en los almacenes de datos de la aplicación


Cuando se instala una aplicación, el sistema le proporciona sus propios almacenes de datos por usuario para la configuración y los archivos. No es necesario saber dónde se encuentran los datos, porque el sistema es responsable de administrar el almacenamiento físico, lo que garantiza que los datos se mantienen aislados de otras aplicaciones y de otros usuarios. El sistema también conserva el contenido de estos almacenes de datos cuando el usuario instala una actualización de la aplicación y quita el contenido de estos almacenes de datos por completo y limpiamente cuando se desinstala la aplicación.

Dentro de su almacén de datos de aplicaciones, cada aplicación tiene directorios raíz definidos por el sistema: uno para los archivos locales, uno para los archivos móviles y otro para los archivos temporales. La aplicación puede agregar nuevos archivos y nuevos contenedores a cada uno de estos directorios raíz.

## <a name="local-app-data"></a>Datos de la aplicación local


Los datos de la aplicación local deben usarse para cualquier información que deba conservarse entre sesiones de aplicación y no es adecuada para los datos de aplicaciones móviles. Los datos que no son aplicables en otros dispositivos también se deben almacenar aquí. No hay ninguna restricción de tamaño general en los datos locales almacenados. Use el almacén de datos de la aplicación local para los datos que no tienen sentido para el itinerancia y para conjuntos de datos grandes.

### <a name="retrieve-the-local-app-data-store"></a>Recuperación del almacén de datos de la aplicación local

Para poder leer o escribir datos de la aplicación local, debe recuperar el almacén de datos de la aplicación local. Para recuperar el almacén de datos de la aplicación local, use la propiedad [**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) para obtener la configuración local de la aplicación como un objeto [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) . Use la propiedad [**ApplicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) para obtener los archivos de un objeto [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) . Use la propiedad [**ApplicationData. LocalCacheFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder) para obtener la carpeta en el almacén de datos de la aplicación local, donde puede guardar los archivos que no están incluidos en la copia de seguridad y la restauración.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Crear y recuperar una configuración local simple

Para crear o escribir un valor de configuración, use la propiedad [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) para tener acceso a la configuración del contenedor de `localSettings` que obtuvimos en el paso anterior. En este ejemplo se crea un valor denominado `exampleSetting`.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Para recuperar la configuración, se usa la misma propiedad [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) que se usó para crear la configuración. En este ejemplo se muestra cómo recuperar la configuración que acabamos de crear.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Crear y recuperar un valor compuesto local

Para crear o escribir un valor compuesto, cree un objeto [**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) . En este ejemplo se crea un valor compuesto denominado `exampleCompositeSetting` y se agrega al contenedor `localSettings`.

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

Para crear y actualizar un archivo en el almacén de datos de la aplicación local, use las API de archivo, como [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) y [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). En este ejemplo se crea un archivo denominado `dataFile.txt` en el contenedor `localFolder` y se escribe la fecha y hora actuales en el archivo. El valor **ReplaceExisting** de la enumeración [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indica que debe reemplazar el archivo si ya existe.

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

Para abrir y leer un archivo en el almacén de datos de la aplicación local, use las API de archivo, como [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)y [**Windows. Storage. FileIO. ReadTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). En este ejemplo se abre el archivo `dataFile.txt` creado en el paso anterior y se lee la fecha del archivo. Para obtener más información sobre cómo cargar recursos de archivo desde varias ubicaciones, consulte [How to Upload File Resources](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

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

## <a name="roaming-data"></a>Datos móviles


Si usa datos móviles en la aplicación, los usuarios pueden mantener sincronizados fácilmente los datos de la aplicación de la aplicación en varios dispositivos. Si un usuario instala la aplicación en varios dispositivos, el sistema operativo mantiene sincronizados los datos de la aplicación, lo que reduce la cantidad de trabajo de configuración que el usuario debe realizar para la aplicación en el segundo dispositivo. La itinerancia también permite que los usuarios continúen una tarea, como la creación de una lista, justo donde se dejaron incluso en un dispositivo diferente. El sistema operativo replica los datos móviles en la nube cuando se actualiza y sincroniza los datos con los demás dispositivos en los que está instalada la aplicación.

El sistema operativo limita el tamaño de los datos de la aplicación que cada aplicación puede desplazar. Consulte [**ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota). Si la aplicación alcanza este límite, ninguno de los datos de la aplicación de la aplicación se replicará en la nube hasta que el total de datos de la aplicación móvil de la aplicación sea inferior al límite. Por esta razón, se recomienda usar datos móviles solo para preferencias de usuario, vínculos y archivos de datos pequeños.

Los datos móviles para una aplicación están disponibles en la nube siempre que el usuario tenga acceso a ellos desde algún dispositivo dentro del intervalo de tiempo necesario. Si el usuario no ejecuta una aplicación durante más tiempo que este intervalo de tiempo, los datos móviles se quitan de la nube. Si un usuario desinstala una aplicación, los datos móviles no se quitan automáticamente de la nube, sino que se conservan. Si el usuario vuelve a instalar la aplicación en el intervalo de tiempo, los datos móviles se sincronizan desde la nube.

### <a name="roaming-data-dos-and-donts"></a>Los datos móviles y no lo hacen

- Usar itinerancia para preferencias de usuario y personalizaciones, vínculos y archivos de datos pequeños. Por ejemplo, use la itinerancia para conservar las preferencias de color de fondo de un usuario en todos los dispositivos.
- Use itinerancia para permitir que los usuarios continúen una tarea en todos los dispositivos. Por ejemplo, datos de aplicaciones móviles como el contenido de un correo electrónico con dibujo o la página que se ha visto más recientemente en una aplicación de lector.
- Controle el evento de [**cambio**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) de datos actualizando los datos de la aplicación. Este evento se produce cuando los datos de la aplicación acaban de sincronizarse desde la nube.
- Referencias de itinerancia a contenido en lugar de datos sin procesar. Por ejemplo, roaming una dirección URL en lugar del contenido de un artículo en línea.
- En cuanto a la configuración importante en el tiempo, use la configuración *HighPriority* asociada a [**RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings).
- No itinerancia datos de la aplicación que son específicos de un dispositivo. Parte de la información solo es pertinente localmente, como un nombre de ruta de acceso a un recurso de archivo local. Si decide mover la información local, asegúrese de que la aplicación puede recuperarse si la información no es válida en el dispositivo secundario.
- No se desplazan grandes conjuntos de datos de aplicaciones. Hay un límite en la cantidad de datos de aplicación que una aplicación puede desplazar. Use la propiedad [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) para obtener este valor máximo. Si una aplicación alcanza este límite, no se pueden mover datos hasta que el tamaño del almacén de datos de la aplicación ya no supera el límite. Al diseñar la aplicación, considere la posibilidad de colocar una enlazada a datos de mayor tamaño para que no supere el límite. Por ejemplo, si al guardar un estado de juego se requiere 10 KB cada uno, la aplicación podría permitir que el usuario almacene hasta 10 juegos.
- No utilice la itinerancia para los datos que se basan en la sincronización instantánea. Windows no garantiza una sincronización instantánea; la itinerancia podría retrasarse significativamente si un usuario está sin conexión o en una red de latencia alta. Asegúrese de que la interfaz de usuario no dependa de la sincronización instantánea.
- No use itinerancia para los datos que cambian con frecuencia. Por ejemplo, si la aplicación realiza un seguimiento de la información que cambia con frecuencia, como la posición en una canción por segundo, no la almacena como datos de aplicaciones móviles. En su lugar, elija una representación menos frecuente que aún proporcione una buena experiencia del usuario, como la canción que se está reproduciendo.

### <a name="roaming-pre-requisites"></a>Requisitos previos de itinerancia

Cualquier usuario puede beneficiarse de los datos de aplicaciones móviles si usa un cuenta de Microsoft para iniciar sesión en su dispositivo. Sin embargo, los usuarios y los administradores de directivas de grupo pueden desactivar los datos de aplicaciones móviles en un dispositivo en cualquier momento. Si un usuario decide no usar una cuenta de Microsoft o deshabilita las capacidades de datos móviles, podrá seguir usando la aplicación, pero los datos de la aplicación serán locales para cada dispositivo.

Los datos almacenados en [**PasswordVault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault) solo realizarán la transición si un usuario ha realizado un dispositivo de "confianza". Si un dispositivo no es de confianza, los datos protegidos en este almacén no serán móviles.

### <a name="conflict-resolution"></a>Resolución de conflictos

Los datos móviles de la aplicación no están diseñados para usarse de forma simultánea en más de un dispositivo a la vez. Si surge un conflicto durante la sincronización porque se cambió una unidad de datos determinada en dos dispositivos, el sistema siempre favorecerá el valor que se escribió en último lugar. Esto garantiza que la aplicación use la información más actualizada. Si la unidad de datos es una configuración compuesta, la resolución de conflictos se seguirá produciendo en el nivel de la unidad de configuración, lo que significa que se sincronizará el compuesto con el cambio más reciente.

### <a name="when-to-write-data"></a>Cuándo se deben escribir los datos

En función de la duración esperada de la configuración, los datos deben escribirse en momentos diferentes. Los datos de la aplicación que cambian con poca frecuencia o lentamente deben escribirse inmediatamente. Sin embargo, los datos de la aplicación que cambian con frecuencia solo se deben escribir periódicamente a intervalos regulares (por ejemplo, una vez cada 5 minutos), así como cuando la aplicación se suspende. Por ejemplo, una aplicación de música puede escribir la configuración de "canción actual" cada vez que se inicia la reproducción de una nueva canción; sin embargo, la posición real de la canción solo debe escribirse en suspensión.

### <a name="excessive-usage-protection"></a>Protección de uso excesiva

El sistema tiene varios mecanismos de protección para evitar el uso inadecuado de los recursos. Si los datos de la aplicación no se transforman según lo esperado, es probable que el dispositivo se haya restringido temporalmente. Esperar algún tiempo normalmente resolverá esta situación automáticamente y no se requiere ninguna acción.

### <a name="versioning"></a>Control de versiones

Los datos de la aplicación pueden emplear el control de versiones para actualizar de una estructura de datos a otra. El número de versión es diferente de la versión de la aplicación y se puede establecer en. Aunque no se exige, se recomienda encarecidamente que use números de versión crecientes, ya que las complicaciones no deseadas (incluida la pérdida de datos) podrían producirse si intenta realizar la transición a un número de versión de datos más bajo que representa los datos más recientes.

Los datos de la aplicación solo se mueven entre las aplicaciones instaladas con el mismo número de versión. Por ejemplo, los dispositivos de la versión 2 cambiarán los datos entre sí y los dispositivos de la versión 3 realizarán la misma, pero no se producirá la itinerancia entre un dispositivo que ejecute la versión 2 y un dispositivo que ejecute la versión 3. Si instala una nueva aplicación que utiliza varios números de versión en otros dispositivos, la aplicación recién instalada sincronizará los datos de la aplicación asociados con el número de versión más alto.

### <a name="testing-and-tools"></a>Pruebas y herramientas

Los desarrolladores pueden bloquear su dispositivo para desencadenar una sincronización de datos de aplicaciones móviles. Si parece que los datos de la aplicación no se transfieran dentro de un período de tiempo determinado, compruebe los siguientes elementos y asegúrese de que:

- Los datos móviles no superan el tamaño máximo (consulte [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) para obtener más información).
- Los archivos se cierran y se liberan correctamente.
- Hay al menos dos dispositivos que ejecutan la misma versión de la aplicación.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>Registrarse para recibir notificaciones cuando cambien los datos móviles

Para usar datos de aplicaciones móviles, debe registrarse para los cambios de datos móviles y recuperar los contenedores de datos móviles para que pueda leer y escribir la configuración.

1.  Regístrese para recibir una notificación cuando cambien los datos móviles.

    El evento de [**cambio**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) de datos le informa cuando cambian los datos móviles. En este ejemplo se establece `DataChangeHandler` como controlador para los cambios de datos móviles.

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

2.  Obtenga los contenedores de los archivos y la configuración de la aplicación.

    Use la propiedad [**applicationdata. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) para obtener la configuración y la propiedad [**applicationdata. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) para obtener los archivos.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Crear y recuperar la configuración de itinerancia

Use la propiedad [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) para tener acceso a la configuración del contenedor de `roamingSettings` que obtuvimos en la sección anterior. En este ejemplo se crea una configuración simple denominada `exampleSetting` y un valor compuesto denominado `composite`.

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

Para crear y actualizar un archivo en el almacén de datos de la aplicación móvil, use las API de archivo, como [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) y [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). En este ejemplo se crea un archivo denominado `dataFile.txt` en el contenedor `roamingFolder` y se escribe la fecha y hora actuales en el archivo. El valor **ReplaceExisting** de la enumeración [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indica que debe reemplazar el archivo si ya existe.

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

Para abrir y leer un archivo en el almacén de datos de la aplicación móvil, use las API de archivo, como [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)y [ **Windows. Storage. FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). En este ejemplo se abre el archivo `dataFile.txt` creado en la sección anterior y se lee la fecha del archivo. Para obtener más información sobre cómo cargar recursos de archivo desde varias ubicaciones, consulte [How to Upload File Resources](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

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


## <a name="temporary-app-data"></a>Datos de aplicación temporales


El almacén de datos de la aplicación temporal funciona como una caché. Sus archivos no se desplazan y se pueden quitar en cualquier momento. La tarea de mantenimiento del sistema puede eliminar automáticamente los datos almacenados en esta ubicación en cualquier momento. El usuario también puede borrar archivos del almacén de datos temporal mediante el liberador de espacio en disco. Los datos temporales de la aplicación se pueden usar para almacenar información temporal durante una sesión de la aplicación. No hay ninguna garantía de que estos datos se conserven más allá del final de la sesión de la aplicación, ya que el sistema puede recuperar el espacio usado si es necesario. La ubicación está disponible a través de la propiedad [**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) .

### <a name="retrieve-the-temporary-data-container"></a>Recuperar el contenedor de datos temporal

Use la propiedad [**ApplicationData. TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) para obtener los archivos. En los pasos siguientes se usa la variable `temporaryFolder` de este paso.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Crear y leer archivos temporales

Para crear y actualizar un archivo en el almacén de datos de la aplicación temporal, use las API de archivo, como [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) y [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). En este ejemplo se crea un archivo denominado `dataFile.txt` en el contenedor `temporaryFolder` y se escribe la fecha y hora actuales en el archivo. El valor **ReplaceExisting** de la enumeración [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indica que debe reemplazar el archivo si ya existe.


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

Para abrir y leer un archivo en el almacén de datos de la aplicación temporal, use las API de archivo, como [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)y [ **Windows. Storage. FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). En este ejemplo se abre el archivo `dataFile.txt` creado en el paso anterior y se lee la fecha del archivo. Para obtener más información sobre cómo cargar recursos de archivo desde varias ubicaciones, consulte [How to Upload File Resources](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

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

## <a name="organize-app-data-with-containers"></a>Organizar los datos de la aplicación con contenedores


Para ayudarle a organizar los archivos y la configuración de los datos de la aplicación, cree contenedores (representados por objetos [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) ) en lugar de trabajar directamente con directorios. Puede agregar contenedores a los almacenes de datos de la aplicación local, móvil y temporal. Los contenedores se pueden anidar hasta 32 niveles de profundidad.

Para crear un contenedor de configuración, llame al método [**ApplicationDataContainer. CreateContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer) . En este ejemplo se crea un contenedor de configuración local denominado `exampleContainer` y se agrega una configuración denominada `exampleSetting`. El valor **Always** de la enumeración [**ApplicationDataCreateDisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition) indica que se crea el contenedor si aún no existe.

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

## <a name="delete-app-settings-and-containers"></a>Eliminar la configuración de la aplicación y los contenedores


Para eliminar una configuración simple que la aplicación ya no necesita, use el método [**ApplicationDataContainerSettings. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove) . En este ejemplo se deletesthe `exampleSetting` configuración local que se ha creado anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Para eliminar una configuración compuesta, use el método [**ApplicationDataCompositeValue. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove) . En este ejemplo se elimina la configuración compuesta de `exampleCompositeSetting` local creada en un ejemplo anterior.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Para eliminar un contenedor, llame al método [**ApplicationDataContainer. DeleteContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer) . En este ejemplo se elimina el contenedor de configuración de `exampleContainer` local que hemos creado anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Control de versiones de los datos de la aplicación


Opcionalmente, puede hacer una versión de los datos de la aplicación para la aplicación. Esto le permite crear una versión futura de la aplicación que cambia el formato de los datos de la aplicación sin causar problemas de compatibilidad con la versión anterior de la aplicación. La aplicación comprueba la versión de los datos de la aplicación en el almacén de datos y, si la versión es inferior a la versión que espera la aplicación, la aplicación debe actualizar los datos de la aplicación al nuevo formato y actualizar la versión. Para obtener más información, vea la propiedad[**Application. version**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version) y el método [**ApplicationData. SetVersionAsync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync) .

## <a name="related-articles"></a>Artículos relacionados

* [**Windows. Storage. ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**Windows. Storage. ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**Windows. Storage. ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**Windows. Storage. ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**Windows. Storage. ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)
