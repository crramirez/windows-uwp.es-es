---
description: Existen varios esquemas de URI (identificador uniforme de recursos) que puedes usar para hacer referencia a archivos que provienen del paquete de la aplicación, las carpetas de datos de la aplicación o la nube. También puedes usar un esquema de URI para hacer referencia a cadenas cargadas desde archivos de recursos (.resw) de la aplicación.
title: Esquemas de URI
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 8806992ebb7f4335ca0a748c1b2bce4a6de39fae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031557"
---
# <a name="uri-schemes"></a>Esquemas de URI

Existen varios esquemas de URI (identificador uniforme de recursos) que puedes usar para hacer referencia a archivos que provienen del paquete de la aplicación, las carpetas de datos de la aplicación o la nube. También puedes usar un esquema de URI para hacer referencia a cadenas cargadas desde archivos de recursos (.resw) de la aplicación. Puede usar estos esquemas de URI en el código, en el marcado XAML, en el manifiesto del paquete de la aplicación o en las plantillas de notificación del icono y del sistema.

## <a name="common-features-of-the-uri-schemes"></a>Características comunes de los esquemas de URI

Todos los esquemas descritos en este tema siguen las reglas de esquema de URI típicas para la normalización y la recuperación de recursos. Vea [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) para obtener la sintaxis genérica de un URI.

Todos los esquemas de URI definen la parte jerárquica por [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) como los componentes de la autoridad y la ruta de acceso del URI.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Esto significa que hay esencialmente tres componentes en un URI. Inmediatamente después de las dos barras diagonales del *esquema* URI se encuentra un componente (que puede estar vacío) denominado *autoridad* . Y inmediatamente después es la *ruta de acceso* . Tomando el URI `http://www.contoso.com/welcome.png` como ejemplo, el esquema es " `http://` ", la autoridad es " `www.contoso.com` " y la ruta de acceso es " `/welcome.png` ". Otro ejemplo es el URI `ms-appx:///logo.png` , donde los componentes de la entidad están vacíos y toman un valor predeterminado.

El componente de fragmento se omite en el procesamiento específico de esquemas de los URI mencionados en este tema. Durante la comparación y la recuperación de recursos, el componente de fragmento no tiene ningún cojinete. Sin embargo, las capas por encima de la implementación específica pueden interpretar el fragmento para recuperar un recurso secundario.

La comparación tiene lugar byte para byte después de la normalización de todos los componentes IRI.

## <a name="case-insensitivity-and-normalization"></a>Distinción de mayúsculas y minúsculas y normalización

Todos los esquemas de URI descritos en este tema siguen las reglas de URI típicas (RFC 3986) para la normalización y la recuperación de recursos para los esquemas. La forma normalizada de estos URI mantiene los caracteres no reservados de mayúsculas y minúsculas y porcentajes de la RFC 3986.

En todos los esquemas de URI descritos en este tema, el *esquema* , la *autoridad* y la *ruta de acceso* no distinguen entre mayúsculas y minúsculas, o bien el sistema los procesa sin distinción entre mayúsculas y minúsculas. **Nota:** La única excepción a esa regla es la *autoridad* de `ms-resource` , que distingue entre mayúsculas y minúsculas.

## <a name="ms-appx-and-ms-appx-web"></a>MS-appx y MS-appx-Web

