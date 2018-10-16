---
author: laurenhughes
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: Crear y convertir una asignación de grupo de contenido de origen
description: Para preparar la aplicación de la Plataforma universal de Windows (UWP) para la instalación en streaming de la aplicación para UWP, tienes que crear una asignación de grupo de contenido. En este artículo encontrarás detalles, recomendaciones y consejos que te serán de utilidad para crear y convertir una asignación de grupo de contenido.
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, asignación de grupo de contenido, instalación en streaming, instalación en streaming de la app para uwp, asignación de grupo de contenido de origen
ms.localizationpriority: medium
ms.openlocfilehash: 4ce32958d5a99dc9f3f772d6272450a4f2b0f81b
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4616424"
---
# <a name="create-and-convert-a-source-content-group-map"></a>Crear y convertir una asignación de grupo de contenido de origen

Para preparar la aplicación de la Plataforma universal de Windows (UWP) para la instalación en streaming de la aplicación para UWP, tienes que crear una asignación de grupo de contenido. En este artículo encontrarás detalles, recomendaciones y consejos que te serán de utilidad para crear y convertir una asignación de grupo de contenido.

## <a name="creating-the-source-content-group-map"></a>Crear la asignación de grupo de contenido de origen

Tendrás que crear un archivo `SourceAppxContentGroupMap.xml` y, a continuación, usar Visual Studio o la herramienta **MakeAppx.exe** para convertir este archivo en la versión final: `AppxContentGroupMap.xml`. Es posible omitir un paso creando el `AppxContentGroupMap.xml` desde cero, aunque te recomendamos (además de que suele ser más fácil) que crees `SourceAppxContentGroupMap.xml` y lo conviertas, ya que no se suelen admitir los caracteres comodín en `AppxContentGroupMap.xml` (y estos son realmente útiles). 

Veamos un sencillo escenario en el cual resulta de utilidad la instalación en streaming de la aplicación para UWP. 

Supongamos que has creado un juego para UWP, pero el tamaño de la aplicación final es de más de 100GB. Que se va a tardar mucho tiempo en descargar desde Microsoft Store, que puede ser conveniente. Si decides usar la instalación en streaming de la aplicación para UWP, puedes especificar en qué orden se descargarán los archivos de la aplicación. Si indicas a la Tienda que se deben descargar primero los archivos esenciales, el usuario podrá interactuar con tu aplicación mucho antes, mientras que los archivos no esenciales se descargan en segundo plano.

> [!NOTE]
> Recuerda que usar la instalación en streaming de aplicaciones para UWP depende totalmente de la organización de archivos de la aplicación. A la hora de realizar la instalación en streaming de la aplicación para UWP, te recomendamos que tengas en cuenta cuanto antes el diseño del contenido de la aplicación, para que puedas distribuir los archivos de la misma de una manera más sencilla.

Primero, crearemos un archivo `SourceAppxContentGroupMap.xml`.

Antes de entrar en detalles, aquí te mostramos un ejemplo de un archivo `SourceAppxContentGroupMap.xml` sencillo y completo:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

Existen dos componentes principales en la asignación de grupo de contenido: la sección **necesarios**, que contiene el grupo de contenido necesario y la sección **automáticos**, que puede contener varios grupos de contenido automáticos.

### <a name="required-content-group"></a>Grupo de contenido necesario

El grupo de contenido necesario es un solo grupo de contenido que se encuentra en el elemento `<Required>` de `SourceAppxContentGroupMap.xml`. Un grupo de contenido necesario debe contener todos los archivos necesarios para poder iniciar la aplicación con una experiencia de usuario mínima. Debido a la compilación de .NET Native, todo el código (es decir, el código ejecutable de la aplicación) debe formar parte del grupo necesario y dejar los activos y otros archivos a los grupos automáticos.

Por ejemplo, si la aplicación es un juego, el grupo necesario puede incluir archivos que se usan en el menú o en la pantalla principal del juego.

Este es el fragmento de código del archivo de ejemplo original `SourceAppxContentGroupMap.xml`: 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

Estos son algunos puntos importantes que tienes que tener en cuenta:

- El elemento `<ContentGroup>` que se encuentra en `<Required>` **debe** denominarse "Required". Este nombre está reservado únicamente para el grupo de contenido necesario y no puede usarse con ningún otro elemento `<ContentGroup>` de la asignación de grupo de contenido final.
- Solo hay un elemento `<ContentGroup>`. Esto es intencionado, ya que debe haber un solo grupo de archivos esenciales.
- El archivo de este ejemplo es un único archivo `.exe`. Un grupo de contenido necesario no se limita a un solo archivo, sino que puede haber varios. 

Una manera sencilla de empezar a escribir en el archivo, es abrir una nueva página en un editor de texto, guardar el archivo mediante la opción "Guardar como" en la carpeta de proyecto de la aplicación y denominar el archivo creado de la siguiente manera: `SourceAppxContentGroupMap.xml`.

> [!IMPORTANT]
> Si estás desarrollando una aplicación para UWP de C++, necesitas ajustar las propiedades de archivo de `SourceAppxContentGroupMap.xml`. Establece la propiedad `Content` en **true** y la propiedad `File Type` en **archivo XML**. 

