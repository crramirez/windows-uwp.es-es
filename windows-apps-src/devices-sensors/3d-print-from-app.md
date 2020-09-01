---
title: Impresión en 3D desde la aplicación
description: Aprende a agregar la función de impresión en 3D a tu aplicación universal de Windows. En este tema se explica cómo iniciar el cuadro de diálogo de impresión en 3D después de asegurarte de que el modelo 3D se puede imprimir y tiene el formato correcto.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 3dprinting, impresión en 3D
ms.localizationpriority: medium
ms.openlocfilehash: b89fb14b8e554452674e0c7b0bc31b6314cce253
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175499"
---
# <a name="3d-printing-from-your-app"></a>Impresión en 3D desde la aplicación

**API importantes**

-   [**Windows. Graphics. Printing3D**](/uwp/api/Windows.Graphics.Printing3D)

Aprende a agregar la función de impresión en 3D a tu aplicación universal de Windows. En este tema se explica cómo cargar datos de geometría 3D en la aplicación e iniciar el cuadro de diálogo de impresión en 3D después de asegurarse de que el modelo 3D se puede imprimir y tiene el formato correcto. Para obtener un ejemplo práctico de estos procedimientos, vea el [ejemplo de UWP de impresión en 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> En el código de ejemplo de esta guía, la elaboración de informes y el control de errores se ha simplificado en gran medida por simplicidad.

## <a name="setup"></a>Configurar


En la clase de aplicación que va a tener la funcionalidad de impresión en 3D, agregue el espacio de nombres [**Windows. Graphics. Printing3D**](/uwp/api/Windows.Graphics.Printing3D) .

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

En esta guía se utilizarán los siguientes espacios de nombres adicionales.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

A continuación, asigne a los campos de miembro de la clase una utilidad. Declare un objeto [**Print3DTask**](/uwp/api/Windows.Graphics.Printing3D.Print3DTask) para representar la tarea de impresión que se va a pasar al controlador de impresión. Declare un objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) para que contenga el archivo de datos 3D original que se cargará en la aplicación. Declara un objeto [**Printing3D3MFPackage**](/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage), que representa un modelo 3D listo para imprimir con todos los metadatos necesarios.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Crear una interfaz de usuario sencilla

Este ejemplo incluye tres controles de usuario: un botón cargar que llevará un archivo a la memoria del programa, un botón corregir que modificará el archivo según sea necesario y un botón imprimir que iniciará el trabajo de impresión. En el código siguiente se crean estos botones (con sus controladores de eventos de clic) en la clase. CS ' archivo XAML correspondiente.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Agrega **TextBlock** para comentarios de la interfaz de usuario.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Obtener los datos de 3D


El método con el que la aplicación obtiene datos de geometría 3D variará. La aplicación puede recuperar datos de un examen 3D, descargar datos del modelo desde un recurso Web o generar una malla 3D mediante programación con fórmulas matemáticas o datos proporcionados por el usuario. Para que resulte más sencillo, en esta guía se mostrará cómo cargar un archivo de datos 3D (de cualquiera de los distintos tipos de archivo comunes) en la memoria del programa desde el almacenamiento del dispositivo. La [Biblioteca de modelos de 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) proporciona distintos modelos que puedes descargar fácilmente en el dispositivo.

