---
title: Procedimientos recomendados para escribir en archivos
description: Conozca los procedimientos recomendados para usar diversos métodos de escritura de archivos de las clases FileIO y PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a6a1d93b1deaad084ff25db946199b678b35703c
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369510"
---
# <a name="best-practices-for-writing-to-files"></a>Procedimientos recomendados para escribir en archivos

**API importantes**

* [**Clase FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Clase PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Los desarrolladores a veces se encuentran con un conjunto de problemas comunes al usar los métodos **Escribir** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) para realizar operaciones de E/S del sistema de archivos. Por ejemplo, los problemas comunes incluyen:

* Un archivo que está parcialmente escrito.
* La aplicación recibe una excepción al llamar a uno de los métodos.
* Las operaciones dejan archivos .TMP con un nombre de archivo similar al nombre del archivo de destino.

Los métodos **Escribir** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) incluyen lo siguiente:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Este artículo proporciona detalles sobre cómo funcionan estos métodos para que los desarrolladores entiendan mejor cuándo y cómo usarlos. En este artículo se proporciona instrucciones y no intenta proporcionar una solución para todos los posibles problemas de E/S de archivos. 

> [!NOTE]
> Este artículo se enfoca en los métodos **FileIO** en ejemplos y discusiones. Sin embargo, los métodos **PathIO** siguen una trama similar y la mayor parte de la guía en este artículo se aplica a esos métodos también. 

## <a name="convenience-vs-control"></a>Comodidad frente a control

Un objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) no es un identificador de archivo como el modelo de programación nativo de Win32. En cambio, un [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) es una representación de un archivo con métodos para manipular su contenido.

