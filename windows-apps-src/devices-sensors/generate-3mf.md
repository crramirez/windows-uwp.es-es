---
description: Describe la estructura del tipo de archivo de formato de fabricación 3D y cómo puede crearse y manipularse con la API Windows.Graphics.Printing3D.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Generar un paquete 3MF
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7beafb754ead837dd9218d4a9e0223334e12bfb1
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034658"
---
# <a name="generate-a-3mf-package"></a>Generar un paquete 3MF

**API importantes**

-   [**Windows. Graphics. Printing3D**](/uwp/api/windows.graphics.printing3d)

En esta guía, se describe la estructura del documento de formato de fabricación 3D y cómo puede crearse y manipularse con la API [**Windows.Graphics.Printing3D**](/uwp/api/windows.graphics.printing3d).

## <a name="what-is-3mf"></a>¿Qué es 3MF?

El formato de fabricación 3D es un conjunto de convenciones de uso de XML para describir el aspecto y la estructura de modelos 3D para la fabricación (impresión 3D). Define un conjunto de partes (algunas necesarias y algunas opcionales), y sus relaciones, con el objetivo de proporcionar toda la información necesaria para un dispositivo de fabricación 3D. Un conjunto de datos que se adhiere al formato de fabricación 3D puede guardarse como un archivo con la extensión .3mf.

