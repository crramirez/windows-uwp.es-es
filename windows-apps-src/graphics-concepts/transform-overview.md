---
title: Introducción a las transformaciones
description: Las transformaciones de matriz controlan gran parte de las matemáticas de bajo nivel de los gráficos 3D.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0f8efd1984ae8a726870bd8e7aaa3960baf91218
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156219"
---
# <a name="transform-overview"></a>Introducción a las transformaciones

Las transformaciones de matriz controlan gran parte de las matemáticas de bajo nivel de los gráficos 3D.

La canalización de geometría toma los vértices como entrada. El motor de transformación aplica las transformaciones mundo, vista y proyección a los vértices, recorta el resultado y pasa todo al rasterizador.

| Transformación y espacio                           | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Coordenadas del modelo en el espacio del modelo              | En el encabezado de la canalización, los vértices de un modelo se declaran en relación con un sistema de coordenadas local. Se trata de un origen local y una orientación. A menudo, esta orientación de las coordenadas se conoce como *espacio de modelo*. Las coordenadas individuales se denominan *coordenadas del modelo*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Transformación universal en el espacio universal              | La primera fase de la canalización de geometría transforma los vértices de un modelo de su sistema de coordenadas local en un sistema de coordenadas que usan todos los objetos de una escena. El proceso de reorientar los vértices se denomina [transformación universal](world-transform.md), que convierte del espacio de modelo a una nueva orientación denominada *espacio universal*. Cada vértice del espacio universal se declara mediante *coordenadas universales*.                                                                                                                                                                                                                                                                                                                           |
| Ver transformación en el espacio de la vista (espacio de la cámara) | En la fase siguiente, los vértices que describen el mundo 3D están orientados con respecto a una cámara. Es decir, la aplicación elige un punto de vista para la escena y las coordenadas del espacio universal se reubican y giran en torno a la vista de la cámara, lo que convierte el espacio universal en el espacio de la *vista* (también conocido como espacio de la *cámara*). Esta es la [transformación](view-transform.md)de la vista, que convierte el espacio universal en el espacio de la vista.                                                                                                                                                                                                                                                                                                                        |
| Transformación de proyección en el espacio de proyección    | La siguiente fase es la [transformación de proyección](projection-transform.md), que convierte de la vista espacio en el espacio de proyección. En esta parte de la canalización, los objetos normalmente se escalan con relación a su distancia desde el visor para dar la sensación de profundidad a una escena. los objetos de cierre se crean para que aparezcan más grandes que los objetos lejanos. Para simplificar, en esta documentación se hace referencia al espacio en el que los vértices existen después de la transformación de proyección como *espacio de proyección*. Algunos libros de gráficos pueden hacer referencia al espacio de proyección como *espacio homogéneo posterior a la perspectiva*. No todas las transformaciones de proyección escalan el tamaño de los objetos en una escena. Una proyección como esta se denomina a veces una proyección *afín* o *ortogonal*. |
| Recorte en el espacio de pantalla                      | En la parte final de la canalización, se quitan los vértices que no serán visibles en la pantalla, de modo que el rasterizador no dedique tiempo a calcular los colores y el sombreado para algo que nunca se verá. Este proceso se denomina *recorte*. Después del recorte, los vértices restantes se escalan según los parámetros de la ventanilla y se convierten en coordenadas de pantalla. Los vértices resultantes, que se muestran en la pantalla cuando se rasteriza la escena, existen en el *espacio de pantalla*.                                                                                                                                                                                                                                                    |

 

Las transformaciones se usan para convertir la geometría de objeto de un espacio de coordenadas a otro. Direct3D usa matrices para realizar transformaciones 3D. Las matrices crean transformaciones 3D. Puede combinar matrices para generar una matriz única que abarque varias transformaciones.

Puede transformar las coordenadas entre el espacio del modelo, el espacio universal y el espacio de la vista.

-   [Transformación universal](world-transform.md) : convierte el espacio del modelo al espacio universal.
-   [Vista](view-transform.md) de la transformación: convierte el espacio universal en el espacio de vista.
-   [Transformación de proyección](projection-transform.md) : convierte el espacio de vista en el espacio de proyección.

## <a name="span-idmatrix_transformsspanspan-idmatrix_transformsspanspan-idmatrix_transformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>Transformaciones de matriz


En las aplicaciones que funcionan con gráficos 3D, puede usar transformaciones geométricas para hacer lo siguiente:

-   Expresar la ubicación de un objeto con respecto a otro objeto.
-   Rotar y ajustar el tamaño de los objetos.
-   Cambiar las posiciones, las direcciones y las perspectivas de visualización.

