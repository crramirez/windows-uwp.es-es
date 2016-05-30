---
author: PatrickFarley
title: Impresión 3D desde la aplicación
description: Aprende a agregar la función de impresión 3D a tu aplicación universal de Windows. En este tema se explica cómo iniciar el cuadro de diálogo de impresión 3D después de asegurarte de que tu modelo 3D se puede imprimir y en el formato correcto.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
---

# Impresión 3D desde la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

Aprende a agregar la función de impresión en 3D a la aplicación universal de Windows. En este tema se explica cómo cargar datos de geometría 3D en la aplicación e iniciar el cuadro de diálogo de impresión en 3D después de asegurarse de que el modelo 3D se puede imprimir y tiene el formato correcto. Para obtener un ejemplo práctico de estos procedimientos en acción, consulta el [ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

## Configuración de clase


En la clase que va a incluir la funcionalidad de impresión en 3D, agrega el espacio de nombres [Windows.Graphics.Printing3D](https://msdn.microsoft.com/library/windows/apps/dn998169).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

En esta guía específica, se usarán los siguientes espacios de nombres adicionales:

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Después, asigna a la clase algunos campos de miembro útiles. Declara un objeto [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) para que funcione como referencia para la tarea de impresión que se pasará al controlador de impresión. Declara un objeto [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) para almacenar el archivo de datos 3D original. Por último, declara un objeto [Printing3D3MFPackage](https://msdn.microsoft.com/library/windows/apps/dn998063), que representa un modelo 3D de listo para imprimir con todos los metadatos necesarios.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## Crear una interfaz de usuario sencilla


En este ejemplo se usan tres controles de usuario: un botón de carga que coloca un archivo en la memoria de programa, un botón de corrección que modifica el archivo según sea necesario y un botón de impresión que inicia el trabajo de impresión. El siguiente código genera estos botones (con sus controladores de eventos de clic) en el archivo XAML de la clase:

[!code-xml[Botones](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Agrega **TextBlock** para comentarios de la interfaz de usuario.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]

## Obtener los datos de 3D


El método con el que la aplicación obtiene datos de geometría 3D para imprimir variará. La aplicación puede recuperar datos de una digitalización 3D, extraer datos de modelo de un recurso web o generar una malla 3D mediante programación a través de ecuaciones. Para que resulte más sencillo, en esta guía se cargará un archivo de datos 3D (de cualquiera de los distintos tipos de archivo comunes) en la memoria del programa desde el Explorador de archivos.

En tu método `OnLoadClick`, usa la clase [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) para cargar un único archivo en la memoria de la aplicación.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## Usar la aplicación 3D Builder para convertir a formato de fabricación 3D (.3mf)

En este punto, es posible cargar un archivo de datos 3D en la memoria de la aplicación. Sin embargo, los datos de geometría 3D tienen muchos formatos diferentes y no todos son eficaces para la impresión 3D. En Windows 10, se usa el tipo de archivo de formato de fabricación 3D (.3mf) para todas las tareas de impresión 3D.

> **Nota**: El tipo de archivo 3MF ofrece una gran cantidad de funciones que no se incluyen en este tutorial. Para obtener más información sobre 3MF y las características que proporciona a los creadores y los consumidores de productos 3D, consulta [3MF Specification](http://3mf.io/what-is-3mf/3mf-specification/) (Especificación 3MF). Para obtener información sobre cómo aprovechar estas características mediante las API de Windows 10, consulta el tutorial [Generar un paquete 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

Afortunadamente, la aplicación [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) puede abrir archivos de los formatos 3D más populares y guardarlos como archivos .3mf. En este ejemplo, donde puede variar el tipo de archivo, una solución muy sencilla es abrir 3D Builder y pedir al usuario que guarde los datos importados como un archivo .3mf y que vuelva a cargarlo.

> **Nota**: Además de convertir los formatos de archivo, **3D Builder** proporciona herramientas sencillas para editar modelos, agregar datos de color y realizar otras operaciones específicas de la impresión, por lo que a menudo vale la pena integrarlo en una aplicación que se encargue de la impresión en 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## Reparar los datos de modelo de impresión en 3D

No todos los datos de modelo 3D se pueden imprimir, aunque tengan el formato .3mf. Para que la impresora determine correctamente qué espacio se rellenará y qué debe dejarse vacío, los modelos que se van a imprimir deben ser una sola combinación perfecta, deben tener normales de superficie hacia el exterior y una geometría variada. Los problemas en estas áreas pueden surgir de diversas formas y pueden ser difíciles de detectar en formas complejas. Afortunadamente, las soluciones actuales de software suelen ser suficientes para convertir la geometría sin procesar en formas 3D imprimibles. Esto se hace con el método `OnFixClick`.

El archivo de datos 3D se debe convertir para implementar [IRandomAccessStream](https://msdn.microsoft.com/library/windows/apps/br241731), que se puede usar para generar un objeto [Printing3DModel](https://msdn.microsoft.com/library/windows/apps/mt203679).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Ahora, el objeto **Printing3DModel** está reparado y se puede imprimir. Usa **SaveModelToPackageAsync** para asignar el modelo al objeto Printing3D3MFPackage que declaraste al crear la clase.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## Ejecutar la tarea de impresión: crear un controlador de TaskRequested


Más adelante, cuando se muestre el cuadro de diálogo de impresión en 3D al usuario y este elija iniciar la impresión, la aplicación deberá pasar los parámetros deseados a la canalización de impresión en 3D. La API de impresión 3D generará el evento **TaskRequested**. Debes escribir un método para controlar este evento correctamente. Como de costumbre, debe ser del mismo tipo que su evento: el evento **TaskRequested** tiene parámetros [Print3DManager](https://msdn.microsoft.com/library/windows/apps/dn998029) (su objeto remitente) y un objeto [Print3DTaskRequestedEventArgs](https://msdn.microsoft.com/library/windows/apps/dn998051) que contiene la mayor parte de la información pertinente. El tipo devuelto es **void**.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

El propósito principal de este método es usar el objeto *args* para enviar **Printing3D3MFPackage** a la canalización. El tipo **Print3DTaskRequestedEventArgs** tiene una propiedad: **Request**. Es del tipo [Print3DTaskRequest](https://msdn.microsoft.com/library/windows/apps/dn998050) y representa una solicitud de trabajo de impresión. Su método **CreateTask** permite que el programa envíe la información correcta al trabajo de impresión y devuelve una referencia al objeto [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) que se envía a la canalización de impresión 3D.

**CreateTask** tiene los siguientes parámetros de entrada: un elemento **string** para el nombre del trabajo de impresión, un elemento **string** para el identificador de la impresora que se usará y un delegado **Print3DTaskSourceRequestedHandler**. El delegado se invoca automáticamente cuando se produce el evento **3DTaskSourceRequested** (lo hace la propia API). Lo importante a tener en cuenta es que este delegado se invoca cuando se inicia un trabajo de impresión y es el responsable de proporcionar el paquete de impresión 3D correcto.

**Print3DTaskSourceRequestedHandler** usa un parámetro, un objeto [Print3DTaskSourceRequestedArgs](https://msdn.microsoft.com/library/windows/apps/dn998056) que proporciona los datos que se enviarán. El único método público de esta clase, **SetSource**, acepta el paquete que se va a imprimir. Implementa un delegado **Print3DTaskSourceRequestedHandler** de la siguiente manera:

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

A continuación, llama a **CreateTask** mediante el delegado recién definido `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

El valor devuelto **Print3DTask** se asigna a la variable de clase declarada al principio. Ahora puedes usar (opcionalmente) esta referencia para controlar ciertos eventos que inició la tarea:

[!code-cs[Opcional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> **Nota**: Debes implementar los métodos `Task_Submitting` y `Task_Completed` si quieres registrarlos en estos eventos.

## Ejecutar la tarea de impresión: abrir el cuadro de diálogo de impresión 3D


La parte final del código necesario para imprimir desde tu aplicación es la que inicia el cuadro de diálogo de impresión 3D. Al igual que una ventana del cuadro de diálogo de impresión convencional, el cuadro de diálogo de impresión 3D proporciona una serie de especificaciones de impresión de última hora y permite que el usuario elija la impresora que se va a usar (si está conectada a través de USB o la red).

Primero, registra tu método `MyTaskRequested` con el evento **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Después de registrar el controlador de eventos **TaskRequested**, puedes invocar el método **ShowPrintUIAsync**, que abre el cuadro de diálogo de impresión en 3D en la ventana de la aplicación actual.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Por último, es conveniente eliminar el registro de los controladores de eventos una vez que la aplicación reanude el control: [!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## Temas relacionados

[Generar un paquete 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)

[Ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 






<!--HONumber=May16_HO2-->


