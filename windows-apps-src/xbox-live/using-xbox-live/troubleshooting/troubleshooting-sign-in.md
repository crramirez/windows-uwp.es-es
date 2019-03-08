---
title: Solución de problemas de Xbox Live inicio de sesión
description: Obtenga información sobre cómo solucionar problemas relacionados con el inicio de sesión Xbox Live.
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, inicio de sesión, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: ff8d66105d8a1a44708bf23a681767a3044cb654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630330"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Solución de problemas de Xbox Live inicio de sesión

Hay varios problemas que pueden causar dificultades para inicio de sesión.  Si sigue los pasos descritos en [empezar a trabajar con Visual Studio para juegos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) puede minimizar la oportunidad de los errores inesperados.

## <a name="common-issues"></a>Problemas comunes

### <a name="sandbox-problems"></a>Problemas de espacio aislado

Por lo general, debe familiarizarse con el concepto de espacios aislados y cómo se relacionan con la Xbox Live.  Puede ver más información en el [espacios aislados de Xbox Live](../../xbox-live-sandboxes.md) guía.

En pocas palabras, los espacios aislados de exigir un control aislamiento y acceso a contenido antes de la versión comercial.  Usuarios sin acceso a su espacio aislado de desarrollo no se pueden realizar cualquier lectura o escribir las operaciones que pertenecen a su título.  También puede publicar las variaciones de configuración de servicio a diferentes entornos limitados para las pruebas.

Algunos aspectos que debe tener cuidado con lo que respecta a los espacios aislados se describen a continuación.

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>Cuenta de desarrollador no tiene acceso al espacio aislado adecuado para el acceso de tiempo de ejecución

