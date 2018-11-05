---
author: Jwmsft
Description: The master/detail pattern displays a master list and the details for the currently selected item. This pattern is frequently used for email and contact lists/address books.
title: Panel maestro/detalles
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b30835e31e86c0c98d0c134ed28adca4413650c9
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6034155"
---
# <a name="masterdetails-pattern"></a>Patrón de maestro y detalles

 

El patrón de maestro y detalles tiene un panel maestro (normalmente con una [vista de lista](lists.md)) y un panel de detalles para el contenido. Cuando se selecciona un elemento en la lista maestra, se actualiza el panel de detalles. Este patrón se usa con frecuencia para libretas de direcciones y de correos electrónicos.

> **API importantes**: [Clase ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView), [Clase SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![Ejemplo del patrón de maestro y detalles](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>¿Es este el patrón adecuado?

El patrón de maestro y detalles funciona bien si quieres:

-   Crear una aplicación de correo electrónico, una libreta de direcciones o cualquier aplicación que se base en un diseño de detalles de lista.
-   Buscar y dar prioridad a una gran colección de contenido.
-   Permitir la rápida adición y eliminación de elementos de una lista mientras se trabaja cambiando entre contextos continuamente.

## <a name="choose-the-right-style"></a>Elegir el estilo adecuado

Al implementar el patrón de maestro y detalles, te recomendamos que uses el estilo apilado o el estilo en paralelo, según la cantidad de espacio disponible en pantalla.

| Ancho de ventana disponible | Estilo recomendado |
|------------------------|-------------------|
| 320epx - 640epx        | Apilado           |
| 641epx o más ancho       | En paralelo      |

 
## <a name="stacked-style"></a>Estilo apilado

En el estilo apilado, solo hay un panel visible a la vez: el maestro o los detalles.

![Vista de maestro y detalles en el modo apilado](images/patterns-md-stacked.png)

El usuario comienza en el patrón de panel y "explora en profundidad" hasta el panel de detalles, seleccionando un elemento en la lista maestra. Para el usuario, parece que las vistas de maestro y detalles existen en dos páginas independientes.

### <a name="create-a-stacked-masterdetails-pattern"></a>Crear un patrón de maestro y detalles apilado

Una forma de crear el patrón de maestro y detalles apilado es usar páginas independientes para la vista maestra y de detalles. Coloca la vista maestra en una sola página y el panel de detalles en otra página independiente.

![Partes de la vista de maestro y detalles con estilo apilado](images/patterns-md-stacked-parts.png)

En la página de vista maestra, un control de [vista de lista](lists.md) funciona bien para presentar listas que pueden contener imágenes y texto. 

En la página de vista de detalles, usa el [elemento de contenido](../layout/layout-panels.md) que sea más apropiado. Si tienes muchos campos independientes, plantéate usar un diseño de **cuadrícula** para organizar los elementos en un formulario.

Para la navegación entre páginas, consulta el [historial de navegación y navegación hacia atrás para las aplicaciones para UWP](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Estilo en paralelo

En el estilo en paralelo, los paneles de maestro y de detalles están visibles al mismo tiempo.

![Patrón de maestro y detalles](images/patterns-masterdetail-400x227.png)

La lista del panel de maestro usa una selección visual para indicar el elemento seleccionado actual. Al seleccionar un elemento nuevo en la lista de maestro, se actualiza el panel de detalles.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Crear un patrón de maestro y detalles en paralelo

Una forma de crear un patrón de maestro y detalles en paralelo es usar el control [vista en dos paneles](split-view.md). Coloca la vista maestra en el panel de vista en dos paneles y la vista de detalles en el contenido de vista en dos paneles.

![partes de la vista en dos paneles de la vista maestra y detalles](images/patterns_md_splitview_parts.png)

En el panel maestro, un control de [vista de lista](lists.md) funciona bien para presentar listas que pueden contener imágenes y texto.

En el contenido de detalles, usa el [elemento de contenido](../layout/layout-panels.md) que sea más apropiado. Si tienes muchos campos independientes, plantéate usar un diseño de **cuadrícula** para organizar los elementos en un formulario.

## <a name="adaptive-layout"></a>Diseño adaptativo

Para implementar un patrón de maestro y detalles de cualquier tamaño de pantalla, crea una interfaz de usuario con capacidad de respuesta y con [diseño adaptativo](../layout/layouts-with-xaml.md).

![diseño de vista maestra y detalles adaptativo](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Crear un patrón de maestro y detalles adaptativo
Para crear un diseño adaptativo, define diferentes [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate) para la interfaz de usuario y declara puntos de interrupción para los distintos estados con [**AdaptiveTriggers**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

Las siguientes muestras implementan el patrón de maestro y detalles con diseños adaptativos y muestran datos enlazados a recursos estáticos, de bases de datos y en línea: 
- [Muestra de maestro y detalles](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [Muestra de panel maestro y detalles con selección](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Muestra de panel maestro y detalles de Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Muestra de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [Muestra de lector RSS](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>Artículos relacionados

- [Listas](lists.md)
- [Buscar](search.md)
- [Barras de la aplicación y de comandos](app-bars.md)
- [Clase ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Clase SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
