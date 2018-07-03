---
author: serenaz
title: Introducción a Windows ML
description: Crea tu primera aplicación de Windows ML con este tutorial paso a paso.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows aprendizaje automático, winml, windows ML
ms.localizationpriority: medium
ms.openlocfilehash: eec2ada8e3aadad134381a93bca2652133912b2e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843628"
---
# <a name="get-started-with-windows-ml"></a>Introducción a Windows ML

En este tutorial, pasaremos a crear una sencilla aplicación para UWP que usa un modelo de aprendizaje automático para reconocer un dígito numérico dibujado por el usuario. Este tutorial se centra principalmente en cómo cargar y usar Windows ML en tu aplicación.

## <a name="prerequisites"></a>Requisitos previos

- [SDK de Windows10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) (Compilación 17110 o posterior)
- [Visual Studio](https://developer.microsoft.com/windows/downloads)

## <a name="1-download-the-sample"></a>1. Descargar la muestra

En primer lugar, tendrás que descargar nuestra [muestra MNIST_GetStarted](https://github.com/Microsoft/Windows-Machine-Learning) desde GitHub. Hemos proporcionado una plantilla con controles y eventos XAML implementados, que incluyen:

- Un [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) para dibujar el dígito.
- [Botones](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button) para interpretar el dígito y borrar el canvas.
- Rutinas auxiliares para convertir la salida de InkCanvas en un [VideoFrame](https://docs.microsoft.com/uwp/api/windows.media.videoframe).

Un ejemplo MNIST completo también está disponible para su descarga desde GitHub.

## <a name="2-open-project-in-visual-studio-preview"></a>2. Abrir el proyecto en Visual Studio Preview

Inicia Visual Studio Preview y abre la aplicación de muestra MNIST. (Si la solución se muestra como no disponible, tendrás que hacer clic con el botón derecho y seleccionar "Volver a cargar el proyecto".)

En el explorador de soluciones, el proyecto tiene tres archivos de código principales:

- `MainPage.xaml` -Todo nuestro código XAML para crear la interfaz de usuario para el InkCanvas, botones y etiquetas.
- `MainPage.xaml.cs` - Lugar en el que reside el código de la aplicación.
- `Helper.cs` - Rutinas auxiliares para recortar y convertir formatos de imagen.

![Explorador de soluciones de Visual Studio con archivos del proyecto](images/get-started1.png)

## <a name="3-build-and-run-project"></a>3. Compilar y ejecutar el proyecto

En la barra de herramientas de Visual Studio, cambia la plataforma de soluciones de "ARM" a "x64" para ejecutar el proyecto en el equipo local.

Para ejecutar el proyecto, haz clic en el botón **Iniciar depuración** en la barra de herramientas o pulsa F5. La aplicación debe mostrar un InkCanvas donde los usuarios pueden escribir un dígito, un botón "Reconocer" para interpretar el número, un campo de etiqueta vacío donde se mostrará el dígito interpretado como texto y un botón "Borrar dígito" para borrar el InkCanvas.

![Captura de pantalla de la aplicación](images/get-started2.png)

**Nota**: Si has descargado una versión superior a 17110, es posible que debas cambiar la versión de destino de implementación del proyecto. En la solución de proyecto, ve a **Propiedades**. En la pestaña Aplicación, establece la versión de destino para que coincida con el sistema operativo y el SDK.

## <a name="4-download-a-model"></a>4. Descargar un modelo

A continuación, vamos a obtener un modelo de aprendizaje automático para agregarlo a nuestra aplicación. Para este tutorial, usaremos un **modelo MNIST** previamente formado con el [Kit de herramientas de aprendizaje de Microsoft (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) y [exportado al formato ONNX](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb).

Si usas la muestra de MNIST_GetStarted desde GitHub, el modelo de MNIST ya se ha incluido en la carpeta Assets y tendrás que agregarla a la aplicación como un elemento existente. También puedes descargar el modelo previamente formado desde [ONNX Model Zoo](https://github.com/onnx/models) en GitHub.

Si estás interesado en formar tu propio modelo, puedes seguir este [tutorial](train-ai-model.md) que usamos para formar este modelo MNIST.

## <a name="5-add-the-model"></a>5. Agregar el modelo

Después de descargar el modelo MNIST, haz clic con el botón derecho en la carpeta Assets en el Explorador de soluciones y selecciona "**Agregar** > **elemento existente**". Señala con el selector de archivos a la ubicación de su modelo ONNX y haz clic en Agregar.

El proyecto ahora debería tener dos archivos nuevos:

- `MNIST.onnx` - tu modelo formado.
- `MNIST.cs` - el código generado por Windows ML.

![Explorador de soluciones con nuevos archivos](images/get-started3.png)

Para asegurarte de que el modelo se genera cuando se compila nuestra aplicación, haz clic con el botón derecho en el archivo `mnist.onnx` y selecciona **Propiedades**. Para **Acción de generación**, selecciona **Contenido**.

Ahora, veamos el código recién generado en el archivo `MNIST.cs`. Tenemos tres clases:

- **MNISTModel** crea el modelo de aprendizaje automático, enlaza las entradas específicas y genera la salida del modelo y evalúa el modelo de forma asincrónica. 
- **MNISTModelInput** inicializa los tipos de entrada que el modelo espera. En este caso, la entrada espera un VideoFrame.
- **MNISTModelOutput** inicializa los tipos que el modelo generará. En este caso, el resultado será una lista denominada "classLabel" de tipo `<long>` y un diccionario denominado "predicción" de tipo `<long, float>`

Ahora usaremos estas clases para cargar, enlazar y evaluar el modelo en nuestro proyecto.

## <a name="6-load-bind-and-evaluate-the-model"></a>6. Cargar, enlazar y evaluar el modelo

Para las aplicaciones de Windows ML, el patrón que queremos seguir es: Cargar > Enlazar > Evaluar.

- Carga el modelo de aprendizaje automático.
- Enlaza entradas y salidas para el modelo.
- Evalúa el modelo y muestra los resultados.

Vamos a usar el código de interfaz generado en `MNIST.cs` para cargar, enlazar y evaluar el modelo en nuestra aplicación.

Primero, en `MainPage.xaml.cs`, vamos a crear una instancia del modelo, entradas y salidas.

```csharp
namespace MNIST_Demo
{
    public sealed partial class MainPage : Page
    {
        private MNISTModel ModelGen = new MNISTModel();
        private MNISTModelInput ModelInput = new MNISTModelInput();
        private MNISTModelOutput ModelOutput = new MNISTModelOutput();
        ...
    }
}
```

A continuación, en LoadModel(), cargaremos el modelo.

```csharp
private async void LoadModel()
{
    //Load a machine learning model
    StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
    ModelGen = await MNISTModel.CreateMNISTModel(modelFile);
}
```

A continuación, queremos enlazar nuestras entradas y salidas al modelo. 

En este caso, nuestro modelo espera una entrada del tipo VideoFrame. Utilizando nuestras funciones auxiliares incluidas en `helper.cs`, copiaremos el contenido de InkCanvas, lo convertiremos al tipo VideoFrame y lo enlazaremos a nuestro modelo.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
     //Bind model input with contents from InkCanvas
     ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
}
```

Para la salida, simplemente llamamos EvaluateAsync() con la entrada especificada. Dado que el modelo devuelve una lista de dígitos con una probabilidad correspondiente, necesitamos analizar la lista devuelta para determinar qué dígito tenía la probabilidad más alta y lo mostraremos.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
    //Bind model input with contents from InkCanvas
    ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
    
    //Evaluate the model
    ModelOutput = await ModelGen.EvaluateAsync(ModelInput);
            
    //Iterate through evaluation output to determine highest probability digit
    float maxProb = 0;
    int maxIndex = 0;
    for (int i = 0; i < 10; i++)
    {
        if (ModelOutput.Plus214_Output_0[i] > maxProb)
        {
            maxIndex = i;
            maxProb = ModelOutput.Plus214_Output_0[i];
        }
    }
    numberLabel.Text = maxIndex.ToString();
}
```

Por último, borraremos el InkCanvas para permitir que los usuarios puedan dibujar otro número.

```csharp
private void clearButton_Click(object sender, RoutedEventArgs e)
{
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    numberLabel.Text = "";
}
```

## <a name="7-launch-the-app"></a>7. Iniciar la aplicación

Una vez que compilemos e iniciemos la aplicación, podremos reconocer un número dibujado en InkCanvas.

![completar aplicación](images/get-started4.png)

## <a name="8-next-steps"></a>8. Pasos siguientes

Eso es todo, has creado tu primera aplicación de Windows ML. Para obtener más ejemplos que muestran cómo usar Windows ML, echa un vistazo a [Aplicaciones de muestra](samples.md).