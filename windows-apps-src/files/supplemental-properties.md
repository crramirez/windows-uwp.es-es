---
title: Uso de propiedades adicionales
description: Introducción al uso de propiedades adicionales y detalles sobre cómo se implementan en Windows
ms.date: 01/10/2017
ms.topic: article
keywords: API de Windows 10, uwp, WinRT, indizador, búsqueda
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369263"
---
# <a name="using-supplemental-properties"></a>Uso de propiedades adicionales  

## <a name="summary"></a>Resumen  
- Propiedades adicionales de permitir que las aplicaciones etiquetar archivos con propiedades sin cambiar el archivo 
- Es útil para los casos donde tiene las propiedades que son difíciles de calcular o el archivo no se puede modificar 
- Uso de propiedades adicionales es igual que con cualquier otra propiedad en el sistema de propiedades de Windows  

## <a name="introduction"></a>Introducción 
Muchas de las nuevas aplicaciones interesantes en los últimos años requieren ejecutando operaciones intensivas de CPU en los archivos de usuario para extraer propiedades útiles de los archivos más allá de los aspectos básicos, como la fecha de creación. Estas aplicaciones de reconocimiento de objetos en imágenes, extracción de intención en los correos electrónicos y el análisis de texto para agrupar documentos juntos. Esto se basa en cómo informática eficaz ahora está disponible en el consumidor la mayoría de los equipos.   

Hacer que los metadatos que se puede buscar al instante permite a los usuarios a ser exponencialmente más productivo. Basta con conocer que su hija se encuentra en una imagen es interesante, pero poder buscar la imagen de ella con su abuela es mucho más útil. Hace que la experiencia de uso de una idea del equipo más personal y más activa. Al igual que un usuario en la máquina está llegando a ayudarle a encontrar sus recuerdos valiosas. 

Durante décadas, la solución de búsqueda rápida en Windows ha sido el indexador y de Creators Update se ha actualizado para admitir estos nuevos escenarios. Las aplicaciones son capaces de etiquetar archivos con propiedades adicionales más allá de los que se extraen por el sistema. Estas propiedades se tratan como ciudadanos de primera clase  

## <a name="windows-properties"></a>Propiedades de Windows 
El [sistema de propiedades de Windows](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) ha sido una parte fundamental de la interacción con los archivos durante años. Permite que las aplicaciones leer las propiedades de archivos sin tener que entender el funcionamiento interno de todos los formatos de archivo diferentes o idiomas que podría ser un archivo en. Todo lo que se abstraen por usted como desarrollador, lo único que debe hacer es solicitar una lista y especificar el orden ascendente o descendente.  

El sistema de propiedades es imbricado con el indizador de Windows: lee todas las propiedades de los archivos dentro de su ámbito y almacenarlos. Más adelante cuando una aplicación solicita una lista de todas las .docx en una carpeta se ordene por fecha de modificación, exceptuando aquellos creados por John Smith el indizador puede devolver la lista al instante.  

La desventaja para el funcionamiento conjunto de estos sistemas es que el indizador que se usa para solicitar todas las propiedades almacenaría acerca de un archivo esté disponible al instante. Esto lo limitaban saber acerca de las propiedades más interesantes que tardan más tiempo para calcular porque no hay un requisito de tiempo de disco duro.  

Mediante las propiedades aunque es fácil, la aplicación puede solicitar un conjunto ordenado de propiedades sobre un archivo, muy similar a trabajar con una base de datos, o puede pasar una consulta, como el uso de un motor de búsqueda. El indizador procesará la consulta y devolver los resultados. Esto dará a los desarrolladores la flexibilidad necesaria para combinar sus filtros (por ejemplo la única de archivos de jpg de búsqueda) con la consulta del usuario (nombre de archivo a partir de "ave"). 

## <a name="supplemental-properties"></a>Propiedades adicionales 

Propiedades adicionales se comportan igual que las propiedades normales de Windows con una diferencia muy importante: no se van a escribirse cuando el archivo se agrega al indizador. Por otra aplicación en el sistema más adelante, se debe agregar una propiedad adicional. Es posible que dos minutos más tarde cuando finaliza el reconocimiento de objetos o podría ser días más tarde. 

Una vez que se escribe la propiedad puede se busca, filtran, ordenan o agrupan al igual que cualquier otra propiedad en el sistema. También se puede utilizar en consultas combinadas con otras propiedades del sistema, ya sea adicionales o no. Esto le ofrecen flexibilidad para combinar propiedades suplementarias fácilmente con el código existente de sistema de archivos sin tener que realizar una reescritura.  

### <a name="example-scenarios"></a>Situaciones que sirven de ejemplo 

Hay miles de propiedades diferentes que podría escribir en una propiedad adicional, pero hay un par de escenarios clave para este tutorial tenga que volver al:  

