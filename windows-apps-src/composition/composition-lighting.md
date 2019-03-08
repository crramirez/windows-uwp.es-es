---
title: Iluminación de composición
description: Las API de iluminación de composición puede utilizarse para agregar iluminación 3D dinámica a la aplicación.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 733ce75942a05482ade88c1510e788f1cbd515d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602210"
---
# <a name="using-lights-in-windows-ui"></a>Uso de las luces en la interfaz de usuario de Windows

Las APIs Windows.UI.Composition permiten crear efectos y animaciones en tiempo real. Composición iluminación permite iluminación 3D en aplicaciones 2D. En esta introducción, se ejecutará a través de la funcionalidad de cómo configurar las luces de composición, identificar los objetos visuales para recibir cada luz y usar efectos para definir los materiales para su contenido.

> [!NOTE]
> Cómo leer [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) aplican objetos [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar la IU de XAML, vea [iluminación XAML](xaml-lighting.md).

Iluminación de composición le permite crear interesantes de la interfaz de usuario, ya que permite:

- Transformación de un claro e independiente de otros objetos de la escena para habilitar escenarios envolventes como escenas de reproducción de música.
- La capacidad de emparejar un objeto con una luz de forma que se muevan entre sí independientemente del resto de la escena para habilitar escenarios como Fluent [revelar](/windows/uwp/design/style/reveal) resaltar.
- Transformación de la luz y la escena completa como un grupo para crear materiales y la profundidad.

Iluminación de composición es compatible con tres conceptos clave: **Luz**, **destinos**, y **SceneLightingEffect**.

## <a name="light"></a>Claro

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) le permite crear diversas luces y colocarlos en el espacio de coordenadas. Estas luces tener como destino los objetos visuales que desea identificar como iluminados.

### <a name="light-types"></a>Tipos de luz

| Tipo | Descripción |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Una fuente de luz que emite luz no direccional que aparece se refleja en todos los elementos de la escena. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Una gran infinitamente fuente de luz distante que emite luz en una dirección única. Al igual que el sol. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Punto de fuente de luz que emite luz en todas las direcciones. Al igual que una bombilla. |
| [SpotLight](/uwp/api/windows.ui.composition.spotlight) | Una fuente de luz que emite conos internas y externas de la luz. Al igual que una linterna. |

## <a name="targets"></a>Destinos

Cuando un objeto Visual de destino de las luces (agregar a [destinos](/uwp/api/windows.ui.composition.compositionlight.targets) lista), el objeto Visual y todos sus descendientes son conscientes de y responder a esta fuente de luz. Esto puede ser algo tan sencillo como una configuración de un origen PointLight en la raíz de un árbol y todos los objetos visuales siguiente reaccionar a la animación de la dirección de punto de luces.

**ExclusionsFromTargets** le ofrece la capacidad de quitar la iluminación de un objeto visual o de un subárbol de objetos visuales de forma similar, como la adición de destinos. Los elementos secundarios en el árbol cuya raíz está en el objeto visual que se ha excluido no se encienden como resultado.

### <a name="sample-targets"></a>Ejemplo (destinos)

En el ejemplo siguiente, usamos un CompositionPointLight como destino un TextBlock en XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Mediante la adición de animación para el desplazamiento de la luz puntual, un efecto shimmering se consigue fácilmente.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Consulte la sección completa [texto de reflejos](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) ejemplo en la cocina de ejemplo WindowUIDevLabs para obtener más información.

## <a name="restrictions"></a>Restricciones

Hay varios factores a tener en cuenta al determinar qué contenido estarán iluminado CompositionLight.

Concepto | Detalles
--- | ---
**Luz ambiente** | Agregar una luz no ambiente a la escena se desactivará toda la luz existente.  Elementos no apunta a una luz ambiente no aparecerá en negros.  Para iluminar objetos visuales adyacentes no apunta a la luz de manera natural, utilice una luz ambiente junto con otras luces.
**Número de luces** | Puede usar cualquier dos luces de composición que no es ambiente en cualquier combinación a la interfaz de usuario de destino. Las luces de ambientales no están restringidas; punto, punto y luces distantes son.
**duración** | CompositionLight puede experimentar condiciones de duración (ejemplo: el recolector de elementos no utilizados puede reciclar el objeto de luz antes de usarla).  Se recomienda que mantenga una referencia a las luces mediante la adición de luces como un miembro para ayudar a la aplicación Administración de vigencia.
**Transformaciones** | Las luces se deben colocar en un nodo por encima de la interfaz de usuario que usa efectos como [perspectiva transformaciones](/windows/uwp/design/layout/3-d-perspective-effects) dentro de la estructura visual para dibujar correctamente.
**Destinos y espacio de coordenadas** | CoordinateSpace es el espacio visual en la que todo se deben establecer las propiedades de las luces. Debe ser CompositionLight.Targets dentro del árbol CoordinateSpace.

## <a name="lighting-properties"></a>Propiedades de iluminación

