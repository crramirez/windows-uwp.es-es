---
author: stevewhims
Description: This topic explains the general concept of qualifiers, how to use them, and the purpose of each of the qualifier names.
title: Adaptar los recursos al idioma, escala, contraste alto y otros calificadores
template: detail.hbs
ms.author: stwhi
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 8f3aa529e1c292bcea816e21222ca2a5e07f4319
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2018
ms.locfileid: "5167089"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Adaptar los recursos al idioma, la escala, el contraste alto y otros calificadores

Este tema explica el concepto general de calificadores de recursos, cómo usarlos y la finalidad de cada uno de los nombres de calificador. Consulta [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) para ver una tabla de referencia de todos los valores de calificador posibles.

La aplicación puede cargar activos y recursos adaptados a contextos de tiempo de ejecución, como el idioma de la pantalla, contraste alto, [mostrar el factor de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)y muchos otros. La manera de hacerlo es dar nombres a las carpetas o los archivos de los recursos para que coincidan con los nombres de calificador y valores de calificador que correspondan a esos contextos. Por ejemplo, puede que quieras que tu aplicación cargue un conjunto diferente de activos de imagen en modo de contraste alto.

Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Nombre de calificador, valor de calificador y calificador

Un nombre de calificador es una clave que se asigna a un conjunto de valores de calificador. Estos son el nombre de calificador y los valores de calificador de contraste.

| Contexto | Nombre de calificador | Valores de calificador |
| :--------------- | :--------------- | :--------------- |
| Configuración de contraste alto | contraste | estándar, alto, negro y blanco |

Combina un nombre de calificador con un valor de calificador, para formar un calificador. `<qualifier name>-<qualifier value>` es el formato de un calificador. `contrast-standard` es un ejemplo de calificador.

Por lo tanto, para contraste alto, el conjunto de calificadores es `contrast-standard`, `contrast-high`, `contrast-black` y `contrast-white`. Los nombres de calificador y los valores de calificador no distinguen mayúsculas de minúsculas. Por ejemplo, `contrast-standard` y `Contrast-Standard` son el mismo calificador.

## <a name="use-qualifiers-in-folder-names"></a>Usar calificadores en los nombres de carpeta

Este es un ejemplo del uso de calificadores para dar nombres a carpetas que contienen los archivos de activos. Usa los calificadores en los nombres de carpeta si tienes varios archivos de activos por calificador. De este modo, se define el calificador una vez en el nivel de carpeta y el calificador se aplica a todos los elementos que se encuentran dentro de la carpeta.

```
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Si das nombres a las carpetas como en el ejemplo anterior, la aplicación usa la configuración de contraste alto para cargar archivos de recursos desde la carpeta con el nombre del calificador apropiado. Por lo tanto, si el valor es negro en contraste alto, a continuación se cargan los archivos de recursos de la carpeta `\Assets\Images\contrast-black`. Si el valor es Ninguno (es decir, el equipo no está en modo de contraste alto), a continuación se cargan los archivos de recursos de la carpeta `\Assets\Images\contrast-standard`.

## <a name="use-qualifiers-in-file-names"></a>Usar calificadores en nombres de carpetas

En lugar de crear y asignar nombres a carpetas, puedes usar un calificador para que asigne nombres a los propios archivos de recursos. Puede que prefieras hacerlo si solamente tienes un archivo de recursos por calificador. Este es un ejemplo.

```
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

El archivo cuyo nombre contiene el calificador más apropiado para la configuración es el que se carga. Esta lógica de coincidencia funciona del mismo modo para los nombres de archivo que para los nombres de carpeta.

## <a name="reference-a-string-or-image-resource-by-name"></a>Hacer referencia a un recurso de cadena o imagen por nombre

