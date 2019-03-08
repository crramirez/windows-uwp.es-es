---
title: Unity para UWP con secuencias de comandos de .NET
description: Agregar compatibilidad de Xbox Live a Unity para UWP con back-end .NET scripting para ID@Xbox y administrados asociados
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, Unity
ms.localizationpriority: medium
ms.openlocfilehash: 8c4ca9d58f89e215563adcc7985b978641efdf07
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594090"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>Agregar compatibilidad de Xbox Live a Unity para UWP con back-end .NET scripting para ID@Xbox y administrados asociados

**(1) instalar Unity**

Instalar Unity 5.3 o versiones posteriores y durante la unidad de proceso de instalación, compruebe el componente "Backend de secuencias de comandos de .NET de Windows Store".

![](../images/unity/unity1-install.png)

**(2) abra un proyecto Unity nuevo o existente**

Puede ser un proyecto 3D o 2D. Cualquiera de estos tipos funcionará con el SDK de Xbox Live.

**(3) importar la versión más reciente del paquete de activos de WinRT Unity de Xbox Live en que se puede encontrar https://github.com/Microsoft/xbox-live-api/releases**

**(4) agregue y adjuntar una nueva C\# script a un objeto de Unity.**

Por ejemplo, haga clic en un objeto de Unity como la cámara"Main" y haga clic en "Agregar componente" \| "Nueva secuencia de comandos" \| C\# Script \| y asígnele el nombre "XboxLiveScript". Cualquier objeto de juego lo hará.

**(5) compile el proyecto de Unity.**

1.  Ir al archivo \| configuración de compilación, haga clic en Windows Store y asegúrese de que haga clic en "Switch Platform"

2.  Haga clic en "Agregar escenas abrir" para agregar la escena actual a la compilación

3.  En el cuadro combinado SDK, elija "Universal 10"

4.  En el cuadro combinado de tipo de compilación UWP, elija "D3D", pero "XAML" también funcionará si lo prefiere.

5.  Haga clic en el "Unity C\# proyectos" casilla de verificación para generar los proyectos de ensamblado Csharp.dll

6.  Haga clic en "Build" para que Unity generar el proyecto de UWP de Visual Studio que encapsula su juego Unity en una aplicación de UWP. Cuando se pedirá una ubicación, cree una carpeta nueva para evitar confusiones, ya que muchos de los archivos nuevos se crearán. Se recomienda llamar a la carpeta "Build" y, a continuación, seleccione esa carpeta

![](../images/unity/unity3-buildsettings.png)


**(6) abra el proyecto UWP generado en Visual Studio**

Unity abrirá la carpeta de salida del proyecto en el explorador.  Omitir el archivo .sln no existe.  En su lugar, vaya a la carpeta de compilación y abra el generados .sln en Visual Studio.  

Verá 3 proyectos de esta solución.

1.  Assembly-CSharp. Esto es donde se encontrarán los scripts de Xbox Live

2.  Assembly-Csharp-firstpass. Este proyecto se puede omitir para nuestros propósitos.

3.  Según el nombre del proyecto de aplicación UWP. Se trata de una aplicación para UWP tradicional que hospeda el motor de Unity. Esto es donde lo establecerá algunas configuración Xbox Live similar a una aplicación UWP tradicional.


**(7) agregar configuración de Xbox Live a la aplicación para UWP**

Siga la página de documentación denominada [adición de Xbox Live a un proyecto UWP nuevo o existente](get-started-with-visual-studio-and-uwp.md)

**8) agregar código de Xbox Live a la secuencia de comandos**

Copie y pegue este ejemplo de código de Xbox Live en la secuencia de comandos que adjunta al objeto de juego. Esta secuencia de comandos aparecerá en el proyecto de "ensamblado CSharp". Puede cambiar el código según sea necesario.

```csharp
#if NETFX_CORE

using UnityEngine;
using System;
using Microsoft.Xbox.Services.System;

public class XboxLiveScript : MonoBehaviour
{
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();
    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
    string debugText = "";

    void Start()
    {
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;

        UIDispatcher = cw.Dispatcher;
        SignIn();
    }

    void Update()
    {
    }

    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }

    async void SignIn()
    {
        SignInResult result = await m_user.SignInAsync(UIDispatcher);

        if (result.Status == SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }
    }
}

#endif
```

**9) para compilar y ejecutar la aplicación para UWP desde Visual Studio**

Esto iniciará la aplicación como una aplicación UWP normal y permitir las llamadas de Xbox Live operar como necesiten un contenedor de la aplicación para UWP a la función.

**10) vuelva a compilar si realiza cambios en cualquier cosa en Unity**  
Si cambia cualquier cosa en Unity, debe volver a generar el proyecto para UWP.

Tenga en cuenta que Unity reemplazará el archivo pfx al volver a compilar lo que hará que Xbox Live iniciar sesión en un error, por lo que debe actualizar dentro del proyecto de Unity para evitar este problema.

Para ello, vaya al archivo \| configuración de compilación, haga clic en "Configuración de compilación" en el Reproductor de Windows Store y haga clic en el botón PFX para reemplazar el archivo PFX con la que obtuvo anteriormente. También puede eliminar el archivo PFX cada vez que se recompile el proyecto de Unity.

## <a name="troubleshooting-common-issues"></a>Solución de problemas comunes

**1)** Unity si tiene que un script asociado puede no cargarse y luego asegúrese de que se hizo el paso 3 para arrastrar el archivo WinMD en el panel activos de proyecto de Unity

**2)** si la aplicación bloquea inmediatamente durante el inicio o al intentar ejecutar esta línea de código:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Asegúrese de que ha agregado un archivo de texto xboxservices.config al proyecto y en sus propiedades, el conjunto "Acción de compilación" a "Contenido" y el conjunto de "Copy to Output Directory" a "Copiar siempre".

> [!NOTE]
> Todos los valores dentro de xboxservices.config distinguen mayúsculas de minúsculas.

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

d) el archivo .pfx estándar proporcionado por Unity no tendrá la identidad correcta, por lo que lo elimine desde el disco y quite la línea en el archivo .csproj que hace referencia a él o derecha, haga clic en el proyecto en Visual Studio y elija "Store" \| "asociar aplicación con el Store "que colocará hacia abajo de un archivo .pfx adecuado.  No olvide, a continuación, vaya a Unity, haga clic en "Configuración de compilación" en el Reproductor de Windows Store y haga clic en el botón PFX para reemplazar el archivo .pfx con la que obtuvo de la acción de "Asociar aplicación con el Store" de Visual Studio.
