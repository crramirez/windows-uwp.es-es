---
author: serenaz
title: Cómo formar un modelo para Windows ML en Visual Studio
description: Aprende a formar un modelo para Windows ML con Visual Studio Tools for AI con este tutorial paso a paso.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, aprendizaje automático de Windows, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 1b54b0665a2483b8a0be710f505e928c852f4dba
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842672"
---
# <a name="how-to-train-a-model-for-windows-ml-in-visual-studio"></a>Cómo formar un modelo para Windows ML en Visual Studio

En este tutorial, usaremos [Visual Studio Tools for AI](http://aka.ms/vstoolsforai), una extensión de desarrollo para crear, probar e implementar soluciones de Deep Learning y AI, para formar un modelo para la aplicación de muestra MNIST en [Introducción](get-started.md).

Formaremos el modelo con el marco [Microsoft Cognitive Toolkit (CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit) y el [conjunto de datos MNIST](http://yann.lecun.com/exdb/mnist/), que tiene un conjunto de entrenamiento de 60.000 ejemplos y un conjunto de pruebas de 10.000 ejemplos de dígitos manuscritos. Guardaremos, a continuación, el modelo usando el formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/) para usarlo con Windows ML.

## <a name="prerequisites"></a>Requisitos previos
### <a name="install-visual-studio-tools-for-ai"></a>Instalar Visual Studio Tools for AI
Para empezar, tendrás que descargar e instalar [Visual Studio](https://www.visualstudio.com/downloads/). Una vez abierto Visual Studio, activa la extensión **Visual Studio Tools for AI**:

1. Haz clic en la barra de menús en Visual Studio and selecciona "Extensiones y actualizaciones..."
2. Haz clic en la pestaña "En línea" y selecciona "Buscar en Visual Studio Marketplace".
3. Busca "Visual Studio Tools for AI." 
3. Haz clic en el botón **Descargar**. 
4. Después de la instalación, reinicia Visual Studio. 

La extensión se activará una vez que se reinicie Visual Studio. Si estás teniendo problemas, echa un vistazo a [Buscar extensiones de Visual Studio](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions).

### <a name="download-sample-code"></a>Descargar ejemplo de código
Descarga el repositorio [Muestras para AI](https://github.com/Microsoft/samples-for-ai) en GitHub. Los ejemplos cubren tareas iniciales con aprendizaje profundo en TensorFlow, CNTK, Theano y mucho más.

### <a name="install-cntk"></a>Instalar CNTK
Instala [CNTK para Python en Windows](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-windows-python?tabs=cntkpy24). Ten en cuenta que también tendrás que instalar Python si no lo has hecho.

Como alternativa, para preparar el equipo para el desarrollo de modelos de aprendizaje profundo, consulta [Preparación del entorno de desarrollo](https://github.com/Microsoft/samples-for-ai/blob/master/README.md) para un instalador simplificado para instalar Python, CNTK, TensorFlow, controladores GPU NVIDIA (opcional) y mucho más.

## <a name="1-open-project"></a>1. Abrir el proyecto

Inicia Visual Studio y selecciona **Archivo > Abrir > Proyecto/Solución**. En las muestras de repositorio de AI, selecciona la carpeta **examples\cntk\python** y abre el archivo **CNTKPythonExamples.sln**.

![Abrir la solución](images/open-solution.png)

## <a name="2-train-the-model"></a>2. Formar el modelo

Para establecer el proyecto MNIST como proyecto de inicio, haz clic con el botón derecho en el nodo proyecto de python y selecciona **Establecer como proyecto de inicio**.

![Abrir la solución](images/mnist-startup.png)

A continuación, abre el archivo train_mnist_onnx.py y **ejecuta** el proyecto presionando F5 o el botón de ejecución verde.

## <a name="3-view-the-model-and-add-it-to-your-app"></a>3. Ver el modelo y agregarlo a la aplicación

Ahora, el archivo de modelo **mnist.onnx** formado debe estar en la carpeta samples-for-ai/examples/cntk/python/MNIST. Puedes usar este archivo de modelo **mnist.onnx** formado para compilar la aplicación de muestra MNIST en [Introducción](get-started.md). 

## <a name="4-learn-more"></a>4. Más información
Para obtener información sobre cómo acelerar la formación de modelos de aprendizaje profundo con [Máquinas virtuales de GPU de Azure](https://docs.microsoft.com/en-us/visualstudio/ai/tensorflow-vm) y más, visita [Inteligencia Artificial en Microsoft](https://www.microsoft.com/ai) y [Tecnologías de aprendizaje de máquina de Microsoft](https://docs.microsoft.com/en-us/azure/machine-learning/#More-Microsoft-Machine-Learning-Technologies).