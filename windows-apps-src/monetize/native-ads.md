---
author: Xansky
description: Obtén información sobre cómo agregar anuncios nativos a tu aplicación para UWP.
title: Anuncios nativos
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, control de anuncios, anuncio nativo
ms.localizationpriority: medium
ms.openlocfilehash: 4a529cc360d6a357cf4ff6aa36370c4339b37ed6
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614308"
---
# <a name="native-ads"></a>Anuncios nativos

Un anuncio nativo es un formato de anuncio basado en componentes, donde cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual. Puedes integrar estos elementos en la aplicación con tus propias fuentes, colores, animaciones y otros componentes de interfaz de usuario para unir una experiencia de usuario discreta que se ajuste a la apariencia de la aplicación a la vez que también se obtiene un alto rendimiento de los anuncios.

Para los anunciantes, los anuncios nativos proporcionan ubicaciones de alto rendimiento, porque la experiencia de anuncios está estrechamente integrada en la aplicación y, por lo tanto, los usuarios tienden a interactuar más con estos tipos de anuncios.

> [!NOTE]
> Los anuncios nativos solo se admiten actualmente para aplicaciones para UWP basadas en XAML para Windows 10. Está prevista la compatibilidad para aplicaciones para UWP escrita con HTML y JavaScript para una futura versión del SDK de Microsoft Advertising.

## <a name="prerequisites"></a>Requisitos previos

* Instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior de VisualStudio. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Integrar un anuncio nativo en la aplicación

Sigue estas instrucciones para integrar un anuncio nativo en la aplicación y confirma que la implementación de anuncio nativo muestra un anuncio de prueba.

1. En Visual Studio, abre el proyecto o crea uno nuevo.
    > [!NOTE]
    > Si estás usando un proyecto existente, abre el archivo Package.appxmanifest en el proyecto y asegúrate de que la funcionalidad **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios dinámicos.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia al SDK de Microsoft Advertising correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega una referencia al SDK de Microsoft Advertising en el proyecto:

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y luego selecciona **Agregar referencia...**
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

4. En el archivo de código adecuado de la aplicación (por ejemplo, en MainPage.xaml.cs o un archivo de código para otra página), agrega las siguientes referencias de espacio de nombres.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  En una ubicación adecuada de tu aplicación (por ejemplo, en ```MainPage``` o en otra página), declara un objeto [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) y varios campos de cadenas que representen el id. de aplicación y el id. de unidad del anuncio nativo. En el siguiente ejemplo de código, se asignan los campos `myAppId` y `myAdUnitId` a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para anuncios nativos.
    > [!NOTE]
    > Cada **NativeAdsManagerV2** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control de anuncio nativo y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Store, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **NativeAdsManagerV2** y conecta controladores de eventos para los eventos **AdReady** y **ErrorOccurred** del objeto.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  Cuando estés listo para mostrar un anuncio nativo, llama al método **RequestAd** para capturar un anuncio.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  Cuando un anuncio nativo esté listo para tu aplicación, se llama al controlador de eventos [AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) y un objeto [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) que representa el anuncio nativo se pasa al parámetro *e*. Usa las propiedades **NativeAdV2** para obtener cada elemento del anuncio nativo y mostrar estos elementos en la página. Asegúrate de llamar también al método **RegisterAdContainer** para registrar el elemento de la interfaz de usuario que actúa como un contenedor para el anuncio nativo; esto es necesario para realizar un seguimiento adecuado de los clics y de las impresiones de anuncios.
    > [!NOTE]
    > Algunos de los elementos del anuncio nativo son necesarios y siempre se deben mostrar en la aplicación. Para más información, consulta nuestras [directrices para anuncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Por ejemplo, supongamos que tu aplicación contiene un ```MainPage``` (o alguna otra página) con el siguiente **StackPanel**. Este **StackPanel** contiene una serie de controles que muestran diferentes elementos de un anuncio nativo, como el título, la descripción, las imágenes, el texto de *patrocinado por* y un botón que mostrará el texto de la *llamada a la acción*.

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

    El siguiente ejemplo de código muestra un controlador de eventos **AdReady** que muestra cada elemento del anuncio nativo en los controles del **StackPanel** y, a continuación, llama al método **RegisterAdContainer** para registrar el **StackPanel**. Este código supone que se ejecuta desde el archivo de código subyacente para la página que contiene el **StackPanel**.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  Define un controlador de eventos para que el evento **ErrorOccurred** controle los errores relacionados con el anuncio nativo. En el ejemplo siguiente se escribe la información de error en la ventana **Salida** de Visual Studio durante las pruebas.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  Compila y ejecuta la aplicación para verla con un anuncio de prueba.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publicar tu aplicación con anuncios dinámicos

Después de confirmar que la implementación de anuncios nativo muestra correctamente un anuncio de prueba, sigue estas instrucciones para configurar la aplicación para que muestre anuncios reales y envía la aplicación actualizada a la Tienda.

1.  Asegúrate de que la implementación de anuncio nativo sigue nuestras [directrices para anuncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

2.  En el panel del Centro de desarrollo, ve a la página [Anuncios desde la aplicación](../publish/in-app-ads.md) y [crea una unidad de anuncios](set-up-ad-units-in-your-app.md#live-ad-units). Especifica el tipo de unidad de anuncio **Nativo**. Anota el id. de la unidad de anuncios y el id. de la aplicación.
    > [!NOTE]
    > Los valores de id. de la aplicación para unidades de anuncios de prueba y unidades de anuncios dinámicas de UWP tienen diferentes formatos. Los valores de id. de la aplicación de prueba son GUID. Al crear una unidad de anuncio dinámica para UWP en el panel de información, el valor de id. de la aplicación de la unidad de anuncios siempre coincide con el id. de Microsoft Store de la aplicación (un valor de id. de Microsoft Store de muestra se parece a 9NBLGGH4R315).

3. También tienes la opción de habilitar la mediación de anuncios para el anuncio nativo mediante la configuración de las opciones de la sección [Configuración de la mediación](../publish/in-app-ads.md#mediation) de la página [Anuncios desde la aplicación](../publish/in-app-ads.md). La mediación de anuncios te permite maximizar las funcionalidades de ingresos por anuncios y de promoción de la aplicación mostrando anuncios de múltiples redes de anuncio.

4.  En el código, reemplaza los valores de unidades de anuncio de prueba (es decir, los parámetros *applicationId* y *adUnitId* del constructor [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor)) por los valores dinámicos generados en el Centro de desarrollo.

5.  [Envía la aplicación](../publish/app-submissions.md) a la Store desde el panel del Centro de desarrollo.

6.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel de información del Centro de desarrollo.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Administrar unidades de anuncios para varios anuncios nativos en tu aplicación

Puedes usar varias colocaciones de anuncios nativos en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncio diferente para cada colocación de anuncio nativo. Con unidades de anuncios diferentes para cada anuncio nativo podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Artículos relacionados

* [Directrices para anuncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [Anuncios desde la aplicación](../publish/in-app-ads.md)
* [Configurar unidades de anuncios para la aplicación](set-up-ad-units-in-your-app.md)
