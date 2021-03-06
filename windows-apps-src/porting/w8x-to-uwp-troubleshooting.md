---
description: Solucionar los problemas que pueda tener al migrar Windows Runtime 8. x a UWP
title: Solución de problemas de migración de Windows Runtime 8.x a UWP
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0e915aec7f37600ce821f1a097a8aa5ac7ca76b
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133108"
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>Solución de problemas de migración de Windows Runtime 8.x a UWP


El tema anterior era [Migración del proyecto](w8x-to-uwp-porting-to-a-uwp-project.md).

Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto. Con esto en mente, puedes avanzar temporalmente si comentas o quitas cualquier código que no sea esencial y más tarde regresas allí para restaurar lo que has quitado. La tabla de solución de problemas de síntomas y soluciones de este tema puede resultarte útil en esta etapa, aunque no sustituye la lectura de los siguientes temas. Siempre puedes consultar la tabla a medida que avances a través de los temas posteriores.

## <a name="tracking-down-issues"></a>Seguimiento de problemas

Las excepciones de análisis XAML pueden ser difíciles de diagnosticar, especialmente si no hay ningún mensaje de error significativo dentro de la excepción. Asegúrate de que el depurador está configurado para capturar las primeras excepciones (para probar y capturar la excepción de análisis desde el principio). Podrás inspeccionar la variable de excepción en el depurador para determinar si el mensaje o HRESULT tiene información útil. Asimismo, consulta la ventana de salida de Visual Studio para comprobar si presenta mensajes de error emitidos por el analizador XAML.

Si tu aplicación finaliza y todo lo que sabes es que se produjo una excepción no controlada durante el análisis de marcado XAML, esto podría ser el resultado de una referencia a un recurso faltante (es decir, un recurso cuya clave existe para las aplicaciones Universal 8.1 pero no para aplicaciones de Windows 10 como, por ejemplo, algunas clave de estilo **TextBlock** del sistema). O bien, podría tratarse de una excepción que se produce dentro de un control **UserControl**, un control personalizado o un panel de diseño personalizado.

Un último recurso es una división binaria. Quita aproximadamente la mitad del marcado de una página y vuelve a ejecutar la aplicación. A continuación, sabrá si el error está en algún lugar dentro de la mitad que quitó (que ahora debe restaurar en cualquier caso) o en la mitad que *no* ha quitado. Repite el proceso dividiendo la mitad que contiene el error y así sucesivamente, hasta que reduzcas a cero el problema.

## <a name="targetplatformversion"></a>TargetPlatformVersion

En esta sección se explica qué hacer si, al abrir un proyecto de Windows 10 en Visual Studio, aparece el mensaje "Se requiere una actualización de Visual Studio. Uno o varios proyectos requieren un Platform SDK \<version\> que no está instalado o se incluye como parte de una actualización futura de Visual Studio".

-   En primer lugar, determina el número de versión del SDK de Windows 10 que tienes instalada. Vaya a **C: \\ archivos de programa (x86) \\ Windows \\ kits 10 \\ include \\ \<versionfoldername\> ** y tome nota de *\<versionfoldername\>* , que estará en notación cuádruple, "Major. minor. Build. revision".
-   Abre el archivo de proyecto para editar y buscar los elementos `TargetPlatformVersion` y `TargetPlatformMinVersion`. Edítela para que tenga el aspecto siguiente, reemplazando *\<versionfoldername\>* por el número de versión de notación cuádruple que encontró en el disco:

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Solución de problemas de síntomas y soluciones

La información de las soluciones de la tabla está destinada a ofrecerte información suficiente para aclarar tu mente. A medida que leas los temas posteriores, encontrarás más detalles sobre cada uno de estos problemas.

| Síntoma | Solución |
|---------|--------|
| Al abrir un proyecto de Windows 10 en Visual Studio, aparece el mensaje "Se requiere una actualización de Visual Studio. Uno o varios proyectos requieren un platform SDK &lt;versión&gt; que no está instalado o se incluye como parte de una actualización futura de Visual Studio". | Consulta la sección [TargetPlatformVersion](#targetplatformversion) en este tema. |
| Se genera una excepción System.InvalidCastException cuando se llama a InitializeComponent en un archivo xaml.cs.| Esto puede ocurrir cuando tienes más de un archivo xaml (al menos uno de los cuales está calificado como MRT) que comparten el mismo archivo xaml.cs y los elementos tienen atributos x:Name que no son coherentes entre los dos archivos xaml. Intenta agregar el mismo nombre a los mismos elementos en los dos archivos xaml u omite los nombres por completo. |
| Cuando se ejecuta en el dispositivo, la aplicación finaliza o cuando se inicia desde Visual Studio, aparece el error "no se puede activar la aplicación Windows Runtime 8. x... \[ \] . Error en la solicitud de activación con el error "Windows no pudo comunicarse con la aplicación de destino. Esto indica normalmente que se anuló el proceso de la aplicación de destino. \[…\]”. | El problema podría ser el código imprescindible que se ejecuta en tus propias páginas o en las propiedades enlazadas (u otros tipos) durante la inicialización. O bien, el problema podría producirse al analizar el archivo XAML que estaba a punto de mostrarse cuando la aplicación finalizó (si se inicia desde Visual Studio, es la página de inicio). Busca claves de recurso no válidas o prueba algunas de las instrucciones de la sección "Seguimiento de problemas" de este tema.|
| El compilador o el analizador XAML (o una excepción en tiempo de ejecución) muestra el error "*No se pudo resolver el recurso '\<resourcekey\>'*". | La clave de recurso no es aplicable para las aplicaciones de la Plataforma universal de Windows (UWP) (este es el caso de algunos recursos de Windows Phone, por ejemplo). Busca el recurso equivalente correcto y actualiza el marcado. Algunos ejemplos que podrías encontrar al instante son las claves del sistema como `PhoneAccentBrush`. |
| El compilador de C# muestra el error "no se*encontró el tipo o el nombre del espacio de nombres ' \<name\> ' \[ ... \] *" o "el*tipo o el nombre del espacio de nombres ' ' no \<name\> existe en el espacio de nombres \[ ... \] *" o "*el nombre del tipo o del espacio de nombres ' ' no \<name\> existe en el contexto actual*'. | Es posible que esto signifique que el tipo se implemente en un SDK de extensión (aunque puede haber casos en los que la solución no sea tan fácil). Usa el contenido de referencia de las [API de Windows](/uwp/) para determinar con qué extensión implementa el SDK la API y, a continuación, usa el comando **Agregar** > **Referencia** de Visual Studio para agregar una referencia a ese SDK al proyecto. Si la aplicación está dirigida al conjunto de API que se conoce como la familia de dispositivos universales, es fundamental que uses la clase [**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) para probar en tiempo de ejecución la presencia del SDK de extensión antes de llamarlas (esto se denomina código adaptable). Si existe una API universal, siempre es preferible a una API de un SDK de extensión. Para obtener más información, consulta [SDK de extensión](w8x-to-uwp-porting-to-a-uwp-project.md). |

El siguiente tema es [Migración de XAML y la interfaz de usuario](w8x-to-uwp-porting-xaml-and-ui.md).