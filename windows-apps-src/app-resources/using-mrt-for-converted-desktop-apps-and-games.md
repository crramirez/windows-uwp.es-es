---
title: Usar MRT para aplicaciones y juegos de escritorio convertidos
description: Al empaquetar la aplicación o juego en .NET o Win32 como un paquete .msix o .appx, puedes aprovechar el sistema de administración de recursos para cargar recursos de la aplicación adaptados al contexto en tiempo de ejecución. En este tema se describen en profundidad las técnicas.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP, MRT, PRI. recursos, juegos, Centennial, convertidor de aplicaciones de escritorio, MUI, ensamblado satélite
ms.localizationpriority: medium
ms.openlocfilehash: b86cbcfcc5a6c6284b993dcad1325b108b1ab353
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636495"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Usar el sistema de administración de recursos de Windows 10 en una aplicación o juego heredados

Las aplicaciones y juegos de .NET y Win32 suelen estar localizados en distintos idiomas para expandir su mercado direccionable total. Para más información sobre la propuesta de valor de localizar la aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md). Al empaquetar la aplicación o juego en .NET o Win32 como un paquete .msix o .appx, puedes aprovechar el sistema de administración de recursos para cargar recursos de la aplicación adaptados al contexto en tiempo de ejecución. En este tema se describen en profundidad las técnicas.

Hay muchas maneras de localizar una aplicación Win32 tradicional, pero Windows 8 presentó un [nuevo sistema de administración de recursos](/previous-versions/windows/apps/jj552947(v=win.10)) que funciona a través de lenguajes de programación, a través de tipos de aplicación, y proporciona funcionalidad sobre la localización simple y por encima de ella. En este tema se hará referencia a este sistema como "MRT". Históricamente, eso era la "tecnología de recursos moderna", pero el término "moderno" ya no se incluye. También es posible que el administrador de recursos se conozca como MRM (Administrador de recursos moderno) o PRI (índice de recursos del paquete).

Combinado con la implementación basada en MSIX o. appx (por ejemplo, desde el Microsoft Store), MRT puede ofrecer automáticamente los recursos más aplicables para un usuario o dispositivo determinado, lo que minimiza el tamaño de la descarga e instalación de la aplicación. Esta reducción de tamaño puede ser significativa para las aplicaciones con una gran cantidad de contenido localizado, quizás en el orden de varios *gigabytes* para juegos AAA. Entre las ventajas adicionales de MRT se incluyen las listas localizadas en el shell de Windows y el Microsoft Store, la lógica de reserva automática cuando el idioma preferido de un usuario no coincide con los recursos disponibles.

En este documento se describe la arquitectura de alto nivel de MRT y se proporciona una guía de migración para ayudar a trasladar las aplicaciones Win32 heredadas a MRT con cambios mínimos en el código. Una vez que se mueve a MRT, se ponen a disposición del desarrollador ventajas adicionales (como la capacidad de segmentar recursos por factor de escala o tema del sistema). Tenga en cuenta que la localización basada en MRT funciona para aplicaciones para UWP y aplicaciones Win32 procesadas por el puente de escritorio (también conocido como "Centennial").

