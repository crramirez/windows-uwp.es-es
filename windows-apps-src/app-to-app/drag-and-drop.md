---
description: "En este artículo se explica cómo agregar la funcionalidad de arrastrar y colocar elementos en la aplicación para la Plataforma universal de Windows (UWP)."
title: Arrastrar y colocar
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
ms.sourcegitcommit: 03f3f86ed1310e6e3ac5f53cc5e81ebef708a1a2
ms.openlocfilehash: ffa2f0f368a61ef4f3003c1fa03e143b26c6859b

---
# Arrastrar y colocar

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este artículo se explica cómo agregar la funcionalidad de arrastrar y colocar elementos en la aplicación para la Plataforma universal de Windows (UWP). Arrastrar y soltar es una forma clásica y natural de interactuar con contenido, como imágenes y archivos. Una vez implementada, la opción para arrastrar y colocar elementos funciona perfectamente en todos los sentidos; por ejemplo, de una aplicación a otra, de una aplicación al escritorio y del escritorio a una aplicación.

## Establecer áreas válidas

Usa las propiedades [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) y [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) para designar las áreas de la aplicación que son válidas para arrastrar y colocar.

En el siguiente marcado se muestra cómo establecer un área específica de la aplicación como un valor válido para colocar usando [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) en XAML. Si un usuario intenta colocar elementos en otro lugar, el sistema no se lo permitirá. Si quieres que los usuarios puedan colocar elementos en cualquier parte de la aplicación, establece todo el fondo como destino para colocar elementos.

[!code-xml[Principal](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

Con la opción para arrastrar elementos, seguramente quieras especificar qué elementos se pueden arrastrar. Probablemente los usuarios quieran arrastrar tan solo determinados elementos, como imágenes, y no todo el contenido de la aplicación. Aquí te mostramos cómo establecer [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) mediante XAML.

[!code-xml[Principal](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

No es necesario realizar ninguna otra acción para poder arrastrar elementos, a menos que quieras personalizar la interfaz de usuario (de la cual hablamos más adelante en este artículo). La opción para colocar elementos requiere algunos pasos más.

## Controlar el evento DragOver

El evento [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) se desencadena cuando un usuario arrastra un elemento sobre la aplicación, pero aún no lo ha colocado. En este controlador, debes especificar qué tipo de operaciones admite la aplicación mediante el uso de la propiedad [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation). La opción Copiar es la más común.

[!code-cs[Principal](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## Procesar el evento Drop

El evento [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) se produce cuando el usuario suelta elementos en un área de colocación válida. Procésalos con la propiedad [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView).

Para facilitar las cosas, en el siguiente ejemplo supondremos que el usuario ha colocado una sola foto y accede a ella. En realidad, los usuarios pueden colocar simultáneamente varios elementos de diferentes formatos. Para gestionar esta posibilidad, la aplicación debe comprobar qué tipos de archivos se han colocado y procesarlos en consecuencia. Además, debe notificar al usuario si este está tratando de hacer algo que la aplicación no admite.

[!code-cs[Principal](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## Personalizar la interfaz de usuario

El sistema proporciona una interfaz de usuario predeterminada para arrastrar y colocar elementos. Sin embargo, puedes personalizar diversas partes de la interfaz de usuario mediante el establecimiento de títulos y glifos personalizados o, directamente, optar por no mostrar ninguna interfaz de usuario. Para personalizar la interfaz de usuario, usa la propiedad [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride).

[!code-cs[Principal](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## Consulta también

* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUiOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)


<!--HONumber=Jun16_HO4-->


