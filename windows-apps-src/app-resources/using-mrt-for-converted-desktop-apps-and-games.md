---
title: Uso de MRT para juegos y aplicaciones de escritorio convertidos
description: Al empaquetar la aplicación o juego .NET o Win32 como un paquete AppX, puedes aprovechar el sistema de administración de recursos para cargar recursos de una aplicación adaptados al contexto en tiempo de ejecución. Este tema detallado describe las técnicas.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. recursos, juegos, centennial, convertidor de aplicaciones de escritorio, mui, ensamblado satélite
ms.localizationpriority: medium
ms.openlocfilehash: 77cf9444e06920da0eae3ae430fe78c9f5a188ad
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682540"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Usar el sistema de administración de recursos de Windows 10 en una aplicación o juego heredado.

Con frecuencia, las aplicaciones .NET y Win32 están localizadas en diferentes idiomas, para así ampliar el mercado al que pueden dirigirse. Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md). Al empaquetar la aplicación o el juego de .NET o Win32 como un paquete MSIX o AppX, puede aprovechar el sistema de administración de recursos para cargar los recursos de la aplicación adaptados al contexto en tiempo de ejecución. Este tema detallado describe las técnicas.

Existen muchas formas para localizar las aplicaciones Win32 tradicionales, pero Windows 8 introdujo un nuevo [sistema de administración de recursos](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) que funciona entre lenguajes de programación y tipos de aplicaciones, además de proporcionar funcionalidad que va más allá de una simple localización. Este sistema se denominará "MRT" en este tema. Históricamente, eso significaba "Tecnología de recursos modernos", pero ha dejado de usarse el término "Modernos". También se puede llamar al administrador de recursos MRM (administrador de recursos modernos) o PRI (índice de recursos del paquete).

Combinado con la implementación basada en MSIX o AppX (por ejemplo, desde el Microsoft Store), MRT puede ofrecer automáticamente los recursos más aplicables para un usuario o dispositivo determinado, lo que minimiza el tamaño de la descarga e instalación de la aplicación. Esta reducción de tamaño puede ser significativa para las aplicaciones que tienen una gran cantidad de contenido localizado, quizás del orden de varios *gigabytes* para juegos AAA. Entre las ventajas adicionales de MRT se encuentran las listas localizadas en el shell de Windows y en Microsoft Store, una lógica de reserva automática cuando el idioma preferido de un usuario no coincide con los recursos disponibles.

En este documento se describe la arquitectura de nivel superior de MRT y se proporciona una guía de portabilidad para mover las aplicaciones Win32 heredadas a MRT con cambios de código mínimos. Una vez movidas a MRT, el desarrollador dispondrá de ventajas adicionales (por ejemplo, la posibilidad de segmentar recursos por factor de escala o tema del sistema). Ten en cuenta que la localización basada en MRT funciona tanto en aplicaciones para UWP como en aplicaciones Win32 procesadas por el Puente de dispositivo de escritorio (también llamado "Centennial").

