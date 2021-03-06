---
title: Interpolación de triángulos
description: Durante la representación, la canalización interpola los datos de vértices en cada triángulo.
ms.assetid: 1A76DD78-CED7-42BE-BA81-B9050CD3AF9B
keywords:
- Interpolación de triángulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e8017cd75ed3dfd4129d6c15d668648792cc8d0a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592910"
---
# <a name="triangle-interpolation"></a>Interpolación de triángulos


Durante la representación, la canalización interpola los datos de vértices en cada triángulo. Los datos de vértice pueden ser una amplia variedad de datos y pueden incluir, entre otros, color, color especular, alfa difuso (opacidad del triángulo), alfa especular y un factor de niebla. Para la canalización de vértices programable, el factor de niebla proviene del registro de niebla. Para la canalización de vértices de función fija, el factor de niebla proviene del alfa especular.

Para algunos datos de vértice, la interpolación depende del modo de sombreado actual, como se indica a continuación:

| Modo de sombreado | Descripción                                                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Plana         | Solo el factor de niebla se interpola en modo de pantalla plana. Para todos los demás valores interpolados, se aplica el color del primer vértice en el triángulo en todo el rostro. |
| Gouraud      | Se realiza la interpolación lineal entre los tres vértices.                                                                                                               |

 

El color de difusión y el color especular se tratan de manera diferente, según el modelo de color. En el modelo de color RGB, el sistema usa los componentes de color rojo, verde y azul en la interpolación.

El componente alfa de un color se trata como un valor interpolado diferente porque los controladores de dispositivos pueden implementar la transparencia de dos maneras distintas: mediante el uso de mezcla de texturas o mediante punteado.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




