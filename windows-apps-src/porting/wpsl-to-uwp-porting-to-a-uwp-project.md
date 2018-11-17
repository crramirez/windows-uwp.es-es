---
author: stevewhims
description: Comenzar el proceso de migración creando un nuevo proyecto de Windows 10 en Visual Studio y copiando los archivos en él.
title: Migración de proyectos de WindowsPhone Silverlight a proyectos de UWP
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d1224c1707d3e86c9ddd309ecf06bd0c0767fb83
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7157650"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>Migración de proyectos de WindowsPhone Silverlight a proyectos de UWP


El tema anterior era [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md).

Comenzar el proceso de migración creando un nuevo proyecto de Windows 10 en Visual Studio y copiando los archivos en él.

## <a name="create-the-project-and-copy-files-to-it"></a>Crear el proyecto y copiar archivos en él

1.  Inicia Microsoft Visual Studio2015 y crea un nuevo proyecto de aplicación vacía (Windows Universal). Para obtener más información, consulta [rápidamente la aplicación de Windows Runtime 8.x con plantillas (C#, en C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232). Tu nuevo proyecto crea un paquete de la aplicación (un archivo appx) que se ejecutará en todas las familias de dispositivos.
2.  En el proyecto de aplicación WindowsPhone Silverlight, identifica todos los archivos de código fuente y los archivos de activos visuales que quieres reutilizar. Con el Explorador de archivos, copia los modelos de datos, los modelos de vista, los recursos visuales, los diccionarios de recursos, la estructura de carpetas y cualquier otro elemento que quieras reutilizar en tu nuevo proyecto. Copia o crea subcarpetas en el disco según sea necesario.
3.  Copia vistas (por ejemplo, MainPage.xaml y MainPage.xaml.cs) en el nodo del proyecto nuevo, también. De nuevo, crea nuevas subcarpetas según sea necesario y quita las vistas existentes del proyecto. Pero antes de sobrescribir o quitar una vista que Visual Studio ha generado, conserva una copia ya que puede resultar útil consultarla más adelante. La primera fase de migración de una aplicación WindowsPhone Silverlight se centra en que se parezca buena y funcionan bien en una familia de dispositivos. Más adelante, desviarás tu atención a asegurarte de que las vistas de adaptan correctamente a todos los factores de forma y, opcionalmente, a agregar código adaptable para obtener el máximo partido de una familia de dispositivos en particular.
4.  En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Selecciona los archivos que has copiado, haz clic con el botón secundario en ellos y haz clic en **Incluir en el proyecto**. Esto incluirá automáticamente sus carpetas contenedoras. Entonces puedes desactivar **Mostrar todos los archivos** si quieres. Un flujo de trabajo alternativo, si lo prefieres, es usar el comando **Agregar elemento existente**, después de crear las subcarpetas necesarias en el **Explorador de soluciones** de Visual Studio. Vuelve a comprobar que los recursos visuales tienen la opción **Acción de compilación** establecida en **Contenido** y la opción **Copiar en el directorio de resultados** establecida en **No copiar**.
5.  Las diferencias en los nombres de clases y de espacios de nombres generarán un gran número de errores de compilación en esta etapa. Por ejemplo, si abres las vistas que Visual Studio ha generado, verás que son del tipo [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) y no **PhoneApplicationPage**. Hay un gran número de diferencias de marcado XAML y de código imperativo que se explican con detalle en los temas siguientes de esta guía de migración. Pero progresarás más rápidamente siguiendo estos pasos generales: cambia "clr-namespace" por "using" en las declaraciones de prefijo del espacio de nombres en el marcado XAML; usa el tema [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md) y el comando de Visual Studio **Buscar y reemplazar** para crear cambios masivos en el código fuente (por ejemplo, reemplaza "System.Windows" por "Windows.UI.Xaml"); y en el editor de código imperativo de Visual Studio usa los comandos **Resolver** y **Organizar instrucciones Using** del menú contextual para los cambios más específicos.

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

A menos que la aplicación esté destinada a la familia de dispositivos que implementa la API, tendrás que usar la clase [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para probar la presencia de la API antes de llamarla (esto se denomina "código adaptable"). A continuación, se evaluará esta condición donde se ejecute la aplicación, pero solo se evaluará en true en dispositivos en los que la API está presente y, por tanto, está disponible para ser llamada. Usa únicamente los SDK de extensión y el código adaptable después de comprobar primero si existe una API universal. En la sección siguiente se proporcionan algunos ejemplos.

Consulta también [Manifiesto del paquete de la aplicación](#the-app-package-manifest).

## <a name="maximizing-markup-and-code-reuse"></a>Maximización de la reutilización de código y marcado

Encontrarás que refactorizar un poco o agregar código adaptable (lo cual se explica a continuación), te permitirá maximizar el marcado y el código que funciona en todas las familias de dispositivo. A continuación se indican más detalles.

-   Los archivos que son comunes a todas las familias de dispositivo no necesitan ninguna consideración especial. Esos archivos serán usados por la aplicación en todas las familias de dispositivos en las que se ejecuta. Esto incluye los archivos de marcado XAML, los archivos de código fuente imperativo y los archivos de recursos.
-   Es posible que tu aplicación detecte la familia de dispositivos en la que se está ejecutando y navegar a una vista que se ha diseñado específicamente para esa familia de dispositivos. Para obtener más información, consulta [Detección de la plataforma en la que se está ejecutando la aplicación](wpsl-to-uwp-input-and-sensors.md).
-   Una técnica similar que puede resultar útil si no existe otra alternativa es proporcionar a un archivo de marcado o archivo **ResourceDictionary** (o la carpeta que contiene el archivo) un nombre especial que se carga automáticamente en tiempo de ejecución solo cuando la aplicación se ejecuta en una familia de dispositivos en particular. Esta técnica se ilustra en el caso práctico [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md).
-   Para usar funciones que no están disponibles en todas las familias de dispositivos (por ejemplo, impresoras, escáneres o el botón de la cámara) puedes escribir código adaptable. Consulta el tercer ejemplo en [Compilación condicional y código adaptable](#conditional-compilation-and-adaptive-code) en este tema.
-   Si quieres admitir WindowsPhone Silverlight y Windows 10, es posible que puedas compartir archivos de código fuente entre proyectos. Pasos a seguir: en Visual Studio, haz clic con el botón derecho en el proyecto en **Explorador de soluciones**, selecciona **Agregar elemento existente**, selecciona los archivos para compartir y haz clic en **Agregar como vínculo**. Almacena tus archivos de código fuente en una carpeta común en el sistema de archivos donde puedan verlos los proyectos vinculados a ellos y no te olvides de agregarlos al control de código fuente. Si puedes factorizar el código fuente imperativo para que la mayor parte de un archivo, si no todo, funcione en ambas plataformas, no necesitas tener dos copias de este. Puedes encapsular la lógica específica de la plataforma del archivo dentro de directivas de compilación condicional siempre que sea posible, o condiciones de tiempo de ejecución cuando sea necesario. Consulta la sección siguiente y [Directivas de preprocesador de C#](http://msdn.microsoft.com/library/ed8yd1ha.aspx).
-   Para volver a usarse en el nivel binario, en lugar del nivel de código fuente, existen bibliotecas de clases portables que admiten el subconjunto de las API de .NET que están disponibles en WindowsPhone Silverlight, así como el subconjunto para aplicaciones de Windows 10 (núcleo. NET). Los conjuntos de bibliotecas de clases portables tienen compatibilidad binaria con estas plataformas .NET y más. Usa Visual Studio para crear un proyecto destinado a una biblioteca de clases portable. Consulta [Desarrollo multiplataforma con la biblioteca de clases portable](http://msdn.microsoft.com/library/gg597391.aspx).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilación condicional y código adaptable

Si quieres admitir WindowsPhone Silverlight y Windows 10 en un solo archivo de código, a continuación, puedes hacerlo. Si buscas en el proyecto de Windows 10 en las páginas de propiedades del proyecto, verás que el proyecto define WINDOWS\_UAP como un símbolo de compilación condicional. En general, puedes usar la siguiente lógica para realizar la compilación condicional.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Si tienes código que compartes entre una aplicación WindowsPhone Silverlight y una aplicación de Windows Runtime 8.x, es posible que ya tienes código fuente con una lógica como esta:

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

Si es así y ahora quieres además compatibilidad con Windows 10, a continuación, puedes hacerlo, demasiado.

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

Quizá hayas usado la compilación condicional para limitar el control del botón Atrás del hardware para Windows Phone. En Windows 10, el evento de botón Atrás es un concepto universal. Los botones Atrás implementados en hardware o en software generarán el evento [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596), por lo que ese es el que hay que controlar.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

Quizá hayas usado la compilación condicional para limitar el control del botón de la cámara del hardware para Windows Phone. En Windows 10, el botón de cámara de hardware es un concepto específico de la familia de dispositivos móviles. Debido a que se ejecutará un paquete de la aplicación en todos los dispositivos, cambiamos nuestra condición de tiempo de compilación a una condición de tiempo de ejecución mediante el uso de código adaptable. Para ello, usamos la clase [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para consultar en tiempo de ejecución la presencia de la clase [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557). **HardwareButtons** se define en el SDK de la extensión móvil, por lo que deberemos agregar una referencia a ese SDK al proyecto para poder compilar este código. No obstante, ten en cuenta que el controlador solo se ejecutará en un dispositivo que implementa los tipos definidos en el SDK de extensión móvil y que esa es la familia de dispositivos móviles. Por lo tanto, el siguiente código solo cuida el uso de las características que están presentes, aunque lo consigue de una forma distinta de la compilación condicional.

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

La configuración del proyecto (incluidas las referencias de SDK de extensión) determina el área de superficie de API que tu aplicación puede llamar. Pero el manifiesto del paquete de la aplicación es lo que determina el conjunto real de dispositivos en los que los clientes pueden instalar la aplicación desde la Tienda. Para obtener información, consulta los ejemplos de [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

Es importante saber cómo editar el manifiesto del paquete de la aplicación, porque en los temas siguientes se habla sobre el uso de diversas declaraciones, funcionalidades y otras opciones de configuración que algunas características necesitan. Puedes usar el editor de manifiestos del paquete de aplicaciones de Visual Studio para editarlo. Si el **Explorador de soluciones** no se muestra, selecciónalo en el menú **Ver**. Haz doble clic en **Package.appxmanifest**. Esta acción abre la ventana del editor de manifiestos. Selecciona la pestaña apropiada para realizar cambios y, luego, guárdalos. Asegúrate de que el elemento **pm:PhoneIdentity** en el manifiesto de la aplicación migrada coincide con lo que está en el manifiesto de la aplicación que vas a migrar (consulta el tema [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) para obtener más detalles).

Consulta la [referencia del esquema para Windows 10 de manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/dn934820).

El siguiente tema es [Solución de problemas](wpsl-to-uwp-troubleshooting.md).

