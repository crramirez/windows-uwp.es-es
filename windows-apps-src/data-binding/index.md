---
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: Enlace de datos
description: El enlace de datos es una forma para que la interfaz de usuario de la aplicación muestre los datos y, opcionalmente, se mantenga sincronizada con dichos datos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 96bb0ca90ee3a55f43837fa92bad750b95b248eb
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66362439"
---
# <a name="data-binding"></a>Enlace de datos

El enlace de datos es una forma para que la interfaz de usuario de la aplicación muestre los datos y, opcionalmente, se mantenga sincronizada con dichos datos. El enlace de datos permite separar lo que concierne a los datos de lo que concierne a la interfaz de usuario, lo que da como resultado un modelo conceptual más sencillo y una mejor legibilidad, comprobación y mantenimiento de la aplicación. En el marcado, puedes usar la [extensión de marcado {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) o la [extensión de marcado {Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension). Incluso puedes usar una combinación de ambos en la misma aplicación, aun en el mismo elemento de la interfaz de usuario. {x:Bind} es nuevo en Windows 10 y tiene un mejor rendimiento.

| Tema | Descripción |
|-------|-------------|
| [Introducción al enlace de datos](data-binding-quickstart.md) | En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o cómo enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP). Además, te mostramos cómo controlar la representación de los elementos, implementar una vista de detalles basada en una selección y convertir datos para mostrarlos. Para obtener información más detallada, consulta [Enlace de datos a profundidad](data-binding-in-depth.md). | 
| [Enlace de datos en profundidad](data-binding-in-depth.md) | En este tema se describen detalladamente las características del enlace de datos. |
| [Datos de ejemplo sobre la superficie de diseño y para la creación de prototipos](displaying-data-in-the-designer.md) | Para hacer que los controles se rellenen con datos en el diseñador de Visual Studio (de modo que puedas trabajar en el diseño de la aplicación, las plantillas y otras propiedades visuales), puedes usar los datos de ejemplo en tiempo de diseño de distintas maneras. Los datos de ejemplo también pueden ser muy útiles y ahorrarte tiempo si creas una aplicación de diseño de bocetos (o prototipos). Puedes usar los datos de ejemplo del boceto o el prototipo en tiempo de ejecución para ilustrar tus ideas sin tener que conectarte a los datos dinámicos reales. |
| [Enlazar datos jerárquicos y crear una vista de tipo maestro/detalles](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | Puedes hacer una vista de tipo maestro/detalles (también conocida como lista/detalles) de varios niveles de datos jerárquicos al enlazar controles de elementos a instancias de [<strong>CollectionViewSource</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) que están enlazadas juntas en una cadena. |
| [Enlace de datos y MVVM](data-binding-and-mvvm.md) | En este tema se describe el patrón de diseño de arquitectura de interfaz de usuario Model-View-ViewModel (MVVM). El enlace de datos es la base de MVVM y permite el acoplamiento flexible entre el código de interfaz de usuario y que no es de interfaz de usuario. |
