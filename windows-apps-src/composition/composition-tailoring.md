---
title: Adaptación de composición
description: Usar las API de composición para adaptar la interfaz de usuario y optimizar el rendimiento para dar cabida a la configuración de usuario y las características del dispositivo.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9692a8ef21e9f62114b38c6bb5d15199b8c0e04a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318151"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Adaptación de efectos y experiencias con la interfaz de usuario de Windows

Interfaz de usuario de Windows proporciona muchos efectos atractivos, animaciones y medios de diferenciación. Sin embargo, todavía es una parte necesaria de la creación de aplicaciones de éxito cumple las expectativas del usuario para el rendimiento y capacidad de personalización. La plataforma Universal de Windows es compatible con una familia de grande y diversa de dispositivos que tienen funcionalidades y características diferentes. Con el fin de proporcionar una experiencia inclusiva para todos los usuarios, deberá asegurarse de que escalen sus aplicaciones en dispositivos y respetar las preferencias del usuario. Personalización de la interfaz de usuario puede proporcionar una manera eficaz de aprovechar las capacidades de un dispositivo y garantizar una experiencia de usuario agradable y inclusive.

Personalización de la interfaz de usuario es una categoría amplio que abarca el trabajo de alto rendimiento, la interfaz de usuario atractiva con respecto a las áreas siguientes:

- Respetar y adaptar a las configuraciones de usuario para los efectos
- Ajustando la configuración de usuario para las animaciones
- Optimización de la interfaz de usuario para las capacidades de hardware determinado

Aquí, trataremos cómo adaptar sus efectos y animaciones con la capa Visual en las áreas anteriores, pero hay muchos otros métodos para adaptar la aplicación para garantizar una experiencia de usuario final excepcional. Documentos de instrucciones están disponibles sobre cómo [adaptar la interfaz de usuario](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) para varios dispositivos y [crear interfaz de usuario con capacidad de respuesta](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Configuración de los efectos de usuario

Los usuarios pueden personalizar su experiencia de Windows para una variedad de motivos, que las aplicaciones deben respetar y adaptarse a. Puede controlar los usuarios finales de un área está cambiando los tipos de efectos verán usados a lo largo de su sistema.

### <a name="transparency-effects-settings"></a>Configuración de los efectos de transparencia

Este tipo una configuración de efecto, los usuarios pueden personalizar está volviendo a efectos de transparencia activado/desactivado. Se puede encontrar en la aplicación de configuración en personalización > colores, o a través de la aplicación de configuración > Accesibilidad > para mostrar.

![Opción de transparencia en la configuración](images/tailoring-transparency-setting.png)

Cuando se activa, cualquier efecto que usa la transparencia aparecerá como se esperaba. Esto se aplica a acrílico, HostBackdropBrush o cualquier gráfico de efecto personalizado que no es completamente opaco.

Si está desactivada, material acrílico automáticamente recurrirá a un color sólido porque ha escuchado pincel acrílico del XAML para este evento de forma predeterminada. En este caso, vemos que la aplicación Calculadora adecuadamente revirtiendo a un color sólido cuando no están habilitados los efectos de transparencia:

![Calculadora de acrílico](images/tailoring-acrylic.png)
![Calculadora con acrílico responde a la configuración de transparencia](images/tailoring-acrylic-fallback.png)

Sin embargo, se necesita para la aplicación de efectos de cualquier personalizado responder a la [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) propiedad o [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) eventos y switch out el efecto de efecto gráfico de utilizar un efecto que no tiene transparencia. Un ejemplo de esto está por debajo:

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

## <a name="animations-settings"></a>Configuración de las animaciones

De forma similar, las aplicaciones deben escuchar y responder a la [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) propiedad para garantizar que las animaciones son activar o desactivar según la configuración de usuario en la configuración > Accesibilidad > para mostrar.

![Opción de animaciones en configuración](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Al aprovechar las capacidades de API

Aprovechando el [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) API, puede detectar la composición de qué características están disponibles y el rendimiento en dado el hardware y adaptar el diseño para asegurarse de que los usuarios finales obtienen un alto rendimiento y experiencia atractiva en cualquier dispositivo. Las API proporcionan un medio para comprobar las capacidades del hardware del sistema con el fin de implementar efecto estable el escalado en una variedad de factores de forma. Esto facilita la adecuadamente adaptar la aplicación para crear un atractivo y la experiencia del usuario final perfecta.

Esta API proporciona métodos y un agente de escucha de eventos que puede utilizarse para realizar el efecto de las decisiones de la interfaz de usuario de aplicación de escalado. La característica detecta también cómo el sistema puede controlar la composición compleja y las operaciones de representación y, a continuación, devuelve la información en un modelo fácil de usar para los desarrolladores a utilizar.

### <a name="using-composition-capabilities"></a>Uso de las capacidades de composición

La funcionalidad de CompositionCapabilities ya se aprovechan características como material acrílico, donde el material se recurre a un efecto de rendimiento más según el escenario y el hardware.

La API se puede agregar al código existente en unos pocos pasos sencillos.

1. Adquirir el objeto de funcionalidad en el constructor de la aplicación.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registre un agente de escucha del evento de cambio de capacidades para la aplicación.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Agregar contenido al método de devolución de llamada de evento para controlar distintos niveles de funcionalidades. Esto puede o no puede ser similar al siguiente paso siguiente.
1. Al usar efectos, compruebe primero el objeto de funcionalidad. Considere el uso de comprobaciones condicionales o cambie las instrucciones de control, dependiendo de cómo desea adaptar los efectos.

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

Código de ejemplo completo se puede encontrar en el [repositorio de Github de la interfaz de usuario de Windows](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Rápida frente a efectos de baja velocidad

Según los comentarios de proporcionado [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) y [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) métodos de la API CompositionCapabilities, la aplicación puede decidir intercambiar los efectos caros o no se admite para otros efectos de su elección que están optimizados para el dispositivo. Algunos efectos se sabe que más consumen muchos recursos que otras constantemente y deben usarse con moderación y otros efectos que pueden usarse más libremente. Para todos los efectos, sin embargo, la atención debe usarse cuando encadenamiento y animar como algunos escenarios o combinaciones pueden cambiar las características de rendimiento del gráfico del efecto. A continuación se muestran algunas características de rendimiento por regla general de los efectos individuales:

- Efectos que se sabe que tienen impacto alto rendimiento son los siguientes: Desenfoque gausiano, máscara de sombras, BackDropBrush, HostBackDropBrush y capa Visual. No se recomienda para los dispositivos de gama baja [(9.1-9.3 de nivel de características)](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)y debe utilizarse con precaución en dispositivos de gama alta.
- Efectos con impacto en el rendimiento medio incluyen la matriz de colores, determinados BlendModes de efecto de Blend (luminosidad, Color, saturación y tono), (según el escenario), SpotLight y SceneLightingEffect BorderEffect. Estos efectos pueden funcionar con determinados escenarios en los dispositivos de gama baja, pero cuidado se debe usar al encadenamiento y animación. Se recomienda restringir el uso a dos o menos y animar en sólo las transiciones.
- Todos los demás efectos tienen un rendimiento bajo impacto y funcionan en todos los escenarios razonables cuando la animación y el encadenamiento.

## <a name="related-articles"></a>Artículos relacionados

- [Técnicas de diseño dinámico de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Adaptación de dispositivo UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
