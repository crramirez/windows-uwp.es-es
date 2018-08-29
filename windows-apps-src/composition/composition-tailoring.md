---
author: daneuber
title: Adaptación de la composición
description: Utilice las API de composición para adaptar la interfaz de usuario, optimizar el rendimiento y adaptarse a la configuración de usuario y las características del dispositivo.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 66384c4df3195ae0fff35ae5dd7e1b1983204068
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2919110"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personalización de efectos y experiencias con la interfaz de usuario de Windows

Interfaz de usuario de Windows proporciona muchos efectos hermosos, animaciones y medios de diferenciación. Sin embargo, cumplir las expectativas de rendimiento y capacidad de personalización del usuario sigue siendo una parte necesaria de la creación de aplicaciones de éxito. La plataforma de Windows Universal es compatible con una familia de dispositivos, que tienen diferentes características y capacidades de grande y diversa. Con el fin de proporcionar una experiencia de inclusión para todos los usuarios, debe asegurarse de su escala de aplicaciones entre dispositivos y respetar las preferencias del usuario. Personalización de la interfaz de usuario puede proporcionar una manera eficaz para aprovechar las capacidades de un dispositivo y garantizar una experiencia de usuario agradable e inclusive.

Personalización de la interfaz de usuario es una gran categoría que engloba el trabajo de rendimiento, hermosa interfaz de usuario con respecto a las siguientes áreas:

- Respetar y adaptarse a la configuración del usuario para efectos
- Configuración de usuario con capacidad para animaciones
- Optimizar la interfaz de usuario para las capacidades de hardware determinada

En este caso, analizaremos cómo adaptar sus efectos y animaciones con la capa Visual en las áreas mencionadas anteriormente, pero hay muchos otros medios para adaptar la aplicación para asegurar una experiencia óptima al usuario final. Documentos de guía están disponibles en cómo [adaptar la interfaz de usuario](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) para varios dispositivos y [crear la interfaz de usuario con capacidad de respuesta](/design/layout/responsive-design.md).

## <a name="user-effects-settings"></a>Configuración de efectos de usuario

Los usuarios pueden personalizar su experiencia de Windows para una variedad de motivos, que deben respetar y adaptarse a las aplicaciones. Un área que puede controlar a los usuarios finales está cambiando los tipos de efectos que ven utilizados en su sistema.

### <a name="transparency-effects-settings"></a>Configuración de efectos de transparencia

Un tal configuración de efecto, los usuarios pueden personalizar se activación o desactiva los efectos de transparencia. Esto puede encontrarse en la configuración de la aplicación en personalización > colores, o a través de la configuración de la aplicación > Accesibilidad > Mostrar.

![Opción de transparencia en la configuración](images/tailoring-transparency-setting.png)

Cuando se activa, cualquier efecto que utiliza transparencia aparecerá como se esperaba. Esto se aplica a acrílico, HostBackdropBrush o cualquier gráfico de efecto personalizado que no es totalmente opaco.

Cuando se desactiva, material acrílico retrocederá automáticamente a un color sólido como pincel acrílico del XAML ha tenido en cuenta este evento por defecto. Aquí, vemos la aplicación Calculadora adecuadamente revirtiendo a un color sólido cuando no se habilitan los efectos de transparencia:

![Calculadora con acrílico](images/tailoring-acrylic.png)
![Calculadora con acrílico responde a la configuración de transparencia](images/tailoring-acrylic-fallback.png)

Sin embargo, para los efectos personalizados la aplicación necesita responder a la propiedad [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) o el evento de [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) y cambiar el gráfico de efecto, efecto al utilizar un efecto que no tiene transparencia. Un ejemplo de esto es a continuación:

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>Configuración de animaciones

Del mismo modo, deben escuchar y responder a la propiedad [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) para asegurarse animaciones están encendidas o apagado basadas en configuración de usuario en configuración de aplicaciones > Accesibilidad > Mostrar.

![Opción de animaciones en la configuración](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Aprovechamiento de las funciones de API

Al aprovechar la API [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) , puede detectar qué composición características están disponibles y el rendimiento en dada hardware y adaptar el diseño para asegurarse de que los usuarios finales obtener una experiencia hermosa y alto rendimiento en cualquier dispositivo. La API proporcionan un medio para comprobar las capacidades de sistema de hardware para implementar el efecto ordenado escala en una variedad de factores de forma. Esto facilita adaptar adecuadamente la aplicación para crear una hermosa y una experiencia transparente al usuario final.

Esta API proporciona métodos y un detector de eventos puede utilizarse para hacer efecto de escala de decisiones para la interfaz de usuario de la aplicación. La función detecta qué tan bien el sistema puede manejar la composición compleja y las operaciones de representación y, a continuación, devuelve la información en un modelo fácil de consumir para que los desarrolladores utilizan.

### <a name="using-composition-capabilities"></a>Utilizando las capacidades de composición

Características como material de acrílico, donde el material se recurre a un efecto de rendimiento más dependiendo de la situación y el hardware ya que se aprovecha la funcionalidad de CompositionCapabilities.

La API puede agregarse al código existente en unos cuantos pasos sencillos.

1. Adquirir el objeto de funciones en el constructor de la aplicación.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registre un agente de escucha de evento de cambio de capacidades para la aplicación.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Agregar contenido al método de devolución de llamada de eventos para controlar diversos niveles de capacidades. Esto puede o no puede ser similar a la siguiente paso siguiente.
1. Cuando se utilizan efectos, compruebe primero el objeto de funciones. Considere el uso de comprobaciones condicionales o cambie las instrucciones de control, dependiendo de cómo desea adaptar los efectos.

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

Código de ejemplo completo se puede encontrar en el [repositorio de Github de interfaz de usuario de Windows](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Modo rápido frente a efectos lento

Basado en los comentarios de los métodos [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) y [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) proporcionados en la API de CompositionCapabilties, la aplicación puede decidir intercambiar los efectos costosos o no compatibles con otros efectos de su elección que se optimizan para el dispositivo. Algunos efectos son conocidos por ser coherente requiere más recursos que otros y deben utilizarse con moderación y otros efectos que se pueden utilizar más libremente. Para todos los efectos, sin embargo, debe tenerse cuidado al encadenamiento y como algunos escenarios o combinaciones de animación pueden cambiar las características de rendimiento del gráfico de efecto. A continuación se muestran algunas características de rendimiento de la regla de los efectos individuales:

- Efectos que se saben que tienen impacto de alto rendimiento son los siguientes: Desenfoque gausiano, máscara de sombras, BackDropBrush, HostBackDropBrush y capa Visual. Éstos no se recomiendan para low-end de dispositivos [(nivel de función 9.1 9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)y deben utilizarse con cautela en dispositivos de high-end.
- Efectos con el impacto en el rendimiento medio incluyen la matriz de colores, determinados BlendModes de efecto mezcla (luminosidad, Color, saturación y tono), SpotLight, SceneLightingEffect y (según el escenario) BorderEffect. Estos efectos pueden trabajar con determinados escenarios en dispositivos de gama baja, pero debe tenerse cuidado al encadenamiento y animación. Se recomienda restringir el uso a dos o menos y animación de transiciones sólo.
- Todos los demás efectos tienen impacto de bajo rendimiento y trabajan en todas las situaciones razonables al animar y encadenamiento.

## <a name="related-articles"></a>Artículos relacionados

- [Técnicas de diseño de capacidad de respuesta de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Adaptación de dispositivos UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