* Se debe usar una cuenta de prueba (también conocida como cuenta de desarrollo) o una cuenta de desarrollador autorizados para iniciar sesión en un título que se encuentra en desarrollo.  Asegúrese de que está intentando iniciar sesión con uno o se crean las otras cuentas de prueba en XDP en [ https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts ](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts). Puede autorizar a una cuenta de xbox live para desarrolladores asociada en el centro de partners en [https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* Asegúrese de que la cuenta tiene acceso a su título se publica en el recinto de seguridad.  Las cuentas de prueba creadas en XDP heredan los permisos de la cuenta XDP que los creó

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>El dispositivo no está en el recinto de seguridad correcta

El dispositivo en que está desarrollando debe establecerse en un espacio aislado de desarrollo.  Puede establecer el uso de espacio aislado en Xbox One *Xbox One Manager*.  Para Windows 10 Desktop, puede usar la secuencia de comandos SwitchSandbox.cmd que se encuentra en el directorio de herramientas de la instalación del SDK de Xbox Live.

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>Configuración del servicio de su título no está publicado en el espacio aislado de desarrollo correcto

Asegúrese de que se publica la configuración del servicio de su título en un espacio aislado de desarrollo.  No se puede iniciar sesión en Xbox Live en un espacio aislado de desarrollo determinado para un título, a menos que title se publica en el mismo espacio aislado.  Consulte la [documentación XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) para obtener información sobre cómo hacerlo. Puede leer el [Partner Center documentación](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration) para aprender a publicar la configuración del centro de partners.

### <a name="ids-configured-incorrectly"></a>Identificadores de configurados incorrectamente

Hay varias piezas de Id. necesario para configurar el juego.  Puede ver más información en [empezar a trabajar con Visual Studio para juegos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) y [Introducción a entre jugar a juegos](../../get-started-with-partner/get-started-with-cross-play-games.md) según el tipo de título está creando.

Algunos aspectos a tener en cuenta son:

* Asegurarse de que escribir correctamente el identificador de la aplicación en XDP o centro de partners
* Asegúrese de que escribe correctamente el PFN en XDP o centro de partners
* Compruebe que haya creado un xboxservices.config en el mismo directorio que el proyecto de Visual Studio como se describe en el [adición de Xbox Live a un proyecto UWP nuevo o existente](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) guía.
* Asegúrese de que la "identidad del paquete" en su appxmanifest es correcta.  Esto se muestra en el centro de partners como "/ identidad/nombre del paquete" en la sección identidad de aplicación.

### <a name="title-id-or-scid-not-configured-correctly"></a>Id. de título o ¿SCID no está configurado correctamente

* Para los títulos UWP, el título de la Id. y ¿SCID debe establecerse en el valor correcto en el archivo xboxservices.config.  Asegúrese también de que este archivo es el formato correcto como UTF8.  Puede ver más información en [empezar a trabajar con Visual Studio para juegos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md). El archivo xboxservices.config distingue mayúsculas de minúsculas.
* Para los títulos XDK, estos valores se establecen en el archivo package.appxmanifest.
* Puede ver ejemplos de UWP y XDK configuración título en el directorio de ejemplos del SDK de Xbox Live.

## <a name="test-using-the-xbox-app"></a>Probar mediante el App Xbox

Si está desarrollando una aplicación de UWP, puede depurar algunos problemas mediante el App Xbox:

1. Establecer el espacio aislado del dispositivo a un espacio aislado de desarrollo mediante el script SwitchSandbox.cmd
2. Abra el App Xbox e intente iniciar sesión con una cuenta de prueba con acceso al mismo espacio aislado.

Si es capaz de iniciar sesión correctamente, a continuación, comprueba que el espacio aislado de desarrollo se ha configurado correctamente en el dispositivo y la cuenta de prueba tiene acceso a él.

Si se siguen produciendo errores de inicio de sesión, es probable que la configuración del servicio no está publicada en su recinto de seguridad, o su xboxservices.config no está configurado correctamente. El archivo xboxservices.config distingue mayúsculas de minúsculas.

## <a name="debug-based-on-error-code"></a>Según el código de error de depuración

Estas son algunas de los códigos de error, es posible que vea al inicio de sesión y los pasos que puede seguir para depurar estas.  Verá el código de error como se muestra en la siguiente captura de pantalla.

![captura de pantalla de inicio de sesión de Error de 0x8015DC12](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003 la aplicación está deshabilitada o configurada incorrectamente

1. Intente eliminar el archivo PFX.

![archivo pfx en el Explorador de soluciones](../../images/troubleshooting/pfx_file.png)

Si no iniciar sesión en Visual Studio con el Microsoft Account usada para el aprovisionamiento de la aplicación en el centro de partners, Visual Studio automáticamente generar un archivo pfx firma según su Account personal de Microsoft o su cuenta de dominio. Al crear el paquete appx, Visual Studio usará ese auto pfx generado para firmar el paquete y modificar la parte "publisher" de la identidad del paquete en package.appxmanifest. Como resultado, los bits generados (en concreto, appxmanifest.xml) tendrá la identidad de otro paquete que desea usar. 

2. Compruebe que el archivo package.appxmanifest está establecido en la misma identidad de aplicación como su título en el centro de partners. Puede haga clic con el botón derecho en el proyecto y elija Store -> asociar aplicación con Store... como se muestra en la siguiente captura de pantalla. O bien, edite manualmente el archivo package.appxmanifest. Consulte [empezar a trabajar con Visual Studio para juegos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) para obtener más información.

![Asociar con la tienda](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>Error de aislamiento del contenido 0x8015DC12

En resumen, esto significa que el dispositivo o el usuario no tiene acceso para el título especificado.

1. Esto podría significar que no está utilizando una cuenta de prueba para intentar iniciar sesión o la cuenta de prueba no tiene acceso al mismo espacio aislado que ha iniciado sesión como. Compruebe las instrucciones sobre cómo crear cuentas de prueba en el [documentación XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx) y [documentación del centro de partners.](../../xbox-live-test-accounts.md) Si es necesario crear una nueva cuenta de prueba con acceso al espacio aislado adecuado.

Es posible que deba quitar la cuenta antigua de Windows 10, puede hacerlo, vaya a la configuración desde el menú Inicio y, a continuación, vaya a las cuentas

2. Compruebe que el título se publica en el espacio aislado que está intentando usar. Consulte la [documentación XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) o [Partner Center documentación](../../xbox-live-service-configuration.md#sandbox-ids) para obtener información sobre cómo hacerlo.

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 inesperada o título desconocido

Compruebe la configuración de Id. de aplicación y el centro de desarrollo de enlace en XDP. Puede ver las instrucciones de [agregar Xbox Live compatibilidad para una nueva o existente UWP Studio Visual](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)

### <a name="0x87dd000e-title-not-authorized"></a>título 0x87DD000E no autorizado

Compruebe que el dispositivo se establece en el espacio aislado de desarrollo adecuadas y que el usuario tiene acceso al espacio aislado. Consulte la [probar mediante el App Xbox](#test-using-the-xbox-app) sección para obtener más información sobre cómo puede comprobar con el App Xbox.

Si esto no resuelve el problema, también Compruebe la configuración de enlace del centro de desarrollo y el Id. de aplicación como se describió anteriormente.

Si recibe un error que no se describe aquí, consulte la lista de errores en la documentación de xbox::services::xbox_live_error_code para obtener más información acerca de los códigos de error. También puede hacer referencia a errors.h en el XSAPI incluye.

Después de todos estos pasos, si todavía no se puede iniciar sesión con su título, publique un subproceso de soporte técnico en el [foros](https://forums.xboxlive.com) o póngase en contacto con su madre.
