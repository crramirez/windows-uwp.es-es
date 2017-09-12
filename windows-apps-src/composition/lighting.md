---
author: jebishop
ms.assetid: fb8ae71d-5c88-4c85-9257-a9607d5179b1
title: "lighting, iluminación"
description: "Los objetos de luz se usan junto con el objeto SceneLightingEffect para simular la reflexión e iluminación dinámicas."
ms.author: jimwalk
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3f922cae8aa0787f8be6496997df1021dda8e142
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2017
---
# <a name="lighting"></a>Iluminación

Los objetos [**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) se usan junto con el objeto [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) para simular la reflexión e iluminación dinámicas.

Puedes aplicar luces a [**elementos visuales**](https://msdn.microsoft.com/library/windows/apps/Dn706858) y [**elementos de interfaz de usuario**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) de XAML.

## <a name="applying-lights-to-xaml-uielements"></a>Aplicar luces a elementos de interfaz de usuario de XAML

Los objetos [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) se usan para aplicar objetos [**CompositionLights**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar dinámicamente los elementos de interfaz de usuario de XAML. El objeto XamlLight proporciona métodos para dirigirse a elementos de interfaz de usuario u otros pinceles de XAML, aplicar luces a los árboles de elementos de interfaz de usuario y ayudar a administrar la duración de recursos CompositionLight en función de si están en uso.

* Si seleccionas un objeto **Brush** con un objeto XamlLight, la luz iluminará todas las partes de cualesquiera UIElements que usen dicho pincel.
* Si seleccionas un **UIElement** con un objeto XamlLight, en ese caso la luz iluminará todo el UIElement y sus UIElement secundarios.

## <a name="creating-and-using-a-xamllight"></a>Crear y usar un objeto XamlLight

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) es una clase base que se puede usar para crear las luces personalizadas.

Este es un ejemplo de un objeto XamlLight personalizado que usa una propiedad adjunta para dirigirse a elementos específicos:

```csharp
public sealed class FancyOrangeSpotLight : XamlLight
{
    // Register an attached proeprty that enables apps to set a UIElement or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(Boolean),
        typeof(FancyOrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );
    public static void SetIsTarget(DependencyObject target, Boolean value)
    {
        target.SetValue(IsTargetProperty, value);
    }
    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (Boolean) target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj,
                                            DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (Boolean)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        CompositionLight = Window.Current.Compositor.CreateSpotLight();
        CompositionLight.InnerConeColor = Colors.Orange;
        CompositionLight.OuterConeColor = Colors.Yellow;
        CompositionLight.InnerConeAngleInDegrees = 30;
        CompositionLight.OuterConeAngleInDegrees = 45;
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen. The CompositionLight should be disposed when no longer required.
        CompositionLight.Dispose();
        CompositionLight = null;
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return typeof(FancyPointerTrackerLight).FullName;
    }
}
```

Este es un ejemplo en el que se muestran diferentes usos posibles de la luz personalizada definida anteriormente:

```xml
<Page>
    <Page.Lights>
        <local:SimpleOrangeSpotLight />
    </Page.Lights>

    <StackPanel>
        <!-- this border will be lit by a FancyOrangeSpotLight, but not its children -->
        <Border BorderThickness="1">
            <Border.BorderBrush>
                <SolidColorBrush Color="Orange" local:FancyOrangeSpotLight.IsTarget="true" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border and its children will be lit by FancyOrangeSpotLight -->
        <Border BorderThickness="2" local:FancyOrangeSpotLight.IsTarget="true">
            <Border.BorderBrush>
                <SolidColorBrush Color="Purple" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border will not be lit -->
        <Border BorderThickness="2">
            <Border.BorderBrush>
                <SolidColorBrush Color="Green" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>
    </StackPanel>
<Page>
```

> [!Important]
> El establecimiento del objeto UIElement.Lights en el marcado, como se muestra en el ejemplo anterior, solo se admite para las aplicaciones con una versión mínima igual a Windows 10 Creators Update o una versión posterior. Para las aplicaciones que se dirigen a versiones anteriores, las luces se deben crear en el código subyacente.

## <a name="additional-resources"></a>Recursos adicionales

* Muestras avanzadas de la interfaz de usuario y la composición en [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs).