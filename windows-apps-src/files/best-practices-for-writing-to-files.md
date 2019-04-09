---
title: Procedimientos recomendados para escribir en archivos
description: Obtenga información sobre procedimientos recomendados para usar varios archivos escribir métodos de las clases FileIO y PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e0fcb903bd272bd10d434a27d41e6e4558a624ea
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334903"
---
# <a name="best-practices-for-writing-to-files"></a>Procedimientos recomendados para escribir en archivos

**API importantes**

* [**Clase FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Clase PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Los desarrolladores a veces se ejecute en un conjunto de problemas comunes al usar el **escribir** métodos de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) clases para realizar operaciones de E/S del sistema de archivos. Por ejemplo, problemas comunes incluyen:

* Parcialmente se escribe un archivo.
* La aplicación recibe una excepción al llamar a uno de los métodos.
* Las operaciones de dejan atrás. Archivos TMP con un nombre similar al nombre del archivo de destino.

El **escribir** métodos de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) clases incluyen lo siguiente:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Este artículo se proporcionan detalles sobre el funcionan de estos métodos de modo que los desarrolladores comprender mejor cuándo y cómo usarlas. En este artículo se proporciona instrucciones y no intenta proporcionar una solución para todos los problemas de E/S de archivo posibles. 

> [!NOTE]
> En este artículo se centra en la **FileIO** métodos en los ejemplos y discusiones. Sin embargo, el **PathIO** métodos siguen un patrón similar y la mayoría de las instrucciones de este artículo también se aplica a esos métodos. 

## <a name="convenience-vs-control"></a>Comodidad frente a control

Un [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) objeto no es un identificador de archivo, como el modelo de programación de Win32 nativo. En su lugar, un [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) es una representación de un archivo con los métodos para manipular su contenido.

Descripción de este concepto es útil al realizar la E/S con un **StorageFile**. Por ejemplo, el [escribir en un archivo](quickstart-reading-and-writing-files.md#writing-to-a-file) sección presenta tres formas de escribir en un archivo:

* Mediante el [ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) método.
* Creación de un búfer y, a continuación, llamando a la [ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) método.
* El modelo de cuatro pasos mediante una secuencia:
  1. [Abra](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) el archivo para obtener una secuencia.
  2. [Obtener](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) un flujo de salida.
  3. Crear un [ **DataWriter** ](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) de objetos y llamar a la correspondiente **escribir** método.
  4. [Confirmar](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) los datos en el sistema de escritura de datos y [vaciar](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) el flujo de salida.

Los dos primeros escenarios son los más usados por aplicaciones. Escribir en el archivo en una sola operación es más fácil de programar y mantener, y también quita la responsabilidad de la aplicación deben enfrentarse a muchas de las complejidades de E/S de archivos. Sin embargo, esta comodidad tiene un costo: la pérdida de control de toda la operación y la capacidad de detectar errores en puntos específicos.

## <a name="the-transactional-model"></a>El modelo transaccional

El **escribir** métodos de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) clases ajustan a los pasos en la escritura terceros modelo descrito anteriormente, con una capa adicional. Esta capa se encapsula en una transacción de almacenamiento.

