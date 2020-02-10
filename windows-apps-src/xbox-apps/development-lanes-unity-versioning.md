---
title: 'Unity: Control de versiones del proyecto para UWP'
description: Control de versiones del proyecto para UWP.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: b98fba394fb326d60451f07938504e99a92d764d
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089491"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: Control de versiones del proyecto para UWP

¿Todavía no has compilado tu juego Unity para Xbox con la Plataforma universal de Windows (UWP)?  Como primer paso, consulta el documento [Llevar los juegos Unity a UWP en Xbox](development-lanes-unity.md).

Existen diferentes motivos por los que quisieras agregar partes de tu directorio de UWP generado al control de versiones, una de ellas es agregar dependencias (por ejemplo, el SDK de Xbox Live).  Usaremos este escenario como ejemplo para este tutorial y esperamos que te sirva de ayuda para resolver las necesidades individuales de tu proyecto.

***Declinación de responsabilidades: usaremos Git como solución de control de versiones.  Si el suyo es distinto, los conceptos todavía se deben traducir.***

Para refrescarte la memoria, este es el aspecto actual del directorio de nuestro juego, ***ScrapyardPhoenix***:

![Carpeta de destino de la compilación](images/build-destination.png)

Y este es el aspecto de nuestro directorio para UWP:

![Solución de VS para UWP](images/uwp-vs-solution.png)

En este directorio, lo único que nos interesa es una carpeta, la carpeta ***ScrapyardPhoenix*** (inserta el nombre de tu juego aquí).  En nuestro control de versiones, todo lo demás puede omitirse.

***¿No está familiarizado con lo que es un archivo. gitignore?  Vea [gitignore](https://git-scm.com/docs/gitignore).***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

Vamos a seleccionar unos cuantos archivos y carpetas diferentes de la carpeta **UWP/ScrapyardPhoenix** para agregarlos a nuestro control de versiones.  En primer lugar, echemos un vistazo general en detalle:

![Directorio de compilación de UWP](images/uwp-build-directory.png)  

## <a name="folders"></a>Folders  

`Assets` | ***incluyen*** | Contiene imágenes de Microsoft Store  
***omitir*** `Data`   | | Donde Unity compila el proyecto en (escenas, sombreadores, scripts, Prefabs, etc.)  
`Dependencies` | ***incluyen*** | Esta carpeta se creó para mantener todas las dependencias de UWP en (por ejemplo, XboxLiveSDK. dll).  
`Properties` | ***incluyen*** | Contiene opciones más avanzadas que puede modificar el desarrollador  
***omitir*** `Unprocessed` | | Contiene archivos de `.dll` y `.pdb` de Unity  

## <a name="files"></a>Files  

`App.cs` | ***incluyen*** | Punto de entrada para la aplicación de UWP; se puede modificar y ampliar con otros archivos de código fuente.  
`Package.appxmanifest` | ***incluyen*** | Archivo de origen del manifiesto del paquete de la aplicación para el paquete. msix o. appx  
`project.json` | ***incluyen*** | Describe los paquetes de NuGet de los que depende el `*.csproj`  
`ScrapyardPhoenix.csproj` | ***incluyen*** | Describe el destino de compilación de UWP; Si agrega dependencias adicionales al proyecto de UWP, este archivo `*.csproj` contendrá esa información.  
***omitir*** `ScrapyardPhoenix.csproj.user` | | Este archivo contiene información de usuario local

## <a name="resulting-gitignore"></a>Archivo .gitignore resultante

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

Y ya está; ahora tus compañeros de equipo estarán sincronizados con el proyecto para UWP que has generado. Ahora puedes agregar activos adicionales, código fuente y dependencias al proyecto UWP.

Puedes encontrarse algunos otros ejemplos de control de versiones de la carpeta UWP en [estos ejemplos](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## <a name="adding-dependencies-to-your-uwp-app"></a>Agregar dependencias a tu aplicación para UWP

Para agregar dependencias a archivos DLL y WINMD, colócalos en la carpeta **Assets** (Activos) de Unity en la carpeta **Plugins** (Complementos), luego selecciónalos y establece su configuración de plataforma de destino adecuadamente en el Inspector.

![Solución para UWP](images/uwp-solution.PNG)

***ScrapyardPhoenix (Universal Windows)*** es el proyecto al que agregarías una referencia; por ejemplo, el SDK de Xbox Live.

## <a name="see-also"></a>Vea también
- [Llevar los juegos existentes a Xbox](development-lanes-landing.md)
- [UWP en Xbox One](index.md)
