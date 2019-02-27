---
title: Procedimientos recomendados para escribir en archivos
description: Obtén información sobre los procedimientos recomendados para usar el archivo diversos métodos de las clases FileIO y PathIO de escritura.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115716"
---
# <a name="best-practices-for-writing-to-files"></a>Procedimientos recomendados para escribir en archivos

**API importantes**

* [**Clase FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Clase PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Los desarrolladores a veces se ejecutan en un conjunto de problemas comunes al usar los métodos de **escritura** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) para realizar operaciones de E/S del sistema de archivos. Por ejemplo, los problemas comunes se incluyen:

• Un archivo se escribe parcialmente • la aplicación recibe una excepción cuando se llama a uno de los métodos. • Las operaciones de dejan atrás. Archivos TMP con un nombre similar al nombre del archivo de destino.

Los métodos de **escritura** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) incluyen lo siguiente:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Este artículo se proporcionan detalles sobre el funcionan de estos métodos de modo que los desarrolladores comprenden mejor cuándo y cómo usarlos. En este artículo se proporciona instrucciones y no intenta proporcionar una solución para todos los problemas de E/S de archivo posibles. 

> [!NOTE]
> En este artículo se centra en los métodos **FileIO** en las discusiones y ejemplos. Sin embargo, los métodos de **PathIO** siguen un patrón similar y la mayoría de las instrucciones de este artículo también se aplica a estos métodos. 

## <a name="conveience-vs-control"></a>Conveience frente al control

Un objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) no es un identificador de archivo, como el modelo de programación de Win32 nativo. En su lugar, un [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) es una representación de un archivo con métodos para manipular su contenido.

Descripción de este concepto es útil cuando se realiza E/S con un **StorageFile**. Por ejemplo, la sección de [escritura en un archivo](quickstart-reading-and-writing-files.md#writing-to-a-file) presenta tres maneras de escribir en un archivo:

* Mediante el método [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) .
* Al crear un búfer y, a continuación, llamar al método de [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) .
* El modelo de cuatro pasos con un flujo:
  1. [Abre](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) el archivo para obtener una secuencia.
  2. [Obtener](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) un flujo de salida.
  3. Crear un objeto de [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) y llamar al método correspondiente de **escribir** .
  4. [Confirmar](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) los datos en el sistema de escritura de datos y [Vaciar](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) el flujo de salida.

Los dos primeros escenarios son los más usados habitualmente por las aplicaciones. Escritura en el archivo en una sola operación es más fácil de mantener y de código y también quita la responsabilidad de la aplicación de lidiar con gran parte de la complejidad de E/S de archivo. Sin embargo, esta comodidad implica un coste: la pérdida de control de toda la operación y la capacidad para detectar errores en puntos específicos.

## <a name="the-transactional-model"></a>El modelo transaccional

Los métodos de **escritura** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) ajustan los pasos en el tercer modelo de escritura que se ha descrito anteriormente, con una capa adicional. Este nivel se encapsula en una transacción de almacenamiento.

Para proteger la integridad del archivo original en caso de que algo va mal al escribir los datos, los métodos **Write** usan un modelo transaccional abriendo el archivo con [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Este proceso crea un objeto de [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) . Después de crea este objeto de transacción, las API de escriben los datos después de una forma similar a la muestra de [Acceso de archivo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) o el ejemplo de código en el artículo [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) .

El siguiente diagrama muestra las tareas subyacentes realizadas por el método **WriteTextAsync** en una operación de escritura se realiza correctamente. Esta ilustración proporciona una vista simplificada de la operación. Por ejemplo, omite pasos como la finalización de codificación y asincrónica de texto en subprocesos diferentes.

![Diagrama de secuencia de llamada de API de UWP para escribir en un archivo](images/file-write-call-sequence.svg)

Las ventajas de usar los métodos de **escritura** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) en lugar del modelo de cuatro pasos más complejo con un flujo son:

* Una llamada de API para controlar todos los pasos intermedios, incluidos los errores.
* Si algo va mal, se guarda el archivo original.
* El estado del sistema intentará mantenerse lo más limpio posible.

Sin embargo, con tantos posibles intermedios puntos de error, hay una mayor probabilidad de error. Cuando se produce un error puede ser difícil de entender donde no se pudo el proceso. Las siguientes secciones presentan algunos de los errores que podrían surgir al usar los métodos de **escribir** y proporcionar soluciones posibles.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Códigos de error comunes para los métodos de escritura de las clases FileIO y PathIO

