---
Description: Utilizar caracteres de UTF-8 codificación óptima compatibilidad entre aplicaciones web y otros * plataformas de nix (variantes, Linux y Unix), minimizar los errores de localización y reducir las pruebas de carga de trabajo.
title: Utilice la página de códigos UTF-8 de Windows
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, globalización, localización
ms.localizationpriority: medium
ms.openlocfilehash: 453d58b0d52aaa24461784b6f393b26b93e572a1
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827381"
---
# <a name="use-the-utf-8-code-page"></a>Utilice la página de códigos UTF-8

Use [UTF-8](http://www.utf-8.com/) codificación óptima compatibilidad entre aplicaciones web y otros caracteres * plataformas de nix (variantes, Linux y Unix), minimizar los errores de localización y reducir las pruebas de carga de trabajo.

UTF-8 es la página de códigos universal para la internacionalización y es compatible con todos los puntos de código Unicode mediante la codificación de ancho variable de 1 a 4 bytes. Se utiliza de forma mayoritaria en la web y es el valor predeterminado para * nix plataformas.

## <a name="-a-vs--w-apis"></a>-Un frente a las API -W
  
A menudo, las API de Win32 admite tanto - A -W variantes y.

-A variantes reconocen la página de códigos ANSI configurada en el sistema y compatibilidad con char *, mientras que las variantes -W operan en UTF-16 y soporte técnico `WCHAR`.

Hasta hace poco, Windows ha hecho énfasis variantes -W "Unicode" a través de: una API. Sin embargo, las versiones recientes han usado en la página de códigos ANSI y - una API como un medio para introducir UTF-8 admite para las aplicaciones. Si la página de códigos ANSI se configura para UTF-8, - A las API funcionan en UTF-8. Este modelo tiene la ventaja de admitir código existente creado con - una API sin realizar ningún cambio en el código.

## <a name="set-a-process-code-page-to-utf-8"></a>Establezca una página de códigos de proceso en UTF-8

Puede forzar un proceso para usar UTF-8 como la página de códigos de proceso mediante el appxmanifest para aplicaciones empaquetadas, o el manifiesto de la fusión de sin empaquetar aplicaciones mediante la propiedad ActiveCodePage.

Puede declarar esta propiedad y compila el destino o ejecutar en Windows anteriores, pero debe controlar la detección de página de código heredado y conversión como de costumbre (con una versión de destino mínimo de 1 de 19H, la página de códigos del proceso siempre será UTF-8).

## <a name="examples"></a>Ejemplos

**Manifiesto de AppX para una aplicación empaquetada:**

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

**Manifiesto de fusión para una aplicación sin empaquetar de Win32:**

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

## <a name="code-page-conversion"></a>Conversión de página de códigos

Como Windows funciona de forma nativa en UTF-16 (WCHAR), es posible que deba convertir los datos UTF-8 a UTF-16 (o viceversa) para interoperar con las API de Windows.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) y [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) permiten convertir entre UTF-8 y UTF-16 (WCHAR) (y otras páginas de códigos). Esto es especialmente útil cuando una API de Win32 heredada podría entender solo WCHAR. Estas funciones permiten convertir la entrada de UTF-8 en WCHAR para pasar a un -W API y, a continuación, convertir los resultados si es necesario.
Al usar estas funciones en Windows con CodePage CP_UTF8, utilice dwFlags 0 ó MB_ERR_INVALID_CHARS, de lo contrario se produce un ERROR_INVALID_FLAGS.

Nota: CP_ACP equivale a CP_UTF8 solo si se ejecuta en Windows versión 1903 (es posible que la actualización de 2019) y la propiedad ActiveCodePage que se ha descrito anteriormente se establece en UTF-8. En caso contrario, respeta la página de códigos del sistema heredado. Se recomienda usar CP_UTF8 explícitamente.

## <a name="related-topics"></a>Temas relacionados

- [Páginas de códigos](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Identificadores de página de código](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)