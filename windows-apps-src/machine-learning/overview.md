---
author: serenaz
title: Introducción a Windows ML
description: Obtén información sobre aprendizaje automático de Windows y cómo desarrollar con Windows ML.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, aprendizaje automático de windows
ms.localizationpriority: medium
ms.openlocfilehash: a2470cff6b5c7f07c720a38d0bff00486e4c6b27
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842875"
---
# <a name="windows-ml-overview"></a>Introducción a Windows ML

![gráfico de aprendizaje automático de Windows](images/brain.png)

## <a name="what-is-machine-learning"></a>¿Qué es el aprendizaje automático?

Aprendizaje automático (ML) permite a los equipos a usar los datos existentes para predecir los comportamientos y los resultados esperados. Mediante el procesamiento de los datos recopilados previamente, los modelos de compilación de algoritmos de ML pueden predecir el resultado correcto cuando se presenten con una entrada nueva. Por ejemplo, un modelo puede aprender a evaluar los mensajes de correo electrónico (entrada) como spam o no spam (salida).

La fase de creación del modelo se denomina "aprendizaje". Una vez formado con los datos existentes, el modelo puede realizar predicciones con datos nuevos, nunca antes vistos datos, que se llama "inferencia", "evaluación" o "puntuación".

A menudo, los modelos formados generan mejores resultados que los programas escritos para seguir un conjunto estricto de instrucciones, especialmente para tareas complejas con muchas combinaciones posibles de entradas y salidas. Por ejemplo, los algoritmos de recomendación ofrecen recomendaciones personalizadas para millones de usuarios de comercio electrónico y sitios de streaming de multimedia que serían prácticamente imposibles sin aprendizaje automático. Otro campo que aprovecha el aprendizaje automático es la visión de los equipos, que permite a los equipos clasificar e identificar imágenes después del aprendizaje en imágenes etiquetadas anteriormente.

