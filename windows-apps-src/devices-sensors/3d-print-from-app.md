---
title: Impresión en 3D desde la aplicación
description: Aprende a agregar la función de impresión en 3D a tu aplicación universal de Windows. En este tema se explica cómo iniciar el cuadro de diálogo de impresión en 3D después de asegurarte de que el modelo 3D se puede imprimir y tiene el formato correcto.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 3dprinting, impresión en 3d
ms.localizationpriority: medium
ms.openlocfilehash: e2ed99720afdccef297d46853d4a2445b497195e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321695"
---
# <a name="3d-printing-from-your-app"></a>Impresión en 3D desde la aplicación

**API importantes**

-   [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D)

Aprende a agregar la función de impresión en 3D a tu aplicación universal de Windows. En este tema se explica cómo cargar datos de geometría 3D en la aplicación e iniciar el cuadro de diálogo de impresión en 3D después de asegurarse de que el modelo 3D se puede imprimir y tiene el formato correcto. Para obtener un ejemplo práctico de estos procedimientos, consulta el [ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> En el código de ejemplo de esta guía, los informes y el control de errores se han simplificado bastante por motivos de simplicidad.

## <a name="setup"></a>Programa de instalación


En la clase de aplicación que debe incluir la funcionalidad de impresión en 3D, agrega el espacio de nombres [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

En esta guía, se usarán los siguientes espacios de nombres adicionales.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Después, asigna a la clase campos de miembro útiles. Declara un objeto [**Print3DTask**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTask) para que represente la tarea de impresión que se pasará al controlador de impresión. Declara un objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) para que contenga el archivo de datos 3D original que debe cargarse en la app. Declara un objeto [**Printing3D3MFPackage**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage), que representa un modelo 3D listo para imprimir con todos los metadatos necesarios.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Crear una interfaz de usuario sencilla

En este ejemplo se incluyen tres controles de usuario: un botón de carga que colocará un archivo en la memoria de programa, un botón de corrección que modificará el archivo según sea necesario y un botón de impresión que iniciará el trabajo de impresión. El siguiente código crea estos botones (con sus controladores de eventos al hacer clic) en el archivo XAML de tu correspondiente clase .cs.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Agrega **TextBlock** para comentarios de la interfaz de usuario.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Obtener los datos de 3D


El método con el que la aplicación obtiene datos de geometría 3D variará. La aplicación puede recuperar datos de una digitalización 3D, descargar datos de modelo de un recurso web o generar una malla 3D mediante programación a través de fórmulas matemáticas o entradas del usuario. Para que resulte más sencillo, en esta guía se mostrará cómo cargar un archivo de datos 3D (de cualquiera de los distintos tipos de archivo comunes) en la memoria del programa desde el almacenamiento del dispositivo. La [Biblioteca de modelos de 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) proporciona distintos modelos que puedes descargar fácilmente en el dispositivo.

En tu método `OnLoadClick`, usa la clase [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para cargar un único archivo en la memoria de la aplicación.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Usar la aplicación 3D Builder para convertir a formato de fabricación 3D (.3mf)

En este punto, es posible cargar un archivo de datos 3D en la memoria de la aplicación. Sin embargo, los datos de geometría 3D pueden proporcionarse en muchos formatos diferentes y no todos son eficaces para la impresión 3D. En Windows 10, se usa el tipo de archivo 3D Manufacturing Format (formato de fabricación 3D) (.3mf) para todas las tareas de impresión 3D.

> [!NOTE]  
> El tipo de archivo .3mf ofrece más funciones de las que se incluyen en este tutorial. Para obtener más información sobre 3MF y las características que proporciona a los creadores y los consumidores de productos 3D, consulta [3MF Specification](https://3mf.io/what-is-3mf/3mf-specification/) (Especificación 3MF). Para obtener información sobre cómo usar estas características mediante las API de Windows 10, consulta el tutorial [Generar un paquete 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

La aplicación [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) puede abrir archivos de los formatos 3D más populares y guardarlos como archivos .3mf. En este ejemplo, donde el tipo de archivo puede variar, una solución muy sencilla es abrir la aplicación 3D Builder y pedir al usuario que guarde los datos importados como un archivo .3mf y que vuelva a cargarlo.

> [!NOTE]  
> Además de convertir los formatos de archivo, 3D Builder proporciona herramientas sencillas para editar modelos, agregar datos de color y realizar otras operaciones específicas de la impresión, por lo que a menudo vale la pena integrarla en la aplicación que se encargue de la impresión en 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Reparar los datos de modelo de impresión en 3D

No todos los datos de modelo 3D se pueden imprimir, aunque sean del tipo .3mf. Para que la impresora determine correctamente qué espacio se rellenará y qué debe dejarse vacío, los modelos que se hayan de imprimir deben ser cada uno de ellos una sola combinación perfecta, deben tener normales de superficie hacia el exterior y una geometría variada. Los problemas en estas áreas pueden surgir de diversas formas y pueden ser difíciles de detectar en formas complejas. Sin embargo, las soluciones contemporáneas de software suelen ser suficientes para convertir la geometría sin procesar en formas 3D imprimibles. Esto se conoce como *reparar* el modelo y se realizará el método `OnFixClick`.

El archivo de datos 3D se debe convertir para implementar [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream), que se puede usar para generar un objeto [**Printing3DModel**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3DModel).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Ahora, el objeto **Printing3DModel** está reparado y se puede imprimir. Usa [**SaveModelToPackageAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) para asignar el modelo al objeto **Printing3D3MFPackage** que se haya declarado al crear la clase.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Ejecutar la tarea de impresión: crear un controlador de TaskRequested


Más adelante, cuando se muestre el cuadro de diálogo de impresión en 3D al usuario y este elija iniciar la impresión, la aplicación deberá pasar los parámetros deseados a la canalización de impresión en 3D. La API de impresión 3D generará el evento **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** . Debes escribir un método para controlar este evento correctamente. Como siempre, el método de controlador debe ser del mismo tipo como su evento: El **TaskRequested** eventos tiene parámetros [ **Print3DManager** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (una referencia a su objeto de remitente) y un [  **Print3DTaskRequestedEventArgs** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) objeto, que contiene la mayor parte de la información pertinente.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

El propósito principal de este método es usar el parámetro *args* para enviar un objeto **Printing3D3MFPackage** por la canalización. El **Print3DTaskRequestedEventArgs** tipo tiene una propiedad: [**Solicitar**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Es del tipo [**Print3DTaskRequest**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) y representa una solicitud de trabajo de impresión. Su método [**CreateTask**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) permite al programa enviar la información correcta respecto al trabajo de impresión y devuelve una referencia al objeto **Print3DTask** que se haya enviado a la canalización de impresión 3D.

**CreateTask** tiene los siguientes parámetros de entrada: Un elemento string para el nombre del trabajo de impresión, un elemento string para el identificador de la impresora que se debe utilizar y un delegado [**Print3DTaskSourceRequestedHandler**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler). El delegado se invoca automáticamente cuando se produce el evento **3DTaskSourceRequested** (lo hace la propia API). Lo importante a tener en cuenta es que este delegado se invoca cuando se inicia un trabajo de impresión y es el responsable de proporcionar el paquete de impresión 3D correcto.

**Print3DTaskSourceRequestedHandler** usa un parámetro, un objeto [**Print3DTaskSourceRequestedArgs**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) que proporciona los datos que se enviarán. El único método público de esta clase, [**SetSource**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource), acepta el paquete que se imprimirá. Implementa un delegado **Print3DTaskSourceRequestedHandler** de la siguiente manera.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

A continuación, llama a **CreateTask** mediante el delegado recién definido, `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

El valor devuelto **Print3DTask** se asigna a la variable de clase declarada al principio. Ahora puedes usar (opcionalmente) esta referencia para controlar ciertos eventos producidos por la tarea.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Debes implementar un método `Task_Submitting` y un método `Task_Completed` si quieres registrarlos en estos eventos.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Ejecutar la tarea de impresión: Abrir el cuadro de diálogo de impresión 3D


La parte final del código necesario es la que inicia el cuadro de diálogo de impresión 3D. Al igual que una ventana de un cuadro de diálogo de impresión convencional, el cuadro de diálogo de impresión 3D proporciona una serie de opciones de impresión de última hora y permite al usuario elegir la impresora que se usará (si está conectada a través de USB o la red).

Registra tu método `MyTaskRequested` con el evento **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Después de registrar el controlador de eventos **TaskRequested**, puedes invocar el método [**ShowPrintUIAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync), que abre el cuadro de diálogo de impresión en 3D en la ventana de la aplicación actual.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Por último, es conveniente eliminar el registro de los controladores de eventos una vez que la aplicación reanude el control.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Temas relacionados

[Generar un paquete 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[Ejemplo UWP de impresión 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