En muchas situaciones, puede seguir usando los formatos de localización y el código fuente existentes mientras se integra con MRT para resolver los recursos en tiempo de ejecución y minimizar los tamaños de descarga, no es un enfoque de todo o nada. En la tabla siguiente se resume el trabajo y el costo y la ventaja estimados de cada fase. En esta tabla no se incluyen las tareas que no son de localización, como proporcionar iconos de aplicación de alta resolución o de alto contraste. Para obtener más información sobre cómo proporcionar varios recursos para iconos, iconos, etc., vea [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Trabajo</th>
<th>Prestación</th>
<th>Costo estimado</th>
</tr>
<tr>
<td>Localizar el manifiesto del paquete</td>
<td>Trabajo mínimo necesario para que el contenido localizado aparezca en el shell de Windows y en el Microsoft Store</td>
<td>Pequeño</td>
</tr>
<tr>
<td>Usar MRT para identificar y buscar recursos</td>
<td>Requisito previo para minimizar el tamaño de la descarga e instalación; Reserva de idioma automática</td>
<td>Mediana</td>
</tr>
<tr>
<td>Paquetes de recursos de compilación</td>
<td>Último paso para minimizar los tamaños de descarga e instalación</td>
<td>Pequeño</td>
</tr>
<tr>
<td>Migrar a API y formatos de recursos de MRT</td>
<td>Tamaños de archivo significativamente más pequeños (según la tecnología de recursos existente)</td>
<td>Grande</td>
</tr>
</table>

## <a name="introduction"></a>Introducción

La mayoría de las aplicaciones no triviales contienen elementos de interfaz de usuario conocidos como *recursos* desacoplados del código de la aplicación (en contraposición con *los valores codificados* de forma rígida que se crean en el propio código fuente). Hay varias razones para preferir recursos sobre valores codificados de forma rígida-facilidad de edición por parte de desarrolladores que no son de, por ejemplo, una de las razones principales es permitir que la aplicación Elija distintas representaciones del mismo recurso lógico en tiempo de ejecución. Por ejemplo, el texto que se va a mostrar en un botón (o la imagen que se va a mostrar en un icono) puede variar en función de los idiomas que comprenda el usuario, las características del dispositivo de pantalla o si el usuario tiene habilitadas las tecnologías de asistencia.

Por lo tanto, el propósito principal de cualquier tecnología de administración de recursos es traducir, en tiempo de ejecución, una solicitud de un *nombre de recurso* lógico o simbólico (como `SAVE_BUTTON_LABEL` ) en el mejor *valor* real posible (por ejemplo, "guardar") de un conjunto de *candidatos* posibles (por ejemplo, "Save", "speichern" o "저장"). MRT proporciona este tipo de función y permite a las aplicaciones identificar candidatos de recursos mediante una amplia variedad de atributos, denominados *calificadores*, como el idioma del usuario, el factor de escala de la pantalla, el tema seleccionado del usuario y otros factores del entorno. MRT incluso admite calificadores personalizados para las aplicaciones que lo necesiten (por ejemplo, una aplicación podría proporcionar diferentes recursos gráficos para los usuarios que han iniciado sesión con una cuenta frente a usuarios invitados, sin agregar explícitamente esta comprobación en todas las partes de su aplicación). MRT funciona con recursos de cadena y con recursos basados en archivos, donde los recursos basados en archivos se implementan como referencias a los datos externos (los propios archivos).

### <a name="example"></a>Ejemplo

A continuación se muestra un ejemplo sencillo de una aplicación que tiene etiquetas de texto en dos botones ( `openButton` y `saveButton` ) y un archivo PNG que se usa para un logotipo ( `logoImage` ). Las etiquetas de texto se localizan en inglés y en alemán, y el logotipo está optimizado para pantallas de escritorio normales (factor de escala del 100%) y teléfonos de alta resolución (factor de escala del 300%). Tenga en cuenta que este diagrama presenta una vista conceptual de alto nivel del modelo; no se asigna exactamente a la implementación de.

:::image type="content" source="images\conceptual-resource-model.png" alt-text="Captura de pantalla de una etiqueta de código fuente, una etiqueta de tabla de búsqueda y una etiqueta de archivo en disco.&quot;:::

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la `GetResource` pseudo-función usa MRT para buscar esos nombres de recurso en la tabla de recursos (conocido como archivo PRI) y encontrar el candidato más adecuado en función de las condiciones ambientales (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, las cadenas se utilizan directamente. En el caso de la imagen del logotipo, las cadenas se interpretan como nombres de archivo y los archivos se leen en el disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia &quot;más cercano" en función de un conjunto de reglas de reserva (consulte el [sistema de administración de recursos](/previous-versions/windows/apps/jj552947(v=win.10)) para más información).

Tenga en cuenta que MRT admite recursos que se adaptan a más de un calificador; por ejemplo, si la imagen del logotipo contenía texto incrustado que también necesitaba localizarse, el logotipo tendría cuatro candidatos: EN/Scale-100, DE/Scale-100, EN/Scale-300 y DE/Scale-300.

### <a name="sections-in-this-document"></a>Secciones de este documento

En las secciones siguientes se describen las tareas de alto nivel necesarias para integrar MRT con la aplicación.

#### <a name="phase-0-build-an-application-package"></a>Fase 0: compilar un paquete de aplicación

En esta sección se describe cómo obtener la compilación de la aplicación de escritorio existente como un paquete de aplicación. No se utiliza ninguna característica de MRT en esta fase.

#### <a name="phase-1-localize-the-application-manifest"></a>Fase 1: localizar el manifiesto de aplicación

En esta sección se describe cómo localizar el manifiesto de la aplicación (para que aparezca correctamente en el shell de Windows) mientras se sigue usando el formato de recursos heredado y la API para empaquetar y buscar recursos. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: usar MRT para identificar y buscar recursos

En esta sección se describe cómo modificar el código de la aplicación (y, posiblemente, el diseño de recursos) para localizar recursos mediante MRT, mientras sigue usando los formatos de recursos y las API existentes para cargar y consumir los recursos. 

#### <a name="phase-3-build-resource-packs"></a>Fase 3: creación de paquetes de recursos

En esta sección se describen los cambios finales necesarios para separar los recursos en distintos *paquetes de recursos*, lo que reduce el tamaño de la descarga (e instalación) de la aplicación.

### <a name="not-covered-in-this-document"></a>No se trata en este documento

Después de completar las fases 0-3 anteriores, tendrá una aplicación "bundle" que se puede enviar al Microsoft Store y que minimizará el tamaño de la descarga e instalación de los usuarios al omitir los recursos que no necesitan (por ejemplo, los idiomas que no hablan). Se pueden realizar mejoras adicionales en el tamaño y la funcionalidad de la aplicación realizando un paso final.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Fase 4: migración a las API y los formatos de recursos de MRT

Esta fase está fuera del ámbito de este documento; conlleva el traslado de los recursos (especialmente cadenas) de formatos heredados, como archivos dll de MUI o ensamblados de recursos de .NET, a archivos PRI. Esto puede dar lugar a un mayor ahorro de espacio para la descarga & los tamaños de instalación. También permite el uso de otras características de MRT, como minimizar la descarga e instalación de archivos de imagen según el factor de escala, la configuración de accesibilidad, etc.

## <a name="phase-0-build-an-application-package"></a>Fase 0: compilar un paquete de aplicación

Antes de realizar cambios en los recursos de la aplicación, primero debe reemplazar la tecnología de empaquetado e instalación actual con la tecnología estándar de empaquetado e implementación de UWP. Hay tres formas de hacerlo:

* Si tiene una aplicación de escritorio de gran tamaño con un instalador complejo o utiliza muchos puntos de extensibilidad del sistema operativo, puede usar la herramienta de convertidor de aplicaciones de escritorio para generar el diseño del archivo UWP y la información del manifiesto desde el instalador de la aplicación existente (por ejemplo, un MSI).
* Si tiene una aplicación de escritorio más pequeña con relativamente pocos archivos o un instalador simple y sin enlaces de extensibilidad, puede crear manualmente la información del manifiesto y el diseño del archivo.
* Si va a recompilar desde el origen y desea actualizar la aplicación para que sea una aplicación de UWP pura, puede crear un nuevo proyecto en Visual Studio y basarse en el IDE para realizar gran parte del trabajo.

Si desea usar [Desktop App Converter](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw), consulte [empaquetar una aplicación de escritorio con el convertidor de aplicaciones de escritorio](/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) para obtener más información sobre el proceso de conversión. Puede encontrar un conjunto completo de ejemplos de convertidor de escritorio en [el repositorio de github de puente de escritorio a ejemplos de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Si desea crear manualmente el paquete, tendrá que crear una estructura de directorios que incluya todos los archivos de la aplicación (ejecutables y contenido, pero no código fuente) y un archivo de manifiesto del paquete (. appxmanifest). Puede encontrar un ejemplo en [el ejemplo Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), pero un archivo de manifiesto de paquete básico que ejecuta el ejecutable de escritorio denominado `ContosoDemo.exe` es el siguiente, donde el <span style="background-color: yellow">texto resaltado</span> se reemplazaría por sus propios valores.

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

Para obtener más información sobre el archivo de manifiesto del paquete y el diseño del paquete, vea [manifiesto del paquete](/uwp/schemas/appxpackage/appx-package-manifest)de la aplicación.

Por último, si usa Visual Studio para crear un nuevo proyecto y migrar el código existente en, vea [crear una aplicación "Hello, World"](../get-started/create-a-hello-world-app-xaml-universal.md). Puede incluir el código existente en el nuevo proyecto, pero es probable que tenga que realizar cambios significativos en el código (especialmente en la interfaz de usuario) para poder ejecutarlo como una aplicación de UWP pura. Estos cambios están fuera del ámbito de este documento.

## <a name="phase-1-localize-the-manifest"></a>Fase 1: localizar el manifiesto

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Paso 1,1: actualizar cadenas & recursos en el manifiesto

En la fase 0 creó un archivo de manifiesto del paquete básico (. appxmanifest) para la aplicación (en función de los valores proporcionados para el convertidor, extraído del MSI o introducido manualmente en el manifiesto), pero no contendrá información localizada, ni admitirá características adicionales como activos del icono de inicio de alta resolución, etc.

Para asegurarse de que el nombre y la descripción de la aplicación están localizados correctamente, debe definir algunos recursos en un conjunto de archivos de recursos y actualizar el manifiesto del paquete para que haga referencia a ellos.

#### <a name="creating-a-default-resource-file"></a>Crear un archivo de recursos predeterminado

El primer paso es crear un archivo de recursos predeterminado en el idioma predeterminado (por ejemplo, Inglés de EE. UU.). Puede hacerlo manualmente con un editor de texto o a través del diseñador de recursos en Visual Studio.

Si desea crear los recursos manualmente:

1. Cree un archivo XML denominado `resources.resw` y colóquelo en una `Strings\en-us` subcarpeta del proyecto. Use el código BCP-47 adecuado si el idioma predeterminado no es el Inglés de EE. UU.
2. En el archivo XML, agregue el siguiente contenido, donde el <span style="background-color: yellow">texto resaltado</span> se reemplaza por el texto correspondiente de la aplicación, en el idioma predeterminado.

> [!NOTE]
> Existen restricciones en cuanto a las longitudes de algunas de estas cadenas. Para obtener más información, consulte [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

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

Si desea usar el diseñador en Visual Studio:

1. Cree la `Strings\en-us` carpeta (u otro lenguaje según corresponda) en el proyecto y agregue un **nuevo elemento** a la carpeta raíz del proyecto con el nombre predeterminado `resources.resw` . Asegúrese de elegir el **archivo de recursos (. resw)** y no el **Diccionario de recursos** . un diccionario de recursos es un archivo que usan las aplicaciones XAML.
2. Con el diseñador, escriba las cadenas siguientes (use el mismo, `Names` pero reemplace el `Values` por el texto correspondiente de la aplicación):

:::image type="content" source="images\editing-resources-resw.png" alt-text="Captura de pantalla de una etiqueta de código fuente, una etiqueta de tabla de búsqueda y una etiqueta de archivo en disco.&quot;:::

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la `GetResource` pseudo-función usa MRT para buscar esos nombres de recurso en la tabla de recursos (conocido como archivo PRI) y encontrar el candidato más adecuado en función de las condiciones ambientales (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, las cadenas se utilizan directamente. En el caso de la imagen del logotipo, las cadenas se interpretan como nombres de archivo y los archivos se leen en el disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia &quot;más cercano" :::

> [!NOTE]
> Si empieza con el diseñador de Visual Studio, siempre puede presionar para editar el XML directamente `F7` . Pero si comienza con un archivo XML mínimo, *el diseñador no reconocerá el archivo* porque faltan muchos metadatos adicionales. puede corregirlo copiando la información XSD reutilizable de un archivo generado por el diseñador en el archivo XML editado a mano.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Actualización del manifiesto para hacer referencia a los recursos

Después de haber definido los valores en el `.resw` archivo, el siguiente paso es actualizar el manifiesto para hacer referencia a las cadenas de recursos. De nuevo, puede editar un archivo XML directamente o confiar en el diseñador de manifiestos de Visual Studio.

Si está editando XML directamente, abra el `AppxManifest.xml` archivo y realice los cambios siguientes en los <span style="background-color: lightgreen">valores resaltados</span> : Use este texto *exacto* , no el texto específico de la aplicación. No hay ningún requisito para usar estos nombres de recursos exactos; &mdash; puede elegir el suyo propio, &mdash; pero todo lo que elija debe coincidir exactamente con lo que haya en el `.resw` archivo. Estos nombres deben coincidir `Names` con el que ha creado en el `.resw` archivo, con el prefijo del `ms-resource:` esquema y el `Resources/` espacio de nombres. 

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

Si usa el diseñador de manifiestos de Visual Studio, abra el archivo. appxmanifest y cambie los valores <span style="background-color: lightgreen">resaltados</span> en la pestaña **aplicación* y la pestaña *empaquetado* :

:::image type="content" source="images\editing-application-info.png" alt-text="Captura de pantalla de una etiqueta de código fuente, una etiqueta de tabla de búsqueda y una etiqueta de archivo en disco.&quot;:::

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la `GetResource` pseudo-función usa MRT para buscar esos nombres de recurso en la tabla de recursos (conocido como archivo PRI) y encontrar el candidato más adecuado en función de las condiciones ambientales (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, las cadenas se utilizan directamente. En el caso de la imagen del logotipo, las cadenas se interpretan como nombres de archivo y los archivos se leen en el disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia &quot;más cercano" :::

:::image type="content" source="images\editing-packaging-info.png" alt-text="Captura de pantalla de una etiqueta de código fuente, una etiqueta de tabla de búsqueda y una etiqueta de archivo en disco.&quot;:::

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la `GetResource` pseudo-función usa MRT para buscar esos nombres de recurso en la tabla de recursos (conocido como archivo PRI) y encontrar el candidato más adecuado en función de las condiciones ambientales (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, las cadenas se utilizan directamente. En el caso de la imagen del logotipo, las cadenas se interpretan como nombres de archivo y los archivos se leen en el disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia &quot;más cercano" :::

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Paso 1,2: compilar el archivo PRI, crear un paquete MSIX y comprobar que funciona

Ahora debería poder compilar el `.pri` archivo e implementar la aplicación para comprobar que la información correcta (en el idioma predeterminado) aparece en el menú Inicio.

Si va a compilar en Visual Studio, simplemente presione `Ctrl+Shift+B` para compilar el proyecto y, a continuación, haga clic con el botón derecho en el proyecto y elija en `Deploy` el menú contextual.

Si va a compilar manualmente, siga estos pasos para crear un archivo de configuración para `MakePRI` la herramienta y generar el `.pri` archivo en sí (se puede encontrar más información en el paquete de la [aplicación manual](/windows/msix/package/manual-packaging-root)):

1. Abra un símbolo del sistema para desarrolladores desde la carpeta **visual studio 2017** o **Visual Studio 2019** en el menú Inicio.
2. Cambie al directorio raíz del proyecto (el que contiene el archivo. appxmanifest y la carpeta **Strings** ).
3. Escriba el comando siguiente, reemplazando "contoso_demo.xml" por un nombre adecuado para el proyecto y "en-US" con el idioma predeterminado de la aplicación (o bien, guárdelo en-US, si procede). Tenga en cuenta que el archivo XML se crea en el directorio principal (**no** en el directorio del proyecto), ya que no forma parte de la aplicación (puede elegir cualquier otro directorio que desee, pero no olvide sustituirlo en comandos futuros).

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Puede escribir `makepri createconfig /?` para ver lo que hace cada parámetro, pero en Resumen:
      * `/cf` establece el nombre de archivo de configuración (el resultado de este comando).
      * `/dq` establece los calificadores predeterminados, en este caso el idioma. `en-US`
      * `/pv` establece la versión de la plataforma, en este caso Windows 10
      * `/o` establece que se sobrescriba el archivo de salida si existe.

4. Ahora tiene un archivo de configuración, ejecútelo `MakePRI` de nuevo para buscar recursos en el disco y empaquetarlos en un archivo PRI. Reemplace "contoso_demop.xml" por el nombre de archivo XML que usó en el paso anterior y asegúrese de especificar el directorio principal para la entrada y la salida: 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Puede escribir `makepri new /?` para ver lo que hace cada parámetro, pero en pocas palabras:
      * `/pr` establece la raíz del proyecto (en este caso, el directorio actual).
      * `/cf` establece el nombre de archivo de configuración creado en el paso anterior
      * `/of` establece el archivo de salida. 
      * `/mf` crea un archivo de asignación (para que podamos excluir los archivos del paquete en un paso posterior).
      * `/o` establece que se sobrescriba el archivo de salida si existe.

5. Ahora tiene un `.pri` archivo con los recursos de idioma predeterminados (por ejemplo, en-US). Para comprobar que funcionó correctamente, puede ejecutar el siguiente comando:

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Puede escribir `makepri dump /?` para ver lo que hace cada parámetro, pero en pocas palabras:
      * `/if` establece el nombre de archivo de entrada 
      * `/of` establece el nombre de archivo de salida (se `.xml` anexará automáticamente)
      * `/o` establece que se sobrescriba el archivo de salida si existe.

6. Por último, puede abrir `..\resources.xml` en un editor de texto y comprobar que muestra los `<NamedResource>` valores (como `ApplicationDescription` y `PublisherDisplayName` ) junto con `<Candidate>` los valores del idioma predeterminado elegido (habrá otro contenido al principio del archivo; omitir eso por ahora).

Puede abrir el archivo de asignación `..\resources.map.txt` para comprobar que contiene los archivos necesarios para el proyecto (incluido el archivo PRI, que no forma parte del directorio del proyecto). Lo importante es que el archivo de asignación *no* incluya una referencia al `resources.resw` archivo porque el contenido de ese archivo ya se ha incrustado en el archivo PRI. Sin embargo, contendrá otros recursos, como los nombres de archivo de las imágenes.

#### <a name="building-and-signing-the-package"></a>Compilar y firmar el paquete 

Ahora se crea el archivo PRI, puede compilar y firmar el paquete:

1. Para crear el paquete de la aplicación, ejecute el siguiente comando reemplazando `contoso_demo.appx` por el nombre del archivo. msix/. appx que quiere crear y asegurándose de elegir un directorio diferente para el archivo (en este ejemplo se usa el directorio primario; puede estar en cualquier parte, pero **no** debe ser el directorio del proyecto).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Puede escribir `makeappx pack /?` para ver lo que hace cada parámetro, pero en pocas palabras:
      * `/m` establece el archivo de manifiesto que se va a usar
      * `/f` establece el archivo de asignación que se va a usar (creado en el paso anterior). 
      * `/p` establece el nombre del paquete de salida.
      * `/o` establece que se sobrescriba el archivo de salida si existe.

2. Una vez creado el paquete, debe estar firmado. La forma más fácil de obtener un certificado de firma es crear un proyecto de Windows universal vacío en Visual Studio y copiar el `.pfx` archivo que crea, pero puede crear uno manualmente mediante `MakeCert` las `Pvk2Pfx` utilidades y, como se describe en [Cómo crear un certificado de firma de paquete de aplicación](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate).

    > [!IMPORTANT]
    > Si crea manualmente un certificado de firma, asegúrese de colocar los archivos en un directorio diferente al del proyecto de origen o el origen del paquete, de lo contrario podría incluirse como parte del paquete, incluida la clave privada.

3. Para firmar el paquete, use el siguiente comando. Tenga en cuenta que el `Publisher` especificado en el `Identity` elemento de `AppxManifest.xml` debe coincidir con el `Subject` del certificado (este **no** es el `<PublisherDisplayName>` elemento, que es el nombre para mostrar localizado que se va a mostrar a los usuarios). Como de costumbre, reemplace los `contoso_demo...` nombres de archivo por los que correspondan al proyecto y (**muy importante**) Asegúrese de que el `.pfx` archivo no está en el directorio actual (de lo contrario, se habría creado como parte del paquete, incluida la clave de firma privada):

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Puede escribir `signtool sign /?` para ver lo que hace cada parámetro, pero en pocas palabras:
      * `/fd` establece el algoritmo de Resumen de archivo (SHA256 es el valor predeterminado para. appx).
      * `/a` seleccionará automáticamente el mejor certificado
      * `/f` especifica el archivo de entrada que contiene el certificado de firma.

Por último, ahora puede hacer doble clic en el `.appx` archivo para instalarlo, o bien, si prefiere la línea de comandos, puede abrir un símbolo del sistema de PowerShell, cambiar al directorio que contiene el paquete y escribir lo siguiente (reemplazando `contoso_demo.appx` por el nombre del paquete):

```CMD
add-appxpackage contoso_demo.appx
```

Si recibe errores sobre el certificado que no es de confianza, asegúrese de que se agrega al almacén del equipo (**no** al almacén del usuario). Para agregar el certificado al almacén del equipo, puede usar la línea de comandos o el explorador de Windows.

Para usar la línea de comandos:

1. Ejecute un símbolo del sistema de Visual Studio 2017 o de Visual Studio 2019 como administrador.
2. Cambie al directorio que contiene el `.cer` archivo (Recuerde asegurarse de que está fuera de los directorios de origen o de proyecto).
3. Escriba el comando siguiente, reemplazando `contoso_demo.cer` por el nombre de archivo:
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Puede ejecutar `certutil -addstore /?` para ver lo que hace cada parámetro, pero en pocas palabras:
      * `-addstore` agrega un certificado a un almacén de certificados.
      * `TrustedPeople` indica el almacén en el que se coloca el certificado

Para usar el explorador de Windows:

1. Navegue hasta la carpeta que contiene el `.pfx` archivo.
2. Haga doble clic en el `.pfx` archivo y debería aparecer el **Asistente para importación de Certicicate**
3. Elija `Local Machine` y haga clic en `Next`
4. Acepte la solicitud de elevación de administración de control de cuentas de usuario, si aparece, y haga clic en `Next`
5. Escriba la contraseña de la clave privada, si hay alguna, y haga clic en `Next`
6. Seleccione `Place all certificates in the following store`.
7. Haga clic en `Browse` y elija la `Trusted People` carpeta (**no** "editores de confianza")
8. Haga clic en `Next` y después `Finish`

Después de agregar el certificado al `Trusted People` almacén, intente instalar de nuevo el paquete.

Ahora debería ver que la aplicación aparece en la lista "todas las aplicaciones" del menú Inicio, con la información correcta del `.resw`  /  `.pri` archivo. Si ve una cadena en blanco o la cadena `ms-resource:...` , se ha producido un error, compruebe las ediciones y asegúrese de que son correctas. Si hace clic con el botón derecho en la aplicación en el menú Inicio, puede anclarla como icono y comprobar que también se muestra la información correcta.

### <a name="step-13-add-more-supported-languages"></a>Paso 1,3: agregar más idiomas admitidos

Una vez realizados los cambios en el manifiesto del paquete y que se haya `resources.resw` creado el archivo inicial, es fácil agregar más idiomas.

#### <a name="create-additional-localized-resources"></a>Crear recursos localizados adicionales

En primer lugar, cree los valores de recursos localizados adicionales. 

Dentro de la `Strings` carpeta, cree carpetas adicionales para cada idioma que admita mediante el código BCP-47 adecuado (por ejemplo, `Strings\de-DE` ). Dentro de cada una de estas carpetas, cree un `resources.resw` archivo (mediante un editor XML o el diseñador de Visual Studio) que incluya los valores de recursos traducidos. Se supone que ya tiene las cadenas localizadas disponibles en algún lugar y solo tiene que copiarlas en el `.resw` archivo; este documento no cubre el paso de traducción en sí. 

Por ejemplo, el `Strings\de-DE\resources.resw` archivo podría tener un aspecto similar al siguiente, con el <span style="background-color: yellow">texto resaltado</span> cambiado de `en-US` :

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

En los pasos siguientes se supone que ha agregado recursos para `de-DE` y `fr-FR` , pero se puede seguir el mismo patrón para cualquier lenguaje.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Actualizar el manifiesto del paquete para enumerar los idiomas admitidos

El manifiesto del paquete debe actualizarse para enumerar los idiomas admitidos por la aplicación. El convertidor de aplicaciones de escritorio agrega el idioma predeterminado, pero los demás deben agregarse explícitamente. Si está editando el `AppxManifest.xml` archivo directamente, actualice el `Resources` nodo como se indica a continuación, agregando tantos elementos como necesite y sustituyendo los <span style="background-color: yellow">idiomas adecuados que admita</span> y asegurándose de que la primera entrada de la lista es el idioma predeterminado (fallback). En este ejemplo, el valor predeterminado es inglés (EE. UU.) con compatibilidad adicional para alemán (Alemania) y francés (Francia):

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Si usa Visual Studio, no debe hacer nada; si mira, `Package.appxmanifest` debería ver el valor especial <span style="background-color: yellow">x-Generate</span> , que hace que el proceso de compilación Inserte los idiomas que encuentra en el proyecto (según las carpetas denominadas con códigos BCP-47). Tenga en cuenta que este no es un valor válido para un manifiesto del paquete real; solo funciona para proyectos de Visual Studio:

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Volver a compilar con los valores localizados

Ahora puede compilar e implementar la aplicación, de nuevo, y si cambia la preferencia de idioma en Windows, debería ver que los valores recién localizados aparecen en el menú Inicio (a continuación se proporcionan instrucciones sobre cómo cambiar el idioma).

Para Visual Studio, de nuevo puede usar `Ctrl+Shift+B` para compilar y hacer clic con el botón secundario en el proyecto en `Deploy` .

Si va a compilar el proyecto manualmente, siga los mismos pasos anteriores, pero agregue los idiomas adicionales, separados por un carácter de subrayado, a la lista de calificadores predeterminados ( `/dq` ) al crear el archivo de configuración. Por ejemplo, para admitir los recursos en inglés, alemán y francés agregados en el paso anterior:

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Esto creará un archivo PRI que contiene todos los languagesthat especificados que puede usar fácilmente para las pruebas. Si el tamaño total de los recursos es pequeño, o solo se admite un número reducido de idiomas, es posible que la aplicación de envío sea aceptable. solo si desea tener las ventajas de minimizar el tamaño de instalación o descarga de los recursos que necesita para realizar el trabajo adicional de creación de paquetes de idioma independientes.

#### <a name="test-with-the-localized-values"></a>Prueba con los valores localizados

Para probar los nuevos cambios localizados, solo tiene que agregar un nuevo idioma de interfaz de usuario preferido a Windows. No es necesario descargar paquetes de idioma, reiniciar el sistema o hacer que toda la interfaz de usuario de Windows aparezca en un idioma extranjero. 

1. Ejecutar la `Settings` aplicación ( `Windows + I` )
2. Vaya a `Time & language`.
3. Vaya a `Region & language`.
4. Haga clic en `Add a language`
5. Escriba (o seleccione) el idioma que desee (por ejemplo, `Deutsch` o `German` )
 * Si hay subidiomas, elija el que desee (por ejemplo, `Deutsch / Deutschland` ).
6. Seleccione el nuevo idioma en la lista de idiomas
7. Haga clic en `Set as default`

Ahora, abra el menú Inicio y busque la aplicación y debería ver los valores localizados para el idioma seleccionado (otras aplicaciones también podrían aparecer localizadas). Si no ve el nombre traducido de inmediato, espere unos minutos hasta que se actualice la memoria caché del menú Inicio. Para volver al lenguaje nativo, simplemente conviértalo en el idioma predeterminado en la lista de idiomas. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Paso 1,4: localizar más partes del manifiesto del paquete (opcional)

Se pueden localizar otras secciones del manifiesto del paquete. Por ejemplo, si la aplicación controla extensiones de archivo, debe tener una `windows.fileTypeAssociation` extensión en el manifiesto, utilizando el <span style="background-color: lightgreen">texto resaltado de color verde</span> tal como se muestra (ya que hará referencia a los recursos) y reemplazando el <span style="background-color: yellow">texto resaltado de color amarillo</span> por la información específica de la aplicación:

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

También puede Agregar esta información mediante el diseñador de manifiestos de Visual Studio, mediante la `Declarations` pestaña, tomando nota de los <span style="background-color: lightgreen">valores resaltados</span>:

:::image type="content" source="images\editing-declarations-info.png" alt-text="Captura de pantalla de una etiqueta de código fuente, una etiqueta de tabla de búsqueda y una etiqueta de archivo en disco.&quot;:::

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la `GetResource` pseudo-función usa MRT para buscar esos nombres de recurso en la tabla de recursos (conocido como archivo PRI) y encontrar el candidato más adecuado en función de las condiciones ambientales (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, las cadenas se utilizan directamente. En el caso de la imagen del logotipo, las cadenas se interpretan como nombres de archivo y los archivos se leen en el disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia &quot;más cercano" :::

Ahora, agregue los nombres de recursos correspondientes a cada uno de los `.resw` archivos y reemplace el <span style="background-color: yellow">texto resaltado</span> por el texto correspondiente de la aplicación (Recuerde hacer esto para *cada idioma admitido*):

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

Esto se mostrará en parte del shell de Windows, como el explorador de archivos:

:::image type="content" source="images\file-type-tool-tip.png" alt-text="Captura de pantalla de una etiqueta de código fuente, una etiqueta de tabla de búsqueda y una etiqueta de archivo en disco.&quot;:::

En el gráfico, el código de aplicación hace referencia a los tres nombres de recursos lógicos. En tiempo de ejecución, la `GetResource` pseudo-función usa MRT para buscar esos nombres de recurso en la tabla de recursos (conocido como archivo PRI) y encontrar el candidato más adecuado en función de las condiciones ambientales (el idioma del usuario y el factor de escala de la pantalla). En el caso de las etiquetas, las cadenas se utilizan directamente. En el caso de la imagen del logotipo, las cadenas se interpretan como nombres de archivo y los archivos se leen en el disco. 

Si el usuario habla un idioma distinto del inglés o el alemán, o tiene un factor de escala de pantalla distinto del 100% o 300%, MRT elige el candidato de coincidencia &quot;más cercano":::

Compile y pruebe el paquete como antes, y ejerza cualquier nuevo escenario que muestre las nuevas cadenas de la interfaz de usuario.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: usar MRT para identificar y buscar recursos

En la sección anterior se mostró cómo usar MRT para localizar el archivo de manifiesto de la aplicación de forma que el shell de Windows pueda mostrar correctamente el nombre de la aplicación y otros metadatos. No se requirieron cambios en el código para esto; simplemente se requiere el uso de `.resw` archivos y algunas herramientas adicionales. En esta sección se muestra cómo usar MRT para buscar recursos en los formatos de recursos existentes y usar el código de control de recursos existente con cambios mínimos.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Suposiciones sobre el diseño de archivos existente & código de aplicación

Dado que hay muchas maneras de localizar las aplicaciones de escritorio de Win32, en este documento se simplificarán algunas suposiciones sobre la estructura de la aplicación existente que deberá asignar a su entorno específico. Es posible que tenga que realizar algunos cambios en el código base o en el diseño de los recursos existente para que se ajusten a los requisitos de MRT y que estén en gran medida en el ámbito de este documento.

#### <a name="resource-file-layout"></a>Diseño del archivo de recursos

En este artículo se da por supuesto que todos los recursos localizados tienen los mismos nombres de archivo (por ejemplo, o `contoso_demo.exe.mui` `contoso_strings.dll` ), `contoso.strings.xml` pero que se colocan en carpetas diferentes con nombres BCP-47 ( `en-US` , `de-DE` , etc.). No importa cuántos archivos de recursos tenga, cuáles son sus nombres, cuáles son los formatos de archivo y las API asociadas, etc. Lo único que importa es que todos los recursos *lógicos* tienen el mismo nombre de archivo (pero se colocan en un directorio *físico* diferente). 

Como ejemplo de contador, si la aplicación usa una estructura de archivos sin formato con un solo `Resources` directorio que contiene los archivos `english_strings.dll` y `french_strings.dll` , no se asignará bien a MRT. Una estructura mejor sería un `Resources` directorio con subdirectorios y archivos `en\strings.dll` y `fr\strings.dll` . También es posible usar el mismo nombre de archivo base, pero con calificadores incrustados, como `strings.lang-en.dll` y `strings.lang-fr.dll` , pero el uso de directorios con los códigos de idioma es conceptualmente más sencillo, por lo que nos centraremos en él.

>[!NOTE]
> Todavía es posible usar MRT y las ventajas del empaquetado, incluso si no puede seguir esta Convención de nomenclatura de archivos; solo requiere más trabajo.

Por ejemplo, la aplicación puede tener un conjunto de comandos de interfaz de usuario personalizados (usados para etiquetas de botón, etc.) en un archivo de texto simple denominado <span style="background-color: yellow">ui.txt</span>, que se distribuye en una carpeta <span style="background-color: yellow">UICommands</span> :

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

En este artículo se da por supuesto que en algún momento del código desea localizar el archivo que contiene un recurso localizado, cargarlo y usarlo. Las API que se usan para cargar los recursos, las API usadas para extraer los recursos, etc., no son importantes. En pseudocódigo, hay básicamente tres pasos:

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT solo requiere cambiar los dos primeros pasos de este proceso: cómo se determinan los mejores recursos candidatos y cómo se encuentran. No es necesario cambiar el modo de carga o uso de los recursos (aunque proporciona funciones para hacerlo si desea aprovecharlos).

Por ejemplo, la aplicación podría usar la API de Win32 `GetUserPreferredUILanguages` , la función de CRT `sprintf` y la API `CreateFile` de Win32 para reemplazar las tres funciones de pseudocódigo anteriores y, a continuación, analizar manualmente el archivo de texto para buscar `name=value` pares. (Los detalles no son importantes; esto es simplemente ilustrar que MRT no tiene ningún impacto en las técnicas que se usan para administrar los recursos una vez que se han encontrado).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Paso 2,1: el código cambia para usar MRT para buscar archivos

No es difícil cambiar el código para usar MRT para encontrar recursos. Requiere el uso de una serie de tipos WinRT y unas pocas líneas de código. Los tipos principales que usará son los siguientes:

* [ResourceContext](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), que encapsula el conjunto activo de valores de calificador (idioma, factor de escala, etc.).
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (la versión de WinRT, no la versión de .net), que permite el acceso a todos los recursos del archivo PRI
* [ResourceMap](/uwp/api/windows.applicationmodel.resources.core.resourcemap), que representa un subconjunto específico de los recursos del archivo PRI (en este ejemplo, los recursos basados en archivos y los recursos de cadena)
* [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), que representa un recurso lógico y todos los posibles candidatos
* [ResourceCandidate](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), que representa un único recurso candidato concreto 

En pseudo-Code, la manera de resolver un nombre de archivo de recursos determinado (como `UICommands\ui.txt` en el ejemplo anterior) es la siguiente:

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

Tenga en cuenta que, en concreto, el código **no** solicita una carpeta de lenguaje específico, como `UICommands\en-US\ui.txt` , aunque eso sea el modo en que los archivos existen en el disco. En su lugar, solicita el nombre de archivo *lógico* `UICommands\ui.txt` y se basa en MRT para buscar el archivo en disco adecuado en uno de los directorios de idioma.

Desde aquí, la aplicación de ejemplo podría seguir usando `CreateFile` para cargar `absoluteFileName` y analizar los `name=value` pares exactamente igual que antes; no es necesario cambiar ninguna de esas lógicas en la aplicación. Si está escribiendo en C# o C++/CX, el código real no es mucho más complicado que este (y, en realidad, muchas de las variables intermedias pueden ser eliden). Consulte la sección sobre la **carga de recursos de .net**, más adelante. Las aplicaciones de C++/WRL-based serán más complejas debido a las API de bajo nivel basadas en COM que se usan para activar y llamar a las API de WinRT, pero los pasos fundamentales que se toman son los mismos; vea la sección sobre la **carga de recursos de MUI de Win32**, más adelante.

#### <a name="loading-net-resources"></a>Carga de recursos de .NET

Dado que .NET tiene un mecanismo integrado para buscar y cargar recursos (conocidos como "ensamblados satélite"), no hay código explícito para reemplazar como en el ejemplo sintético anterior. en .NET, solo necesita los archivos dll de recursos en los directorios adecuados y se ubican automáticamente. Cuando una aplicación se empaqueta como un MSIX o un. appx mediante paquetes de recursos, la estructura de directorios es algo diferente, en lugar de tener los directorios de recursos como subdirectorios del directorio principal de la aplicación, son del mismo nivel (o no aparecen en absoluto si el usuario no tiene el idioma indicado en sus preferencias). 

Por ejemplo, imagine una aplicación .NET con el siguiente diseño, donde todos los archivos existen en la `MainApp` carpeta:

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

Después de la conversión a. appx, el diseño tendrá un aspecto similar al siguiente, suponiendo que `en-US` era el idioma predeterminado y que el usuario tiene el alemán y el francés en la lista de idiomas:

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

Dado que los recursos localizados ya no existen en los subdirectorios de la ubicación de instalación del archivo ejecutable principal, se produce un error en la resolución de recursos integrada de .NET. Afortunadamente, .NET tiene un mecanismo bien definido para controlar los intentos de carga de ensamblados con errores: el `AssemblyResolve` evento. Una aplicación .NET que usa MRT debe registrarse para este evento y proporcionar el ensamblado que falta para el subsistema de recursos de .NET. 

A continuación se muestra un ejemplo conciso de cómo usar las API de WinRT para buscar ensamblados satélite utilizados por .NET. el código tal y como se muestra se comprime intencionadamente para mostrar una implementación mínima, aunque puede ver que se asigna estrechamente al pseudo código anterior, y `ResolveEventArgs` que proporciona el nombre del ensamblado que necesitamos encontrar. Puede encontrar una versión ejecutable de este código (con comentarios detallados y control de errores) en el archivo del `PriResourceRsolver.cs` [ejemplo de resolución de **ensamblados .net** en github](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo).

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

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with .appx, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Dada la clase anterior, agregaría lo siguiente en algún momento en el código de inicio de la aplicación (antes de que sea necesario cargar los recursos localizados):

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

El entorno de tiempo de ejecución de .NET generará el `AssemblyResolve` evento siempre que no pueda encontrar los archivos dll de recursos, momento en el cual el controlador de eventos proporcionado buscará el archivo deseado a través de MRT y devolverá el ensamblado.

> [!NOTE]
> Si la aplicación ya tiene un `AssemblyResolve` controlador para otros propósitos, tendrá que integrar el código que resuelve los recursos con el código existente.

#### <a name="loading-win32-mui-resources"></a>Cargar recursos de MUI de Win32

La carga de los recursos de la MUI de Win32 es esencialmente la misma que cargar los ensamblados satélite de .NET, pero con el código C++/CX o c++/WRL en su lugar. El uso de C++/CX permite un código mucho más sencillo que coincide estrechamente con el código de C# anterior, pero usa extensiones del lenguaje C++, modificadores del compilador y un tiempo de ejecución adicional que podría querer evitar. En ese caso, el uso de C++/WRL proporciona una solución mucho más reducida a costa de un código más detallado. No obstante, si está familiarizado con la programación de ATL (o COM en general), WRL debe sentirse familiar. 

La siguiente función de ejemplo muestra cómo usar C++/WRL para cargar un archivo DLL de recursos específico y devolver un `HINSTANCE` que se puede usar para cargar más recursos mediante las API de recursos habituales de Win32. Tenga en cuenta que, a diferencia del ejemplo de C# que inicializa explícitamente `ResourceContext` con el lenguaje solicitado por el tiempo de ejecución de .net, este código se basa en el idioma actual del usuario.

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

## <a name="phase-3-building-resource-packs"></a>Fase 3: creación de paquetes de recursos

Ahora que tiene un "paquete FAT" que contiene todos los recursos, hay dos rutas de acceso para crear paquetes de paquetes y recursos principales independientes con el fin de minimizar los tamaños de descarga e instalación:

* Tome un paquete FAT existente y ejecútelo a través [de la herramienta Generador de paquetes](https://www.microsoft.com/store/apps/9nblggh43pmq) para crear paquetes de recursos automáticamente. Este es el enfoque preferido si tiene un sistema de compilación que ya produce un paquete FAT y desea hacerlo después para generar los paquetes de recursos.
* Generar directamente los paquetes de recursos individuales y compilarlos en una agrupación. Este es el enfoque preferido si tiene más control sobre el sistema de compilación y puede compilar los paquetes directamente.

### <a name="step-31-creating-the-bundle"></a>Paso 3,1: creación del paquete

#### <a name="using-the-bundle-generator-tool"></a>Usar la herramienta Generador de agrupación

Para poder usar la herramienta Generador de agrupación, el archivo de configuración de PRI creado para el paquete debe actualizarse manualmente para quitar la `<packaging>` sección.

Si usa Visual Studio, consulte para asegurarse de [que los recursos se instalan en un dispositivo independientemente de si un dispositivo los requiere](/previous-versions/dn482043(v=vs.140)) para obtener información sobre cómo compilar todos los idiomas en el paquete principal mediante la creación de los archivos `priconfig.packaging.xml` y `priconfig.default.xml` .

Si está editando archivos manualmente, siga estos pasos: 

1. Cree el archivo de configuración de la misma manera que antes, sustituyendo la ruta de acceso, el nombre de archivo y los idiomas correctos:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Abra manualmente el `.xml` archivo creado y elimine toda la `&lt;packaging&rt;` sección (pero mantenga todo lo demás intacto):

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Compile el `.pri` archivo y el `.appx` paquete como antes, utilizando el archivo de configuración actualizado y los nombres de archivo y directorio adecuados (vea más arriba para obtener más información sobre estos comandos):

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. Una vez creado el paquete, use el siguiente comando para crear la agrupación, con los nombres de archivo y directorio adecuados:

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Ahora puede pasar al paso final, firmar (consulte a continuación).

#### <a name="manually-creating-resource-packages"></a>Creación manual de paquetes de recursos

La creación manual de paquetes de recursos requiere la ejecución de un conjunto de comandos ligeramente diferente para compilar `.pri` archivos independientes y `.appx` , que son similares a los comandos usados anteriormente para crear paquetes FAT, por lo que se proporciona una explicación mínima. Nota: todos los comandos suponen que el directorio actual es el directorio que contiene el `AppXManifest.xml` archivo, pero todos los archivos se colocan en el directorio principal (puede usar un directorio diferente, si es necesario, pero no debe contaminar el directorio del proyecto con ninguno de estos archivos). Como siempre, reemplace los nombres de archivo "Contoso" por los suyos propios.

1. Use el siguiente comando para crear un archivo de configuración que nombra **solo** el idioma predeterminado como calificador predeterminado; en este caso, `en-US` :

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Cree un archivo y un archivo predeterminados `.pri` `.map.txt` para el paquete principal, además de un conjunto adicional de archivos para cada idioma que se encuentre en el proyecto, con el siguiente comando:

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Use el comando siguiente para crear el paquete principal (que contiene el código ejecutable y los recursos de idioma predeterminados). Como siempre, cambie el nombre como considere oportuno, aunque debe colocar el paquete en un directorio independiente para que la creación del paquete sea más fácil más adelante (en este ejemplo se usa el `..\bundle` directorio):

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Una vez creado el paquete principal, use el siguiente comando una vez para cada idioma adicional (es decir, repita este comando para cada archivo de asignación de idioma generado en el paso anterior). De nuevo, la salida debe estar en un directorio independiente (el mismo que el paquete principal). Tenga en cuenta que el idioma se especifica **tanto** en la `/f` opción como en la `/p` opción y el uso del nuevo `/r` argumento (que indica que se desea un paquete de recursos):

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Combine todos los paquetes del directorio de la agrupación en un único `.appxbundle` archivo. La nueva `/d` opción especifica el directorio que se va a usar para todos los archivos de la agrupación (este es el motivo por el que los `.appx` archivos se colocan en un directorio independiente en el paso anterior):

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

El último paso para compilar el paquete es la firma.

### <a name="step-32-signing-the-bundle"></a>Paso 3,2: firmar la agrupación

Una vez que haya creado el `.appxbundle` archivo (a través de la herramienta Generador de paquetes o manualmente), tendrá un único archivo que contiene el paquete principal, además de todos los paquetes de recursos. El paso final consiste en firmar el archivo para que Windows lo instale:

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Esto producirá un `.appxbundle` archivo firmado que contiene el paquete principal, además de todos los paquetes de recursos específicos del idioma. Se puede hacer doble clic en él como un archivo de paquete para instalar la aplicación junto con los idiomas adecuados según las preferencias de idioma de Windows del usuario.

## <a name="related-topics"></a>Temas relacionados

* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](tailor-resources-lang-scale-contrast.md)