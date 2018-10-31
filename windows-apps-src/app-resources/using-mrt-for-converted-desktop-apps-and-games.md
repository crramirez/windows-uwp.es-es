---
author: ptorr-msft
title: Uso de MRT para juegos y aplicaciones de escritorio convertidos
description: Al empaquetar la aplicación o juego .NET o Win32 como un paquete AppX, puedes aprovechar el sistema de administración de recursos para cargar recursos de una aplicación adaptados al contexto de tiempo de ejecución. Este tema detallado describe las técnicas.
ms.author: ptorr
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. recursos, juegos, centennial, convertidor de aplicaciones de escritorio, mui, ensamblado satélite
ms.localizationpriority: medium
ms.openlocfilehash: 927e0c5438ea11b751fba40cb76210d0bce112d4
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5820400"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Usar el sistema de administración de recursos de Windows 10 en una aplicación o juego heredado.

## <a name="overview"></a>Introducción

Con frecuencia, las aplicaciones .NET y Win32 están localizadas en diferentes idiomas, para así ampliar el mercado al que pueden dirigirse. Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md). Al empaquetar la aplicación o juego .NET o Win32 como un paquete AppX, puedes aprovechar el sistema de administración de recursos para cargar recursos de una aplicación adaptados al contexto de tiempo de ejecución. Este tema detallado describe las técnicas.

Existen muchas formas para localizar las aplicaciones Win32 tradicionales, pero Windows 8 introdujo un nuevo [sistema de administración de recursos](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx) que funciona entre lenguajes de programación y tipos de aplicaciones, además de proporcionar funcionalidad que va más allá de una simple localización. Este sistema se denominará "MRT" en este tema. Históricamente, eso significaba "Tecnología de recursos modernos", pero ha dejado de usarse el término "Modernos". También se puede llamar al administrador de recursos MRM (administrador de recursos modernos) o PRI (índice de recursos del paquete).

En combinación con la implementación basada en AppX (por ejemplo, en Microsoft Store), MRT puede entregar automáticamente los recursos que mejor se aplican a un usuario/dispositivo determinado, lo que minimiza los tamaños de descarga e instalación de la aplicación. Esta reducción de tamaño puede ser significativa para las aplicaciones que tienen una gran cantidad de contenido localizado, quizás del orden de varios *gigabytes* para juegos AAA. Entre las ventajas adicionales de MRT se encuentran las listas localizadas en el shell de Windows y en Microsoft Store, una lógica de reserva automática cuando el idioma preferido de un usuario no coincide con los recursos disponibles.

En este documento se describe la arquitectura de nivel superior de MRT y se proporciona una guía de portabilidad para mover las aplicaciones Win32 heredadas a MRT con cambios de código mínimos. Una vez movidas a MRT, el desarrollador dispondrá de ventajas adicionales (por ejemplo, la posibilidad de segmentar recursos por factor de escala o tema del sistema). Ten en cuenta que la localización basada en MRT funciona tanto en aplicaciones para UWP como en aplicaciones Win32 procesadas por el Puente de dispositivo de escritorio (también llamado "Centennial").

