---
description: En este tema se explica el concepto general de calificadores, cómo usarlos y el propósito de cada uno de los nombres de calificador.
title: Adaptar los recursos al idioma, escala, alto contraste y otros calificadores
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 59f0b636384ba133e985f0704e2033c1acc5f15e
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2020
ms.locfileid: "89412019"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Adaptar los recursos al idioma, escala, alto contraste y otros calificadores

En este tema se explica el concepto general de calificadores de recursos, cómo usarlos y la finalidad de cada uno de los nombres de calificador. Vea [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) para obtener una tabla de referencia de todos los posibles valores de calificador.

La aplicación puede cargar activos y recursos que se adaptan a los contextos en tiempo de ejecución, como el idioma de visualización, el contraste alto, el [factor de escala de visualización](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)y muchos otros. La manera de hacerlo es asignar un nombre a las carpetas o los archivos de los recursos para que coincidan con los nombres de calificador y los valores de calificador que corresponden a esos contextos. Por ejemplo, puede que desee que la aplicación cargue un conjunto diferente de recursos de imagen en el modo de contraste alto.

Para más información sobre la propuesta de valor de localizar la aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Nombre del calificador, valor de calificador y calificador

Un nombre de calificador es una clave que se asigna a un conjunto de valores de calificador. Estos son los valores de nombre de calificador y calificador para el contraste.

| Context | Nombre del calificador | Valores de calificador |
| :--------------- | :--------------- | :--------------- |
| Configuración de contraste alto | contraste | estándar, alto, negro, blanco |

Combine un nombre de calificador con un valor de calificador para formar un calificador. `<qualifier name>-<qualifier value>` es el formato de un calificador. `contrast-standard` es un ejemplo de un calificador.

Por lo tanto, para el contraste alto, el conjunto de calificadores es `contrast-standard` ,, `contrast-high` `contrast-black` y `contrast-white` . Los nombres y los valores de calificador no distinguen mayúsculas de minúsculas. Por ejemplo, `contrast-standard` y `Contrast-Standard` son el mismo calificador.

## <a name="use-qualifiers-in-folder-names"></a>Usar calificadores en nombres de carpeta

Este es un ejemplo del uso de calificadores para asignar nombres a las carpetas que contienen archivos de recursos. Use calificadores en nombres de carpeta si tiene varios archivos de recursos por calificador. De este modo, se establece el calificador una vez en el nivel de carpeta y el calificador se aplica a todo el contenido de la carpeta.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Si asigna un nombre a las carpetas como en el ejemplo anterior, la aplicación usa la configuración de contraste alto para cargar los archivos de recursos desde la carpeta denominada para el calificador adecuado. Por lo tanto, si el valor es contraste alto negro, se cargan los archivos de recursos en la `\Assets\Images\contrast-black` carpeta. Si el valor es ninguno (es decir, el equipo no está en modo de contraste alto), se cargan los archivos de recursos de la `\Assets\Images\contrast-standard` carpeta.

## <a name="use-qualifiers-in-file-names"></a>Usar calificadores en nombres de archivo

En lugar de crear y asignar nombres a las carpetas, puede usar un calificador para asignar nombre a los propios archivos de recursos. Puede que prefiera hacerlo si solo tiene un archivo de recursos por cada calificador. A continuación se muestra un ejemplo.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

El archivo cuyo nombre contiene el calificador más apropiado para la configuración es el que se carga. Esta lógica coincidente funciona de la misma manera para los nombres de archivo que para los nombres de carpeta.

## <a name="reference-a-string-or-image-resource-by-name"></a>Hacer referencia a un recurso de cadena o imagen por nombre

