---
title: Introducción a las herramientas de Xbox One
description: Obtenga información sobre cómo acceder al portal de dispositivos de Xbox One mediante la aplicación de dev Home en el kit de desarrollo de Xbox One.
ms.date: 10/04/2017
ms.topic: article
keywords: Windows 10, UWP, Xbox One, herramientas
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed9df02ba929d170eca5b37e4376220e93e4902f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238320"
---
# <a name="introduction-to-xbox-one-tools"></a>Introducción a las herramientas de Xbox One

En esta sección se explica cómo acceder al portal de dispositivos de Xbox a través de la aplicación dev Home.

## <a name="dev-home"></a>Dev Home

Dev Home es una experiencia de herramientas del kit de desarrollo de Xbox One diseñada para ayudar en la productividad del desarrollador. Ofrece funcionalidad para administrar y configurar el kit de desarrollo.

Dev Home es la aplicación predeterminada que se abre cuando se inicia la consola en modo de desarrollador. También puede abrir dev Home seleccionando el icono de **dev Home** en la pantalla principal. Si no ves ninguna ventana, la consola no está en modo de desarrollador.

Para obtener más información sobre dev Home, consulte [la Página principal del Desarrollador en la consola (dev Home)](dev-home.md).

## <a name="xbox-device-portal"></a>Portal de dispositivos de Xbox
El portal de dispositivos de Xbox es una herramienta de administración de dispositivos basada en explorador que le permite agregar juegos y aplicaciones, agregar cuentas de prueba de Xbox Live, cambiar espacios aislados y mucho más.

Para habilitar el portal de dispositivos de Xbox en la consola Xbox One:

1. Seleccione el icono de **Inicio de dev** en la pantalla principal.

  ![Seleccionar la ventana de Dev Home](images/introduction-to-xbox-one-tools-1.png)

2. En dev Home, vaya a la pestaña **Inicio** y, en la sección **acceso remoto** , seleccione **configuración de acceso remoto**.

  ![Herramienta de administración remota](images/introduction-to-xbox-one-tools-2.png)

3. Active la casilla **Habilitar el portal de dispositivos Xbox** .

4. En **autenticación**, seleccione la casilla **requerir autenticación para acceder de forma remota a esta consola desde la web o las herramientas de PC** .

5. Escriba un **nombre de usuario** y una __contraseña__y seleccione **Guardar**. Estas credenciales se usan para autenticar el acceso al kit de desarrollo desde un explorador.

6. Seleccione **cerrar**y, en la pestaña **Inicio** , anote la dirección URL que aparece en la herramienta de **acceso remoto** .

7. Escriba la dirección URL en el explorador. Recibirá una advertencia sobre el certificado que se proporcionó, similar a la siguiente captura de pantalla, porque el certificado de seguridad firmado por la consola Xbox One no se considera un editor conocido y de confianza. En Edge, haga clic en **detalles** y, luego, **vaya a la página web** para acceder al portal de dispositivos de Xbox.

    ![Advertencia de certificado de seguridad](images/introduction-to-xbox-one-tools-3.png)

8. Inicie sesión con las credenciales que ha configurado.

## <a name="xbox-dev-mode-companion"></a>Complemento del modo de desarrollo de Xbox
Complemento de modo de desarrollo de Xbox es una herramienta que te permite trabajar en la consola sin salir del equipo. La aplicación permite ver la pantalla de la consola y enviarle elementos de entrada. Para obtener más información, consulta [Complemento de modo de desarrollo de Xbox](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Vea también
- [Cómo usar Fiddler con Xbox One al desarrollar para la UWP](uwp-fiddler.md)
- [Introducción al Portal de dispositivos Windows](../debug-test-perf/device-portal.md)
- [UWP en Xbox One](index.md)