En muchos casos, puedes seguir usando los formatos de localización y el código fuente existentes a pesar de la integración con MRT para resolver recursos en tiempo de ejecución y minimizar los tamaños de descarga: no es un enfoque de todo o nada. En la tabla siguiente se resume el trabajo y la relación costo/beneficio estimada de cada etapa. Esta tabla no incluyen tareas que no sean de localización, como proporcionar iconos de aplicación de alta resolución o de contraste alto. Para obtener más información sobre cómo proporcionar múltiples activos para ventanas, iconos, etc., consulta [Adaptar los recursos al idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Trabajo</th>
<th>Ventaja</th>
<th>Costo estimado</th>
</tr>
<tr>
<td>Localizar el manifiesto AppX</td>
<td>El trabajo mínimo indispensable para que el contenido localizado aparezca en el shell de Windows y en Microsoft Store</td>
<td>Pequeña</td>
</tr>
<tr>
<td>Usar MRT para identificar y localizar recursos</td>
<td>Requisito previo para minimizar los tamaños de descarga e instalación; idioma de reserva automático</td>
<td>Medio</td>
</tr>
<tr>
<td>Crear paquetes de recursos</td>
<td>Último paso para minimizar los tamaños de descarga e instalación</td>
<td>Bajo</td>
</tr>
<tr>
<td>Migrar a los formatos de recursos y API de MRT</td>
<td>Tamaños de archivos significativamente menores (en función de la tecnología de recursos existente)</td>
<td>Grande</td>
</tr>
</table>

## <a name="introduction"></a>Introducción

La mayoría de aplicaciones no triviales contienen elementos de la interfaz de usuario conocidos como *recursos* que están desacoplados del código de la aplicación (a diferencia de los *valores codificados de forma rígida* que se crean en el propio código fuente). Existen varios motivos por los que son preferibles los recursos en lugar de los valores codificados de forma rígida (por ejemplo, son fáciles de editar para las personas que no son desarrolladores), pero uno de los motivos principales es que permiten que la aplicación elija diferentes representaciones del mismo recurso lógico en tiempo de ejecución. Por ejemplo, el texto que aparece en un botón (o la imagen que aparece en un icono) pueden diferir en función de los idiomas que el usuario comprenda, las características del dispositivo de pantalla o si el usuario tiene habilitadas las tecnologías de asistencia.

Por lo tanto, el propósito principal de cualquier tecnología de administración de recursos es traducir, en tiempo de ejecución, una solicitud de un *nombre de recurso* lógico o simbólico (como `SAVE_BUTTON_LABEL`) al mejor *valor* real posible (p. ej., "Guardar") de un conjunto de posibles *candidatos* (p. ej., "Guardar", "Speichern" o "저장"). MRT proporciona esta función y permite que las aplicaciones identifiquen los candidatos de recursos mediante una amplia variedad de atributos, denominados *calificadores*, como el idioma del usuario, el factor de escala de pantalla, el tema seleccionado por el usuario y otros factores del entorno. MRT es compatible incluso con calificadores personalizados para las aplicaciones que lo necesiten (por ejemplo, una aplicación puede proporcionar activos de gráficos diferentes para los usuarios que han iniciado sesión con una cuenta frente a los usuarios invitados, sin agregar explícitamente esta comprobación en cada parte de su aplicación). MRT funciona con recursos de cadenas y recursos basados en archivos, donde los recursos basados en archivos se implementan como referencias a datos externos (los propios archivos). 

### <a name="example"></a>Por ejemplo

Este es un ejemplo sencillo de una aplicación que tiene etiquetas de texto en dos botones (`openButton` y `saveButton`), así como un archivo PNG que se usa para un logotipo (`logoImage`). Las etiquetas de texto están localizadas en inglés y alemán, y el logotipo está optimizado para pantallas de escritorio normales (factor de escala del 100%) y teléfonos de alta resolución (factor de escala del 300%). Ten en cuenta que en este diagrama se presenta una visión conceptual de nivel superior del modelo; no se corresponde exactamente con la implementación:

<p><img src="images\conceptual-resource-model.png"/></p>

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la pseudo-función `GetResource` usa MRT para buscar estos nombres de recursos en la tabla de recursos (llamada archivo PRI) y encuentra el candidato más adecuado en función de las condiciones de entorno (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, se usan las cadenas directamente. En el caso de la imagen de logotipo, las cadenas se interpretan como nombres de archivo y se leen los archivos del disco. 

Si el usuario habla un idioma diferente del inglés o alemán, o tiene un factor de escala de pantalla que no es del 100 o 300%, MRT selecciona el candidato coincidente "más próximo", en función de un conjunto de reglas de reserva (consulta [el tema **Sistema de administración de recursos** en MSDN](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx) para más información). 

Ten en cuenta que MRT es compatible con recursos adaptados a más de un calificador, por ejemplo, si la imagen del logotipo contiene texto incrustado que también se tiene que localizar, el logotipo tendrá cuatro candidatos: EN/Scale-100, DE/Scale-100, EN/Scale-300 y DE/Scale-300.

### <a name="sections-in-this-document"></a>Secciones de este documento

En las secciones siguientes se describen las tareas de nivel superior necesarias para integrar MRT con la aplicación.

**Fase 0: compilar un paquete de aplicación**

En esta sección se indica cómo compilar la aplicación de escritorio existente como paquete de la aplicación. No se usa ninguna de las características de MRT en esta etapa.

**Fase 1: localizar el manifiesto de la aplicación**

En esta sección se describe cómo localizar el manifiesto de la aplicación (para que aparezca correctamente en el shell de Windows), a pesar de que sigas usando el formato y la API de los recursos heredados para empaquetar y localizar los recursos. 

**Fase 2: usar MRT para identificar y localizar recursos**

En esta sección se describe cómo modificar el código de la aplicación (y posiblemente el diseño de recursos) para localizar recursos mediante MRT, a pesar de que sigas usando las API y los formatos de los recursos existentes para cargar y consumir los recursos. 

**Fase 3: crear paquetes de recursos**

En esta sección se resumen los cambios finales necesarios para separar los recursos en distintos *paquetes de recursos* y minimizar así el tamaño de la descarga (e instalación) de la aplicación.

### <a name="not-covered-in-this-document"></a>Lo que no se trata en este documento

Después de completar las fases 0 a 3 anteriores, tendrás un "conjunto" de aplicaciones que puede enviarse a Microsoft Store y que minimizará el tamaño de la descarga e instalación para los usuarios, omitiendo los recursos que no necesiten (por ejemplo, idiomas que no hablen). Se pueden realizar otras mejoras en el tamaño y la funcionalidad de la aplicación siguiendo un último paso. 

**Fase 4: migrar a formatos y API de recursos de MRT**

Esta fase está fuera del ámbito de este documento. implica mover los recursos (especialmente cadenas) desde formatos heredados, como MUI DLL o ensamblados de recursos .NET a archivos PRI. Esto puede ahorrar más espacio para los tamaños de descarga e instalación. También permite usar otras características de MRT, por ejemplo, minimizar la descarga e instalación de los archivos de imagen en función del factor de escala, la configuración de accesibilidad, etc.

- - -

## <a name="phase-0-build-an-application-package"></a>Fase 0: compilar un paquete de aplicación

Antes de realizar cambios en los recursos de la aplicación, tienes que reemplazar la tecnología de empaquetado e instalación actual por la tecnología estándar de empaquetado e implementación de UWP. Hay tres maneras de hacerlo:

0. Si tienes una aplicación de escritorio grande con un instalador complejo o usas un gran número de puntos de extensibilidad de sistema operativo, puedes usar la herramienta Desktop App Converter para generar el diseño del archivo UWP y la información del manifiesto desde el instalador de aplicación existente (por ejemplo, un MSI)
0. Si tienes una aplicación de escritorio más pequeña con relativamente pocos archivos o un programa de instalación simple y sin enlaces de extensibilidad, puedes crear el diseño del archivo y la información del manifiesto manualmente.
0. Si vuelves a compilar desde el origen y quieres actualizar la aplicación para sea una aplicación para UWP "pura", puedes crear un nuevo proyecto en Visual Studio y confiar en IDE para que realice la mayor parte del trabajo.

Si quieres usar [Desktop App Converter](https://aka.ms/converter), consulta [el tema **Puente de escritorio a UWP: Desktop App Converter** en MSDN](https://aka.ms/converterdocs) para más información sobre el proceso de conversión. Encontrarás un conjunto completo de ejemplos de Desktop Converter en [el repositorio de GitHub **Desktop app bridge to UWP Samples**](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Si quieres crear manualmente el paquete, tendrás que crear una estructura de directorios que incluya todos los archivos de la aplicación (ejecutables y contenido, pero no código fuente) y un archivo `AppXManifest.xml`. Encontrarás un ejemplo en [la muestra de GitHub de **Hola mundo**](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), pero el siguiente es un archivo `AppXManifest.xml` básico que ejecuta el ejecutable de escritorio denominado `ContosoDemo.exe`, donde tienes que reemplazar el <span style="background-color: yellow">texto resaltado</span> por tus propios valores:

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap"&gt;
    &lt;Identity Name="<span style="background-color: yellow">Contoso.Demo</span>"
              Publisher="<span style="background-color: yellow">CN=Contoso.Demo</span>"
              Version="<span style="background-color: yellow">1.0.0.0</span>" /&gt;
    &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: yellow">Contoso App</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: yellow">Contoso, Inc</span>&lt;/PublisherDisplayName&gt;
    &lt;Logo&gt;Assets\StoreLogo.png&lt;/Logo&gt;
  &lt;/Properties&gt;
    &lt;Dependencies&gt;
    &lt;TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" /&gt;
  &lt;/Dependencies&gt;
    &lt;Resources&gt;
    &lt;Resource Language="<span style="background-color: yellow">en-US</span>" /&gt;
  &lt;/Resources&gt;
    &lt;Applications&gt;
    &lt;Application Id="<span style="background-color: yellow">ContosoDemo</span>" Executable="<span style="background-color: yellow">ContosoDemo.exe</span>" 
                 EntryPoint="Windows.FullTrustApplication"&gt;
    &lt;uap:VisualElements DisplayName="<span style="background-color: yellow">Contoso Demo</span>" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="<span style="background-color: yellow">Contoso Demo</span>"&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
    &lt;Capabilities&gt;
    &lt;rescap:Capability Name="runFullTrust" /&gt;
  &lt;/Capabilities&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Para más información sobre el diseño del archivo y paquete `AppXManifest.xml`, consulta [el tema **Manifiesto del paquete de la aplicación** en MSDN](https://msdn.microsoft.com/en-us/library/windows/apps/br211474.aspx).

Por último, si usas Visual Studio para crear un nuevo proyecto y migrar el código existente, consulta [la documentación de MSDN para crear un nuevo proyecto de UWP](https://msdn.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). Puedes incluir el código existente en el nuevo proyecto, pero probablemente tendrás que realizar cambios significativos en el código (en especial en la interfaz de usuario) para la ejecución como una aplicación para UWP "pura". Estos cambios están fuera del ámbito de este documento.

***

## <a name="phase-1-localize-the-application-manifest"></a>Fase 1: localizar el manifiesto de la aplicación

### <a name="step-11-update-strings--assets-in-the-appxmanifest"></a>Paso 1.1: actualización de cadenas y activos en AppXManifest

En la fase 0 creaste un archivo `AppXManifest.xml` básico para tu aplicación (basado en los valores proporcionados al convertidor, extraídos del archivo MSI o introducidos manualmente en el manifiesto), pero no contendrá información localizada, ni será compatible con características adicionales como activos del icono Inicio de alta resolución. 

Para garantizar que el nombre y la descripción de la aplicación estén localizados correctamente, debes definir algunos recursos en un conjunto de archivos de recursos y actualizar el manifiesto AppX para hacer referencia a ellos.

**Creación de un archivo de recursos predeterminado**

El primer paso es crear un archivo de recursos predeterminado en el idioma predeterminado (p. ej., inglés de Estados Unidos). Puedes hacerlo manualmente con un editor de texto o con el diseñador de recursos en Visual Studio.

Si quieres crear los recursos manualmente:

0. Crea un archivo XML llamado `resources.resw` y colócalo en una subcarpeta `Strings\en-us` del proyecto. 
 * Usa el código BCP-47 adecuado si el idioma predeterminado no es inglés de Estados Unidos. 
0. En el archivo XML, agrega el contenido siguiente, donde el <span style="background-color: yellow">texto resaltado</span> se reemplaza por el texto adecuado para la aplicación, en el idioma predeterminado.

**Nota** Existen restricciones sobre las longitudes de algunas de estas cadenas. Consulta [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements?branch=live) para obtener más información.

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, USA</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (EN)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

Si quieres usar el diseñador de Visual Studio:

0. Crea la carpeta `Strings\en-us` (o de otro idioma si corresponde) en el proyecto y agrega un **Nuevo elemento** a la carpeta raíz del proyecto, con el nombre predeterminado de `resources.resw`
 * Asegúrate de elegir **Archivo de recursos (.resw)** y no **Diccionario de recursos**; un diccionario de recursos es un archivo que usan las aplicaciones XAML
0. Con el diseñador, escribe las siguientes cadenas (usa el mismo elemento `Names` pero reemplaza `Values` por el texto adecuado para la aplicación):

<img src="images\editing-resources-resw.png"/>

Nota: Si empiezas con el diseñador de Visual Studio, siempre puedes editar el XML directamente presionando `F7`. Sin embargo, si empiezas con un archivo XML mínimo, *el diseñador no reconocerá el archivo* porque le falta una gran cantidad de metadatos adicionales. Para solucionar este problema, copia la información de XSD reutilizable de un archivo generado por el diseñador en el archivo XML que has editado manualmente. 

**Actualización del manifiesto para hacer referencia a los recursos**

Una vez definidos los valores en el archivo `.resw`, el siguiente paso es actualizar el manifiesto para que haga referencia a las cadenas de recursos. De nuevo, puedes editar un archivo XML directamente o confiar en el diseñador de manifiestos de Visual Studio.

Si editas el XML directamente, abre el archivo `AppxManifest.xml` y realiza los siguientes cambios en los <span style="background-color: lightgreen">valores resaltados</span>; usa este texto *exacto*, no un texto específico para la aplicación. No es obligado usar estos nombres exactos para los recursos; puedes elegir los tuyos propios, pero los que elijas deben coincidir exactamente con lo que aparece en el archivo `.resw`. Estos nombres deben coincidir con el elemento `Names` que creaste en el archivo `.resw`, con el prefijo del esquema `ms-resource:` y el espacio de nombres `Resources/`. 

*Nota: Muchos de los elementos del manifiesto se han omitido en este fragmento de código, no elimines nada.*

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;Package&gt;
  &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PackageDisplayName</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PublisherDisplayName</span>&lt;/PublisherDisplayName&gt;
  &lt;/Properties&gt;
  &lt;Applications&gt;
    &lt;Application&gt;
      &lt;uap:VisualElements DisplayName="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDisplayName</span>"
        Description="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDescription</span>"&gt;
        &lt;uap:DefaultTile ShortName="<span style="background-color: lightgreen">ms-resource:Resources/TileShortName</span>"&gt;
          &lt;uap:ShowNameOnTiles&gt;
            &lt;uap:ShowOn Tile="square150x150Logo" /&gt;
          &lt;/uap:ShowNameOnTiles&gt;
        &lt;/uap:DefaultTile&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Si usas el diseñador de manifiestos de Visual Studio, abre el archivo `Package.appxmanifest` y cambia los <span style="background-color: lightgreen">valores resaltados</span> de la pestaña `Application` y la pestaña `Packaging`:

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-appx-package-and-verify-its-working"></a>Paso 1.2: compilación de un archivo PRI, creación de un paquete AppX y comprobación del funcionamiento

Ahora ya puedes compilar el archivo `.pri` e implementar la aplicación para comprobar que la información correcta (en el idioma predeterminado) aparece en el menú Inicio. 

Si vas a compilar en Visual Studio, simplemente presiona `Ctrl+Shift+B` para compilar el proyecto, haz clic con el botón derecho en el proyecto y elige `Deploy` del menú contextual. 

Si vas a compilar manualmente, sigue estos pasos para crear un archivo de configuración para la herramienta `MakePRI` y generar el propio archivo `.pri` (puedes encontrar más información en [el tema **Empaquetado manual de la aplicación** en MSDN](https://docs.microsoft.com/en-us/windows/uwp/packaging/manual-packaging-root)):

0. Abre un símbolo del sistema de desarrollador en la carpeta `Visual Studio 2015` del menú Inicio.
0. Cambia al directorio raíz del proyecto (el que contiene el archivo `AppxManifest.xml` y la carpeta `Strings`).
0. Escribe el comando siguiente, sustituyendo "contoso_demo.xml" por un nombre adecuado para tu proyecto y "en-US" por el idioma predeterminado de la aplicación (o mantén en-US si corresponde). Ten en cuenta que el archivo xml se crea en el directorio principal (**no** en el directorio de proyecto) ya que no es parte de la aplicación (puedes elegir cualquier otro directorio que quieras, pero asegúrate de sustituirlo para futuros comandos).

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
```

0. Puedes escribir `makepri createconfig /?` para ver qué hace cada parámetro; pero, en resumen:
 * `/cf` establece el nombre de archivo de configuración (la salida de este comando).
 * `/dq` establece los calificadores predeterminados, en este caso, el idioma `en-US`
 * `/pv` establece la versión de la plataforma, en este caso Windows 10.
 * `/o` lo establece en sobrescribir el archivo de salida, si existe.
0. Ahora que tienes un archivo de configuración, vuelve a ejecutar `MakePRI` para buscar en el disco los recursos y empaquetarlos en un archivo PRI. Reemplaza "contoso_demop.xml" por el nombre de archivo XML que usaste en el paso anterior y asegúrate de especificar el directorio principal para la entrada y la salida: 

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. Puedes escribir `makepri new /?` para ver qué hace cada parámetro; pero, en resumen:
 * `/pr` establece la raíz del proyecto (en este caso, el directorio actual).
 * `/cf` establece el nombre de archivo de configuración, creado en el paso anterior.
 * `/of` establece el archivo de salida. 
 * `/mf` crea un archivo de asignación (de modo que se puedan excluir archivos del paquete en un paso posterior).
 * `/o` lo establece en sobrescribir el archivo de salida, si existe.
0. Ahora tienes un archivo `.pri` con los recursos de idioma predeterminados (p. ej., en-US). Para comprobar que funcionan correctamente, puedes ejecutar el comando siguiente:

    `makepri dump /if ..\resources.pri /of ..\resources /o`
0. Puedes escribir `makepri dump /?` para ver qué hace cada parámetro; pero, en resumen:
 * `/if` establece el nombre de archivo de entrada. 
 * `/of` establece el nombre de archivo de salida (`.xml` se agregará automáticamente).
 * `/o` lo establece en sobrescribir el archivo de salida, si existe.
0. Por último, puedes abrir `..\resources.xml` en un editor de texto y comprobar que se enumeran los valores de `<NamedResource>` (como `ApplicationDescription` y `PublisherDisplayName`) junto con los valores de `<Candidate>` para el idioma predeterminado elegido (habrá otro contenido al principio del archivo; ignóralo por ahora).

Si quieres, puedes abrir el archivo de asignación `..\resources.map.txt` para comprobar que contiene los archivos necesarios para el proyecto (incluido el archivo PRI, que no forma parte del directorio del proyecto). Es importante saber que el archivo de asignación *no* incluirá una referencia a tu fichero `resources.resw` porque el contenido de ese archivo ya se ha incrustado en el archivo PRI. Sin embargo, contendrá otros recursos como los nombres de archivo de las imágenes.

**Creación y firma del paquete**

Ahora que se ha compilado el archivo PRI, puedes compilar y firmar el paquete:

0. Para crear el paquete de la aplicación, ejecuta el siguiente comando reemplazando `contoso_demo.appx` por el nombre del fichero AppX que quieres crear y asegurándote de que eliges un directorio diferente para el fichero `.AppX` (en esta muestra se usa el directorio principal; puede estar cualquier lugar, pero **no** puede ser el directorio del proyecto):

    `makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o`
0. Puedes escribir `makeappx pack /?` para ver qué hace cada parámetro; pero, en resumen:
 * `/m` establece el archivo de manifiesto que se va a usar.
 * `/f` establece el archivo de asignación que se va a usar (creado en el paso anterior). 
 * `/p` establece el nombre del paquete de salida.
 * `/o` lo establece en sobrescribir el archivo de salida, si existe.
0. Una vez creado el paquete, se tiene que firmar. La forma más sencilla de obtener un certificado de firma es crear un proyecto Universal de Windows en Visual Studio y copiando el `.pfx` crea el archivo, pero puedes crear uno manualmente con el `MakeCert` y `Pvk2Pfx` utilidades como se describe en [el **cómo crear un certificado de firma del paquete de aplicación** tema en MSDN] (https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx). 
 * **Importante:** Si creas manualmente un certificado de firma, asegúrate de colocar los archivos en un directorio diferente del proyecto de origen o el origen del paquete, de lo contrario, podría incluirse como parte del paquete, incluida la clave privada.
0. Para firmar el paquete, usa el comando siguiente. Ten en cuenta que el elemento `Publisher` especificado en el elemento `Identity` de `AppxManifest.xml` debe coincidir con el elemento `Subject` del certificado (**no** es el elemento `<PublisherDisplayName>`, que es el nombre localizado que se muestra a los usuarios). Como es habitual, reemplaza los nombres de archivo `contoso_demo...` por los nombres adecuados para el proyecto y (**muy importante**) asegúrate de que el archivo `.pfx` no está en el directorio actual (de lo contrario se habría ha creado como parte del paquete, incluida la clave privada de firma):

    `signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx`
0. Puedes escribir `signtool sign /?` para ver qué hace cada parámetro; pero, en resumen:
 * `/fd` establece el algoritmo de compendio del archivo (SHA256 es el valor predeterminado de AppX).
 * `/a` seleccionará automáticamente el mejor certificado.
 * `/f` especifica el archivo de entrada que contiene el certificado de firma.

Por último, puedes hacer doble clic en el archivo `.appx` para instalarlo o, si prefieres la línea de comandos, puedes abrir un símbolo del sistema de PowerShell, ir al directorio que contiene el paquete y escribir lo siguiente (sustituyendo `contoso_demo.appx` por el nombre del paquete):

```CMD
    add-appxpackage contoso_demo.appx
```

Si recibes errores que indican que el certificado no es de confianza, asegúrate de que se agrega al almacén del equipo (**no** al almacén del usuario). Para agregar el certificado al almacén del equipo, puedes usar la línea de comandos o el Explorador de Windows.

Para usar la línea de comandos:

0. Ejecuta el comando de Visual Studio 2015 como Administrador.
0. Cambia al directorio que contiene el archivo `.cer` (recuerda asegurarte de que está fuera de los directorios del proyecto o del origen).
0. Escribe el siguiente comando, sustituyendo `contoso_demo.cer` por el nombre de archivo:

    `certutil -addstore TrustedPeople contoso_demo.cer`
0. Puedes ejecutar `certutil -addstore /?` para ver qué hace cada parámetro; pero, en resumen:
 * `-addstore` agrega un certificado al almacén de certificados.
 * `TrustedPeople` indica el almacén en el que se coloca el certificado.

Para usar el Explorador de Windows:

0. Navega hasta la carpeta que contiene el archivo `.pfx`.
0. Haz doble clic en el archivo `.pfx`, debe aparecer **Asistente para importación de certificados**.
0. Elige `Local Machine` y haz clic en `Next`
0. Acepta la petición de elevación del administrador de Control de cuentas de usuario, si aparece, y haz clic en `Next`
0. Escribe la contraseña de la clave privada, si existe una, y haz clic en `Next`
0. Selecciona `Place all certificates in the following store`
0. Haz clic en `Browse` y elige la carpeta `Trusted People` (**no** "Editores de confianza")
0. Haz clic en `Next` y, a continuación, en `Finish`

Después de agregar el certificado al almacén `Trusted People`, intenta volver a instalar el paquete.

Ahora tu aplicación debería aparecer en la lista "Todas las aplicaciones" del menú Inicio, con la información correcta del archivo `.resw` / `.pri`. Si ves una cadena vacía o la cadena `ms-resource:...`, algo no ha ido bien: vuelve a comprobar las ediciones y asegúrate de que sean correctas. Si haces clic con el botón derecho en la aplicación en el menú Inicio, puedes anclarla como un icono y comprobar que allí también se muestra la información correcta.

### <a name="step-13-add-more-supported-languages"></a>Paso 1.3: adición de más idiomas compatibles

Una vez realizados los cambios en el manifiesto AppX y después de crear el archivo `resources.resw` inicial, agregar más idiomas es fácil.

**Crear recursos localizados adicionales**

Primero, crea los valores de los recursos localizados adicionales. 

Dentro de la carpeta `Strings`, crea más carpetas para cada idioma compatible usando el código BCP-47 apropiado (por ejemplo, `Strings\de-DE`). En cada una de estas carpetas, crea un archivo `resources.resw` (con un editor XML o el diseñador de Visual Studio) que incluya los valores de los recursos traducidos. Se supone que las cadenas localizadas ya están disponibles en algún lugar y solo tienes que copiarlas en el archivo `.resw`; este documento no explica el paso de traducción en sí mismo. 

Por ejemplo, el archivo `Strings\de-DE\resources.resw` podría tener el siguiente aspecto, con el <span style="background-color: yellow">texto resaltado</span> diferente de `en-US`:

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, DE</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (DE)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

En los siguientes pasos se supone que has agregado recursos para `de-DE` y `fr-FR`, pero se puede seguir el mismo patrón para cualquier idioma.

**Actualizar el manifiesto AppX para la lista de idiomas compatibles**

Hay que actualizar el manifiesto AppX para que liste los idiomas compatibles con la aplicación. Desktop App Converter agrega el idioma predeterminado, pero los demás deben agregarse explícitamente. Si editas el archivo `AppxManifest.xml` directamente, actualiza el nodo `Resources` de la manera siguiente, agregando todos los elementos que necesites, sustituyendo los <span style="background-color: yellow">idiomas adecuados compatibles</span> y asegurándote de que la primera entrada de la lista sea el idioma predeterminado (de reserva). En este ejemplo, el valor predeterminado es el inglés (Estados Unidos), con compatibilidad adicional con el alemán (Alemania) y el francés (Francia):

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">EN-US</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">DE-DE</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">FR-FR</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

Si usas Visual Studio, no necesitas hacer nada más; si consultas `Package.appxmanifest` verás el valor <span style="background-color: yellow">x-generate</span>, que hace que el proceso de compilación inserte los idiomas que encuentre en el proyecto (en función de las carpetas nombradas con códigos BCP-47). Ten en cuenta que no es un valor válido para un manifiesto Appx real; solo funciona para proyectos de Visual Studio:

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">x-generate</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

**Recompilar con los valores localizados**

Ahora puedes volver a compilar e implementar la aplicación y, si cambias la preferencia de idioma en Windows, aparecerán los valores recién localizados en el menú Inicio (las instrucciones sobre cómo cambiar el idioma las encontrarás a continuación).

En Visual Studio, nuevamente puedes usar `Ctrl+Shift+B` para compilar y hacer clic con el botón derecho en el proyecto para `Deploy`.

Si compilas manualmente el proyecto, sigue los pasos anteriores, pero agrega los idiomas adicionales, separados por guiones bajos, a la lista de calificadores predeterminada (`/dq`) al crear el archivo de configuración. Por ejemplo, para la compatibilidad con los recursos en inglés, alemán y francés agregados en el paso anterior:

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Esto creará un archivo PRI que contiene todos los idiomas especificados que puedes usar fácilmente para la prueba. Si el tamaño total de los recursos es pequeño, o solo son compatibles unos pocos idiomas, podría ser aceptable para tu aplicación de envío; solo si quieres disfrutar de las ventajas de la minimización del tamaño de instalación y descarga de los recursos necesitarás realizar algún trabajo adicional para compilar paquetes de idiomas independientes.

**Probar con los valores localizados**

Para probar los nuevos cambios localizados, basta con agregar un nuevo idioma preferido de interfaz de usuario a Windows. No es necesario descargar paquetes de idiomas, reiniciar el sistema o que la interfaz de usuario de Windows completa aparezca en otro idioma. 

0. Ejecuta la aplicación `Settings` (`Windows + I`)
0. Ve a `Time & language`
0. Ve a `Region & language`
0. Haz clic en `Add a language`
0. Escribe (o selecciona) el idioma que quieras (p. ej., `Deutsch` o `German`)
 * Si hay subidiomas, elige el que quieras (p. ej., `Deutsch / Deutschland`)
0. Selecciona el nuevo idioma en la lista de idiomas
0. Haz clic en `Set as default`

Ahora abre el menú Inicio y busca la aplicación. Deberías ver los valores localizados para el idioma seleccionado (otras aplicaciones también pueden aparecer localizadas). Si no ves el nombre localizado enseguida, espera unos minutos hasta que se actualice la memoria caché del menú Inicio. Para volver a tu idioma nativo, simplemente haz que sea el idioma predeterminado en la lista de idiomas. 

### <a name="step-14-localizing-more-parts-of-the-appx-manifest-optional"></a>Paso 1.4: localización de más partes del manifiesto AppX (opcional)

Se pueden localizar otras secciones del manifiesto AppX. Por ejemplo, si la aplicación controla extensiones de archivo, necesitas una extensión `windows.fileTypeAssociation` en el manifiesto, indicando el <span style="background-color: lightgreen">texto resaltado en verde</span> exactamente tal como se muestra (ya que hará referencia a recursos) y reemplazando el <span style="background-color: yellow">texto resaltado en amarillo</span> por información específica de la aplicación:

<blockquote>
<pre>
&lt;Extensions&gt;
  &lt;uap:Extension Category="windows.fileTypeAssociation"&gt;
    &lt;uap:FileTypeAssociation Name="default"&gt;
      &lt;uap:DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeDisplayName</span>&lt;/uap:DisplayName&gt;
      &lt;uap:Logo&gt;<span style="background-color: yellow">Assets\StoreLogo.png</span>&lt;/uap:Logo&gt;
      &lt;uap:InfoTip&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeInfoTip</span>&lt;/uap:InfoTip&gt;
      &lt;uap:SupportedFileTypes&gt;
        &lt;uap:FileType ContentType="<span style="background-color: yellow">application/x-contoso</span>"&gt;<span style="background-color: yellow">.contoso</span>&lt;/uap:FileType&gt;
      &lt;/uap:SupportedFileTypes&gt;
    &lt;/uap:FileTypeAssociation&gt;
  &lt;/uap:Extension&gt;
&lt;/Extensions&gt;
</pre>
</blockquote>

También puedes agregar esta información con el diseñador de manifiestos de Visual Studio, en la pestaña `Declarations`, tomando nota de los <span style="background-color: lightgreen">valores resaltados</span>:

<p><img src="images\editing-declarations-info.png"/></p>

Ahora, agrega los nombres de recursos correspondientes a cada uno de los archivos `.resw`, reemplazando el <span style="background-color: yellow">texto resaltado</span> por el texto adecuado para tu aplicación (recuerda que tienes que hacer esto para *cada idioma compatible*):

<blockquote>
<pre>
... existing content...

&lt;data name="FileTypeDisplayName"&gt;
  &lt;value&gt;<span style="background-color: yellow">Contoso Demo File</span>&lt;/value&gt;
&lt;/data&gt;
&lt;data name="FileTypeInfoTip"&gt;
  &lt;value&gt;<span style="background-color: yellow">Files used by Contoso Demo App</span>&lt;/value&gt;
&lt;/data&gt;
</pre>
</blockquote>

A continuación, este se mostrará en partes del shell de Windows, como el Explorador de archivos:

<p><img src="images\file-type-tool-tip.png"/></p>

Compila y prueba el paquete como antes, empleando los nuevos escenarios que deben mostrar las nuevas cadenas de interfaz de usuario.

- - -

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: usar MRT para identificar y localizar recursos

En la sección anterior se mostraba cómo usar MRT para localizar el archivo de manifiesto de la aplicación para que el shell de Windows pueda mostrar correctamente el nombre de la aplicación y otros metadatos. No se necesitaron cambios en el código; simplemente se requirió el uso de archivos `.resw` y algunas herramientas adicionales. En esta sección se mostrará cómo usar MRT para localizar los recursos en los formatos de recursos existentes y con el código de control de recursos existente con unos cambios mínimos.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Suposiciones sobre el diseño de archivos y el código de aplicación existentes

Debido a que hay muchas formas de localizar las aplicaciones de escritorio de Win32, en este documento, para simplificar, haremos algunas suposiciones sobre la estructura existente de la aplicación que tendrás que asignar a tu entorno específico. Es posible que tengas que realizar algunos cambios en el código base o el diseño de recursos existentes para cumplir con los requisitos de MRT. Están completamente fuera del ámbito de este documento.

**Diseño del archivo de recursos**

En estas notas del producto se supone que los recursos localizados tienen todos los mismos nombres de archivo (p. ej., `contoso_demo.exe.mui`, `contoso_strings.dll` o `contoso.strings.xml`) pero que están colocados en distintas carpetas con nombres BCP-47 (`en-US`, `de-DE`, etc.). Independientemente de cuántos archivos de recursos tengas, cuáles sean sus nombres, cuáles sean los formatos de archivo o API asociadas, etc., lo único importante es que cada recurso *lógico* tenga el mismo nombre de archivo (pero colocado en un directorio *físico* diferente). 

Como contraejemplo, si la aplicación usa una estructura de archivo plana con un solo directorio `Resources` que contiene los archivos `english_strings.dll` y `french_strings.dll`, no se asignará bien a MRT. Una estructura mejor sería un directorio `Resources` con subdirectorios y archivos `en\strings.dll` y `fr\strings.dll`. También es posible usar el mismo nombre de archivo base, pero con calificadores incrustados, como `strings.lang-en.dll` y `strings.lang-fr.dll`, pero el uso de directorios con códigos de idioma es conceptualmente más sencillo, por lo que es en lo que nos centraremos.

**Nota** Sigue pudiéndose usar MRT y las ventajas de empaquetado de AppX, incluso si no se puede seguir esta convención de nombres de archivos; tan solo requerirá más trabajo.

Por ejemplo, la aplicación puede tener un conjunto de comandos de interfaz de usuario personalizados (usados para las etiquetas de los botones, etc.) en un archivo de texto sencillo denominado <span style="background-color: yellow">ui.txt</span>, preparado en una carpeta <span style="background-color: yellow">UICommands</span>:

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

**Código de carga de recursos**

En estas notas del producto se supone que en algún punto del código quieres localizar el archivo que contiene un recurso localizado, cargarlo y, a continuación, volver a usarlo. Las API que se usan para cargar los recursos, las API usadas para extraer los recursos, etc. no son importantes. En seudocódigo, hay básicamente tres pasos:

```
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
```

MRT solo requiere el cambio de los dos primeros pasos de este proceso: cómo determinar los mejores recursos candidato y cómo localizarlos. No requiere que cambies cómo cargar o usar los recursos (aunque proporciona instalaciones para hacerlo si quieres usarlas).
 
Por ejemplo, la aplicación puede usar la API de Win32 `GetUserPreferredUILanguages`, la función de CRT `sprintf` y la API de Win32 `CreateFile` para reemplazar las tres funciones de seudocódigo anteriores y, luego, analizar manualmente el archivo de texto buscando pares de `name=value`. (Los detalles no son importantes; es simplemente para ilustrar que MRT no tiene ningún efecto en las técnicas usadas para controlar los recursos una vez localizados).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Paso 2.1: cambios en el código para usar MRT para localizar archivos

El cambio del código para usar MRT para localizar recursos no es difícil. Requiere el uso de un conjunto de tipos de WinRT y unas pocas líneas de código. Los tipos principales que usarás son los siguientes:

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), que encapsula el conjunto de valores de calificadores activos actualmente (idioma, factor de escala, etc.).
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (la versión de WinRT, no la versión de .NET), que permite el acceso a todos los recursos del archivo PRI.
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), que representa un subconjunto específico de los recursos en el archivo PRI (en este ejemplo, los recursos basados en archivos frente a los recursos de cadenas).
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), que representa un recurso lógico y todos sus posibles candidatos.
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), que representa un único recurso candidato concreto. 

En seudocódigo, la forma de resolver un nombre de archivo de recurso específico (como `UICommands\ui.txt` en el ejemplo anterior) es la siguiente:

```
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
```

Ten en cuenta que el código **no** solicita una carpeta de idioma específico, como `UICommands\en-US\ui.txt`, aunque es el modo en que los archivos existen en el disco. En su lugar, pide el nombre de archivo *lógico* `UICommands\ui.txt` y se basa en MRT para encontrar el archivo en disco adecuado en uno de los directorios de idioma.

A partir de aquí, la aplicación de muestra puede continuar usando `CreateFile` para cargar `absoluteFileName` y analizar los pares de `name=value` igual que antes; no es necesario cambiar nada de esa lógica en la aplicación. Si escribes en C# o C++/CX, el código real no es mucho más complicado que este (y, de hecho, muchas de las variables intermedias pueden evitarse); consulta la sección **Carga de recursos de .NET**, más adelante. Las aplicaciones basadas en C++/WRL serán más complejas debido a las API basadas en COM de nivel inferior que se usan para activar y llamar a las API de WinRT, pero los pasos fundamentales que se siguen son los mismos: consulta la sección **Carga de recursos de Win32 MUI**, más adelante.

**Carga de recursos de .NET**

Debido a que .NET tiene integrado un mecanismo para localizar y cargar recursos (denominados "ensamblados satélite"), no hay ningún código explícito que se tenga que reemplazar como en el ejemplo sintético anterior: en .NET solo se necesitan los archivos DLL de recursos en los directorios adecuados y se localizan automáticamente. Cuando una aplicación se empaqueta como AppX con paquetes de recursos, la estructura de directorios es un poco diferente: en lugar de que los directorios de recursos sean subdirectorios del directorio de la aplicación principal, son del mismo nivel (o no están presentes en absoluto si el idioma no aparece en las preferencias del usuario). 

Por ejemplo, imagina una aplicación .NET con el siguiente diseño, en que todos los archivos existen en la carpeta `MainApp`:

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

Después de la conversión a AppX, el diseño tendrá un aspecto similar al siguiente, suponiendo que `en-US` sea el idioma predeterminado y el usuario tenga el alemán y el francés en su lista de idiomas:

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

Puesto que los recursos localizados ya no existen en subdirectorios debajo de la ubicación de instalación del ejecutable principal, se produce un error en la resolución de recursos de .NET integrados. Por suerte, .NET tiene un mecanismo bien definido para controlar los intentos de carga de ensamblados con errores: el evento `AssemblyResolve`. Es necesario registrar una aplicación .NET que use MRT para este evento y proporcionar el ensamblado que falta para el subsistema de recursos de .NET. 

Un ejemplo conciso de cómo usar las API de WinRT para localizar los ensamblados satélite que .NET usa es el siguiente; el código tal como se presenta está comprimido intencionadamente para mostrar una implementación mínima, aunque puedes ver que se corresponde estrechamente con el seudocódigo anterior y el elemento `ResolveEventArgs` pasado proporciona el nombre del ensamblado que necesitamos localizar. Se puede encontrar una versión ejecutable de este código (con comentarios detallados y control de errores) en el archivo `PriResourceRsolver.cs` de [la muestra sobre la **resolución de ensamblado .NET** en GitHub](https://aka.ms/fvgqt4).

```C#
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with AppX, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Considerando la clase anterior, agregarías lo siguiente en algún lugar al principio del código de inicio de la aplicación (antes de que todos los recursos localizados se necesitaran cargar):

```C#
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

El tiempo de ejecución de .NET generará el evento `AssemblyResolve` siempre que no pueda encontrar las DLL de recursos y, en este punto, el controlador de eventos proporcionado localizará el archivo deseado a través de MRT y devolverá el ensamblado.

**Nota** Si la aplicación ya tiene un controlador `AssemblyResolve` para otros fines, tendrás que integrar el código de resolución de recursos con el código existente.

**Carga de recursos de Win32 MUI**

La carga de recursos de Win32 MUI es básicamente igual que la carga de ensamblados de satélite de .NET, pero usa el código C++/CX o C++/WRL en su lugar. El uso de C++/CX permite un código mucho más sencillo que coincide estrechamente con el código C# anterior, pero usa las extensiones del lenguaje C++, los modificadores de compilador y sobrecarga de tiempo de ejecución adicional que seguramente querrás evitar. Si es así, C++/WRL proporciona una solución con un impacto mucho menor a costa de código más detallado. No obstante, si estás familiarizado con la programación ATL (o COM en general), WRL te resultará familiar. 

La función de ejemplo siguiente muestra cómo usar C++/WRL para cargar un archivo DLL de recursos específico y devolver un elemento `HINSTANCE` que puede usarse para cargar más recursos mediante la API de recursos de Win32 habitual. Ten en cuenta que, a diferencia de la muestra de C# que inicializa explícitamente el elemento `ResourceContext` con el idioma solicitado por el tiempo de ejecución de .NET, este código se basa en el idioma actual del usuario.

```CPP
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>Fase 3: creación de paquetes de recursos

Ahora que ya tienes un "fat pack" que contiene todos los recursos, hay dos rutas de acceso para la compilación del paquete principal independiente y los paquetes de recursos con el fin de minimizar los tamaños de descarga e instalación:

0. Tomar un fat pack existente y ejecutarlo a través de [la herramienta Bundle Generator](https://aka.ms/bundlegen) para crear automáticamente paquetes de recursos. Este es el enfoque preferido si tienes un sistema de compilación que ya produce un fat pack y quieres post-procesarlo para generar paquetes de recursos.
0. Producir directamente los paquetes de recursos individuales y compilarlos en un conjunto. Este es el enfoque preferido si tienes más control sobre el sistema de compilación y puedes compilar los paquetes directamente.

### <a name="step-31-creating-the-bundle"></a>Paso 3.1: creación del conjunto

**Con la herramienta Bundle Generator**

Para poder usar la herramienta Bundle Generator, tienes que actualizar manualmente el archivo de configuración PRI creado para el paquete, para quitar la sección `<packaging>`.

Si usas Visual Studio, consulta [el tema **asegurarse de que los recursos estén instalados...** en MSDN](https://msdn.microsoft.com/en-us/library/dn482043.aspx) para información sobre cómo compilar todos los idiomas en el paquete principal mediante la creación de los archivos `priconfig.packaging.xml` y `priconfig.default.xml`.

Si editas manualmente los archivos, sigue estos pasos: 

0. Crea el archivo de configuración de la misma forma que antes, sustituyendo la ruta de acceso, el nombre de archivo y los idiomas por los valores correctos:

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o`
0. Abre manualmente el archivo `.xml` creado y elimina toda la sección `&lt;packaging&rt;` (pero mantén todo lo demás intacto):

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes" ?&gt; 
&lt;resources targetOsVersion="10.0.0" majorVersion="1"&gt;
  &lt;!-- Packaging section has been deleted... --&gt;
  &lt;index root="\" startIndexAt="\"&gt;
    &lt;default&gt;
    ...
    ...
</pre>
</blockquote>

0. Compila el archivo `.pri` y el paquete `.appx` como antes, con el archivo de configuración actualizado y los nombres de archivo y directorio apropiados (consulta más arriba para información sobre estos comandos):

```CMD
makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
```

0. Una vez creado el paquete, usa el siguiente comando para crear el conjunto, con los nombres de archivo y directorio apropiados:

    `BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo`

Ahora puedes ir al último paso, que es la firma (consulta más adelante).

**Creación manual de los paquetes de recursos**

Crear manualmente los paquetes de recursos requiere la ejecución de un conjunto de comandos ligeramente diferente para crear archivos `.pri` y `.appx` independientes; son similares a los comandos usados anteriormente para crear fat packs, por lo que solo se proporciona una explicación mínima. Nota: En todos los comandos se supone que el directorio actual es el directorio que contiene el archivo `AppXManifest.xml`, pero todos los archivos se colocan en el directorio principal (puedes usar un directorio diferente, si es necesario, pero no tienes que contaminar el directorio del proyecto con ninguno de estos archivos). Como siempre, reemplaza los nombres de archivo "Contoso" por tus propios nombres de archivo.

0. Usa el siguiente comando para crear un archivo de configuración que nombre **solo** el idioma predeterminado como calificador predeterminado (en este caso, `en-US`):

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o`
0. Crea los archivos `.pri` y `.map.txt` predeterminados para el paquete principal, además de un conjunto adicional de archivos para cada idioma que se encuentre en el proyecto, con el siguiente comando:

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. Usa el siguiente comando para crear el paquete principal (que contiene el código ejecutable y los recursos del idioma predeterminado). Como siempre, cambia el nombre como consideres más oportuno, aunque tienes que colocar el paquete en un directorio separado para que luego sea más fácil crear el conjunto (en este ejemplo se usa el directorio `..\bundle`):

    `makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o`
0. Después de crear el paquete principal, usa el siguiente comando una vez para cada idioma adicional (es decir, repite este comando para cada archivo de asignación de idioma generado en el paso anterior). De nuevo, la salida debe estar en un directorio independiente (el mismo que el paquete principal). Observa que el idioma se especifica en **ambas** opciones, `/f` y `/p`, y que se usa el nuevo argumento `/r` (que indica que se quiere un paquete de recursos):

    `makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o`
0. Combina todos los paquetes del directorio del conjunto en un solo archivo `.appxbundle`. La nueva opción `/d` especifica el directorio que se usará para todos los archivos del conjunto (por eso los archivos `.appx` se colocaron en un directorio independiente en el paso anterior):

    `makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o`

El último paso para compilar el paquete es la firma.

### <a name="step-32-signing-the-bundle"></a>Paso 3.2: firma del conjunto

Cuando hayas creado el archivo `.appxbundle` (ya sea mediante la herramienta Bundle Generator o manualmente) tendrás un único archivo que contiene el paquete principal y todos los paquetes de recursos. El paso final consiste en firmar el archivo para que Windows lo instale:

    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle

Esto generará un archivo `.appxbundle` firmado que contiene el paquete principal y todos los paquetes de recursos específicos del idioma. Se puede hacer doble clic en él, como un archivo de paquete, para instalar la aplicación y los idiomas correspondientes en función de las preferencias de idioma de Windows del usuario.

## <a name="related-topics"></a>Temas relacionados

* [Adaptar los recursos al idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md)