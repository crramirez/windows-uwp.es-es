---
author: serenaz
title: Integrar un modelo en la aplicación con Windows ML
description: Integra un modelo en tu aplicación siguiendo el patrón cargar, enlazar y evaluar.
ms.author: sezhen
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, winml, aprendizaje automático de Windows
ms.localizationpriority: medium
ms.openlocfilehash: 8631c07b45a8642a1de700e424d3ca4f147456fc
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842582"
---
# <a name="integrate-a-model-into-your-app-with-windows-ml"></a>Integrar un modelo en la aplicación con Windows ML

La [generación automática de código](overview.md#automatic-interface-code-generation) de Windows ML crea una interfaz que llama a las [API de Windows ML](/uwp/api/windows.ai.machinelearning.preview), lo que te permite interactuar fácilmente con tu modelo. Usando las clases la interfaz generado clases de contenedor generadas, seguirás el patrón de cargar, enlazar y evaluar para integrar tu modelo ML en tu aplicación.

![cargar, enlazar, evaluar](images/load-bind-evaluate.png)

En este artículo, usaremos el modelo MNIST desde [Introducción](get-started.md) para demostrar cómo cargar, enlazar y evaluar un modelo en tu aplicación.

## <a name="add-the-model"></a>Agregar el modelo

En primer lugar, tendrás que agregar tu modelo ONNX a los activos de tu proyecto de Visual Studio. Si vas a crear una aplicación para UWP con [Visual Studio (versión 15.7 - Preview 1)](https://www.visualstudio.com/vs/preview/), a continuación, Visual Studio generará automáticamente las clases de contenedor en un archivo nuevo. Para otros flujos de trabajo, deberás usar la herramienta [mlgen](overview.md#automatic-interface-code-generation) para generar clases de contenedor.

A continuación se incluyen las clases de contenedor de Windows ML para el modelo MNIST. Usaremos el resto de este artículo para explicar cómo usar estas clases en la aplicación.

```csharp
public sealed class MNISTModelInput
{
    public VideoFrame Input3 { get; set; }
}

public sealed class MNISTModelOutput
{
    public IList<float> Plus214_Output_0 { get; set; }
    public MNISTModelOutput()
    {
        this.Plus214_Output_0 = new List<float>();
    }
}

public sealed class MNISTModel
{
    private LearningModelPreview learningModel;
    public static async Task<MNISTModel> CreateMNISTModel(StorageFile file)
    {
        LearningModelPreview learningModel = await LearningModelPreview.LoadModelFromStorageFileAsync(file);
        MNISTModel model = new MNISTModel();
        model.learningModel = learningModel;
        return model;
    }
    public async Task<MNISTModelOutput> EvaluateAsync(MNISTModelInput input) {
        MNISTModelOutput output = new MNISTModelOutput();
        LearningModelBindingPreview binding = new LearningModelBindingPreview(learningModel);
        binding.Bind("Input3", input.Input3);
        binding.Bind("Plus214_Output_0", output.Plus214_Output_0);
        LearningModelEvaluationResultPreview evalResult = await learningModel.EvaluateAsync(binding, string.Empty);
        return output;
    }
}
```

**Nota**: Para asegurarte de que tu modelo se genera cuando compilas tu aplicación, haz clic con el botón derecho en el archivo `.onnx` y selecciona **Propiedades**. Para **Acción de generación**, selecciona **Contenido**.

## <a name="load"></a>Load

Una vez que hayas generado las clases de contenedor, podrás cargar el modelo en tu aplicación.

La clase MNISTModel representa el modelo MNIST, y para cargar el modelo, llamamos al método CreateMNISTModel, pasando el archivo ONNX como parámetro.

```csharp
// Load the model
StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
MNISTModel model = MNISTModel.CreateMNISTModel(modelFile);
```

## <a name="bind"></a>Enlazar

El código generado también incluye clases de contenedor Input y Output. La clase Input representa las entradas esperados del modelo y la clase Output representa las salidas esperados del modelo.

Para inicializar el objeto de entrada del modelo, llame al constructor de clase Input, pasando los datos de aplicación y asegúrate de que los datos de entrada coinciden con el tipo de entrada que el modelo espera. La clase MNISTModelInput espera un VideoFrame, por lo que usamos un método auxiliar para obtener un VideoFrame para la entrada.

```csharp
//Bind the input data to the model
MNISTModelInputs ModelInput = new MNISTModelInputs();
ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
```

Los objetos de salida se inicializan en valores *Null* y se actualizarán con los resultados del modelo después del siguiente paso, Evaluar.

## <a name="evaluate"></a>Evaluar

Una vez que se hayan inicializado sus entradas, llame al método EvaluateAsync del modelo para evaluar el modelo en los datos de entrada. EvaluateAsync enlaza sus entradas y salidas al objeto del modelo y evalúa el modelo en las entradas.

```csharp
// Evaluate the model
MNISTModelOuput ModelOutput = model.EvaluateAsync(ModelInput);
```

Después de la evaluación, la salida contiene los resultados del modelo, que ahora puedes ver y analizar. Dado que el modelo MNIST genera una lista de probabilidades, realizamos la iteración a través de la lista para encontrar y mostrar el dígito con la probabilidad más alta.

```csharp
 //Iterate through output to determine highest probability digit
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
```

## <a name="using-the-windows-ml-apis-directly"></a>Usar directamente las API de Windows ML

Aunque el generador de código automático de Windows ML proporciona una interfaz apropiada para interactuar con el modelo, no es necesario usar las clases de contenedor. En su lugar, puedes llamar a las API de Windows ML directamente en tu aplicación.
Si decides hacerlo, seguirás el mismo patrón: cargar tu modelo, enlazar tus entradas y salidas y evaluar.

Para obtener más información sobre cómo usar las API, consulta la [referencia de API de Windows ML](/uwp/api/windows.ai.machinelearning.preview).
