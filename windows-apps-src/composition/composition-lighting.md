---
author: daneuber
title: Iluminación de composición
description: Las API de iluminación de composición puede usarse para agregar iluminación 3D dinámicas a la aplicación.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e634b18fffc4f601f6512d6ceeed51efbe9c1886
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2911774"
---
# <a name="using-lights-in-windows-ui"></a>Uso de luces en la interfaz de usuario de Windows

Las APIs Windows.UI.Composition te permiten crear efectos y animaciones en tiempo real. Iluminación de composición permite iluminación 3D en aplicaciones 2D. En esta introducción, se ejecutará a través de la funcionalidad de cómo configurar las luces de composición, identificar los elementos visuales para la recepción de cada luz y usar efectos para definir los materiales para el contenido.

> [!NOTE]
> Para leer cómo aplican los objetos de [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar objetos UIElements de XAML, consulta [la iluminación de XAML](xaml-lighting.md).

Iluminación de composición te permite crear interesantes de la interfaz de usuario al permitir que:

- Transformación de una luz, con independencia de otros objetos de la escena para permitir escenarios de envolventes como escenas de reproducción de música.
- La capacidad para emparejar un objeto con una luz para que se muevan juntos independiente del resto de la escena para habilitar escenarios como Fluent [Mostrar](/design/style/reveal.md) resaltado.
- Transformación de la luz y la escena completa como un grupo para crear materiales y profundidad.

Iluminación de composición admite tres conceptos clave: **luz**, **destinos**y **SceneLightingEffect**.

## <a name="light"></a>Luz

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) te permite crear luces diversas y colocarlos en el espacio de coordenadas. Estas luces dirigirse a elementos visuales que quieres identificar como iluminado por la luz.

### <a name="light-types"></a>Tipos de luz

| Tipo | Descripción |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Una fuente de luz que emite luz no direccional que aparece refleja todo el contenido de la escena. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Una gran infinitamente distante fuente de luz que emite luz en una sola dirección. Como el sol. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Una fuente de punto de luz que emite luz en todas las direcciones. Al igual que una bombilla. |
| [Foco de luz](/uwp/api/windows.ui.composition.spotlight) | Una fuente de luz que emite conos internos y externos de luz. Al igual que una linterna. |

## <a name="targets"></a>Destinos

Cuando las luces como destino un elemento Visual (agregar a la lista de [destinos](/uwp/api/windows.ui.composition.compositionlight.targets) ), el objeto Visual y todos sus descendientes conocemos y responder a esta fuente de luz. Esto puede ser algo tan simple como una configuración de un origen de PointLight en la raíz de un árbol y todos los elementos visuales siguientes reaccionar a la animación de la dirección de luces de punto.

**ExclusionsFromTargets** te ofrece la capacidad para eliminar la iluminación de un elemento visual o de un subárbol de elementos visuales de manera similar, como agregar destinos. Elementos secundarios en el árbol de raíz de forma del elemento visual que se excluye no están iluminados en consecuencia.

### <a name="sample-targets"></a>Muestra (destinos)

En el siguiente ejemplo, usamos un CompositionPointLight seleccionar como destino un TextBlock de XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Mediante la adición de animación para el desplazamiento de la luz puntual, se consigue fácilmente un efecto shimmering.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Consulta el ejemplo completo [Relucir de texto](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) en la cocina de muestra WindowUIDevLabs para obtener más información.

## <a name="restrictions"></a>Restricciones

Hay varios factores a tener en cuenta al determinar qué contenido estarán iluminado CompositionLight.

Concepto | Detalles
--- | ---
**Luz de ambiente** | Agregar una luz que no sean ambiental a la escena apagará toda la luz existente.  Elementos que no es el objetivo una luz que no sean ambiente aparecerá en negros.  Para iluminar elementos visuales que le rodea no dirigidos por la luz en una forma natural, usa la luz de ambiente junto con otras luces.
**Número de luces** | Puede utilizar los dos luces de composición que no sean ambiente en cualquier combinación de la interfaz de usuario de destino. Las luces de ambiente no están restringidas; manchas, punto y las luces distantes son.
**Ciclo de vida** | CompositionLight puede experimentar condiciones de ciclo de vida (ejemplo: el recolector de elementos no utilizados puede reciclar el objeto de luz antes de que se usa).  Se recomienda mantener una referencia a las luces agregando las luces como un miembro para ayudar a las aplicaciones administrar del ciclo de vida.
**Transformaciones** | Las luces deben colocarse en un nodo por encima de la interfaz de usuario que usa efectos, como [transformaciones de perspectiva](/design/layout/3-d-perspective-effects.md) en la estructura visual que deben dibujarse correctamente.
**Espacio de coordenadas y destinos** | CoordinateSpace es al espacio visual que todas las propiedades de las luces deben establecerse en. CompositionLight.Targets debe estar dentro del árbol CoordinateSpace.

## <a name="lighting-properties"></a>Propiedades de iluminación

