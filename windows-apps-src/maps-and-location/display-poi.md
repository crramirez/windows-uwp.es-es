---
author: msatranjr
title: "Mostrar puntos de interés en un mapa"
description: "Agrega puntos de interés a un mapa con marcadores, imágenes, formas y elementos de la interfaz de usuario de XAML."
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
translationtype: Human Translation
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: aec420d6591546e63c6343d7151afe9e95d1afd8

---

# Mostrar puntos de interés en un mapa


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Agrega puntos de interés a un mapa con marcadores, imágenes, formas y elementos de la interfaz de usuario de XAML. Un punto de interés es un punto concreto del mapa que representa algo de interés. Por ejemplo, la ubicación de un negocio, ciudad o amigo.

**Sugerencia** Para saber cómo puedes indicar puntos de interés en tu aplicación, descarga la muestra siguiente del [repositorio de muestras universales de Windows](http://go.microsoft.com/fwlink/p/?LinkId=619979) que encontrarás en GitHub.

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Para mostrar marcadores, imágenes y formas en el mapa, agrega los objetos [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077), [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) y [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) a la colección [**MapElements**](https://msdn.microsoft.com/library/windows/apps/dn637033) del control de mapa. Puedes usar el enlace de datos o agregar elementos mediante programación, pero no es posible realizar el enlace a la colección **MapElements** mediante declaración en el marcado XAML.

Muestra elementos de la interfaz de usuario de XAML en el mapa como, por ejemplo, [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) o [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) y agrégalos como una propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Asimismo, también los puedes agregar a la clase [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) o puedes enlazar **MapItemsControl** a una colección de elementos.

En resumen:

-   [Agrega MapIcon al mapa](#mapicon) para mostrar una imagen, por ejemplo, un marcador con texto opcional.
-   [Agrega MapPolygon al mapa](#mappolygon) para mostrar una forma multipunto.
-   [Agrega MapPolyline al mapa](#mappolyline) para mostrar las líneas en el mapa.
-   [Agrega código XAML al mapa](#mapxaml) para mostrar elementos de la interfaz de usuario personalizados.

Si quieres colocar en el mapa un elevado número de elementos, considera la posibilidad de [superponer imágenes en mosaico en el mapa](overlay-tiled-images.md). Para mostrar carreteras en el mapa, consulta [Mostrar rutas e indicaciones](routes-and-directions.md).

## Agregar un MapIcon


Para mostrar una imagen (como, por ejemplo, un marcador) con texto opcional en el mapa, usa la clase [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077). Igualmente, para aceptar la imagen predeterminada o proporcionar una imagen personalizada, puedes usar la propiedad [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078). En la siguiente imagen se muestra la imagen predeterminada de un elemento **MapIcon** que consta de la propiedad [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088), la cual muestra ningún valor especificado, un título corto, un título largo y un título muy largo.

![ejemplo de un elemento MapIcon con títulos de diferentes longitudes.](images/mapctrl-mapicons.png)

En el ejemplo siguiente se muestra un mapa de la ciudad de Seattle, el cual contiene un elemento [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) en la imagen predeterminada y un título opcional para indicar la ubicación de la torre Space Needle. También se centra el mapa en el icono y se acerca el zoom. Para obtener información general sobre cómo usar el control de mapa, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

```csharp
      private void displayPOIButton_Click(object sender, RoutedEventArgs e)
      {
         // Specify a known location.
         BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
         Geopoint snPoint = new Geopoint(snPosition);

         // Create a MapIcon.
         MapIcon mapIcon1 = new MapIcon();
         mapIcon1.Location = snPoint;
         mapIcon1.NormalizedAnchorPoint = new Point(0.5, 1.0);
         mapIcon1.Title = "Space Needle";
         mapIcon1.ZIndex = 0;

         // Add the MapIcon to the map.
         MapControl1.MapElements.Add(mapIcon1);

         // Center the map over the POI.
         MapControl1.Center = snPoint;
         MapControl1.ZoomLevel = 14;
      }
```

Este ejemplo muestra el siguiente punto de interés en el mapa (la imagen predeterminada en el centro).

![mapa con MapIcon](images/displaypoidefault.png)

La línea de código siguiente te permite mostrar el elemento [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) con una imagen personalizada guardada en la carpeta Assets del proyecto. La propiedad [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) del elemento **MapIcon** espera un valor de tipo [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Para este tipo es necesaria una declaración **using** para el espacio de nombres [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

**Sugerencia** Si usas la misma imagen para varios iconos del mapa, declara la clase [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) en el nivel de página o aplicación para mejorar el rendimiento.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Ten en cuenta lo siguiente si quieres trabajar con la clase [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077):

-   La propiedad [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) admite un tamaño máximo de imagen de 2048×2048 píxeles.
-   De manera predeterminada, no se garantiza que se muestre la imagen del icono del mapa. Esto es debido a que se ocultará si bloquea otros elementos o etiquetas del mapa. Para mantener la imagen siempre visible, establece la propiedad [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) del icono de mapa en [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314).
-   Asimismo, tampoco se garantiza que se muestre el elemento [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) de la clase [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077). Si no ves el texto, disminuye su tamaño; para ello tienes que reducir el valor de la propiedad [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).
-   Si muestras la imagen de un elemento [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) que señala a una ubicación específica del mapa (por ejemplo, un marcador o una flecha), puedes establecer el valor de la propiedad [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) en la ubicación aproximada del puntero de la imagen. Si dejas el valor de **NormalizedAnchorPoint** como el valor predeterminado (0, 0), que representa la esquina superior izquierda de la imagen, puede que los cambios realizados en la propiedad [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) del mapa hagan que la imagen apunte a otra ubicación.

## Agregar un elemento MapPolygon


Usa la clase [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) para mostrar una forma multipunto en el mapa. En el siguiente ejemplo, obtenido de una [muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), se muestra en el mapa un cuadro rojo con el borde azul.

```csharp
private void mapPolygonAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolygon mapPolygon = new MapPolygon();
   mapPolygon.Path = new Geopath(new List<BasicGeoposition>() {
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },

   });
           
   mapPolygon.ZIndex = 1;
   mapPolygon.FillColor = Colors.Red;
   mapPolygon.StrokeColor = Colors.Blue;
   mapPolygon.StrokeThickness = 3;
   mapPolygon.StrokeDashed = false;
   myMap.MapElements.Add(mapPolygon);
}
```

## Agregar un elemento MapPolyline


Usa la clase [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) para mostrar una línea en el mapa. En el siguiente ejemplo, obtenido de una [muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), se muestra una línea discontinua en el mapa.

```csharp
private void mapPolylineAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolyline mapPolyline = new MapPolyline();
   mapPolyline.Path = new Geopath(new List<BasicGeoposition>() {                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
   });
              
   mapPolyline.StrokeColor = Colors.Black;
   mapPolyline.StrokeThickness = 3;
   mapPolyline.StrokeDashed = true;
   myMap.MapElements.Add(mapPolyline);
}
```

## Agregar XAML


Visualiza elementos de la interfaz de usuario personalizados en el mapa mediante XAML. Coloca XAML en el mapa especificando la ubicación y el punto de anclaje normalizado del XAML.

-   Llama a [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) para establecer la ubicación del mapa donde debe colocarse el código XAML.
-   Llama a [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) para establecer la ubicación relativa en el código XAML que corresponde a la ubicación especificada.

En el ejemplo siguiente se muestra un mapa de la ciudad de Seattle y se agrega un control [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) de XAML para indicar la ubicación de la torre Space Needle. También se centra el mapa en el área y se acerca el zoom. Para obtener información general sobre cómo usar el control de mapa, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
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

![](images/displaypoixaml.png)

Los siguientes ejemplos muestran cómo agregar elementos de interfaz de usuario de XAML directamente en el marcado XAML de la página mediante el enlace de datos. Igual que sucede con otros elementos XAML que muestran contenido, [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) es la propiedad de contenido predeterminada de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), y no es necesario especificarla expresamente en el marcado XAML.

En este ejemplo se indica cómo puedes mostrar dos controles XAML como objetos secundarios implícitos de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
</maps:MapControl>
```

En este ejemplo se indica cómo puedes mostrar dos controles XAML incluidos dentro de la clase [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094).

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

En este ejemplo se muestra una colección de elementos XAML enlazados a la clase [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094).

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{Binding StateOverlays}">
    <maps:MapItemsControl.ItemTemplate>
      <DataTemplate>
        <StackPanel Background="Black" Tapped="Overlay_Tapped">
          <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Name}" maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
        </StackPanel>
      </DataTemplate>
    </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

## Temas relacionados

* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP traffic app sample (Ejemplo de aplicación de tráfico de UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)





<!--HONumber=Aug16_HO3-->