Consulta [Hacer referencia a un identificador de recursos de cadena desde el marcado XAML](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml-markup), [Hacer referencia a un identificador de recursos de cadena desde código](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code) y [Hacer referencia a un recurso de imagen u otro activo en el marcado y código XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Los calificadores real y neutro concuerdan
No es necesario proporcionar un archivo de recursos para *cada* valor de calificador. Por ejemplo, si te encuentras con que solo necesitas un activo visual para contraste alto y uno para contraste estándar, puedes ponerles nombres a esos activos de esta forma.

```
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

El nombre del primer archivo contiene el calificador `contrast-high`. Ese calificador es una coincidencia *real* para cualquier configuración de contraste alto cuando el contraste alto está *activado*. En otras palabras, es una estrecha coincidencia, por lo que se prefiere. Una coincidencia *real* solo se puede producir si el calificador contiene un valor *real*, como hace este. En este caso, `high` es un valor *real* de `contrast`.

El archivo denominado `logo.png` no tiene ningún calificador de contraste. La ausencia de un calificador es un valor *neutro*. Si no se encuentra ninguna coincidencia preferida, el valor neutro actúa como coincidencia de reserva. En este ejemplo, si el contraste alto está *desactivado*, no hay ninguna coincidencia real. La coincidencia *neutra* es la mejor coincidencia que se puede encontrar, por lo que se carga el activo `logo.png`.

Si desearas cambiar el nombre de `logo.png` a `logo.contrast-standard.png`, en ese caso el nombre de archivo contendría un valor de calificador real. Con el contraste alto desactivado, habría una coincidencia real con `logo.contrast-standard.png`, y ese sería el archivo de activos que se cargaría. Por lo tanto, se cargarían los mismos archivos, en las mismas condiciones, pero debido a distintas coincidencias.

Si solo necesitas un conjunto de activos de contraste alto y un conjunto para contraste estándar, puedes usar nombres de carpeta en lugar de nombres de archivo. En este caso, al omitir el nombre de carpeta por completo, obtienes la coincidencia neutra.

```
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Para obtener más información sobre cómo funciona la coincidencia de calificador, consulta [Sistema de administración de recursos](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Varios calificadores

Puedes combinar los calificadores en los nombres de carpetas y archivos. Por ejemplo, es aconsejable que tu aplicación cargue activos de imagen cuando el modo de contraste alto esté activado *y* el factor de escala de pantalla sea de 400. Una manera de hacerlo es con carpetas anidadas.

```
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Para `logo.png` y los demás archivos que se cargarán, la configuración debe coincidir con *ambos* calificadores.

Otra opción consiste en combinar varios calificadores en un nombre de carpeta.

```
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

En un nombre de carpeta, puedes combinar varios calificadores separados por un carácter de guión bajo. `<qualifier1>[_<qualifier2>...]` es el formato.

Puedes combinar varios calificadores en un nombre de archivo en el mismo formato.

```
\Assets\Images\logo.contrast-high_scale-400.png
```

Dependiendo de las herramientas y el flujo de trabajo que usas para la creación de activos, o de lo que te resulte más fácil leer o administrar, puede elegir una estrategia de nomenclatura única para todos los calificadores o puedes combinarlas para los distintos calificadores.

## <a name="alternateform"></a>AlternateForm

El calificador `alternateform` se usa para proporcionar una forma alternativa de un recurso para alguna finalidad especial. Normalmente esto lo usan solo los desarrolladores japoneses de aplicaciones para proporcionar una cadena furigana para la que `msft-phonetic` está reservada (consulta la sección "Admitir Furigana para las cadenas en japonés que puedan ordenarse" en [Cómo prepararse para la localización](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)).

El sistema de destino o la aplicación deben proporcionar un valor con respecto al cual concuerden los calificadores `alternateform`. No uses el prefijo `msft-` para tus propios valores de calificador `alternateform` personalizados.

## <a name="configuration"></a>Configuración

Es poco probable que necesites el nombre de calificador `configuration`. Puede usarse para especificar los recursos que se aplican solo a un determinado entorno de tiempo de creación, como los recursos solo para pruebas.

El calificador `configuration` se usa para cargar un recurso que coincida en la mayor medida posible con el valor de la variable de entorno `MS_CONFIGURATION_ATTRIBUTE_VALUE`. Por tanto, puedes poner la variable al valor de la cadena que se ha asignado a los recursos relevantes, por ejemplo `designer` o `test`.

## <a name="contrast"></a>Contraste

El calificador `contrast` se usa para proporcionar recursos que se ajusten mejor a la configuración de contraste alto.

## <a name="custom"></a>Personalizado

La aplicación puede establecer un valor para el calificador `custom` y, a continuación, se cargan los recursos que coincidan más con ese valor. Por ejemplo, puedes cargar recursos basados en la licencia de la aplicación. Cuando se inicia la aplicación, comprueba su licencia y usa eso como el valor del calificador `custom`, llamando a [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_), como se muestra en el ejemplo de código.

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

En este escenario, tendrías que facilitar los nombres de los recursos que incluyen los calificadores `custom-premium`, `custom-standard` y `custom-trial`.

## <a name="devicefamily"></a>DeviceFamily

Es poco probable que necesites el nombre de calificador `devicefamily`. Puedes y debes evitar usarlo siempre que sea posible, porque hay técnicas que puedes usar en su lugar, que son mucho más adecuadas y robustas. Estas técnicas se describen en [Detección de la plataforma en que se está ejecutando la aplicación](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) y [Código adaptable para versiones](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Pero, como último recurso, es posible usar calificadores devicefamily para dar nombre a carpetas que contengan las vistas XAML (una vista XAML es un archivo XAML que contiene diseños y controles de interfaz de usuario).

```
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

O bien, puedes ponerles nombres a los archivos.

```
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

En cualquier caso cada copia de `MainPage.[<qualifier>].xaml` comparte una `MainPage.xaml.cs` común, que no se modifica en el proyecto por lo relativo a nombre, ubicación y contenido.

También puedes usar un calificador devicefamily para dar nombre a un archivo de recursos (`.resw`) o a una carpeta. Por ejemplo, cuando la aplicación se ejecuta en la familia de dispositivos móviles, el elemento de interfaz de usuario `<TextBlock x:Uid="DeviceFriendlyName"/>` usará los recursos de texto y de primer plano definidos en tu archivo `Resources.devicefamily-mobile.resw`, caso de que lo contenga.

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Para obtener más información sobre el uso de un archivo de recursos, consulta [Localizar las cadenas de interfaz de usuario](localize-strings-ui-manifest.md).

## <a name="dxfeaturelevel"></a>DXFeatureLevel

Es poco probable que necesites el nombre de calificador `dxfeaturelevel`. Fue diseñado para usarse con los activos de juego de Direct3D, para hacer que los recursos de nivel inferior se cargaran, para coincidir con una configuración de hardware de nivel inferior propia del tiempo. Pero ahora esa configuración de hardware ya se usa tan poco que te recomendamos que no uses este calificador.

## <a name="homeregion"></a>HomeRegion

El calificador `homeregion` corresponde a la configuración del usuario para el país o región. Representa la ubicación principal del usuario. Los valores incluyen cualquier [etiqueta de región BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) que sea válida. Es decir, cualquier código de región de dos letras **ISO 3166-1 alpha-2**, además del conjunto de códigos geográficos de tres dígitos **numérico ISO 3166-1** para regiones compuestas (consulta la [Composición M49 de códigos de región de la División de Estadística de las Naciones Unidas](http://go.microsoft.com/fwlink/p/?linkid=247929)). Los códigos de "Agrupaciones económicas seleccionadas y otras" no son válidos.

## <a name="language"></a>Idioma

Un calificador `language` corresponde a la configuración de idioma de la pantalla. Los valores incluyen cualquier [etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) que sea válida. Para obtener una lista de idiomas, consulta el [Registro de subetiquetas de idioma de IANA](http://go.microsoft.com/fwlink/p/?linkid=227303).

Si quieres que tu aplicación admita diferentes idiomas de pantalla y tienes literales de cadena en el código o en el marcado XAML, entonces mueve esas cadenas fuera del código o marcado y a un archivo de recursos (`.resw`). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación.

Normalmente se usa un calificador `language` para dar nombre a las carpetas que contienen los archivos de recursos (`.resw`).

```
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

Puedes omitir la parte `language-` de un calificador `language` (es decir, el nombre de calificador). No puedes hacerlo con los otros tipos de calificadores; y sólo puedes hacerlo en un nombre de carpeta.

```
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

En lugar de dar nombres a carpetas, puedes usar un calificador `language` para que los archivos de recursos se den nombre ellos mismos.

```
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Consulta [Localizar las cadenas de interfaz de usuario](localize-strings-ui-manifest.md) para obtener más información sobre cómo hacer la aplicación localizable mediante el uso de recursos de cadenas y cómo hacer referencia a un recurso de cadena en la aplicación.

## <a name="layoutdirection"></a>LayoutDirection

Un calificador `layoutdirection` corresponde a la dirección del diseño de la configuración de idioma de la pantalla. Por ejemplo, es posible que una imagen deba reflejarse para los idiomas de lectura de derecha a izquierda, cómo el árabe o el hebreo. Los paneles de diseño y las imágenes de la interfaz de usuario responderán a la dirección del diseño correctamente si estableces su propiedad [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consulta [Ajustar el diseño y las fuentes y admitir RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). Sin embargo, el calificador `layoutdirection` es para los casos donde el simple volteo no es adecuado, y permite responder a la direccionalidad del alineamiento específico del orden de lectura y el texto de formas más generales.

## <a name="scale"></a>Escala

Windows selecciona automáticamente un factor de escala para cada pantalla en función de su valor de PPP (puntos por pulgada) y la distancia de visualización del dispositivo. Consulta [Píxeles efectivos y factor de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Debes crear las imágenes con varios tamaños recomendados (al menos 100, 200 y 400) para que Windows pueda elegir el tamaño perfecto o pueda utilizar el tamaño más cercano y ajustar su escala. Para que Windows pueda identificar qué archivo físico contiene el tamaño correcto de imagen para el factor de escala de visualización, deberás usar un calificador `scale`. La escala de un recurso coincide con el valor de [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale) o el recurso con la siguiente escala mayor.

Este es un ejemplo de cómo establecer el calificador del nivel de carpeta.

```
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Y este ejemplo lo establece en el nivel de archivo.

```
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Para obtener información sobre la calificación de un recurso tanto para `scale` como para `targetsize`, consulta [Calificar un recurso de imagen para targetsize](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>TargetSize

El calificador `targetsize` se usa principalmente para especificar los [iconos de asociación de tipo de archivo](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh127427) o los [iconos de protocolo](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/bb266530) a mostrar en el Explorador de archivos. El valor del calificador representa la longitud del lado de una imagen cuadrada en píxeles (físicos) sin procesar. Se carga el recurso cuyo valor coincide con la opción de configuración Vista del Explorador de archivos; o el recurso con el siguiente valor más grande, en caso de ausencia de coincidencia exacta.

Puedes definir activos que representen varios tamaños de valor de calificador `targetsize` para el icono de la aplicación (`/Assets/Square44x44Logo.png`) en la pestaña de activos visuales del diseñador de manifiesto de paquete de la aplicación.

Para obtener información sobre la calificación de un recurso tanto para `scale` como para `targetsize`, consulta [Qualify an image resource for targetsize](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Tema

El calificador `theme` se usa para proporcionar recursos que coincidan mejor con la configuración predeterminada del modo de aplicación o la invalidación de la aplicación usando [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application?branch=master.RequestedTheme).

## <a name="important-apis"></a>API importantes

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)

## <a name="related-topics"></a>Temas relacionados

* [Píxeles efectivos y factor de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Sistema de administración de recursos](resource-management-system.md)
* [Cómo prepararse para la localización](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)
* [Detección de la plataforma en la que se está ejecutando la aplicación](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Información general de familias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [Localizar las cadenas de interfaz de usuario](localize-strings-ui-manifest.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Composición M49 de códigos de regiones de la División de estadística de las Naciones Unidas](http://go.microsoft.com/fwlink/p/?linkid=247929)
* [Registro de subetiquetas de idiomas IANA](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Ajustar el diseño y las fuentes, y admitir la escritura de derecha a izquierda](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
