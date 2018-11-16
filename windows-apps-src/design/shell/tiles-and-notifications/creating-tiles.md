---
author: mijacobs
Description: A tile is an app's representation on the Start menu. Every app has a tile. When you create a new Universal Windows Platform (UWP) app project in Microsoft Visual Studio, it includes a default tile that displays your app's name and logo.
title: Iconos
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f4388b67335bce497987ab22e3b281cf86e029af
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6661723"
---
# <a name="tiles-for-uwp-apps"></a>Iconos de aplicaciones para UWP

 

Un *icono* es una representación de la aplicación en el menú Inicio. Todas las aplicaciones tienen un icono. Al crear un nuevo proyecto de aplicación de Plataforma universal de Windows (UWP) en Microsoft Visual Studio, incluye un icono predeterminado que muestra el nombre y el logotipo de la aplicación.Windows muestra este icono cuando se instala la aplicación por primera vez. Cuando se instala la aplicación, puedes cambiar el contenido del icono a través de notificaciones; por ejemplo, puedes cambiar el icono para comunicar nueva información al usuario, como titulares de noticias o el asunto del mensaje sin leer más reciente.

## <a name="configure-the-default-tile"></a>Configurar el icono predeterminado


Cuando creas un nuevo proyecto en Visual Studio, se crea un icono predeterminado sencillo que muestra el nombre y el logotipo de la aplicación.

Para editar el icono, haz doble clic en el archivo **Package.appxmanifest** del proyecto UWP principal para abrir el diseñador (o haz clic con el botón derecho del ratón y selecciona Ver código).

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
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

    Debes cambiar estas imágenes por las tuyas, Tienes la opción de proporcionar imágenes para diferentes escalas visuales, pero no tienes que proporcionarlas todas. Para garantizar que tu aplicación se ve bien en una gran variedad de dispositivos, te recomendamos que ofrezcas versiones de escala de 100 %, 200 % y 400 % de cada imagen. Consulta [activos de ventana e icono](app-assets.md) para obtener más información sobre la generación de estos activos.

    Las imágenes a escala siguen esta convención de nomenclatura:
    
    *&lt;nombre de imagen&gt;*.scale-*&lt;factor de escala&gt;*.*&lt;extensión de archivo de imagen&gt;* 

    Por ejemplo: SplashScreen.scale-100.png

    Cuando hagas referencia a la imagen, deberás referirte a ella como *&lt;nombre de imagen&gt;*.*&lt;extensión de archivo de imagen&gt;* ("SplashScreen.png" en este ejemplo). El sistema seleccionará automáticamente la imagen a escala adecuada para el dispositivo desde las imágenes que hayas proporcionado.

-   No es necesario, pero sí muy recomendable, ofrecer logotipos para tamaños de iconos grandes y anchos para que el usuario pueda cambiar el tamaño del icono de la aplicación a esos tamaños. Para ofrecer estas imágenes adicionales, crea un elemento **DefaultTile** y usa los atributos **Wide310x150Logo** y **Square310x310Logo** para especificar las imágenes adicionales:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>Usar notificaciones para personalizar el icono


Cuando se instale la aplicación, puedes usar las notificaciones para personalizar tu icono. Puedes hacerlo la primera vez que se inicie la aplicación o en respuesta a un evento, como una notificación de inserción.

Para obtener información sobre cómo enviar notificaciones de icono, consulta [Enviar una notificación de icono local](sending-a-local-tile-notification.md).
