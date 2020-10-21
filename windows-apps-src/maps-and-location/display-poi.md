---
title: Mostrar puntos de interés en un mapa
description: Agrega puntos de interés a un mapa con marcadores, imágenes, formas y elementos de la interfaz de usuario de XAML.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, mapa, ubicación, chinchetas
ms.localizationpriority: medium
ms.openlocfilehash: feaf5edc4a25ebbc6dd3e3b7eb484ff63f8b1a23
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297773"
---
# <a name="display-points-of-interest-on-a-map"></a>Mostrar puntos de interés en un mapa

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

Agrega puntos de interés a un mapa con marcadores, imágenes, formas y elementos de la interfaz de usuario de XAML. Un punto de interés es un punto concreto del mapa que representa algo de interés. Por ejemplo, la ubicación de un negocio, ciudad o amigo.

Para obtener más información sobre cómo mostrar el interés en la aplicación, descargue el ejemplo siguiente del [repositorio Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) en github: [ejemplo de mapa de plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).

Para mostrar chinchetas, imágenes y formas en el mapa, agregue los objetos [**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon), [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard),  [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)y [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) a una colección **MapElement** de un objeto [**MapElementsLayer**](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) . A continuación, agregue ese objeto de capa a la colección de **capas** de un control de mapa.

