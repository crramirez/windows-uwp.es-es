---
title: Seguimiento de peligros frente a recursos de grupos de iconos
description: "Para recursos que no son de streaming, Direct3D puede impedir ciertas condiciones de riesgo durante la representación, pero, ya que el seguimiento de riesgos sería a nivel de iconos para los recursos de streaming, las condiciones de riesgo de seguimiento durante la representación de los recursos de streaming pueden ser demasiado altas."
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- Seguimiento de peligros frente a recursos de grupos de iconos
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 5321da19c4c168b48d7e2ba2f3edb7da7880e944
ms.lasthandoff: 02/07/2017

---

# <a name="hazard-tracking-versus-tile-pool-resources"></a>Seguimiento de peligros frente a recursos de grupos de iconos


Para recursos que no son de streaming, Direct3D puede impedir ciertas condiciones de riesgo durante la representación, pero, ya que el seguimiento de riesgos sería a nivel de iconos para los recursos de streaming, las condiciones de riesgo de seguimiento durante la representación de los recursos de streaming pueden ser demasiado altas.

Por ejemplo, para los recursos que no son de streaming, el tiempo de ejecución no permite que cualquier subrecurso determinado esté vinculado como entrada (por ejemplo, una vista de recursos del sombreador) y salida (por ejemplo, una vista de destino de representación) al mismo tiempo. Si se encuentra este caso, el tiempo de ejecución desenlaza la entrada. Esta sobrecarga de seguimiento en el tiempo de ejecución es barata y se realiza en el nivel de subrecurso. Una de las ventajas de esta sobrecarga de seguimiento es que minimiza las posibilidades de que las aplicaciones dependan accidentalmente del orden de ejecución del sombreador de hardware. El orden de ejecución del sombreador de hardware puede variar entre diferentes GPU, si no se encuentra en una unidad de procesamiento de gráfico (GPU) determinada.

Realizar un seguimiento de cómo se enlazan los recursos puede resultar demasiado caro para los recursos de streaming, dado que el seguimiento se realiza a nivel de iconos. Surgen nuevos problemas, como la posible validación de los intentos para representar contenido en una vista de destino de representación con un icono asignado a varias áreas de la superficie simultáneamente. Si resulta que este seguimiento de riesgo por icono es demasiado caro para el tiempo de ejecución, lo ideal es que al menos haya una opción en la capa de depuración.

Una aplicación debe informar al controlador de pantalla de que está esperando a que se complete la primera operación antes de que puedan empezar las siguientes. Debe hacerlo cuando ha emitido una operación de escritura o lectura para un recurso de streaming que haga referencia a la memoria del grupo de iconos a la que también se hará referencia mediante recursos de streaming independientes en operaciones de lectura o escritura posteriores.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Asignaciones a un grupo de iconos](mappings-are-into-a-tile-pool.md)

 

 





