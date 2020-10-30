---
description: Use la codificación de caracteres UTF-8 para ofrecer una compatibilidad óptima entre las aplicaciones web y otras \* plataformas basadas en Nix (UNIX, Linux y variantes), minimizar los errores de localización y reducir la sobrecarga de las pruebas.
title: Usar la página de códigos UTF-8 de Windows
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 8aefe7bc45bc7c41347fe8fc4b8192347c3e4354
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034308"
---
# <a name="use-the-utf-8-code-page"></a>Usa la página de códigos UTF-8

Use la codificación de caracteres [UTF-8](http://www.utf-8.com/) para ofrecer una compatibilidad óptima entre las aplicaciones web y otras \* plataformas basadas en Nix (UNIX, Linux y variantes), minimizar los errores de localización y reducir la sobrecarga de las pruebas.

UTF-8 es la página de códigos universal para la internacionalización y puede codificar todo el juego de caracteres Unicode. Se usa de forma generalizada en la web y es el valor predeterminado para las plataformas basadas en * nix.

> [!NOTE]
> Un carácter codificado toma entre 1 y 4 bytes. La codificación UTF-8 admite secuencias de bytes más largas, hasta 6 bytes, pero el punto de código más grande de Unicode 6,0 (U + 10FFFF) solo toma 4 bytes.

## <a name="-a-vs--w-apis"></a>-Una API de vs.-W
  
Las API de Win32 suelen admitir las variantes-A y-W.

-Las variantes reconocen la página de códigos ANSI configurada en el sistema y admiten `char*` , mientras que las variantes de-W funcionan en UTF-16 y admiten `WCHAR` .

Hasta hace poco, Windows ha resaltado "Unicode"-W variantes a través de las API. Sin embargo, las versiones recientes han usado la página de códigos ANSI y las API como un medio para introducir la compatibilidad con UTF-8 en las aplicaciones. Si la página de códigos ANSI está configurada para UTF-8,-las API funcionan en UTF-8. Este modelo tiene la ventaja de admitir el código existente compilado con las API-A sin necesidad de realizar ningún cambio en el código.

## <a name="set-a-process-code-page-to-utf-8"></a>Establecer una página de códigos del proceso en UTF-8

A partir de la versión 1903 de Windows (actualización de mayo de 2019), puede usar la propiedad ActiveCodePage en appxmanifest para aplicaciones empaquetadas o el manifiesto de fusión para aplicaciones sin empaquetar, a fin de forzar que un proceso use UTF-8 como página de códigos del proceso.

Puede declarar esta propiedad y destino o ejecutar en compilaciones anteriores de Windows, pero debe administrar la detección y conversión de páginas de códigos heredadas como de costumbre. Con una versión de destino mínima de la versión 1903 de Windows, la página de códigos del proceso siempre será UTF-8, por lo que la detección y conversión de la página de códigos heredada se puede evitar.

## <a name="examples"></a>Ejemplos

**Manifiesto de appx para una aplicación empaquetada:**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**Manifiesto de fusión para una aplicación Win32 desempaquetada:**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> Agregar un manifiesto a un ejecutable existente desde la línea de comandos con `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Conversión de páginas de códigos

Como Windows funciona de forma nativa en UTF-16 ( `WCHAR` ), es posible que tenga que convertir los datos UTF-8 a UTF-16 (o viceversa) para interoperar con las API de Windows.

[MultiByteToWideChar](/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) y [WideCharToMultiByte](/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) permiten realizar conversiones entre UTF-8 y UTF-16 ( `WCHAR` ) (y otras páginas de códigos). Esto es especialmente útil cuando una API de Win32 heredada solo puede entender `WCHAR` . Estas funciones permiten convertir la entrada UTF-8 en `WCHAR` para pasar a una API-W y, a continuación, volver a convertir los resultados si es necesario.
Cuando se usan estas funciones con `CodePage` establecido en `CP_UTF8` , `dwFlags` se usa `0` o `MB_ERR_INVALID_CHARS` , en caso contrario, `ERROR_INVALID_FLAGS` se produce una.

> [!NOTE]
> `CP_ACP` equivale a `CP_UTF8` solo si se ejecuta en la versión 1903 de Windows (actualización 2019 de mayo) o superior y la propiedad ActiveCodePage descrita anteriormente está establecida en UTF-8. De lo contrario, respeta la página de códigos del sistema heredado. Se recomienda usar `CP_UTF8` explícitamente.

## <a name="related-topics"></a>Temas relacionados

- [Páginas de códigos](/windows/desktop/Intl/code-pages)
- [Identificadores de páginas de códigos](/windows/desktop/Intl/code-page-identifiers)
