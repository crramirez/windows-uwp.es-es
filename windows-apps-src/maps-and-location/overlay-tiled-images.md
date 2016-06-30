---
author: msatranjr
title: "Superponer imágenes en mosaico en un mapa"
description: "Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante orígenes de icono. Usa orígenes de icono para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo."
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 71d044eb19e71786da39ca71d4f4fbd2d87645be

---

# Superponer imágenes en mosaico en un mapa


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante los orígenes de icono. Usa orígenes de icono para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo.

**Sugerencia** Para obtener más información sobre el uso de mapas en la aplicación, descarga la muestra siguiente de [Windows-universal-samples repo](http://go.microsoft.com/fwlink/p/?LinkId=619979) (repositorio de muestras universales de Windows) que encontrarás en GitHub.

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Información general de la imagen en mosaico


Los servicios de mapa, como Nokia Maps y Mapas de Bing, dividen los mapas en iconos cuadrados para una rápida recuperación y presentación. Estos iconos tienen un tamaño de 256 x 256 píxeles y se representan previamente con varios niveles de detalle. Muchos de los servicios de terceros también proporcionan datos basados en mapas que están divididos en iconos. Usa los orígenes de icono para recuperar iconos de terceros o para crear tus propios iconos personalizados y superponerlos en el mapa que se muestra en [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

**Importante**  
Al usar la opción de orígenes de icono, no tienes que escribir código para solicitar o colocar los iconos individuales. La clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) se encarga de solicitar los iconos a medida que los necesita. Cada solicitud especifica las coordenadas X e Y, y el nivel de zoom para el icono individual. Solo debes indicar el formato del URI o el nombre de archivo que se debe usar para recuperar los iconos en la propiedad **UriFormatString**. Es decir, se insertan los parámetros reemplazables en el URI base o en el nombre de archivo para indicar dónde se deben pasar las coordenadas X e Y, y el nivel de zoom para cada icono.

Este es un ejemplo de la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) de una clase [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) que muestra los parámetros reemplazables para las coordenadas X e Y, y el nivel de zoom.

``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

 

(Las coordenadas X e Y representan la ubicación del icono individual en el mapa del mundo con el nivel de detalle especificado. El sistema de numeración del icono empieza desde {0, 0} en la esquina superior izquierda del mapa. Por ejemplo, el icono en {1, 2} es la segunda columna de la tercera fila de la cuadrícula de iconos).