Esta tabla presentan los códigos de error comunes que los desarrolladores de aplicaciones surgir al usar los métodos de **escribir** . Los pasos descritos en la tabla corresponden a los pasos en el diagrama anterior.

|  Nombre del error (value)  |  Pasos  |  Causas  |  Soluciones  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0 X 80070005)  |  5  |  El archivo original se podría marcar para su eliminación, posiblemente de una operación anterior.  |  Vuelve a intentar la operación.</br>Asegúrate de que se sincroniza el acceso al archivo.  |
|  ERROR_SHARING_VIOLATION (0 X 80070020)  |  5  |  Se abre el archivo original por otra escritura exclusiva.   |  Vuelve a intentar la operación.</br>Asegúrate de que se sincroniza el acceso al archivo.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19-20  |  No se pudo reemplazar el archivo original (file.txt) porque está en uso. Otro proceso u operación obtenido acceso al archivo antes de que podría reemplazarse.  |  Vuelve a intentar la operación.</br>Asegúrate de que se sincroniza el acceso al archivo.  |
|  ERROR_DISK_FULL (0 X 80070070)  |  7, 14, 16, 20  |  El modelo de transacción crea un archivo adicional, y Esto consume almacenamiento adicional.  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  Esto puede ocurrir debido a varias operaciones de E/S pendientes o los tamaños de archivo grandes.  |  Un enfoque más detallado mediante el control de la secuencia puede resolver el error.  |
|  E_FAIL (0 X 80004005) |  Cualquiera  |  Varios  |  Vuelve a intentar la operación. Si el error persiste, podría ser un error de la plataforma y la aplicación debe finalizar porque está en un estado incoherente. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Otras consideraciones para los Estados de archivo que podrían dar lugar a errores

Además de los errores devueltos por los métodos de **escritura** , estas son algunas directrices sobre lo que una aplicación puede esperar al escribir en un archivo.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Datos se escriben en el archivo solo si completar la operación

La aplicación no debe hacer ninguna suposición sobre los datos en el archivo mientras una operación de escritura está en curso. Al intentar obtener acceso al archivo antes de que finalice una operación podría provocar datos incoherentes. La aplicación debe ser responsable de realizar el seguimiento de operaciones pendientes de E/s.

### <a name="readers"></a>Lectores

Si el archivo que se escriben en también está usando un lector educado (es decir, abierto con [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), lecturas posteriores se provocarán un error ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). A veces, las aplicaciones intentar volver a abrir el archivo de lectura nuevo mientras la operación de **escritura** está en curso. Esto podría provocar una condición de carrera en el que **escribir** , por último, se produce un error al intentar sobrescribir el archivo original porque no se puede reemplazar.

### <a name="files-from-knownfolders"></a>Archivos de KnownFolders

La aplicación podría no ser la única aplicación que está intentando acceder a un archivo que resida en cualquiera de los [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). La próxima vez que se trata de leer el archivo no hay ninguna garantía de que si la operación es correcta, el contenido de que una aplicación se escribió en el archivo permanecerá constante. Además, compartir o acceso denegado errores sea más habituales en este escenario.

### <a name="conflicting-io"></a>E/S en conflicto

Se pueden reducir las posibilidades de errores de simultaneidad si nuestra aplicación usa los métodos de **escritura** para los archivos de datos locales, pero algunas precaución sigue siendo necesaria. Si varias operaciones de **escritura** se va a enviar al mismo tiempo en el archivo, no hay ninguna garantía sobre qué datos se terminan en el archivo. Para mitigar esta situación, te recomendamos que la aplicación serializa las operaciones de **escritura** al archivo.

### <a name="tmp-files"></a>~ Archivos TMP

En ocasiones, si la operación se cancela forzosamente (por ejemplo, si la aplicación se suspenda o finalice el sistema operativo), la transacción se confirma no o cerrada correctamente. Esto puede dejar archivos con una (. ~ TMP) extensión. Considera la posibilidad de eliminar estos archivos temporales (si existe en los datos locales de la aplicación) al controlar la activación de la aplicación.

## <a name="considerations-based-on-file-types"></a>Consideraciones en función de los tipos de archivo

Algunos errores pueden volverse más frecuentes según el tipo de archivos, la frecuencia en el que se está acceso a ellos y su tamaño de archivo. Por lo general, hay tres categorías de archivos que puede tener acceso la aplicación:

* Los archivos se crean y editan por el usuario en la carpeta de datos locales de la aplicación. Estos se crean y modificar solo mientras se usa la aplicación, y existen solo dentro de la aplicación.
* Metadatos de la aplicación. La aplicación usa estos archivos para realizar un seguimiento de su propio estado.
* Otros archivos en ubicaciones del sistema de archivos donde la aplicación ha declarado funcionalidades para acceder a. Normalmente, estos se encuentran en uno de los [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

La aplicación tiene control total sobre las dos primeras categorías de archivos, porque forman parte de los archivos de paquete de la aplicación y se accede a la aplicación exclusivamente. Para los archivos de la última categoría, la aplicación debe tener en cuenta que otros servicios del sistema operativo y aplicaciones tengan acceso a los archivos al mismo tiempo.

Dependiendo de la aplicación, acceso a los archivos puede variar en la frecuencia:

* Muy bajo. Por lo general, estos son los archivos que se abren cuando cuando los inicios de la aplicación y se ha guardado cuando se suspende la aplicación.
* Bajo. Estos son los archivos que el usuario está específicamente realizando una acción (por ejemplo, al guardar o cargar).
* Medio o alto. Estos son los archivos en el que la aplicación debe actualizar constantemente datos (por ejemplo, las características de guardado automático o metadatos constantes seguimiento).

Para el tamaño de archivo, considera la posibilidad de los datos de rendimiento en el siguiente gráfico para el método **WriteBytesAsync** . Este gráfico compara el tiempo para completar un tamaño de archivo de vs operación, a través de un rendimiento medio de operaciones de 10000 por el tamaño del archivo en un entorno controlado.

![Rendimiento WriteBytesAsync](images/writebytesasync-performance.png)

Los valores de tiempo en el eje y se omiten intencionadamente desde este gráfico porque las configuraciones y los diferentes tipos de hardware, se obtendrá valores de tiempo absoluto diferentes. Sin embargo, hemos observado estos tendencias de forma coherente en nuestras pruebas:

* Para los archivos muy pequeños (< = 1 MB): el tiempo para completar las operaciones es rápido de forma coherente.
* Para archivos más grandes (> 1 MB): el tiempo para completar las operaciones se inicia para aumentar exponencialmente.

## <a name="io-during-app-suspension"></a>E/S durante la suspensión de la aplicación

La aplicación debe diseñada para controlar la suspensión si quieres guardar la información de estado o metadatos para su uso en sesiones posteriores. Para obtener información acerca de la suspensión de la aplicación, consulta [esta entrada de blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)y [ciclo de vida de aplicación](../launch-resume/app-lifecycle.md) .

A menos que el sistema operativo conceda ejecución extendida a la aplicación, cuando se suspende la aplicación tiene 5 segundos para liberar sus recursos y guardar sus datos. Para la confiabilidad y el usuario mejor experiencia, siempre asumir el tiempo que tendrás que controlar las tareas de suspensión es limitado. Ten en cuenta las siguientes directrices durante el período de tiempo para controlar las tareas de suspensión de 5 segundos:

* Intenta mantener la E/S a una cantidad mínima para evitar las condiciones de carrera provocadas por las operaciones de vaciado y lanzamiento.
* Evitar la escritura de archivos que requieren cientos de milisegundos o más escribir.
* Si la aplicación usa los métodos de **escritura** , ten en cuenta todos los pasos intermedios que requieren de estos métodos.

Si tu aplicación funciona en una pequeña cantidad de datos de estado durante la suspensión, en la mayoría de los casos puedes usar los métodos de **escritura** al vaciar los datos. Sin embargo, si la aplicación usa una gran cantidad de datos de estado, considera la posibilidad de usar secuencias para almacenar los datos de directamente. Esto puede ayudar a reducir el retraso introducido el modelo transaccionales de los métodos de **escribir** . 

Por ejemplo, vea el ejemplo [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) .

## <a name="other-examples-and-resources"></a>Otros ejemplos y recursos

Hay varios ejemplos y otros recursos para escenarios específicos.

### <a name="code-example-for-retrying-file-io-example"></a>Ejemplo de código para volver a intentar el ejemplo de E/S de archivo

El siguiente es un ejemplo de seudocódigo sobre cómo volver a intentar una operación de escritura (C#), suponiendo que la escritura es hacerse después de que el usuario elige un archivo para guardar:

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

La [Programación paralela con .NET blog](https://blogs.msdn.microsoft.com/pfxteam/) es un recurso para obtener instrucciones sobre la programación en paralelo. En concreto, la [registra sobre AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) describe cómo mantener acceso exclusivo a un archivo para las escrituras pero permite el acceso de lectura simultáneo. Ten en cuenta que la serialización de que afectará la E/S rendimiento.

## <a name="see-also"></a>Ver también

* [Crear, escribir y leer archivos](quickstart-reading-and-writing-files.md)