En muchos casos, puedes seguir usando los formatos de localización y el código fuente existentes a pesar de la integración con MRT para resolver recursos en tiempo de ejecución y minimizar los tamaños de descarga: no es un enfoque de todo o nada. En la tabla siguiente se resume el trabajo y la relación costo/beneficio estimada de cada etapa. Esta tabla no incluyen tareas que no sean de localización, como proporcionar iconos de aplicación de alta resolución o de contraste alto. Para obtener más información sobre cómo proporcionar múltiples activos para ventanas, iconos, etc., consulta [Adaptar los recursos al idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Work</th>
<th>Ventaja</th>
<th>Costo estimado</th>
</tr>
<tr>
<td>Localizar el manifiesto del paquete</td>
<td>El trabajo mínimo indispensable para que el contenido localizado aparezca en el shell de Windows y en Microsoft Store</td>
<td>Pequeño</td>
</tr>
<tr>
<td>Usar MRT para identificar y localizar recursos</td>
<td>Requisito previo para minimizar los tamaños de descarga e instalación; idioma de reserva automático</td>
<td>Mediano</td>
</tr>
<tr>
<td>Crear paquetes de recursos</td>
<td>Último paso para minimizar los tamaños de descarga e instalación</td>
<td>Pequeño</td>
</tr>
<tr>
<td>Migrar a los formatos de recursos y API de MRT</td>
<td>Tamaños de archivos significativamente menores (en función de la tecnología de recursos existente)</td>
<td>Grandes</td>
</tr>
</table>

## <a name="introduction"></a>Introducción

La mayoría de aplicaciones no triviales contienen elementos de la interfaz de usuario conocidos como *recursos* que están desacoplados del código de la aplicación (a diferencia de los *valores codificados de forma rígida* que se crean en el propio código fuente). Existen varios motivos por los que son preferibles los recursos en lugar de los valores codificados de forma rígida (por ejemplo, son fáciles de editar para las personas que no son desarrolladores), pero uno de los motivos principales es que permiten que la aplicación elija diferentes representaciones del mismo recurso lógico en tiempo de ejecución. Por ejemplo, el texto que aparece en un botón (o la imagen que aparece en un icono) pueden diferir en función de los idiomas que el usuario comprenda, las características del dispositivo de pantalla o si el usuario tiene habilitadas las tecnologías de asistencia.

Por lo tanto, el propósito principal de cualquier tecnología de administración de recursos es traducir, en tiempo de ejecución, una solicitud de un *nombre de recurso* lógico o simbólico (como `SAVE_BUTTON_LABEL`) al mejor *valor* real posible (p. ej., "Guardar") de un conjunto de posibles *candidatos* (p. ej., "Guardar", "Speichern" o "저장"). MRT proporciona esta función y permite que las aplicaciones identifiquen los candidatos de recursos mediante una amplia variedad de atributos, denominados *calificadores*, como el idioma del usuario, el factor de escala de pantalla, el tema seleccionado por el usuario y otros factores del entorno. MRT es compatible incluso con calificadores personalizados para las aplicaciones que lo necesiten (por ejemplo, una aplicación puede proporcionar activos de gráficos diferentes para los usuarios que han iniciado sesión con una cuenta frente a los usuarios invitados, sin agregar explícitamente esta comprobación en cada parte de su aplicación). MRT funciona con recursos de cadenas y recursos basados en archivos, donde los recursos basados en archivos se implementan como referencias a datos externos (los propios archivos).

### <a name="example"></a>Ejemplo

Este es un ejemplo sencillo de una aplicación que tiene etiquetas de texto en dos botones (`openButton` y `saveButton`), así como un archivo PNG que se usa para un logotipo (`logoImage`). Las etiquetas de texto están localizadas en inglés y alemán, y el logotipo está optimizado para pantallas de escritorio normales (factor de escala del 100 %) y teléfonos de alta resolución (factor de escala del 300 %). Ten en cuenta que en este diagrama se presenta una visión conceptual de nivel superior del modelo; no se corresponde exactamente con la implementación:

<p><img src="images\conceptual-resource-model.png"/></p>

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la pseudo-función `GetResource` usa MRT para buscar estos nombres de recursos en la tabla de recursos (llamada archivo PRI) y encuentra el candidato más adecuado en función de las condiciones de entorno (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, se usan las cadenas directamente. En el caso de la imagen de logotipo, las cadenas se interpretan como nombres de archivo y se leen los archivos del disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia "más cercano" en función de un conjunto de reglas de reserva (consulte el [sistema de administración de recursos](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) para más información).

Tenga en cuenta que MRT admite recursos que se adaptan a más de un calificador; por ejemplo, si la imagen del logotipo contenía texto incrustado que también necesitaba localizarse, el logotipo tendría cuatro candidatos: EN/Scale-100, DE/Scale-100, EN/Scale-300 y DE/Scale-300.

### <a name="sections-in-this-document"></a>Secciones de este documento

En las secciones siguientes se describen las tareas de nivel superior necesarias para integrar MRT con la aplicación.

#### <a name="phase-0-build-an-application-package"></a>Fase 0: Compilar un paquete de aplicación

En esta sección se indica cómo compilar la aplicación de escritorio existente como paquete de la aplicación. No se usa ninguna de las características de MRT en esta etapa.

#### <a name="phase-1-localize-the-application-manifest"></a>Fase 1: Localizar el manifiesto de aplicación

En esta sección se describe cómo localizar el manifiesto de la aplicación (para que aparezca correctamente en el shell de Windows), a pesar de que sigas usando el formato y la API de los recursos heredados para empaquetar y localizar los recursos. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: Usar MRT para identificar y localizar recursos

En esta sección se describe cómo modificar el código de la aplicación (y posiblemente el diseño de recursos) para localizar recursos mediante MRT, a pesar de que sigas usando las API y los formatos de los recursos existentes para cargar y consumir los recursos. 

#### <a name="phase-3-build-resource-packs"></a>Fase 3: Crear paquetes de recursos

En esta sección se resumen los cambios finales necesarios para separar los recursos en distintos *paquetes de recursos* y minimizar así el tamaño de la descarga (e instalación) de la aplicación.

### <a name="not-covered-in-this-document"></a>Lo que no se trata en este documento

Después de completar las fases 0-3 anteriores, tendrá una aplicación "bundle" que se puede enviar al Microsoft Store y que minimizará el tamaño de la descarga e instalación de los usuarios al omitir los recursos que no necesitan (por ejemplo, los idiomas que no hablan). Se pueden realizar otras mejoras en el tamaño y la funcionalidad de la aplicación siguiendo un último paso.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Fase 4: Migrar a los formatos de recursos y API de MRT

Esta fase está fuera del ámbito de este documento. implica mover los recursos (especialmente cadenas) desde formatos heredados, como MUI DLL o ensamblados de recursos .NET a archivos PRI. Esto puede ahorrar más espacio para los tamaños de descarga e instalación. También permite usar otras características de MRT, por ejemplo, minimizar la descarga e instalación de los archivos de imagen en función del factor de escala, la configuración de accesibilidad, etc.

## <a name="phase-0-build-an-application-package"></a>Fase 0: Compilar un paquete de aplicación

Antes de realizar cambios en los recursos de la aplicación, tienes que reemplazar la tecnología de empaquetado e instalación actual por la tecnología estándar de empaquetado e implementación de UWP. Hay tres maneras de hacerlo:

* Si tiene una aplicación de escritorio de gran tamaño con un instalador complejo o utiliza muchos puntos de extensibilidad del sistema operativo, puede usar la herramienta de convertidor de aplicaciones de escritorio para generar el diseño del archivo UWP y la información del manifiesto desde el instalador de la aplicación existente (por ejemplo, un MSI).
* Si tiene una aplicación de escritorio más pequeña con relativamente pocos archivos o un instalador simple y sin enlaces de extensibilidad, puede crear manualmente la información del manifiesto y el diseño del archivo.
* Si va a recompilar desde el origen y desea actualizar la aplicación para que sea una aplicación de UWP pura, puede crear un nuevo proyecto en Visual Studio y basarse en el IDE para realizar gran parte del trabajo.

Si desea usar [Desktop App Converter](https://aka.ms/converter), consulte [empaquetar una aplicación de escritorio con el convertidor de aplicaciones de escritorio](https://aka.ms/converterdocs) para obtener más información sobre el proceso de conversión. Puede encontrar un conjunto completo de ejemplos de convertidor de escritorio en [el repositorio de github de puente de escritorio a ejemplos de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Si desea crear manualmente el paquete, tendrá que crear una estructura de directorios que incluya todos los archivos de la aplicación (ejecutables y contenido, pero no código fuente) y un archivo de manifiesto del paquete (. appxmanifest). Puede encontrar un ejemplo en [el ejemplo Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), pero un archivo de manifiesto de paquete básico que ejecuta el ejecutable de escritorio `ContosoDemo.exe` denominado es el siguiente, donde el <span style="background-color: yellow">texto</span> resaltado se reemplazaría por sus propios valores.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

Para obtener más información sobre el archivo de manifiesto del paquete y el diseño del paquete, vea [manifiesto del paquete](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)de la aplicación.

Por último, si usa Visual Studio para crear un nuevo proyecto y migrar el código existente en, vea [crear una aplicación "Hello, World"](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). Puede incluir el código existente en el nuevo proyecto, pero es probable que tenga que realizar cambios significativos en el código (especialmente en la interfaz de usuario) para poder ejecutarlo como una aplicación de UWP pura. Estos cambios están fuera del ámbito de este documento.

## <a name="phase-1-localize-the-manifest"></a>Fase 1: Localizar el manifiesto

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Paso 1,1: Actualización de cadenas & recursos en el manifiesto

En la fase 0 creó un archivo de manifiesto del paquete básico (. appxmanifest) para la aplicación (en función de los valores proporcionados al convertidor, extraídos del MSI o introducidos manualmente en el manifiesto), pero no contendrá información localizada ni admitirá características adicionales como activos del icono de inicio de alta resolución, etc.

Para asegurarse de que el nombre y la descripción de la aplicación están localizados correctamente, debe definir algunos recursos en un conjunto de archivos de recursos y actualizar el manifiesto del paquete para que haga referencia a ellos.

#### <a name="creating-a-default-resource-file"></a>Creación de un archivo de recursos predeterminado

El primer paso es crear un archivo de recursos predeterminado en el idioma predeterminado (p. ej., inglés de Estados Unidos). Puedes hacerlo manualmente con un editor de texto o con el diseñador de recursos en Visual Studio.

Si quieres crear los recursos manualmente:

1. Crea un archivo XML llamado `resources.resw` y colócalo en una subcarpeta `Strings\en-us` del proyecto. Use el código BCP-47 adecuado si el idioma predeterminado no es el Inglés de EE. UU.
2. En el archivo XML, agrega el contenido siguiente, donde el <span style="background-color: yellow">texto resaltado</span> se reemplaza por el texto adecuado para la aplicación, en el idioma predeterminado.

> [!NOTE]
> Existen restricciones en cuanto a las longitudes de algunas de estas cadenas. Consulta [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) para obtener más información.

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Si quieres usar el diseñador de Visual Studio:

1. Cree la `Strings\en-us` carpeta (u otro lenguaje según corresponda) en el proyecto y agregue un **nuevo elemento** a la carpeta raíz del proyecto con `resources.resw`el nombre predeterminado. Asegúrese de elegir el **archivo de recursos (. resw)** y no el **Diccionario de recursos** . un diccionario de recursos es un archivo que usan las aplicaciones XAML.
2. Con el diseñador, escribe las siguientes cadenas (usa el mismo elemento `Names` pero reemplaza `Values` por el texto adecuado para la aplicación):

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> Si empieza con el diseñador de Visual Studio, siempre puede presionar `F7`para editar el XML directamente. Sin embargo, si empiezas con un archivo XML mínimo, *el diseñador no reconocerá el archivo* porque le falta una gran cantidad de metadatos adicionales. Para solucionar este problema, copia la información de XSD reutilizable de un archivo generado por el diseñador en el archivo XML que has editado manualmente.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Actualización del manifiesto para hacer referencia a los recursos

Después de haber definido los valores en el `.resw` archivo, el siguiente paso es actualizar el manifiesto para hacer referencia a las cadenas de recursos. De nuevo, puedes editar un archivo XML directamente o confiar en el diseñador de manifiestos de Visual Studio.

Si editas el XML directamente, abre el archivo `AppxManifest.xml` y realiza los siguientes cambios en los <span style="background-color: lightgreen">valores resaltados</span>; usa este texto *exacto*, no un texto específico para la aplicación. No es obligado usar estos nombres exactos para los recursos; puedes elegir los tuyos propios, pero los que elijas deben coincidir exactamente con lo que aparece en el archivo `.resw`. Estos nombres deben coincidir con el elemento `Names` que creaste en el archivo `.resw`, con el prefijo del esquema `ms-resource:` y el espacio de nombres `Resources/`. 

> [!NOTE]
> Muchos elementos del manifiesto se han omitido de este fragmento de código. no elimine nada.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

Si usa el diseñador de manifiestos de Visual Studio, abra el archivo. appxmanifest y cambie <span style="background-color: lightgreen"></span> los valores resaltados en la pestaña **aplicación* y la pestaña *empaquetado* :

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Paso 1,2: Cree un archivo PRI, cree un paquete MSIX y compruebe que funciona

Ahora ya puedes compilar el archivo `.pri` e implementar la aplicación para comprobar que la información correcta (en el idioma predeterminado) aparece en el menú Inicio.

Si vas a compilar en Visual Studio, simplemente presiona `Ctrl+Shift+B` para compilar el proyecto, haz clic con el botón derecho en el proyecto y elige `Deploy` del menú contextual.

Si va a compilar manualmente, siga estos pasos para crear un archivo `MakePRI` de configuración para la herramienta `.pri` y generar el archivo en sí (se puede encontrar más información en el paquete de la [aplicación manual](/windows/msix/package/manual-packaging-root)):

1. Abra un símbolo del sistema para desarrolladores desde la carpeta **visual studio 2017** o **Visual Studio 2019** en el menú Inicio.
2. Cambie al directorio raíz del proyecto (el que contiene el archivo. appxmanifest y la carpeta **Strings** ).
3. Escribe el comando siguiente, sustituyendo "contoso_demo.xml" por un nombre adecuado para tu proyecto y "en-US" por el idioma predeterminado de la aplicación (o mantén en-US si corresponde). Tenga en cuenta que el archivo XML se crea en el directorio principal (**no** en el directorio del proyecto), ya que no forma parte de la aplicación (puede elegir cualquier otro directorio que desee, pero no olvide sustituirlo en comandos futuros).

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Puedes escribir `makepri createconfig /?` para ver qué hace cada parámetro; pero, en resumen:
      * `/cf`establece el nombre de archivo de configuración (el resultado de este comando).
      * `/dq`establece los calificadores predeterminados, en este caso el idioma.`en-US`
      * `/pv`establece la versión de la plataforma, en este caso Windows 10
      * `/o`establece que se sobrescriba el archivo de salida si existe.

4. Ahora que tienes un archivo de configuración, vuelve a ejecutar `MakePRI` para buscar en el disco los recursos y empaquetarlos en un archivo PRI. Reemplaza "contoso_demop.xml" por el nombre de archivo XML que usaste en el paso anterior y asegúrate de especificar el directorio principal para la entrada y la salida: 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Puedes escribir `makepri new /?` para ver qué hace cada parámetro; pero, en resumen:
      * `/pr`establece la raíz del proyecto (en este caso, el directorio actual).
      * `/cf`establece el nombre de archivo de configuración creado en el paso anterior
      * `/of`establece el archivo de salida. 
      * `/mf`crea un archivo de asignación (para que podamos excluir los archivos del paquete en un paso posterior).
      * `/o`establece que se sobrescriba el archivo de salida si existe.

5. Ahora tienes un archivo `.pri` con los recursos de idioma predeterminados (p. ej., en-US). Para comprobar que funcionan correctamente, puedes ejecutar el comando siguiente:

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Puedes escribir `makepri dump /?` para ver qué hace cada parámetro; pero, en resumen:
      * `/if`establece el nombre de archivo de entrada 
      * `/of`establece el nombre de archivo`.xml` de salida (se anexará automáticamente)
      * `/o`establece que se sobrescriba el archivo de salida si existe.

6. Por último, puedes abrir `..\resources.xml` en un editor de texto y comprobar que se enumeran los valores de `<NamedResource>` (como `ApplicationDescription` y `PublisherDisplayName`) junto con los valores de `<Candidate>` para el idioma predeterminado elegido (habrá otro contenido al principio del archivo; ignóralo por ahora).

Puede abrir el archivo `..\resources.map.txt` de asignación para comprobar que contiene los archivos necesarios para el proyecto (incluido el archivo PRI, que no forma parte del directorio del proyecto). Es importante saber que el archivo de asignación *no* incluirá una referencia a tu fichero `resources.resw` porque el contenido de ese archivo ya se ha incrustado en el archivo PRI. Sin embargo, contendrá otros recursos como los nombres de archivo de las imágenes.

#### <a name="building-and-signing-the-package"></a>Creación y firma del paquete 

Ahora que se ha compilado el archivo PRI, puedes compilar y firmar el paquete:

1. Para crear el paquete de la aplicación, ejecute el siguiente `contoso_demo.appx` comando reemplazando por el nombre del archivo MSIX/appx que quiere crear y asegurándose de elegir un directorio diferente para el archivo (este ejemplo usa el directorio primario; puede ser cualquier lugar, pero debe  **no** ser el directorio del proyecto).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Puedes escribir `makeappx pack /?` para ver qué hace cada parámetro; pero, en resumen:
      * `/m`establece el archivo de manifiesto que se va a usar
      * `/f`establece el archivo de asignación que se va a usar (creado en el paso anterior). 
      * `/p`establece el nombre del paquete de salida.
      * `/o`establece que se sobrescriba el archivo de salida si existe.

2. Una vez creado el paquete, debe estar firmado. La forma más sencilla de obtener un certificado de firma es crear un proyecto de Windows universal vacío en Visual Studio y `.pfx` copiar el archivo que crea, pero puede crear uno manualmente mediante `MakeCert` las `Pvk2Pfx` utilidades y como se describe en [ Cómo crear un certificado de firma de paquete de aplicación](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate).

    > [!IMPORTANT]
    > Si crea manualmente un certificado de firma, asegúrese de colocar los archivos en un directorio diferente al del proyecto de origen o el origen del paquete, de lo contrario podría incluirse como parte del paquete, incluida la clave privada.

3. Para firmar el paquete, usa el comando siguiente. Ten en cuenta que el elemento `Publisher` especificado en el elemento `Identity` de `AppxManifest.xml` debe coincidir con el elemento `Subject` del certificado (**no** es el elemento `<PublisherDisplayName>`, que es el nombre localizado que se muestra a los usuarios). Como es habitual, reemplaza los nombres de archivo `contoso_demo...` por los nombres adecuados para el proyecto y (**muy importante**) asegúrate de que el archivo `.pfx` no está en el directorio actual (de lo contrario se habría ha creado como parte del paquete, incluida la clave privada de firma):

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Puedes escribir `signtool sign /?` para ver qué hace cada parámetro; pero, en resumen:
      * `/fd`establece el algoritmo de Resumen de archivo (SHA256 es el valor predeterminado para AppX).
      * `/a`seleccionará automáticamente el mejor certificado
      * `/f`especifica el archivo de entrada que contiene el certificado de firma.

Por último, puedes hacer doble clic en el archivo `.appx` para instalarlo o, si prefieres la línea de comandos, puedes abrir un símbolo del sistema de PowerShell, ir al directorio que contiene el paquete y escribir lo siguiente (sustituyendo `contoso_demo.appx` por el nombre del paquete):

```CMD
add-appxpackage contoso_demo.appx
```

Si recibes errores que indican que el certificado no es de confianza, asegúrate de que se agrega al almacén del equipo (**no** al almacén del usuario). Para agregar el certificado al almacén del equipo, puedes usar la línea de comandos o el Explorador de Windows.

Para usar la línea de comandos:

1. Ejecute un símbolo del sistema de Visual Studio 2017 o de Visual Studio 2019 como administrador.
2. Cambia al directorio que contiene el archivo `.cer` (recuerda asegurarte de que está fuera de los directorios del proyecto o del origen).
3. Escribe el siguiente comando, sustituyendo `contoso_demo.cer` por el nombre de archivo:
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Puedes ejecutar `certutil -addstore /?` para ver qué hace cada parámetro; pero, en resumen:
      * `-addstore`agrega un certificado a un almacén de certificados.
      * `TrustedPeople`indica el almacén en el que se coloca el certificado

Para usar el Explorador de Windows:

1. Navega hasta la carpeta que contiene el archivo `.pfx`.
2. Haz doble clic en el archivo `.pfx`, debe aparecer **Asistente para importación de certificados**.
3. Elija `Local Machine` y haga clic en`Next`
4. Acepte la solicitud de elevación de administración de control de cuentas de usuario, si aparece, y haga clic en`Next`
5. Escriba la contraseña de la clave privada, si hay alguna, y haga clic en`Next`
6. No`Place all certificates in the following store`
7. Haz clic en `Browse` y elige la carpeta `Trusted People` (**no** "Editores de confianza")
8. Haga `Next` clic en y después`Finish`

Después de agregar el certificado al almacén `Trusted People`, intenta volver a instalar el paquete.

Ahora tu aplicación debería aparecer en la lista "Todas las aplicaciones" del menú Inicio, con la información correcta del archivo `.resw` / `.pri`. Si ves una cadena vacía o la cadena `ms-resource:...`, algo no ha ido bien: vuelve a comprobar las ediciones y asegúrate de que sean correctas. Si haces clic con el botón derecho en la aplicación en el menú Inicio, puedes anclarla como un icono y comprobar que allí también se muestra la información correcta.

### <a name="step-13-add-more-supported-languages"></a>Paso 1,3: Agregar más idiomas admitidos

Una vez realizados los cambios en el manifiesto del paquete y que se `resources.resw` haya creado el archivo inicial, es fácil agregar más idiomas.

#### <a name="create-additional-localized-resources"></a>Crear recursos localizados adicionales

Primero, crea los valores de los recursos localizados adicionales. 

Dentro de la carpeta `Strings`, crea más carpetas para cada idioma compatible usando el código BCP-47 apropiado (por ejemplo, `Strings\de-DE`). En cada una de estas carpetas, crea un archivo `resources.resw` (con un editor XML o el diseñador de Visual Studio) que incluya los valores de los recursos traducidos. Se supone que las cadenas localizadas ya están disponibles en algún lugar y solo tienes que copiarlas en el archivo `.resw`; este documento no explica el paso de traducción en sí mismo. 

Por ejemplo, el archivo `Strings\de-DE\resources.resw` podría tener el siguiente aspecto, con el <span style="background-color: yellow">texto resaltado</span> diferente de `en-US`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

En los siguientes pasos se supone que has agregado recursos para `de-DE` y `fr-FR`, pero se puede seguir el mismo patrón para cualquier idioma.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Actualizar el manifiesto del paquete para enumerar los idiomas admitidos

El manifiesto del paquete debe actualizarse para enumerar los idiomas admitidos por la aplicación. Desktop App Converter agrega el idioma predeterminado, pero los demás deben agregarse explícitamente. Si editas el archivo `AppxManifest.xml` directamente, actualiza el nodo `Resources` de la manera siguiente, agregando todos los elementos que necesites, sustituyendo los <span style="background-color: yellow">idiomas adecuados compatibles</span> y asegurándote de que la primera entrada de la lista sea el idioma predeterminado (de reserva). En este ejemplo, el valor predeterminado es el inglés (Estados Unidos), con compatibilidad adicional con el alemán (Alemania) y el francés (Francia):

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Si usas Visual Studio, no necesitas hacer nada más; si consultas `Package.appxmanifest` verás el valor <span style="background-color: yellow">x-generate</span>, que hace que el proceso de compilación inserte los idiomas que encuentre en el proyecto (en función de las carpetas nombradas con códigos BCP-47). Tenga en cuenta que este no es un valor válido para un manifiesto del paquete real; solo funciona para proyectos de Visual Studio:

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Recompilar con los valores localizados

Ahora puedes volver a compilar e implementar la aplicación y, si cambias la preferencia de idioma en Windows, aparecerán los valores recién localizados en el menú Inicio (las instrucciones sobre cómo cambiar el idioma las encontrarás a continuación).

En Visual Studio, nuevamente puedes usar `Ctrl+Shift+B` para compilar y hacer clic con el botón derecho en el proyecto para `Deploy`.

Si compilas manualmente el proyecto, sigue los pasos anteriores, pero agrega los idiomas adicionales, separados por guiones bajos, a la lista de calificadores predeterminada (`/dq`) al crear el archivo de configuración. Por ejemplo, para la compatibilidad con los recursos en inglés, alemán y francés agregados en el paso anterior:

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Esto creará un archivo PRI que contiene todos los idiomas especificados que puedes usar fácilmente para la prueba. Si el tamaño total de los recursos es pequeño, o solo son compatibles unos pocos idiomas, podría ser aceptable para tu aplicación de envío; solo si quieres disfrutar de las ventajas de la minimización del tamaño de instalación y descarga de los recursos necesitarás realizar algún trabajo adicional para compilar paquetes de idiomas independientes.

#### <a name="test-with-the-localized-values"></a>Probar con los valores localizados

Para probar los nuevos cambios localizados, basta con agregar un nuevo idioma preferido de interfaz de usuario a Windows. No es necesario descargar paquetes de idiomas, reiniciar el sistema o que la interfaz de usuario de Windows completa aparezca en otro idioma. 

1. Ejecuta la aplicación `Settings` (`Windows + I`)
2. Vete a`Time & language`
3. Vete a`Region & language`
4. Hizo`Add a language`
5. Escribe (o selecciona) el idioma que quieras (p. ej., `Deutsch` o `German`)
 * Si hay subidiomas, elige el que quieras (p. ej., `Deutsch / Deutschland`)
6. Selecciona el nuevo idioma en la lista de idiomas
7. Hizo`Set as default`

Ahora abre el menú Inicio y busca la aplicación. Deberías ver los valores localizados para el idioma seleccionado (otras aplicaciones también pueden aparecer localizadas). Si no ves el nombre localizado enseguida, espera unos minutos hasta que se actualice la memoria caché del menú Inicio. Para volver a tu idioma nativo, simplemente haz que sea el idioma predeterminado en la lista de idiomas. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Paso 1,4: Localizar más partes del manifiesto del paquete (opcional)

Se pueden localizar otras secciones del manifiesto del paquete. Por ejemplo, si la aplicación controla extensiones de archivo, necesitas una extensión `windows.fileTypeAssociation` en el manifiesto, indicando el <span style="background-color: lightgreen">texto resaltado en verde</span> exactamente tal como se muestra (ya que hará referencia a recursos) y reemplazando el <span style="background-color: yellow">texto resaltado en amarillo</span> por información específica de la aplicación:

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

También puedes agregar esta información con el diseñador de manifiestos de Visual Studio, en la pestaña `Declarations`, tomando nota de los <span style="background-color: lightgreen">valores resaltados</span>:

<p><img src="images\editing-declarations-info.png"/></p>

Ahora, agrega los nombres de recursos correspondientes a cada uno de los archivos `.resw`, reemplazando el <span style="background-color: yellow">texto resaltado</span> por el texto adecuado para tu aplicación (recuerda que tienes que hacer esto para *cada idioma compatible*):

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

A continuación, este se mostrará en partes del shell de Windows, como el Explorador de archivos:

<p><img src="images\file-type-tool-tip.png"/></p>

Compila y prueba el paquete como antes, empleando los nuevos escenarios que deben mostrar las nuevas cadenas de interfaz de usuario.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: Usar MRT para identificar y localizar recursos

En la sección anterior se mostraba cómo usar MRT para localizar el archivo de manifiesto de la aplicación para que el shell de Windows pueda mostrar correctamente el nombre de la aplicación y otros metadatos. No se necesitaron cambios en el código; simplemente se requirió el uso de archivos `.resw` y algunas herramientas adicionales. En esta sección se mostrará cómo usar MRT para localizar los recursos en los formatos de recursos existentes y con el código de control de recursos existente con unos cambios mínimos.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Suposiciones sobre el diseño de archivos y el código de aplicación existentes

Debido a que hay muchas formas de localizar las aplicaciones de escritorio de Win32, en este documento, para simplificar, haremos algunas suposiciones sobre la estructura existente de la aplicación que tendrás que asignar a tu entorno específico. Es posible que tengas que realizar algunos cambios en el código base o el diseño de recursos existentes para cumplir con los requisitos de MRT. Están completamente fuera del ámbito de este documento.

#### <a name="resource-file-layout"></a>Diseño del archivo de recursos

En este artículo se da por supuesto que todos los recursos localizados tienen los mismos `contoso_demo.exe.mui` nombres de `contoso.strings.xml`archivo (por ejemplo, o), `contoso_strings.dll` pero que se colocan en`en-US`carpetas diferentes con nombres BCP-47 (, `de-DE`, etc.). No importa cuántos archivos de recursos tenga, cuáles son sus nombres, cuáles son los formatos de archivo y las API asociadas, etc. Lo único que importa es que todos los recursos lógicos tienen el mismo nombre de archivo (pero se colocan en un directorio *físico* diferente). 

Como contraejemplo, si la aplicación usa una estructura de archivo plana con un solo directorio `Resources` que contiene los archivos `english_strings.dll` y `french_strings.dll`, no se asignará bien a MRT. Una estructura mejor sería un directorio `Resources` con subdirectorios y archivos `en\strings.dll` y `fr\strings.dll`. También es posible usar el mismo nombre de archivo base, pero con calificadores incrustados, como `strings.lang-en.dll` y `strings.lang-fr.dll`, pero el uso de directorios con códigos de idioma es conceptualmente más sencillo, por lo que es en lo que nos centraremos.

>[!NOTE]
> Todavía es posible usar MRT y las ventajas del empaquetado, incluso si no puede seguir esta Convención de nomenclatura de archivos; solo requiere más trabajo.

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

#### <a name="resource-loading-code"></a>Código de carga de recursos

En este artículo se da por supuesto que en algún momento del código desea localizar el archivo que contiene un recurso localizado, cargarlo y usarlo. Las API que se usan para cargar los recursos, las API usadas para extraer los recursos, etc. no son importantes. En seudocódigo, hay básicamente tres pasos:

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT solo requiere el cambio de los dos primeros pasos de este proceso: cómo determinar los mejores recursos candidato y cómo localizarlos. No requiere que cambies cómo cargar o usar los recursos (aunque proporciona instalaciones para hacerlo si quieres usarlas).

Por ejemplo, la aplicación puede usar la API de Win32 `GetUserPreferredUILanguages`, la función de CRT `sprintf` y la API de Win32 `CreateFile` para reemplazar las tres funciones de seudocódigo anteriores y, luego, analizar manualmente el archivo de texto buscando pares de `name=value`. (Los detalles no son importantes; es simplemente para ilustrar que MRT no tiene ningún efecto en las técnicas usadas para controlar los recursos una vez localizados).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Paso 2,1: Cambios de código para usar MRT para buscar archivos

El cambio del código para usar MRT para localizar recursos no es difícil. Requiere el uso de un conjunto de tipos de WinRT y unas pocas líneas de código. Los tipos principales que usarás son los siguientes:

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), que encapsula el conjunto de valores de calificadores activos actualmente (idioma, factor de escala, etc.).
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (la versión de WinRT, no la versión de .NET), que permite el acceso a todos los recursos del archivo PRI.
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), que representa un subconjunto específico de los recursos en el archivo PRI (en este ejemplo, los recursos basados en archivos frente a los recursos de cadenas).
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), que representa un recurso lógico y todos sus posibles candidatos.
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), que representa un único recurso candidato concreto. 

