---
title: Iluminación de composición
description: Las API de iluminación de composición se pueden usar para agregar iluminación 3D dinámica a la aplicación.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154709"
---
# <a name="using-lights-in-windows-ui"></a>Usar luces en la interfaz de usuario de Windows

Las API de Windows. UI. Composition permiten crear animaciones y efectos en tiempo real. La iluminación de composición permite la iluminación 3D en aplicaciones 2D. En esta información general, ejecutaremos la funcionalidad de cómo configurar las luces de composición, identificar objetos visuales para recibir cada luz y usar efectos para definir materiales para el contenido.

> [!NOTE]
> Para leer cómo los objetos [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) aplican [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar XAML UIElements, vea [iluminación XAML](xaml-lighting.md).

La iluminación de composición permite crear una interfaz de usuario interesante al permitir:

- Transformación de una luz independiente de otros objetos de la escena para habilitar escenarios envolventes, como escenas de reproducción de música.
- La capacidad de emparejar un objeto con una luz para que se muevan juntos independientemente del resto de la escena para habilitar escenarios como resaltado de [visualización](../design/style/reveal.md) fluida.
- Transformación de la luz y la escena completa como un grupo para crear materiales y profundidad.

La iluminación de composición admite tres conceptos clave: **Light**, **targets**y **SceneLightingEffect**.

## <a name="light"></a>Claro

[CompositionLight](/uwp/api/windows.ui.composition.compositionlight) permite crear diversas luces y colocarlas en el espacio de coordenadas. Estas luces tienen como destino objetos visuales que desea identificar como iluminados por la luz.

### <a name="light-types"></a>Tipos de luz

| Tipo | Descripción |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Una fuente de luz que emite una luz no direccional que aparece reflejada por todo el código de la escena. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Una fuente de luz infinitamente grande de gran tamaño que emite luz en una sola dirección. Como el sol. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Origen de punto de luz que emite luz en todas las direcciones. Como una bombilla. |
| [Principales](/uwp/api/windows.ui.composition.spotlight) | Fuente de luz que emite conos internos y exteriores de luz. Como una linterna. |

## <a name="targets"></a>Destinos

Cuando las luces tienen como destino un visual (agregar a la lista de [destinos](/uwp/api/windows.ui.composition.compositionlight.targets) ), el código Visual y todos sus descendientes son conscientes y responden a esta fuente de luz. Esto puede ser algo tan sencillo como establecer un origen de PointLight en la raíz de un árbol y todos los objetos visuales que se muestran a continuación reaccionan a la animación de la dirección de las luces de punto.

**ExclusionsFromTargets** ofrece la posibilidad de quitar la iluminación de un gráfico visual o de un subárbol de objetos visuales de forma similar a la adición de destinos. Los elementos secundarios en el árbol cuya raíz es el elemento visual que se excluye no se iluminan como resultado.

### <a name="sample-targets"></a>Ejemplo (destinos)

En el ejemplo siguiente, usamos un CompositionPointLight para el destino de un TextBlock de XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Al agregar animación al desplazamiento de la luz puntual, se consigue fácilmente un efecto de Shimmering.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Para obtener más información, consulte el ejemplo de [Shimmer de texto](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) completo en la Galera de ejemplo de WindowUIDevLabs.

## <a name="restrictions"></a>Restricciones

Hay varios factores que se deben tener en cuenta a la hora de determinar qué contenido se iluminará con CompositionLight.

Concepto | Detalles
--- | ---
**Luz ambiente** | Al agregar una luz no ambiente a la escena, se desactivará toda la luz existente.  Los elementos no dirigidos por una luz no ambiente aparecerán en negro.  Para iluminar objetos visuales circundantes no dirigidos por la luz de forma natural, use una luz ambiente junto con otras luces.
**Número de luces** | Puede usar dos luces de composición no ambiente en cualquier combinación para dirigirse a la interfaz de usuario. Las luces ambiente no están restringidas. las luces puntuales, puntuales y distantes son.
**Duración** | CompositionLight puede experimentar condiciones de duración (ejemplo: el recolector de elementos no utilizados puede reciclar el objeto de luz antes de que se use).  Se recomienda mantener una referencia a las luces agregando luces como miembro para ayudar a la aplicación a administrar la duración.
**Transformaciones** | Las luces deben colocarse en un nodo encima de la interfaz de usuario que utilice efectos como las [transformaciones de perspectiva](../design/layout/3-d-perspective-effects.md) de la estructura visual para que se dibujen correctamente.
**Destinos y espacio de coordenadas** | CoordinateSpace es el espacio visual en el que se deben establecer todas las propiedades de las luces. CompositionLight. targets debe estar en el árbol CoordinateSpace.

## <a name="lighting-properties"></a>Propiedades de iluminación

Dependiendo del tipo de luz utilizada, una luz puede tener propiedades para la atenuación y el espacio. No todos los tipos de luces utilizan todas las propiedades.

Propiedad | Descripción
--- | ---
**Color** | [Color](/uwp/api/windows.ui.color) de la luz. Los valores de color de iluminación se definen mediante la difusión de [D3D](../graphics-concepts/light-properties.md) , ambiente y especular que define el color que se va a emitir. La iluminación usa valores RGBA para las luces; no se usa el componente de color alfa.
**Dirección** | Dirección de la luz. La dirección en la que apunta la luz se especifica en relación con el visual [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) .
**Espacio de coordenadas** | Cada visual tiene un espacio de coordenadas 3D implícito. La dirección X está de izquierda a derecha. La dirección Y es de arriba abajo. La dirección Z es un punto fuera del plano. El punto original de esta coordenada es la esquina superior izquierda del visual y la unidad es el píxel independiente del dispositivo (DIP). Desplazamiento de una luz definido en esta coordenada.
**Conos internos y exteriores** | Los focos emiten un cono de luz que tiene dos partes: un cono interior brillante y un cono exterior. La composición permite controlar el color y los ángulos de cono internos y externos.
**Offset** | Desplazamiento de la fuente de luz en relación con el visual del espacio de coordenadas.

> [!NOTE]
> Cuando varias luces alcanzan el mismo visual, o cuando el valor de color de una luz es lo suficientemente grande como para superar 1,0, el color de la luz puede cambiar debido a la compresión de un canal de color de luces.

### <a name="advanced-lighting-properties"></a>Propiedades de iluminación avanzadas

Propiedad | Descripción
--- | ---
**Intensidad** | Controla el brillo de la luz.
**Atenuación** | La atenuación controla cómo se reduce la intensidad de una luz hacia la distancia máxima especificada por la propiedad de intervalo.  Se pueden usar propiedades de atenuación constante, Quadradic y lineal.

## <a name="getting-started-with-lighting"></a>Introducción con iluminación

Siga estos pasos generales para agregar luces:

- Crear y colocar las luces: crear luces y colocarlas en un espacio de coordenadas especificado.
- Identifique los objetos que se deben aclarar: la luz de destino en los objetos visuales pertinentes.
- Opta Defina cómo reaccionan los objetos individuales a las luces: Use SceneLightingEffect con un EffectBrush para personalizar la reflexión ligera para mostrar el objeto SpriteVisual. Los valores predeterminados de reflexión admiten la iluminación de los elementos secundarios del CoordinateSpace de una fuente de luz.  Un visual pintado con un SceneLightingEffect sobrescribe la iluminación predeterminada para ese visual.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) se usa para modificar la iluminación predeterminada aplicada al contenido de una [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) de destino de un [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) se usa con frecuencia para la creación de materiales. Un SceneLightingEffect es un efecto que se usa cuando se desea lograr algo más complejo, como habilitar propiedades reflectantes en una imagen o proporcionar una ilusión de profundidad con un mapa normal. Un SceneLightingEffect proporciona la capacidad de personalizar la interfaz de usuario mediante el uso de propiedades de iluminación como especular y cantidades difusas. Puede personalizar aún más los efectos de iluminación con el resto de la canalización de efectos, lo que le permite mezclar y componer diferentes reacciones de iluminación con el contenido.

