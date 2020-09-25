---
Description: Un icono es una representación de la aplicación en el menú Inicio. Todas las aplicaciones tienen un icono. Al crear un nuevo proyecto de aplicación de Windows en Microsoft Visual Studio, incluye un icono predeterminado que muestra el nombre y el logotipo de la aplicación.
title: Iconos de aplicaciones de Windows
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19c8612188000a3d1161fa746d6e7944667a5104
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218158"
---
# <a name="tiles-for-windows-apps"></a>Iconos de aplicaciones de Windows

 

Un *icono* es una representación de la aplicación en el menú Inicio. Todas las aplicaciones tienen un icono. Al crear un nuevo proyecto de aplicación de Windows en Microsoft Visual Studio, incluye un icono predeterminado que muestra el nombre y el logotipo de la aplicación.Windows muestra este icono cuando se instala la aplicación por primera vez. Cuando se instala la aplicación, puedes cambiar el contenido del icono a través de notificaciones; por ejemplo, puedes cambiar el icono para comunicar nueva información al usuario, como titulares de noticias o el asunto del mensaje sin leer más reciente.

## <a name="configure-the-default-tile"></a>Configurar el icono predeterminado


Cuando creas un nuevo proyecto en Visual Studio, se crea un icono predeterminado sencillo que muestra el nombre y el logotipo de la aplicación.

Para editar el icono, haga doble clic en el archivo **Package. appxmanifest** en el proyecto principal de UWP para abrir el diseñador (o haga clic con el botón derecho en el archivo y seleccione Ver código).

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

    Debes cambiar estas imágenes por las tuyas, Tienes la opción de proporcionar imágenes para diferentes escalas visuales, pero no tienes que proporcionarlas todas. Para garantizar que tu aplicación se ve bien en una gran variedad de dispositivos, te recomendamos que ofrezcas versiones de escala de 100 %, 200 % y 400 % de cada imagen. Consulte [iconos y logotipos de aplicaciones](../../style/app-icons-and-logos.md) para obtener más información sobre la generación de estos recursos.

    Las imágenes a escala siguen esta convención de nomenclatura:
    
    * &lt; nombre &gt; *de la imagen.* &lt; factor &gt; de escala*de escala.* &lt; extensión &gt; de archivo de imagen* 

    Por ejemplo: SplashScreen.scale-100.png

    Al hacer referencia a la imagen, se hace referencia a ella como * &lt; nombre &gt; *de la imagen.* &lt; extensión &gt; de archivo de imagen* ("SplashScreen.png" en este ejemplo). El sistema seleccionará automáticamente la imagen a escala adecuada para el dispositivo desde las imágenes que hayas proporcionado.

-   No es necesario, pero sí muy recomendable, ofrecer logotipos para tamaños de iconos grandes y anchos para que el usuario pueda cambiar el tamaño del icono de la aplicación a esos tamaños. Para proporcionar estas imágenes adicionales, cree un elemento **DefaultTile** y use los atributos **Wide310x150Logo** y **Square310x310Logo** para especificar las imágenes adicionales:
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


Cuando se instale la aplicación, puedes usar las notificaciones para personalizar tu icono. Puede hacerlo la primera vez que se inicie la aplicación o en respuesta a un evento, como una notificación de envío.

Para obtener información sobre cómo enviar notificaciones de icono, consulte [enviar una notificación de icono local](sending-a-local-tile-notification.md).