En el `OnLoadClick` método, use la clase [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para cargar un único archivo en la memoria de la aplicación.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Usar la aplicación 3D Builder para convertir a formato de fabricación 3D (.3mf)

En este punto, es posible cargar un archivo de datos 3D en la memoria de la aplicación. Sin embargo, los datos de geometría 3D pueden proporcionarse en muchos formatos diferentes y no todos son eficaces para la impresión 3D. En Windows 10, se usa el tipo de archivo 3D Manufacturing Format (formato de fabricación 3D) (.3mf) para todas las tareas de impresión 3D.

> [!NOTE]  
> El tipo de archivo. 3MF ofrece más funcionalidad de la que se trata en este tutorial. Para obtener más información sobre 3MF y las características que proporciona a los productores y consumidores de productos 3D, consulte la [especificación 3MF](https://3mf.io/what-is-3mf/3mf-specification/). Para obtener información sobre cómo se usan estas características con las API de Windows 10, vea el tutorial [generación de un paquete 3MF](./generate-3mf.md) .

La aplicación del [generador 3D](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) puede abrir archivos de los formatos 3D más populares y guardarlos como archivos. 3MF. En este ejemplo, donde el tipo de archivo puede variar, una solución muy sencilla consiste en abrir la aplicación de generación 3D y preguntar al usuario que guarde los datos importados como un archivo. 3MF y, a continuación, vuelva a cargarlos.

> [!NOTE]  
> Además de convertir los formatos de archivo, 3D Builder proporciona herramientas sencillas para editar modelos, agregar datos de color y realizar otras operaciones específicas de la impresión, por lo que a menudo vale la pena integrarla en la aplicación que se encargue de la impresión en 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Reparar los datos de modelo de impresión en 3D

No todos los datos de modelo 3D se pueden imprimir, aunque sean del tipo .3mf. Para que la impresora determine correctamente qué espacio se rellenará y qué debe dejarse vacío, los modelos que se hayan de imprimir deben ser cada uno de ellos una sola combinación perfecta, deben tener normales de superficie hacia el exterior y una geometría variada. Los problemas de estas áreas pueden surgir en una gran variedad de formas diferentes y pueden ser difíciles de detectar en formas complejas. Sin embargo, las soluciones de software modernas suelen ser adecuadas para convertir geometrías sin formato en formas 3D imprimibles. Esto se conoce como *reparar* el modelo y se realizará el método `OnFixClick`.

El archivo de datos 3D se debe convertir para implementar [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream), que se puede usar para generar un objeto [**Printing3DModel**](/uwp/api/Windows.Graphics.Printing3D.Printing3DModel).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Ahora, el objeto **Printing3DModel** está reparado y se puede imprimir. Usa [**SaveModelToPackageAsync**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) para asignar el modelo al objeto **Printing3D3MFPackage** que se haya declarado al crear la clase.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Ejecutar la tarea de impresión: crear un controlador de TaskRequested


Más adelante, cuando se muestre el cuadro de diálogo de impresión en 3D al usuario y este elija iniciar la impresión, la aplicación deberá pasar los parámetros deseados a la canalización de impresión en 3D. La API de impresión en 3D generará el evento **[TaskRequested](/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** . Debes escribir un método para controlar este evento correctamente. Como de costumbre, el método del controlador debe ser del mismo tipo que su evento: el evento **TaskRequested** tiene parámetros [**Print3DManager**](/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (una referencia a su objeto remitente) y un objeto [**Print3DTaskRequestedEventArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) que contiene la mayor parte de la información pertinente.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

El propósito principal de este método es usar el parámetro *args* para enviar un objeto **Printing3D3MFPackage** por la canalización. El tipo **Print3DTaskRequestedEventArgs** tiene una propiedad: [**Request**](/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Es del tipo [**Print3DTaskRequest**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) y representa una solicitud de trabajo de impresión. Su método [**CreateTask**](/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) permite al programa enviar la información correcta para el trabajo de impresión y devuelve una referencia al objeto **Print3DTask** que se envió a través de la canalización de impresión 3D.

**CreateTask** tiene los siguientes parámetros de entrada: una cadena para el nombre del trabajo de impresión, una cadena para el identificador de la impresora que se va a usar y un delegado [**Print3DTaskSourceRequestedHandler**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler) . El delegado se invoca automáticamente cuando se produce el evento **3DTaskSourceRequested** (lo hace la propia API). Lo importante a tener en cuenta es que este delegado se invoca cuando se inicia un trabajo de impresión y es el responsable de proporcionar el paquete de impresión 3D correcto.

**Print3DTaskSourceRequestedHandler** toma un parámetro, un objeto [**Print3DTaskSourceRequestedArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) que proporciona los datos que se van a enviar. El único método público de esta clase, [**SetSource**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource), acepta el paquete que se imprimirá. Implementa un delegado **Print3DTaskSourceRequestedHandler** de la siguiente manera.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

A continuación, llama a **CreateTask** mediante el delegado recién definido, `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

El valor devuelto **Print3DTask** se asigna a la variable de clase declarada al principio. Ahora puedes usar (opcionalmente) esta referencia para controlar ciertos eventos producidos por la tarea.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Debes implementar un método `Task_Submitting` y un método `Task_Completed` si quieres registrarlos en estos eventos.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Ejecutar la tarea de impresión: Abrir el cuadro de diálogo de impresión 3D


La parte final del código necesario es la que inicia el cuadro de diálogo de impresión 3D. Al igual que una ventana de diálogo de impresión convencional, el cuadro de diálogo impresión en 3D proporciona varias opciones de impresión de última hora y permite al usuario elegir la impresora que se va a usar (ya sea conectada mediante USB o la red).

Registra tu método `MyTaskRequested` con el evento **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Después de registrar el controlador de eventos **TaskRequested** , puede invocar el método [**ShowPrintUIAsync**](/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync), que abre el cuadro de diálogo de impresión 3D en la ventana de la aplicación actual.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Por último, es conveniente eliminar el registro de los controladores de eventos una vez que la aplicación reanude el control.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Temas relacionados

[Generar un paquete 3MF](./generate-3mf.md)  
[Ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 