Las posibilidades y aplicaciones del aprendizaje automático son infinitas. Para obtener más información sobre la investigación y soluciones, visita [inteligencia artificial en Microsoft](https://www.microsoft.com/ai) y [Plataforma AI de Microsoft](https://azure.microsoft.com/en-us/overview/ai-platform/). Si deseas compilar modelos de AI y aprendizaje automático, también puede comprobar [Servicios de aprendizaje automático de Azure](https://docs.microsoft.com/azure/machine-learning/preview/overview-what-is-azure-ml).

## <a name="what-is-windows-ml"></a>¿Qué es Windows ML?

Windows ML es una plataforma que evalúa los modelos de aprendizaje automático formados en dispositivos con Windows 10, lo cual permite a los desarrolladores usar el aprendizaje automático dentro de las aplicaciones de Windows.

Algunos aspectos importantes de Windows ML incluyen:

- **Aceleración de hardware**
    
    En los dispositivos compatibles con DirectX12, Windows ML acelera la evaluación de modelos de Deep Learning con la GPU. Las optimizaciones de CPU además posibilitan la evaluación de alto rendimiento de algoritmos de ML clásicos y Deep Learning.

- **Evaluación local**

    Windows ML evalúa en hardware local, eliminando problemas de conectividad, ancho de banda y privacidad de datos. La evaluación local también posibilita baja latencia y alto rendimiento para obtener resultados de evaluación rápida.

- **Procesamiento de imágenes**

    Para escenarios de visión del equipo, Windows ML simplifica y optimiza el uso de datos de imágenes, vídeo y cámara controlando el procesamiento previo de fotogramas y proporcionar la configuración de la canalización de cámara para la entrada del modelo.

## <a name="how-to-develop-with-windows-ml"></a>Cómo desarrollar con Windows ML

> [!VIDEO https://www.youtube.com/embed/8MCDSlm326U]

### <a name="system-requirements"></a>Requisitos del sistema

Para compilar aplicaciones que usan Windows ML, necesitarás [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) - Compilación 17110 o posterior

### <a name="onnx-models"></a>Modelos ONNX

Para usar Windows ML, necesitarás un modelo de aprendizaje automático previamente formado con el formato [Open Neural Network Exchange (ONNX)](https://onnx.ai). ML Windows admite la versión 1.0 del formato ONNX, que permite a los desarrolladores usar modelos producidos por marcos de aprendizaje diferentes.

Para obtener una lista de los modelos ONNX disponibles públicamente, consulta [Modelos ONNX](https://github.com/onnx/models) en GitHub.

Para aprender a formar un modelo ONNX con Visual Studio Tools for AI, consulta [Formar un modelo](train-ai-model.md).

### <a name="convert-existing-models-to-onnx"></a>Convertir modelos existentes en ONNX

Muchos marcos de aprendizaje ya admiten originalmente modelos ONNX y hay herramientas de convertidor para muchos marcos y bibliotecas. Para obtener información sobre cómo exportar desde marcos como Caffe 2, PyTorch, CNTK, Chainer y mucho más, consulta [Tutoriales ONNX](https://github.com/onnx/tutorials) en GitHub.

También puedes usar [WinMLTools](https://pypi.org/project/winmltools/) para convertir el modelo de aprendizaje automático formado en el formato ONNX aceptado por Windows ML. WinMLTools admite la conversión de los siguientes formatos:

- Core ML
- Scikit-Learn
- XGBoost
- LibSVM

Para aprender a instalar y usar WinMLTools, consulta [Convertir un modelo](conversion-samples.md).

Con la extensión Visual Studio Tools for AI también puedes usar WinMLTools dentro del IDE de Visual Studio para una experiencia más sencilla mediante clics para convertir tus modelos en formato ONNX. Para obtener más información, visita [Herramientas VS para AI](https://github.com/Microsoft/vs-tools-for-ai/).

### <a name="onnx-operators"></a>Operadores de ONNX

Windows ML admite más de 100 operadores de ONNX en la CPU y acelera el cálculo en GPU compatibles con DirectX12. Para obtener una lista completa de firmas de operador, consulta la documentación de esquemas de operadores de ONNX para los espacios de nombres [ai.onnx](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators.md) (predeterminado) y [ai.onnx.ml](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators-ml.md).

Windows ML admite todos los operadores definidos en la documentación de v1.0 ONNX con las siguientes diferencias:

- Operadores marcados como "experimental" admitidos por Windows ML:
    - Affine
    - Crop
    - FC
    - Identity
    - ImageScaler
    - MeanVarianceNormalization
    - ParametricSoftplus
    - ScaledTanh
    - ThresholdedRelu
    - Upsample
- MatMul - mayor que la multiplicación de matrices 2D no es compatible actualmente, solo se admite en la CPU
- Cast - solo admitido en CPU
- Por el momento no se admiten los siguientes operadores:
    - RandomUniform
    - RandomUniformLike
    - RandomNormal
    - RandomNormalLike

### <a name="automatic-interface-code-generation"></a>Generación automática de código de interfaz

Con un archivo de modelo ONNX, el generador de código de Windows ML crea una interfaz para interactuar con el modelo en la aplicación. La interfaz generada incluye clases de contenedor que representan el modelo, las entradas y las salidas. El código generado llama a la [API de Windows ML](/uwp/api/windows.ai.machinelearning.preview), lo que permite cargar fácilmente, enlazar y evaluar el modelo en el proyecto. Actualmente, el generador de código admite C# y C++/CX.

Para los desarrolladores de UWP, el generador de código automático de Windows ML está integrado originalmente con [Visual Studio](https://developer.microsoft.com/windows/downloads). Dentro de tu proyecto de Visual Studio, solo tienes que agregar el archivo ONNX como un elemento existente y VS generará clases contenedoras de Windows ML en un nuevo archivo de interfaz.

También puedes usar la herramienta de línea de comandos `mlgen.exe`, que se incluye con Windows SDK para generar las clases de contenedor de Windows ML. La herramienta se encuentra en `(SDK_root)\bin\<version>\x64` o `(SDK_root)\bin\<version>\x86`, donde SDK_root es el directorio de instalación de SDK. Para ejecutar la herramienta, usa el comando que se incluye a continuación.

```
mlgen -i INPUT-FILE -l LANGUAGE -n NAMESPACE [-o OUTPUT-FILE]
```

Definición de parámetros de entrada:

- `INPUT-FILE`: el archivo de modelo de ONNX
- `LANGUAGE`: CPPCX o CS
- `NAMESPACE`: el espacio de nombres del código generado
- `OUTPUT-FILE`: la ruta de acceso en la que se escribirá el código generado. Si no se especifica el ARCHIVO DE SALIDA, el código generado se escribe en la salida estándar

Para obtener información sobre cómo usar el código generado en la aplicación, consulta [Integrar un modelo](integrate-model.md).

## <a name="next-steps"></a>Pasos siguientes

Intenta crear tu primera aplicación de Windows ML con un tutorial paso a paso en [Introducción](get-started.md).