Puede transformar cualquier punto (x, y, z) en otro punto (x ', y ', z ') mediante una matriz 4x4, tal como se muestra en la siguiente ecuación.

![ecuación de transformar cualquier punto en otro punto](images/matmult.png)

Realice las ecuaciones siguientes en (x, y, z) y en la matriz para generar el punto (x ', y ', z ').

![ecuaciones para el nuevo punto](images/matexpnd.png)

Las transformaciones más comunes son traducción, rotación y escalado. Puede combinar las matrices que producen estos efectos en una sola matriz para calcular varias transformaciones a la vez. Por ejemplo, puede crear una matriz única para traducir y girar una serie de puntos.

Las matrices se escriben en orden de fila y columna. Una matriz que escala uniformemente los vértices a lo largo de cada eje, conocido como escala uniforme, se representa mediante la siguiente matriz mediante notación matemática.

![ecuación de una matriz para el escalado uniforme](images/matrix.png)

En C++, Direct3D declara las matrices como una matriz bidimensional mediante una estructura de matriz. En el ejemplo siguiente se muestra cómo inicializar una estructura [**D3DMATRIX**](/windows/desktop/direct3d9/d3dmatrix) para que actúe como una matriz de escala uniforme (factor de escala "s").

```cpp
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>Traducir


La ecuación siguiente traduce el punto (x, y, z) a un nuevo punto (x ', y ', z ').

![ecuación de una matriz de traslación para un nuevo punto](images/transl8.png)

Puede crear manualmente una matriz de traducción en C++. En el ejemplo siguiente se muestra el código fuente de una función que crea una matriz para traducir los vértices.

```cpp
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>Escala


La ecuación siguiente escala el punto (x, y, z) mediante valores arbitrarios en las direcciones x-, y-y z-a un punto nuevo (x ', y ', z ').

![ecuación de una matriz de escala para un nuevo punto](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>Rota


Las transformaciones que se describen aquí son para los sistemas de coordenadas de la mano izquierda y, por tanto, pueden ser diferentes de las matrices de transformación que se han encontrado en otro lugar.

La siguiente ecuación gira el punto (x, y, z) alrededor del eje x, lo que produce un nuevo punto (x ', y ', z ').

![ecuación de una matriz de rotación x para un nuevo punto](images/matxrot.png)

La siguiente ecuación gira el punto alrededor del eje y.

![ecuación de una matriz de rotación y para un nuevo punto](images/matyrot.png)

La siguiente ecuación gira el punto alrededor del eje z.

![ecuación de una matriz de rotación z para un nuevo punto](images/matzrot.png)

En estas matrices de ejemplo, la letra griega Theta representa el ángulo de rotación, en radianes. Los ángulos se miden en el sentido de las agujas del reloj al mirar el eje de giro hacia el origen.

En el código siguiente se muestra una función que controla la rotación sobre el eje X.

```cpp
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenating_matricesspanspan-idconcatenating_matricesspanspan-idconcatenating_matricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>Concatenación de matrices


Una ventaja de usar matrices es que se pueden combinar los efectos de dos o más matrices multiplicándolos. Esto significa que, para girar un modelo y, a continuación, convertirlo en una ubicación, no es necesario aplicar dos matrices. En su lugar, se multiplican las matrices de traslación y rotación para producir una matriz compuesta que contiene todos sus efectos. Este proceso, denominado concatenación de matriz, se puede escribir con la siguiente ecuación.

![ecuación de concatenación de matriz](images/matrxcat.png)

En esta ecuación, C es la matriz compuesta que se está creando y M ₁ a MN son las matrices individuales. En la mayoría de los casos, solo se concatenan dos o tres matrices, pero no hay ningún límite.

El orden en el que se realiza la multiplicación de matrices es fundamental. La fórmula anterior refleja la regla de izquierda a derecha de la concatenación de matrices. Es decir, los efectos visibles de las matrices que se usan para crear una matriz compuesta se producen en orden de izquierda a derecha. En el ejemplo siguiente se muestra una matriz universal típica. Imagine que va a crear la matriz mundial para un típica volando Saucer. Probablemente quiera girar el Saucer volando en torno a su centro, el eje y del espacio del modelo, y convertirlo en otra ubicación de la escena. Para lograr este efecto, primero se crea una matriz de rotación y, a continuación, se multiplica por una matriz de traslación, tal como se muestra en la siguiente ecuación.

![ecuación de giro basada en una matriz de rotación y una matriz de traslación](images/wrldexpl.png)

En esta fórmula, R<sub>y</sub> es una matriz de rotación sobre el eje y, y T<sub>w</sub> es una traducción a alguna posición en coordenadas universales.

El orden en que se multiplican las matrices es importante porque, a diferencia de la multiplicación de dos valores escalares, la multiplicación de matrices no es conmutativa. La multiplicación de las matrices en el orden opuesto tiene el efecto visual de traducir el Saucer volando a su posición de espacio mundial y, a continuación, girarlo alrededor del origen mundial.

Independientemente del tipo de matriz que cree, recuerde la regla de izquierda a derecha para asegurarse de que se obtienen los efectos esperados.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Transformaciones](transforms.md)

 

 