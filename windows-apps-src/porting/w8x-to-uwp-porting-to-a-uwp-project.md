---
description: Tienes dos opciones cuando empieza el proceso de migración.
title: Migración de un proyecto de Windows Runtime 8.x a un proyecto de UWP
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 428f6787dfeb18d7ebf02f96acea2a6ab55c7fe7
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938596"
---
# <a name="porting-a-windows-runtime-8x-project-to-a-uwp-project"></a>Migración de un proyecto de Windows Runtime 8.x a un proyecto de UWP



Tienes dos opciones cuando empieza el proceso de migración. Una es editar una copia de los archivos de proyecto existentes, entre los que se incluye el manifiesto del paquete de la aplicación (para esta opción, consulta la información sobre cómo actualizar los archivos de proyecto en [Migrar aplicaciones a la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/mt148501.aspx)). La otra opción es crear un nuevo proyecto de Windows 10 en Visual Studio y copiar los archivos en él. En la primera sección de este tema se describe esa segunda opción, pero el resto del tema contiene información adicional aplicable a ambas opciones. También puedes mantener el nuevo proyecto de Windows 10 en la misma solución que tus proyectos existentes y compartir archivos de código fuente mediante un proyecto compartido. También puedes mantener el nuevo proyecto en una solución específica y compartir archivos de código fuente mediante la característica de archivos vinculados de Visual Studio.

## <a name="create-the-project-and-copy-files-to-it"></a>Crear el proyecto y copiar archivos en él

Estos pasos se centran en la opción de crear un nuevo proyecto de Windows 10 en Visual Studio y copiar los archivos en él. Algunos de los aspectos específicos relacionados con el número de proyectos que creas y qué archivos copias, dependerán de los factores y decisiones descritos en [Si tienes una aplicación Universal 8.1](w8x-to-uwp-root.md) y las secciones siguientes. Estos pasos suponen el caso más simple.