Según el tipo de luz usan, una luz puede tener propiedades de atenuación y el espacio. No todos los tipos de luces usan todas las propiedades.

Propiedad | Descripción
--- | ---
**Color** | El [Color](/uwp/api/windows.ui.color) de la luz. Iluminación color valores están definidos por [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) difuso, ambiente y especular que define el color que se emite. Iluminación usa valores RGBA para las luces; no se usa el componente de color alfa.
**Direction** | La dirección de la luz. La dirección en la que apunta la luz se especifica en relación con su Visual [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) .
**Espacio de coordenadas** | Cada elemento Visual tiene un espacio de coordenadas 3D implícito. Dirección del eje X es de izquierda a derecha. Dirección del eje Y es de arriba a abajo. Dirección Z es un punto fuera el plano. El punto original de esta coordenada es la esquina superior izquierda del elemento visual y la unidad es píxeles independientes de dispositivo (DIP). Desplazamiento de la luz definido en esta coordenada.
**Conos internos y externos** | Los focos de luz emiten un cono de luz que tiene dos partes: un cono interno brillante y un cono externo. Composición permite que controlar a través de color y los ángulos del cono interno y externo.
**Offset** | Desplazamiento de la fuente de luz con respecto a su espacio de coordenadas Visual.

> [!NOTE]
> Al luces varias coincidencias el mismo elemento Visual, o cada vez que se obtiene el valor de color de la luz lo suficientemente grande como para superar 1.0, puede cambiar el color de la luz a causa de compresión de un canal de color de las luces.

### <a name="advanced-lighting-properties"></a>Avanzado de las propiedades de iluminación

Propiedad | Descripción
--- | ---
**Intensidad** | Controla el brillo de la luz.
**Atenuación** | La atenuación controla cómo disminuye la intensidad de la luz hacia la máxima distancia especificada por la propiedad de intervalo.  Constantes, los propiedades de atenuación Quadradic y lineal pueden usarse.

## <a name="getting-started-with-lighting"></a>Introducción a la luz

Sigue estos pasos generales para agregar las luces:

- Crear y colocar las luces: crear luces y ponerlos en un espacio de coordenadas especificado.
- Identificar los objetos a la luz: luz en elementos visuales relevantes de destino.
- [Opcional] Definir objetos individuales cómo reaccionar a las luces: SceneLightingEffect de uso con una EffectBrush para personalizar la reflexión de la luz para mostrar la clase SpriteVisual. Valores predeterminados de reflexión admiten la iluminación de elementos secundarios de CoordinateSpace la fuente de luz.  Un elemento visual pintado con un objeto SceneLightingEffect sobrescribe la luz predeterminada para ese elemento visual.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) se usa para modificar la iluminación predeterminada aplicada al contenido de una [clase SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) el objetivo de un [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) se usa con frecuencia para la creación de materiales. Un objeto SceneLightingEffect es un efecto que se usa cuando desea lograr algo más complejos, como la habilitación de las propiedades reflectantes en una imagen o proporcionar una ilusión de profundidad con un mapa normal. Un objeto SceneLightingEffect proporciona la capacidad de personalizar la interfaz de usuario mediante las propiedades de iluminación, como los importes especulares y difusas. Puede personalizar aún más los efectos de iluminación con el resto de la canalización de efectos que te permite individualmente de fusión y redactar reacciones de iluminación diferentes con el contenido.

> [!NOTE]
> Iluminación de una escena no producirán sombras; es un efecto centrado en la representación 2D.  No tienen en escenarios de iluminación 3D de tener en cuenta que incluyen modelos de iluminación real, incluidas las sombras.


Propiedad | Descripción
--- | ---
**Mapa normal** | NormalMaps crea un efecto de una textura donde un normal apuntando hacia la luz serán más brillante y distancia comillas que normal será más tenue. Para agregar un NormalMap el uso de visual dirigida un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) con Loadedimagesource para cargar un activo NormalMap.
**Ambiente** | Propiedades de ambiente se usan principalmente para controlar el reflejo de color general.
**Especular** | Reflejo especular crea especulares en objetos, haciendo que aparezcan brillante. Puedes controlar el nivel de reflejo especular, así como el nivel de brillo.  Estas propiedades se manipulan para crear efectos materiales como shinny metales o papel brillante.
**Difuso** | Reflejo difuso dispersa la luz en todas las direcciones.
**Modelo de reflectancia** | [Modelo de reflectancia](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) te permite elegir entre [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) y físicamente función Blinn Phong.  Tendrías físicamente función Blinn Phong cuando quieras han comprimido resaltados especulares.

### <a name="sample-scenelightingeffect"></a>Muestra (objeto SceneLightingEffect)

El siguiente ejemplo muestra cómo agregar un mapa normal a un objeto SceneLightingEffect.

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Crear las luces y materiales en la capa Visual](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Introducción a la iluminación](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Cálculos de iluminación](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Repositorio de GitHub de WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)