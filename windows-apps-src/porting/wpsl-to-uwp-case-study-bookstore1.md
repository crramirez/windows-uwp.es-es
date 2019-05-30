---
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: En este tema se presenta un caso práctico de migración de una aplicación muy sencilla de Windows Phone Silverlight a una aplicación de Windows 10 Universal Windows Platform (UWP).
title: Caso práctico de UWP, Bookstore1 Windows Phone Silverlight
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02337d02472b7215f0fb9be47419caf52420e0f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372417"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>Windows Phone Silverlight a UWP caso práctico: Bookstore1


En este tema se presenta un caso práctico de migración de una aplicación muy sencilla de Windows Phone Silverlight a una aplicación de Windows 10 Universal Windows Platform (UWP). Con Windows 10, puede crear un paquete de aplicación única que pueden instalar los clientes en una amplia gama de dispositivos, y eso es lo que haremos en este caso práctico. Consulta [Guía de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

La aplicación que portaremos consta de un enlace **ListBox** enlazado con un modelo de vista. El modelo de vista tiene una lista de libros que muestra el título, el autor y la portada de libro. Las imágenes de portada de libro tienen el valor de **Acción de compilación** establecido en **Contenido** y de **Copiar en el directorio de salida** establecido en **No copiar**.

En los temas anteriores de esta sección se describen las diferencias entre las plataformas y se proporcionan detalles y pautas sobre el proceso de migración de diversos aspectos de una aplicación, entre ellos el marcado XAML, el enlace a un modelo de vista y el acceso a los datos. El objetivo de un caso práctico consiste en complementar esa orientación mostrando un ejemplo real en la práctica. En el caso práctico se supone que has leído las directrices, ya que no se repiten.

**Tenga en cuenta**    al abrir Bookstore1Universal\_10 en Visual Studio, si ve el mensaje "Requiere la actualización de Visual Studio", a continuación, siga los pasos para seleccionar un control de versiones de plataforma de destino en [ TargetPlatformVersion](wpsl-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Descargas

[Descargue el Windows Phone Bookstore1WPSL8 aplicación Silverlight](https://go.microsoft.com/fwlink/?linkid=517053).

[Descargue el Bookstore1Universal\_aplicación para 10 Windows 10](https://go.microsoft.com/fwlink/?linkid=532950).

## <a name="the-windowsphone-silverlight-app"></a>El Windows Phone Silverlight app

Este es el aspecto de Bookstore1WPSL8 (la aplicación que vamos a portar). Es simplemente un cuadro de lista de libros con desplazamiento vertical debajo del encabezado del nombre de aplicación y título de la página.

![aspecto de booksare1wpsl8](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>Migrar a un proyecto de Windows 10

Es una tarea muy rápida crear un nuevo proyecto en Visual Studio, copiar archivos en él desde Bookstore1WPSL8 e incluir los archivos copiados en el nuevo proyecto. Empieza creando un proyecto nuevo de Aplicación vacía (Windows Universal). Asígnele el nombre Bookstore1Universal\_10. Estos son los archivos para copiar a través de Bookstore1WPSL8 a Bookstore1Universal\_10.

-   Copie la carpeta que contiene los archivos de libro portada imagen PNG (la carpeta es \\activos\\CoverImages). Después de copiar la carpeta, en el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botń secundario en la carpeta que has copiado y haz clic en **Incluir en el proyecto**. Este comando es lo que conocemos como "incluir" archivos o carpetas en un proyecto. Cada vez que copies un archivo o carpeta, haz clic en **Actualizar** en el **Explorador de soluciones** y luego incluye el archivo o la carpeta en el proyecto. No es necesario hacer esto para los archivos que reemplaces en el destino.
-   Copie la carpeta que contiene el archivo de origen del modelo de vista (la carpeta es \\ViewModel).
-   Copia MainPage.xaml y reemplaza el archivo en el destino.

Mantenemos la App.xaml y App.xaml.cs que Visual Studio genera automáticamente en el proyecto de Windows 10.

Editar los archivos de marcado y código de origen que acaba de copiar y cambiar las referencias al espacio de nombres Bookstore1WPSL8 a Bookstore1Universal\_10. Una forma rápida de hacerlo es usar la función **Reemplazar en archivos**. En el código imperativo del archivo de origen del modelo de vista, deben realizarse estos cambios de migración:

-   Cambia `System.ComponentModel.DesignerProperties` a `DesignMode` y luego usa el comando **Resolver** en él. Elimina la propiedad `IsInDesignTool` y usa IntelliSense para agregar el nombre de propiedad correcto: `DesignModeEnabled`.
-   Usa el comando **Resolver** en `ImageSource`.
-   Usa el comando **Resolver** en `BitmapImage`.
-   Elimina using `System.Windows.Media;` y `using System.Windows.Media.Imaging;`.
-   Cambiar el valor devuelto por la **Bookstore1Universal\_10.BookstoreViewModel.AppName** propiedad de "BOOKSTORE1WPSL8" a "BOOKSTORE1UNIVERSAL".

En MainPage.xaml, debes realizar los siguientes cambios de puerto:

-   Cambia `phone:PhoneApplicationPage` por `Page` (no olvides las repeticiones en la sintaxis del elemento de propiedad).
-   Elimina las declaraciones de prefijo del espacio de nombres `phone` y `shell`.
-   Cambia "clr-namespace" por "using" en la declaración de prefijo del espacio de nombres restante.

Podemos optar por corregir los errores de compilación de marcado de forma muy sencilla si queremos ver los resultados pronto, aunque implique quitar el marcado temporalmente. Vamos a mantener un registro de la deuda que acumulamos al hacerlo. En este caso es este.

1.  En el elemento raíz **Page** de **MainPage.xaml**, elimina `SupportedOrientations="Portrait"`.
2.  En el elemento raíz **Page** de **MainPage.xaml**, elimina `Orientation="Portrait"`.
3.  En el elemento raíz **Page** de **MainPage.xaml**, elimina `shell:SystemTray.IsVisible="True"`.
4.  En la plantilla de datos `BookTemplate`, elimina las referencias a los estilos `PhoneTextExtraLargeStyle` y `PhoneTextSubtleStyle` de **TextBlock**.
5.  En el `TitlePanel` **StackPanel**, elimina las referencias a los estilos `PhoneTextNormalStyle` y `PhoneTextTitle1Style` de **TextBlock**.

Vamos a trabajar en la interfaz de usuario para la familia de dispositivos móviles primero y después podemos considerar otros factores de forma. Ahora puedes compilar y ejecutar la aplicación. Este es su aspecto en el emulador móvil.

![la aplicación para uwp en versión móvil, con los cambios en el código fuente inicial](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

La vista y el modelo de vista funcionan juntos correctamente y **ListBox** está funcionando. Necesitamos principalmente corregir el estilo y obtener las imágenes que se mostrarán.

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>Saldar la deuda de elementos y algunos estilos iniciales

De forma predeterminada, se admiten todas las orientaciones. La aplicación de Silverlight de Windows Phone explícitamente restringe propio en vertical solo, sin embargo, los elementos de deuda es así \#1 y \#2 están pagando por entrar en el manifiesto del paquete de aplicación en el nuevo proyecto y la comprobación **vertical**en **admite orientaciones**.

Para esta aplicación, elemento \#3 no es una deuda, ya que se muestra la barra de estado (anteriormente denominada Bandeja del sistema) de forma predeterminada. Para los elementos \#4 y \#5, necesitaremos buscar cuatro plataforma Universal de Windows (UWP) **TextBlock** estilos que se corresponden con los estilos de Windows Phone Silverlight que usábamos. Puede ejecutar la aplicación de Windows Phone Silverlight en el emulador y compárelo side-by-side con la ilustración en la [texto](wpsl-to-uwp-porting-xaml-and-ui.md) sección. De hacerlo y consultar las propiedades de los estilos del sistema de Windows Phone Silverlight, podemos hacer que esta tabla.

| Clave de estilo de Windows Phone Silverlight | Clave de estilo de UWP          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
Para establecer estos estilos puedes escribirlos en el editor de marcado o usar las herramientas de XAML de Visual Studio y establecerlos sin necesidad de escribir nada. Para ello, hace doble clic en un **TextBlock** y haga clic en **Editar estilo** &gt; **Aplicar recurso**. Para hacerlo con el **TextBlock**s en la plantilla de elemento, haga clic la **ListBox** y haga clic en **editar plantillas adicionales** &gt; **editar Generados (ItemTemplate) elementos**.

Hay un fondo blanco con 80 % de opacidad detrás de los elementos porque el estilo predeterminado del control **ListBox** establece su fondo en el recurso del sistema `ListBoxBackgroundThemeBrush`. Establece `Background="Transparent"` en **ListBox** para borrar este fondo. Para alinear a la izquierda los **TextBlock** de la plantilla de elemento, edítalos de nuevo tal como se describe más arriba y establece un **Margin** de `"9.6,0"` en ambos **TextBlock**.

Después de hacerlo, y debido a los [cambios relacionados con los píxeles de visualización](wpsl-to-uwp-porting-xaml-and-ui.md), debemos recorrer y multiplicar toda dimensión de tamaño fijo que aún no hayamos modificado (márgenes, ancho, alto, etc.) por 0,8. De este modo, las imágenes deben cambiar de 70 x 70 px a 56 x 56 px, por ejemplo.

Pero obtengamos esas imágenes que vamos a representar antes de mostrar los resultados de la aplicación de estilos.

## <a name="binding-an-image-to-a-view-model"></a>Enlazar una imagen a un modelo de vista

En Bookstore1WPSL8, hicimos lo siguiente:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

En Bookstore1Universal, usamos el [esquema de URI](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)) "ms-appx". Para que podamos mantener el resto del código sin modificar, podemos usar una sobrecarga distinta del constructor **System.Uri** para poner el esquema de URI ms-appx en un URI base y anexarle el resto de la ruta de acceso. Como a continuación:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>Estilos universales

Ahora solo tenemos que hacer algunos retoques finales de estilo y confirmar que la aplicación se vea bien en factores de forma de escritorio (y otros), así como móviles. Los pasos se indican a continuación. Puedes usar los vínculos que aparecen en la parte superior de este tema para descargar los proyectos y ver los resultados de todos los cambios entre este punto y el final del caso práctico.

-   Para ajustar el espaciado entre los elementos, busca la plantilla de datos `BookTemplate` en MainPage.xaml y elimina el atributo `Margin` del elemento raíz **Grid**.
-   Si quieres dar un poco más de espacio al título de la página, puedes restablecer el margen inferior de `-5.6` a `0` en el **TextBlock** del título de la página.
-   Ahora debemos establecer el elemento Background de `LayoutRoot` en el valor predeterminado correcto para que la aplicación tenga una apariencia adecuada cuando se ejecute en todos los dispositivos independientemente de cuál sea el tema. Cámbialo de `"Transparent"` a `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.

Con una aplicación más sofisticada, este es el punto en el que usamos la directriz de [Migración para factores de forma y experiencia de usuario](wpsl-to-uwp-form-factors-and-ux.md) y realmente aprovechamos el factor de forma de cada uno de los muchos dispositivos en los que la aplicación se puede ejecutar. Pero para esta sencilla aplicación podemos dejarlo aquí y ver el aspecto de la aplicación después de esta última secuencia de operaciones de estilo. En realidad tiene el mismo aspecto en dispositivos móviles y de escritorio, aunque el uso del espacio en factores de forma ancha no sea el mejor (investigaremos cómo hacerlo en un caso práctico más adelante).

Consulta [Cambios de tema](wpsl-to-uwp-porting-xaml-and-ui.md) para ver cómo controlar el tema de la aplicación.

![la aplicación para windows 10 portada](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

La aplicación de Windows 10 portada que se ejecutan en un dispositivo móvil

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>Un ajuste opcional en el cuadro de lista para dispositivos móviles

Cuando la aplicación se ejecuta en un dispositivo móvil, el fondo de un cuadro de lista es claro en ambos temas de forma predeterminada. Puede ser el estilo que prefieras y, si es este, no hay nada más que hacer. No obstante, los controles están diseñados para que puedas personalizar su apariencia sin que ello influya en el comportamiento. Por lo tanto, si quieres que el cuadro de lista sea oscuro en el tema oscuro (la apariencia que tenía la aplicación original), sigue [estas instrucciones](w8x-to-uwp-case-study-bookstore1.md) en "Un ajuste opcional".

## <a name="conclusion"></a>Conclusión

En este caso práctico se mostró el proceso de migración de una aplicación muy sencilla (y posiblemente, poco realista). Por ejemplo, se puede usar controles de lista para seleccionar o establecer un contexto de navegación; la aplicación navega a una página con más detalles sobre el elemento que se pulsó. Esta aplicación en particular no hace nada con la selección del usuario y no tiene navegación. Aun así, el caso práctico sirve para dar los primeros pasos y presentar el proceso de migración, así como demostrar técnicas importantes que se puede usar en aplicaciones para UWP reales.

El siguiente caso práctico es [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md), en el que vamos a ver cómo mostrar datos agrupados y obtener acceso a ellos.