#### <a name="tagging-pictures-with-extracted-properties"></a>Etiquetado de imágenes con propiedades extraídas 
Estas aplicaciones pueden usar un modelo de aprendizaje automático entrenado para extraer las características de una imagen que el sistema no conoce como objeto en la imagen. A continuación, puede tomar los objetos que identifica en la imagen y agregarlos al sistema de propiedades para la búsqueda más adelante o agrupación.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Etiquetado de archivos con un identificador específico de la aplicación 
Muchas de las aplicaciones de sincronización de archivos usan su propio identificador único para realizar un seguimiento de los archivos cuando se mueven entre el servidor y varios dispositivos de cliente. El cliente de sincronización puede escribir este identificador para el sistema de propiedades sin llevar a cabo el archivo. Este ID ahora está disponible para la aplicación más adelante para un acceso rápido y está disponible para cualquier otra aplicación en el sistema de lectura al comunicarse con el proveedor de sincronización. 

Hay muchas otras opciones para el uso de propiedades adicionales, pero ambos realizar buenos ejemplos porque requieren búsqueda o la búsqueda rápida, son fragmentos de información no conoce el sistema y no puede agregarse al propio archivo.  

### <a name="using-supplemental-properties"></a>Uso de propiedades adicionales 
Uso de las propiedades adicionales es el mismo que escribir una propiedad normal en el sistema de archivos. Si está familiarizado con el objeto StorageFiles y propiedades, puede omitir esto. En caso contrario, veamos un ejemplo rápido de escribir una sola propiedad en un archivo y, a continuación, leer la misma propiedad volver más tarde.  

### <a name="writing-supplemental-properties"></a>Escribir propiedades adicionales  
El ejemplo solo modificará el primer archivo que busca por motivos de simplicidad, pero por lo general, una aplicación agregará la propiedad a todos los archivos que encuentra.  

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

Hay una comprobación importante si la ubicación está indizada antes de escribir una propiedad. En este ejemplo se usa las opciones de consulta para filtrar a ubicaciones indizadas solo. Si esto no es factible puede comprobar el estado indizado de la carpeta principal (archivo. GetParentAsync(). GetIndexedStateAsync()). En cualquier caso producirá los mismos resultados 

### <a name="reading-supplemental-properties"></a>Leer propiedades adicionales 
Nuevamente, leer una propiedad adicional es lo mismo que leer cualquier otra propiedad del sistema de archivos. En este ejemplo la aplicación simplemente leer una propiedad de un archivo que ya tiene un StorageFile para, pero también podría leer otras propiedades al mismo tiempo.  

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
Hay una comprobación para asegurarse de que el valor procedente del sistema de propiedades es lo que espera. Aunque es poco probable, es posible que se ha borrado el valor dado que la aplicación ha escrito. Esto se explicará en detalle a continuación.  

### <a name="implementation-notes"></a>Notas de implementación 
Hay unas cuantas opciones sutiles que se realizaron en el diseño de las propiedades adicionales. Para ayudar con la implementación de las siguientes secciones se copiaron desde la especificación de diseño de ingeniería de la característica. Proporcionan un vistazo a cómo la característica se diseñó y por qué existen algunas limitaciones. 

### <a name="supplemental-properties-available"></a>Propiedades adicionales disponibles 
Hay solo dos propiedades disponibles para su uso para aplicaciones inicialmente: System.Supplemental.ResourceId y System.Supplemental.AlbumID. Si es necesario para obtener más información que se pueden agregar. El identificador de álbum es una cadena de varios valor que puede usarse para muchas aplicaciones diferentes y el valor de ResourceId se utiliza como un identificador único para proveedores de sincronización en la nube. 

#### <a name="file-system-support"></a>Compatibilidad con sistema de archivos 
Puesto que con formato FAT extraíble es un escenario importante, propiedades suplementarias será compatible con las unidades FAT y NTFS. Esto garantizará que las propiedades adicionales se van a estar disponible para todos los usuarios, independientemente de su tipo de dispositivo.   

### <a name="non-indexed-locations"></a>Ubicaciones no indizadas  
En el escritorio, hay varias carpetas que no están indizadas. En estos casos, las aplicaciones que desee tener acceso a propiedades adicionales todavía. Sin embargo, las propiedades adicionales no están disponibles fuera de ubicaciones indizadas. Este equilibrio se realizó por un par de razones:  

- De forma predeterminada, se indizan todas las bibliotecas y las ubicaciones de almacenamiento en la nube.   
  Estas son las ubicaciones que aplicaciones para UWP a usar principalmente. Hay otras ubicaciones que no están indizadas (unidades de red o del sistema), pero con menos frecuencia se usan para almacenar los datos de usuario. 

- El diseño de la superficie de API de WinRT se da por supuesto que el indizador casi siempre está disponible.  
  Por lo tanto, el indizador ya está disponible en la mayoría de las ubicaciones de las aplicaciones que están interesadas en. Si se encuentran los usuarios para almacenar datos en ubicaciones no indizadas, la solución más fácil será agregar esa ubicación para el índice. Propiedades adicionales, a continuación, el trabajo, enumeración será más rápida y las aplicaciones podrán cambiar un seguimiento de la ubicación.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Leer o escribir propiedades adicionales de un archivo en una ubicación Non-Indexed 
