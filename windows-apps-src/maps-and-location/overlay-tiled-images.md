---
title: Superponer imágenes en mosaico en un mapa
description: Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante orígenes de mosaico. Usa orígenes de mosaico para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo.
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, mapa, ubicación, imágenes, superposición
ms.localizationpriority: medium
ms.openlocfilehash: 8d4a8e3f8a8566fbaae64d44fe876808f094bdd5
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297633"
---
# <a name="overlay-tiled-images-on-a-map"></a>Superponer imágenes en mosaico en un mapa

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante orígenes de mosaico. Usa orígenes de mosaico para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo.

**Sugerencia** Para obtener más información sobre el uso de mapas en su aplicación, descargue el [ejemplo de mapa de plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) en github.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>Información general de la imagen en mosaico

Los servicios de mapa, como Nokia Maps y Mapas de Bing, dividen los mapas en iconos cuadrados para una rápida recuperación y presentación. Estos iconos tienen un tamaño de 256 x 256 píxeles y se representan previamente con varios niveles de detalle. Muchos de los servicios de terceros también proporcionan datos basados en mapas que están divididos en iconos. Usa los orígenes de icono para recuperar iconos de terceros o para crear tus propios iconos personalizados y superponerlos en el mapa que se muestra en [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

**Importante**    Cuando se usan orígenes de mosaicos, no es necesario escribir código para solicitar o colocar mosaicos individuales. La clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) se encarga de solicitar los iconos a medida que los necesita. Cada solicitud especifica las coordenadas X e Y, y el nivel de zoom para el icono individual. Solo debes indicar el formato del URI o el nombre de archivo que se debe usar para recuperar los iconos en la propiedad **UriFormatString**. Es decir, se insertan los parámetros reemplazables en el URI base o en el nombre de archivo para indicar dónde se deben pasar las coordenadas X e Y, y el nivel de zoom para cada icono.

Este es un ejemplo de la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) de una clase [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) que muestra los parámetros reemplazables para las coordenadas X e Y, y el nivel de zoom.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

(Las coordenadas X e Y representan la ubicación del icono individual en el mapa del mundo con el nivel de detalle especificado. El sistema de numeración del icono empieza desde {0, 0} en la esquina superior izquierda del mapa. Por ejemplo, el icono en {1, 2} es la segunda columna de la tercera fila de la cuadrícula de iconos).

Para obtener más información acerca del sistema de iconos que usan los servicios de asignación, consulta [Bing Maps Tile System (Sistema de iconos de Mapas de Bing)](/bingmaps/articles/bing-maps-tile-system).

### <a name="overlay-tiles-from-a-tile-source"></a>Superponer los iconos de un origen de iconos

Superpón imágenes en mosaico desde un origen de iconos en un mapa mediante [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource).

1.  Crea una instancia de una de las tres clases de orígenes de datos de icono que se heredan de la clase [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource).

    -   [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)
    -   [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)
    -   [**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)

    Configura **UriFormatString** para solicitar los iconos mediante la inserción de parámetros reemplazables en el URI base o el nombre de archivo.

    El siguiente ejemplo crea una instancia de una clase [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource). Este ejemplo especifica el valor de [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) en el constructor de la clase **HttpMapTileDataSource**.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  Crea una instancia y configura una clase [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource). Especifica la clase [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) que has configurado en el paso anterior, como una propiedad [**DataSource**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) de **MapTileSource**.

    En el siguiente ejemplo se especifica la propiedad [**DataSource**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) en el constructor de la clase [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource).

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    Puedes restringir las condiciones en que los iconos se muestran mediante las propiedades de la clase [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource).

    -   Solo muestra los iconos dentro de un área geográfica concreta al proporcionar un valor para la propiedad [**Bounds**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds).
    -   Solo muestra los iconos en determinados niveles de detalle al proporcionar un valor para la propiedad [**ZoomLevelRange**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange).

    Opcionalmente, configura otras propiedades de la clase [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) que afectan a la carga o a la visualización de los iconos, como [**Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer), [**AllowOverstretch**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.allowoverstretch), [**IsRetryEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.isretryenabled) y [**IsTransparencyEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.istransparencyenabled).

