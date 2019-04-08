---
title: Introducción a las transformaciones
description: Las transformaciones matriciales controlan muchos de los cálculos de bajo nivel de los gráficos 3D.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b6be8ee8aa67196581907087d99e0324d741a00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640150"
---
# <a name="transform-overview"></a>Introducción a las transformaciones


Las transformaciones matriciales controlan muchos de los cálculos de bajo nivel de los gráficos 3D.

La canalización de geometría toma vértices como entrada. El motor de transformación aplica las transformaciones de mundo, vista y proyección a los vértices, recorta el resultado y pasa todo al rasterizador.

| Transformación y espacio                           | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Coordenadas del modelo en el espacio de modelo              | Al principio de la canalización, se declaran los vértices de un modelo con respecto a un sistema de coordenadas local. Este es un origen local y una orientación. Esta orientación de coordenadas se suele denominar *espacio de modelo*. Coordenadas individuales se denominan *coordenadas del modelo*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Transformación de mundos en espacio de mundos              | La primera fase de la canalización de geometría transforma los vértices de un modelo de su sistema de coordenadas local en un sistema de coordenadas que usan todos los objetos de una escena. El proceso de cambiar la orientación los vértices se denomina la [Transformación de mundos](world-transform.md), que se convierte del espacio de modelo en una nueva orientación llamada *espacio de mundos*. Cada vértice en el espacio de mundo se declara usando *coordenadas del mundo*.                                                                                                                                                                                                                                                                                                                           |
| Transformación de vista en espacio de vista (espacio de cámara) | En la siguiente fase, los vértices que describen el mundo en 3D se orientan con respecto a una cámara. Es decir, la aplicación elige un punto de vista para la escena y las coordenadas del espacio de mundo se reubican y giran alrededor de la vista de la cámara, convirtiendo el espacio de mundo en *espacio de vista* (también llamado *espacio de cámara*). Esta es la [Transformación de vista](view-transform.md), que se convierte de espacio de mundo en espacio de vista.                                                                                                                                                                                                                                                                                                                        |
| Transformación de la proyección en espacio de proyección    | La siguiente fase es la [Transformación de la proyección](projection-transform.md), que se convierte de espacio de vista en espacio de proyección. En esta parte de la canalización, los objetos se escalan normalmente en relación a la distancia a la que están del visor para crear la ilusión de profundidad en una escena; los objetos cercanos parecen más grandes que los objetos distantes. Por cuestiones de simplicidad, esta documentación se refiere al espacio en el que existen vértices después de la transformación de la proyección en *espacio de proyección*. Algunos libros de gráficos pueden hacer referencia a un espacio de proyección como *espacio homogéneo posterior a la perspectiva*. No todas las transformaciones de proyección modifican a escala el tamaño de los objetos de una escena. Estas proyecciones pueden denominarse *proyecciones afines* o *ortogonales*. |
| Recorte en el espacio de pantalla                      | En la parte final de la canalización, se quitan los vértices que no estarán visibles en la pantalla, de modo que el rasterizador no pierde tiempo calculando los colores y sombreados de algo que nunca se verá. Este proceso se denomina *recorte*. Después del recorte, los vértices restantes se modifican a escala según los parámetros de ventanilla y se convierten en coordenadas de pantalla. Los vértices resultantes, que se muestran en la pantalla cuando la escena se rasteriza, existen en *espacio de pantalla*.                                                                                                                                                                                                                                                    |

 

Las transformaciones se usan para convertir la geometría de objetos de un espacio de coordenadas a otro. Direct3D usa matrices para realizar transformaciones 3D. Las matrices crean transformaciones 3D. Puedes combinar matrices para producir una única matriz que abarque varias transformaciones.

Puedes transformar coordenadas entre espacio de modelo, espacio del mundo y espacio de vista.

-   [Transformación de mundos](world-transform.md): convierte un espacio de modelo en un espacio de mundo.
-   [Transformación de vista](view-transform.md): convierte de espacio del mundo en espacio de vista.
-   [Transformación de proyección](projection-transform.md): convierte un espacio de vista en un espacio de proyección.

## <a name="span-idmatrixtransformsspanspan-idmatrixtransformsspanspan-idmatrixtransformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>Transformaciones de matriz


En las aplicaciones que funcionan con gráficos 3D, puedes usar transformaciones geométricas para hacer lo siguiente:

-   Expresar la ubicación de un objeto con respecto a otro objeto.
-   Girar y cambiar el tamaño de objetos.
-   Cambiar posiciones de visualización, direcciones y perspectivas.