Use el `ms-appx` esquema de `ms-appx-web` URI o para hacer referencia a un archivo que procede del paquete de la aplicación (consulte [empaquetar aplicaciones](../packaging/index.md)). Los archivos del paquete de la aplicación suelen ser imágenes estáticas, datos, código y archivos de diseño. El `ms-appx-web` esquema tiene acceso a los mismos archivos que `ms-appx` , pero en el compartimiento Web. Para obtener ejemplos y más información, consulte [hacer referencia a una imagen u otro recurso desde el marcado y el código XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Nombre de esquema (MS-appx y MS-appx-Web)

El nombre de esquema del URI es la cadena "MS-appx" o "MS-appx-Web".

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autoridad (MS-appx y MS-appx-Web)

La autoridad es el nombre de identidad del paquete que se define en el manifiesto del paquete. Por lo tanto, se limita en el formato de URI y IRI (identificador de recursos internacionalizado) al conjunto de caracteres permitido en un nombre de identidad de paquete. El nombre del paquete debe ser el nombre de uno de los paquetes en el gráfico de dependencias del paquete de la aplicación en ejecución actual.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Si aparece cualquier otro carácter en la autoridad, se producirá un error en la recuperación y la comparación. El valor predeterminado de la entidad es el paquete de la aplicación que se está ejecutando actualmente.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Información de usuario y puerto (MS-appx y MS-appx-Web)

El `ms-appx` esquema, a diferencia de otros esquemas populares, no define un componente de información de usuario o de puerto. @" and "Como no se permiten ":" como valores de autoridad válidos, se producirá un error en la búsqueda si se incluyen. Se produce un error en cada uno de los siguientes.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Ruta de acceso (MS-appx y MS-appx-Web)

El componente de ruta de acceso coincide con la sintaxis genérica RFC 3986 y admite caracteres no ASCII en IRIs. El componente de ruta de acceso define la ruta de acceso de archivo lógica o física de un archivo. Dicho archivo se encuentra en una carpeta asociada a la ubicación de instalación del paquete de la aplicación, para la aplicación especificada por la entidad.

Si la ruta de acceso hace referencia a una ruta de acceso física y a un nombre de archivo, se recuperará el recurso de archivo físico. Pero si no se encuentra ningún archivo físico, el recurso real devuelto durante la recuperación se determina mediante la negociación de contenido en tiempo de ejecución. Esta determinación se basa en la configuración de la aplicación, el sistema operativo y el usuario, como el idioma, el factor de escala de la pantalla, el tema, el contraste alto y otros contextos en tiempo de ejecución. Por ejemplo, se puede tener en cuenta una combinación de los idiomas de la aplicación, la configuración de pantalla del sistema y la configuración de contraste alto del usuario al determinar el valor real del recurso que se va a recuperar.

```xml
ms-appx:///images/logo.png
```

El URI anterior puede recuperar realmente un archivo en el paquete de la aplicación actual con el siguiente nombre de archivo físico.

<blockquote>
<pre>
\Images\fr-FR\logo.scale-100_contrast-white.png
</blockquote>
</pre>

También puede recuperar el mismo archivo físico haciendo referencia a él directamente por su nombre completo.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

El componente de ruta de acceso de `ms-appx(-web)` es, como los URI genéricos, con distinción de mayúsculas y minúsculas. Sin embargo, cuando el sistema de archivos subyacente por el que se tiene acceso al recurso no distingue entre mayúsculas y minúsculas, como en el caso de NTFS, la recuperación del recurso se realiza sin distinción de mayúsculas y minúsculas.

La forma normalizada del URI mantiene el uso de mayúsculas y minúsculas, y la descodificación de porcentaje (un símbolo "%" seguido de la representación hexadecimal de dos dígitos) de RFC 3986 caracteres no reservados. Los caracteres "?", "#", "/", "*" y "" "(el carácter de comillas dobles) deben estar codificados por porcentaje en una ruta de acceso para representar datos como nombres de archivo o carpeta. Todos los caracteres codificados por porcentaje están descodificados antes de la recuperación. Por lo tanto, para recuperar un archivo denominado Hello # World.html, use este URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Consulta (MS-appx y MS-appx-Web)

Los parámetros de consulta se omiten durante la recuperación de recursos. La forma normalizada de los parámetros de consulta mantiene el uso de mayúsculas y minúsculas. Los parámetros de consulta no se omiten durante la comparación.

## <a name="ms-appdata"></a>ms-appdata

Use el `ms-appdata` esquema de URI para hacer referencia a los archivos que proceden de las carpetas de datos locales, móviles y temporales de la aplicación. Para obtener más información sobre estas carpetas de datos de la aplicación, vea [almacenar y recuperar la configuración y otros datos](../design/app-settings/store-and-retrieve-app-data.md)de la aplicación.

El `ms-appdata` esquema de URI no realiza la negociación de contenido en tiempo de ejecución que lo hacen [MS-appx y MS-appx-web](#ms-appx-and-ms-appx-web) . Sin embargo, puede responder al contenido de [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) y cargar los recursos adecuados de los datos de la aplicación con su nombre de archivo físico completo en el URI.

### <a name="scheme-name-ms-appdata"></a>Nombre de esquema (MS-AppData)

El nombre de esquema del URI es la cadena "MS-AppData".

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autoridad (MS-AppData)

La autoridad es el nombre de identidad del paquete que se define en el manifiesto del paquete. Por lo tanto, se limita en el formato de URI y IRI (identificador de recursos internacionalizado) al conjunto de caracteres permitido en un nombre de identidad de paquete. El nombre del paquete debe ser el nombre del paquete de la aplicación en ejecución actual.

```xml
ms-appdata://Contoso.MyApp/
```

Si aparece cualquier otro carácter en la autoridad, se producirá un error en la recuperación y la comparación. El valor predeterminado de la entidad es el paquete de la aplicación que se está ejecutando actualmente.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Información de usuario y puerto (MS-AppData)

El `ms-appdata` esquema, a diferencia de otros esquemas populares, no define un componente de información de usuario o de puerto. @" and "Como no se permiten ":" como valores de autoridad válidos, se producirá un error en la búsqueda si se incluyen. Se produce un error en cada uno de los siguientes.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Ruta de acceso (MS-AppData)

El componente de ruta de acceso coincide con la sintaxis genérica RFC 3986 y admite caracteres no ASCII en IRIs. Dentro de la ubicación [Windows. Storage. ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) hay tres carpetas reservadas para el almacenamiento de estado local, móvil y temporal. El `ms-appdata` esquema permite el acceso a los archivos y carpetas de esas ubicaciones. El primer segmento del componente de ruta de acceso debe especificar la carpeta concreta de la siguiente manera. Por lo tanto, la forma "ruta-vacía" de "hier-Part" no es válida.

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

El componente de ruta de acceso de `ms-appdata` es, como los URI genéricos, con distinción de mayúsculas y minúsculas. Sin embargo, cuando el sistema de archivos subyacente por el que se tiene acceso al recurso no distingue entre mayúsculas y minúsculas, como en el caso de NTFS, la recuperación del recurso se realiza sin distinción de mayúsculas y minúsculas.

La forma normalizada del URI mantiene el uso de mayúsculas y minúsculas, y la descodificación de porcentaje (un símbolo "%" seguido de la representación hexadecimal de dos dígitos) de RFC 3986 caracteres no reservados. Los caracteres "?", "#", "/", "*" y "" "(el carácter de comillas dobles) deben estar codificados por porcentaje en una ruta de acceso para representar datos como nombres de archivo o carpeta. Todos los caracteres codificados por porcentaje están descodificados antes de la recuperación. Por lo tanto, para recuperar un archivo local denominado Hello # World.html, use este URI.

```xml
ms-appdata://local/Hello%23World.html
```

La recuperación del recurso y la identificación del segmento de ruta de acceso de nivel superior se controlan después de la normalización de puntos (".. /./b/c"). Por lo tanto, los URI no pueden ser puntos a su vez de una de las carpetas reservadas. Por lo tanto, no se permite el siguiente URI.

```xml
ms-appdata:///local/../hello/logo.png
```

Pero este URI está permitido (aunque redundante).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Consulta (MS-AppData)

Los parámetros de consulta se omiten durante la recuperación de recursos. La forma normalizada de los parámetros de consulta mantiene el uso de mayúsculas y minúsculas. Los parámetros de consulta no se omiten durante la comparación.

## <a name="ms-resource"></a>recurso MS

Use el `ms-resource` esquema de URI para hacer referencia a las cadenas cargadas desde los archivos de recursos de la aplicación (. resw). Para obtener ejemplos y más información sobre los archivos de recursos, consulte [localizar cadenas en la interfaz de usuario y el manifiesto del paquete de la aplicación](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Nombre de esquema (MS-Resource)

El nombre de esquema del URI es la cadena "MS-Resource".

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autoridad (MS-Resource)

La autoridad es el mapa de recursos de nivel superior definido en el índice de recursos del paquete (PRI), que normalmente se corresponde con el nombre de identidad del paquete que se define en el manifiesto del paquete. Vea [empaquetar aplicaciones](../packaging/index.md)). Por lo tanto, se limita en el formato de URI y IRI (identificador de recursos internacionalizado) al conjunto de caracteres permitido en un nombre de identidad de paquete. El nombre del paquete debe ser el nombre de uno de los paquetes en el gráfico de dependencias del paquete de la aplicación en ejecución actual.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Si aparece cualquier otro carácter en la autoridad, se producirá un error en la recuperación y la comparación. El valor predeterminado de la entidad es el nombre del paquete que distingue entre mayúsculas y minúsculas de la aplicación que se está ejecutando actualmente.

```xml
ms-resource:///
```

La autoridad distingue entre mayúsculas y minúsculas, y la forma normalizada mantiene su caso. La búsqueda de un recurso, sin embargo, no distingue entre mayúsculas y minúsculas.

### <a name="user-info-and-port-ms-resource"></a>Información de usuario y puerto (MS-Resource)

El `ms-resource` esquema, a diferencia de otros esquemas populares, no define un componente de información de usuario o de puerto. @" and "Como no se permiten ":" como valores de autoridad válidos, se producirá un error en la búsqueda si se incluyen. Se produce un error en cada uno de los siguientes.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Ruta de acceso (MS-Resource)

La ruta de acceso identifica la ubicación jerárquica del subárbol [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) (consulte [sistema de administración de recursos](/previous-versions/windows/apps/jj552947(v=win.10))) y el [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) en él. Normalmente, se corresponde con el nombre de archivo (sin incluir la extensión) de los archivos de recursos (. resw) y el identificador de un recurso de cadena que contiene.

Para obtener ejemplos y más información, consulte [localizar cadenas en la interfaz de usuario y el manifiesto del paquete de la aplicación](localize-strings-ui-manifest.md) y la [compatibilidad con las notificaciones del icono y del sistema para el idioma, la escala y el contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

El componente de ruta de acceso de `ms-resource` es, como los URI genéricos, con distinción de mayúsculas y minúsculas. Sin embargo, la recuperación subyacente realiza una [comparestringordinal (](/windows/desktop/api/winstring/nf-winstring-windowscomparestringordinal) con *ignoreCase* establecida en `true` .

La forma normalizada del URI mantiene el uso de mayúsculas y minúsculas, y la descodificación de porcentaje (un símbolo "%" seguido de la representación hexadecimal de dos dígitos) de RFC 3986 caracteres no reservados. Los caracteres "?", "#", "/", "*" y "" "(el carácter de comillas dobles) deben estar codificados por porcentaje en una ruta de acceso para representar datos como nombres de archivo o carpeta. Todos los caracteres codificados por porcentaje están descodificados antes de la recuperación. Por lo tanto, para recuperar un recurso de cadena de un archivo de recursos denominado `Hello#World.resw` , use este URI.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Consulta (MS-Resource)

Los parámetros de consulta se omiten durante la recuperación de recursos. La forma normalizada de los parámetros de consulta mantiene el uso de mayúsculas y minúsculas. Los parámetros de consulta no se omiten durante la comparación. Los parámetros de consulta se comparan con distinción de mayúsculas y minúsculas.

Los desarrolladores de componentes concretos superpuestos sobre este análisis de URI pueden optar por usar los parámetros de consulta tal y como se ven.

## <a name="related-topics"></a>Temas relacionados

* [Identificador uniforme de recursos (URI): Sintaxis genérica](https://www.ietf.org/rfc/rfc3986.txt)
* [Empaquetado de aplicaciones](../packaging/index.md)
* [Referencia a una imagen u otro recurso desde el marcado y el código XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Almacenar y recuperar la configuración y otros datos de aplicación](../design/app-settings/store-and-retrieve-app-data.md)
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md)
* [Sistema de administración de recursos](/previous-versions/windows/apps/jj552947(v=win.10))
* [Compatibilidad con las notificaciones de icono y del sistema para el idioma, la escala y el contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
