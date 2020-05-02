---
title: Uso de propiedades complementarias
description: Introducción al uso de propiedades complementarias y detalles sobre cómo se implementan en Windows
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, API WinRT, Indexador, Búsqueda
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66369263"
---
# <a name="using-supplemental-properties"></a>Uso de propiedades complementarias  

## <a name="summary"></a>Resumen  
- Las propiedades complementarias permiten a las aplicaciones etiquetar los archivos con propiedades, sin cambiar el archivo. 
- Son útiles para los casos donde las propiedades son difíciles de calcular o el archivo no se puede modificar. 
- Usar propiedades complementarias es igual a usar cualquier otra propiedad del sistema de propiedades de Windows  

## <a name="introduction"></a>Introducción 
Muchas de las nuevas e interesantes aplicaciones de los últimos años requieren la ejecución de operaciones intensivas de CPU en archivos de usuario para extraer propiedades útiles de los archivos, además de los aspectos básicos como la fecha de creación. Estas aplicaciones reconocen objetos en imágenes, extraen la intención de los correos electrónicos y analizan texto para agrupar los documentos. Esto está motivado por la potencia de la informática que está disponible en la mayoría de los equipos de los consumidores.   

Hacer que los metadatos sean aptos para la búsqueda al instante permite a los usuarios ser exponencialmente más productivos. Saber que tu hija se encuentra en una imagen es interesante, pero poder buscar una imagen de ella con su abuela es mucho más útil. Hace que la experiencia de uso de un ordenador sea más personal y viva. Como si alguien en la máquina te ayudara a encontrar tus preciados recuerdos. 

Durante décadas, la solución de búsqueda rápida en Windows ha sido el indexador y en Creators Update se ha actualizado para admitir estos nuevos escenarios. Las aplicaciones son capaces de etiquetar archivos con otras propiedades, además de las que se extraen por el sistema. Estas propiedades se tratan como ciudadanos de primera clase.  

## <a name="windows-properties"></a>Propiedades de Windows 
El [sistema de propiedades de Windows](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) ha sido una pieza clave en la interacción con los archivos durante años. Permite que las aplicaciones lean propiedades de archivos sin tener que entender el funcionamiento interno de los diferentes formatos de archivo o los lenguajes en los que puede estar. Todo esto se ha abstraído para el desarrollador, lo único que tienes que hacer es solicitar una lista y especificar el orden ascendente o descendente.  

El sistema de propiedades está interrelacionado con el indexador de Windows: lee todas las propiedades de los archivos dentro de su ámbito y las almacena. Más adelante, cuando una aplicación solicita que una lista de todos los .docx de una carpeta se ordenen por fecha de modificación, exceptuando aquellos creados por John Smith, el indexador puede devolver la lista al instante.  

La desventaja del funcionamiento conjunto de estos sistemas es que el indexador solicitaba todas las propiedades que almacenaba en un archivo para que estuviesen disponibles al instante. Esto lo limitaba ya que no conocía propiedades más interesantes que tardan más tiempo en calcularse por un requisito difícil.  

Usar propiedades es fácil, la aplicación puede solicitar un conjunto ordenado de propiedades sobre un archivo, muy similar a trabajar con una base de datos, o puede pasar una consulta como al usar un motor de búsqueda. El indexador procesará la consulta y devolverá los resultados. Esto dará a los desarrolladores la flexibilidad necesaria para agrupar sus filtros (por ejemplo, buscar solo archivos jpg) con la consulta del usuario (nombre de archivo que empieza por "ave"). 

## <a name="supplemental-properties"></a>Propiedades complementarias 

Las propiedades complementarias se comportan igual que las propiedades normales de Windows, con una diferencia muy importante: no se escriben cuando el archivo se agrega al indexador. Las propiedades complementarias se deben agregar más adelante por otra aplicación del sistema. Podría ser dos minutos después de que se complete el reconocimiento de objetos o podría ser días más tarde. 

Una vez que se escribe la propiedad, se puede buscar, filtrar, ordenar o agrupar igual que cualquier otra propiedad del sistema. También se puede usar en consultas combinadas con otras propiedades del sistema, ya sean complementarias o no. De este modo, se ofrece flexibilidad para agrupar fácilmente propiedades complementarias con el código existente del sistema de archivos, sin tener que volver a escribir.  

### <a name="example-scenarios"></a>Escenarios de ejemplo 

Hay miles de propiedades diferentes que podrías escribir para una propiedad complementaria, pero hay un par de escenarios clave a los que este tutorial suele regresar:  