Una vez estés creando `SourceAppxContentGroupMap.xml`, es buena idea usar los caracteres comodín de los nombres de archivo; para obtener más información, consulta la sección [Sugerencias y trucos para usar caracteres comodín](#wildcards).

Si has desarrollado tu aplicación con Visual Studio, te recomendamos que lo incluyas en el grupo de contenido necesario:

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

Si agregas el nombre de un archivo comodín único, se incluirán los archivos que se agregaron al proyecto desde Visual Studio, como la aplicación ejecutable o los archivos DLL. Las carpetas WinMetadata y Propiedades se usarán para incluir el resto de carpetas que cree Visual Studio. Las ventajas de los caracteres comodín es que te permiten seleccionar las imágenes de logotipo y de SplashScreen necesarias para poder instalar la aplicación.

Recuerda que no puedes usar el carácter comodín doble "**" en la raíz de la estructura de archivos para incluir todos los archivos en el proyecto, ya que esto crearía un error al intentar convertir `SourceAppxContentGroupMap.xml` al archivo `AppxContentGroupMap.xml`.

Asimismo, también es importante que recuerdes que los archivos de superficie (AppxManifest.xml, AppxSignature.p7x, resources.pri, etc.) no deben incluirse en la asignación de grupo de contenido. Si incluyes archivos de superficie en uno de los nombres de archivo comodín especificados, estos se ignorarán.

### <a name="automatic-content-groups"></a>Grupos de contenido automáticos

Los grupos de contenido automáticos son activos que se descargan en segundo plano mientras el usuario está interactuando con los grupos de contenido ya descargados. Estos grupos contienen archivos adicionales que no son esenciales para iniciar la aplicación. Por ejemplo, puedes reestructurar grupos de contenido automáticos en niveles diferentes definiendo cada nivel como un grupo de contenido independiente. Tal como se explicó en la sección del grupo de contenido necesario: debido a la compilación de .NET Native, todo el código (es decir, el archivo ejecutable de la aplicación) debe formar parte del grupo necesario, dejando los activos y otros archivos para los grupos automáticos.

Echemos un vistazo al grupo de contenido automático de nuestro ejemplo `SourceAppxContentGroupMap.xml`:
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

El diseño del grupo automático es muy similar al grupo necesario, pero tiene unas pocas excepciones:

- Existen varios grupos de contenido.
- Los grupos de contenido automáticos pueden tener nombres exclusivos, excepto el nombre "Required", que está reservado para el grupo de contenido necesario.
- Los grupos de contenido automáticos no pueden tener **ningún** archivo del grupo de contenido necesario. 
- Un grupo de contenido automático puede contener archivos que también estén en otros grupos de contenido automáticos. Los archivos se descargarán una sola vez y desde el primer grupo de contenido automático que los contenga.

#### Sugerencias y trucos para usar caracteres comodín<a name="wildcards"></a>

El diseño del archivo de la asignación de grupos de contenido siempre está relacionado con la carpeta raíz del proyecto.

En nuestro ejemplo, se usan los caracteres comodín en ambos elementos `<ContentGroup>` para recuperar todos los archivos que se encuentran en el nivel de archivo "Assets\Level2" o "Assets\Level3". Si estás usando una estructura de carpetas más profunda, puedes usar el carácter comodín doble:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

También puedes usar caracteres comodín con texto para los nombres de archivo. Por ejemplo, si quieres incluir todos los archivos en la carpeta "Assets" con un nombre de archivo que contenga el "Level2" puedes usar algo así:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>Convert SourceAppxContentGroupMap.xml to AppxContentGroupMap.xml

Para convertir `SourceAppxContentGroupMap.xml` a su versión final `AppxContentGroupMap.xml`, puedes usar Visual Studio 2017 o la herramienta de línea de comandos **MakeAppx.exe**.

Para usar Visual Studio y así convertir la asignación de grupos de contenido:
1. Agrega `SourceAppxContentGroupMap.xml` a la carpeta del proyecto
2. Cambia la acción de compilación de `SourceAppxContentGroupMap.xml` a "AppxSourceContentGroupMap" en la ventana Propiedades
2. Haz clic con el botón derecho en el explorador de soluciones
3. Ve a la Tienda -> Convierte el archivo de asignación de grupos de contenido

Si no desarrollaste la aplicación en Visual Studio o si prefieres usar la línea de comandos, usa la herramienta **MakeAppx.exe** para convertir `SourceAppxContentGroupMap.xml`. 

Un comando **MakeAppx.exe** sencillo tiene un aspecto similar al siguiente:
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

La opción /s especifica la ruta de acceso a `SourceAppxContentGroupMap.xml`, y /f especifica la ruta de acceso a `AppxContentGroupMap.xml`. La última opción, /d, especifica el directorio que debe usarse para expandir los caracteres comodín del nombre de archivo; en este caso, es el directorio del proyecto de la aplicación.

Para obtener más información sobre las opciones que puedes usar con **MakeAppx.exe**, abre un símbolo del sistema, ve a **MakeAppx.exe** y escribe:

```syntax
MakeAppx convertCGM /?
```

Eso es todo lo que tendrás que hacer para que el último archivo `AppxContentGroupMap.xml` esté listo en la aplicación. No hay aún más que hacer antes de que la aplicación esté totalmente lista en Microsoft Store. Para obtener más información sobre el proceso completo de la instalación en streaming de aplicaciones para UWP, consulta [esta entrada de blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).
