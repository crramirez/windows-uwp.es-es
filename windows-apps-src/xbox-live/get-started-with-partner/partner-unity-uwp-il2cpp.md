---
title: Unity para UWP con backend IL2CPP
description: Agregar compatibilidad de Xbox Live a Unity para UWP con back-end IL2CPP secuencias de comandos para ID@Xbox y administrados asociados
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622920"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Agregar compatibilidad de Xbox Live a Unity para UWP con back-end IL2CPP secuencias de comandos para ID@Xbox y administrados asociados

## <a name="overview"></a>Introducción

Compatibilidad de tiempo de ejecución de Windows para IL2CPP en Unity

Con el lanzamiento de Unity 5.6f3 el motor ha incluido una nueva característica que permite a los desarrolladores usar los componentes de Windows en tiempo de ejecución (WinRT) directamente en la secuencia de comandos mediante la inclusión en el proyecto de juego directamente. Hasta que los 5.6 desarrolladores necesitaban un complemento o dll para admitir cualquier característica de plataforma (incluidos los SDK de Xbox Live) desde el script de juegos de UWP. Esta nueva capa de proyección elimina la necesidad de complemento e introduce un flujo de trabajo nueva y simplificada compatible solo con juegos que elija el back-end de scripting IL2CPP.

Para obtener más información sobre cómo empezar, vea la documentación de Unity: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Pasos

**(1) instalar Unity**

Instalar Unity 5.6 o posterior y asegúrese de que tiene el **secuencias de comandos de Windows Store Il2CPP back-end** seleccionado durante la instalación

**(2) Instale Visual Studio Tools para Unity versión 3.1 y anteriormente para que IntelliSense admite al usar archivos Winmd** para Visual Studio 2015, se puede encontrar en https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Para Visual Studio 2017, puede agregar el componente dentro del instalador de Visual Studio 2017.

**(3) abra un proyecto Unity nuevo o existente**

**(4) cambiar la plataforma a la plataforma Universal de Windows en el menú de configuración de compilación de Unity**

**(5) habilitar el back-end IL2CPP secuencias de comandos en la configuración del Reproductor de Unity y establece la compatibilidad con la API en .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**(6) importar la versión más reciente del paquete de activos de Unity de Xbox Live WinRT** se puede encontrar en https://github.com/Microsoft/xbox-live-api/releases

**(7) agregar y asociar una nueva C\# script a un objeto de Unity.**

Por ejemplo, haga clic en un objeto de Unity como la cámara"Main" y haga clic en "Agregar componente" \| "Nueva secuencia de comandos" \| C\# Script \| y asígnele el nombre "XboxLiveScript". Cualquier objeto de juego lo hará.

**8) abra el script en Visual Studio (con VSTU 3.1 + instalado)**

Observará que los dos proyectos, abra el script juego XboxLiveTest.cs en el proyecto de "Player" generado por VSTU

![](../images/unity/unity-il2cpp-2.png)

Esto es un proyecto especial que se genera para UWP e incluye referencias para los archivos winmd que ha colocado en los recursos.
También definirá la "#if ENABLE_WINMD_SUPPORT" definir automáticamente para IntelliSense y resaltado de sintaxis funcione correctamente.

**9) agregue el siguiente código de Xbox Live en el archivo de origen XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**10) asegúrese de que tiene capacidad de 'InternetClient' seleccionado en la configuración de publicación que se encuentra en configuración del Reproductor**

![](../images/unity/unity-il2cpp-3.png)

**11) compile el proyecto de Unity.**

1.  Ir al archivo \| configuración de compilación, haga clic en **Universal Windows Platform** y asegúrese de que haga clic en **Cambiar plataforma**

2.  Haga clic en "Agregar escenas abrir" para agregar la escena actual a la compilación

3.  En el cuadro combinado SDK, elija "Universal 10"

4.  En el cuadro combinado de tipo de compilación UWP, elija "D3D", pero "XAML" también funcionará si lo prefiere.

