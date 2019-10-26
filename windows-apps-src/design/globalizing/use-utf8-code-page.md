---
Description: Use la codificación de caracteres UTF-8 para ofrecer una compatibilidad óptima entre las aplicaciones web y otras plataformas basadas en * Nix (UNIX, Linux y variantes), minimizar los errores de localización y reducir la sobrecarga de las pruebas.
title: Usar la página de códigos UTF-8 de Windows
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, globalización, localización
ms.localizationpriority: medium
ms.openlocfilehash: be3aade0289911f878d960fb62bde49b8ef840a8
ms.sourcegitcommit: 3a06cf3f8bd00e5e6eac3b38ee7e3c7cf4bc5197
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72888747"
---
# <a name="use-the-utf-8-code-page"></a>Usa la página de códigos UTF-8

Use la codificación de caracteres [UTF-8](http://www.utf-8.com/) para ofrecer una compatibilidad óptima entre las aplicaciones web y otras plataformas basadas en * Nix (UNIX, Linux y variantes), minimizar los errores de localización y reducir la sobrecarga de las pruebas.

UTF-8 es la página de códigos universal para la internacionalización y admite todos los puntos de código Unicode con codificación de ancho variable de 1-6 bytes. Se usa de forma generalizada en la web y es el valor predeterminado para las plataformas basadas en * nix.

## <a name="-a-vs--w-apis"></a>-Una API de vs.-W
  
Las API de Win32 suelen admitir las variantes-A y-W.

-Las variantes reconocen la página de códigos ANSI configurada en el sistema y admiten `char*`, mientras que las variantes de-W funcionan en UTF-16 y admiten el `WCHAR`.

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
> Agregue un manifiesto a un ejecutable existente desde la línea de comandos con `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Conversión de páginas de códigos

Como Windows funciona de forma nativa en UTF-16 (`WCHAR`), es posible que tenga que convertir los datos UTF-8 a UTF-16 (o viceversa) para interoperar con las API de Windows.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) y [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) permiten realizar conversiones entre UTF-8 y utf-16 (`WCHAR`) (y otras páginas de códigos). Esto es especialmente útil cuando una API de Win32 heredada solo puede comprender `WCHAR`. Estas funciones permiten convertir la entrada UTF-8 en `WCHAR` para pasarla a una API-W y, a continuación, volver a convertir los resultados si es necesario.
Al usar estas funciones con `CodePage` establecido en `CP_UTF8`, utilice `dwFlags` de `0` o `MB_ERR_INVALID_CHARS`, de lo contrario se produce una `ERROR_INVALID_FLAGS`.

Nota: `CP_ACP` equivale a `CP_UTF8` solo si se ejecuta en la versión 1903 de Windows (actualización 2019 de mayo) o superior y la propiedad ActiveCodePage descrita anteriormente está establecida en UTF-8. De lo contrario, respeta la página de códigos del sistema heredado. Se recomienda usar `CP_UTF8` explícitamente.

## <a name="related-topics"></a>Temas relacionados

- [Páginas de códigos](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Identificadores de páginas de códigos](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