Comprender este concepto es útil cuando se realiza E/S con un **StorageFile**. Por ejemplo, la sección [Escritura en un archivo ](quickstart-reading-and-writing-files.md#writing-to-a-file) presenta tres formas de escribir en un archivo:

* Utilizando el método [**FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync).
* Al crear un búfer y luego llamar al método [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync).
* El modelo de cuatro pasos mediante una secuencia:
  1. [Abrir](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) el archivo para obtener una secuencia.
  2. [Obtener](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) un flujo de salida.
  3. Crear un objeto [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) y llame al método **Escritura** correspondiente.
  4. [Confirmar](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) los datos en el escritor de datos y [ vacíe](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) el flujo de salida.

Los dos primeros escenarios son los más usados por aplicaciones. Escribir en el archivo en una sola operación es más fácil de programar y mantener, y también quita la responsabilidad de la aplicación de enfrentarse a muchas de las complejidades de E/S de archivos. Sin embargo, esta comodidad tiene un costo: la pérdida de control de toda la operación y la capacidad de detectar errores en puntos específicos.

## <a name="the-transactional-model"></a>El modelo transaccional

Los métodos **Escritura** de las clases [ **FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) ajustan los pasos en el tercer modelo de escritura descrito anteriormente, con una capa añadida. Este nivel se encapsula en una transacción de almacenamiento.

Para proteger la integridad del archivo original en caso de que algo salga mal al escribir los datos, los métodos **Escritura** utilizan un modelo transaccional al abrir el archivo utilizando [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Este proceso crea un objeto [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction). Después de crea este objeto de transacción, las API escriben los datos después de una manera similar al ejemplo de [acceso al archivo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) o en el ejemplo de código en el artículo [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction).

El siguiente diagrama ilustra las tareas subyacentes realizadas por el método **WriteTextAsync**  en una operación de escritura exitosa. Esta ilustración se proporciona una vista simplificada de la operación. Por ejemplo, omita los pasos como la finalización de async y codificación de texto en diferentes subprocesos.

![Diagrama de secuencia de llamada de API UWP para escribir en un archivo](images/file-write-call-sequence.svg)

Las ventajas de usar los métodos **Escritura** de las clases [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) y [**PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) en lugar del modelo de cuatro pasos más complejo que usa una secuencia son:

* Una llamada de API para controlar todos los pasos intermedios, incluidos los errores.
* El archivo original se mantiene si algo va mal.
* El estado del sistema intentará mantenerse lo más limpio posible.

Sin embargo, con tantos posibles puntos intermedios de error, existe una mayor probabilidad de error. Cuando se produce un error, puede ser difícil entender dónde falló el proceso. Las siguientes secciones presentan algunos de los errores que puede encontrar al usar los métodos **Escritura** y brindan posibles soluciones.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Códigos de error comunes para los métodos de escritura de las clases FileIO y PathIO

Esta tabla presenta los códigos de error comunes que los desarrolladores de aplicaciones encuentran cuando usan los métodos de **Escritura**. Los pasos en la tabla corresponden a los pasos en el diagrama anterior.

|  Nombre de error (valor)  |  Pasos  |  Causas  |  Soluciones  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  El archivo original puede estar marcado para su eliminación, posiblemente de una operación anterior.  |  Vuelva a intentar la operación.</br>Asegúrese de que el acceso al archivo está sincronizado.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  El archivo original es abierto por otra operación de escritura exclusiva.   |  Vuelva a intentar la operación.</br>Asegúrese de que el acceso al archivo está sincronizado.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  El archivo original (archivo.txt) no se pudo reemplazar porque está en uso. Otro proceso u operación obtuvo acceso al archivo antes de que pudiera ser reemplazado.  |  Vuelva a intentar la operación.</br>Asegúrese de que el acceso al archivo está sincronizado.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  El modelo de transacción crea un archivo adicional, y esto consume almacenamiento adicional.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  Esto puede suceder debido a múltiples operaciones de E/S pendientes o archivos de gran tamaño.  |  Un enfoque más granular mediante el control de la secuencia podría resolver el error.  |
|  E_FAIL (0x80004005) |  Cualquiera  |  Varios  |  Vuelva a intentar la operación. Si aún falla, podría tratarse de un error de plataforma y la aplicación debería finalizar porque se encuentra en un estado incoherente. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Otras consideraciones para los estados de archivo que pueden conducir a errores

Además de los errores devueltos por los métodos de **Escritura**, aquí hay algunas instrucciones sobre lo que puede esperar una aplicación al escribir en un archivo.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Los datos se escribieron en el archivo solo si se completó la operación

Su aplicación no debe realizar ninguna suposición sobre los datos en el archivo mientras una operación de escritura está en progreso. Intentar obtener acceso al archivo antes de que se complete una operación puede producir datos incoherentes. Su aplicación debe ser responsable del seguimiento de las E/S pendientes.

### <a name="readers"></a>Lectores

Si el archivo en el que se está escribiendo también está siendo utilizado por un lector educado (es decir, abierto con [ **FileAccessMode.Read** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), las lecturas posteriores fallarán con un error ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Algunas veces las aplicaciones vuelven a intentar abrir el archivo para leerlo nuevamente mientras la operación de **Escritura** está en curso. Esto podría dar como resultado una condición de carrera en la que la **Escritura** falla al intentar sobrescribir el archivo original porque no se puede reemplazar.

### <a name="files-from-knownfolders"></a>Archivos de KnownFolders

Su aplicación podría no ser la única aplicación que está intentando acceder a un archivo que reside en cualquiera de los [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). La próxima vez que intente leer el archivo no hay ninguna garantía de que si la operación se realiza correctamente, el contenido de una aplicación que escribió en el archivo permanecerá constante. Además, compartir o acceder a los errores denegados se vuelve más común en este escenario.

### <a name="conflicting-io"></a>E/S conflictiva

Las posibilidades de errores de concurrencia pueden reducirse si nuestra aplicación utiliza los métodos de **Escritura** para los archivos en sus datos locales, pero aún se requiere cierta precaución. Si se envían varias operaciones de **Escritura** simultáneamente al archivo, no hay ninguna garantía sobre qué datos terminan en el archivo. Para mitigar esto, recomendamos que su aplicación serialice operaciones de **Escritura** en el archivo.

### <a name="tmp-files"></a>~ Archivos TMP

Ocasionalmente, si la operación se cancela a la fuerza (por ejemplo, si la aplicación fue suspendida o terminada por el sistema operativo), la transacción no se confirma o cierra adecuadamente. Esto puede dejar archivos con una extensión (. ~ TMP). Considere eliminar estos archivos temporales (si existen en los datos locales de la aplicación) al controlar la activación de la aplicación.

## <a name="considerations-based-on-file-types"></a>Consideraciones basadas en tipos de archivos

Algunos errores pueden volverse más predominantes según el tipo de archivos, la frecuencia con la que se accede y el tamaño de su archivo. En general, hay tres categorías de archivos a los que puede acceder su aplicación:

* Archivos creados y editados por el usuario en la carpeta de datos locales de su aplicación. Estos se crean y editan solo mientras se usa su aplicación, y existen solo dentro de la aplicación.
* Metadatos de la aplicación Su aplicación usa estos archivos para realizar un seguimiento de su propio estado.
* Otros archivos en ubicaciones del sistema de archivos a los que su aplicación ha declarado funcionalidades de acceso. Estos se encuentran más comúnmente en uno de los [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Su aplicación tiene control total sobre las dos primeras categorías de archivos, ya que son parte de los archivos de paquetes de su aplicación y solo pueden acceder a ella. Para los archivos de la última categoría, su aplicación debe ser consciente de que otras aplicaciones y servicios del sistema operativo pueden tener acceso los archivos al mismo tiempo.

Dependiendo de la aplicación, el acceso a los archivos puede variar en la frecuencia:

* Muy baja. Por lo general, estos son archivos que se abren una vez cuando se inicia la aplicación y se guardan cuando la aplicación se suspende.
* Bajo. Estos son archivos en los que el usuario está realizando específicamente una acción (como guardar o cargar).
* Media o alta. Estos son archivos en los que la aplicación debe actualizar los datos constantemente (por ejemplo, funciones de guardado automático o seguimiento constante de metadatos).

Para el tamaño de archivo, tenga en cuenta los datos de rendimiento en el siguiente cuadro para el método **WriteBytesAsync**. Esta tabla compara el tiempo para completar una operación frente al tamaño del archivo, en un rendimiento promedio de 10000 operaciones por tamaño de archivo en un ambiente controlado.

![Rendimiento de WriteBytesAsync](images/writebytesasync-performance.png)

Los valores de tiempo en el eje y se omiten intencionadamente en este gráfico porque las configuraciones y hardware diferentes darán como resultado valores de tiempo absoluto diferentes. Sin embargo, hemos observado sistemáticamente estas tendencias en nuestras pruebas:

* Para archivos muy pequeños (< = 1 MB): El tiempo para completar las operaciones es consistentemente rápido.
* Para archivos más grandes (> 1 MB): El tiempo para completar las operaciones comienza a aumentar exponencialmente.

## <a name="io-during-app-suspension"></a>E/S durante la suspensión de la aplicación

su aplicación debe estar diseñada para controlar la suspensión si desea mantener información de estado o metadatos para usar en sesiones posteriores. Para obtener información de fondo sobre la suspensión de la aplicación, consulte [ Ciclo de vida de la aplicación ](../launch-resume/app-lifecycle.md) y [ esta publicación de blog ](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

A menos que el sistema operativo conceda una ejecución extendida a su aplicación, cuando se suspende la aplicación, tiene 5 segundos para liberar todos sus recursos y guardar sus datos. Para la mejor confiabilidad y experiencia de usuario, siempre asuma que el tiempo que tiene para controlar las tareas de suspensión es limitado. Tenga en cuenta las siguientes instrucciones durante el período de 5 segundos de tiempo para controlar las tareas de suspensión:

* Intente mantener E/S al mínimo para evitar condiciones de carrera causadas por las operaciones de vaciado y versión.
* Evite la escritura de archivos que requieren cientos de milisegundos o más para escribir.
* Si su aplicación utiliza los métodos de **Escritura**, tenga en cuenta todos los pasos intermedios que requieren estos métodos.

Si su aplicación funciona con una pequeña cantidad de datos de estado durante la suspensión, en la mayoría de los casos puede usar los métodos de **Escritura** para vaciar los datos. Sin embargo, si su aplicación utiliza una gran cantidad de datos de estado, considere usar flujos para almacenar directamente los datos. Esto puede ayudar a reducir el retraso introducido por el modelo transaccional de los métodos de **Escritura**. 

Para ver un ejemplo, vea la muestra [ BasicSuspension ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension).

## <a name="other-examples-and-resources"></a>Otros ejemplos y recursos

Estos son algunos ejemplos y otros recursos para escenarios específicos.

### <a name="code-example-for-retrying-file-io-example"></a>Ejemplo de código para volver a intentar el ejemplo de E/S de archivo

El siguiente es un ejemplo de pseudocódigo sobre cómo reintentar una escritura (C #), asumiendo que la escritura se realizará después de que el usuario elija un archivo para guardarlo:

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

La [programación paralela con .NET blog](https://devblogs.microsoft.com/pfxteam/) es un gran recurso para obtener orientación sobre la programación paralela. En particular, la [ publicación sobre AsyncReaderWriterLock ](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) describe cómo mantener el acceso exclusivo a un archivo para las escrituras al tiempo que permite el acceso de lectura simultáneo. Tenga en cuenta que la serialización de E/S afectará el rendimiento.

## <a name="see-also"></a>Consulte también

* [Crear, escribir y leer archivos](quickstart-reading-and-writing-files.md)