En seudocódigo, la forma de resolver un nombre de archivo de recurso específico (como `UICommands\ui.txt` en el ejemplo anterior) es la siguiente:

<blockquote>
<pre>
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
</blockquote>
</pre>

Ten en cuenta que el código **no** solicita una carpeta de idioma específico, como `UICommands\en-US\ui.txt`, aunque es el modo en que los archivos existen en el disco. En su lugar, pide el nombre de archivo *lógico*`UICommands\ui.txt` y se basa en MRT para encontrar el archivo en disco adecuado en uno de los directorios de idioma.

A partir de aquí, la aplicación de muestra puede continuar usando `CreateFile` para cargar `absoluteFileName` y analizar los pares de `name=value` igual que antes; no es necesario cambiar nada de esa lógica en la aplicación. Si escribes en C# o C++/CX, el código real no es mucho más complicado que este (y, de hecho, muchas de las variables intermedias pueden evitarse); consulta la sección **Carga de recursos de .NET**, más adelante. Las aplicaciones basadas en C++/WRL serán más complejas debido a las API basadas en COM de nivel inferior que se usan para activar y llamar a las API de WinRT, pero los pasos fundamentales que se siguen son los mismos: consulta la sección **Carga de recursos de Win32 MUI**, más adelante.

#### <a name="loading-net-resources"></a>Carga de recursos de .NET

