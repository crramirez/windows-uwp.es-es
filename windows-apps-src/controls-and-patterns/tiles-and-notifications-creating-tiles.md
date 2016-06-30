---
author: mijacobs
Description: "Un icono es una representación de la aplicación en el menú Inicio. Todas las aplicaciones tienen un icono. Al crear un nuevo proyecto de aplicación para la Plataforma universal de Windows (UWP) en Microsoft Visual Studio, incluye un icono predeterminado que muestra el nombre y el logotipo de la aplicación."
title: Iconos
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.sourcegitcommit: d3fe62d4de00c42079d62d105acdbb21e296ba5f
ms.openlocfilehash: a9f5d25dfd359364fa8e16666b03c7c105a867dd

---

# Iconos de aplicaciones para UWP





Un *icono* es una representación de la aplicación en el menú Inicio. Todas las aplicaciones tienen un icono. Al crear un nuevo proyecto de aplicación de Plataforma universal de Windows (UWP) en Microsoft Visual Studio, incluye un icono predeterminado que muestra el nombre y el logotipo de la aplicación. Windows muestra este icono cuando se instala la aplicación por primera vez. Cuando se instala la aplicación, puedes cambiar el contenido del icono a través de notificaciones; por ejemplo, puedes cambiar el icono para comunicar nueva información al usuario, como titulares de noticias o el asunto del mensaje sin leer más reciente.

## <span id="Configure_the_default_tile"></span><span id="configure_the_default_tile"></span><span id="CONFIGURE_THE_DEFAULT_TILE"></span>Configurar el icono predeterminado


Cuando creas un nuevo proyecto en Visual Studio, se crea un icono predeterminado sencillo que muestra el nombre y el logotipo de la aplicación.

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Hay algunos elementos que deberías actualizar:

-   DisplayName: reemplaza este valor por el nombre que quieras que aparezca en el icono.
-   ShortName: dado que no hay espacio limitado para que el nombre para mostrar quepa en los iconos, recomendamos que especifiques un ShortName también, para asegurarte de que el nombre de la aplicación no se trunca.
-   Imágenes de logotipo:

    Debes cambiar estas imágenes por las tuyas, Tienes la opción de proporcionar imágenes para diferentes escalas visuales, pero no tienes que proporcionarlas todas. Para garantizar que tu aplicación se ve bien en una variedad de dispositivos, te recomendamos que ofrezcas versiones de escala de 100 %, 200 % y 400 % de cada imagen.

    Las imágenes a escala siguen esta convención de nomenclatura: 
    
    *
              &lt;nombre de imagen&gt;*.scale-*&lt;factor de escala&gt;*.*&lt;extensión de archivo de imagen&gt;*  
    
    Por ejemplo: SmallLogo.scale-100.png

    Cuando hace referencia a la imagen, se refiere a ella como *&lt;nombre de imagen&gt;*.*&lt;extensión de archivo de imagen&gt;* ("SmallLogo.png" en este ejemplo). El sistema seleccionará automáticamente la imagen a escala adecuada para el dispositivo desde las imágenes que hayas proporcionado.

-   No es necesario, pero sí muy recomendable, ofrecer logotipos para tamaños de iconos grandes y anchos para que el usuario pueda cambiar el tamaño del icono de la aplicación a esos tamaños. Para ofrecer estas imágenes adicionales, crea un elemento `DefaultTile` y usa los atributos `Wide310x150Logo` y `Square310x310Logo` para especificar las imágenes adicionales:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <span id="Use_notifications_to_customize_your_tile"></span><span id="use_notifications_to_customize_your_tile"></span><span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"></span>Usar notificaciones para personalizar el icono


Cuando se instale la aplicación, puedes usar las notificaciones para personalizar tu icono. Puedes hacerlo la primera vez que se inicia la aplicación o en respuesta a algún evento, como una notificación de inserción.

1.  Crea una carga XML (en forma de un [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)) que describa el icono.

    -   Windows 10 presenta un nuevo esquema de icono adaptable que puedes usar. Para obtener instrucciones, consulta [Iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md). Para el esquema, consulta el artículo [Esquema de iconos adaptativos](tiles-and-notifications-adaptive-tiles-schema.md). 

    -   Puedes usar las plantillas de icono de Windows 8.1 para definir el icono. Para obtener más información, consulta [Creación de iconos y distintivos (Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260).

2.  Crear un objeto de notificación de icono y pásale el [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) que has creado. Hay varios tipos de objetos de notificación:
    -   Un objeto [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) para actualizar el icono de inmediato.
    -   Un objeto [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) para actualizar el icono en algún momento del futuro.

3.  Usa la [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623) para crear un objeto [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628).
4.  Llama al método [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) y pásale el objeto de notificación de icono que creaste en el paso 2.

 

 







<!--HONumber=Jun16_HO4-->


