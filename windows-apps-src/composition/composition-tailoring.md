---
title: Adaptación de composición
description: Usar las API de composición para adaptar la interfaz de usuario, optimizar el rendimiento y dar cabida a la configuración de usuario y las características del dispositivo.
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b975c8fc8cf0770dd73d8749733ae5636f2ee296
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9058716"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personalización de efectos & experiencias con la interfaz de usuario de Windows

La interfaz de usuario de Windows proporciona muchas medio, animaciones y efectos atractivos para la diferenciación. Sin embargo, cumplir las expectativas del usuario para el rendimiento y capacidad de personalización sigue siendo una parte necesaria de creación de aplicaciones se realiza correctamente. La plataforma Universal de Windows admite una familia de dispositivos, que tienen diferentes características y funcionalidades de grande y diversa. Con el fin de proporcionar una experiencia inclusiva para todos los usuarios, debes asegurarte de la escala de las aplicaciones en todos los dispositivos y respetar las preferencias del usuario. Adaptación de la interfaz de usuario puede proporcionar una forma eficaz para aprovechar las funcionalidades de un dispositivo y garantizar una experiencia de usuario agradable e inclusivo.

Adaptación de la interfaz de usuario es una categoría general que abarque el trabajo de rendimiento, la interfaz de usuario atractivas con respecto a las siguientes áreas:

- Respetar y adaptarse a la configuración de usuario de efectos
- Adaptación de la configuración de usuario para las animaciones
- Optimizar la interfaz de usuario para las capacidades de determinado hardware

Aquí, trataremos cómo adaptar sus efectos y animaciones con la capa Visual en las áreas anteriores, pero hay muchos otros medios para adaptar tu aplicación para garantizar una experiencia de usuario final excelente. Documentos de orientaciones están disponibles en cómo [adaptar la interfaz de usuario](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) para varios dispositivos y [crear la interfaz de usuario dinámica](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Configuración de efectos de usuario

Los usuarios pueden personalizar su experiencia de Windows para una variedad de motivos, que deben respetar y adaptarse a las aplicaciones. Controlar los usuarios finales de un área está cambiando los tipos de efectos que ven utilizados a lo largo de su sistema.

### <a name="transparency-effects-settings"></a>Configuración de efectos de transparencia

Un efecto configuración semejante los usuarios pueden personalizar está convirtiendo los efectos de transparencia encendido/apagado. Esto puede encontrarse en la aplicación configuración de personalización > colores, o a través de > accesibilidad > pantalla de configuración de la aplicación.

![Opción de transparencia en configuración](images/tailoring-transparency-setting.png)

Cuando se activa, cualquier efecto que usa la transparencia aparecerá según lo esperado. Esto se aplica a acrílico, HostBackdropBrush o cualquier gráfico de efecto personalizado que no es totalmente opaco.

Cuando se desactiva, material acrílico automáticamente recurrirá a un color sólido porque se ha escuchado pincel de acrílico de XAML para este evento de manera predeterminada. Aquí, ver la aplicación de calculadora correctamente recurrir a un color sólido cuando no están habilitados los efectos de transparencia:

![Calculadora con acrílico](images/tailoring-acrylic.png)
![Calculadora con acrílico responder a la configuración de transparencia](images/tailoring-acrylic-fallback.png)

Sin embargo, para los efectos personalizados la aplicación debe responder a la propiedad [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) o el evento de [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) y cambiar el gráfico de efecto o el efecto a usar un efecto que no tiene transparencia. Un ejemplo de esto es a continuación:

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

Del mismo modo, las aplicaciones deben escuchar y responder a la propiedad [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) para garantizar que las animaciones se activen o desactivado en función de la configuración de usuario de configuración > accesibilidad > pantalla.

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

Mediante el aprovechamiento de la API [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) , puedes detectar qué composición características están disponibles y eficaz en cierta cantidad de hardware y adaptar el diseño para garantizar que los usuarios finales obtener una experiencia atractivas y eficaz en cualquier dispositivo. Las API proporcionan un medio para comprobar las funcionalidades del sistema de hardware con el fin de implementar suave efecto ajuste de escala en una variedad de factores de forma. Esto facilita la correctamente adaptar la aplicación para crear un hermoso y la experiencia del usuario final sin interrupciones.

Esta API proporciona métodos y un agente de escucha de eventos que se puede usar para hacer que el efecto ajuste de escala de las decisiones de la interfaz de usuario de la aplicación. La característica detecta lo bien que funcionan en que el sistema puede controlar composición complejos y las operaciones de representación y, a continuación, devuelve la información en un modelo de-de consumir a utilizar los desarrolladores.

### <a name="using-composition-capabilities"></a>Uso de las funcionalidades de composición

La funcionalidad de CompositionCapabilities ya que se aprovechan de características como material acrílico, donde el material retrocede un efecto de rendimiento más según el escenario y el hardware.

La API se puede agregar en el código existente con unos sencillos pasos.

1. Adquirir el objeto de funciones en el constructor de la aplicación.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registra un agente de escucha de eventos cambiados de funcionalidades de la aplicación.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Agregar contenido al método de devolución de llamada de eventos para controlar los distintos niveles de funcionalidades. Esto puede o no puede ser similar al siguiente paso siguiente.
1. Al usar efectos, comprueba primero el objeto de funciones. Considera la posibilidad de usar comprobaciones condicionales o cambiar las instrucciones de control, dependiendo de cómo quieres adaptar los efectos.

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

## <a name="fast-vs-slow-effects"></a>Modo rápido frente a los efectos lento

En función de los comentarios de los métodos [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) y [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) proporcionados en la API de CompositionCapabilities, la aplicación puede decidir intercambiar efectos costosos o no es compatibles con otros efectos de su elección que estén optimizados para el dispositivo. Algunos efectos se saben que requiere más recursos que otros de forma coherente y deben usarse con moderación y se pueden usar otros efectos más libremente. Para todos los efectos, sin embargo, la atención debe usarse cuando encadenamiento y animación como algunos escenarios o combinaciones pueden cambiar las características de rendimiento del gráfico de efecto. A continuación mostramos algunas características de rendimiento de la regla general de los efectos individuales:

- Los efectos que se saben que tienen impacto de alto rendimiento son los siguientes: Desenfoque gausiano, máscara de sombras, BackDropBrush, HostBackDropBrush y capa Visual. Estos no se recomiendan para dispositivos de gama baja [(nivel de característica 9.1 9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)y deben usarse con precaución en dispositivos de gama alta.
- Efectos con impacto en el rendimiento medio incluyen la matriz de colores, determinados BlendModes de efecto de fusión (luminosidad, Color, saturación y tono), contenido destacado, SceneLightingEffect y (según el escenario) BorderEffect. Estos efectos pueden funcionar con determinados escenarios en los dispositivos de gama baja, pero debe usarse cuidado cuando encadenamiento y animar. Recomendamos limitar el uso a dos o menos y animación de transiciones solo.
- Todos los demás efectos tienen impacto de bajo rendimiento y funcionan en todos los escenarios razonables cuando la animación y encadenamiento.

## <a name="related-articles"></a>Artículos relacionados

- [Técnicas de diseño con capacidad de respuesta de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Adaptación de dispositivo UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