Debido a que .NET tiene integrado un mecanismo para localizar y cargar recursos (denominados "ensamblados satélite"), no hay ningún código explícito que se tenga que reemplazar como en el ejemplo sintético anterior: en .NET solo se necesitan los archivos DLL de recursos en los directorios adecuados y se localizan automáticamente. Cuando una aplicación se empaqueta como un MSIX o un AppX con paquetes de recursos, la estructura de directorios es algo diferente, en lugar de tener los directorios de recursos como subdirectorios del directorio principal de la aplicación, son del mismo nivel (o no están presentes en absoluto si el usuario no incluye el idioma en sus preferencias). 

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

```csharp
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

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

El tiempo de ejecución de .NET generará el evento `AssemblyResolve` siempre que no pueda encontrar las DLL de recursos y, en este punto, el controlador de eventos proporcionado localizará el archivo deseado a través de MRT y devolverá el ensamblado.

> [!NOTE]
> Si la aplicación ya tiene un `AssemblyResolve` controlador para otros propósitos, tendrá que integrar el código que resuelve los recursos con el código existente.

#### <a name="loading-win32-mui-resources"></a>Carga de recursos de Win32 MUI

La carga de recursos de Win32 MUI es básicamente igual que la carga de ensamblados de satélite de .NET, pero usa el código C++/CX o C++/WRL en su lugar. El uso de C++/CX permite un código mucho más sencillo que coincide estrechamente con el código C# anterior, pero usa las extensiones del lenguaje C++, los modificadores de compilador y sobrecarga de tiempo de ejecución adicional que seguramente querrás evitar. Si es así, C++/WRL proporciona una solución con un impacto mucho menor a costa de código más detallado. No obstante, si estás familiarizado con la programación ATL (o COM en general), WRL te resultará familiar. 

La función de ejemplo siguiente muestra cómo usar C++/WRL para cargar un archivo DLL de recursos específico y devolver un elemento `HINSTANCE` que puede usarse para cargar más recursos mediante la API de recursos de Win32 habitual. Ten en cuenta que, a diferencia de la muestra de C# que inicializa explícitamente el elemento `ResourceContext` con el idioma solicitado por el tiempo de ejecución de .NET, este código se basa en el idioma actual del usuario.

```cpp
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

