---
author: stevewhims
Description: There are several URI (Uniform Resource Identifier) schemes that you can use to refer to files that come from your app's package, your app's data folders, or the cloud. You can also use a URI scheme to refer to strings loaded from your app's Resources Files (.resw).
title: Esquemas de URI
template: detail.hbs
ms.author: stwhi
ms.date: 10/16/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 59cd664e268e9e62786728aeb122ec52acd721c0
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5559978"
---
# <a name="uri-schemes"></a>Esquemas de URI

Existen varios esquemas de URI (identificador uniforme de recursos) que puedes usar para hacer referencia a archivos que provienen del paquete de la aplicación, las carpetas de datos de la aplicación o la nube. También puedes usar un esquema de URI para hacer referencia a cadenas cargadas desde archivos de recursos (.resw) de la aplicación. Puedes usar estos esquemas de URI en el código, en el marcado XAML, en el manifiesto del paquete de la aplicación, o en la ventana y las plantillas de notificación del sistema.

## <a name="common-features-of-the-uri-schemes"></a>Características comunes de los esquemas de URI

Todos los esquemas descritos en este tema siguen las reglas de esquema URI típicas para la normalización y la recuperación de recursos. Consulta [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444) para conocer la sintaxis genérica de un URI.

Todos los esquemas de URI definen la parte jerárquica conforme a [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444), como los componentes de ruta y de autoridad del URI.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Esto significa que básicamente hay tres componentes de un URI. Inmediatamente después de las dos barras inclinadas deI *esquema* de URI se encuentra un componente (que puede estar vacío) denominado *autoridad*. Y, justo después, está la *ruta de acceso*. Tomando el URI `http://www.contoso.com/welcome.png` como ejemplo, el esquema es "`http://`", la autoridad es "`www.contoso.com`" y la ruta de acceso es "`/welcome.png`". Otro ejemplo es el URI `ms-appx:///logo.png`, donde los componentes de la autoridad están vacío y toman un valor predeterminado.

El procesamiento específico del esquema de los URI mencionados en este tema ignora el componente de fragmento. Durante la recuperación y la comparación de recursos, el componente de fragmento no tiene aceptación. Sin embargo, las capas por encima de una implementación específica pueden interpretar el fragmento para recuperar un recurso secundario.

La comparación tiene lugar byte a byte después de la normalización de todos los componentes de IRI.

## <a name="case-insensitivity-and-normalization"></a>Independencia entre mayúsculas y minúsculas y normalización

Todos los esquemas de URI descritos en este tema siguen las reglas de esquema de URI (RFC 3986)típicas para la normalización y la recuperación de recursos. La forma normalizada de estos URI mantiene el uso de mayúsculas y minúsculas, y decodifica mediante el símbolo de porcentaje los caracteres RFC 3986 no reservados.

Para todos los esquemas de URI descritos en este tema, *esquema*, *autoridad* y *ruta de acceso* no distinguen entre mayúsculas y minúsculas de manera estándar, o bien los procesa el sistema sin distinguir entre mayúsculas y minúsculas. **Nota** La única excepción a esa regla es la *autoridad* de `ms-resource`, que distingue entre mayúsculas y minúsculas.

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx y ms-appx-web