>[!NOTE]
> En las versiones anteriores, en esta guía se mostró cómo agregar elementos de mapa a la colección [**MapElement**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) . Aunque puede seguir usando este enfoque, omitirá algunas de las ventajas del nuevo modelo de capas de mapa. Para obtener más información, consulte la sección [trabajar con capas](#layers) de esta guía.

También puede mostrar elementos de la interfaz de usuario XAML, como un [**botón**](/uwp/api/Windows.UI.Xaml.Controls.Button), un elemento [**HyperlinkButton**](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)o un [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) en la asignación, agregándolos a [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl) o como [**elementos secundarios**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) de la [**clase MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

Si quieres colocar en el mapa un elevado número de elementos, considera la posibilidad de [superponer imágenes en mosaico en el mapa](overlay-tiled-images.md). Para mostrar las carreteras en el mapa, consulte [Mostrar rutas y direcciones](routes-and-directions.md)

## <a name="add-a-pushpin"></a>Agregar un marcador

Para mostrar una imagen (como, por ejemplo, un marcador) con texto opcional en el mapa, usa la clase [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon). Igualmente, para aceptar la imagen predeterminada o proporcionar una imagen personalizada, puedes usar la propiedad [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image). En la siguiente imagen se muestra la imagen predeterminada de un elemento **MapIcon** que consta de la propiedad [**Title**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title), la cual muestra ningún valor especificado, un título corto, un título largo y un título muy largo.

![ejemplo de un elemento MapIcon con títulos de diferentes longitudes.](images/mapctrl-mapicons.png)

En el ejemplo siguiente se muestra un mapa de la ciudad de Seattle, el cual contiene un elemento [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) en la imagen predeterminada y un título opcional para indicar la ubicación de la torre Space Needle. También se centra el mapa en el icono y se acerca el zoom. Para obtener información general sobre el uso del control de mapa, vea [Mostrar mapas con las vistas 2D, 3D y streetside](display-maps.md).

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

La línea de código siguiente te permite mostrar el elemento [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) con una imagen personalizada guardada en la carpeta Assets del proyecto. La propiedad [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) del elemento **MapIcon** espera un valor de tipo [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Para este tipo es necesaria una declaración **using** para el espacio de nombres [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Si usa la misma imagen para varios iconos de mapa, declare [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) en el nivel de página o aplicación para obtener el mejor rendimiento.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Ten en cuenta lo siguiente si quieres trabajar con la clase [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon):

-   La propiedad [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) admite un tamaño máximo de imagen de 2048×2048 píxeles.
-   De manera predeterminada, no se garantiza que se muestre la imagen del icono del mapa. Esto es debido a que se ocultará si bloquea otros elementos o etiquetas del mapa. Para mantenerla visible, establezca la propiedad [**CollisionBehaviorDesired**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.collisionbehaviordesired) del icono de mapa en [**MapElementCollisionBehavior. RemainVisible**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapElementCollisionBehavior).
-   Asimismo, tampoco se garantiza que se muestre el elemento [**Title**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) de la clase [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon). Si no ve el texto, reduzca el valor de la propiedad [**zoomLevel (**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) de la [**clase MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).
-   Si muestras la imagen de un elemento [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) que señala a una ubicación específica del mapa (por ejemplo, un marcador o una flecha), puedes establecer el valor de la propiedad [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.normalizedanchorpoint) en la ubicación aproximada del puntero de la imagen. Si dejas el valor de **NormalizedAnchorPoint** como el valor predeterminado (0, 0), que representa la esquina superior izquierda de la imagen, puede que los cambios realizados en la propiedad [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) del mapa hagan que la imagen apunte a otra ubicación.
-   Si no establece explícitamente una [altitud](/uwp/api/windows.devices.geolocation.basicgeoposition) y [AltitudeReferenceSystem](/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem), el [**mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) se colocará en la superficie.

## <a name="add-a-3d-pushpin"></a>Agregar un PIN 3D

Puedes agregar objetos tridimensionales a un mapa. Use la clase [MapModel3D](/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) para importar un objeto 3D desde un archivo de [formato de fabricación 3D (3MF)](https://3mf.io/specification/) .

Esta imagen usa copas de café 3D para marcar las ubicaciones de las cafeterías en un barrio.

![tazas en mapas](images/mugs.png)

En el código siguiente se agrega una taza de café al mapa mediante la importación de un archivo 3MF. Para simplificar las cosas, este código agrega la imagen al centro del mapa, pero es probable que el código agregue la imagen a una ubicación específica.

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

## <a name="add-an-image"></a>Añadir una imagen

Mostrar imágenes grandes relacionadas con ubicaciones de mapa como una imagen de un restaurante o un punto de referencia. A medida que los usuarios alejan, la imagen se reducirá proporcionalmente en tamaño para que el usuario pueda ver más de la asignación. Es un poco diferente de un [**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) que marca una ubicación específica, suele ser pequeña y sigue siendo el mismo tamaño que los usuarios acercar y alejar un mapa.

![Imagen de MapBillboard](images/map-billboard.png)

En el código siguiente se muestra el [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) presentado en la imagen anterior.

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

Hay tres partes de este código que merece la pena examinar un poco más cerca: la imagen, la cámara de referencia y la propiedad [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) .

### <a name="image"></a>Imagen

En este ejemplo se muestra una imagen personalizada guardada en la carpeta **assets** del proyecto. La propiedad [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) espera un valor de tipo [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Para este tipo es necesaria una declaración **using** para el espacio de nombres [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Si usa la misma imagen para varios iconos de mapa, declare [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) en el nivel de página o aplicación para obtener el mejor rendimiento.

### <a name="reference-camera"></a>Cámara de referencia

 Dado que una imagen de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) se escala hacia arriba y hacia fuera a medida que el [**zoomLevel (**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) de la asignación cambia, es importante definir dónde en ese [**zoomLevel (**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) aparece la imagen a una escala de 1x normal. Esa posición se define en la cámara de referencia de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)y, para establecerla, tendrá que pasar un objeto [**MapCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) al constructor de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard).

 Puede definir la posición que desee en un [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint)y, a continuación, utilizar ese [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint) para crear un objeto [**MapCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) .  Sin embargo, en este ejemplo, solo se usa el objeto [**MapCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) devuelto por la propiedad [**ActualCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) del control de mapa. Esta es la cámara interna del mapa. La posición actual de esa cámara se convierte en la posición de la cámara de referencia; posición en la que la imagen de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) aparece en una escala de 1x.

 Si la aplicación proporciona a los usuarios la capacidad de alejar el mapa, la imagen se reduce de tamaño porque la cámara interna de Maps está aumentando en altitud mientras que la imagen en la escala 1x permanece fija en la posición de la cámara de referencia.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

[**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) es el punto de la imagen que está anclado a la propiedad [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). El punto 0.5, 1 es el centro inferior de la imagen. Dado que hemos establecido la propiedad [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) en el centro del control del mapa, el centro inferior de la imagen se delimitará en el centro del control de mapas. Si desea que la imagen aparezca centrada directamente sobre un punto, establezca [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) en 0.5, 0.5.  

## <a name="add-a-shape"></a>Adición de una forma

Usa la clase [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon) para mostrar una forma multipunto en el mapa. En el siguiente ejemplo, obtenido de una [muestra de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl), se muestra en el mapa un cuadro rojo con el borde azul.

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

## <a name="add-a-line"></a>Adición de una línea


Usa la clase [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) para mostrar una línea en el mapa. En el siguiente ejemplo, obtenido de una [muestra de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl), se muestra una línea discontinua en el mapa.

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

-   Llama a [**SetLocation**](/windows/desktop/tablet/icontextnode-setlocation) para establecer la ubicación del mapa donde debe colocarse el código XAML.
-   Establezca la ubicación relativa en el código XAML que corresponde a la ubicación especificada mediante una llamada a [**SetNormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setnormalizedanchorpoint).

En el ejemplo siguiente se muestra un mapa de la ciudad de Seattle y se agrega un control [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) de XAML para indicar la ubicación de la torre Space Needle. También se centra el mapa en el área y se acerca el zoom. Para obtener información general sobre el uso del control de mapa, vea [Mostrar mapas con las vistas 2D, 3D y streetside](display-maps.md).

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

Los siguientes ejemplos muestran cómo agregar elementos de interfaz de usuario de XAML directamente en el marcado XAML de la página mediante el enlace de datos. Igual que sucede con otros elementos XAML que muestran contenido, [**Children**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) es la propiedad de contenido predeterminada de la clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), y no es necesario especificarla expresamente en el marcado XAML.

En este ejemplo se muestra cómo mostrar dos controles XAML como elementos secundarios implícitos de la [**clase MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl). Estos controles aparecen en el mapa en las ubicaciones enlazadas a datos.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

Establezca estas ubicaciones mediante el uso de propiedades en el archivo de código subyacente.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

En este ejemplo se muestra cómo mostrar dos controles XAML contenidos dentro de un [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl). Estos controles aparecen en el mapa en las ubicaciones enlazadas a datos.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

En este ejemplo se muestra una colección de elementos XAML enlazados a un [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl).

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

La ``ItemsSource`` propiedad en el ejemplo anterior se enlaza a una propiedad de tipo [IList](/dotnet/api/system.collections.ilist) en el archivo de código subyacente.

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

## <a name="working-with-layers"></a>Trabajar con capas

Los ejemplos de esta guía agregan elementos a una colección [MapElementLayers](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) . A continuación, se muestra cómo agregar esa colección a la propiedad **Layers** del control de mapa. En las versiones anteriores, en esta guía se mostró cómo agregar elementos de mapa a la colección [**MapElement**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) de la siguiente manera:

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

Aunque puede seguir usando este enfoque, omitirá algunas de las ventajas del nuevo modelo de capas de mapa. Al agrupar los elementos en capas, puede manipular cada capa de forma independiente entre sí. Por ejemplo, cada capa tiene su propio conjunto de eventos para que puedas responder a un evento en un nivel específico y realizar una acción específica para dicho evento.

Además, puede enlazar XAML directamente a un [microjugador](/uwp/api/windows.ui.xaml.controls.maps.maplayer). Esto es algo que no se puede hacer mediante la colección [MapElement](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)  .

Una forma de hacerlo es mediante una clase de modelo de vista, una página XAML de código subyacente y una página XAML.

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

Conecte la clase de modelo de vista a la página de código subyacente.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>Página XAML

En la página XAML, enlace a la propiedad en la clase de modelo de vista que devuelve la capa.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>Temas relacionados

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Directrices de diseño para mapas](./display-maps.md)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechar los mapas y la ubicación entre teléfonos, tabletas y equipos en tus aplicaciones Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)
* [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)
* [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline)