## <a name="phase-3-building-resource-packs"></a>Fase 3: Crear paquetes de recursos

Ahora que ya tienes un "fat pack" que contiene todos los recursos, hay dos rutas de acceso para la compilación del paquete principal independiente y los paquetes de recursos con el fin de minimizar los tamaños de descarga e instalación:

* Tomar un fat pack existente y ejecutarlo a través de [la herramienta Bundle Generator](https://aka.ms/bundlegen) para crear automáticamente paquetes de recursos. Este es el enfoque preferido si tienes un sistema de compilación que ya produce un fat pack y quieres post-procesarlo para generar paquetes de recursos.
* Producir directamente los paquetes de recursos individuales y compilarlos en un conjunto. Este es el enfoque preferido si tienes más control sobre el sistema de compilación y puedes compilar los paquetes directamente.

### <a name="step-31-creating-the-bundle"></a>Paso 3,1: Crear la agrupación

#### <a name="using-the-bundle-generator-tool"></a>Con la herramienta Bundle Generator

Para poder usar la herramienta Bundle Generator, tienes que actualizar manualmente el archivo de configuración PRI creado para el paquete, para quitar la sección `<packaging>`.

Si usa Visual Studio, consulte para asegurarse de [que los recursos se instalan en un dispositivo independientemente de si un dispositivo los requiere](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)) para obtener información sobre cómo compilar todos los idiomas en el paquete principal `priconfig.packaging.xml` mediante `priconfig.default.xml` la creación de los archivos y .