A continuación, en el caso de que una aplicación intenta escribir una propiedad adicional en una ubicación que no está indizada, la llamada API producirá una excepción. Se trata de la misma excepción que se produce como cuando alguien intenta actualizar el System.Music.AlbumArtist en un archivo .docx (argumentos no válidos).  
 
### <a name="change-notifications"></a>Las notificaciones de cambios:  
Notificaciones de cambio de UWP y seguimiento de cambios seguirán funcionando para las propiedades adicionales, como lo hacen para las propiedades estándares. Esto permitirá que las aplicaciones que proporcionan datos para realizar un seguimiento de todos los cambios que sucede a uno de sus aplicaciones 
  
### <a name="invalidating-properties"></a>Invalidar las propiedades:  
Las propiedades adicionales en un archivo pueden ser actualizadas cada vez que un archivo se modifica o se mueven en el sistema. Insertar los datos de aplicaciones serán las mismas con la información acerca de si los datos es válidos o deben actualizarse para que el sistema sólo proporcionará las herramientas para poder averiguar a sí mismos.  
 
En el caso se modifica un archivo, pero no ha movido o cambiado de nombre, todas las propiedades adicionales en el archivo permanecerá sin cambios. Las aplicaciones podrán registrarse para recibir notificaciones de cambio a través de la superficie de API existente y actualizar las propiedades según sea necesario. 
 
Si se mueve el archivo, las propiedades se van a invalidar. La aplicación recibirá las notificaciones de cambio de cualquier eliminación, crear, cambiar el nombre o movido dependiendo de cómo se realiza exactamente de la operación. Una vez que la aplicación ha recibido la notificación de cambio podrá inspeccionar el archivo y actualice las propiedades adicionales en el archivo según sea necesario. 
 
### <a name="indexer-rebuilds"></a>Recompilaciones de indizador  
En ocasiones tiene el índice del sistema volver a generar uno de una serie de motivos: podría cambiar el esquema de propiedades, podría permitir que el usuario EDP o simplemente el archivo de base de datos podría resultar dañado. En estos casos, no se conservarán las propiedades adicionales. Hemos considerado hacer el trabajo para intentar conservar las propiedades adicionales cuando se regenera el índice, pero hubo un par de bloqueadores de elementos principales:  

### <a name="protecting-the-data"></a>Protección de datos 
En el caso donde el archivo de base de datos está dañado, ya sea por errores de disco o software malintencionado, va a ser imposible proteger los datos que se almacenaron en ese archivo. Deberá almacenarse en otro lugar en el sistema o de algún modo aislado del resto de la base de datos. 

Puesto que ya que estamos realizando mucho trabajo para realizar el índice menos probable que esté dañado, esto reducirá la tasa de incidencia de este caso de todos modos.  
Mantener la asignación entre los archivos y sus metadatos durante las recompilaciones 

Incluso si el índice puede proteger los datos a través de una recompilación, es imposible saber si el archivo ha cambiado mientras se volvía a generar el índice. Los datos que está protegiendo el índice desde el archivo podrían ser ya no es válidos si se modifica o se movió el archivo.  
Comportamiento 

En el caso de una regeneración de indizador, se perderán todos los datos complementarios. Las aplicaciones será responsables de poner los datos de vuelta en el indizador que se perdió durante la regeneración. Esto coloca una carga adicional en las aplicaciones, pero se considera razonable, ya que siempre contendrá el estado para todos sus datos maestro.  

### <a name="recovering"></a>Recuperación 
Una vez que las aplicaciones que haya observado que se regenera el índice, estarán responsables de actualizar las propiedades adicionales cuando lo consideren oportuno.  
### <a name="privacy"></a>Privacidad 
Algunas de las propiedades que pueden escribirse en los archivos se van a que los usuarios no puede compartirlas con otras aplicaciones. Las aplicaciones deben poder indicar que la información que va a escribir en las propiedades se va a ser privada para cualquiera sus aplicaciones compartidas con tan solo unos otras aplicaciones o públicas a cada aplicación en el sistema.  

Aunque esto es potencialmente una característica interesante para algunos de los usuarios pioneros de la característica, se sienten que la obtención de propiedades públicas todavía se va a agregar mucho valor para el diseño. Por lo tanto, se ha marcado como bueno disponer y seguimos debemos crear la característica sin soporte técnico para ocultar los valores si es necesario. Agregarlo más adelante abrirá más escenarios, por lo que es importante tener en cuenta en los diseños.  

## <a name="conclusions"></a>Conclusiones 
Eso es todo, propiedades adicionales son una forma sencilla para almacenar más de las propiedades de archivo en el sistema. Su uso por supuesto es opcional, pero puede proporcionar a la aplicación un borde a través de otras aplicaciones que no se puedan ordenar y buscar los datos lo más rápido. 

Estamos deseando verle aplicaciones empezar a usar estas propiedades. Si tiene alguna pregunta sobre cómo usar el encabezado háganoslo saber en los comentarios a continuación 
