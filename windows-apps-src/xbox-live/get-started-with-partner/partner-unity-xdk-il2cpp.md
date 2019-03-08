---
title: Unity para XDK con backend IL2CPP
description: Agregar compatibilidad de Xbox Live a Unity para XDK con back-end IL2CPP secuencias de comandos para ID@Xbox y administrados asociados
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608470"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Agregar compatibilidad de Xbox Live a Unity para XDK con back-end IL2CPP secuencias de comandos para ID@Xbox y administrados asociados

## <a name="overview"></a>Introducción

Compatibilidad de tiempo de ejecución de Windows para IL2CPP en Unity

Con el lanzamiento de Unity 5.6f3 el motor ha incluido una nueva característica que permite a los desarrolladores usar los componentes de Windows en tiempo de ejecución (WinRT) directamente en la secuencia de comandos mediante la inclusión en el proyecto de juego directamente. Hasta que los 5.6 desarrolladores necesitaban un complemento o dll para admitir cualquier característica de plataforma (incluidos los SDK de Xbox Live) desde el script de juego. Esta nueva capa de proyección elimina la necesidad de complemento e introduce un flujo de trabajo nueva y simplificada compatible solo con juegos que elija el back-end de scripting IL2CPP.

Para obtener más información sobre cómo empezar, vea la documentación de Unity: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Pasos

**(1) instalar Unity**

Instale Unity 5.6 o posterior y asegúrese de que tiene instalada la extensión del editor Xbox One.

**(2) Instale Visual Studio Tools para Unity versión 3.1 y anteriormente para que IntelliSense admite al usar archivos Winmd** para Visual Studio 2015, se puede encontrar en https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Para Visual Studio 2017, puede agregar el componente dentro del instalador de Visual Studio 2017.

**(3) abra un proyecto Unity nuevo o existente**

**(4) se va a la plataforma Xbox One en el menú de configuración de compilación de Unity**

**(5) habilitar el back-end IL2CPP secuencias de comandos en la configuración del Reproductor de Unity y establece la compatibilidad con la API en .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**(6) se va a la secuencia de comandos compilador Roslyn**

**(7) las bibliotecas de sistema adecuado Xbox One todos se agregará automáticamente al proyecto y pasos adicionales no necesarios para incluir los archivos binarios de la plataforma.**

**8) para agregar y asociar una nueva C\# script a un objeto de Unity.**

Por ejemplo, haga clic en un objeto de Unity como la cámara"Main" y haga clic en "Agregar componente" \| "Nueva secuencia de comandos" \| C\# Script \| y asígnele el nombre "XboxLiveScript". Cualquier objeto de juego lo hará.

**9) abra el script en Visual Studio (con VSTU 3.1 + instalado)**

Observará que los dos proyectos, abra el script juego XboxLiveTest.cs en el proyecto de "Player" generado por VSTU

![](../images/unity/unity-il2cpp-2.png)

Esto es un proyecto especial que se genera para XDK e incluye referencias para los archivos winmd que ha colocado en los recursos.
También definirá la "#if ENABLE_WINMD_SUPPORT" definir automáticamente para IntelliSense y resaltado de sintaxis funcione correctamente.

**10) agregue el siguiente código de Xbox Live en el archivo de origen XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**11) asegúrese de que tiene capacidad de 'InternetClient' seleccionado en la configuración de publicación que se encuentra en configuración del Reproductor**

![](../images/unity/unity-il2cpp-3.png)

**12) compile el proyecto de Unity.**