#### <a name="tagging-pictures-with-extracted-properties"></a>Etiquetado de imágenes con propiedades extraídas 
Estas aplicaciones pueden usar un modelo entrenado de ML para extraer características de una imagen que el sistema no conoce, como un objeto de la imagen. A continuación, puede tomar los objetos que identifica en la imagen y agregarlos al sistema de propiedades para la búsqueda o agrupación posteriores.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Etiquetado de archivos con un id. específico de la aplicación 
Muchas de las aplicaciones de sincronización de archivos usan su propio id. único para realizar un seguimiento de los archivos cuando se mueven entre el servidor y varios dispositivos cliente. El cliente de sincronización puede escribir este id. en el sistema de propiedades sin afectar al archivo. Este id. estará disponible para la aplicación para el acceso rápido y para que cualquier otra aplicación del sistema pueda leerlo al comunicarse con el proveedor de sincronización. 

Hay muchas otras opciones para usar propiedades complementarias, pero estas son dos buenos ejemplos porque requieren búsqueda o búsqueda rápida, son fragmentos de información que el sistema no conoce y no pueden agregarse al propio archivo.  

### <a name="using-supplemental-properties"></a>Uso de propiedades complementarias 
Usar las propiedades complementarias es igual a escribir una propiedad normal en el sistema de archivos. Si estás familiarizado con el uso del objeto StorageFiles y sus propiedades, puedes omitir esto. En caso contrario, veamos un ejemplo rápido de escribir una sola propiedad en un archivo y leer la misma propiedad más tarde.  

### <a name="writing-supplemental-properties"></a>Escribir propiedades complementarias  
En el ejemplo, solo se modificará el primer archivo que se encuentre por motivos de simplicidad, pero por lo general, una aplicación agregará la propiedad a todos los archivos que encuentre.  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

Se efectúa una comprobación importante de si la ubicación está indexada antes de escribir una propiedad. En este ejemplo, se usan las opciones de consulta para filtrar y obtener solo las ubicaciones indexadas. Si esto no es factible, puedes comprobar el estado indexado de la carpeta principal (file.GetParentAsync().GetIndexedStateAsync()). En cualquier caso producirá los mismos resultados. 

### <a name="reading-supplemental-properties"></a>Leer propiedades complementarias 
De nuevo, leer una propiedad complementaria es lo mismo que leer cualquier otra propiedad del sistema de archivos. En este ejemplo, la aplicación simplemente leerá una propiedad de un archivo para la que ya tiene un objeto StorageFile, pero también podría leer otras propiedades al mismo tiempo.  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
Se realiza una comprobación para asegurarse de que el valor procedente del sistema de propiedades es lo que espera. Aunque es poco probable, es posible que se haya borrado el valor desde que la aplicación la ha escrito. Esto se explicará en detalle a continuación.  

### <a name="implementation-notes"></a>Notas de la implementación 
Se realizaron unas cuantas opciones sutiles en el diseño de las propiedades complementarias. Para ayudar con la implementación, las siguientes secciones se copiaron de la especificación de diseño de ingeniería de la característica. Son un vistazo de cómo se diseñó la característica y por qué existen algunas limitaciones. 

### <a name="supplemental-properties-available"></a>Propiedades complementarias disponibles 
Inicialmente, solo hay dos propiedades disponibles para su uso en aplicaciones: System.Supplemental.ResourceId y System.Supplemental.AlbumID. Si son necesarias más, se pueden agregar. El id. de álbum es una cadena de varios valores que puede usarse para muchas aplicaciones diferentes y el valor de ResourceId se utiliza como un id. único para proveedores de sincronización en la nube. 

#### <a name="file-system-support"></a>Compatibilidad del sistema de archivos 
Puesto que los medios extraíbles con formato FAT son un escenario importante, las propiedades complementarias serán compatibles con FAT y las unidades NTFS. Esto garantizará que las propiedades complementarias vayan a estar disponibles para todos los usuarios, independientemente de su tipo de dispositivo.   

### <a name="non-indexed-locations"></a>Ubicaciones no indexadas  
En el escritorio, hay varias carpetas que no están indexadas. En estos casos, también puede haber aplicaciones que quieran tener acceso a propiedades complementarias. Sin embargo, las propiedades complementarias no están disponibles fuera de ubicaciones indexadas. Esta solución intermedia se adoptó por un par de motivos:  

- De manera predeterminada, se indexan todas las bibliotecas y las ubicaciones de almacenamiento en la nube.   
  Estas son las ubicaciones que las aplicaciones para UWP usarán principalmente. Hay otras ubicaciones que no están indexadas (unidades de red o del sistema), pero se usan con menos frecuencia para almacenar datos de usuario. 

- En el diseño de la superficie de la API de WinRT se da por supuesto que el indexador casi siempre está disponible.  
  Por lo tanto, el indexador ya está disponible en la mayoría de las ubicaciones en las que están interesadas las aplicaciones. Si se detecta que los usuarios almacenan datos en ubicaciones no indexadas, la solución más fácil será agregar esa ubicación al índice. Las propiedades complementarias funcionarán, la enumeración será más rápida y las aplicaciones podrán realizar un seguimiento de los cambios de la ubicación.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Leer o escribir propiedades complementarias de un archivo en una ubicación no indexada 
