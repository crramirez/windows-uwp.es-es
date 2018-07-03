---
author: wschin
title: Convertir modelos ML existentes en ONNX
description: Los ejemplos de código muestran cómo usar WinMLTools para convertir modelos existentes en scikit-learn y ML Core en formato ONNX.
ms.author: sezhen
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, aprendizaje automático de Windows, WinML, WinMLTools, ONNX, ONNXMLTools, scikit-learn, Core ML
ms.localizationpriority: medium
ms.openlocfilehash: 7b2e9c8b661ccd2b0358882992da6c4f160b49f0
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843005"
---
# <a name="convert-existing-ml-models-to-onnx"></a>Convertir modelos ML existentes en ONNX

[WinMLTools](https://pypi.org/project/winmltools/) permite a los usuarios convertir modelos formados en otros marcos en formato ONNX. Aquí se muestra cómo instalar el paquete WinMLTools y cómo convertir modelos existentes en scikit-learn y Core ML en ONNX a través de código Python.

## <a name="install-winmltools"></a>Instalar WinMLTools

WinMLTools es un paquete de Python (winmltools) que admite las versiones 2.7 y 3.6 de Python. Si estás trabajando en un proyecto de ciencia de datos, recomendamos instalar una distribución científica de Python como Anaconda.

WinMLTools Sigue el [proceso de instalación del paquete python estándar](https://packaging.python.org/installing/). Desde el entorno de python, ejecuta:

```
pip install winmltools
```

WinMLTools presenta las siguientes dependencias:

- numpy v1.10.0+
- onnxmltools 0.1.0.0+
- protobuf v.3.1.0+

Para actualizar los paquetes de dependencia, ejecute el comando pip con el argumento '-U'.

```
pip install -U winmltools
```

Sigue [onnxmltools](https://github.com/onnx/onnxmltools) en GitHub para obtener más información acerca de las dependencias de onnxmltools.

Detalles adicionales sobre cómo usar WinMLTools pueden encontrarse en la documentación específica del paquete con la función de ayuda.

```
help(winmltools)
```

> [!NOTE]
> Con la extensión Visual Studio Tools for AI también puedes usar WinMLTools dentro del IDE de Visual Studio para una experiencia más sencilla mediante clics para convertir tus modelos en formato ONNX. Para obtener más información, visita [Herramientas VS para AI](https://github.com/Microsoft/vs-tools-for-ai/).

## <a name="scikit-learn-models"></a>Modelos scikit-learn

El siguiente fragmento de código forma una máquina de vector de soporte técnico lineal en scikit-learn y convierte el modelo en ONNX.

~~~python
# First, we create a toy data set.
# The training matrix X contains three examples, with two features each.
# The label vector, y, stores the labels of all examples.
X = [[0.5, 1.], [-1., -1.5], [0., -2.]]
y = [1, -1, -1]

# Then, we create a linear classifier and train it.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()
linear_svc.fit(X, y)

# To convert scikit-learn models, we need to specify the input feature's name and type for our converter. 
# The following line means we have a 2-D float feature vector, and its name is "input" in ONNX.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
linear_svc_onnx = convert_sklearn(linear_svc, name='LinearSVC', input_features=[('input', 'float', 2)])    

# Now, we save the ONNX model into binary format.
from winmltools.utils import save_model
save_model(linear_svc_onnx, 'linear_svc.onnx')

# If you'd like to load an ONNX binary file, our tool can also help.
from winmltools.utils import load_model
linear_svc_onnx = load_model('linear_svc.onnx')

# To see the produced ONNX model, we can print its contents or save it in text format.
print(linear_svc_onnx)
from winmltools.utils import save_text
save_text(linear_svc_onnx, 'linear_svc.txt')

# The conversion of linear regression is very similar. See the example below.
from sklearn.svm import LinearSVR
linear_svr = LinearSVR()
linear_svr.fit(X, y)
linear_svr_onnx = convert_sklearn(linear_svr, name='LinearSVR', input_features=[('input', 'float', 2)])   
~~~

Los usuarios puedan reemplazar `LinearSVC` por otros modelos de scikit-learn como `RandomForestClassifier`. Ten en cuenta que la [generador de código automático](overview.md#automatic-interface-code-generation) usa el parámetro `name` para generar las variables y los nombres de clase. Si no se proporciona `name`,se genera un GUID, el cual no cumplirá con las convenciones de nomenclatura de variables para lenguajes como C++/C#. 

## <a name="scikit-learn-pipelines"></a>Canalizaciones scikit-learn

A continuación, mostramos cómo las canalizaciones scikit-learn pueden convertirse en ONNX.

~~~python
# First, we create a toy data set.
# Notice that the first example's last feature value, 300, is large.
X = [[0.5, 1., 300.], [-1., -1.5, -4.], [0., -2., -1.]]
y = [1, -1, -1]

# Then, we declare a linear classifier.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()

# One common trick to improve a linear model's performance is to normalize the input data.
from sklearn.preprocessing import Normalizer
normalizer = Normalizer()

# Here, we compose our scikit-learn pipeline. 
# First, we apply our normalization. 
# Then we feed the normalized data into the linear model.
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(normalizer, linear_svc)
pipeline.fit(X, y)

# Now, we convert the scikit-learn pipeline into ONNX format. 
# Compared to the previous example, notice that the specified feature dimension becomes 3.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
pipeline_onnx = convert_sklearn(linear_svc, name='NormalizerLinearSVC', input_features=[('input', 'float', 3)])   

# We can print the fresh ONNX model.
print(pipeline_onnx)

# We can also save the ONNX model into binary file for later use.
from winmltools.utils import save_model
save_model(pipeline_onnx, 'pipeline.onnx')

# Our conversion framework provides limited support of heterogeneous feature values.
# We cannot have numerical types and string type in one feature vector. 
# Let's assume that the first two features are floats and the last feature is integer (encoded a categorical attribute).
X_heter = [[0.5, 1., 300], [-1., -1.5, 400], [0., -2., 100]]

# One popular way to represent categorical is one-hot encoding.
from sklearn.preprocessing import OneHotEncoder
one_hot_encoder = OneHotEncoder(categorical_features=[2])

# Let's initialize a classifier. 
# It will be right after the one-hot encoder in our pipeline.
linear_svc = LinearSVC()

# Then, we form a two-stage pipeline.
another_pipeline = make_pipeline(one_hot_encoder, linear_svc)
another_pipeline.fit(X_heter, y)

# Now, we convert, save, and load the converted model. 
# For heterogeneous feature vectors, we need to seperately specify their types for all homogeneous segments.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
another_pipeline_onnx = convert_sklearn(another_pipeline, name='OneHotLinearSVC', input_features=[('input', 'float', 2), ('another_input', 'int64', 1)])
save_model(another_pipeline_onnx, 'another_pipeline.onnx')
from winmltools.utils import load_model
loaded_onnx_model = load_model('another_pipeline.onnx')

# Of course, we can print the ONNX model to see if anything went wrong.
print(another_pipeline_onnx)
~~~

## <a name="core-ml-models"></a>Modelos Core ML

Aquí, damos por hecho que la ruta de acceso de un ejemplo de archivo de Core ML es *example.mlmodel*.

~~~python
from coremltools.models.utils import load_spec
# Load model file
model_coreml = load_spec('example.mlmodel')
from winmltools import convert_coreml
# Convert it!
# The automatic code generator (mlgen) uses the name parameter to generate class names.
model_onnx = convert_coreml(model_coreml, name='ExampleModel')   
~~~

El `model_onnx` es un objeto [ModelProto](https://github.com/onnx/onnxmltools/blob/0f453c3f375c1ae928b83a4c7909c82c013a5bff/onnxmltools/proto/onnx-ml.proto#L176) de ONNX. Podemos guardarlo en dos formatos diferentes.

~~~python
from winmltools.utils import save_model
# Save the produced ONNX model in binary format
save_model(model_onnx, 'example.onnx')
# Save the produced ONNX model in text format
from winmltools.utils import save_text
save_text(model_onnx, 'example.txt')
~~~

**Nota**: CoreMLTools es un paquete de Python proporcionado por Apple, pero no está disponible en Windows. Si necesitas instalar el paquete en Windows, instala el paquete directamente desde el repositorio:

```
pip install git+https://github.com/apple/coremltools
```

## <a name="core-ml-models-with-image-inputs-or-outputs"></a>Modelos Core ML con entradas y salidas de imagen

Debido a la falta de tipos de imagen en ONNX, la conversión de modelos de imagen Core ML (es decir, modelos con imágenes como entradas y salidas) requiere algunos pasos de preprocesamiento y posprocesamiento.

El objetivo de preprocesamiento es asegurarte de que la imagen de entrada tenga el formato correcto como un tensor ONNX. Se supone que *X* es una imagen de entrada con forma [C, H, W] en Core ML. En ONNX, la variable *X* sería un tensor de punto flotante con la misma forma y *X*[0,:, :]/*X*[1,:, :]/*X*[2,:,:] almacena el canal de rojo/verde/azul de la imagen. Para las imágenes de escala de grises en Core ML, su formato son [1, H, W]-tensors en ONNX ya que solo tenemos un canal.

Si el modelo de Core ML original genera una imagen, convierte manualmente tensores de salida de punto flotante de ONNX en imágenes. Hay dos pasos básicos. El primer paso es truncar valores mayores de 255a 255 y cambiar todos los valores negativos a 0. El segundo paso es redondear todos los valores de píxeles a valores enteros (añadiendo 0,5 y, a continuación, truncando los decimales). El orden de canales de salida (por ejemplo, RGB o BGR) se indica en el modelo de Core ML. Para generar la salida de imagen adecuada, es posible que tengamos que transformar o colocar de forma aleatoria para recuperar el formato deseado.

Aquí tenemos en cuenta un modelo de Core ML, FNS-Candy, descargado de [GitHub](https://github.com/likedan/Awesome-CoreML-Models), como un ejemplo concreto de conversión para demostrar la diferencia entre los formatos ONNX y Core ML. Ten en cuenta que todos los comandos posteriores se ejecutan en un entorno de python.

En primer lugar, cargamos el modelo de Core ML y examinamos los formatos de entrada y salida.

~~~python
from coremltools.models.utils import load_spec
model_coreml = load_spec('FNS-Candy.mlmodel')
model_coreml.description # Print the content of Core ML model description
~~~

Salida de pantalla:

~~~
...
input {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
output {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
~~~

En este caso, la entrada y salida son una imagen BGR de 720 x 720. Nuestro siguiente paso es convertir el modelo Core ML con WinMLTools.

~~~python
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from onnxmltools import convert_coreml
model_onnx = convert_coreml(model_coreml, name='FNSCandy')    
~~~

Un método alternativo para ver los formatos de entrada y salida del modelo en ONNX, es usar el siguiente comando:

~~~python
model_onnx.graph.input # Print out the ONNX input tensor's information
~~~

Salida de pantalla:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

La entrada producida (indicado mediante *X*) en ONNX es un tensor 4-D. Los 3 últimos ejes son C, H y W, respectivamente. La primera dimensión es "None" lo que significa que este modelo ONNX se puede aplicar a cualquier número de imágenes. Para aplicar este modelo para procesar un lote de 2 imágenes, la primera imagen corresponde a *X*[0,:,:,:] mientras que *X*[1,:,:,:] corresponde a la segunda imagen. Los canales rojo/verde/azul de la primera imagen son *X*[0, 0,:, :]/*X*[0, 1,:, :]/*X*[0, 2,:,:] y similares para la segunda imagen.

~~~python
model_onnx.graph.output # Print out the ONNX output tensor's information
~~~

Salida de pantalla:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

Como puedes ver, el formato producido es idéntico al formato de entrada original del modelo. Sin embargo, en este caso, no es una imagen porque los valores de píxeles son números enteros, no números de punto flotante. Para volver a convertir una imagen, trunca valores mayores de 255en 255, cambia los valores negativos a 0 y redondea todos los valores decimales a valores enteros.