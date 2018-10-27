---
author: normesta
title: Mostrar puntos de interés en un mapa
description: Agrega puntos de interés a un mapa con marcadores, imágenes, formas y elementos de la interfaz de usuario de XAML.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.author: normesta
ms.date: 08/11/2017
ms.topic: article
keywords: Windows 10, uwp, mapa, map, ubicación, location, marcadores, pushpins
ms.localizationpriority: medium
ms.openlocfilehash: 13c0ea463cbab97e03c87c4e558bba0eff92300c
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5711102"
---
# <a name="display-points-of-interest-on-a-map"></a>Mostrar puntos de interés en un mapa

Agrega puntos de interés a un mapa con marcadores, imágenes, formas y elementos de la interfaz de usuario de XAML. Un punto de interés es un punto concreto del mapa que representa algo de interés. Por ejemplo, la ubicación de un negocio, ciudad o amigo.

Para obtener más información sobre la presentación de puntos de interés en tu aplicación, descarga la muestra siguiente desde el [repo de muestras universales de Windows](http://go.microsoft.com/fwlink/p/?LinkId=619979) en GitHub: [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977).

Para mostrar marcadores, imágenes y formas en el mapa añadiendo objetos [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077), [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard),  [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) y [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) a una colección **MapElements** de un objeto [**MapElementsLayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer). A continuación, agrega ducho nivel a la colección **Layers** de un control de mapa.

>[!NOTE]
> En versiones anteriores, esta guía mostraba cómo agregar elementos del mapa a la colección [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements). Aunque aún puedes usar este enfoque, puede que te pierdas algunas de las ventajas del nuevo modelo del nivel de mapa. Para obtener información, consulta la sección [Trabajar con niveles](#layers) de esta guía.

También puede mostrar elementos de la interfaz de usuario de XAML como por ejemplo [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) o a [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) en el mapa añadiéndolos a [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) o como propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

Si quieres colocar en el mapa un elevado número de elementos, considera la posibilidad de [superponer imágenes en mosaico en el mapa](overlay-tiled-images.md). Para mostrar carreteras en el mapa, consulta [Mostrar rutas e indicaciones](routes-and-directions.md).

## <a name="add-a-pushpin"></a>Agregar un marcador

Para mostrar una imagen (como, por ejemplo, un marcador) con texto opcional en el mapa, usa la clase [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077). Igualmente, para aceptar la imagen predeterminada o proporcionar una imagen personalizada, puedes usar la propiedad [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078). En la siguiente imagen se muestra la imagen predeterminada de un elemento **MapIcon** que consta de la propiedad [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088), la cual muestra ningún valor especificado, un título corto, un título largo y un título muy largo.

![ejemplo de un elemento MapIcon con títulos de diferentes longitudes.](images/mapctrl-mapicons.png)

En el ejemplo siguiente se muestra un mapa de la ciudad de Seattle, el cual contiene un elemento [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) en la imagen predeterminada y un título opcional para indicar la ubicación de la torre Space Needle. También se centra el mapa en el icono y se acerca el zoom. Para obtener información general sobre cómo usar el control de mapa, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

Este ejemplo muestra el siguiente punto de interés en el mapa (la imagen predeterminada en el centro).

![mapa con MapIcon](images/displaypoidefault.png)

La línea de código siguiente te permite mostrar el elemento [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) con una imagen personalizada guardada en la carpeta Assets del proyecto. La propiedad [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) del elemento **MapIcon** espera un valor de tipo [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Para este tipo es necesaria una declaración **using** para el espacio de nombres [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

>[!NOTE]
>Si usas la misma imagen para varios iconos del mapa, declara la clase [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) en el nivel de página o aplicación para mejorar el rendimiento.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Ten en cuenta lo siguiente si quieres trabajar con la clase [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077):

-   La propiedad [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) admite un tamaño máximo de imagen de 2048×2048 píxeles.
-   De manera predeterminada, no se garantiza que se muestre la imagen del icono del mapa. Esto es debido a que se ocultará si bloquea otros elementos o etiquetas del mapa. Para mantener la imagen siempre visible, establece la propiedad [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) del icono de mapa en [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314).
-   Asimismo, tampoco se garantiza que se muestre el elemento [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) de la clase [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077). Si no ves el texto, disminuye su tamaño; para ello tienes que reducir el valor de la propiedad [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).
-   Si muestras la imagen de un elemento [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) que señala a una ubicación específica del mapa (por ejemplo, un marcador o una flecha), puedes establecer el valor de la propiedad [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) en la ubicación aproximada del puntero de la imagen. Si dejas el valor de **NormalizedAnchorPoint** como el valor predeterminado (0, 0), que representa la esquina superior izquierda de la imagen, puede que los cambios realizados en la propiedad [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) del mapa hagan que la imagen apunte a otra ubicación.
-   Si no estableces explícitamente una propiedad [Altitude](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.basicgeoposition) y [AltitudeReferenceSystem](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem), la [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) se colocará en la superficie.

## <a name="add-a-3d-pushpin"></a>Agregar un marcador 3D

Puedes agregar objetos tridimensionales a un mapa. Utiliza la clase [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) para importar un objeto 3D desde un archivo con [formato de fabricación 3D (3MF)](http://3mf.io/specification/).

Esta imagen utiliza tazas de café 3D para marcar las ubicaciones de cafeterías en un barrio.

![tazas en mapas](images/mugs.png)

El siguiente código agrega una taza de café en el mapa usando la importación de un archivo 3MF. Para hacerlo más sencillo, este código agrega la imagen en el centro del mapa, pero el código es probable que añada la imagen en una ubicación específica.

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>Agregar una imagen

Muestra imágenes de gran tamaño relacionadas con ubicaciones de mapa como una imagen de un restaurante o un punto de referencia. Cuando los usuarios alejan la imagen, esta se reducirá proporcionalmente de tamaño para permitir que el usuario vea más del mapa. Esto es algo diferente a un [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) que marca una ubicación específica, suele ser pequeño y conserva el mismo tamaño, conforme los usuarios acercan o alejan un mapa.

![Imagen de MapBillboard](images/map-billboard.png)

El siguiente código muestra el [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) presentado en la imagen anterior.

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

Hay tres partes de este código que vale la pena examinar un poco más de cerca: la imagen, la cámara de referencia y la propiedad [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint).

### <a name="image"></a>Imagen

Este ejemplo muestra una imagen personalizada guardada en la carpeta **Activos** del proyecto. La propiedad [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) del elemento [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) espera un valor de tipo [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Para este tipo es necesaria una declaración **using** para el espacio de nombres [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

>[!NOTE]
>Si usas la misma imagen para varios iconos del mapa, declara la clase [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) en el nivel de página o aplicación para mejorar el rendimiento.

### <a name="reference-camera"></a>Cámara de referencia

 Dado que una imagen [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) se escala vertical y horizontalmente conforme el [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) del mapa cambia, es importante definir en qué lugar de ese [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) la imagen aparece en una escala de 1x normal. Esa posición se define en la cámara de referencia del [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard), y para establecerla, tendrás que pasar un objeto [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) al constructor del [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard).

 Puedes definir la posición que quieras en un [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) y, a continuación, usar ese [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) para crear un objeto [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera).  Sin embargo, en este ejemplo, solo usamos el objeto [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) devuelto por la propiedad [**ActualCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) del control de mapa. Se trata de la cámara interna del mapa. La posición actual de la cámara se convierte en la posición de la cámara de referencia; la posición donde la imagen de [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) aparece en escala de 1x.

 Si tu aplicación ofrece a los usuarios la capacidad de alejar el mapa, se reduce el tamaño de la imagen porque la cámara interna de mapas sube en altitud mientras la imagen en escala de 1x permanece fija en la posición de la cámara de referencia.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

El [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) es el punto de la imagen que está anclado a la propiedad [**Ubicación**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) de [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). El punto 0.5,1 es el centro de la parte inferior de la imagen. Como hemos establecido la propiedad [**Ubicación**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) de [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) en el centro del control del mapa, se anclará el centro de la parte inferior de la imagen al centro del control de mapas. Si quieres que la imagen aparezca centrada directamente sobre un punto, Establece establezca [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) en 0,5, 0,5.  

## <a name="add-a-shape"></a>Agregar una forma

Usa la clase [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) para mostrar una forma multipunto en el mapa. En el siguiente ejemplo, obtenido de una [muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), se muestra en el mapa un cuadro rojo con el borde azul.

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>Agregar una línea


Usa la clase [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) para mostrar una línea en el mapa. En el siguiente ejemplo, obtenido de una [muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), se muestra una línea discontinua en el mapa.

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>Agregar XAML

Visualiza elementos de la interfaz de usuario personalizados en el mapa mediante XAML. Coloca XAML en el mapa especificando la ubicación y el punto de anclaje normalizado del XAML.

-   Llama a [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) para establecer la ubicación del mapa donde debe colocarse el código XAML.
-   Llama a [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) para establecer la ubicación relativa en el código XAML que corresponde a la ubicación especificada.

En el ejemplo siguiente se muestra un mapa de la ciudad de Seattle y se agrega un control [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) de XAML para indicar la ubicación de la torre Space Needle. También se centra el mapa en el área y se acerca el zoom. Para obtener información general sobre cómo usar el control de mapa, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

Este ejemplo muestra un borde azul en el mapa.

![Captura de pantalla de xaml mostrada en el punto de intersección del mapa](images/displaypoixaml.png)

Los siguientes ejemplos muestran cómo agregar elementos de interfaz de usuario de XAML directamente en el marcado XAML de la página mediante el enlace de datos. Igual que sucede con otros elementos XAML que muestran contenido, [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) es la propiedad de contenido predeterminada de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), y no es necesario especificarla expresamente en el marcado XAML.

En este ejemplo se indica cómo puedes mostrar dos controles XAML como objetos secundarios implícitos de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Estos controles aparecen en el mapa en las ubicaciones enlazadas de datos.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

Establece estas ubicaciones usando propiedades del archivo de código subyacente.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

Este ejemplo muestra cómo mostrar dos controles XAML incluidos dentro de un [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094). Estos controles aparecen en el mapa en las ubicaciones enlazadas de datos.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

En este ejemplo se muestra una colección de elementos XAML enlazados a la clase [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094).

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

La propiedad ``ItemsSource`` en el ejemplo anterior se enlaza a una propiedad de tipo [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist?view=netframework-4.70) en el archivo de código subyacente.

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>Trabajar con niveles

Los ejemplos de esta guía agregar elementos a una colección [MapElementLayers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer). A continuación, se muestra cómo agregar dicha colección a la propiedad **Layers** del control de mapa. En versiones anteriores, esta guía mostraba cómo agregar elementos del mapa a la colección [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) como se indica a continuación:

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

Aunque aún puedes usar este enfoque, puede que te pierdas algunas de las ventajas del nuevo modelo del nivel de mapa. Al agrupar los elementos en niveles, puede manipular cada nivel de forma independiente entre sí. Por ejemplo, cada capa tiene su propio conjunto de eventos para que puedas responder a un evento en una capa específica y realizar una acción específica para dicho evento.

Asimismo, puede enlazar XAML directamente a [MapLayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maplayer). Esto es algo que no se puede hacer con la colección [MapElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements).

Una forma de hacerlo es mediante el uso de una clase de modelo de vista, el código subyacente de una página XAML y una página XAML.

### <a name="view-model-class"></a>Clase de modelo de vista

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>Código subyacente de una página XAML

Conecta la clase de modelo de vista a la página de código subyacente.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>Página XAML

En la página XAML, enlaza a la propiedad en la clase de modelo de vista que devuelve el nivel.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>Artículos relacionados

* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP traffic app sample (Ejemplo de aplicación de tráfico de UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)
