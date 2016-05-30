---
author: mcleblanc
description: Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto.
title: Solución de problemas de migración de Windows Runtime 8.x a UWP
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
---

# Solución de problemas de migración de Windows Runtime 8.x a UWP

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El tema anterior era [Migración del proyecto](w8x-to-uwp-porting-to-a-uwp-project.md).

Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto. Con esto en mente, puedes avanzar temporalmente si comentas o quitas cualquier código que no sea esencial y más tarde regresas allí para restaurar lo que has quitado. La tabla de solución de problemas de síntomas y soluciones de este tema puede resultarte útil en esta etapa, aunque no sustituye la lectura de los siguientes temas. Siempre puedes consultar la tabla a medida que avances a través de los temas posteriores.

## Seguimiento de problemas

Las excepciones de análisis XAML pueden ser difíciles de diagnosticar, especialmente si no hay ningún mensaje de error significativo dentro de la excepción. Asegúrate de que el depurador está configurado para capturar las primeras excepciones (para probar y capturar la excepción de análisis desde el principio). Podrás inspeccionar la variable de excepción en el depurador para determinar si el mensaje o HRESULT tiene información útil. Asimismo, consulta la ventana de salida de Visual Studio para comprobar si presenta mensajes de error emitidos por el analizador XAML.

Si tu aplicación finaliza y todo lo que sabes es que se produjo una excepción no controlada durante el análisis de marcado XAML, esto podría ser el resultado de una referencia a un recurso faltante (es decir, un recurso cuya clave existe para las aplicaciones Universal 8.1 pero no para aplicaciones de Windows 10 como, por ejemplo, algunas clave de estilo **TextBlock** del sistema). O bien, podría tratarse de una excepción dentro de un **UserControl**, un control personalizado o un panel de diseño personalizado.

Un último recurso es una división binaria. Quita aproximadamente la mitad del marcado de una página y vuelve a ejecutar la aplicación. De este modo, sabrás si el error está en a mitad que quitaste (que ahora debes restaurar) o en la mitad que *no* quitaste. Repite el proceso dividiendo la mitad que contiene el error y así sucesivamente, hasta que reduzcas a cero el problema.

## TargetPlatformVersion

En esta sección se explica qué hacer si, al abrir un proyecto de Windows 10 en Visual Studio, aparece el mensaje "Se requiere una actualización de Visual Studio. Uno o varios proyectos requieren un Platform SDK <version> que no está instalado o se incluye como parte de una actualización futura de Visual Studio".

-   En primer lugar, determina el número de versión del SDK de Windows 10 que tienes instalada. Ve a **C:\\Archivos de programa (x86)\\Windows Kits\\10\\Include\\<versionfoldername>** y toma nota de *<versionfoldername>*, que tendrá la notación cuádruple "Principal.Secundaria.Compilación.Revisión".
-   Abre el archivo de proyecto para editar y buscar los elementos `TargetPlatformVersion` y `TargetPlatformMinVersion`. Edítalos para que tengan un aspecto como el siguiente, sustituyendo *<versionfoldername>* con el número de versión en notación cuádruple que encontraste en el disco:

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## Solución de problemas de síntomas y soluciones

La información de las soluciones de la tabla está destinada a ofrecerte información suficiente para aclarar tu mente. A medida que leas los temas posteriores, encontrarás más detalles sobre cada uno de estos problemas.

| Síntoma | Solución |
|---------|--------|
| Al abrir un proyecto de Windows 10 en Visual Studio, aparece el mensaje "Se requiere una actualización de Visual Studio. Uno o varios proyectos requieren un platform SDK &lt;version&gt; que no está instalado o se incluye como parte de una actualización futura de Visual Studio". | Consulta la sección [TargetPlatformVersion](#targetplatformversion) en este tema. |
| Se genera una excepción System.InvalidCastException cuando se llama a InitializeComponent en un archivo xaml.cs.| Esto puede ocurrir cuando tienes más de un archivo xaml (al menos uno de los cuales está calificado como MRT) que comparten el mismo archivo xaml.cs y los elementos tienen atributos x:Name que no son coherentes entre los dos archivos xaml. Intenta agregar el mismo nombre a los mismos elementos en los dos archivos XAML u omite los nombres por completo. |
| Si se ejecuta en el dispositivo, la aplicación finaliza o, si se inicia desde Visual Studio, se muestra el error "No se puede activar la aplicación de la Tienda Windows \[…\]. Error en la solicitud de activación con el error "Windows no pudo comunicarse con la aplicación de destino. Esto indica normalmente que se anuló el proceso de la aplicación de destino. \[…\]”. | El problema podría ser el código imprescindible que se ejecuta en tus propias páginas o en las propiedades enlazadas (u otros tipos) durante la inicialización. O bien, el problema podría producirse al analizar el archivo XAML que estaba a punto de mostrarse cuando la aplicación finalizó (si se inicia desde Visual Studio, es la página de inicio). Busca claves de recurso no válidas o prueba algunas de las instrucciones de la sección "Seguimiento de problemas" de este tema.|
| El compilador o el analizador XAML (o una excepción en tiempo de ejecución) muestra el error "*No se pudo resolver el recurso '<resourcekey>'*". | La clave de recurso no es aplicable para las aplicaciones de la Plataforma universal de Windows (UWP) (este es el caso de algunos recursos de Windows Phone, por ejemplo). Busca el recurso equivalente correcto y actualiza el marcado. Algunos ejemplos que podrías encontrar al instante son las claves del sistema como `PhoneAccentBrush`. |
| El compilador de C# muestra el error "*No se puede encontrar el tipo o el nombre de espacio de nombres '<name>' \[...\]*" o "*El tipo o el nombre del espacio de nombres '<name>' no existe en el espacio de nombres \[...\]*" o "*El tipo o el nombre del espacio de nombres '<name>' no existe en el contexto actual*". | Es posible que esto signifique que el tipo se implemente en un SDK de extensión (aunque puede haber casos en los que la solución no sea tan fácil). Usa el contenido de referencia de las [API de Windows](https://msdn.microsoft.com/library/windows/apps/bg124285) para determinar con qué extensión implementa el SDK la API y, a continuación, usa el comando **Agregar** > **Referencia** de Visual Studio para agregar una referencia a ese SDK al proyecto. Si la aplicación está dirigida al conjunto de API que se conoce como la familia de dispositivos universales, es fundamental que uses la clase [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para probar en tiempo de ejecución la presencia del SDK de extensión antes de llamarlas (esto se denomina código adaptable). Si existe una API universal, siempre es preferible a una API de un SDK de extensión. Para obtener más información, consulta [SDK de extensión](w8x-to-uwp-porting-to-a-uwp-project.md#extension-sdks). |

El siguiente tema es [Migración de XAML y la interfaz de usuario](w8x-to-uwp-porting-xaml-and-ui.md).



<!--HONumber=May16_HO2-->