Para obtener más información acerca del sistema de iconos que usan los servicios de asignación, consulta [Bing Maps Tile System (Sistema de iconos de Mapas de Bing)](http://go.microsoft.com/fwlink/p/?LinkId=626692).

### Superponer los iconos de un origen de iconos

Superpón imágenes en mosaico desde un origen de iconos en un mapa mediante [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141).

1.  Crea una instancia de una de las tres clases de orígenes de datos de icono que se heredan de la clase [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141).

    -   [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)
    -   [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)
    -   [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983)

    Configura **UriFormatString** para solicitar los iconos mediante la inserción de parámetros reemplazables en el URI base o el nombre de archivo.

    El siguiente ejemplo crea una instancia de una clase [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986). Este ejemplo especifica el valor de [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) en el constructor de la clase **HttpMapTileDataSource**.

    ```cs
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  Crea una instancia y configura una clase [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144). Especifica la clase [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141) que has configurado en el paso anterior, como una propiedad [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149) de **MapTileSource**.

    En el siguiente ejemplo se especifica la propiedad [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149) en el constructor de la clase [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).

    ```cs
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    Puedes restringir las condiciones en que los iconos se muestran mediante las propiedades de la clase [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).

    -   Solo muestra los iconos dentro de un área geográfica concreta al proporcionar un valor para la propiedad [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147).
    -   Solo muestra los iconos en determinados niveles de detalle al proporcionar un valor para la propiedad [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171).

    Opcionalmente, configura otras propiedades de la clase [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) que afectan a la carga o a la visualización de los iconos, como [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157), [**AllowOverstretch**](https://msdn.microsoft.com/library/windows/apps/dn637145), [**IsRetryEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637153) y [**IsTransparencyEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637155).

3.  Agrega la clase [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) a la colección [**TileSources**](https://msdn.microsoft.com/library/windows/apps/dn637053) de [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

    ```cs
         MapControl1.TileSources.Add(tileSource);
    ```

## Superponer iconos desde un servicio web


Superpón imágenes en mosaico recuperadas desde un servicio web mediante la clase [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986).

1.  Crea una instancia de una clase [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986).
2.  Especifica el formato del URI que el servicio web espera, como el valor de la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992). Para crear este valor, inserta los parámetros reemplazables en el URI base. Por ejemplo, en la siguiente muestra de código, el valor de **UriFormatString** es:

    ``` syntax
        http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    El servicio web debe admitir un URI que contenga los parámetros reemplazables {x}, {y}, y {zoomlevel}. La mayoría de los servicios web (Nokia, Bing o Google, por ejemplo) admiten URI con este formato. Si el servicio web requiere argumentos adicionales que no estén disponibles con la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992), deberás crear un URI personalizado. Crea y devuelve un URI personalizado mediante el control de los eventos [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn636993). Para obtener más información, consulta la sección [Proporcionar un URI personalizado](#customuri) que figura más adelante en este tema.

3.  A continuación, sigue los pasos restantes que se han descrito anteriormente en [Información general de la imagen en mosaico](#tileintro).

El siguiente ejemplo, se superponen los iconos de un servicio web ficticio en un mapa de Norteamérica. El valor de la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) se especifica en el constructor de la clase [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986). En este ejemplo, solo se muestran los iconos dentro de los límites geográficos especificados en la propiedad opcional [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147).

> [!div class="tabbedCodeSnippets"]
```csharp
        private void AddHttpMapTileSource()
        {
            // Create the bounding box in which the tiles are displayed.
            // This example represents North America.
            BasicGeoposition northWestCorner =
                new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
            BasicGeoposition southEastCorner =
                new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
            GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

            // Create an HTTP data source.
            // This example retrieves tiles from a fictitious web service.
            HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
                "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

            // Optionally, add custom HTTP headers if the web service requires them.
            dataSource.AdditionalRequestHeaders.Add("header name", "header value");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.Bounds = boundingBox;
            MapControl1.TileSources.Add(tileSource);
        }
```
```cpp
void MainPage::AddHttpMapTileSource()
{
       BasicGeoposition northWest = { 48.38544, -124.667360 };
       BasicGeoposition southEast = { 25.26954, -80.30182 };
       GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

       auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
             "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

       dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

       auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
       tileSource->Bounds = boundingBox;

       this->MapControl1->TileSources->Append(tileSource);
}
```

## Superponer iconos desde el almacenamiento local


Superpón imágenes en mosaico almacenadas como archivos en el almacenamiento local mediante la clase [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994). Normalmente, estos archivos se empaquetan y distribuyen con la aplicación.

1.  Crea una instancia de una clase [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994).
2.  A continuación, especifica el formato de los nombres de archivo en el valor de la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998). Para crear este valor, inserta los parámetros reemplazables en el nombre de archivo base. Por ejemplo, en la siguiente muestra de código, el valor de [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) es:

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    Si el formato de los nombres de archivo requiere argumentos adicionales que no están disponibles en la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998), deberás crear un URI personalizado. Crea y devuelve un URI personalizado mediante el control del evento [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn637001). Para obtener más información, consulta la sección [Proporcionar un URI personalizado](#customuri) que figura más adelante en este tema.

3.  A continuación, sigue los pasos restantes que se han descrito anteriormente en [Información general de la imagen en mosaico](#tileintro).

Puedes usar los siguientes protocolos y ubicaciones para cargar mosaicos desde un almacenamiento local:

| URI | Más información |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | Apunta a la raíz de la carpeta de instalación de la aplicación. |
|  | Esta es la ubicación a la que hace referencia la propiedad [Package.InstalledLocation](https://msdn.microsoft.com/library/windows/apps/br224681). |
| ms-appdata:///local/ | Apunta a la raíz del almacenamiento local de la aplicación. |
|  | Esta es la ubicación a la que hace referencia la propiedad [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621). |
| ms-appdata:///temp | Apunta a la carpeta temporal de la aplicación. |
|  | Esta es la ubicación a la que hace referencia la propiedad [ApplicationData.TemporaryFolder](https://msdn.microsoft.com/library/windows/apps/br241629). |

 

El ejemplo siguiente carga iconos almacenados como archivos en la carpeta de instalación de la aplicación mediante el protocolo `ms-appx:///`. El valor de [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) se especifica en el constructor de la clase [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994). En este ejemplo, los iconos solo se muestran cuando el nivel de zoom del mapa está dentro del intervalo que especifica la propiedad opcional [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171).

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

## Proporcionar un URI personalizado


Si los parámetros reemplazables disponibles en la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) de la clase [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) o la propiedad [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) de la clase [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994) no son suficientes para recuperar los iconos, deberás crear un URI personalizado. Crea y devuelve un URI personalizado al proporcionar un controlador personalizado para el evento **UriRequested**. El evento **UriRequested** se genera para cada icono individual.

1.  En el controlador personalizado del evento **UriRequested**, se combinan los argumentos personalizados requeridos con las propiedades [**X**](https://msdn.microsoft.com/library/windows/apps/dn610743), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn610744) y [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn610745) de la clase [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177), para crear el URI personalizado.
2.  Devuelve el URI personalizado en la propiedad [**Uri**](https://msdn.microsoft.com/library/windows/apps/dn610748) de la clase [**MapTileUriRequest**](https://msdn.microsoft.com/library/windows/apps/dn637173), que está incluido en la propiedad [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637179) de la clase [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177).

En el siguiente ejemplo se muestra cómo proporcionar un URI personalizado mediante la creación de un controlador personalizado para el evento **UriRequested**. También se muestra cómo implementar el modelo de aplazamiento si tienes que hacer algo asincrónicamente para crear el URI personalizado.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## Superponer iconos desde un origen personalizado


Superpón iconos personalizados mediante la clase [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983). Crea iconos mediante programación en la memoria sobre la marcha o escribe tu propio código para cargar los iconos existentes de otro origen.

Para crear o cargar iconos personalizados, debes proporcionar un controlador personalizado del evento [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984). El evento **BitmapRequested** se genera para cada icono individual.

1.  En el controlador personalizado del evento [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984), combina los argumentos personalizados requeridos con las propiedades [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) y [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) de la clase [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132), para crear o recuperar un icono personalizado.
2.  Devuelve el icono personalizado en la propiedad [**PixelData**](https://msdn.microsoft.com/library/windows/apps/dn637140) de la clase [**MapTileBitmapRequest**](https://msdn.microsoft.com/library/windows/apps/dn637128), que está incluido en la propiedad [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637134) de la clase [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132). Ten en cuenta que la propiedad **PixelData** es del tipo [**IRandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701664).

En el siguiente ejemplo se muestra cómo proporcionar iconos personalizados mediante la creación de un controlador personalizado para el evento **BitmapRequested**. Este ejemplo crea iconos rojos idénticos que son parcialmente opacos. Asimismo, el ejemplo pasa por alto las propiedades [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) y [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) de la clase [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132). Aunque no se trata de un ejemplo real, muestra igualmente cómo crear sobre la marcha iconos personalizados en la memoria. En el ejemplo también se muestra cómo implementar el modelo de aplazamiento si tienes que hacer algo asincrónicamente para crear los iconos personalizados.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessSteram::get()
{
       int pixelHeight = 256;
       int pixelWidth = 256;
       int bpp = 4;
 
       Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);
       
       for (int y = 0; y < pixelHeight; y++)
       {
              for (int x = 0; x < pixelWidth; x++)
              {
                     int pixelIndex = y * pixelWidth + x;
                     int byteIndex = pixelIndex * bpp;
 
                     // Set the current pixel bytes.
                     bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
                     bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
                     bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
                     bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
              }
       }
 
       // Create RandomAccessStream from byte array.
       InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
       IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
       DataWriter^ writer = ref new DataWriter(outputStream);
       writer->WriteBytes(bytes);
 
       create_task(writer->StoreAsync()).then([writer](unsigned int)
       {
              create_task(writer->FlushAsync());
       });
 
       return randomAccessStream;
}
```

## Reemplazo del mapa predeterminado


Reemplazar todo el mapa con iconos personalizados o de terceros:

-   Especifica [**MapTileLayer**](https://msdn.microsoft.com/library/windows/apps/dn637143).**BackgroundReplacement** como el valor de la propiedad [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157) de la clase [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).
-   Especifica [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).**None** como el valor de la propiedad [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) de la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

## Temas relacionados

* [Bing Maps Developer Center (Centro para desarrolladores de Mapas de Bing)](https://www.bingmapsportal.com/)
* [UWP map sample (Muestra de mapa de UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)




<!--HONumber=Jun16_HO4-->