> [!NOTE]
> La iluminación de escenas no produce sombras; es un efecto centrado en la representación en 2D.  No tiene en cuenta los escenarios de iluminación 3D que incluyen modelos de iluminación reales, incluidas las sombras.


Propiedad | Descripción
--- | ---
**Asignación normal** | NormalMaps crear un efecto de textura en el que una señal normal que apunte hacia la luz será más brillante y se verá atenuada una señal normal. Para agregar un NormalMap al elemento visual de destino, use [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) con LoadedImageSurface para cargar un recurso de NormalMap.
**Ambiente** | Principalmente, las propiedades de ambiente se utilizan para controlar la reflexión de color global.
**Especular** | La reflexión especular crea resaltados en los objetos, por lo que aparecen de forma brillante. Puede controlar el nivel de reflexión especular así como el nivel de brillo.  Estas propiedades se manipulan para crear efectos materiales como metales shinnys o papel satinado.
**Difusa** | La reflexión difusa dispersión la luz en todas las direcciones.
**Modelo de reflectancia** | El [modelo de reflectancia](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) permite elegir entre [Blinn Phong](/visualstudio/designers/how-to-create-a-basic-phong-shader) y Blinn Phong basado físicamente.  Elegiría físicamente Blinn Phong si desea tener reflejos especulares condensados.

### <a name="sample-scenelightingeffect"></a>Ejemplo (SceneLightingEffect)

En el ejemplo siguiente se muestra cómo agregar un mapa normal a un SceneLightingEffect.

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

- [Crear materiales y luces en la capa visual](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Información general sobre iluminación](../graphics-concepts/lighting-overview.md)
- [API de CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities)
- [Matemáticas de iluminación](../graphics-concepts/mathematics-of-lighting.md)
- [SceneLightingEffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Repositorio de GitHub de WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples)