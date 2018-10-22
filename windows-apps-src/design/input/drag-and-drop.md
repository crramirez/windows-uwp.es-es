---
description: En este artículo se explica cómo agregar la funcionalidad de arrastrar y colocar elementos en la aplicación para la Plataforma universal de Windows (UWP).
title: Arrastrar y colocar
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a4685a891facab39cb984f0b2d5f697e22477233
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5398426"
---
# <a name="drag-and-drop"></a>Arrastrar y colocar

Arrastrar y colocar es una forma intuitiva de transferir datos dentro de una aplicación o entre aplicaciones en el escritorio de Windows. Arrastrar y colocar permite al usuario transferir datos entre aplicaciones o dentro de una aplicación mediante un gesto estándar (presionar, colocar y aplicar panorámica con el dedo o presionar y aplicar panorámica con un mouse o un lápiz).

> **API importantes**: [CanDrag property](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag), [AllowDrop property](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 

El origen de arrastre, que es la aplicación o área donde se desencadena el gesto de arrastre, proporciona los datos que se van a transferir rellenando un objeto de paquete de datos que puede contener formatos de datos estándar, incluidos texto, RTF, HTML, mapas de bits, elementos de almacenamiento o formatos de datos personalizados. El origen también indica el tipo de operaciones que admite: copiar, mover o enlazar. Cuando se libera el puntero, se produce la colocación. El destino para colocar, que es la aplicación o área situada debajo del puntero, procesa el paquete de datos y devuelve el tipo de operación que realizó.

Durante la operación de arrastrar y colocar, la interfaz de usuario de arrastre proporciona una indicación visual del tipo de operación de arrastrar y colocar que se está llevando a cabo. El origen proporciona inicialmente esta información visual, pero los destinos pueden cambiarla a medida que el puntero pasa por ellos.

La operación de arrastrar y colocar moderna está disponible en todos los dispositivos que admiten UWP. Permite la transferencia de datos entre o dentro de cualquier tipo de aplicación, incluidas las aplicaciones clásicas de Windows, aunque este artículo se centra en la API de XAML para la operación de arrastrar y colocar moderna. Una vez implementada, la opción para arrastrar y colocar elementos funciona perfectamente en todos los sentidos; por ejemplo, de una aplicación a otra, de una aplicación al escritorio y del escritorio a una aplicación.

Esta es una introducción de lo que debes hacer para habilitar arrastrar y colocar en tu aplicación:

1. Habilita el arrastre en un elemento estableciendo su propiedad **CanDrag** en true.  
2. Crea el paquete de datos. El sistema controla imágenes y texto de forma automática, pero, para otro contenido, deberás controlar los eventos **DragStarted** y **DragCompleted** y usarlos para crear tu propio paquete de datos. 
3. Habilita la operación de colocar estableciendo la propiedad **AllowDrop** en **true** en todos los elementos que pueden recibir contenido descartado. 
4. Controla el evento **DragOver** para informar al sistema del tipo de operaciones de arrastrar que puede recibir el elemento. 
5. Procesa el evento **Drop** para recibir el contenido descartado. 



## <a name="enable-dragging"></a>Habilitar el arrastre

Para habilitar el arrastre en un elemento, establece su propiedad [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) en **true**. Esto hace que el elemento (y los elementos que contiene, en el caso de colecciones como ListView) se pueda arrastrar.

Especifica qué elementos se pueden arrastrar. Probablemente los usuarios no quieran arrastrar todo el contenido de la aplicación, sino solo determinados elementos, como imágenes o texto. 

Aquí te mostramos cómo establecer [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag).

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

No es necesario realizar ninguna otra acción para poder arrastrar elementos, a menos que quieras personalizar la interfaz de usuario (de la cual hablamos más adelante en este artículo). La opción para colocar elementos requiere algunos pasos más.

## <a name="construct-a-data-package"></a>Crear un paquete de datos 

En la mayoría de los casos, el sistema creará un paquete de datos de forma automática. El sistema controla automáticamente lo siguiente:
* Imágenes
* Texto 

Para otro contenido, deberás controlar los eventos **DragStarted** y **DragCompleted** y usarlos para crear tu propio [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage).

## <a name="enable-dropping"></a>Habilitar la operación de colocar

En la siguiente revisión se muestra cómo establecer un área específica de la aplicación como un valor válido para la operación de colocar mediante [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) en XAML. Si un usuario intenta colocar elementos en otro lugar, el sistema no se lo permitirá. Si quieres que los usuarios puedan colocar elementos en cualquier parte de la aplicación, establece todo el fondo como destino para colocar elementos.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>Controlar el evento DragOver

El evento [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) se desencadena cuando un usuario arrastra un elemento sobre la aplicación, pero aún no lo ha colocado. En este controlador, debes especificar qué tipo de operaciones admite la aplicación mediante el uso de la propiedad [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation). La opción Copiar es la más común.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Procesar el evento Drop

El evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) se produce cuando el usuario suelta elementos en un área de colocación válida. Procésalos con la propiedad [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView).