En el caso de que una aplicación intente escribir una propiedad complementaria en una ubicación que no está indexada actualmente, la llamada a la API iniciará una excepción. Se trata de la misma excepción que se inicia cuando alguien intenta actualizar System.Music.AlbumArtist en un archivo .docx (argumentos no válidos).  
 
### <a name="change-notifications"></a>Notificaciones de cambios:  
Las notificaciones de cambios de UWP y el seguimiento de cambios seguirán funcionando para las propiedades complementarias, como lo hacen para las propiedades estándar. Esto permitirá que las aplicaciones que proporcionan datos realicen un seguimiento de todos los cambios que se producen en una de sus aplicaciones. 
  
### <a name="invalidating-properties"></a>Invalidación de propiedades:  
Las propiedades complementarias de un archivo pueden quedarse obsoletas cuando se modifica un archivo o se mueve en el sistema. Las aplicaciones que insertan los datos serán las que dispongan de la información de si los datos son válidos o deben actualizarse, por lo que el sistema solo proporcionará las herramientas para que puedan averiguarlo.  
 
En el caso de que se modifique un archivo, pero no se mueva ni cambie de nombre, todas las propiedades complementarias del archivo permanecerán sin cambios. Las aplicaciones podrán registrarse para recibir notificaciones de cambio a través de la superficie de API existente y actualizar las propiedades según sea necesario. 
 
Si se mueve el archivo, se invalidarán las propiedades. La aplicación recibirá las notificaciones de cambio de eliminación, creación, cambio de nombre o movimiento en función de cómo se realice exactamente la operación. Una vez que la aplicación haya recibido la notificación de cambio podrá inspeccionar el archivo y actualizar las propiedades complementarias en el archivo según sea necesario. 
 
### <a name="indexer-rebuilds"></a>Recompilaciones del indexador  
En ocasiones, el índice del sistema se tiene que volver a compilar por algún motivo: podría cambiar el esquema de propiedades, el usuario podría habilitar EDP o simplemente el archivo de base de datos podría estar dañado. En estos casos, no se conservarán las propiedades complementarias. Consideramos intentar conservar las propiedades complementarias cuando se recompila el índice, pero surgieron un par de obstáculos importantes:  

### <a name="protecting-the-data"></a>Protección de datos 
En el caso en el que el archivo de base de datos esté dañado, ya sea por errores de disco o software malintencionado, va a ser imposible proteger los datos que se almacenaron en ese archivo. Debería estar almacenado en otro lugar del sistema o aislado de algún modo del resto de la base de datos. 

De todas formas, como ya estamos realizando mucho trabajo para que sea menos probable que se dañe el índice, se reducirá la tasa de incidencia de este caso.  
Mantener la asignación entre los archivos y sus metadatos durante las recompilaciones 

Incluso si el índice puede proteger los datos a través de una recompilación, es imposible saber si el archivo ha cambiado mientras se volvía a compilar el índice. Los datos del archivo que protege el índice podrían ya no ser válidos si se modificó o se movió el archivo.  
Comportamiento 

En el caso de una recompilación del indexador, se perderán todos los datos complementarios. Las aplicaciones serán responsables de poner los datos que se perdieron durante la regeneración de nuevo en el indexador. Esto representa una carga adicional para las aplicaciones, pero se considera razonable, ya que siempre retendrán el estado maestro para todos sus datos.  

### <a name="recovering"></a>Recuperación 
Una vez que las aplicaciones sepan que se recompila el índice, serán responsables de actualizar las propiedades complementarias cuando lo consideren oportuno.  
### <a name="privacy"></a>Privacidad 
Puede que los usuarios no quieran compartir con otras aplicaciones algunas de las propiedades que se escriben en los archivos. Las aplicaciones deben poder indicar que la información que escriben en las propiedades va a ser privada para sus aplicaciones, compartida con tan solo unas cuantas aplicaciones o públicas para todas las aplicaciones del sistema.  

Aunque sea potencialmente una característica interesante para algunos de los usuarios pioneros de la característica, sienten que haciendo públicas las propiedades todavía va a agregar más valor al diseño. Por lo tanto, se ha marcado como acertada, y seguiremos compilando la característica sin admitir la ocultación de los valores si es necesario. Agregarla más adelante abrirá más escenarios, por lo que es importante tener en cuenta los diseños.  

## <a name="conclusions"></a>Conclusiones 
Eso es todo, las propiedades complementarias son una forma sencilla de almacenar más propiedades de archivo en el sistema. Su uso es, por supuesto, opcional, pero puede proporcionar a la aplicación una ventaja sobre otras aplicaciones que no pueden ordenar y buscar datos tan rápidamente. 

Estamos deseando ver que las aplicaciones empiezan a usar estas propiedades. Si tienes alguna pregunta sobre cómo usar el encabezado, háznoslo saber en los comentarios a continuación. 