3.  Agrega la clase [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) a la colección [**TileSources**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.tilesources) de [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>Superponer iconos desde un servicio web


Superpón imágenes en mosaico recuperadas desde un servicio web mediante la clase [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource).

1.  Crea una instancia de una clase [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource).
2.  Especifica el formato del URI que el servicio web espera, como el valor de la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring). Para crear este valor, inserta los parámetros reemplazables en el URI base. Por ejemplo, en la siguiente muestra de código, el valor de **UriFormatString** es:

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    El servicio web debe admitir un URI que contenga los parámetros reemplazables {x}, {y}, y {zoomlevel}. La mayoría de los servicios web (Nokia, Bing o Google, por ejemplo) admiten URI con este formato. Si el servicio web requiere argumentos adicionales que no estén disponibles con la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring), deberás crear un URI personalizado. Crea y devuelve un URI personalizado mediante el control de los eventos [**UriRequested**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.urirequested). Para obtener más información, consulta la sección [Proporcionar un URI personalizado](#customuri) que figura más adelante en este tema.

3.  A continuación, sigue los pasos restantes que se han descrito anteriormente en [Información general de la imagen en mosaico](#tileintro).

El siguiente ejemplo, se superponen los iconos de un servicio web ficticio en un mapa de Norteamérica. El valor de la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) se especifica en el constructor de la clase [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource). En este ejemplo, solo se muestran los iconos dentro de los límites geográficos especificados en la propiedad opcional [**Bounds**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds).

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

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
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

## <a name="overlay-tiles-from-local-storage"></a>Superponer iconos desde el almacenamiento local


Superpón imágenes en mosaico almacenadas como archivos en el almacenamiento local mediante la clase [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource). Normalmente, estos archivos se empaquetan y distribuyen con la aplicación.

1.  Crea una instancia de una clase [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource).
2.  A continuación, especifica el formato de los nombres de archivo en el valor de la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring). Para crear este valor, inserta los parámetros reemplazables en el nombre de archivo base. Por ejemplo, en la siguiente muestra de código, el valor de [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) es:

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    Si el formato de los nombres de archivo requiere argumentos adicionales que no están disponibles en la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring), deberás crear un URI personalizado. Crea y devuelve un URI personalizado mediante el control de los eventos [**UriRequested**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.urirequested). Para obtener más información, consulta la sección [Proporcionar un URI personalizado](#customuri) que figura más adelante en este tema.

3.  A continuación, sigue los pasos restantes que se han descrito anteriormente en [Información general de la imagen en mosaico](#tileintro).

Puedes usar los siguientes protocolos y ubicaciones para cargar mosaicos desde un almacenamiento local:

| Identificador URI | Más información |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | Apunta a la raíz de la carpeta de instalación de la aplicación. |
|  | Esta es la ubicación a la que hace referencia la propiedad [Package.InstalledLocation](/uwp/api/windows.applicationmodel.package.installedlocation). |
| ms-appdata:///local/ | Apunta a la raíz del almacenamiento local de la aplicación. |
|  | Esta es la ubicación a la que hace referencia la propiedad [ApplicationData.LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder). |
| ms-appdata:///temp | Apunta a la carpeta temporal de la aplicación. |
|  | Esta es la ubicación a la que hace referencia la propiedad [ApplicationData.TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder). |

 

El ejemplo siguiente carga iconos almacenados como archivos en la carpeta de instalación de la aplicación mediante el protocolo `ms-appx:///`. El valor de [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) se especifica en el constructor de la clase [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource). En este ejemplo, los iconos solo se muestran cuando el nivel de zoom del mapa está dentro del intervalo que especifica la propiedad opcional [**ZoomLevelRange**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange).

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

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>Proporcionar un URI personalizado

Si los parámetros reemplazables disponibles en la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) de la clase [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) o la propiedad [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) de la clase [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) no son suficientes para recuperar los iconos, deberás crear un URI personalizado. Crea y devuelve un URI personalizado al proporcionar un controlador personalizado para el evento **UriRequested**. El evento **UriRequested** se genera para cada icono individual.

1.  En el controlador personalizado del evento **UriRequested**, se combinan los argumentos personalizados requeridos con las propiedades [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.y) y [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.zoomlevel) de la clase [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs), para crear el URI personalizado.
2.  Devuelve el URI personalizado en la propiedad [**Uri**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequest.uri) de la clase [**MapTileUriRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequest), que está incluido en la propiedad [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.request) de la clase [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs).

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

## <a name="overlay-tiles-from-a-custom-source"></a>Superponer iconos desde un origen personalizado

Superpón iconos personalizados mediante la clase [**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource). Crea iconos mediante programación en la memoria sobre la marcha o escribe tu propio código para cargar los iconos existentes de otro origen.

Para crear o cargar iconos personalizados, debes proporcionar un controlador personalizado del evento [**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested). El evento **BitmapRequested** se genera para cada icono individual.

1.  En el controlador personalizado del evento [**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested), combina los argumentos personalizados requeridos con las propiedades [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) y [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) de la clase [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs), para crear o recuperar un icono personalizado.
2.  Devuelve el icono personalizado en la propiedad [**PixelData**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequest.pixeldata) de la clase [**MapTileBitmapRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequest), que está incluido en la propiedad [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.request) de la clase [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs). Ten en cuenta que la propiedad **PixelData** es del tipo [**IRandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.IRandomAccessStreamReference).

En el siguiente ejemplo se muestra cómo proporcionar iconos personalizados mediante la creación de un controlador personalizado para el evento **BitmapRequested**. Este ejemplo crea iconos rojos idénticos que son parcialmente opacos. Asimismo, el ejemplo pasa por alto las propiedades [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) y [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) de la clase [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs). Aunque no se trata de un ejemplo real, muestra igualmente cómo crear sobre la marcha iconos personalizados en la memoria. En el ejemplo también se muestra cómo implementar el modelo de aplazamiento si tienes que hacer algo asincrónicamente para crear los iconos personalizados.

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

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
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

## <a name="replace-the-default-map"></a>Reemplazo del mapa predeterminado

Reemplazar todo el mapa con iconos personalizados o de terceros:

-   Especifica [**MapTileLayer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileLayer).**BackgroundReplacement** como el valor de la propiedad [**Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer) de la clase [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource).
-   Especifica [**MapStyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle).**None** como el valor de la propiedad [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) de la clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

## <a name="related-topics"></a>Temas relacionados

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Directrices de diseño para mapas](./display-maps.md)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechar los mapas y la ubicación entre teléfonos, tabletas y equipos en tus aplicaciones Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
