---
description: En este artículo se explica cómo agregar arrastrar y colocar en la aplicación de Windows.
title: Arrastrar y colocar
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e661eb0859e9720e31fabb6e5a7b33857de28b7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363678"
---
# <a name="drag-and-drop"></a>Arrastrar y colocar

Arrastrar y colocar es una manera intuitiva de transferir datos dentro de una aplicación o entre aplicaciones en el escritorio de Windows. Arrastrar y colocar permite que el usuario transfiera datos entre aplicaciones o dentro de una aplicación mediante un gesto estándar (mantenga presionado el dedo con el dedo o pulse y haga clic con un mouse o un lápiz).

> **API importantes**: [propiedad CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag), [propiedad AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop) 

El origen de arrastre, que es la aplicación o el área donde se desencadena el movimiento de arrastre, proporciona los datos que se van a transferir rellenando un objeto de paquete de datos que puede contener formatos de datos estándar, incluidos texto, RTF, HTML, mapas de bits, elementos de almacenamiento o formatos de datos personalizados. El origen también indica el tipo de operaciones que admite: copiar, desplace o link. Cuando se libera el puntero, se produce la eliminación. El destino de colocación, que es la aplicación o área situada debajo del puntero, procesa el paquete de datos y devuelve el tipo de operación que realiza.

Durante la operación de arrastrar y colocar, la interfaz de usuario de arrastre proporciona una indicación visual del tipo de operación de arrastrar y colocar que se está llevando a cabo. El origen proporciona los comentarios visuales inicialmente, pero los destinos los pueden cambiar cuando el puntero se mueve sobre ellos.

La función de arrastrar y colocar moderno está disponible en todos los dispositivos que admiten UWP. Permite la transferencia de datos entre o dentro de cualquier tipo de aplicación, incluidas las aplicaciones clásicas de Windows, aunque este artículo se centra en la API XAML para arrastrar y colocar de forma moderna. Una vez implementada, la opción para arrastrar y colocar elementos funciona perfectamente en todos los sentidos; por ejemplo, de una aplicación a otra, de una aplicación al escritorio y del escritorio a una aplicación.

Aquí se muestra información general sobre lo que debe hacer para habilitar la función de arrastrar y colocar en la aplicación:

1. Habilite el arrastre en un elemento estableciendo su propiedad **CanDrag** en true.  
2. Cree el paquete de datos. El sistema controla las imágenes y el texto automáticamente, pero para el resto de contenido, deberá controlar los eventos **DragStarted** y **DragCompleted** y usarlos para construir su propio paquete de datos. 
3. Habilite la eliminación estableciendo la propiedad **AllowDrop** en **true** en todos los elementos que pueden recibir contenido quitado. 
4. Controle el evento **DragOver** para que el sistema sepa qué tipo de operaciones de arrastre puede recibir el elemento. 
5. Procese el evento **Drop** para recibir el contenido quitado. 



## <a name="enable-dragging"></a>Habilitar arrastre

Para habilitar el arrastre en un elemento, establezca la propiedad [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag) en **true**. Esto hace que el elemento, y los elementos que contiene, en el caso de colecciones como ListView--arrastrable.

Sea específico de lo que se puede arrastrar. Los usuarios no querrán arrastrar todo en la aplicación, solo algunos elementos, como imágenes o texto. 

Aquí se muestra cómo establecer [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag).

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDragArea":::

No es necesario realizar ninguna otra acción para poder arrastrar elementos, a menos que quieras personalizar la interfaz de usuario (de la cual hablamos más adelante en este artículo). La opción para colocar elementos requiere algunos pasos más.

## <a name="construct-a-data-package"></a>Construir un paquete de datos 

En la mayoría de los casos, el sistema creará un paquete de datos automáticamente. El sistema controla automáticamente:
* Imágenes
* Texto 

Para el resto de contenido, deberá controlar los eventos **DragStarted** y **DragCompleted** y usarlos para construir su propio [paquete de objetos](/uwp/api/windows.applicationmodel.datatransfer.datapackage).

## <a name="enable-dropping"></a>Habilitar eliminación

En el siguiente marcado se muestra cómo establecer un área específica de la aplicación como un valor válido para la operación de colocar mediante [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) en XAML. Si un usuario intenta colocar elementos en otro lugar, el sistema no se lo permitirá. Si quieres que los usuarios puedan colocar elementos en cualquier parte de la aplicación, establece todo el fondo como destino para colocar elementos.

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDropArea":::


