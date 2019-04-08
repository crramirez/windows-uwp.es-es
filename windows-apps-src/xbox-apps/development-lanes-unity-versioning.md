---
title: 'Unity: Control de versiones del proyecto para UWP'
description: Control de versiones del proyecto para UWP.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 064eaf42fe7d664be273cd7e2222fa5d90be1a11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608870"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: Control de versiones de su proyecto UWP

¿Todavía no has compilado tu juego Unity para Xbox con la Plataforma universal de Windows (UWP)?  Como primer paso, consulta el documento [Llevar los juegos Unity a UWP en Xbox](development-lanes-unity.md).

Existen diferentes motivos por los que quisieras agregar partes de tu directorio de UWP generado al control de versiones, una de ellas es agregar dependencias (por ejemplo, el SDK de Xbox Live).  Usaremos este escenario como ejemplo para este tutorial y esperamos que te sirva de ayuda para resolver las necesidades individuales de tu proyecto.

***Declinación de responsabilidades: Vamos a usar Git como nuestra solución de control de versión.  Si el suyo es diferente, todavía deben traducir los conceptos.***

Para refrescarte la memoria, este es el aspecto actual del directorio de nuestro juego, ***ScrapyardPhoenix***:

![Carpeta de destino de la compilación](images/build-destination.png)

Y este es el aspecto de nuestro directorio para UWP:

![Solución de VS para UWP](images/uwp-vs-solution.png)

En este directorio, lo único que nos interesa es una carpeta, la carpeta ***ScrapyardPhoenix*** (inserta el nombre de tu juego aquí).  En nuestro control de versiones, todo lo demás puede omitirse.

***¿No conoce qué .gitignore es un archivo?  Consulte [gitignore](https://git-scm.com/docs/gitignore).***

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

## <a name="folders"></a>Carpetas  

`Assets` | ***Incluir*** | Contiene las imágenes de Microsoft Store  
`Data`   | ***Omitir*** | Donde Unity compila el proyecto (escenas, sombreadores, Scripts, prefabricados, etcetera.)  
`Dependencies` | ***Incluir*** | Esta carpeta es uno que creó para mantener todas las dependencias UWP en (por ejemplo, XboxLiveSDK.dll)  
`Properties` | ***Incluir*** | Contiene la configuración más avanzada que se puede modificar por el desarrollador  
`Unprocessed` | ***Omitir*** | Contiene Unity `.dll` y `.pdb` archivos  

## <a name="files"></a>Archivos  

`App.cs` | ***Incluir*** | Punto de entrada para la aplicación para UWP; Esto se puede modificar y extender con otros archivos de origen  
`Package.appxmanifest` | ***Incluir*** | Archivo de manifiesto de origen del paquete de aplicación para su AppX  
`project.json` | ***Incluir*** | Describe los paquetes de NuGet su `*.csproj` depende de  
`ScrapyardPhoenix.csproj` | ***Incluir*** | Describe el destino de compilación UWP; Si agrega las dependencias adicionales a su UWP del proyecto, esto `*.csproj` archivo contendrán esa información  
`ScrapyardPhoenix.csproj.user` | ***Omitir*** | Este archivo contiene información de usuario local

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

## <a name="see-also"></a>Consulte también
- [Llevar los juegos existentes a Xbox](development-lanes-landing.md)
- [UWP en Xbox One](index.md)
