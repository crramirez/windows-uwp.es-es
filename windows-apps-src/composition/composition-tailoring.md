---
title: Adaptación de composición
description: Use las API de composición para adaptar la interfaz de usuario, optimizar el rendimiento y adaptarse a la configuración del usuario y a las características del dispositivo.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 95a7355241f9ba4cc7b4bb743b78ac09169d65d9
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213682"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personalización de efectos & experiencias con la interfaz de usuario de Windows

La interfaz de usuario de Windows proporciona muchos efectos atractivos, animaciones y medias de diferenciación. Sin embargo, el cumplimiento de las expectativas de los usuarios para el rendimiento y la personalización sigue siendo una parte necesaria de la creación de aplicaciones correctas. El Plataforma universal de Windows admite una familia grande y diversa de dispositivos, que tienen diferentes características y capacidades. Con el fin de proporcionar una experiencia inclusiva a todos los usuarios, debe asegurarse de que las aplicaciones se escalan en todos los dispositivos y respetan las preferencias del usuario. La personalización de la interfaz de usuario puede proporcionar una manera eficaz de aprovechar las capacidades de un dispositivo y garantizar una experiencia de usuario agradable y inclusiva.

La personalización de la interfaz de usuario es una amplia categoría que abarca el trabajo de una interfaz de usuario eficaz y atractiva con respecto a las siguientes áreas:

- Respetar y adaptar a la configuración de usuario para efectos
- Adaptación de la configuración de usuario para animaciones
- Optimizar la interfaz de usuario para las capacidades de hardware dadas

Aquí veremos cómo adaptar sus efectos y animaciones con la capa visual en las áreas anteriores, pero hay muchos otros medios para adaptar la aplicación a fin de garantizar una excelente experiencia del usuario final. Los documentos de orientación están disponibles en cómo [adaptar la interfaz de usuario](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) para varios dispositivos y [crear una interfaz de usuario con capacidad de respuesta](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Configuración de efectos de usuario

Los usuarios pueden personalizar su experiencia con Windows por diversos motivos, las aplicaciones que deben respetar y adaptarse. Un área que los usuarios finales pueden controlar es cambiar los tipos de efectos que se usan en su sistema.

### <a name="transparency-effects-settings"></a>Configuración de efectos de transparencia

Uno de estos efectos que los usuarios pueden personalizar es activar o desactivar los efectos de transparencia. Esto puede encontrarse en la aplicación de configuración, en Personalización > colores, o a través de la aplicación de configuración > la accesibilidad > presentación.

![Opción de transparencia en configuración](images/tailoring-transparency-setting.png)

Cuando está activado, los efectos que utilicen la transparencia aparecerán de la manera esperada. Esto se aplica a acrílico, HostBackdropBrush o cualquier gráfico de efecto personalizado que no sea totalmente opaco.

Cuando está desactivada, el material acrílico se revierte automáticamente a un color sólido, ya que el pincel acrílico de XAML ha escuchado este evento de forma predeterminada. Aquí vemos que la aplicación de calculadora se revierte correctamente a un color sólido cuando no están habilitados los efectos de transparencia:

Calculadora de ![con acrílico](images/tailoring-acrylic.png)
![calculadora con la respuesta acrílico a la configuración de transparencia](images/tailoring-acrylic-fallback.png)

Sin embargo, para cualquier efecto personalizado, la aplicación debe responder a la propiedad [UISettings. AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) o al evento [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) y desactivar el gráfico efecto/efecto para usar un efecto que no tenga transparencia. A continuación se muestra un ejemplo de esto:

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

Del mismo modo, las aplicaciones deben escuchar y responder a la propiedad [UISettings. AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) para asegurarse de que las animaciones se activan o desactivan en función de la configuración de usuario en la configuración > facilitar el acceso > presentación.

![Opción de animaciones en configuración](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Aprovechamiento de la API de funcionalidades

Al aprovechar las API de [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) , puede detectar qué características de composición están disponibles y cómo se aplican a un hardware determinado y adaptar el diseño para garantizar que los usuarios finales obtengan una experiencia eficaz y atractiva en cualquier dispositivo. Las API proporcionan un medio para comprobar las capacidades del sistema de hardware con el fin de implementar el escalado de efectos correctos en una variedad de factores de forma. Esto facilita la adaptación adecuada de la aplicación para crear una experiencia de usuario final atractiva y sin problemas.

Esta API proporciona métodos y un agente de escucha de eventos que se puede usar para tomar decisiones de escalado de efectos para la interfaz de usuario de la aplicación. La característica detecta el grado en que el sistema puede controlar las operaciones complejas de composición y representación y, a continuación, devuelve la información en un modelo fácil de usar que los desarrolladores usan.

### <a name="using-composition-capabilities"></a>Uso de las funcionalidades de composición

La funcionalidad CompositionCapabilities ya se está aprovechando para características como el material acrílico, donde el material vuelve a un efecto más efectivo en función del escenario y del hardware.

La API se puede Agregar al código existente en unos pocos pasos sencillos.

1. Adquiera el objeto Capabilities en el constructor de la aplicación.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registra un agente de escucha de eventos de cambio de funcionalidad de la aplicación.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Agregue contenido al método de devolución de llamada de evento para controlar diversos niveles de funcionalidad. Esto puede ser o no similar al siguiente paso siguiente.
1. Al usar efectos, compruebe primero el objeto de funcionalidades. Considere la posibilidad de utilizar comprobaciones condicionales o instrucciones de control de conmutador, en función de cómo desee personalizar los efectos.

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

Puede encontrar código de ejemplo completo en el [repositorio de github](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)de la interfaz de usuario de Windows.

## <a name="fast-vs-slow-effects"></a>Efectos rápidos frente a efectos lentos

En función de los comentarios de los métodos [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) y [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) proporcionados en la API de CompositionCapabilities, la aplicación puede optar por intercambiar efectos costosos o no admitidos para otros efectos de su elección que estén optimizados para el dispositivo. Se sabe que algunos efectos tienen un uso más intensivo de los recursos que otros y deben usarse con moderación, y otros efectos se pueden usar con mayor libertad. Sin embargo, para todos los efectos, se debe tener cuidado al encadenar y animar, ya que algunos escenarios o combinaciones pueden cambiar las características de rendimiento del gráfico de efectos. A continuación se muestra una regla de características de rendimiento Thumb para efectos individuales:

- Los efectos que se sabe que tienen un alto impacto en el rendimiento son los siguientes: Desenfoque gausiano, máscara de sombra, BackDropBrush, HostBackDropBrush y visual de capa. No se recomiendan para dispositivos de Low [-End (nivel de características 9.1-9.3)](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)y deben usarse con prudencia en dispositivos de gama alta.
- Entre los efectos con un impacto de rendimiento medio se incluyen la matriz de colores, ciertos efectos de mezcla BlendModes (luminosidad, color, saturación y matiz), SpotLight, SceneLightingEffect y (según el escenario) BorderEffect. Estos efectos pueden funcionar con ciertos escenarios en dispositivos de low-end, pero debe tener cuidado al encadenar y animar. Se recomienda restringir el uso a dos o menos y animar solo en las transiciones.
- Todos los demás efectos tienen un bajo impacto en el rendimiento y funcionan en todos los escenarios razonables al animar y encadenar.

## <a name="related-articles"></a>Artículos relacionados

- [Técnicas de diseño con capacidad de respuesta de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Personalización de dispositivos UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
