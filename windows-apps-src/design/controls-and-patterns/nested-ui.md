---
Description: Use nested UI to enable multiple actions on a list item
title: Interfaz de usuario anidada en elementos de lista
label: Nested UI in list items
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 60a29717-56f2-4388-a9ff-0098e34d5896
pm-contact: chigy
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8edb38b8ae91d836e283a8eb37830850bf504db4
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7854919"
---
# <a name="nested-ui-in-list-items"></a>Interfaz de usuario anidada en elementos de lista

 

La interfaz de usuario anidada es una interfaz de usuario (IU) que expone los controles accionables anidados incluidos dentro de un contenedor que también puede presentar un foco independiente.

Puedes usar la interfaz de usuario anidada para presentar a un usuario las opciones adicionales que le ayudarán a acelerar la realización de acciones importantes. Sin embargo, cuantas más acciones expongas, más complicada será la interfaz de usuario. Debes prestar especial atención si decides usar este patrón de interfaz de usuario. En este artículo se proporcionan directrices para ayudar a determinar el mejor curso de acción para tu interfaz de usuario concreta.

> **API importantes**: [Clase ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [Clase GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

En este artículo, se trata la creación de la interfaz de usuario anidada en los elementos [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) y [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx). Aunque en esta sección no se tratan otros casos de interfaz de usuario anidada, estos conceptos son transferibles. Antes de comenzar, debes estar familiarizado con las instrucciones generales para usar los controles ListView o GridView en la interfaz de usuario, que encontrarás en los artículos [Listas](lists.md) y [Vista de lista y vista de cuadrícula](listview-and-gridview.md).

En este artículo, se usan los términos *lista*, *elemento de lista* e *interfaz de usuario anidada* tal como se definen aquí:
- *Lista* hace referencia a una colección de elementos incluidos en una vista de lista o una vista de cuadrícula.
- *Elemento de lista* hace referencia a un elemento individual sobre el que un usuario puede actuar en una lista.
- *Interfaz de usuario anidada* hace referencia a los elementos de interfaz de usuario de un elemento de lista sobre los que un usuario puede actuar de manera independiente a las acciones sobre el propio elemento de lista.

![Partes de la interfaz de usuario anidada](images/nested-ui-example-1.png)

> NOTA&nbsp;&nbsp; ListView y GridView derivan ambos de la clase [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), por lo que tienen la misma funcionalidad, pero muestran los datos de manera diferente. En este artículo, cuando hablamos sobre las listas, la información corresponde a ambos controles, ListView y GridView.

## <a name="primary-and-secondary-actions"></a>Acciones principales y secundarias

Al crear la interfaz de usuario con una lista, ten en cuenta las acciones que el usuario puede llevar a cabo desde esos elementos de lista.  

- ¿Un usuario puede hacer clic en el elemento para realizar una acción?
    - Por lo general, al hacer clic en un elemento de lista se inicia una acción, pero no es obligatorio.
- ¿Hay más de una acción que el usuario pueda llevar a cabo?
    - Por ejemplo, al pulsar un correo electrónico en una lista se abre ese correo electrónico. Sin embargo, es posible que haya otras acciones, como eliminar correo electrónico, que el usuario quiera realizar sin abrirlo antes. Beneficiaría al usuario poder acceder a esta acción directamente en la lista.
- ¿Cómo se deben exponer las acciones al usuario?
    - Ten en cuenta todos los tipos de entrada. Algunas formas de interfaz de usuario anidada funcionan perfectamente con un método de entrada, pero podrían no funcionar con otros.  

La *acción principal* es lo que el usuario espera que suceda al pulsar el elemento de lista.

Las *acciones secundarias* suelen ser aceleradores asociados a elementos de lista. Estos aceleradores pueden ser para la administración de la lista o acciones relacionadas con el elemento de lista.

## <a name="options-for-secondary-actions"></a>Opciones de acciones secundarias

Al crear la interfaz de usuario de la lista, primero debes tener en cuenta todos los métodos de entrada que admite UWP. Para obtener más información sobre los distintos tipos de entrada, consulta [Información básica de entradas](../input/index.md).

Después de asegurarte de que la aplicación admite todas las entradas que admite UWP, debes decidir si las acciones secundarias de la aplicación son lo suficientemente importantes para exponerlas como aceleradores en la lista principal. Recuerda que cuantas más acciones expongas, más complicada será la interfaz de usuario. ¿Necesitas realmente exponer las acciones secundarias en la interfaz de usuario de la lista principal o puedes ponerlas en otro lugar?

Considera la posibilidad de exponer acciones adicionales en la interfaz de usuario de la lista principal cuando esas acciones deban ser accesibles para cualquier entrada en todo momento.

Si decides que no es necesario colocar acciones secundarias en la interfaz de usuario de la lista principal, hay otras formas de exponerlas al usuario. Estas son algunas opciones que puedes tener en cuenta para colocar acciones secundarias.

### <a name="put-secondary-actions-on-the-detail-page"></a>Colocar acciones secundarias en la página de detalles

Coloca las acciones secundarias en la página a la que navega el elemento de lista al pulsarlo. Si usas el patrón de maestro y detalles, la página de detalles suele ser un buen lugar para colocar acciones secundarias.

Para obtener más información, consulta [Patrón maestro y detalles](master-details.md).

### <a name="put-secondary-actions-in-a-context-menu"></a>Colocar acciones secundarias en un menú contextual

Coloca las acciones secundarias en un menú contextual al que el usuario pueda acceder mediante un clic con el botón derecho o mediante pulsar y sostener. De este modo, el usuario podrá realizar una acción, como eliminar un correo electrónico, sin tener que cargar la página de detalles. También se recomienda que estas opciones estén disponibles en la página de detalles, ya que los menús contextuales están destinados a ser aceleradores en lugar de la interfaz de usuario principal.

Para exponer las acciones secundarias cuando la entrada proviene de un controlador para juegos o un control remoto, se recomienda usar un menú contextual.

Para obtener más información, consulta [Menús contextuales y controles flotantes](menus.md).

### <a name="put-secondary-actions-in-hover-ui-to-optimize-for-pointer-input"></a>Colocar acciones secundarias en la interfaz de usuario al mantener el mouse para optimizar la entrada de puntero

Si prevés que la aplicación se usará con frecuencia con la entrada de puntero, como, por ejemplo, con el mouse y el lápiz, y quieres que las acciones secundarias estén disponibles para esas entradas, puedes mostrar las acciones secundarias solo al mantener el mouse. Este acelerador está visible solo cuando se usa una entrada de puntero, por lo que debes asegurarte de usar las otras opciones para admitir también otros tipos de entrada.

![Interfaz de usuario anidada al mantener el mouse](images/nested-ui-hover.png)


Para obtener más información, consulta [Interacciones de mouse](../input/mouse-interactions.md).

## <a name="ui-placement-for-primary-and-secondary-actions"></a>Colocación de la interfaz de usuario para las acciones principales y secundarias

Si decides que las acciones secundarias se deben exponer en la interfaz de usuario de la lista principal, se recomienda seguir las directrices siguientes.

Al crear un elemento de lista con acciones principales y secundarias, coloca la acción principal a la izquierda y las acciones secundarias a la derecha. Para las culturas que leen de izquierda a derecha, los usuarios asocian las acciones en el lado izquierdo del elemento de lista como la acción principal.

En estos ejemplos, hablaremos sobre la interfaz de usuario de la lista donde el elemento fluye más horizontalmente (es más ancho que alto). Sin embargo, es posible que tengas elementos de lista de forma más cuadrada, o más altos que anchos. Por lo general, estos son los elementos que se usan en una cuadrícula. Para estos elementos, si la lista no se desplaza verticalmente, puedes colocar las acciones secundarias en la parte inferior del elemento de lista, en lugar de colocarlas a la derecha.

## <a name="consider-all-inputs"></a>Ten en cuenta todas las entradas

Si decides usar la interfaz de usuario anidada, evalúa también la experiencia del usuario con todos los tipos de entrada. Como se mencionó anteriormente, la interfaz de usuario anidada funciona a la perfección para algunos tipos de entrada. Sin embargo, no siempre funciona bien con otros. En particular, las entradas de teclado, controlador y control remoto pueden tener dificultades para acceder a los elementos de la interfaz de usuario anidada. Asegúrate de seguir las instrucciones siguientes para garantizar que UWP funcione con todos los tipos de entrada.

## <a name="nested-ui-handling"></a>Control de la interfaz de usuario anidada

Si tienes más de una acción anidada en el elemento de lista, se recomienda seguir estas instrucciones para controlar la navegación con la entrada de teclado, controlador para juegos, control remoto u otra entrada que no sea de puntero.

### <a name="nested-ui-where-list-items-perform-an-action"></a>Interfaz de usuario anidada donde los elementos de lista realizan una acción

Si la interfaz de usuario de la lista con elementos anidados admite acciones como invocar y seleccionar (uno o varios elementos) o las operaciones de arrastrar y colocar, se recomiendan estas técnicas direccionales para navegar por los elementos de la interfaz de usuario anidada.

![Partes de la interfaz de usuario anidada](images/nested-ui-navigation.png)

**Controlador para juegos**

Cuando la entrada proviene de un controlador para juegos, proporciona esta experiencia del usuario:

- Desde **A**, la tecla de dirección derecha coloca el foco en **B**.
- Desde **B**, la tecla de dirección derecha coloca el foco en **C**.
- Desde **C**, la tecla de dirección derecha no está operativa, o bien, si hay un elemento de interfaz de usuario activable a la derecha de Lista, coloca el foco en este.
- Desde **C**, la tecla de dirección izquierda coloca el foco en **B**.
- Desde **B**, la tecla de dirección izquierda coloca el foco en **A**.
- Desde **A**, la tecla de dirección izquierda no está operativa, o bien, si hay un elemento de interfaz de usuario activable a la derecha de Lista, coloca el foco en este.
- Desde **A**, **B** o **C**, la tecla de dirección abajo coloca el foco en **D**.
- Desde el elemento de interfaz de usuario a la izquierda de Elemento de lista, la tecla de dirección derecha coloca el foco en **A**.
- Desde el elemento de interfaz de usuario a la derecha de Elemento de lista, la tecla de dirección izquierda coloca el foco en **A**.

**Teclado**

Si la entrada proviene de un teclado, esta es la experiencia que obtiene el usuario:

- Desde **A**, la tecla TAB coloca el foco en **B**.
- Desde **B**, la tecla TAB coloca el foco en **C**.
- Desde **C**, la tecla TAB coloca el foco en el siguiente elemento activable de la interfaz de usuario en el orden de tabulación.
- Desde **C**, las teclas Mayús+TAB colocan el foco en **B**.
- Desde **B**, las teclas Mayús+TAB o de flecha izquierda colocan el foco en **A**.
- Desde **A**, las teclas Mayús+TAB colocan el foco en el siguiente elemento de interfaz de usuario activable en el orden de tabulación inverso.
- Desde **A**, **B** o **C**, la tecla de dirección abajo sitúa el foco en **D**.
- Desde el elemento de interfaz de usuario a la izquierda de Elemento de lista, la tecla TAB coloca el foco en **A**.
- Desde el elemento de interfaz de usuario a la derecha de Elemento de lista, las teclas Mayús+TAB colocan el foco en **C**.

Para conseguir esta interfaz de usuario, establece [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) en **true** en la lista. [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) puede ser cualquier valor.

Para que el código lo implemente, consulta la sección [Ejemplo](#example) de este artículo.

### <a name="nested-ui-where-list-items-do-not-perform-an-action"></a>Interfaz de usuario anidada donde los elementos de lista no realizan ninguna acción

Puedes usar una vista de lista, porque proporciona virtualización y un comportamiento de desplazamiento optimizado, sin tener una acción asociada con un elemento de lista. Por lo general, estas interfaces de usuario usan el elemento de lista solo para agrupar elementos y garantizar que se desplazan como un conjunto.

Este tipo de interfaz de usuario tiende a ser mucho más complicado que los ejemplos anteriores, con una gran cantidad de elementos anidados sobre los que el usuario puede actuar.

![Partes de la interfaz de usuario anidada](images/nested-ui-grouping.png)


Para conseguir esta interfaz de usuario, establece las siguientes propiedades en la lista:
- [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) en **None**.
- [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) en **false**.
- [IsFocusEngagementEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isfocusengagementenabled.aspx) en **true**.

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Cuando los elementos de lista no realizan ninguna acción, se recomienda seguir estas instrucciones para controlar la navegación con un controlador para juegos o un teclado.

**Controlador para juegos**

Cuando la entrada proviene de un controlador para juegos, proporciona esta experiencia del usuario:

- Desde Elemento de lista, la tecla de dirección abajo coloca el foco en el siguiente elemento de lista.
- Desde Elemento de lista, la tecla izquierda/derecha no está operativa o, si existe un elemento de interfaz de usuario activable a la derecha de Lista, coloca el foco en este.
- Desde Elemento de lista, el botón 'A' coloca el foco en la interfaz de usuario anidada en la prioridad arriba/abajo e izquierda/derecha.
- Dentro de la interfaz de usuario anidada, sigue el modelo de navegación de foco XY.  El foco solo puede navegar por la interfaz de usuario anidada dentro del elemento de lista actual hasta que el usuario pulse el botón 'B' y el foco se vuelva a colocar en el elemento de lista.

**Teclado**

Si la entrada proviene de un teclado, esta es la experiencia que obtiene el usuario:

- Desde Elemento de lista, la tecla de dirección abajo coloca el foco en el siguiente elemento de lista.
- Desde Elemento de lista, la tecla izquierda/derecha no está operativa.
- Desde Elemento de lista, al presionar la tecla TAB se coloca el foco en la siguiente tabulación entre el elemento de Interfaz de usuario anidada.
- Desde uno de los elementos de Interfaz de usuario anidada, al presionar la tecla TAB se recorren los elementos de Interfaz de usuario anidada en orden de tabulación.  Después de visitar todos los elementos de Interfaz de usuario anidada, se coloca el foco en el siguiente control en el orden de tabulación después de ListView.
- Las teclas Mayús+TAB realizan la acción inversa a la tecla TAB.

## <a name="example"></a>Ejemplo

En este ejemplo se muestra cómo implementar la [interfaz de usuario anidada donde los elementos de lista realizan una acción](#nested-ui-where-list-items-perform-an-action).

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```
