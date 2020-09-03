---
Description: El patrón de maestro y detalles muestra una lista maestra y los detalles del elemento seleccionado actual. Este patrón se usa con frecuencia para listas de contactos o libretas de direcciones y correo electrónico.
title: Maestro/detalles
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b414a1177bbfad670e3b623babce068a299a713e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172605"
---
# <a name="masterdetails-pattern"></a>Patrón de master y detalles

 

El patrón de maestro y detalles tiene un panel maestro (normalmente con una [vista de lista](lists.md)) y un panel de detalles para el contenido. Cuando se selecciona un elemento en la lista maestra, se actualiza el panel de detalles. Este patrón se usa con frecuencia para libretas de direcciones y correo electrónico.

> **API importantes**: [clase ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), [clase SplitView](/uwp/api/windows.ui.xaml.controls.splitview)

![Ejemplo del patrón de maestro y detalles](images/HIGSecOne_MasterDetail.png)

> [!TIP]
> Si deseas usar un control XAML que implemente este patrón automáticamente, se recomienda el [control XAML MasterDetailsView](/windows/communitytoolkit/controls/masterdetailsview) del kit de herramientas de la comunidad de Windows.

## <a name="is-this-the-right-pattern"></a>¿Es este el patrón adecuado?

El patrón de maestro y detalles es adecuado para las operaciones siguientes:

-   Compilar una aplicación de correo electrónico, una libreta de direcciones o cualquier aplicación que se base en un diseño de detalles de lista.
-   Buscar y priorizar una gran colección de contenido.
-   Permitir la adición y eliminación rápidas de elementos de una lista mientras se trabaja cambiando entre contextos continuamente.

## <a name="choose-the-right-style"></a>Elegir el estilo adecuado

Al implementar el patrón de maestro y detalles, te recomendamos que uses el estilo apilado o el estilo en paralelo, según la cantidad de espacio disponible en pantalla.

| Ancho de ventana disponible | Estilo recomendado |
|------------------------|-------------------|
| 320 epx - 640 epx        | Apilado           |
| 641 epx o más ancho       | En paralelo      |

 
## <a name="stacked-style"></a>Estilo apilado

En el estilo apilado, solo hay un panel visible a la vez: el maestro o el de detalles.

![Vista de maestro y detalles en el modo apilado](images/patterns-md-stacked.png)

El usuario comienza en el panel maestro y "explora en profundidad" hasta el panel de detalles mediante la selección de un elemento de la lista maestra. Para el usuario, parece que las vistas de maestro y detalles existen en dos páginas independientes.

### <a name="create-a-stacked-masterdetails-pattern"></a>Crear un patrón de maestro y detalles apilado

Una forma de crear el patrón de maestro y detalles apilado es usar páginas independientes para el panel maestro y el de detalles. Coloca la vista maestra en una página, y el panel de detalles en una página independiente.

![Partes del panel de detalles de estilo apilado](images/patterns-md-stacked-parts.png)

En la página de vista maestra, un control de [vista de lista](lists.md) funciona bien para presentar listas que pueden contener imágenes y texto. 

En la página de vista de detalles, usa el [elemento de contenido](../layout/layout-panels.md) que sea más apropiado. Si tienes muchos campos independientes, considera la posibilidad de usar un diseño de **cuadrícula** para organizar los elementos en un formulario.

Para la navegación entre páginas, consulta el [historial de navegación y navegación hacia atrás para las aplicaciones de Windows](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Estilo en paralelo

En el estilo en paralelo, los paneles de maestro y de detalles están visibles al mismo tiempo.

![Patrón de maestro y detalles](images/patterns-masterdetail-400x227.png)

La lista del panel de maestro presenta un elemento visual de selección para indicar el elemento seleccionado actualmente. Al seleccionar un elemento nuevo en la lista maestra, se actualiza el panel de detalles.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Crear un patrón de maestro y detalles en paralelo

Una forma de crear un patrón de maestro y detalles en paralelo es usar el control [vista en dos paneles](split-view.md). Coloca la vista maestra en el panel de vista en dos paneles, y la vista de detalles en el contenido de vista en dos paneles.

![partes de la vista en dos paneles de la vista maestra y detalles](images/patterns_md_splitview_parts.png)

En el panel maestro, un control de [vista de lista](lists.md) es adecuado para presentar listas que puedan contener imágenes y texto.

En el contenido de detalles, usa el [elemento de contenido](../layout/layout-panels.md) que sea más apropiado. Si tienes muchos campos independientes, considera la posibilidad de usar un diseño de **cuadrícula** para organizar los elementos en un formulario.

## <a name="adaptive-layout"></a>Diseño adaptativo

Para implementar un patrón de maestro y detalles para cualquier tamaño de pantalla, crea una interfaz de usuario con capacidad de respuesta y con [diseño adaptativo](../layout/layouts-with-xaml.md).

![diseño adaptativo de vista maestra y detalles](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Crear un patrón adaptativo de maestro y detalles
Para crear un diseño adaptativo, define diferentes [**VisualStates**](/uwp/api/windows.ui.xaml.visualstate) para la interfaz de usuario y declara puntos de interrupción para los distintos estados con [**AdaptiveTriggers**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

Los siguientes ejemplos implementan el patrón de maestro y detalles con diseños adaptativos, y muestran datos enlazados a recursos estáticos, de bases de datos y en línea: 
- [Ejemplo de maestro y detalles](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [Ejemplo de panel maestro y detalles con selección](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Ejemplo de panel maestro y detalles de Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Ejemplo de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [Ejemplo de lector RSS](https://github.com/Microsoft/Windows-appsample-rssreader)

> [!TIP]
> Si deseas usar un control XAML que implemente este patrón automáticamente, se recomienda el [control XAML MasterDetailsView](/windows/communitytoolkit/controls/masterdetailsview) del kit de herramientas de la comunidad de Windows.

## <a name="related-articles"></a>Artículos relacionados

- [Listas](lists.md)
- [Búsqueda](search.md)
- [Barras de la aplicación y de comandos](app-bars.md)
- [Clase ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Clase SplitView](/uwp/api/windows.ui.xaml.controls.splitview)