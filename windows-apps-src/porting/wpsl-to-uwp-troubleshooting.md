---
description: Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto.
title: Solución de problemas de la migración de Windows Phone Silverlight a UWP
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a2c1353882ade36b5c1b82d0b75967d010ae0dc7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174829"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>Solución de problemas de la migración de Windows Phone Silverlight a UWP


El tema anterior era [Migración del proyecto](wpsl-to-uwp-porting-to-a-uwp-project.md).

Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto. Con esto en mente, puedes avanzar temporalmente si comentas o quitas cualquier código que no sea esencial y más tarde regresas allí para restaurar lo que has quitado. La tabla de solución de problemas de síntomas y soluciones de este tema puede resultarte útil en esta etapa, aunque no sustituye la lectura de los siguientes temas. Siempre puedes consultar la tabla a medida que avances a través de los temas posteriores.

## <a name="tracking-down-issues"></a>Seguimiento de problemas

Las excepciones de análisis XAML pueden ser difíciles de diagnosticar, especialmente si no hay ningún mensaje de error significativo dentro de la excepción. Asegúrate de que el depurador está configurado para capturar las primeras excepciones (para probar y capturar la excepción de análisis desde el principio). Podrás inspeccionar la variable de excepción en el depurador para determinar si el mensaje o HRESULT tiene información útil. Asimismo, consulta la ventana de salida de Visual Studio para comprobar si presenta mensajes de error emitidos por el analizador XAML.

Si tu aplicación finaliza y todo lo que sabes es que se produjo una excepción no controlada durante el análisis de marcado XAML, podría ser el resultado de una referencia a un recurso ausente (es decir, un recurso cuya clave existe para aplicaciones Windows Phone Silverlight pero no para aplicaciones de Windows 10 como, por ejemplo, algunas clave de estilo **TextBlock** del sistema). O bien, podría ser una excepción que se produce dentro de un control **UserControl**, un control personalizado o un panel de diseño personalizado.

Un último recurso es una división binaria. Quita aproximadamente la mitad del marcado de una página y vuelve a ejecutar la aplicación. A continuación, sabrá si el error está en algún lugar dentro de la mitad que quitó (que ahora debe restaurar en cualquier caso) o en la mitad que *no* ha quitado. Repite el proceso dividiendo la mitad que contiene el error y así sucesivamente, hasta que reduzcas a cero el problema.

## <a name="targetplatformversion"></a>TargetPlatformVersion

En esta sección se explica qué hacer si, al abrir un proyecto de Windows 10 en Visual Studio, aparece el mensaje "Se requiere una actualización de Visual Studio. Uno o varios proyectos requieren un platform SDK &lt;versión&gt; que no está instalado o se incluye como parte de una actualización futura de Visual Studio".

-   En primer lugar, determina el número de versión del SDK de Windows 10 que tienes instalada. Vaya a **C: \\ archivos de programa (x86) los \\ kits de Windows \\ 10 \\ incluyen \\ &lt; versionfoldername &gt; ** y tome nota de * &lt; versionfoldername &gt; *, que tendrá la notación cuádruple "Major. minor. Build. revision".
-   Abre el archivo de proyecto para editar y buscar los elementos `TargetPlatformVersion` y `TargetPlatformMinVersion`. Edítela para que tenga el siguiente aspecto y reemplace * &lt; versionfoldername &gt; * por el número de versión de notación cuádruple que encontró en el disco:

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Solución de problemas de síntomas y soluciones

La información de las soluciones de la tabla está destinada a ofrecerte información suficiente para aclarar tu mente. A medida que leas los temas posteriores, encontrarás más detalles sobre cada uno de estos problemas.

