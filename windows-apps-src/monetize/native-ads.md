---
author: mcleanbyron
description: "Obtén información sobre cómo agregar anuncios nativos a tu aplicación para UWP."
title: Anuncios nativos
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, control de anuncios, anuncio nativo
ms.openlocfilehash: 47a69e48f04c670a462c34083af1117d2c7908d8
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2017
---
# <a name="native-ads"></a>Anuncios nativos

Un anuncio nativo es un formato de anuncio basado en componentes, donde cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual. Puedes integrar estos elementos en la aplicación con tus propias fuentes, colores, animaciones y otros componentes de interfaz de usuario para unir una experiencia de usuario discreta que se ajuste a la apariencia de la aplicación a la vez que también se obtiene un alto rendimiento de los anuncios.

Para los anunciantes, los anuncios nativos proporcionan ubicaciones de alto rendimiento, porque la experiencia de anuncios está estrechamente integrada en la aplicación y, por lo tanto, los usuarios tienden a interactuar más con estos tipos de anuncios.

> [!NOTE]
> Para proporcionar anuncios nativos para la versión pública de la aplicación en la Tienda, debes crear un unidad de anuncio **Nativo** en la página **Monetizar con anuncios** del panel del Centro de desarrollo. La capacidad para crear unidades de anuncios **Nativo** solo está disponible actualmente para seleccionar los desarrolladores que participan en un programa piloto, pero queremos que esta característica esté disponible para todos los desarrolladores pronto. Si estás interesado en unirte a nuestro programa piloto, ponte en contacto con nosotros en aiacare@microsoft.com.

> [!NOTE]
> Los anuncios nativos solo se admiten actualmente para aplicaciones para UWP basadas en XAML para Windows 10. Está prevista la compatibilidad para aplicaciones para UWP escrita con HTML y JavaScript para una futura versión del SDK de Microsoft Advertising.

## <a name="prerequisites"></a>Requisitos previos

* Instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) (versión 10.0.4 o posterior) con Visual Studio 2015 o una versión posterior de Visual Studio. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md). Puedes instalar el SDK en el equipo de desarrollo mediante el instalador MSI o bien, puedes instalar el SDK para su uso en un proyecto específico mediante el paquete NuGet.

## <a name="integrate-a-native-ad-into-your-app"></a>Integrar un anuncio nativo en la aplicación

Sigue estas instrucciones para integrar un anuncio nativo en la aplicación y confirma que la implementación de anuncio nativo muestra un anuncio de prueba.

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia al SDK de Microsoft Advertising correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**

4.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

5.  En el **Administrador de referencias**, haz clic en Aceptar.

6. En el archivo de código adecuado de la aplicación (por ejemplo, en MainPage.xaml.cs o un archivo de código para otra página), agrega las siguientes referencias de espacio de nombres.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

7.  En una ubicación adecuada de tu aplicación (por ejemplo, en ```MainPage``` o en otra página), declara un objeto [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.aspx) y varios campos de cadenas que representen el id. de aplicación y el id. de unidad del anuncio nativo. En el siguiente ejemplo de código, se asignan los campos `myAppId` y `myAdUnitId` a los [valores de prueba](test-mode-values.md) para anuncios nativos.

    > [!NOTE]
    > Cada **NativeAdsManager** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control de anuncio nativo y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Tienda, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

8.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **NativeAdsManager** y conecta controladores de eventos para los eventos [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.adready.aspx) y [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.erroroccurred.aspx) del objeto.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

9.  Cuando estés listo para mostrar un anuncio nativo, llama al método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.requestad.aspx) para capturar un anuncio.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