Usa el `ms-appx` o el esquema de URI `ms-appx-web` para hacer referencia a un archivo que procede del paquete de la aplicación (consulta [Empaquetado de aplicaciones](../packaging/index.md)). Los archivos del paquete de aplicaciones son normalmente imágenes estáticas, datos, código y archivos de distribución. El esquema `ms-appx-web` accede a los mismos archivos que `ms-appx`, pero en el compartimiento web. Para ver ejemplos y obtener más información, consulta [Referenciar a una imagen u otro activo de código y marcado XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Nombre de esquema (ms-appx y ms-appx-web)

El nombre del esquema de URI es la cadena "ms-appx" o "ms-appx-web".

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autoridad (ms-appx y ms-appx-web)

La autoridad es el nombre de identidad de paquete que se define en el manifiesto del paquete. Por lo tanto, está limitado tanto en la forma del URI como del IRI (identificador de recursos internacionalizado) al conjunto de caracteres permitidos en un nombre de identidad del paquete. El nombre del paquete debe ser el nombre de uno de los paquetes del gráfico de dependencia del paquete de la aplicación actualmente en ejecución.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Si aparecen otros caracteres en la autoridad, en ese caso la recuperación y la comparación producirán errores. El valor predeterminado de la autoridad es el paquete de la aplicación actualmente en ejecución.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Información del usuario y puerto (ms-appx y ms-appx-web)

El esquema `ms-appx`, a diferencia de otros esquemas populares, no define un componente de información del usuario o puerto. Dado que no se permite el uso de "@" and ":" como valores de autoridad válidos, la búsqueda generará un error caso de que estén incluidos. Cada uno de los siguientes elementos generará un error:

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Ruta (ms-appx y ms-appx-web)

El componente de ruta de acceso coincide con la sintaxis de RFC 3986 genérica y admite caracteres que no sean ASCII en los IRI. El componente de ruta de acceso define la ruta al archivo físico o lógico de un archivo. Ese archivo está en una carpeta asociada con la ubicación instalada del paquete de la aplicación, para la aplicación especificada por la autoridad.

Si la ruta de acceso hace referencia a un nombre físico de ruta de acceso y archivo, en ese caso se recupera ese activo de archivo físico. Pero, de no encontrarse ese archivo físico, en ese caso el recurso real devuelto durante la recuperación se determina usando negociación de contenidos en tiempo de ejecución. Esta determinación se basa en la aplicación, el sistema operativo y la configuración del usuario, como el idioma, el mostrar el factor de escala, el tema, el contraste alto y otros contextos de tiempo de ejecución. Por ejemplo, una combinación de los idiomas de la aplicación, la configuración de pantalla del sistema y la configuración de contraste alto del usuario pueden tenerse en cuenta al determinar el valor del recurso real que se recuperará:

```xml
ms-appx:///images/logo.png
```

El URI anterior puede recuperar en realidad un archivo del paquete de la aplicación actual con el siguiente nombre de archivo físico.

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

Por supuesto, podrías recuperar también ese mismo archivo físico haciendo referencia al mismo directamente por su nombre completo.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

El componente de ruta de acceso de `ms-appx(-web)` distingue entre mayúsculas y minúsculas, al igual que los URI genéricos. Sin embargo, cuando el sistema de archivos subyacente mediante el cual se accede al recurso distingue entre mayúsculas y minúsculas, como para NTFS, la recuperación del recurso se realiza sin distinguir entre mayúsculas y minúsculas.

La forma normalizada del URI mantiene mayúsculas y minúsculas y decodifica mediante el símbolo de porcentaje (un símbolo "%" seguido de la representación hexadecimal de dos dígitos) los caracteres RFC 3986 no reservados. Los caracteres "?", "#", "/", "*" y '”' (carácter de comilla doble) deben codificarse con caracteres de porcentaje en las rutas de acceso para representar datos como los nombres de archivo o carpeta. Todos los caracteres codificados con símbolos de porcentaje se decodifican antes de la recuperación. De esta manera, para recuperar un archivo llamado Hello#World.html, usa este URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Consulta (ms-appx y ms-appx-web)

Los parámetros de consulta se ignoran durante la recuperación de recursos. La forma normalizada de los parámetros de consulta mantiene las mayúsculas y las minúsculas. Los parámetros de consulta no se ignoran durante la comparación.

## <a name="ms-appdata"></a>ms-appdata

Usa el esquema de URI `ms-appdata` para hacer referencia a archivos provenientes de las carpetas de datos temporales, móviles y locales de la aplicación. Para más información sobre estas carpetas de datos de la aplicación, consulta [Almacenar y recuperar la configuración y otros datos de aplicaciones](../design/app-settings/store-and-retrieve-app-data.md).

El esquema de URI `ms-appdata` no realiza la negociación de contenido en tiempo de ejecución que hacen [ms-appx y ms-appx-web](#ms-appx-and-ms-appx-web). Pero puedes responder al contenido de [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) y cargar los activos adecuados desde los datos de la aplicación mediante sus nombre completos de archivo físico del URI.

### <a name="scheme-name-ms-appdata"></a>Nombre de esquema (ms-appdata)

El nombre del esquema de URI es la cadena "ms-appdata".

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autoridad (ms-appdata)

La autoridad es el nombre de identidad de paquete que se define en el manifiesto del paquete. Por lo tanto, está limitado tanto en la forma del URI como del IRI (identificador de recursos internacionalizado) al conjunto de caracteres permitidos en un nombre de identidad del paquete. El nombre de paquete debe ser el nombre del paquete de la aplicación que actualmente está en ejecución.

```xml
ms-appdata://Contoso.MyApp/
```

Si aparecen otros caracteres en la autoridad, en ese caso la recuperación y la comparación producirán errores. El valor predeterminado de la autoridad es el paquete de la aplicación actualmente en ejecución.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Información de usuario y puerto (ms-appdata)

El esquema `ms-appdata`, a diferencia de otros esquemas populares, no define un componente de información de usuario o puerto. Dado que no se permite el uso de "@" and ":" como valores de autoridad válidos, la búsqueda generará un error caso de que estén incluidos. Cada uno de los siguientes elementos generará un error:

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Ruta (ms-appdata)

El componente de ruta de acceso coincide con la sintaxis de RFC 3986 genérica y admite caracteres que no sean ASCII en los IRI. Dentro de la ubicación [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) se encuentran tres carpetas reservadas para el almacenamiento de estado local, móvil y temporal. El esquema `ms-appdata` permite el acceso a archivos y carpetas de esas ubicaciones. El primer segmento del componente de ruta de acceso debe especificar la carpeta particular de la siguiente manera. De esta manera, la forma "path-empty" de "hier-part" no es legal.

Carpeta local.

```xml
ms-appdata:///local/
```

Carpeta temporal.

```xml
ms-appdata:///temp/
```

Carpeta móvil.

```xml
ms-appdata:///roaming/
```

El componente de ruta de acceso de `ms-appdata` distingue entre mayúsculas y minúsculas, al igual que los URI genéricos. Sin embargo, cuando el sistema de archivos subyacente mediante el cual se accede al recurso distingue entre mayúsculas y minúsculas, como para NTFS, la recuperación del recurso se realiza sin distinguir entre mayúsculas y minúsculas.

La forma normalizada del URI mantiene mayúsculas y minúsculas y decodifica mediante el símbolo de porcentaje (un símbolo "%" seguido de la representación hexadecimal de dos dígitos) los caracteres RFC 3986 no reservados. Los caracteres "?", "#", "/", "*" y '”' (carácter de comilla doble) deben codificarse con caracteres de porcentaje en las rutas de acceso para representar datos como los nombres de archivo o carpeta. Todos los caracteres codificados con símbolos de porcentaje se decodifican antes de la recuperación. De esta manera, para recuperar un archivo local denominado Hello#World.html, usa este URI.

```xml
ms-appdata://local/Hello%23World.html
```

La recuperación del recurso y la identificación del segmento de ruta de acceso de nivel superior se controlan después de la normalización de los puntos (".././b/c"). Por lo tanto, los URI no pueden aplicarse puntos a sí mismos excepto en una de las carpetas reservadas. De esta manera, no se permite el siguiente URI.

```xml
ms-appdata:///local/../hello/logo.png
```

Pero se permite este URI (aunque redundante).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Consulta (ms-appdata)

Los parámetros de consulta se ignoran durante la recuperación de recursos. La forma normalizada de los parámetros de consulta mantiene las mayúsculas y las minúsculas. Los parámetros de consulta no se ignoran durante la comparación.

## <a name="ms-resource"></a>ms-resource

Usa el esquema de URI `ms-resource` para hacer referencia a cadenas cargadas desde archivos de recursos (.resw) de la aplicación. Para obtener ejemplos y más información sobre identificadores de recursos, consulta [Localizar las cadenas de la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Nombre de esquema (ms-resource)

El nombre del esquema de URI es la cadena "ms-resource".

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autoridad (ms-resource)

La autoridad es el mapa de recursos de nivel superior definido en el índice de recursos de paquete (PRI), que generalmente corresponde al nombre de identidad del paquete definido en el manifiesto del paquete. Consulta [Empaquetado de aplicaciones](../packaging/index.md)). Por lo tanto, está limitado tanto en la forma del URI como del IRI (identificador de recursos internacionalizado) al conjunto de caracteres permitidos en un nombre de identidad del paquete. El nombre del paquete debe ser el nombre de uno de los paquetes del gráfico de dependencia del paquete de la aplicación actualmente en ejecución.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Si aparecen otros caracteres en la autoridad, en ese caso la recuperación y la comparación producirán errores. El valor predeterminado de la autoridad es nombre del paquete, con distinción entre mayúsculas y minúsculas, de la aplicación actualmente en ejecución.

```xml
ms-resource:///
```

La autoridad distingue entre mayúsculas y minúsculas y la forma normalizada mantiene las mayúsculas y las minúsculas. La búsqueda de un recurso se realiza, sin embargo, sin distinguir entre mayúsculas y minúsculas.

### <a name="user-info-and-port-ms-resource"></a>Información de usuario y puerto (ms-resource)

El esquema `ms-resource`, a diferencia de otros esquemas populares, no define un componente de información de usuario o puerto. Dado que no se permite el uso de "@" and ":" como valores de autoridad válidos, la búsqueda generará un error caso de que estén incluidos. Cada uno de los siguientes elementos generará un error.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Ruta (ms-resource)

La ruta de acceso identifica la ubicación jerárquica del subárbol [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) (consulta el [Sistema de administración de recursos](https://msdn.microsoft.com/library/windows/apps/jj552947)) y [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) dentro de él. En general, esto corresponde al nombre de archivo (no se incluye la extensión) de un archivo de recursos (.resw) y el identificador de un recurso de cadena dentro de él.

Para obtener ejemplos y más información, consulta [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md) y [Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

El componente de ruta de acceso de `ms-resource` distingue entre mayúsculas y minúsculas, al igual que los URI genéricos. No es así cuando la recuperación subyacente efectúa un [CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628) con *ignoreCase* establecido en `true`.

La forma normalizada del URI mantiene mayúsculas y minúsculas y decodifica mediante el símbolo de porcentaje (un símbolo "%" seguido de la representación hexadecimal de dos dígitos) los caracteres RFC 3986 no reservados. Los caracteres "?", "#", "/", "*" y '”' (carácter de comilla doble) deben codificarse con caracteres de porcentaje en las rutas de acceso para representar datos como los nombres de archivo o carpeta. Todos los caracteres codificados con símbolos de porcentaje se decodifican antes de la recuperación. Por lo tanto, para recuperar un recurso de cadena de un archivo de recursos denominado Hello#World.resw, usa este URI.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Consulta (ms-resource)

Los parámetros de consulta se ignoran durante la recuperación de recursos. La forma normalizada de los parámetros de consulta mantiene las mayúsculas y las minúsculas. Los parámetros de consulta no se ignoran durante la comparación. La comparación de los parámetros de consulta distingue entre mayúsculas y minúsculas.

Los desarrolladores de componentes específicos con capas por encima de este análisis de URI pueden elegir usar los parámetros de consulta como consideren más oportuno.

## <a name="related-topics"></a>Temas relacionados

* [Identificador uniforme de recursos (URI): Sintaxis genérica](http://go.microsoft.com/fwlink/p/?LinkId=263444)
* [Empaquetado de aplicaciones](../packaging/index.md)
* [Hacer referencia a una imagen u otros activos de código y marcado XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Almacenar y recuperar la configuración y otros datos de aplicación](../design/app-settings/store-and-retrieve-app-data.md)
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md)
* [Sistema de administración de recursos](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto.](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)