Puedes transformar cualquier punto (x, y, z) en otro punto (x', y', z') mediante el uso de una matriz 4 x 4, como se muestra en la siguiente ecuación.

![ecuación de transformación de cualquier punto en otro punto](images/matmult.png)

Realiza las siguientes ecuaciones en (x, y, z) y la matriz para producir el punto (x', y', z').

![ecuaciones para el nuevo punto](images/matexpnd.png)

Las transformaciones más comunes son traslación, rotación y escala. Puedes combinar las matrices que producen estos efectos en una única matriz para calcular varias transformaciones a la vez. Por ejemplo, puedes crear una única matriz para trasladar y girar una serie de puntos.

Las matrices se escriben en orden de fila-columna. Una matriz que escala uniformemente vértices en cada eje, denominado escala uniforme, se representa mediante la siguiente matriz usando notación matemática.

![ecuación de una matriz para escala uniforme](images/matrix.png)

En C++, Direct3D declara matrices como una matriz de dos dimensiones, una estructura de matriz. El siguiente ejemplo muestra cómo inicializar una estructura de [**D3DMATRIX**](https://msdn.microsoft.com/library/windows/desktop/bb172573) para que funcione como una matriz de escala uniforme (factor de escala "s").

```
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>Traducir


La siguiente ecuación traslada el punto (x, y, z) a un nuevo punto (x', y', z').

![ecuación de una matriz de traslación para un nuevo punto](images/transl8.png)

Puedes crear manualmente una matriz de traslación en C++. El siguiente ejemplo muestra el código fuente de una función que crea una matriz para trasladar los vértices.

```
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


La siguiente ecuación escala el punto (x, y, z) según valores arbitrarios en las direcciones x-, y- y z- para ir a un nuevo punto (x', y', z').

![ecuación de una matriz de escala para un nuevo punto](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>Girar


Las transformaciones que se describen aquí son para los sistemas de coordenadas izquierdos y, por lo tanto, pueden ser diferentes de las matrices de transformación que hayas visto en otro lugar.

La siguiente ecuación gira el punto (x, y, z) alrededor del eje x, generando un nuevo punto (x', y', z').

![ecuación de una matriz de rotación x para un nuevo punto](images/matxrot.png)

La siguiente ecuación gira el punto alrededor del eje y.

![ecuación de una matriz de rotación y para un nuevo punto](images/matyrot.png)

La siguiente ecuación gira el punto alrededor del eje z.

![ecuación de una matriz de rotación z para un nuevo punto](images/matzrot.png)

En estas matrices de ejemplo, la letra griega theta significa el ángulo de rotación, en radianes. Los ángulos se miden en el sentido de las agujas del reloj mirando el eje de rotación hacia el origen.

El siguiente código muestra una función para controlar la rotación alrededor del eje X.

```
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

## <a name="span-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>Concatenación de Matrices


Una ventaja de usar matrices es que se pueden combinar los efectos de dos o más matrices multiplicándolas. Esto significa que, para girar un modelo y después trasladarlo a alguna ubicación, no es necesario aplicar dos matrices. En su lugar, se multiplican las matrices de rotación y traslación para generar una matriz compuesta que contiene todos los efectos. Este proceso, denominado concatenación de matriz, se puede escribir en la siguiente ecuación.

![ecuación de concatenación de matriz](images/matrxcat.png)

En esta ecuación, C es la matriz compuesta que se está creando y desde M₁ a Mₙ son las matrices individuales. En la mayoría de los casos, se concatenan solo dos o tres matrices, pero no hay ningún límite.

El orden en que se realiza la multiplicación de matrices es fundamental. La fórmula anterior refleja la regla de izquierda a derecha de la concatenación de matrices. Es decir, los efectos visibles de las matrices que usas para crear una matriz compuesta se producen en el orden de izquierda a derecha. En el siguiente ejemplo se muestra una matriz de mundo típica. Imagina que estás creando la matriz de mundo para un platillo volador típico. Probablemente quieras girar el platillo volador alrededor de su centro (el eje y del espacio de modelo) y trasladarlo a otra ubicación en la escena. Para lograr este efecto, primero debes crear una matriz de rotación y, a continuación, multiplicarla por una matriz de traslación, como se muestra en la siguiente ecuación.

![ecuación de giro en función de una matriz de rotación y una matriz de traslación](images/wrldexpl.png)

En esta fórmula, R<sub>y</sub> es una matriz de rotación alrededor del eje y, y T<sub>w</sub> es una traslación a alguna posición en coordenadas del mundo.

El orden en el que se multiplican las matrices es importante porque, a diferencia de dos valores escalares, la multiplicación de matrices no es conmutativa. La multiplicación de matrices en orden tiene el efecto visual de la traslación del platillo hasta su posición en el espacio del mundo y, a continuación, la rotación alrededor del origen del mundo.

Independientemente de qué tipo de matriz vas a crear, recuerda la regla de izquierda a derecha para asegurarte de obtener los efectos esperados.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Transformaciones](transforms.md)

 

 