1.  Inicia Microsoft Visual Studio2015 y crea un nuevo proyecto de aplicación vacía (Windows Universal). Para obtener más información, consulta [rápidamente la aplicación de Windows Runtime 8.x con plantillas (C#, en C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232). Tu nuevo proyecto crea un paquete de la aplicación (un archivo appx) que se ejecutará en todas las familias de dispositivo.
2.  En tu proyecto de aplicación Universal 8.1, identifica todos los archivos de código fuente y los archivos de activos visuales que quieres reutilizar. Con el Explorador de archivos, copia los modelos de datos, los modelos de vista, los recursos visuales, los diccionarios de recursos, la estructura de carpetas y cualquier otro elemento que quieras reutilizar en tu nuevo proyecto. Copia o crea subcarpetas en el disco según sea necesario.
3.  Copia vistas (por ejemplo, MainPage.xaml y MainPage.xaml.cs) en el nuevo proyecto también. De nuevo, crea nuevas subcarpetas según sea necesario y quita las vistas existentes del proyecto. Pero antes de sobrescribir o quitar una vista que Visual Studio ha generado, conserva una copia, ya que puede resultar útil consultarla más adelante. La primera fase de migración de una aplicación Universal 8.1 se centra en que tenga un buen aspecto y funcione correctamente en una familia de dispositivos. Más adelante, desviarás tu atención a asegurarte de que las vistas de adaptan correctamente a todos los factores de forma y, opcionalmente, a agregar código adaptable para obtener el máximo partido de una familia de dispositivos en particular.
4.  En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Selecciona los archivos que has copiado, haz clic con el botón secundario en ellos y haz clic en **Incluir en el proyecto**. Esto incluirá automáticamente sus carpetas contenedoras. Entonces puedes desactivar **Mostrar todos los archivos** si quieres. Un flujo de trabajo alternativo, si lo prefieres, es usar el comando **Agregar elemento existente**, después de crear las subcarpetas necesarias en el **Explorador de soluciones** de Visual Studio. Vuelve a comprobar que los recursos visuales tienen la opción **Acción de compilación** establecida en **Contenido** y la opción **Copiar en el directorio de resultados** establecida en **No copiar**.
5.  Es probable que se vean algunos errores de compilación en esta fase. Pero si sabes lo que necesitas cambiar, puedes usar el comando **Buscar y reemplazar** de Visual Studio para realizar cambios masivos en tu código fuente; en el editor de código imperativo de Visual Studio, usa los comandos **Resolve** y **Organizar instrucciones Using** del menú contextual para realizar cambios más específicos.

## <a name="maximizing-markup-and-code-reuse"></a>Maximización de la reutilización de código y marcado

Encontrarás que refactorizar un poco o agregar código adaptable (lo cual se explica a continuación), te permitirá maximizar el marcado y el código que funciona en todas las familias de dispositivo. A continuación se indican más detalles.

-   Los archivos que son comunes a todas las familias de dispositivo no necesitan ninguna consideración especial. Esos archivos serán usados por la aplicación en todas las familias de dispositivos en las que se ejecuta. Esto incluye los archivos de marcado XAML, los archivos de código fuente imperativo y los archivos de recursos.
-   Es posible que tu aplicación detecte la familia de dispositivos en la que se está ejecutando y navegar a una vista que se ha diseñado específicamente para esa familia de dispositivos. Para obtener más información, consulta [Detección de la plataforma en la que se está ejecutando la aplicación](w8x-to-uwp-input-and-sensors.md).
-   Una técnica similar que puede resultar útil si no existe otra alternativa es proporcionar a un archivo de marcado o archivo **ResourceDictionary** (o la carpeta que contiene el archivo) un nombre especial que se carga automáticamente en tiempo de ejecución solo cuando la aplicación se ejecuta en una familia de dispositivos en particular. Esta técnica se ilustra en el caso práctico [Bookstore1](w8x-to-uwp-case-study-bookstore1.md).
-   Debe poder eliminar muchas de las directivas de compilación condicional en el código fuente de la aplicación Universal 8.1 si solo necesitas admitir Windows 10. Consulta [Compilación condicional y código adaptable](#conditional-compilation-and-adaptive-code) en este tema.
-   Para usar características que no están disponibles en todas las familias de dispositivos (por ejemplo, impresoras, escáneres o el botón de la cámara) puedes escribir código adaptable. Consulta el tercer ejemplo en [Compilación condicional y código adaptable](#conditional-compilation-and-adaptive-code) en este tema.
-   Si quieres admitir Windows 8.1, Windows Phone 8.1 y Windows 10, a continuación, puedes mantener tres proyectos en la misma solución y compartir código con un proyecto compartido. Como alternativa, puedes compartir archivos de código fuente entre proyectos. Pasos a seguir: en Visual Studio, haz clic con el botón derecho en el proyecto en **Explorador de soluciones**, selecciona **Agregar elemento existente**, selecciona los archivos para compartir y haz clic en **Agregar como vínculo**. Almacena tus archivos de código fuente en una carpeta común en el sistema de archivos donde puedan verlos los proyectos vinculados a ellos. Y no te olvides de agregarlos al control de origen.
-   Para la reutilización en el nivel binario, en lugar del nivel de código fuente, consulta [Crear componentes de Windows en tiempo de ejecución en C# y Visual Basic](http://msdn.microsoft.com/library/windows/apps/xaml/br230301.aspx). También hay bibliotecas de clases portables que admiten el subconjunto de las API de .NET que están disponibles en .NET Framework para Windows 8.1, Windows Phone 8.1 y aplicaciones de Windows 10 (núcleo. NET) y el conjunto completo de .NET Framework. Los conjuntos de bibliotecas de clases portables tienen compatibilidad binaria con estas plataformas. Usa Visual Studio para crear un proyecto destinado a una biblioteca de clases portable. Consulta [Desarrollo multiplataforma con la biblioteca de clases portable](http://msdn.microsoft.com/library/gg597391.aspx).

## <a name="extension-sdks"></a>SDK de extensiones

La mayoría de las API de Windows Runtime a las que tu aplicación Universal 8.1 ya llama se implementan en el conjunto de API denominado "familia de dispositivos universales". Pero algunas se implementan en los SDK de extensión y Visual Studio solo reconoce las API que están implementadas por la familia de dispositivos de destino de la aplicación o por los SDK de extensión a los que has hecho referencia.

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

Consulta también [Manifiesto del paquete de la aplicación](#app-package-manifest).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilación condicional y código adaptable

Si usas la compilación condicional (con C# directivas de preprocesador) para que los archivos de código funcionen en Windows 8.1 y Windows Phone 8.1, ahora puedes revisar esa compilación condicional a la luz del trabajo de convergencia realizado en Windows 10. Convergencia significa que, en la aplicación de Windows 10, algunas condiciones se pueden quitar por completo. Otras cambian a comprobaciones en tiempo de ejecución, como se muestra en los siguientes ejemplos.

**Nota**  si quieres admitir Windows 8.1, Windows Phone 8.1 y Windows 10 en un solo archivo de código, puedes hacerlo también. Si buscas en el proyecto de Windows 10 en las páginas de propiedades del proyecto, verás que el proyecto define WINDOWS\_UAP como un símbolo de compilación condicional. Por tanto, puedes usarlo en combinación con WINDOWS\_APP y WINDOWS\_PHONE\_APP. Estos ejemplos muestran el caso más simple de eliminación de la compilación condicional de una aplicación Universal 8.1 de sustitución del código equivalente para una aplicación de Windows 10.

Este primer ejemplo muestra el patrón de uso de la API **PickSingleFileAsync** (que se aplica únicamente a Windows 8.1) y la API **PickSingleFileAndContinue** (que se aplica únicamente a Windows Phone 8.1).

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

Windows 10 converge en [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) API, por lo que simplifica el código para esto:

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

En este ejemplo, se controla el botón Atrás de hardware, pero solo en Windows Phone.

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

En Windows 10, el evento de botón Atrás es un concepto universal. Los botones Atrás implementados en hardware o en software generarán el evento [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596), por lo que ese es el que hay que controlar.

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

Este ejemplo final es similar al anterior. Aquí tratamos el botón de la cámara de hardware, pero solo en el código compilado en el paquete de la aplicación de Windows Phone.

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

En Windows 10, el botón de cámara de hardware es un concepto específico de la familia de dispositivos móviles. Debido a que se ejecutará un paquete de la aplicación en todos los dispositivos, cambiamos nuestra condición de tiempo de compilación a una condición de tiempo de ejecución mediante el uso de código adaptable. Para ello, usamos la clase [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para consultar en tiempo de ejecución la presencia de la clase [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557). **HardwareButtons** se define en el SDK de la extensión móvil, por lo que deberemos agregar una referencia a ese SDK al proyecto para poder compilar este código. No obstante, ten en cuenta que el controlador solo se ejecutará en un dispositivo que implementa los tipos definidos en el SDK de extensión móvil y que esa es la familia de dispositivos móviles. Por lo tanto, este código es moralmente equivalente al código Universal 8.1 en que procura usar únicamente características que están presentes, aunque lo consiga de un modo distinto.

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

Consulta también [Detección de la plataforma en la que se está ejecutando la aplicación](w8x-to-uwp-input-and-sensors.md).

## <a name="app-package-manifest"></a>Manifiesto del paquete de la aplicación

El tema [Qué ha cambiado en Windows 10](https://msdn.microsoft.com/library/windows/apps/dn705793) enumera los cambios en la referencia del esquema del manifiesto del paquete para Windows 10, incluidos los elementos que se han agregado, quitados y cambiado. Para obtener información de referencia sobre todos los elementos, atributos y tipos del esquema, consulta [Jerarquía de elemento](https://msdn.microsoft.com/library/windows/apps/dn934819). Si vas a portar una aplicación de la Tienda de Windows Phone o si la aplicación es una actualización a una aplicación de la Tienda de Windows Phone, asegúrate de que el elemento **PM: phoneidentity** coincide con lo que está en el manifiesto de la aplicación anterior (usa los mismos GUID que la Tienda asignó a la aplicación). Esto garantiza que los usuarios de la aplicación que van a actualizar a Windows 10 recibirán tu nueva aplicación como una actualización, no un duplicado. Consulta el tema de referencia [**PM: phoneidentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) para obtener más detalles.

La configuración del proyecto (incluidas las referencias de SDK de extensión) determina el área de superficie de API que tu aplicación puede llamar. Pero el manifiesto del paquete de la aplicación es lo que determina el conjunto real de dispositivos en los que los clientes pueden instalar la aplicación desde la Tienda. Para obtener información, consulta los ejemplos de [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

Puedes editar el manifiesto del paquete de la aplicación para establecer varias declaraciones, capacidades y otras opciones de configuración que algunas funciones necesitan. Puedes usar el editor de manifiestos del paquete de aplicaciones de Visual Studio para editarlo. Si el **Explorador de soluciones** no se muestra, selecciónalo en el menú **Ver**. Haz doble clic en **Package.appxmanifest**. Esto abre la ventana del editor de manifiestos. Selecciona la pestaña apropiada para realizar cambios y luego guarda.

El siguiente tema es [Solución de problemas](w8x-to-uwp-troubleshooting.md).

## <a name="related-topics"></a>Temas relacionados

* [Desarrollar aplicaciones para la Plataforma universal de Windows.](http://msdn.microsoft.com/library/dn975273.aspx)
* [Rápidamente su tiempo de ejecución de Windows app 8.x con plantillas (C#, C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Creación de componentes de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Desarrollo multiplataforma con la biblioteca de clases portable](http://msdn.microsoft.com/library/gg597391.aspx)