| Síntoma | Solución |
|---------|--------|
| El compilador o el analizador XAML muestra el error "_El nombre "&lt;nombreDeTipo&gt;" no existe en el espacio de nombres [...]._" | Si &lt;nombreDeTipo&gt; es un tipo personalizado, en las declaraciones de prefijo del espacio de nombres del marcado XAML, cambia "clr-namespace" a "using" y quita los tokens de ensamblado. Para los tipos de plataforma, esto significa que el tipo no se aplica a la Plataforma universal de Windows (UWP), por lo que debes buscar el equivalente y actualizar el marcado. Los ejemplos que podrían surgir al instante son `phone:PhoneApplicationPage` y `shell:SystemTray.IsVisible`. | 
| El compilador o el analizador XAML muestra el error "_El miembro"&lt;nombreDeMiembro&gt;"no se reconoce o no es accesible._" o "_La propiedad"&lt;NombreDePropiedad&gt;"no se encontró en el tipo [...]._". | Estos errores comenzarán a mostrarse después de haber migrado algunos nombres de tipo como, por ejemplo, la clase **Page** raíz. El miembro o la propiedad no se aplican a la UWP, por lo que debes buscar el equivalente y actualizar el marcado. Los ejemplos que podrían surgir al instante son `SupportedOrientations` y `Orientation`. |
| El compilador o el analizador XAML muestra el error "_No se encontró la propiedad [...] que se puede asociar [...]._" o "_Miembro que se puede asociar desconocido [...]._" | Es probable que la causa sea el tipo en lugar de la propiedad adjunta; en este caso, se mostrará un error de tipo, que desaparecerá al solucionarlo. Los ejemplos que podrían surgir al instante son `phone:PhoneApplicationPage.Resources` y `phone:PhoneApplicationPage.DataContext`. | 
|El compilador o el analizador XAML (o una excepción en tiempo de ejecución) muestra el error "_No se pudo resolver el recurso '&lt;claveDeRecurso&gt;'._" | La clave de recurso no se aplica a aplicaciones para la Plataforma universal de Windows (UWP). Busca el recurso equivalente correcto y actualiza el marcado. Algunos ejemplos que podrían surgir al instante son las claves de estilo **TextBlock** del sistema como, por ejemplo, `PhoneTextNormalStyle`. |
| El compilador de C# muestra el error "_No se puede encontrar el tipo o el nombre de espacio de nombres '&lt;nombre&gt;' [...]_" o "_El tipo o el nombre del espacio de nombres '&lt;nombre&gt;' no existe en el espacio de nombres [...]_" o "_El tipo o el nombre del espacio de nombres '&lt;nombre&gt;' no existe en el contexto actual_". | Probablemente esto signifique que el compilador aún no conoce el espacio de nombres de UWP correcto para un tipo. Usa el comando **Resolver** de Visual Studio para corregirlo. <br/>Si la API no está en el conjunto de API que se conoce como familia de dispositivos universales (en otras palabras, la API se implementa en un SDK de extensión), usa las [SDK de extensión](wpsl-to-uwp-porting-to-a-uwp-project.md).<br/>Puede haber otros casos en que la migración sea menos sencilla. Los ejemplos que podrían surgir al instante son `DesignerProperties` y `BitmapImage`. | 
|Cuando se ejecuta en el dispositivo, la aplicación finaliza o cuando se inicia desde Visual Studio, aparece el error "no se puede activar la aplicación Windows Runtime 8. x [...]. Error en la solicitud de activación con el error "Windows no pudo comunicarse con la aplicación de destino. Esto indica normalmente que se anuló el proceso de la aplicación de destino. […]”. | El problema podría ser el código imprescindible que se ejecuta en tus propias páginas o en las propiedades enlazadas (u otros tipos) durante la inicialización. O bien, el problema podría producirse al analizar el archivo XAML que estaba a punto de mostrarse cuando la aplicación finalizó (si se inicia desde Visual Studio, es la página de inicio). Busque claves de recursos no válidas o pruebe algunas de las instrucciones de la sección [problemas de seguimiento](#tracking-down-issues) de este tema.|
| _XamlCompiler error WMC0055: no se puede asignar el valor de texto ' &lt; Your Stream Geometry &gt; ' a la propiedad ' clip ' de tipo ' RectangleGeometry '_ | En UWP, el tipo de la aplicación para UWP de [Microsoft DirectX](/windows/desktop/directx) y XAML C++. |
| _Error de XamlCompiler WMC0001: tipo "RadialGradientBrush" desconocido en el espacio de nombres XML [...]_ | UWP no tiene el tipo **RadialGradientBrush**. Quitar el **RadialGradientBrush** del marcado y usa cualquier otro tipo de aplicación para UWP de [Microsoft DirectX](/windows/desktop/directx) y XAML C++. |
| _XamlCompiler error WMC0011: miembro desconocido ' OpacityMask ' en el elemento ' &lt; UIElement Type &gt; '_ | La aplicación para UWP de [Microsoft DirectX](/windows/desktop/directx) y XAML C++. |
| _Se produjo una primera excepción de tipo ' System. Runtime. InteropServices. COMException ' en SYSTEM.NI.DLL. Información adicional: la aplicación llamó a una interfaz que se serializó en un subproceso diferente. (Excepción de HRESULT: 0x8001010E (RPC_E_WRONG_THREAD))._ | El trabajo que estés haciendo debe realizarse en el subproceso de interfaz de usuario. Llama al [**CoreWindow.GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)). |
| Hay una animación en ejecución, pero no tiene ningún efecto en su propiedad de destino. | Haz que la animación sea independiente o establece `EnableDependentAnimation="True"` en ella. Vea [animación](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Al abrir un proyecto de Windows 10 en Visual Studio, aparece el mensaje "Se requiere una actualización de Visual Studio. Uno o varios proyectos requieren un platform SDK &lt;versión&gt; que no está instalado o se incluye como parte de una actualización futura de Visual Studio". | Consulta la sección [TargetPlatformVersion](#targetplatformversion) en este tema. |
| Se genera una excepción System.InvalidCastException cuando se llama a InitializeComponent en un archivo xaml.cs. | Esto puede ocurrir cuando tienes más de un archivo xaml (al menos uno de los cuales está calificado como MRT) que comparten el mismo archivo xaml.cs y los elementos tienen atributos x:Name que no son coherentes entre los dos archivos xaml. Intenta agregar el mismo nombre a los mismos elementos en los dos archivos xaml u omite los nombres por completo. | 

El siguiente tema es [Migración de XAML y la interfaz de usuario](wpsl-to-uwp-porting-xaml-and-ui.md).