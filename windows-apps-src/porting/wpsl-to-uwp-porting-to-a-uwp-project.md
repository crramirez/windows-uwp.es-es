---
description: El proceso de migración se empieza creando un nuevo proyecto de Windows 10 en Visual Studio y copiando los archivos en él.
title: Trasladar Windows Phone proyectos de Silverlight a proyectos de UWP
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73f90cb52f0bb5756c4e9b67783dbfd286289377
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167459"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>Trasladar Windows Phone proyectos de Silverlight a proyectos de UWP


El tema anterior era [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md).

El proceso de migración se empieza creando un nuevo proyecto de Windows 10 en Visual Studio y copiando los archivos en él.

## <a name="create-the-project-and-copy-files-to-it"></a>Crear el proyecto y copiar archivos en él

1.  Inicia Microsoft Visual Studio 2015 y crea un nuevo proyecto de aplicación vacía (universal de Windows). Para obtener más información, consulte [JumpStart your Windows Runtime 8. x App Using Templates (C#, C++, Visual Basic)](/previous-versions/windows/apps/hh768232(v=win.10)). Tu nuevo proyecto crea un paquete de la aplicación (un archivo appx) que se ejecutará en todas las familias de dispositivos.
2.  En el proyecto de aplicación Windows Phone Silverlight, identifica todos los archivos de código fuente y los archivos de recursos visuales que quieres reutilizar. Con el Explorador de archivos, copia los modelos de datos, los modelos de vista, los recursos visuales, los diccionarios de recursos, la estructura de carpetas y cualquier otro elemento que quieras reutilizar en tu nuevo proyecto. Copia o crea subcarpetas en el disco según sea necesario.
3.  Copia vistas (por ejemplo, MainPage.xaml y MainPage.xaml.cs) en el nodo del proyecto nuevo, también. De nuevo, crea nuevas subcarpetas según sea necesario y quita las vistas existentes del proyecto. Pero antes de sobrescribir o quitar una vista que Visual Studio ha generado, conserva una copia ya que puede resultar útil consultarla más adelante. La primera fase de migración de una aplicación Windows Phone Silverlight se centra en que tenga un buen aspecto y funcione correctamente en una familia de dispositivos. Más adelante, desviarás tu atención a asegurarte de que las vistas de adaptan correctamente a todos los factores de forma y, opcionalmente, a agregar código adaptable para obtener el máximo partido de una familia de dispositivos en particular.
4.  En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Selecciona los archivos que has copiado, haz clic con el botón secundario en ellos y haz clic en **Incluir en el proyecto**. Esto incluirá automáticamente sus carpetas contenedoras. Entonces puedes desactivar **Mostrar todos los archivos** si quieres. Un flujo de trabajo alternativo, si lo prefieres, es usar el comando **Agregar elemento existente**, después de crear las subcarpetas necesarias en el **Explorador de soluciones** de Visual Studio. Vuelve a comprobar que los recursos visuales tienen la opción **Acción de compilación** establecida en **Contenido** y la opción **Copiar en el directorio de resultados** establecida en **No copiar**.
5.  Las diferencias en los nombres de clases y de espacios de nombres generarán un gran número de errores de compilación en esta etapa. Por ejemplo, si abres las vistas que Visual Studio ha generado, verás que son del tipo [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) y no **PhoneApplicationPage**. Hay un gran número de diferencias de marcado XAML y de código imperativo que se explican con detalle en los temas siguientes de esta guía de migración. Pero progresarás más rápidamente siguiendo estos pasos generales: cambia "clr-namespace" por "using" en las declaraciones de prefijo del espacio de nombres en el marcado XAML; usa el tema [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md) y el comando de Visual Studio **Buscar y reemplazar** para crear cambios masivos en el código fuente (por ejemplo, reemplaza "System.Windows" por "Windows.UI.Xaml"); y en el editor de código imperativo de Visual Studio usa los comandos **Resolver** y **Organizar instrucciones Using** del menú contextual para los cambios más específicos.

## <a name="extension-sdks"></a>SDK de extensiones

La mayor parte de las API de la Plataforma universal de Windows (UWP) que tu aplicación migrada llamará están implementadas en el conjunto de API que se conoce como la familia de dispositivos universales. Pero algunas se implementan en los SDK de extensión y Visual Studio solo reconoce las API que están implementadas por la familia de dispositivos de destino de la aplicación o por los SDK de extensión a los que has hecho referencia.

Si se producen errores de compilación sobre espacios de nombres o tipos o miembros no encontrados, es probable que esta sea la causa. Abre el tema de las API en la documentación de referencia de la API y navega hasta la sección Requisitos, donde podrás consultar cuál es la familia de dispositivos de la implementación. Si esa no es la familia de dispositivos de destino, para que la API esté disponible para el proyecto, necesitarás una referencia al SDK de extensión de esa familia de dispositivos.

Haz clic en **Proyecto** &gt; **Agregar referencia** &gt; **Windows Universal** &gt; **Extensiones** y selecciona el SDK de extensión apropiado. Por ejemplo, si las API a las que quieres llamar solo están disponibles en la familia de dispositivos móviles y se introdujeron en la versión 10.0.x.y, selecciona **Extensiones de Windows Mobile para UWP**.

Ello agregará la siguiente referencia al archivo de proyecto:

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

El nombre y el número de versión coinciden con las carpetas de la ubicación de instalación del SDK. Por ejemplo, la información anterior coincide con este nombre de carpeta:

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

A menos que la aplicación esté destinada a la familia de dispositivos que implementa la API, tendrás que usar la clase [**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) para probar la presencia de la API antes de llamarla (esto se denomina "código adaptable"). A continuación, se evaluará esta condición donde se ejecute la aplicación, pero solo se evaluará en true en dispositivos en los que la API está presente y, por tanto, está disponible para ser llamada. Usa únicamente los SDK de extensión y el código adaptable después de comprobar primero si existe una API universal. En la sección siguiente se proporcionan algunos ejemplos.

Consulta también [Manifiesto del paquete de la aplicación](#the-app-package-manifest).

## <a name="maximizing-markup-and-code-reuse"></a>Maximización de la reutilización de código y marcado

Encontrarás que refactorizar un poco o agregar código adaptable (lo cual se explica a continuación), te permitirá maximizar el marcado y el código que funciona en todas las familias de dispositivo. A continuación se indican más detalles.

-   Los archivos que son comunes a todas las familias de dispositivo no necesitan ninguna consideración especial. Esos archivos serán usados por la aplicación en todas las familias de dispositivos en las que se ejecuta. Esto incluye los archivos de marcado XAML, los archivos de código fuente imperativo y los archivos de recursos.
-   Es posible que tu aplicación detecte la familia de dispositivos en la que se está ejecutando y navegar a una vista que se ha diseñado específicamente para esa familia de dispositivos. Para obtener más información, consulta [Detección de la plataforma en la que se está ejecutando la aplicación](wpsl-to-uwp-input-and-sensors.md).
-   Una técnica similar que puede resultar útil si no existe otra alternativa es proporcionar a un archivo de marcado o archivo **ResourceDictionary** (o la carpeta que contiene el archivo) un nombre especial que se carga automáticamente en tiempo de ejecución solo cuando la aplicación se ejecuta en una familia de dispositivos en particular. Esta técnica se ilustra en el caso práctico [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md).
-   Para usar funciones que no están disponibles en todas las familias de dispositivos (por ejemplo, impresoras, escáneres o el botón de la cámara) puedes escribir código adaptable. Vea el tercer ejemplo de la [compilación condicional y el código adaptable](#conditional-compilation-and-adaptive-code) de este tema.
-   Si quieres admitir tanto Windows Phone Silverlight como Windows 10, es posible que puedas compartir archivos de código fuente entre proyectos. Pasos a seguir: en Visual Studio, haz clic con el botón derecho en el proyecto en **Explorador de soluciones**, selecciona **Agregar elemento existente**, selecciona los archivos para compartir y haz clic en **Agregar como vínculo**. Almacena tus archivos de código fuente en una carpeta común en el sistema de archivos donde puedan verlos los proyectos vinculados a ellos y no te olvides de agregarlos al control de código fuente. Si puedes factorizar el código fuente imperativo para que la mayor parte de un archivo, si no todo, funcione en ambas plataformas, no necesitas tener dos copias de este. Puedes encapsular la lógica específica de la plataforma del archivo dentro de directivas de compilación condicional siempre que sea posible, o condiciones de tiempo de ejecución cuando sea necesario. Consulta la sección siguiente y [Directivas de preprocesador de C#](/dotnet/articles/csharp/language-reference/preprocessor-directives/index).
-   Para su reutilización en el nivel binario, en lugar del nivel de código fuente, existen bibliotecas de clases portables que admiten el subconjunto de API de .NET que están disponibles en Windows Phone Silverlight, así como el subconjunto para aplicaciones de Windows 10 (núcleo .NET). Los conjuntos de bibliotecas de clases portables tienen compatibilidad binaria con estas plataformas .NET y más. Usa Visual Studio para crear un proyecto destinado a una biblioteca de clases portable. Consulta [Desarrollo multiplataforma con la biblioteca de clases portable](/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilación condicional y código adaptable

Si quieres compatibilidad con Windows Phone Silverlight y Windows 10 en un solo archivo de código, también puedes hacerlo. Si busca en el proyecto de Windows 10 en las páginas de propiedades del proyecto, verá que el proyecto define WINDOWS \_ UAP como un símbolo de compilación condicional. En general, puedes usar la siguiente lógica para realizar la compilación condicional.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Si tiene código que ha compartido entre una aplicación Windows Phone Silverlight y una aplicación Windows Runtime 8. x, es posible que ya tenga código fuente con lógica similar a la siguiente:

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

Si es así y ahora quieres además compatibilidad con Windows 10, también puedes hacerlo.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

Quizá hayas usado la compilación condicional para limitar el control del botón Atrás del hardware para Windows Phone. En Windows 10, el evento de botón Atrás es un concepto universal. Los botones Atrás implementados en hardware o en software generarán el evento [**BackRequested**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested), por lo que ese es el que hay que controlar.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

Quizá hayas usado la compilación condicional para limitar el control del botón de la cámara del hardware para Windows Phone. En Windows 10, el botón de la cámara de hardware es un concepto específico de la familia de dispositivos móviles. Debido a que se ejecutará un paquete de la aplicación en todos los dispositivos, cambiamos nuestra condición de tiempo de compilación a una condición de tiempo de ejecución mediante el uso de código adaptable. Para ello, usamos la clase [**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) para consultar en tiempo de ejecución la presencia de la clase [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons). **HardwareButtons** se define en el SDK de la extensión móvil, por lo que tendremos que agregar una referencia a ese SDK a nuestro proyecto para poder compilar este código. No obstante, ten en cuenta que el controlador solo se ejecutará en un dispositivo que implementa los tipos definidos en el SDK de extensión móvil y que esa es la familia de dispositivos móviles. Por lo tanto, el siguiente código solo cuida el uso de las características que están presentes, aunque lo consigue de una forma distinta de la compilación condicional.

```csharp
       // Note: Cache the value instead of querying it more than once.
        bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
            ("Windows.Phone.UI.Input.HardwareButtons");

        if (isHardwareButtonsAPIPresent)
        {
            Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
                this.HardwareButtons_CameraPressed;
        }

    ...

    private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
    {
        // Handle the event.
    }
```

Consulta también [Detección de la plataforma en la que se está ejecutando la aplicación](wpsl-to-uwp-input-and-sensors.md).

## <a name="the-app-package-manifest"></a>Manifiesto del paquete de la aplicación

La configuración del proyecto (incluidas las referencias de SDK de extensión) determina el área de superficie de API que tu aplicación puede llamar. Pero el manifiesto del paquete de la aplicación es lo que determina el conjunto real de dispositivos en los que los clientes pueden instalar la aplicación desde la Tienda. Para obtener información, consulta los ejemplos de [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily).

Es importante saber cómo editar el manifiesto del paquete de la aplicación, porque en los temas siguientes se habla sobre el uso de diversas declaraciones, funcionalidades y otras opciones de configuración que algunas características necesitan. Puedes usar el editor de manifiestos del paquete de aplicaciones de Visual Studio para editarlo. Si el **Explorador de soluciones** no se muestra, selecciónalo en el menú **Ver**. Haga doble clic en **Package.appxmanifest**. Esta acción abre la ventana del editor de manifiestos. Selecciona la pestaña apropiada para realizar cambios y, luego, guárdalos. Asegúrate de que el elemento **pm:PhoneIdentity** en el manifiesto de la aplicación migrada coincide con lo que está en el manifiesto de la aplicación que vas a migrar (consulta el tema [**pm:PhoneIdentity**](/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity) para obtener más detalles).

Consulta [Referencia del esquema del manifiesto del paquete para Windows 10](/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

El siguiente tema es [Solución de problemas](wpsl-to-uwp-troubleshooting.md).