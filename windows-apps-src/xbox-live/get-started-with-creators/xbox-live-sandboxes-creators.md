---
title: Los espacios aislados de Xbox Live
description: Introducción a los espacios aislados de Xbox Live
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590230"
---
# <a name="xbox-live-sandboxes-introduction"></a>Introducción a los espacios aislados de Xbox Live

En el [configuración del servicio de Xbox Live](xbox-live-service-configuration-creators.md) artículo, se explicó que debe configurar la información sobre el título en [centro de partners](https://partner.microsoft.com/dashboard). Esta información incluye cosas como las estadísticas, marcadores, localización y mucho más. Los cambios realizados en la configuración del servicio Xbox Live deben publicarse desde el centro de partners en su espacio aislado de desarrollo antes de que los cambios son recogidos por el resto de Xbox Live y pueden tener acceso en su título.

Un espacio aislado de desarrollo permite trabajar en los cambios realizados en el título en un entorno aislado. Los espacios aislados ofrecen varias ventajas:

1. Puede iterar sobre los cambios en una actualización para el título sin que afecte a la versión que esté en directo en producción.
2. Por motivos de seguridad, algunas herramientas sólo funcionan en un espacio aislado de desarrollo.
3. Otros publicadores no pueden ver lo que está trabajando sin que se va a conceder acceso a su espacio aislado.

De forma predeterminada, las consolas Xbox One y equipos con Windows 10 se encuentran en el espacio aislado de venta directa. Deberá cambiar su PC o en Xbox One en el espacio aislado de desarrollo para tener acceso a esa versión de la configuración del servicio Xbox Live. Es importante recordar cambiar el dispositivo hasta que el espacio aislado de venta directa si necesita probar algo en el sector MINORISTA o desea tomarse un descanso para reproducir el favorito de Xbox Live game.

## <a name="finding-out-about-your-sandbox"></a>Averiguar sobre su espacio aislado

Cuando se crea un título, se crea un espacio aislado para usted. Puede encontrar el identificador de espacio aislado abriendo su producto en **centro de partners** y navegar a **servicios** > **Xbox Live**. El **Id. de espacio aislado** se mostrarán en la parte superior de la página.

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>Cambiar el espacio aislado de desarrollo de su PC
Puede cambiar su PC en el espacio aislado de desarrollo mediante Unity, Windows Device Portal (WPD) o a través de línea de comandos.

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>Requisitos previos
El siguiente debe llevarse a cabo antes de poder cambiar dentro y fuera del espacio aislado de desarrollo en Unity:

1. [Configurar Xbox Live en Unity](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>Cambiar los espacios aislados
Integrado en la configuración de Xbox Live ventana le permite alternar entre el desarrollo y los espacios aislados de venta directa con facilidad. Para empezar, vaya a **Xbox Live** > **configuración** en el menú. Puede ver el recinto de seguridad actual en el **configuración del modo de desarrollador** sección.

1. Si **modo de programador** dice **habilitado**, a continuación, que está actualmente en el espacio aislado de desarrollo asociado con su juego. Puede hacer clic en el **volver al modo de venta directa** botón para intercambiar.
2. Si **modo de programador** dice **deshabilitado**, a continuación, que está actualmente en el espacio aislado de venta directa. Puede hacer clic en el **cambiar al modo de programador** botón para activar su partición.

![XBL habilitado](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>Requisitos previos
El siguiente debe llevarse a cabo antes de cambiar su espacio aislado en Windows Device Portal (WPD):

1. [Portal del programa de instalación de dispositivos en el escritorio de Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>Cambiar los espacios aislados

1. Abra **Portal de desarrollo de Windows** conectándose a ella en el explorador web, como se describe en el [Portal del programa de instalación de dispositivos en el escritorio de Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) artículo.
2. Haga clic en **Xbox Live**.
3. Escriba su espacio aislado de desarrollo en el campo de texto y haga clic en **cambiar**.

![](../images/getting_started/wdp_switch_sandbox.png)

Para volver a la venta directa, puede escribir aquí comercial.

### <a name="command-line"></a>Línea de comandos

#### <a name="prerequisites"></a>Requisitos previos
El siguiente debe llevarse a cabo antes de poder cambiar dentro y fuera del espacio aislado de desarrollo a través de línea de comandos:

1. Descargue el paquete de herramientas de Live en Xbox en [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) y descomprímalo.

#### <a name="switch-sandboxes"></a>Cambiar los espacios aislados
1. Ejecute el archivo por lotes de SwitchSandbox.cmd **modo de administrador**.

Ejecútelo en modo de administrador para cambiar su espacio aislado. El primer argumento es el espacio aislado. Por ejemplo, si está intentando cambiar al espacio aislado MJJSQH.58, usaría este comando:

```cmd
SwitchSandbox.cmd MJJSQH.58
```

Para volver a la venta directa, basta con proporcionar como segundo argumento.

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Cambie su recinto de seguridad de desarrollo de consola Xbox One

### <a name="using-xbox-dev-portal"></a>Mediante el Portal de desarrollo de Xbox

Puede usar el Portal de desarrollo de Xbox para cambiar el espacio aislado en la consola. Para ello, vaya a [principal de desarrollo](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) en la consola y [habilitar Device Portal](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox). Una vez que abra el Portal de desarrollo de Xbox:

2. Haga clic en **Xbox Live**.
3. Escriba su espacio aislado de desarrollo en el campo de texto y pollito **cambiar**.

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>Mediante la interfaz de usuario de consola Xbox One

Puede usar [principal de desarrollo](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) cambiar directamente el espacio aislado en la consola:

1. Haga clic en **cambio Sandbox**, ubicado en **acciones rápidas**.
2. Escriba el identificador de espacio aislado y, a continuación, haga clic en **guardar y reinicie**.

### <a name="sign-in-with-the-xbox-app"></a>Inicie sesión con la aplicación de Xbox

Una vez que haya cambiado el equipo que se usará el recinto de seguridad adecuada para el título de desarrollo desea comprobar que ha iniciado sesión Xbox Live con una cuenta de prueba elegibles. Esto puede hacerse al iniciar sesión en el [aplicación de Xbox Live](https://www.xbox.com/en-US/xbox-app). Una vez que el entorno de desarrollo inicia con el espacio aislado deseado el App Xbox inicio de sesión de usuarios con las mismas restricciones como cualquier otro Xbox Live atenderá que se ejecutan en el espacio aislado. Esto resulta útil para comprobar que está utilizando una cuenta válida para el espacio aislado.