## <a name="handle-the-dragover-event"></a>Controlar el evento DragOver

El evento [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover) se desencadena cuando un usuario arrastra un elemento sobre la aplicación, pero aún no lo ha colocado. En este controlador, debes especificar qué tipo de operaciones admite la aplicación mediante el uso de la propiedad [**AcceptedOperation**](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation). La opción Copiar es la más común.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOver":::

## <a name="process-the-drop-event"></a>Procesar el evento Drop

El evento [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) se produce cuando el usuario suelta elementos en un área de colocación válida. Procésalos con la propiedad [**DataView**](/uwp/api/windows.ui.xaml.drageventargs.dataview).

Para simplificar en el ejemplo siguiente, se supone que el usuario ha quitado una sola foto y ha tenido acceso a ella directamente. En realidad, los usuarios pueden colocar simultáneamente varios elementos de diferentes formatos. La aplicación debe controlar esta posibilidad comprobando qué tipos de archivos se han quitado y cuántos hay y procesar cada uno de ellos en consecuencia. También debe considerar la posibilidad de notificar al usuario si está intentando hacer algo que la aplicación no admite.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_Drop":::

## <a name="customize-the-ui"></a>Personalización de la interfaz de usuario

El sistema proporciona una interfaz de usuario predeterminada para arrastrar y colocar elementos. Sin embargo, puedes personalizar diversas partes de la interfaz de usuario mediante el establecimiento de títulos y glifos personalizados o, directamente, optar por no mostrar ninguna interfaz de usuario. Para personalizar la interfaz de usuario, usa la propiedad [**DragEventArgs.DragUIOverride**](/uwp/api/windows.ui.xaml.drageventargs.draguioverride).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOverCustom":::

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Abrir un menú contextual de un elemento que se pueda arrastrar con la función táctil

Cuando se usa la función táctil, las acciones de arrastrar una clase [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) y abrir su menú contextual comparten gestos táctiles similares, ya que cada uno comienza con la operación de mantener presionado. A continuación te mostramos cómo el sistema elimina la ambigüedad entre las dos acciones para los elementos de la aplicación que admiten ambas: 

* Si un usuario mantiene presionado un elemento y comienza a arrastrar dentro de los 500 milisegundos, se arrastra el elemento y no se muestra el menú contextual. 
* Si el usuario mantiene presionado pero no arrastra dentro de los 500 milisegundos, se abre el menú contextual. 
* Una vez abierto el menú contextual, si el usuario intenta arrastrar el elemento (sin levantar el dedo), se descarta el menú contextual y se iniciará la operación de arrastrar.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Designar un elemento en una clase ListView o GridView como una carpeta

Puedes especificar una clase [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) o [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) como una carpeta. Esto es especialmente útil para escenarios de TreeView y Explorador de archivos. Para ello, establece explícitamente la propiedad [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) en **True** en ese elemento. 

El sistema mostrará automáticamente las animaciones adecuadas para colocar en una carpeta en vez de un elemento sin carpeta. El código de tu aplicación debe seguir controlando el evento [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) en el elemento de la carpeta (así como en el elemento sin carpeta) para actualizar el origen de datos y agregar el elemento a la carpeta de destino.

## <a name="implementing-custom-drag-and-drop"></a>Implementar la función de arrastrar y colocar personalizada

La clase [UIElement](/uwp/api/windows.ui.xaml.uielement) realiza la mayor parte del trabajo de implementación de la función de arrastrar y colocar. Pero si lo desea, puede implementar su propia versión mediante las API del [espacio de nombres Windows. ApplicationModel. datatransfer. DragDrop. Core](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core).

| Funcionalidad | API de WinRT |
| --- | --- |
|  Habilitar arrastre | [CoreDragOperation](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Crear un paquete de datos | [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Entrega de un arrastre al shell  | [CoreDragOperation.StartAsync](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Recepción de la entrega desde el shell  | [CoreDragDropManager](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Consulte también

* [Comunicación entre aplicaciones](index.md)
* [AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop)
* [CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag)
* [DragOver](/uwp/api/windows.ui.xaml.uielement.dragover)
* [AcceptedOperation](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)
* [DataView](/uwp/api/windows.ui.xaml.drageventargs.dataview)
* [DragUIOverride](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)
* [Omisiones](/uwp/api/windows.ui.xaml.uielement.drop)
* [IsDragSource](/uwp/api/windows.ui.xaml.controls.listviewbase.isdragsource)
