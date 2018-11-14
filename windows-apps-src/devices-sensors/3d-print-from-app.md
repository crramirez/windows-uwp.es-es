---
author: PatrickFarley
title: Impresión 3D desde la aplicación
description: Aprende a agregar la función de impresión en 3D a tu aplicación universal de Windows. En este tema se explica cómo iniciar el cuadro de diálogo de impresión en 3D después de asegurarte de que el modelo 3D se puede imprimir y tiene el formato correcto.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 3dprinting, impresión en 3d
ms.localizationpriority: medium
ms.openlocfilehash: 818e1338a1d36d24990f22316dc2072c2c0d7cc5
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6253297"
---
# <a name="3d-printing-from-your-app"></a>Impresión en 3D desde la aplicación

**API importantes**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

Aprende a agregar la función de impresión en 3D a tu aplicación universal de Windows. En este tema se explica cómo cargar datos de geometría 3D en la aplicación e iniciar el cuadro de diálogo de impresión en 3D después de asegurarse de que el modelo 3D se puede imprimir y tiene el formato correcto. Para obtener un ejemplo práctico de estos procedimientos, consulta el [ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> En el código de ejemplo de esta guía, los informes y el control de errores se han simplificado bastante por motivos de simplicidad.

## <a name="setup"></a>Instalación


En la clase de aplicación que debe incluir la funcionalidad de impresión en 3D, agrega el espacio de nombres [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

En esta guía, se usarán los siguientes espacios de nombres adicionales.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Después, asigna a la clase campos de miembro útiles. Declara un objeto [**Print3DTask**](https://msdn.microsoft.com/library/windows/apps/dn998044) para que represente la tarea de impresión que se pasará al controlador de impresión. Declara un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) para que contenga el archivo de datos 3D original que debe cargarse en la app. Declara un objeto [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/dn998063), que representa un modelo 3D listo para imprimir con todos los metadatos necesarios.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Crear una interfaz de usuario sencilla

En este ejemplo se incluyen tres controles de usuario: un botón de carga que colocará un archivo en la memoria de programa, un botón de corrección que modificará el archivo según sea necesario y un botón de impresión que iniciará el trabajo de impresión. El siguiente código crea estos botones (con sus controladores de eventos al hacer clic) en el archivo XAML de tu correspondiente clase .cs.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Agrega **TextBlock** para comentarios de la interfaz de usuario.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Obtener los datos de 3D


El método con el que la aplicación obtiene datos de geometría 3D variará. La aplicación puede recuperar datos de una digitalización 3D, descargar datos de modelo de un recurso web o generar una malla 3D mediante programación a través de fórmulas matemáticas o entradas del usuario. Para que resulte más sencillo, en esta guía se mostrará cómo cargar un archivo de datos 3D (de cualquiera de los distintos tipos de archivo comunes) en la memoria del programa desde el almacenamiento del dispositivo. La [Biblioteca de modelos de 3D Builder](https://developer.microsoft.com/windows/hardware/3d-builder-model-library) proporciona distintos modelos que puedes descargar fácilmente en el dispositivo.

En tu método `OnLoadClick`, usa la clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para cargar un único archivo en la memoria de la aplicación.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Usar la aplicación 3D Builder para convertir a formato de fabricación 3D (.3mf)

En este punto, es posible cargar un archivo de datos 3D en la memoria de la aplicación. Sin embargo, los datos de geometría 3D pueden proporcionarse en muchos formatos diferentes y no todos son eficaces para la impresión 3D. En Windows10, se usa el tipo de archivo 3D Manufacturing Format (formato de fabricación 3D) (.3mf) para todas las tareas de impresión 3D.

> [!NOTE]  
> El tipo de archivo .3mf ofrece más funciones de las que se incluyen en este tutorial. Para obtener más información sobre 3MF y las características que proporciona a los creadores y los consumidores de productos 3D, consulta [3MF Specification](http://3mf.io/what-is-3mf/3mf-specification/) (Especificación 3MF). Para obtener información sobre cómo usar estas características mediante las API de Windows 10, consulta el tutorial [Generar un paquete 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

La aplicación [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) puede abrir archivos de los formatos 3D más populares y guardarlos como archivos .3mf. En este ejemplo, donde el tipo de archivo puede variar, una solución muy sencilla es abrir la aplicación 3D Builder y pedir al usuario que guarde los datos importados como un archivo .3mf y que vuelva a cargarlo.

> [!NOTE]  
> Además de convertir los formatos de archivo, 3D Builder proporciona herramientas sencillas para editar modelos, agregar datos de color y realizar otras operaciones específicas de la impresión, por lo que a menudo vale la pena integrarla en la aplicación que se encargue de la impresión en 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Reparar los datos de modelo de impresión en 3D

No todos los datos de modelo 3D se pueden imprimir, aunque sean del tipo .3mf. Para que la impresora determine correctamente qué espacio se rellenará y qué debe dejarse vacío, los modelos que se hayan de imprimir deben ser cada uno de ellos una sola combinación perfecta, deben tener normales de superficie hacia el exterior y una geometría variada. Los problemas en estas áreas pueden surgir de diversas formas y pueden ser difíciles de detectar en formas complejas. Sin embargo, las soluciones contemporáneas de software suelen ser suficientes para convertir la geometría sin procesar en formas 3D imprimibles. Esto se conoce como *reparar* el modelo y se realizará el método `OnFixClick`.

El archivo de datos 3D se debe convertir para implementar [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731), que se puede usar para generar un objeto [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/mt203679).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Ahora, el objeto **Printing3DModel** está reparado y se puede imprimir. Usa [**SaveModelToPackageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) para asignar el modelo al objeto **Printing3D3MFPackage** que se haya declarado al crear la clase.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Ejecutar la tarea de impresión: crear un controlador de TaskRequested


Más adelante, cuando se muestre el cuadro de diálogo de impresión en 3D al usuario y este elija iniciar la impresión, la aplicación deberá pasar los parámetros deseados a la canalización de impresión en 3D. La API de impresión 3D generará el evento **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)**. Debes escribir un método para controlar este evento correctamente. Como de costumbre, el método del controlador debe ser del mismo tipo que su evento: el evento **TaskRequested** tiene parámetros [**Print3DManager**](https://msdn.microsoft.com/library/windows/apps/dn998029) (una referencia a su objeto remitente) y un objeto [**Print3DTaskRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn998051) que contiene la mayor parte de la información pertinente.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

El propósito principal de este método es usar el parámetro *args* para enviar un objeto **Printing3D3MFPackage** por la canalización. El tipo **Print3DTaskRequestedEventArgs** tiene una propiedad: [**Request**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequestedeventargs.request.aspx). Es del tipo [**Print3DTaskRequest**](https://msdn.microsoft.com/library/windows/apps/dn998050) y representa una solicitud de trabajo de impresión. Su método [**CreateTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequest.createtask.aspx) permite al programa enviar la información correcta respecto al trabajo de impresión y devuelve una referencia al objeto **Print3DTask** que se haya enviado a la canalización de impresión 3D.

**CreateTask** tiene los siguientes parámetros de entrada: Un elemento string para el nombre del trabajo de impresión, un elemento string para el identificador de la impresora que se debe utilizar y un delegado [**Print3DTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedhandler.aspx). El delegado se invoca automáticamente cuando se produce el evento **3DTaskSourceRequested** (lo hace la propia API). Lo importante a tener en cuenta es que este delegado se invoca cuando se inicia un trabajo de impresión y es el responsable de proporcionar el paquete de impresión 3D correcto.

**Print3DTaskSourceRequestedHandler** usa un parámetro, un objeto [**Print3DTaskSourceRequestedArgs**](https://msdn.microsoft.com/library/windows/apps/dn998056) que proporciona los datos que se enviarán. El único método público de esta clase, [**SetSource**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource.aspx), acepta el paquete que se imprimirá. Implementa un delegado **Print3DTaskSourceRequestedHandler** de la siguiente manera.

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

Después de registrar el controlador de eventos **TaskRequested**, puedes invocar el método [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dmanager.showprintuiasync.aspx), que abre el cuadro de diálogo de impresión en 3D en la ventana de la aplicación actual.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Por último, es conveniente eliminar el registro de los controladores de eventos una vez que la aplicación reanude el control.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Artículos relacionados

[Generar un paquete 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[Ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