Vea [hacer referencia a un identificador de recursos de cadena desde el marcado XAML](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml), [hacer referencia a un identificador de recursos de cadena desde el código](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code)y [hacer referencia a una imagen u otro recurso desde código y marcado XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Coincide con el calificador real y neutro
No es necesario proporcionar un archivo de recursos para *cada* valor de calificador. Por ejemplo, si observa que solo necesita un recurso visual para contraste alto y otro para el contraste estándar, puede asignar un nombre a esos recursos como este.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

El primer nombre de archivo contiene el `contrast-high` calificador. Ese calificador es una coincidencia *real* de cualquier valor de contraste alto cuando está *activado*el contraste alto. En otras palabras, se trata de una coincidencia cercana, por lo que es preferible. Una coincidencia *real* solo puede producirse si el calificador contiene un valor *real* , como hace. En este caso, `high` es un valor *real* para `contrast` .

El archivo denominado `logo.png` no tiene ningún calificador de contraste en absoluto. La ausencia de un calificador es un valor *neutro* . Si no se encuentra ninguna coincidencia preferida, el valor neutro sirve como coincidencia de reserva. En este ejemplo, si el contraste alto está *desactivado*, no hay ninguna coincidencia real. La coincidencia *neutra* es la mejor coincidencia que se puede encontrar y, por tanto, `logo.png` se carga el recurso.

Si tuviera que cambiar el nombre de `logo.png` a `logo.contrast-standard.png` , el nombre de archivo contendría un valor de calificador real. Con contraste alto desactivado, debería haber una coincidencia real con `logo.contrast-standard.png` y ese es el archivo de recursos que se cargaría. Por lo tanto, se cargarían los mismos archivos, en las mismas condiciones, pero debido a coincidencias diferentes.

Si solo necesita un conjunto de recursos para el contraste alto y un conjunto para el contraste estándar, puede utilizar nombres de carpeta en lugar de nombres de archivo. En este caso, omitir completamente el nombre de la carpeta le da la coincidencia neutra.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Para obtener más información sobre cómo funciona la coincidencia de calificador, vea [sistema de administración de recursos](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Varios calificadores

Puede combinar calificadores en nombres de archivos y carpetas. Por ejemplo, puede que desee que la aplicación cargue recursos de imagen cuando el modo de contraste alto esté activado *y* el factor de escala de pantalla sea 400. Una manera de hacerlo es con las carpetas anidadas.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Para `logo.png` y los demás archivos que se van a cargar, la configuración debe coincidir con *ambos* calificadores.

Otra opción consiste en combinar varios calificadores en un nombre de carpeta.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

En un nombre de carpeta, se combinan varios calificadores separados con un carácter de subrayado. `<qualifier1>[_<qualifier2>...]` es el formato.

Puede combinar varios calificadores en un nombre de archivo en el mismo formato.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

En función de las herramientas y el flujo de trabajo que use para la creación de recursos, o en lo que más le resulte más fácil de leer o administrar, puede elegir una estrategia de nomenclatura única para todos los calificadores o puede combinarlos para diferentes calificadores.

## <a name="alternateform"></a>AlternateForm

El `alternateform` calificador se usa para proporcionar una forma alternativa de un recurso con fines especiales. Esto lo suelen usar los desarrolladores de aplicaciones en japonés para proporcionar una cadena furigana para la que el valor `msft-phonetic` está reservado (vea la sección "compatibilidad con furigana para cadenas japonesas que se pueden ordenar" en [How to Prepare for Localization](/previous-versions/windows/apps/hh967762(v=win.10))).

El sistema de destino o la aplicación deben proporcionar un valor con el que `alternateform` se cotejan los calificadores. No use el `msft-` prefijo para sus propios `alternateform` valores de calificador personalizados.

## <a name="configuration"></a>Configuración

No es probable que necesite el nombre del `configuration` calificador. Se puede usar para especificar los recursos que solo son aplicables a un entorno de tiempo de creación determinado, como los recursos de solo prueba.

El `configuration` calificador se usa para cargar un recurso que mejor coincida con el valor de la `MS_CONFIGURATION_ATTRIBUTE_VALUE` variable de entorno. Por lo tanto, puede establecer la variable en el valor de cadena que se ha asignado a los recursos correspondientes, por ejemplo `designer` , o `test` .

## <a name="contrast"></a>Compare

El `contrast` calificador se usa para proporcionar recursos que coincidan mejor con la configuración de contraste alto.

## <a name="custom"></a>Personalizado

La aplicación puede establecer un valor para el `custom` calificador y, a continuación, se cargan los recursos que mejor coincidan con ese valor. Por ejemplo, puede que desee cargar recursos en función de la licencia de su aplicación. Cuando se inicia la aplicación, comprueba su licencia y la usa como el valor del `custom` calificador mediante una llamada a [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue), como se muestra en el ejemplo de código.

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

En este escenario, proporcionaría los nombres de los recursos que incluyen los calificadores `custom-premium` , `custom-standard` y `custom-trial` .

## <a name="devicefamily"></a>DeviceFamily

No es probable que necesite el nombre del `devicefamily` calificador. Puede y debe evitar usarlo siempre que sea posible, ya que hay técnicas que puede usar en su lugar y que son mucho más convenientes y eficaces. Estas técnicas se describen en [detectar la plataforma en la que se ejecuta la aplicación y en el](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) [código adaptable](../debug-test-perf/version-adaptive-code.md)de la versión.

Sin embargo, como último recurso, es posible usar calificadores devicefamily para asignar nombres a las carpetas que contienen las vistas XAML (una vista XAML es un archivo XAML que contiene los controles y el diseño de la interfaz de usuario).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

O bien, puede asignar nombres a los archivos.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

En cualquier caso, cada copia de `MainPage.[<qualifier>].xaml` comparte una común `MainPage.xaml.cs` , que permanece sin cambios en el proyecto en términos de nombre, ubicación y contenido.

También puede usar un calificador devicefamily para asignar un nombre a un archivo de recursos ( `.resw` ) o a una carpeta. Por ejemplo, cuando la aplicación se ejecuta en la familia de dispositivos móviles, el elemento de la interfaz de usuario usará `<TextBlock x:Uid="DeviceFriendlyName"/>` los recursos de texto y de primer plano definidos en el `Resources.devicefamily-mobile.resw` archivo si contiene

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Para obtener más información sobre el uso de un archivo de recursos, consulte [localizar las cadenas de la interfaz de usuario](localize-strings-ui-manifest.md).

## <a name="dxfeaturelevel"></a>DXFeatureLevel

No es probable que necesite el nombre del `dxfeaturelevel` calificador. Se diseñó para usarse con los recursos de juegos de Direct3D, con el fin de que se carguen los recursos de nivel inferior para que coincidan con una determinada configuración de hardware de bajo nivel de tiempo. Pero la prevalencia de esa configuración de hardware es ahora tan baja que se recomienda no usar este calificador.

## <a name="homeregion"></a>HomeRegion

El `homeregion` calificador corresponde a la configuración del usuario para el país o la región. Representa la ubicación principal del usuario. Los valores incluyen cualquier [etiqueta de región BCP-47](https://tools.ietf.org/html/bcp47)válida. Es decir, cualquier código **iso 3166-1 alpha-2** de la región de dos letras, más el conjunto de códigos geográficos **ISO 3166-1 numéricos** de tres dígitos para regiones compuestas (consulte la [composición de regiones de M49 de Naciones Unidas](https://unstats.un.org/unsd/methods/m49/m49regin.htm)). Los códigos de "selecciones económicas y otras agrupaciones" no son válidos.

## <a name="language"></a>Idioma

Un `language` calificador corresponde a la configuración de idioma para mostrar. Los valores incluyen cualquier [etiqueta de lenguaje BCP-47](https://tools.ietf.org/html/bcp47)válida. Para obtener una lista de los idiomas, consulte el [registro de subetiquetas del lenguaje IANA](https://www.iana.org/assignments/language-subtag-registry).

Si desea que la aplicación admita diferentes idiomas para mostrar y que tenga literales de cadena en el código o en el marcado XAML, mueva esas cadenas fuera del código o marcado y a un archivo de recursos ( `.resw` ). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación.

Normalmente se utiliza un `language` calificador para asignar un nombre a las carpetas que contienen los archivos de recursos ( `.resw` ).

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

Puede omitir la `language-` parte de un `language` calificador (es decir, el nombre del calificador). No se puede hacer esto con los otros tipos de calificadores; y solo puede hacerlo en un nombre de carpeta.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

En lugar de asignar nombres a las carpetas, puede usar `language` calificadores para asignar nombres a los propios archivos de recursos.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Consulte [localizar las cadenas de la interfaz de usuario](localize-strings-ui-manifest.md) para obtener más información sobre cómo hacer que la aplicación se pueda localizar mediante recursos de cadena y cómo hacer referencia a un recurso de cadena en la aplicación.

## <a name="layoutdirection"></a>LayoutDirection

Un `layoutdirection` calificador corresponde a la dirección de diseño de la configuración de idioma para mostrar. Por ejemplo, puede que sea necesario crear un reflejo de una imagen para un idioma de derecha a izquierda, como el árabe o el hebreo. Los paneles de diseño y las imágenes de la interfaz de usuario responderán correctamente a la dirección de diseño si establece su propiedad [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consulte [ajustar diseño y fuentes, y compatibilidad con RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). Sin embargo, el `layoutdirection` calificador es para los casos en los que el volteo sencillo no es adecuado y permite responder a la direccionalidad del orden de lectura específico y la alineación del texto de maneras más generales.

## <a name="scale"></a>Escala

Windows selecciona automáticamente un factor de escala para cada pantalla en función de sus PPP (puntos por pulgada) y la distancia de visualización del dispositivo. Vea [píxeles efectivos y factor de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Debe crear las imágenes en varios tamaños recomendados (al menos 100, 200 y 400) para que Windows pueda elegir el tamaño perfecto, o bien puede usar el tamaño más cercano y escalarla. Para que Windows pueda identificar qué archivo físico contiene el tamaño correcto de la imagen para el factor de escala de pantalla, se usa un `scale` calificador. La escala de un recurso coincide con el valor de [DisplayInformation. ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale)o con el siguiente recurso a mayor escala.

A continuación se muestra un ejemplo de cómo establecer el calificador en el nivel de carpeta.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Y este ejemplo lo establece en el nivel de archivo.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Para obtener información sobre cómo calificar un recurso para `scale` y `targetsize` , consulte [calificar un recurso de imagen para el destino](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>TargetSize

El `targetsize` calificador se usa principalmente para especificar [iconos de asociaciones de tipo de archivo](/windows/desktop/shell/how-to-assign-a-custom-icon-to-a-file-type) o iconos de [Protocolo](/windows/desktop/search/-search-3x-wds-ph-ui-extensions) que se mostrarán en el explorador de archivos. El valor del calificador representa la longitud lateral de una imagen cuadrada en píxeles sin formato (físicos). Se carga el recurso cuyo valor coincide con la configuración de la vista en el explorador de archivos. o el recurso con el siguiente valor mayor en la ausencia de una coincidencia exacta.

Puede definir recursos que representen varios tamaños de `targetsize` valor de calificador para el icono de la aplicación ( `/Assets/Square44x44Logo.png` ) en la pestaña activos visuales del diseñador de manifiestos del paquete de aplicaciones.

Para obtener información sobre cómo calificar un recurso para `scale` y `targetsize` , consulte [calificar un recurso de imagen para el destino](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Tema

El `theme` calificador se usa para proporcionar los recursos que mejor se adaptan a la configuración predeterminada del modo de aplicación o la invalidación de la aplicación mediante [Application. RequestedTheme](/uwp/api/windows.ui.xaml.application.requestedtheme).


## <a name="shell-light-theme-and-unplated-resources"></a>Subtema claro de Shell y recursos desplanchados
La *actualización de Windows 10 de mayo de 2019* presentó un nuevo tema "claro" para el shell de Windows. Como resultado, algunos recursos de aplicación que se mostraron anteriormente en un fondo oscuro se mostrarán ahora en un fondo claro. En el caso de las aplicaciones que proporcionan recursos sin placa de altform para los conmutadores de barra de tareas y de ventanas (Alt + Tab, vista de tareas, etc.), debe comprobar que tienen un contraste aceptable en un fondo claro.

### <a name="providing-light-theme-specific-assets"></a>Proporcionar recursos específicos del tema ligeros
Las aplicaciones que quieren proporcionar un recurso personalizado para el tema claro de Shell pueden usar un nuevo calificador de recurso de formulario alternativo: `altform-lightunplated` . Este calificador refleja el calificador altform-unplated existente. 

### <a name="downlevel-considerations"></a>Consideraciones de nivel inferior
Las aplicaciones no deben usar el `theme-light` calificador con el `altform-unplated` calificador. Esto producirá un comportamiento imprevisible en RS5 y versiones anteriores de Windows debido a la manera en que se cargan los recursos de la barra de tareas. En versiones anteriores de Windows, la versión de tema claro se puede usar de forma incorrecta. El `altform-lightunplated` calificador evita este problema. 

### <a name="compatibility-behavior"></a>Comportamiento de compatibilidad
Por compatibilidad con versiones anteriores, Windows incluye la lógica para detectar iconos monocromáticos y comprueba si se compara con el fondo previsto. Si el icono no cumple los requisitos de contraste, Windows buscará una versión en blanco de contraste del recurso. Si no está disponible, Windows revertirá al uso de la versión recubierta del recurso.



## <a name="important-apis"></a>API importantes

* [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)

## <a name="related-topics"></a>Temas relacionados

* [Píxeles efectivos y factor de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Sistema de administración de recursos](resource-management-system.md)
* [Preparación para la localización](/previous-versions/windows/apps/hh967762(v=win.10))
* [Detección de la plataforma en la que se está ejecutando la aplicación](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Programación con SDK de extensión](/uwp/extension-sdks/device-families-overview)
* [Localizar las cadenas de la interfaz de usuario](localize-strings-ui-manifest.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [División de estadísticas de Naciones Unidas M49 composición de códigos de región](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
* [Registro de subetiqueta del lenguaje IANA](https://www.iana.org/assignments/language-subtag-registry)
* [Ajustar el diseño y las fuentes y admitir la escritura RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)