Si editas manualmente los archivos, sigue estos pasos: 

1. Crea el archivo de configuración de la misma forma que antes, sustituyendo la ruta de acceso, el nombre de archivo y los idiomas por los valores correctos:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Abre manualmente el archivo `.xml` creado y elimina toda la sección `&lt;packaging&rt;` (pero mantén todo lo demás intacto):

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Compila el archivo `.pri` y el paquete `.appx` como antes, con el archivo de configuración actualizado y los nombres de archivo y directorio apropiados (consulta más arriba para información sobre estos comandos):

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. Una vez creado el paquete, use el siguiente comando para crear la agrupación, con los nombres de archivo y directorio adecuados:

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Ahora puede pasar al paso final, firmar (consulte a continuación).

#### <a name="manually-creating-resource-packages"></a>Creación manual de los paquetes de recursos

Crear manualmente los paquetes de recursos requiere la ejecución de un conjunto de comandos ligeramente diferente para crear archivos `.pri` y `.appx` independientes; son similares a los comandos usados anteriormente para crear fat packs, por lo que solo se proporciona una explicación mínima. Nota: Todos los comandos suponen que el directorio actual es el directorio `AppXManifest.xml` que contiene el archivo, pero todos los archivos se colocan en el directorio principal (puede usar un directorio diferente, si es necesario, pero no debe contaminar el directorio del proyecto con ninguno de Estos archivos). Como siempre, reemplaza los nombres de archivo "Contoso" por tus propios nombres de archivo.

