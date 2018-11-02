---
author: daneuber
title: Adaptación de composición
description: Usa las API de composición para adaptar la interfaz de usuario, optimizar el rendimiento y dar cabida a la configuración de usuario y las características del dispositivo.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2efea81f3520e6fb1a797394656587d2a29201aa
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5974348"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Adaptación de efectos y experiencias con la interfaz de usuario de Windows

La interfaz de usuario de Windows proporciona muchas medio, animaciones y efectos atractivos para la diferenciación. Sin embargo, cumplir las expectativas del usuario para el rendimiento y capacidad de personalización sigue siendo una parte necesaria de creación de aplicaciones se realiza correctamente. La plataforma Universal de Windows admite una familia de dispositivos, lo que tienen diferentes características y funcionalidades de grande y diversa. Con el fin de proporcionar una experiencia inclusiva para todos los usuarios, debes asegurarte de la escala de las aplicaciones en todos los dispositivos y respetar las preferencias del usuario. Adaptación de la interfaz de usuario puede proporcionar una forma eficaz para aprovechar las funcionalidades del dispositivo y garantizar una experiencia de usuario agradable e inclusivo.

Adaptación de la interfaz de usuario es una categoría general que abarque el trabajo de rendimiento, la interfaz de usuario atractivas con respecto a las siguientes áreas:

- Respetar y adaptarse a la configuración de usuario de efectos
- Configuración de usuario para las animaciones de adaptación
- Optimizar la interfaz de usuario para las capacidades de determinado hardware

Aquí, trataremos cómo adaptar tu efectos y animaciones con la capa Visual en las áreas anteriores, pero hay muchos otros medios para adaptar tu aplicación para garantizar una experiencia de usuario final excelente. Documentos de instrucciones están disponibles en cómo [adaptar la interfaz de usuario](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) para varios dispositivos y [crear la interfaz de usuario con capacidad de respuesta](/design/layout/responsive-design.md).

## <a name="user-effects-settings"></a>Configuración de efectos de usuario

Los usuarios pueden personalizar su experiencia de Windows para una variedad de motivos, lo que deben respetar y adaptarse a las aplicaciones. Un área que los usuarios finales pueden controlar está cambiando los tipos de efectos que ven utilizados a lo largo de su sistema.

### <a name="transparency-effects-settings"></a>Configuración de efectos de transparencia

Un efecto configuración semejante los usuarios pueden personalizar está convirtiendo los efectos de transparencia y apagado. Esto puede encontrarse en la aplicación configuración en personalización > colores, o a través de la aplicación Configuración > Accesibilidad > pantalla.

![Opción de transparencia en configuración](images/tailoring-transparency-setting.png)

Cuando activado, cualquier efecto que usa transparencia aparecerá según lo esperado. Esto se aplica a acrílico, HostBackdropBrush o cualquier gráfico de efecto personalizado que no es totalmente opaco.

Cuando se desactiva, material acrílico automáticamente recurrirá a un color sólido porque pincel de acrílico del XAML ha escuchado este evento de manera predeterminada. Aquí, ver la aplicación de calculadora correctamente recurrir a un color sólido cuando no se habilitan efectos de transparencia:

![Calculadora con acrílico](images/tailoring-acrylic.png)
![Calculadora con acrílico responder a la configuración de transparencia](images/tailoring-acrylic-fallback.png)

Sin embargo, para los efectos personalizados la aplicación debe responder a la propiedad [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) o el evento de [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) y cambiar el gráfico de efecto o efecto a usar un efecto que no tiene ninguna transparencia. Un ejemplo de esto es a continuación:

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

Del mismo modo, las aplicaciones deben escuchar y responder a la propiedad [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) para garantizar que las animaciones son activadas o desactivado en función de la configuración de usuario en la configuración > Accesibilidad > pantalla.

![Opción de animaciones en la configuración](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Al aprovechar las capacidades de API

Al aprovechar la [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) API, puede detectar qué composición características están disponibles y eficaz en dado que el hardware y adaptar el diseño para garantizar que los usuarios finales obtener una experiencia atractivas y eficaz en cualquier dispositivo. Las API proporcionan un medio para comprobar las funcionalidades del sistema de hardware para implementar suave efecto ajuste de escala en una variedad de factores de forma. Esto facilita la forma apropiada adaptar la aplicación para crear un hermoso y la experiencia del usuario final sin interrupciones.

Esta API proporciona métodos y un agente de escucha de eventos que se puede usar para hacer que el efecto de ajuste de escala de las decisiones de la aplicación de la interfaz de usuario. La característica detecta cómo de bien el sistema puede controlar composición complejo y las operaciones de representación y, a continuación, devuelve la información en un modelo de-de consumir a utilizar los desarrolladores.

### <a name="using-composition-capabilities"></a>Uso de las funcionalidades de composición

La funcionalidad de CompositionCapabilities ya que se aprovechan de características como material acrílico, donde el material retrocede un efecto eficaz más según el escenario y el hardware.

La API se puede agregar en el código existente con unos sencillos pasos.

1. Adquirir el objeto de funcionalidades en el constructor de la aplicación.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registra un agente de escucha de eventos cambiados de funcionalidades de la aplicación.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Agregar contenido al método de devolución de llamada de eventos para controlar los distintos niveles de funcionalidades. Esto puede o no puede ser similar al siguiente paso siguiente.
1. Al usar efectos, primero comprueba el objeto de funciones. Considera la posibilidad de usar comprobaciones condicionales o cambiar las instrucciones de control, dependiendo de cómo quieres adaptar los efectos.

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

Código de ejemplo completo puede encontrarse en el [repositorio de Github de la interfaz de usuario de Windows](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Rápido frente a los efectos lentos

En función de los comentarios de los métodos de [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) y [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) proporcionados en la API de CompositionCapabilties, puede decidir que la aplicación no o caros efectos de intercambio para otros efectos de su elección que estén optimizadas para el dispositivo. Algunos efectos se saben que requiere más recursos que otros de forma coherente y deben usarse con moderación, y se pueden usar otros efectos más libremente. Para todos los efectos, sin embargo, la atención debe usarse cuando encadenamiento y animación como algunos escenarios o combinaciones pueden cambiar las características de rendimiento del gráfico de efecto. A continuación mostramos algunas características de rendimiento de la regla general de los efectos individuales:

- Los efectos que se saben que tienen impacto de alto rendimiento son los siguientes: Desenfoque gausiano, máscara de sombra, BackDropBrush, HostBackDropBrush y capa Visual. Estos no se recomiendan para dispositivos de gama baja [(nivel de característica 9.1 9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)y deben usarse con precaución en dispositivos de gama alta.
- Efectos con impacto en el rendimiento medio incluyen la matriz de Color, determinados BlendModes de efecto de fusión (luminosidad, Color, saturación y tono), contenido destacado, SceneLightingEffect y (según el escenario) BorderEffect. Estos efectos pueden funcionar con determinados escenarios en los dispositivos de gama baja, pero debe usarse cuidado cuando encadenamiento y animar. Recomendamos limitar el uso a dos o menos y animación de transiciones solo.
- Todos los demás efectos tienen impacto de bajo rendimiento y funcionan en todos los escenarios razonables cuando la animación y encadenamiento.

## <a name="related-articles"></a>Artículos relacionados

- [Técnicas de diseño con capacidad de respuesta de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Adaptación de dispositivo UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
