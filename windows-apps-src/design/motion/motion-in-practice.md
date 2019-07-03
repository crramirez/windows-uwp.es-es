---
Description: Obtenga información sobre cómo Fluent movimiento Fundamentos se fusionan en la aplicación.
title: 'Movimiento en la práctica: animación en las aplicaciones para UWP'
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8cf010533d2d62559bb8dc0d214e04ab917e62bd
ms.sourcegitcommit: d534f81590d881a18d677a648c59913029837a84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67535437"
---
# <a name="bringing-it-together"></a>Reunión de todo

El tiempo, la aceleración, la direccionalidad y la gravedad trabajan de manera conjunta para formar la base del movimiento de Fluent. Cada uno de ellos se tiene en cuenta en el contexto de los demás y se debe aplicar de manera adecuada en el contexto de tu aplicación.

Hay 3 maneras de aplicar los conceptos básicos del movimiento de Fluent en tu aplicación.

:::row:::
    :::column:::
**Animación implícito** interpolación automática y los intervalos entre los valores de un cambio de parámetro para lograr el movimiento de Fluent muy sencilla con los valores normalizados.
    :::column-end:::
    :::column:::
**Animación integrada** componentes del sistema, como los controles comunes y movimiento compartido, son "Fluent de forma predeterminada". Se han aplicado los aspectos básicos de manera coherente con su uso implícito.
    :::column-end:::
    :::column:::
**Si sigue las recomendaciones de orientación de animación personalizada** puede haber ocasiones cuando el sistema no aún proporciona una solución de movimiento exactos para su escenario. En esos casos, use las recomendaciones fundamentales de la línea de base como punto de partida para sus experiencias.
    :::column-end:::
:::row-end:::

**Ejemplo de transición**

![animación funcional](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Dirección de salida hacia delante:</b><br>
Fundido de salida: 150m; Aceleración: Acelere predeterminada <b>avanzando en:</b><br>
Diapositiva de 150px: 300 ms; Aceleración: Reducir la velocidad predeterminada
    :::column-end:::
    :::column:::
<b>Con versiones anteriores a la dirección:</b><br>
Deslice hacia abajo 150 px: 150 MS; Aceleración: Acelere predeterminada <b>dirección hacia atrás en:</b><br>
Fundido de entrada: 300 ms; Aceleración: Reducir la velocidad predeterminada
    :::column-end:::
:::row-end:::

**Ejemplo de objeto**

 ![Movimiento de 300 milisegundos](images/control.gif)

:::row:::
    :::column:::
<b>Expanda la dirección:</b><br>
Crecimiento: 300 ms; Aceleración: Standard
    :::column-end:::
    :::column:::
<b>Contrato de dirección:</b><br>
Crecimiento: 150 MS; Aceleración: Acelerar la predeterminada
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Ejemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/ImplicitTransition">abra la aplicación y ver las transiciones implícitas en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Animaciones implícitas

> Las animaciones implícitas requieren Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o una versión posterior.

Las animaciones implícitas son una manera sencilla para lograr el movimiento de Fluent al interpolar automáticamente entre los valores antiguos y nuevos durante un cambio de parámetro.

Implícitamente, puede animar los cambios realizados en las siguientes propiedades:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacidad**
  - **Rotación**
  - **Escalar**
  - **traducción**

- [Borde](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter), o [Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **En segundo plano**

Todas las propiedades que pueden tener cambios implícitamente animados le corresponde una _transición_ propiedad. Para animar la propiedad, asignar un tipo de transición correspondiente _transición_ propiedad. Esta tabla se muestran los _transición_ propiedades y el tipo de transición que se usará para cada uno de ellos.

| Propiedad animada | Propiedad de transición | Tipo de transición implícita |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

En este ejemplo se muestra cómo usar la propiedad de opacidad y la transición para que el botón de fundido de entrada cuando el control está habilitado y el fundido de salida cuando se deshabilita.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Información general del movimiento](index.md)
- [Control de tiempo y de aceleración](timing-and-easing.md)
- [Direccionalidad y gravedad](directionality-and-gravity.md)
