---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: 'Movimiento en la práctica: animación en las aplicaciones para UWP'
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 87a17d3f73887c9b1b5029e2096c5b41c9444c4e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843943"
---
# <a name="bringing-it-together"></a>Reunión de todo

> [!IMPORTANT]
> En este artículo se describe una funcionalidad que no se ha lanzado aún y que puede sufrir importantes modificaciones antes de que se lance la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

El tiempo, la aceleración, la direccionalidad y la gravedad trabajan de manera conjunta para formar la base del movimiento de Fluent. Cada uno de ellos se tiene en cuenta en el contexto de los demás y se debe aplicar de manera adecuada en el contexto de tu aplicación.

Hay 3 maneras de aplicar los conceptos básicos del movimiento de Fluent en tu aplicación.

:::row::: :::column::: **Animación implícita**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

### **<a name="transition-example"></a>Ejemplo de transición**

![animación funcional](images/pageRefresh.gif)

:::row-end::: :::row::: :::column::: <b>Dirección hacia delante fuera</b>:<br>
        Fundido de salida: 150m; Aceleración: Aceleración predeterminada

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
      
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

 ### **<a name="object-example"></a>Ejemplo de objeto**

 ![Movimiento de 300milisegundos](images/control.gif)
 
:::row::: :::column::: <b>Expansión de dirección</b>:<br>
        Crecimiento: 300ms; Aceleración: Estándar :::column-end::: :::column::: <b>Contrato de dirección</b>:<br>
        Crecimiento: 150ms; Aceleración: Aceleración predeterminada :::column-end::: :::row-end:::

## <a name="related-articles"></a>Artículos relacionados

- [Introducción al movimiento](index.md)
- [Sincronización y aceleración](timing-and-easing.md)
- [Direccionalidad y gravedad](directionality-and-gravity.md)