Según el tipo de luz utilizado, una luz puede tener propiedades de atenuación y espacio. No todos los tipos de luces usan todas las propiedades.

Propiedad | Descripción
--- | ---
**Color** | El [Color](/uwp/api/windows.ui.color) de la luz. Define valores de color de iluminación [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) difuso, ambiente y especular que define el color que se emiten. Iluminación utiliza valores RGBA de luces; no se utiliza el componente alfa del color.
**Dirección** | La dirección de la luz. Se especifica la dirección en la que señala la luz relativo a su [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual.
**Espacio de coordenadas** | Cada objeto Visual tiene un espacio de coordenadas 3D implícito. Dirección del eje X es de izquierda a derecha. Dirección del eje Y es de arriba a abajo. Dirección Z es un punto fuera del plano. El punto de estas coordenadas original es la esquina superior izquierda del objeto visual y la unidad es píxeles independientes de dispositivo (DIP). Desplazamiento de la luz definido en estas coordenadas.
**Conos internas y externas** | Los focos de luz emiten un cono de luz que tiene dos partes: un cono interno brillante y un cono externo. Composición permite que controlar a través de color y los ángulos de cono interno y externo.
**Offset** | Desplazamiento de la fuente de luz en relación con su espacio de coordenadas Visual.

> [!NOTE]
> Cuando varios luces alcanza el mismo objeto Visual, o cada vez que el valor de color de la luz obtiene suficientemente grande como para superar 1.0, puede cambiar el color de la luz a causa de bloqueo de un canal de color de las luces.

### <a name="advanced-lighting-properties"></a>Propiedades de iluminación de avanzada

Propiedad | Descripción
--- | ---
**Intensidad** | Controla el brillo de la luz.
**Atenuación** | La atenuación controla cómo disminuye la intensidad de la luz hacia la máxima distancia especificada por la propiedad de intervalo.  Constante, pueden se usar propiedades de atenuación Quadradic y lineal.

## <a name="getting-started-with-lighting"></a>Introducción a iluminación

Siga estos pasos generales para agregar las luces:

- Cree y coloque las luces: Cree las luces y colocarlos en un espacio de coordenadas especificado.
- Identifique los objetos a la luz: Destino de luz en objetos visuales pertinentes.
- [Opcional] Definir objetos individuales cómo reaccionar ante las luces: Usar SceneLightingEffect con un EffectBrush para personalizar el reflejo de la luz para mostrar el SpriteVisual. Los valores predeterminados de reflexión admiten la iluminación de elementos secundarios de CoordinateSpace de una fuente de luz.  Un objeto visual pintado con un SceneLightingEffect sobrescribe la iluminación predeterminado para el objeto visual.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) se usa para modificar la iluminación predeterminada aplicada al contenido de un [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) que apunta a un [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) se utiliza con frecuencia para la creación de material. Un SceneLightingEffect es un efecto que se usa cuando desea lograr algo más complejo, como habilitar propiedades brillantes de una imagen o proporcionar una ilusión de profundidad con un mapa normal. Un SceneLightingEffect proporciona la capacidad para personalizar la interfaz de usuario mediante el uso de las propiedades de iluminación como cantidades difusos y especulares. Puede personalizar aún más los efectos de iluminación con el resto de la canalización de efectos permitiéndole individualmente de blend y componer las reacciones de iluminación diferentes con el contenido.

> [!NOTE]
> Iluminación de la escena no genera sombras; es un efecto que se centra en la representación 2D.  Esto no tiene en escenarios de iluminación 3D consideración que incluyen modelos de iluminación real, incluidas las sombras.


Propiedad | Descripción
--- | ---
**Mapa normal** | NormalMaps crear un efecto de una textura donde un normal apuntando hacia la luz estarán más brillante e inmediatamente un señalando normal será tenue. Para agregar un NormalMap al destino visual uso un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) utilizando LoadedImageSurface para cargar un recurso NormalMap.
**Ambient** | Las propiedades de ambiente se utilizan principalmente para controlar la reflexión de color general.
**Specular** | Reflexión especular crea resaltes en objetos, haciéndolos aparecer brillante. Puede controlar el nivel de reflexión especular, así como el nivel de brillo.  Estas propiedades se manipulan para crear efectos materiales como shinny metales o papel satinado.
**Color difuso** | Reflexión difusa esparce la luz en todas las direcciones.
**Modelo de reflectancia** | [Modelo de reflectancia](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) le permite elegir entre [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) y basa Blinn Phong físicamente.  Elegiría físicamente en función Blinn Phong cuando desee ha condensado resaltes especulares.

### <a name="sample-scenelightingeffect"></a>Ejemplo (SceneLightingEffect)

El ejemplo siguiente muestra cómo agregar un mapa normal a un SceneLightingEffect.

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

- [Creación de materiales y luces en la capa Visual](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Información general de iluminación](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Matemáticas de iluminación](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Repositorio de GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)
