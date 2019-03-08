---
title: Usar XIM (Unity con IL2CPP)
description: Usando Xbox integrado multijugador con Unity para UWP con secuencias de comandos de IL2CPP back-end
ms.date: 04/03/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, Unity, Xbox integrado multijugador
ms.localizationpriority: medium
ms.openlocfilehash: a600fd253efae1daca34241b105a69514561e01d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646450"
---
# <a name="use-xim-unity-with-il2cpp"></a>Usar XIM (Unity con IL2CPP)

## <a name="overview"></a>Introducción

Compatibilidad de tiempo de ejecución de Windows para IL2CPP en Unity

Con el lanzamiento de Unity 5.6f3 el motor ha incluido una nueva característica que permite a los desarrolladores usar los componentes de Windows en tiempo de ejecución (WinRT) directamente en la secuencia de comandos mediante la inclusión en el proyecto de juego directamente. Hasta que los 5.6 desarrolladores necesitaban un complemento o dll para admitir cualquier característica de plataforma (incluido Xbox integrado Multiplayer) desde el script de juegos de UWP. Esta nueva capa de proyección elimina la necesidad de complemento e introduce un flujo de trabajo nueva y simplificada compatible solo con juegos que elija el back-end de scripting IL2CPP.

- Para obtener más información sobre cómo empezar a trabajar con el uso de WinRT y Unity, vea el [documentación de Unity](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html).
- Para obtener más información sobre cómo agregar compatibilidad de Xbox Live en Unity mediante IL2CPP, consulte el [documentación de Xbox Live](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp) sobre el tema.

## <a name="using-the-xim-unity-asset-package"></a>Mediante el paquete de activos de Unity XIM

### <a name="1-install-unity"></a>1. Instalación de Unity

Instalar Unity 5.6 o posterior y asegúrese de que tiene el **secuencias de comandos de Windows Store Il2CPP back-end** seleccionado durante la instalación

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. Instalar Visual Studio Tools para Unity versión 3.1 y anteriormente para que IntelliSense admite al usar archivos Winmd

Para Visual Studio 2015, se puede encontrar [en Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity). Para Visual Studio 2017, puede agregar el componente dentro del instalador de Visual Studio 2017.

### <a name="3-open-a-new-or-existing-unity-project"></a>3. Abra un proyecto Unity nuevo o existente

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. Cambiar la plataforma a la plataforma Universal de Windows en el menú de configuración de compilación de Unity

![El menú de configuración de compilación de Unity con la plataforma Universal de Windows compilar la configuración seleccionada](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. Habilitar back-end IL2CPP secuencias de comandos en la configuración del Reproductor de Unity y establezca la compatibilidad con la API en .NET 4.6

![La sección de configuración del menú de configuración del Reproductor de Unity con la opción "Compatibilidad con la Api" establecida en ".NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. Importar la versión más reciente del paquete de activos integrada participan varios jugadores WinRT Unity de Xbox

Esto se puede encontrar en https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. Ahora puede usar XIM en las secuencias de comandos

Para obtener instrucciones sobre cómo usar XIM con C#, consulte [usar XIM (C#)](using-xim-cs.md).

El fragmento de código siguiente muestra cómo se puede integrar XIM con el código:

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

Para obtener más información sobre la `ENABLE_WINMD_SUPPORT` #define (directiva), consulte la documentación de Unity en [soporte técnico de Windows en tiempo de ejecución](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)

### <a name="8-required-capability-content"></a>8. Contenido de la capacidad necesaria

Una aplicación mediante XIM inherentemente requiere conexión a y Aceptar conexiones de los recursos de red tanto a través de Internet y la red local. También requiere acceso a los dispositivos de micrófono para admitir el chat de voz. Como resultado, la aplicación debe declarar las funciones "InternetClientServer" y "PrivateNetworkClientServer" y la funcionalidad del dispositivo "Micrófono" en la configuración de publicación que se encuentra en configuración del Reproductor.

![Menú de capacidades de Unity con "InternetClientServer", "PrivateNetworkClientServer" y la capacidad de "Micrófono" seleccionada](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. Compile el proyecto de Unity.

1. Ir al archivo \| configuración de compilación, haga clic en **Universal Windows Platform** y asegúrese de que haga clic en **Cambiar plataforma**

2. Haga clic en "Agregar escenas abrir" para agregar la escena actual a la compilación

3. En el cuadro combinado SDK, elija "Universal 10"

4. En el cuadro combinado de tipo de compilación UWP, elija "D3D", pero "XAML" también funcionará si lo prefiere.

5. Haga clic en "Build" para que Unity generar el proyecto de UWP de Visual Studio que encapsula su juego Unity en una aplicación de UWP.

    Cuando se pedirá una ubicación, cree una carpeta nueva para evitar confusiones, ya que muchos de los archivos nuevos se crearán. Se recomienda llamar a la carpeta "Build" y, a continuación, seleccione esa carpeta.

6. Agregar manifiesto de la red de XIM al proyecto

    Agregue el archivo networkmanifest.xml:

    ![Propiedades de networkmanifest.xml de Visual Studio](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    Consulte [configuración del proyecto XIM](xim-manifest.md) para obtener más información sobre el manifiesto de la red y su contenido.

7. Compilar y ejecutar la aplicación para UWP desde Visual Studio

Esto iniciará la aplicación como una aplicación UWP normal y permitir las llamadas de Xbox Live operar como necesiten un contenedor de la aplicación para UWP a la función.

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. Volver a generar si realiza cambios en cualquier cosa en Unity

Si cambia cualquier cosa en Unity, debe recompilar el proyecto para UWP
