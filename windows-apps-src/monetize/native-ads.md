---
description: Obtenga información sobre cómo usar anuncios nativos, un formato de anuncio basado en componentes donde cada parte del anuncio se entrega a la aplicación como un elemento individual.
title: Anuncios nativos
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Advertising, ad control, ad nativo
ms.localizationpriority: medium
ms.openlocfilehash: 1d3f26049350ce2cc2fc2c16f85989e9ebd5633c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171379"
---
# <a name="native-ads"></a>Anuncios nativos

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Un anuncio nativo es un formato de anuncio basado en componentes, donde cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual. Puede integrar estos elementos en la aplicación con sus propias fuentes, colores, animaciones y otros componentes de la interfaz de usuario para unir una experiencia de usuario discreta que se ajuste a la apariencia y el funcionamiento de la aplicación, a la vez que obtiene un alto rendimiento de los anuncios.

En el caso de los anunciantes, los anuncios nativos proporcionan ubicaciones de alto rendimiento, ya que la experiencia de ad está estrechamente integrada en la aplicación y, por lo tanto, los usuarios suelen interactuar más con estos tipos de anuncios.

> [!NOTE]
> Actualmente, los anuncios nativos solo se admiten para las aplicaciones para UWP basadas en XAML para Windows 10. La compatibilidad con aplicaciones UWP escritas con HTML y JavaScript está prevista para una versión futura del SDK de Microsoft Advertising.

## <a name="prerequisites"></a>Requisitos previos

* Instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) con visual Studio 2015 o una versión posterior de Visual Studio. Para obtener instrucciones de instalación, consulte [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Integrar un ad nativo en la aplicación

Siga estas instrucciones para integrar un ad nativo en la aplicación y confirmar que la implementación de ad nativo muestra un anuncio de prueba.

1. En Visual Studio, abre el proyecto o crea uno nuevo.
    > [!NOTE]
    > Si utiliza un proyecto existente, abra el archivo package. appxmanifest en el proyecto y asegúrese de que la opción **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios en directo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el proyecto tiene como destino una **CPU**, no podrá agregar una referencia al SDK de Microsoft Advertising correctamente en los pasos siguientes. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agregue una referencia al SDK de Microsoft Advertising en el proyecto:

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

4. En el archivo de código adecuado de la aplicación (por ejemplo, en MainPage.xaml.cs o un archivo de código para alguna otra página), agregue las siguientes referencias de espacio de nombres.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  En una ubicación adecuada de la aplicación (por ejemplo, en ```MainPage``` o en otra página), declare un objeto [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) y varios campos de cadena que representen el identificador de la aplicación y el identificador de la unidad de ad de su ad nativo. En el ejemplo de código siguiente se asignan los `myAppId` `myAdUnitId` campos y a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) de los anuncios nativos.
    > [!NOTE]
    > Cada **NativeAdsManagerV2** tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control de ad nativo, y cada unidad de ad está formada por un identificador de *unidad de ad* y un identificador de *aplicación*. En estos pasos, asignará el identificador de unidad de ad de prueba y los valores de ID. de aplicación al control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debe [reemplazar estos valores de prueba con valores dinámicos](#release) del centro de Partners.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), cree una instancia del objeto **NativeAdsManagerV2** y conecte los controladores de eventos para los eventos **AdReady** y **ErrorOccurred** del objeto.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  Cuando esté listo para mostrar un ad nativo, llame al método **RequestAd** para capturar un anuncio.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  Cuando un ad nativo está listo para su aplicación, se llama al controlador de eventos [AdReady](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) y se pasa un objeto [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) que representa el ad nativo al parámetro *e* . Use las propiedades **NativeAdV2** para obtener cada elemento del ad nativo y mostrar estos elementos en la página. Asegúrese de llamar también al método **RegisterAdContainer** para registrar el elemento de la interfaz de usuario que actúa como contenedor para el ad nativo; Esto es necesario para realizar un seguimiento adecuado de las impresiones de anuncios y hacer clic en ellos.
    > [!NOTE]
    > Algunos elementos de ad nativo son obligatorios y deben mostrarse siempre en la aplicación. Para obtener más información, vea nuestras [instrucciones para anuncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Por ejemplo, suponga que la aplicación contiene un ```MainPage``` (o alguna otra página) con el siguiente **StackPanel**. Este elemento **StackPanel** contiene una serie de controles que muestran distintos elementos de un ad nativo, incluido el título, la descripción, las imágenes, *patrocinadas por* texto y un botón que mostrará la *llamada al* texto de la acción.

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    En el ejemplo de código siguiente se muestra un controlador de eventos **AdReady** que muestra cada elemento de ad nativo en los controles de **StackPanel** y, a continuación, llama al método **RegisterAdContainer** para registrar el **StackPanel**. En este código se supone que se ejecuta desde el archivo de código subyacente de la página que contiene el **StackPanel**.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  Defina un controlador de eventos para el evento **ErrorOccurred** con el fin de controlar los errores relacionados con ad nativo. En el ejemplo siguiente se escribe información de error en la ventana de **salida** de Visual Studio durante las pruebas.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  Compile y ejecute la aplicación para verla con un anuncio de prueba.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publique su aplicación con anuncios en vivo

Después de confirmar que la implementación de ad nativo muestra correctamente un anuncio de prueba, siga estas instrucciones para configurar la aplicación para mostrar anuncios reales y enviar la aplicación actualizada a la tienda.

1.  Asegúrese de que la implementación de ad nativo siga nuestras [instrucciones para anuncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

2.  En el centro de Partners, vaya a la página [anuncios en la aplicación](../publish/in-app-ads.md) y [cree una unidad de anuncio](set-up-ad-units-in-your-app.md#live-ad-units). Para el tipo de unidad de ad, especifique **nativo**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.
    > [!NOTE]
    > Los valores de ID. de aplicación para las unidades de anuncios de prueba y las unidades de anuncio de UWP en directo tienen distintos formatos. Los valores de ID. de aplicación de prueba son GUID. Cuando se crea una unidad de ad de UWP activa en el centro de Partners, el valor de identificador de la aplicación de la unidad de anuncio coincide siempre con el identificador de almacén de la aplicación (un valor de identificador de almacén de ejemplo es similar a 9NBLGGH4R315).

3. Opcionalmente, puede habilitar la mediación de anuncios para ad nativo configurando los valores en la sección [configuración de mediación](../publish/in-app-ads.md#mediation) en la página [ADS en la aplicación](../publish/in-app-ads.md) . La mediación de anuncios permite maximizar las funcionalidades de promoción de ingresos y de las aplicaciones de anuncios mediante la visualización de anuncios de varias redes de anuncios.

4.  En el código, reemplace los valores de unidad de ad de prueba (es decir, los parámetros *ApplicationID* y *AdUnitId* del constructor [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) ) con los valores activos generados en el centro de Partners.

5.  [Envíe la aplicación](../publish/app-submissions.md) a la tienda mediante el centro de Partners.

6.  Revise los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de Partners.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Administrar unidades de anuncios para varios anuncios nativos en la aplicación

Puede usar varias ubicaciones nativas de AD en una sola aplicación. En este escenario, se recomienda asignar una unidad de anuncio diferente a cada ubicación nativa de ad. El uso de diferentes unidades de anuncios para ad nativo le permite [configurar por separado los valores de mediación](../publish/in-app-ads.md#mediation) y obtener [datos de informes](../publish/advertising-performance-report.md) discretos para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que sirven a su aplicación.

> [!IMPORTANT]
> Puede usar cada unidad de anuncio en una sola aplicación. Si usa una unidad de anuncio en más de una aplicación, los anuncios no se servirán para esa unidad de ad.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [Anuncios en aplicaciones](../publish/in-app-ads.md)
* [Configurar unidades de anuncio para la aplicación](set-up-ad-units-in-your-app.md)