Para facilitar las cosas, en el siguiente ejemplo supondremos que el usuario ha colocado una sola foto y obtiene acceso a ella directamente. En realidad, los usuarios pueden colocar simultáneamente varios elementos de diferentes formatos. Tu aplicación debe gestionar esta posibilidad comprobando qué tipos de archivos se han colocado y cuántos hay, además de procesar cada uno según corresponda. También debes considerar la posibilidad de notificar al usuario si este está tratando de hacer algo que la aplicación no admite.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>Personalizar la interfaz de usuario

El sistema proporciona una interfaz de usuario predeterminada para arrastrar y colocar elementos. Sin embargo, puedes personalizar diversas partes de la interfaz de usuario mediante el establecimiento de títulos y glifos personalizados o, directamente, optar por no mostrar ninguna interfaz de usuario. Para personalizar la interfaz de usuario, usa la propiedad [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride).

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Abrir un menú contextual de un elemento que se pueda arrastrar con la función táctil

Cuando se usa la función táctil, las acciones de arrastrar una clase [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) y abrir su menú contextual comparten gestos táctiles similares, ya que cada uno comienza con la operación de mantener presionado. A continuación te mostramos cómo el sistema elimina la ambigüedad entre las dos acciones para los elementos de la aplicación que admiten ambas: 

* Si un usuario mantiene presionado un elemento y comienza a arrastrar dentro de los 500 milisegundos, se arrastra el elemento y no se muestra el menú contextual. 
* Si el usuario mantiene presionado pero no arrastra dentro de los 500 milisegundos, se abre el menú contextual. 
* Una vez abierto el menú contextual, si el usuario intenta arrastrar el elemento (sin levantar el dedo), se descarta el menú contextual y se iniciará la operación de arrastrar.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Designar un elemento en una clase ListView o GridView como una carpeta

Puedes especificar una clase [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) o [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) como una carpeta. Esto es especialmente útil para escenarios de TreeView y Explorador de archivos. Para ello, establece explícitamente la propiedad [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) en **True** en ese elemento. 

El sistema mostrará automáticamente las animaciones adecuadas para colocar en una carpeta en vez de un elemento sin carpeta. El código de tu aplicación debe seguir controlando el evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) en el elemento de la carpeta (así como en el elemento sin carpeta) para actualizar el origen de datos y agregar el elemento a la carpeta de destino.

## <a name="implementing-custom-drag-and-drop"></a>Implementación de la operación de arrastrar y colocar personalizada

La clase [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) realiza la mayor parte del trabajo de implementación de arrastrar y colocar automáticamente Pero si quieres, puedes implementar su propia versión usando las API en el [espacio de nombres Windows.ApplicationModel.DataTransfer.DragDrop.Core](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core).

| Funcionalidad | API de WinRT |
| --- | --- |
|  Habilitar el arrastre | [CoreDragOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Crear un paquete de datos | [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Arrastre de entrega al shell  | [CoreDragOperation.StartAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Recibir colocación desde el shell  | [CoreDragDropManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Ver también

* [Comunicación entre aplicaciones](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