En Windows 10, la clase [**Printing3D3MFPackage**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage) del espacio de nombres **Windows.Graphics.Printing3D** es similar a un archivo único .3mf y otras clases se asignan a los elementos XML concretos del archivo. En esta guía se describe cómo cada una de las partes principales de un documento 3MF puede crearse y definirse mediante programación, cómo se puede utilizar la extensión de materiales 3MF y cómo un objeto **Printing3D3MFPackage** puede convertirse y guardarse como un archivo .3mf. Para obtener más información sobre los estándares de 3MF o la extensión de materiales 3MF, consulta [3MF Specification (Especificación 3MF)](https://3mf.io/what-is-3mf/3mf-specification/).

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>Clases principales de la estructura 3MF

La clase **Printing3D3MFPackage** representa un documento 3MF completo y en el centro de un documento 3MF hay su parte de modelo, que representa la clase [**Printing3DModel**](/uwp/api/windows.graphics.printing3d.printing3dmodel). La mayoría de la información que queremos especificar sobre un modelo 3D se almacenará configurando las propiedades de la clase **Printing3DModel** y las propiedades de sus clases subyacentes.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetInitClasses":::

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>Metadatos

El componente de modelo de un documento 3MF puede contener metadatos en forma de pares clave-valor de cadenas almacenadas en la propiedad **Metadata** . Hay una serie de nombres predefinidos de metadatos, pero hay otros pares que se pueden agregar como parte de una extensión (se describe con más detalle en [3MF specification [Especificación 3MF]](https://3mf.io/what-is-3mf/3mf-specification/)). El receptor del paquete (un dispositivo de fabricación 3D) es el que determina si se administran los metadatos y la forma de hacerlo, pero te recomendamos incluir tanta información básica como sea posible en el paquete 3MF:

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetMetadata":::

## <a name="mesh-data"></a>Datos de malla

En el contexto de esta guía, una malla es un cuerpo de geometría de 3 dimensiones construido a partir de un único conjunto de vértices (aunque no es necesario que aparezca como un único sólido). Una parte de la malla se representa mediante la clase [**Printing3DMesh**](/uwp/api/windows.graphics.printing3d.printing3dmesh). Un objeto de malla válido debe contener información sobre la ubicación de todos sus vértices, así como todas las caras de triángulos que existan entre ciertos conjuntos de vértices.

En el siguiente método se agregan los vértices a una malla y luego se les proporciona ubicaciones en el espacio 3D:

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetVertices":::

En el método siguiente se definen todos los triángulos que deben dibujarse a través de estos vértices:

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetTriangleIndices":::

> [!NOTE]
> Todos los triángulos deben tener sus índices definidos en el sentido contrario a las agujas del reloj (cuando se visualiza el triángulo desde fuera del objeto de malla), para que sus vectores normales de cara apunten hacia afuera.

Cuando un objeto Printing3DMesh contiene conjuntos válidos de vértices y triángulos, a continuación, se debe agregar la propiedad **Meshes** al modelo. Todos los objetos **Printing3DMesh** de un paquete deben almacenarse en la propiedad **Meshes** de la clase **Printing3DModel** .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetMeshAdd":::


## <a name="create-materials"></a>Crear materiales


Un modelo 3D puede contener datos de varios materiales. Esta convención está diseñada para sacar provecho de los dispositivos de fabricación 3D que pueden usar varios materiales en un único trabajo de impresión. También hay varios *tipos* de grupos de materiales, cada uno de ellos capaz de admitir un gran número de diferentes materiales individuales. Cada grupo de materiales debe tener un número de identificación de referencia único y cada material de dentro de ese grupo también debe tener un identificador único.

De este modo, los objetos de malla diferentes de dentro de un modelo pueden hacer referencia a estos materiales. Además, cada triángulo de cada malla puede especificar distintos materiales. Incluso pueden representarse diferentes materiales dentro de un solo triángulo, en el que cada vértice del triángulo tenga un material diferente asignado y el material de cara se calcule como el degradado entre ellos.

En esta guía primero se mostrará cómo crear distintos tipos de materiales dentro de sus respectivos grupos de materiales y cómo almacenarlos como recursos en el objeto de modelo. A continuación, se tratará la asignación de distintos materiales a mallas individuales y a triángulos individuales.

### <a name="base-materials"></a>Materiales base

El tipo de material predeterminado es **material base** , que tiene un valor **material de color** (descrito a continuación) y un atributo de nombre que sirve para especificar el *tipo* de material que se debe usar.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetBaseMaterialGroup":::

> [!NOTE]
> El dispositivo de fabricación 3D determinará qué materiales físicos disponibles se asignan a cada elemento de material virtual almacenado en 3MF. La asignación de materiales no tiene que ser 1:1: si una impresora 3D solo usa un material, imprimirá todo el modelo en ese material, independientemente de las caras o los objetos a los que se asignaron distintos materiales.

### <a name="color-materials"></a>Materiales de color

Los **materiales de color** son similares a los **materiales base** , pero no contienen ningún nombre. Por lo tanto, no proporcionan instrucciones sobre el tipo de material que debe usar la máquina. Contienen solamente datos de color y permiten que la máquina elija el tipo de material (y entonces puede que la máquina pida al usuario que realice la elección). En el siguiente código, los objetos `colrMat` del método anterior se usan individualmente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetColorMaterialGroup":::

### <a name="composite-materials"></a>Materiales compuestos

Los **materiales compuestos** simplemente indican al dispositivo de fabricación que se debe usar una combinación uniforme de distintos **materiales base** . Cada **grupo de materiales compuestos** debe hacer referencia exactamente a un **grupo de materiales base** desde el que se deban dibujar ingredientes. Además, los **materiales base** de este grupo que se pondrán a disposición deben aparecer en una lista de **índices de materiales** , a la que cada **material compuesto** haga referencia al especificar las relaciones (cada **material compuesto** es simplemente una relación de **materiales base** ).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetCompositeMaterialGroup":::

### <a name="texture-coordinate-materials"></a>Materiales de coordenadas de textura

3MF admite el uso de imágenes 2D para dar color a las superficies de modelos 3D. De esta forma, el modelo puede transmitir muchos más datos de color por cara de triángulo (en lugar de tener un solo valor de color por vértice de triángulo). Al igual que los **materiales de color** , los materiales de coordenadas de textura solo transmiten datos de color. Para usar una textura 2D, primero debe declararse un recurso de textura:

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetTextureResource":::

> [!NOTE]
> Los datos de texturas pertenecen al paquete 3MF en sí y no a la parte del modelo del paquete.

A continuación, debemos rellenar **Texture3Coord Materials** . Cada uno hace referencia a un recurso de textura y especifica un punto concreto de la imagen (en coordenadas UV).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetTexture2CoordMaterialGroup":::

## <a name="map-materials-to-faces"></a>Asignar materiales a las caras

Para determinar los materiales que se asignan a los vértices de cada triángulo, debemos trabajar algo más en el objeto de malla de nuestro modelo (si un modelo contiene varias mallas, cada una debe tener sus materiales asignados por separado). Como se mencionó anteriormente, los materiales se asignan por vértices por triángulo. Consulta el código siguiente para ver cómo se especifica y se interpreta esta información.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetMaterialIndices":::

## <a name="components-and-build"></a>Componentes y compilación

La estructura de componentes permite al usuario colocar más de un objeto de malla en un modelo 3D imprimible. Un objeto [**Printing3DComponent**](/uwp/api/windows.graphics.printing3d.printing3dcomponent) contiene una única malla y una lista de referencias a otros componentes. En realidad, se trata de una lista de objetos [**Printing3DComponentWithMatrix**](/uwp/api/windows.graphics.printing3d.printing3dcomponentwithmatrix). Cada objeto **Printing3DComponentWithMatrix** contiene un elemento **Printing3DComponent** y, lo que es importante, una matriz de transformación que se aplica a la malla y a los componentes que contiene dicho elemento **Printing3DComponent** .

Por ejemplo, es posible que un modelo de un coche conste de un elemento **Printing3DComponent** de "cuerpo" que contenga la malla para la carrocería del coche. El componente de "cuerpo" puede contener referencias a cuatro objetos **Printing3DComponentWithMatrix** diferentes, que hacen referencia al mismo elemento **Printing3DComponent** con la malla de "rueda" y contienen cuatro matrices de transformación diferentes (al asignar las ruedas a cuatro distintas posiciones de la carrocería del coche). En este escenario, la malla de "cuerpo" y de "rueda" solo necesitaría almacenarse una vez, aunque el producto final presentara cinco mallas en total.

Se debe hacer referencia directamente a todos los objetos **Printing3DComponent** en la propiedad **Components** del modelo. El componente concreto que se debe usar en el trabajo de impresión se almacena en la propiedad **Build** .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetComponents":::

## <a name="save-package"></a>Guardar el paquete
Ahora que tenemos un modelo con materiales y componentes definidos, podemos guardarlo en el paquete.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetSavePackage":::

Esta función garantiza que la textura se ha especificado correctamente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetFixTexture":::

Desde aquí, podemos iniciar un trabajo de impresión dentro de la aplicación (consulta [3D printing from your app [Impresión 3D desde la aplicación]](./3d-print-from-app.md)), o guardar este paquete **Printing3D3MFPackage** como un archivo .3mf.

En el siguiente método se toma un paquete **Printing3D3MFPackage** terminado y se guardan los datos en un archivo .3mf.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetSaveTo3mf":::

## <a name="related-topics"></a>Temas relacionados

[impresión en 3D desde la aplicación](./3d-print-from-app.md)  
[Ejemplo de impresión en 3D de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 
