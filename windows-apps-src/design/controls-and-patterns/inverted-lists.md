---
Description: Usa una lista invertida para agregar nuevos elementos al final.
title: Listas invertidas
label: Inverted lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c552109b243688c2618425adce797c4d208eac31
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66364777"
---
# <a name="inverted-lists"></a>Listas invertidas

 

Puedes usar una vista de lista para presentar una conversación en una experiencia de chat con elementos que sean visualmente distintos para representar al remitente y al receptor.  El uso de colores diferentes y la alineación horizontal para separar los mensajes del remitente y el receptor ayuda al usuario a orientarse rápidamente en una conversación.

> **API importantes**:  [Clase ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [Clase ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel) y [Propiedad ItemsUpdatingScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode)
 
Por lo general, deberás presentar la lista de manera que parezca que crece de abajo arriba en lugar de hacerlo de arriba abajo.  Cuando llega un nuevo mensaje y se agrega al final, los mensajes anteriores se deslizan hacia arriba para dejar espacio, de modo que el usuario centre su atención en este último mensaje.  Sin embargo, si un usuario se desplaza hacia arriba para ver respuestas anteriores, la llegada de un nuevo mensaje no causará ningún cambio visual que afecte a su atención.

![Aplicación de chat con lista invertida](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>Crear una lista invertida

Para crear una lista invertida, usa una vista de lista con una clase [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel) como panel de elementos. En la clase ItemsStackPanel, establece [ItemsUpdatingScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode) en [KeepLastItemInView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsupdatingscrollmode).

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

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Alinea los mensajes del remitente o el receptor en lados opuestos para mostrar claramente el flujo de la conversación a los usuarios.
- Haz que los mensajes existentes desaparezcan de la vista para mostrar el último mensaje si el usuario ya está al final de la conversación en espera del siguiente mensaje.
- No muevas elementos de modo que perturbes la atención de los usuarios si no están leyendo el final de la conversación.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Ejemplo de lista XAML de abajo a arriba](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)