10.  Cuando un anuncio nativo esté listo para tu aplicación, se llama al controlador de eventos **AdReady** y un objeto [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) que representa el anuncio nativo se pasa al parámetro *e*. Usa las propiedades **NativeAd** para obtener cada elemento del anuncio nativo y mostrar estos elementos en la página. Asegúrate de llamar también al método [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) para registrar el elemento de la interfaz de usuario que actúa como un contenedor para el anuncio nativo; esto es necesario para realizar un seguimiento adecuado de los clics y de las impresiones de anuncios.
  > [!NOTE]
  > Algunos de los elementos del anuncio nativo son necesarios y siempre se deben mostrar en la aplicación. Para obtener más información, consulta los [requisitos y directrices](#requirements-and-guidelines) .

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

11.  Define un controlador de eventos para que el evento **ErrorOccurred** controle los errores relacionados con el anuncio nativo. En el ejemplo siguiente se escribe la información de error en la ventana **Salida** de Visual Studio durante las pruebas.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

12.  Compila y ejecuta la aplicación para verla con un anuncio de prueba.

<span id="release" />
## <a name="release-your-app-with-live-ads"></a>Publicar tu aplicación con anuncios dinámicos

Después de confirmar que la implementación de anuncios nativo muestra correctamente un anuncio de prueba, sigue estas instrucciones para configurar la aplicación para que muestre anuncios reales y envía la aplicación actualizada a la Tienda.

1.  Asegúrate de que la implementación de anuncio nativo sigue los [requisitos y directrices](#requirements-and-guidelines) para los anuncios nativos.

2.  En el panel del Centro de desarrollo, ve a la página [Monetizar con anuncios](../publish/monetize-with-ads.md) correspondiente a la aplicación y [crea una unidad de anuncios](../monetize/set-up-ad-units-in-your-app.md). Especifica el tipo de unidad de anuncio **Nativo**. Anota el id. de la unidad de anuncios y el id. de la aplicación.
    > [!IMPORTANT]
    > Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

3. Tienes la opción de habilitar la mediación de anuncios para el anuncio nativo mediante la configuración de las opciones de la sección [Mediación de anuncios](../publish/monetize-with-ads.md#mediation) de la página [Monetizar con anuncios](../publish/monetize-with-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación mostrando anuncios de múltiples redes de anuncio.

4.  En el código, reemplaza los valores de unidades de anuncio de prueba (es decir, los parámetros *applicationId* y *adUnitId* del constructor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx)) por los valores dinámicos generados en el Centro de desarrollo.

5.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

6.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

<span id="requirements-and-guidelines" />
## <a name="requirements-and-guidelines"></a>Requisitos y directrices

Los anuncios nativos le proporcionan mucho control sobre la manera en que presenta el contenido de publicidad a los usuarios. Sigue estos requisitos y directrices para ayudar a garantizar que el mensaje del anunciante llega a los usuarios a la vez que se ayuda a evitar ofrecer una experiencia de anuncios nativos confusa a los usuarios.

### <a name="register-the-container-for-your-native-ad"></a>Registrar el contenedor para el anuncio nativo

En el código, debes llamar al método [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) del objeto **NativeAd** para registrar el elemento de interfaz de usuario que actúa como contenedor para el anuncio nativo y, opcionalmente, los controles que quieras registrar como destinos seleccionables para el anuncio. Esto es necesario para realizar un seguimiento adecuado de los clics y de las impresiones de anuncios.

Hay dos sobrecargas para el método **RegisterAdContainer** que puedes usar:

* Si quieres que sea seleccionable el contenedor completo para todos los elementos de anuncios nativos individuales, llama al método [RegisterAdContainer(FrameworkElement)](https://msdn.microsoft.com/library/windows/apps/mt809188.aspx) y pasa el control del contenedor al método. Por ejemplo, si muestras todos los elementos de anuncios nativos en controles independientes que se hospedan en un **StackPanel** y quieres que sea seleccionable todo el **StackPanel**, pasa el **StackPanel** a este método.

* Si quieres que solo sean seleccionables determinados elementos de anuncios nativos, llama al método [RegisterAdContainer(FrameworkElement, IVector(FrameworkElement))](https://msdn.microsoft.com/library/windows/apps/mt809189.aspx). Solo serán seleccionables los controles que pases al segundo parámetro.

### <a name="required-native-ad-elements"></a>Elementos de anuncios nativos necesarios

Como mínimo, siempre debes mostrar los siguientes elementos de anuncios nativos al usuario en el diseño de anuncio nativo. Si no incluyes estos elementos, puedes ver un rendimiento deficiente y bajas rentabilidades para tu unidad de anuncio.

1. Muestra siempre el título del anuncio nativo (disponible en la propiedad [Título](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.title.aspx) del objeto **NativeAd**). Proporciona espacio suficiente para mostrar 25 caracteres como mínimo. Si el título es más largo, reemplaza el texto adicional por puntos suspensivos.
2. Muestra siempre al menos uno de los siguientes elementos para ayudar a diferenciar la experiencia de anuncios nativos del resto de la aplicación y destacar claramente que el contenido lo proporciona un anunciante:
  * El icono *ad* diferenciable (disponible en la propiedad [AdIcon](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.adicon.aspx) del objeto **NativeAd**). Este icono lo proporciona Microsoft.
  * El texto *patrocinado por* (disponible en la propiedad [SponsoredBy](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.sponsoredby.aspx) del objeto **NativeAd**). Este texto lo proporciona el anunciante.
  * Como alternativa al texto *patrocinado por*, puedes elegir mostrar algún otro texto que ayude a diferenciar la experiencia de anuncios nativos del resto de la aplicación, como "Contenido patrocinado", "Contenido promocional", "Contenido recomendado", etc.

### <a name="user-experience"></a>Experiencia del usuario

El anuncio nativo se debe delinear claramente del resto de la aplicación y tiene un espacio alrededor para impedir los clics accidentales. Usa bordes, diferentes fondos o alguna otra interfaz de usuario para separar el contenido del anuncio del resto de la aplicación. Ten en cuenta que los clics en anuncios accidentales no son beneficios para tus ingresos basados en anuncios o tu experiencia de usuario final a largo plazo.

### <a name="description"></a>Descripción

Si eliges mostrar la descripción para la propiedad (disponible en la [Descripción](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.description.aspx) del anuncio del objeto **NativeAd**), proporciona suficiente espacio para mostrar 75 caracteres como mínimo. Te recomendamos que uses una animación para mostrar el contenido completo de la descripción del anuncio.

### <a name="call-to-action"></a>Llamada a la acción

El texto de la *llamada a la acción* (disponible en la propiedad [CallToAction](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.calltoaction.aspx) del objeto **NativeAd**) es un componente fundamental del anuncio. Si decides mostrar este texto, sigue estas directrices:

* Muestra siempre el texto de la *llamada a la acción* al usuario en un control interactivo como un botón o hipervínculo.
* Muestra siempre el texto de la *llamada a la acción* en su totalidad.
* Asegúrate de que el texto de la *llamada a la acción* es independiente del resto del texto promocional del anunciante.

### <a name="learn-and-optimize"></a>Aprender y optimizar

Te recomendamos que crees y uses unidades de anuncio diferentes para cada colocación de anuncio nativo diferente en tu aplicación. Esto te permite obtener datos de informes independiente para cada colocación de anuncio nativo y puedes usar estos datos para realizar cambios que optimicen el rendimiento de cada colocación de anuncio nativo.

## <a name="related-topics"></a>Temas relacionados

* [Monetizar con anuncios](../publish/monetize-with-ads.md)
* [Configurar unidades de anuncios para la aplicación](../monetize/set-up-ad-units-in-your-app.md)
