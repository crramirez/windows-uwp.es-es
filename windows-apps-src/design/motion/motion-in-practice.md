---
description: Obtenga información sobre cómo los aspectos básicos del movimiento fluida como el tiempo, la aceleración, la direccionalidad y la gravedad se unen en la aplicación.
title: 'Movimiento en la práctica: animación en aplicaciones de Windows'
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
ms.openlocfilehash: 8604d925ffefc96cd74726909afab6e2016cce76
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054535"
---
# <a name="bringing-it-together"></a>Reunir

Los intervalos, la aceleración, la direccionalidad y la gravedad funcionan juntos para formar la base del movimiento fluida. Cada uno tiene que considerarse en el contexto de los demás y aplicarse correctamente en el contexto de la aplicación.

A continuación se muestran tres maneras de aplicar aspectos básicos de movimiento de problemas en la aplicación.

:::row:::
    :::column:::
**Animación implícita** La interpolación automática y el tiempo entre los valores de un parámetro cambian para lograr un movimiento fluida muy sencillo mediante los valores normalizados.
    :::column-end:::
    :::column:::
**Animación integrada** Los componentes del sistema, como los controles comunes y el movimiento compartido, son "de forma predeterminada". Los aspectos básicos se han aplicado de manera coherente con su uso implícito.
    :::column-end:::
    :::column:::
**Animaciones personalizadas: recomendaciones** de la guía Puede haber ocasiones en las que el sistema aún no proporcione una solución de movimiento exacta para su escenario. En esos casos, use las recomendaciones fundamentales básicas como punto de partida para sus experiencias.
    :::column-end:::
:::row-end:::

**Ejemplo de transición**

![animación funcional](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Dirección de reenvío:</b><br>
Fundido de salida: 150m; Aceleración: aceleración predeterminada <b>hacia delante en:</b><br>
Deslizar hacia arriba 150 PX: 300 ms; Aceleración: deceleración predeterminada
    :::column-end:::
    :::column:::
<b>Dirección hacia atrás hacia atrás:</b><br>
Deslizar hacia abajo 150 PX: 150MS; Aceleración: acelerar la <b>dirección predeterminada en:</b><br>
Atenuación: 300 ms; Aceleración: deceleración predeterminada
    :::column-end:::
:::row-end:::

**Ejemplo de objeto**

 ![movimiento 300 ms](images/control.gif)

:::row:::
    :::column:::
<b>Expandir dirección:</b><br>
Crecimiento: 300 ms; Aceleración: estándar
    :::column-end:::
    :::column:::
<b>Contrato de dirección:</b><br>
Crecimiento: 150MS; Aceleración: aceleración predeterminada
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Ejemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene instalada la aplicación de <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> , haga clic aquí para <a href="xamlcontrolsgallery:/item/ImplicitTransition">abrir la aplicación y ver las transiciones implícitas en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Animaciones IMPLÍCITAS

> Las animaciones implícitas requieren Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior.

Las animaciones implícitas son una manera sencilla de lograr el movimiento fluido mediante la interpolación automática entre los valores antiguos y nuevos durante un cambio de parámetro.

Puede animar implícitamente los cambios en las siguientes propiedades:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacidad**
  - **Rotación**
  - **Escala**
  - **Traducción**

- [Borde](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)o [Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **Información preliminar**

Cada propiedad que puede tener cambios animados implícitamente tiene una propiedad de _transición_ correspondiente. Para animar la propiedad, asigne un tipo de transición a la propiedad de _transición_ correspondiente. En esta tabla se muestran las propiedades de _transición_ y el tipo de transición que se va a usar para cada una de ellas.

| Propiedad animada | Propiedad Transition | Tipo de transición implícita |
| -- | -- | -- |
| [UIElement. Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Borde. Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter. Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel. Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

En este ejemplo se muestra cómo usar la propiedad Opacity y la transición para hacer que un botón se atenúe cuando el control está habilitado y se desvanece cuando está deshabilitado.

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

- [Información general sobre movimiento](index.md)
- [Sincronización y aceleración](timing-and-easing.md)
- [Direccionalidad y gravedad](directionality-and-gravity.md)