5.  Haga clic en "Build" para que Unity generar el proyecto de UWP de Visual Studio que encapsula su juego Unity en una aplicación de UWP. Cuando se pedirá una ubicación, cree una carpeta nueva para evitar confusiones, ya que muchos de los archivos nuevos se crearán. Se recomienda llamar a la carpeta "Build" y, a continuación, seleccione esa carpeta

**12) agregar configuración de Xbox Live a su proyecto**

Agregue el archivo xboxservices.config:

![](../images/unity/unity-il2cpp-4.png)

Siga la página de documentación denominada [adición de Xbox Live a un proyecto UWP nuevo o existente](get-started-with-visual-studio-and-uwp.md)

> [!NOTE]
> Todos los valores dentro de xboxservices.config distinguen mayúsculas de minúsculas.

**13) para compilar y ejecutar la aplicación para UWP desde Visual Studio**

Esto iniciará la aplicación como una aplicación UWP normal y permitir las llamadas de Xbox Live operar como necesiten un contenedor de la aplicación para UWP a la función.

**14) vuelva a compilar si realiza cambios en cualquier cosa en Unity**  
Si cambia cualquier cosa en Unity, debe volver a generar el proyecto para UWP.

Tenga en cuenta que Unity reemplazará el archivo pfx al volver a compilar lo que hará que Xbox Live iniciar sesión en un error, por lo que debe actualizar dentro del proyecto de Unity para evitar este problema.

Para ello, vaya al archivo \| configuración de compilación, haga clic en "Configuración de compilación" en el **Universal Windows Platform** Reproductor y haga clic en el botón PFX para reemplazar el archivo PFX de archivos con la que obtuvo anteriormente. También puede eliminar el archivo PFX cada vez que se recompile el proyecto de Unity.

## <a name="troubleshooting-common-issues"></a>Solución de problemas comunes

**1)** Unity si tiene que un script asociado puede no cargarse y luego asegúrese de que se hizo el paso 3 para arrastrar el archivo WinMD en el panel activos de proyecto de Unity

**2)** si la aplicación se bloquea inmediatamente durante el inicio o al intentar ejecutar esta línea de código:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Asegúrese de que ha agregado un archivo de texto xboxservices.config al proyecto y en sus propiedades, el conjunto "Acción de compilación" a "Contenido" y el conjunto de "Copy to Output Directory" a "Copiar siempre".
Asegúrese de que contiene JSON un formato adecuado con el TitleId en formato decimal, como:

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** si inicia la aplicación, pero se produce un error en el inicio de sesión, a continuación, compruebe lo siguiente:

(a) la máquina está establecida en el el espacio aislado para desarrolladores.  Para ello, use el script de SwitchSandbox.cmd en la carpeta \Tools de Xbox Live SDK.

(b) va a iniciar sesión con una cuenta de Xbox Live que tiene acceso al espacio aislado para desarrolladores.  Cuentas de Xbox Live comercial normal no tienen acceso.  Puede usar XDP o centro de partners para crear cuentas de prueba.

c) a su package.appxmanfiest en su aplicación para UWP se establece en la identidad correcta.  Puede editar esta opción manualmente, pero es la manera más fácil solucionar este problema haga clic con el botón derecho en el proyecto en Visual Studio y elija "Store" \| "Asociar aplicación con el Store".

d) el archivo .pfx estándar proporcionado por Unity no tendrá la identidad correcta, por lo que lo elimine desde el disco y quite la línea en el archivo .csproj que hace referencia a él o derecha, haga clic en el proyecto en Visual Studio y elija "Store" \| "asociar aplicación con el Store "que colocará hacia abajo de un archivo .pfx adecuado.  Asegúrese, a continuación, para volver a Unity, haga clic en "Configuración de compilación" en el **Universal Windows Platform** Reproductor y haga clic en el botón PFX para reemplazar el archivo .pfx con la que se obtuvo de la acción de "Asociar aplicación con el Store" de Visual Studio.