Para proteger la integridad del archivo original en caso de que algo va mal mientras escribe los datos, el **escribir** métodos usan un modelo transaccional abriendo el archivo mediante [ **OpenTransactedWriteAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Este proceso crea un [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) objeto. Después de crea este objeto de transacción, las API de escriben los datos después de una manera similar a la [acceso al archivo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) ejemplo o en el ejemplo de código en el [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) artículo.

El siguiente diagrama muestra las tareas subyacentes realizadas por el la **WriteTextAsync** método en una operación de escritura correcta. En esta ilustración proporciona una vista simplificada de la operación. Por ejemplo, omite los pasos como la finalización de async y codificación de texto en diferentes subprocesos.

![Diagrama de secuencia de llamada de API de UWP para escribir en un archivo](images/file-write-call-sequence.svg)

Las ventajas de utilizar el **escribir** métodos de la [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) clases en su lugar del modelo de cuatro pasos más complejo mediante una secuencia son:

* Una llamada de API para controlar todos los pasos intermedios, incluidos los errores.
* El archivo original se mantiene si algo va mal.
* El estado del sistema intentará mantenerse lo más limpio posible.

Sin embargo, con tantos puntos intermedios posible del error, hay mayores posibilidades de error. Cuando se produce un error puede ser difícil de entender que el proceso no pudo. Las siguientes secciones presentan algunos de los errores que pueden surgir al usar el **escribir** métodos y proporcionar soluciones posibles.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Códigos de error comunes para los métodos de escritura de las clases FileIO y PathIO

Esta tabla presentan los códigos de error comunes que los desarrolladores de aplicaciones encuentran al usar el **escribir** métodos. Los pasos descritos en la tabla corresponden a pasos en el diagrama anterior.

|  Error de nombre (valor)  |  Pasos  |  Causas  |  Soluciones  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  El archivo original se podría marcar para su eliminación, posiblemente desde una operación anterior.  |  Vuelva a intentar la operación.</br>Asegúrese de que se sincroniza el acceso al archivo.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  Se abre el archivo original por otra operación de escritura exclusivo.   |  Vuelva a intentar la operación.</br>Asegúrese de que se sincroniza el acceso al archivo.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  No se pudo reemplazar el archivo original (file.txt) porque está en uso. Otro proceso u operación obtenido acceso al archivo antes de que se reemplazarán.  |  Vuelva a intentar la operación.</br>Asegúrese de que se sincroniza el acceso al archivo.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  El modelo de transacción crea un archivo adicional, y Esto consume almacenamiento adicional.  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  Esto puede deberse a varias operaciones de E/S pendientes o tamaños de archivo grandes.  |  Un enfoque más granular mediante el control de la secuencia podría resolver el error.  |
|  E_FAIL (0x80004005) |  Cualquiera  |  Varios  |  Vuelva a intentar la operación. Si sigue sin funcionar, podría ser un error de la plataforma y la aplicación debe finalizar porque está en un estado incoherente. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Otras consideraciones para los Estados de los archivos que podrían conducir a errores

Además de los errores devueltos por la **escribir** métodos, estas son algunas directrices sobre lo que una aplicación puede esperar al escribir en un archivo.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Se escribieron datos en el archivo solo si completó la operación

La aplicación no debe realizar ninguna suposición acerca de los datos en el archivo mientras está en curso una operación de escritura. Intenta obtener acceso al archivo antes de que finalice una operación podría producir datos incoherentes. La aplicación debe ser responsable del seguimiento de E/s pendientes.

### <a name="readers"></a>Lectores

Si se usa el archivo que también es que se escriben en un lector normal (es decir, abierto con [ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), las lecturas subsiguientes se provocarán un error ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). A veces aplicaciones vuelva a intentar abrir el archivo para lectura nuevo mientras el **escribir** operación está en curso. Esto puede dar lugar a una condición de carrera en el que el **escribir** en última instancia, se produce un error al intentar sobrescribir el archivo original porque no se puede reemplazar.

### <a name="files-from-knownfolders"></a>Archivos de KnownFolders

La aplicación podría no ser la única aplicación que está intentando acceder a un archivo que reside en cualquiera de los [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). La próxima vez que intente leer el archivo no hay ninguna garantía de que si la operación se realiza correctamente, el contenido de una aplicación que escribió en el archivo permanecerá constante. Además, compartir o denegación de acceso errores vuelto más comunes en este escenario.

### <a name="conflicting-io"></a>E/S en conflicto

Pueden reducir las posibilidades de errores de simultaneidad si la aplicación usa el **escribir** métodos para los archivos de sus datos locales, pero algunos precaución sigue siendo necesaria. Si hay varios **escribir** operaciones se envían al mismo tiempo en el archivo, no hay ninguna garantía sobre qué datos se terminan en el archivo. Para mitigar esta situación, se recomienda que la aplicación serializa **escribir** operaciones en el archivo.

### <a name="tmp-files"></a>~ Archivos de TMP

En ocasiones, si fuerza se cancela la operación (por ejemplo, si la aplicación se ha suspendido o terminado por el sistema operativo), la transacción no se confirma o cerrada correctamente. Esto puede dejar atrás los archivos con un (. ~ TMP) extensión. Considere la posibilidad de eliminar estos archivos temporales (si existen en los datos locales de la aplicación) al administrar la activación de la aplicación.

## <a name="considerations-based-on-file-types"></a>Consideraciones en función de los tipos de archivo

Algunos errores pueden volverse más predominantes según el tipo de archivos, la frecuencia en la que tiene acceso a ellos y su tamaño de archivo. Por lo general, hay tres categorías de archivos que puede tener acceso la aplicación:

* Los archivos se crean y editan por el usuario en la carpeta de datos locales de la aplicación. Estos se crean y se puede editar sólo cuando se utiliza la aplicación, y existen solo dentro de la aplicación.
* Metadatos de la aplicación. La aplicación usa estos archivos para realizar un seguimiento de su propio estado.
* En las ubicaciones del sistema de archivos donde la aplicación ha declarado funcionalidades de acceso a otros archivos. Estos se encuentran con más frecuencia en uno de los [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

La aplicación tiene control total en las dos primeras categorías de archivos, ya que forman parte de los archivos de paquete de la aplicación y se tiene acceso por la aplicación de forma exclusiva. Para los archivos de la última categoría, la aplicación debe ser consciente de que otras aplicaciones y servicios del sistema operativo pueden tener acceso los archivos al mismo tiempo.

Dependiendo de la aplicación, el acceso a los archivos puede variar con frecuencia:

* Muy baja. Normalmente, estos son archivos que se abren una vez cuando la aplicación inicia y se guardan cuando se suspende la aplicación.
* Bajo. Estos son los archivos que el usuario en concreto está teniendo una acción en (por ejemplo, guardar o cargar).
* Media o alta. Estos son los archivos en el que la aplicación debe actualizar constantemente los datos (por ejemplo, las características de autoguardado o constante metadatos de seguimiento).

Tamaño del archivo, considere la posibilidad de los datos de rendimiento en el siguiente gráfico para la **WriteBytesAsync** método. Esta tabla compara el tiempo para completar un tamaño de archivo de vs de operación a través de un rendimiento medio de 10 000 operaciones por tamaño de archivo en un entorno controlado.

![Rendimiento WriteBytesAsync](images/writebytesasync-performance.png)

Los valores de tiempo en el eje y se omiten intencionadamente en este gráfico porque las configuraciones y hardware diferentes darán como resultado valores de tiempo absoluto diferentes. Sin embargo, hemos observado sistemáticamente estas tendencias en nuestras pruebas:

* Para archivos muy pequeños (< = 1 MB): El tiempo para completar las operaciones es velocidad constante.
* Archivos de mayor tamaño (> 1 MB): El tiempo para completar las operaciones comienza a aumentar exponencialmente.

## <a name="io-during-app-suspension"></a>E/S durante la suspensión de la aplicación

La aplicación deben diseñar para controlar la suspensión si desea mantener la información de estado o los metadatos para su uso en sesiones posteriores. Para obtener información general acerca de la suspensión de la aplicación, consulte [ciclo de vida de aplicación](../launch-resume/app-lifecycle.md) y [esta entrada de blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

A menos que el sistema operativo concede la ejecución extendida a la aplicación, cuando se suspende la aplicación tiene 5 segundos para liberar todos sus recursos y guardar sus datos. Para la confiabilidad y el usuario mejor experiencia, suponer siempre se limita el tiempo del que dispone para controlar tareas de suspensión. Tenga en cuenta las siguientes directrices durante el período de tiempo para controlar tareas de suspensión de 5 segundos:

* Intente mantener E/S al mínimo para evitar condiciones de carrera causadas por las operaciones de vaciado y versión.
* Evitar la escritura de archivos que requieren cientos de milisegundos o más para escribir.
* Si su aplicación usa el **escribir** métodos, tenga en cuenta todos los pasos intermedios que requieren estos métodos.

Si la aplicación funciona en una pequeña cantidad de datos de estado durante la suspensión, en la mayoría de los casos puede usar el **escribir** métodos al vaciar los datos. Sin embargo, si su aplicación usa una gran cantidad de datos de estado, considere el uso de secuencias para almacenar directamente los datos. Esto puede ayudar a reducir el retraso que introduce el modelo transaccional de la **escribir** métodos. 

Para obtener un ejemplo, vea el [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) ejemplo.

## <a name="other-examples-and-resources"></a>Otros ejemplos y recursos

Estos son algunos ejemplos y otros recursos para escenarios específicos.

### <a name="code-example-for-retrying-file-io-example"></a>Ejemplo de código para volver a intentar el ejemplo de archivo E/S

El siguiente es un ejemplo de pseudocódigo acerca de cómo volver a intentar una operación de escritura (C#), suponiendo que la operación de escritura es para hacerse una vez que el usuario selecciona un archivo para guardar:

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>Sincronizar el acceso al archivo

El [Parallel Programming with .NET blog](https://blogs.msdn.microsoft.com/pfxteam/) es un excelente recurso para obtener instrucciones sobre la programación paralela. En concreto, el [escribir comentarios sobre un elemento AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) describe cómo mantener el acceso exclusivo a un archivo para escritura mientras que permita el acceso de lectura simultáneo. Tenga en cuenta que la serialización que e/s afectará al rendimiento.

## <a name="see-also"></a>Vea también

* [Crear, escribir y leer archivos](quickstart-reading-and-writing-files.md)
