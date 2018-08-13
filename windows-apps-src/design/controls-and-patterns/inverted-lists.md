---
author: muhsinking
Description: Use an inverted list to add new items at the bottom.
title: Listas invertidas
label: Inverted lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: fddda4e870ba37d5ec3a2ec2a385e5f8c7c1ed9c
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
ms.locfileid: "1392824"
---
# <a name="inverted-lists"></a>Listas invertidas

 

Puedes usar una vista de lista para presentar una conversación en una experiencia de chat con elementos que sean visualmente distintos para representar al remitente y al receptor.  El uso de colores diferentes y la alineación horizontal para separar los mensajes del remitente y el receptor ayuda al usuario a orientarse rápidamente en una conversación.

> **API importantes**: [Clase ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [Clase ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), [Propiedad ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)
 
Por lo general, deberás presentar la lista de manera que parezca que crece de abajo arriba en lugar de hacerlo de arriba abajo.  Cuando llega un nuevo mensaje y se agrega al final, los mensajes anteriores se deslizan hacia arriba para dejar espacio, de modo que el usuario centre su atención en este último mensaje.  Sin embargo, si un usuario se desplaza hacia arriba para ver respuestas anteriores, la llegada de un nuevo mensaje no causará ningún cambio visual que afecte a su atención.

![Aplicación de chat con lista invertida](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>Crear una lista invertida

Para crear una lista invertida, usa una vista de lista con una clase [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) como panel de elementos. En la clase ItemsStackPanel, establece [ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) en [KeepLastItemInView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx).

> [!IMPORTANT]
> El valor de enumeración **KeepLastItemInView** está disponible a partir de Windows 10, versión 1607. Si la aplicación se ejecuta en versiones anteriores de Windows 10, no puedes usar este valor.

En este ejemplo se muestra cómo alinear los elementos de la vista de lista en la parte inferior y se indica que, al producirse un cambio en los elementos, el último elemento debe permanecer en la vista.
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer

- Alinea los mensajes del remitente o el receptor en lados opuestos para mostrar claramente el flujo de la conversación a los usuarios.
- Haz que los mensajes existentes desaparezcan de la vista para mostrar el último mensaje si el usuario ya está al final de la conversación en espera del siguiente mensaje.
- No muevas elementos de modo que perturbes la atención de los usuarios si no están leyendo el final de la conversación.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Muestra de lista XAML de abajo a arriba](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)