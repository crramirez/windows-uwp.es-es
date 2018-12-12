---
title: Introducción a las herramientas de Xbox One
description: La herramienta específica de Xbox One, Dev Home, con Windows Device Portal.
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp, xbox one, tools, herramientas
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed106095d83ed0c6e055d22a1a0cf229380cff71
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8945897"
---
# <a name="introduction-to-xbox-one-tools"></a>Introducción a las herramientas de Xbox One

En esta sección se describe cómo acceder al Portal de dispositivos para Xbox a través de la aplicación Dev Home.

## <a name="dev-home"></a>Dev Home

Dev Home es una experiencia de herramientas del kit de desarrollo de Xbox One diseñada para ayudar en la productividad del desarrollador. Ofrece funcionalidad para administrar y configurar el kit de desarrollo.

Dev Home es la aplicación predeterminada que se abre cuando se inicia la consola en modo de desarrollador. También puedes abrir Dev Home seleccionando la ventana **Dev Home** en la pantalla principal. Si no ves ninguna ventana, la consola no está en modo de desarrollador.

Para obtener más información sobre Dev Home, consulta [Inicio del desarrollador en la consola (Dev Home)](dev-home.md).

## <a name="xbox-device-portal"></a>Portal de dispositivos para Xbox
El Portal de dispositivos para Xbox es una herramienta de administración de dispositivos basado en el explorador que permite agregar aplicaciones y juegos, agregar cuentas de prueba de Xbox Live, cambiar espacios limitados y mucho más.

Para habilitar el Portal de dispositivos para Xbox en la consola Xbox One:

1. Selecciona la ventana de **Dev Home** en la pantalla principal.

  ![Seleccionar la ventana de Dev Home](images/introduction-to-xbox-one-tools-1.png)

2. En Dev Home, ve a la pestaña **Inicio** y en la sección **Acceso remoto**, selecciona **Configuración de acceso remoto**.

  ![Herramienta de administración remota](images/introduction-to-xbox-one-tools-2.png)

3. Selecciona la casilla **Habilitar Portal de dispositivos para Xbox**.

4. En **Autenticación**, selecciona la casilla **Requerir autenticación para tener acceso remoto a esta consola desde la web o las herramientas de PC**.

5. Escribe un **Nombre de usuario** y __Contraseña__ y selecciona **Guardar**. Estas credenciales se usan para autenticar el acceso a tu kit de desarrollo desde un explorador.

6. Selecciona **Cerrar** y en la pestaña **Inicio**, escribe la dirección URL indicada en la herramienta **Acceso remoto**.

7. Escribe la dirección URL en el explorador. Recibirás una advertencia acerca del certificado proporcionado, similar a la siguiente captura de pantalla, porque el certificado de seguridad firmado por la consola Xbox One no se considera un editor de confianza conocido. En Edge, haz clic en **Detalles** y después en **Acceder a la página web** para acceder al Portal de dispositivos para Xbox.

    ![Advertencia de certificado de seguridad](images/introduction-to-xbox-one-tools-3.png)

8. Inicia sesión con las credenciales que hayas configurado.

## <a name="xbox-dev-mode-companion"></a>Complemento del modo de desarrollo de Xbox
Complemento de modo de desarrollo de Xbox es una herramienta que te permite trabajar en la consola sin salir del equipo. La aplicación permite ver la pantalla de la consola y enviarle elementos de entrada. Para obtener más información, consulta [Complemento de modo de desarrollo de Xbox](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Consulta también
- [Cómo usar Fiddler con Xbox One al desarrollar para la UWP](uwp-fiddler.md)
- [Introducción a Windows Device Portal](../debug-test-perf/device-portal.md)
- [UWP en Xbox One](index.md)