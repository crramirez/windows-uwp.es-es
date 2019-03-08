---
title: Crear y probar un nuevo título de creadores
description: Obtenga información sobre cómo crear un nuevo título de programa de creadores de Xbox Live y publicar en el entorno de prueba.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, creadores, prueba
ms.localizationpriority: medium
ms.openlocfilehash: 5b39a2ed67d2fe77fa8904408b81a329125d73c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594600"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>Cree un nuevo título de programa de creadores de Xbox Live y publique en el entorno de prueba

## <a name="introduction"></a>Introducción

Antes de escribir ningún código de Xbox Live, debe configurar un nuevo título en el portal de configuración del servicio.  Puede aprender más sobre la configuración de servicio en [configuración del servicio de Xbox Live](../xbox-live-service-configuration.md).

En este artículo le guiará a través de todo lo necesario para obtener un título configurado en [centro de partners](https://partner.microsoft.com/dashboard), un nuevo proyecto creado y preparación para las pruebas de Xbox Live. En este artículo se da por supuesto lo siguiente:

1. Usa el programa de creadores de Xbox Live.
2. Está desarrollando un título de la plataforma Universal de Windows (UWP).  Pueden ejecutar los títulos UWP en Xbox One, los equipos de Windows 10 escritorio y mobile
3. Configura el título en [centro de partners](https://partner.microsoft.com/dashboard).
4. El equipo de desarrollo se está ejecutando Windows 10.

> [!NOTE]
> Si forma parte del programa creadores de Xbox Live, los supuestos anteriores se aplican a usted y debe seguir este artículo

## <a name="partner-center-setup"></a>Programa de instalación del centro de partners

Necesita un título de Xbox Live habilitado creado en [centro de partners](https://partner.microsoft.com/dashboard) como un requisito previo para usar cualquier funcionalidad de Xbox Live.

### <a name="create-a-microsoft-account"></a>Crear una cuenta de Microsoft
Si no tienes una Account de Microsoft (también conocido como una MSA), deberá crear primero uno en [Account inicio de sesión en Microsoft](https://go.microsoft.com/fwlink/p/?LinkID=254486). Si tiene una cuenta de Office 365, use Outlook.com o tiene una cuenta de Xbox Live - probablemente ya tiene una MSA.

### <a name="register-as-an-app-developer"></a>Registrarse como desarrollador de aplicaciones
Deberá registrarse como desarrollador de aplicaciones antes de poder crear un nuevo título en el centro de partners.

Para registrarse, vaya a [registrarse como desarrollador de aplicaciones](https://developer.microsoft.com/store/register) y siga el proceso de registro.

### <a name="create-a-new-uwp-title"></a>Crear un nuevo título UWP
Necesitará un título UWP definido en el centro de partners. Para ello, primero vamos a [centro de partners](https://partner.microsoft.com/dashboard).

A continuación, cree un título nuevo. Necesitará reservar un nombre.

![](../images/getting_started/first_xbltitle_newapp.png)

La captura de pantalla muestra la página principal donde estará configurando Xbox Live, ubicado en el **servicios** > **Xbox Live** menú.

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>Habilitar servicios de Xbox Live
Al hacer clic en el **Xbox Live** vincular en **servicios** por primera vez para un producto, se le llevará a la página Habilitar Xbox Live programa de creadores.  A continuación, haga clic en el **habilitar** botón para que aparezca el cuadro de diálogo del programa de instalación de Xbox Live.

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

En el cuadro de diálogo programa de instalación, seleccione las plataformas que desea habilitar los servicios de Xbox Live para (de forma predeterminada se seleccionan Xbox One y equipo con Windows 10).  Haga clic en el **confirmar** botón para habilitar el programa de creadores de Xbox Live para su juego.

> [!IMPORTANT]
> Xbox Live solo se admite para los juegos. Para publicar correctamente su juego con Xbox Live, debe establecer el tipo de producto a "Juego" en la sección Propiedades del envío.

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>Probar la configuración del servicio de Xbox Live en tu juego
Al realizar cambios en la configuración de Xbox Live para su juego, deberá publicar los cambios en un entorno concreto antes de que se recogen el resto de Xbox Live y pueden verse el juego.

Cuando todavía está trabajando en su juego, publica en su propio espacio aislado de desarrollo.  El espacio aislado de desarrollo permite trabajar en los cambios en su juego en un entorno aislado.

Cuando se lance su juego al público, la configuración de Xbox Live se publicará automáticamente en el espacio aislado de venta directa.

De forma predeterminada, las consolas de una Xbox y equipos con Windows 10 están en el espacio aislado de venta directa.

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>Publicar la configuración de Xbox Live en el entorno de prueba

Cada vez que habilite servicios de Xbox Live y realizar cambios en la configuración del servicio de Xbox Live, para que los cambios surtan efecto, deberá publicar estos cambios en su espacio aislado de desarrollo.

En la página de configuración de Xbox Live, haga clic en el **prueba** botón para publicar la configuración actual de Xbox Live en su espacio aislado de desarrollo.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>Autorizar a los dispositivos y usuarios para el espacio aislado de desarrollo

Solo los usuarios y dispositivos autorizados pueden acceder a la configuración de Xbox Live para el juego en su espacio aislado de desarrollo.

De forma predeterminada, todas las consolas de desarrollo Xbox One que ha agregado a la cuenta del centro de partners tienen acceso a su espacio aislado de desarrollo.  Para agregar una consola Xbox One, vaya a [consolas Xbox One administrar](https://partner.microsoft.com/xboxconfig/devices). Si ya está en la cuenta del centro de partners, puede ir a **configuración de la cuenta** > **configuración de la cuenta** > **Dev dispositivos**  >  **Consolas Xbox One de desarrollo**.

También puede autorizar cuentas de Xbox Live normales para tener acceso a su espacio aislado de desarrollo.  Para autorizar el acceso de cuentas de Xbox Live a su espacio aislado de desarrollo, vaya a [administrar cuentas](https://developer.microsoft.com/xboxtestaccounts/configurecreators).

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene un título nuevo creado, ahora puede configurar un título de Xbox Live habilitada en el motor de juego, Visual Studio o el entorno de compilación de elección.

Consulte [guía paso a paso para integrar Xbox Live](creators-step-by-step-guide.md)