1. Usa el siguiente comando para crear un archivo de configuración que nombre **solo** el idioma predeterminado como calificador predeterminado (en este caso, `en-US`):

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Crea los archivos `.pri` y `.map.txt` predeterminados para el paquete principal, además de un conjunto adicional de archivos para cada idioma que se encuentre en el proyecto, con el siguiente comando:

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Usa el siguiente comando para crear el paquete principal (que contiene el código ejecutable y los recursos del idioma predeterminado). Como siempre, cambia el nombre como consideres más oportuno, aunque tienes que colocar el paquete en un directorio separado para que luego sea más fácil crear el conjunto (en este ejemplo se usa el directorio `..\bundle`):

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Después de crear el paquete principal, usa el siguiente comando una vez para cada idioma adicional (es decir, repite este comando para cada archivo de asignación de idioma generado en el paso anterior). De nuevo, la salida debe estar en un directorio independiente (el mismo que el paquete principal). Observa que el idioma se especifica en **ambas** opciones, `/f` y `/p`, y que se usa el nuevo argumento `/r` (que indica que se quiere un paquete de recursos):

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Combina todos los paquetes del directorio del conjunto en un solo archivo `.appxbundle`. La nueva opción `/d` especifica el directorio que se usará para todos los archivos del conjunto (por eso los archivos `.appx` se colocaron en un directorio independiente en el paso anterior):

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

El último paso para compilar el paquete es la firma.

### <a name="step-32-signing-the-bundle"></a>Paso 3,2: Firmar la agrupación

Cuando hayas creado el archivo `.appxbundle` (ya sea mediante la herramienta Bundle Generator o manualmente) tendrás un único archivo que contiene el paquete principal y todos los paquetes de recursos. El paso final consiste en firmar el archivo para que Windows lo instale:

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Esto producirá un archivo `.appxbundle` firmado que contiene el paquete principal, además de todos los paquetes de recursos específicos del idioma. Se puede hacer doble clic en él, como un archivo de paquete, para instalar la aplicación y los idiomas correspondientes en función de las preferencias de idioma de Windows del usuario.

## <a name="related-topics"></a>Temas relacionados

* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](tailor-resources-lang-scale-contrast.md)