---
title: Crear un nuevo título
description: Obtenga información sobre cómo crear un nuevo título para Xbox Live mediante el centro de partners.
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1aa2447a2044bec9b2013b30c05e45342b763fc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656570"
---
# <a name="create-a-new-title-for-xbox-live"></a>Crear un nuevo título para Xbox Live

## <a name="introduction"></a>Introducción

Antes de escribir ningún código, debe configurar un nuevo título en el portal de configuración del servicio.  Puede aprender más sobre la configuración de servicio en [configuración del servicio de Xbox Live](../xbox-live-service-configuration.md)

Este artículo le guiará a través de este proceso con las suposiciones siguientes

1. Está desarrollando un título de la plataforma Universal de Windows (UWP).  Ejecuten los títulos UWP en Xbox One, los equipos de Windows 10 escritorio y mobile
2. Configura el título en [centro de partners](https://partner.microsoft.com/dashboard).
3. Usa Visual Studio con un motor de juego personalizado o Unity.
4. El equipo de desarrollo se está ejecutando Windows 10.

Siempre que los anteriores son true, el resto de este artículo le guiará a través de todo lo necesario para obtener un título configurado en el centro de partners, un nuevo proyecto creado y Xbox Live inicio de sesión de código escrito y probado.

> [!NOTE]
> Si forma parte del programa creadores de Xbox Live, los supuestos anteriores se aplican a usted y debe seguir este artículo.

## <a name="partner-center-setup"></a>Programa de instalación del centro de partners

Necesita un título de Xbox Live habilitado creado en [centro de partners](https://partner.microsoft.com/dashboard) como un requisito previo para cualquier trabajo de la funcionalidad de Xbox Live.

### <a name="create-a-microsoft-account"></a>Crear una cuenta de Microsoft
Si no tienes una Account de Microsoft (también conocido como una MSA), deberá crear primero uno en [ https://go.microsoft.com/fwlink/p/?LinkID=254486 ](https://go.microsoft.com/fwlink/p/?LinkID=254486).  Si tiene una cuenta de Office 365, use Outlook.com o tiene una cuenta de Xbox Live - probablemente ya tiene una MSA.

### <a name="register-as-an-app-developer"></a>Registrarse como desarrollador de aplicaciones
Deberá registrarse como desarrollador de aplicaciones antes de que tiene permiso para crear un nuevo título en el centro de partners.

Para registrarse, vaya a https://developer.microsoft.com/en-us/store/register y siga el proceso de registro.

### <a name="create-a-new-uwp-title"></a>Crear un nuevo título UWP
A continuación, necesita un título UWP definido en el centro de partners.  Para ello, va primero al panel

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

A continuación, cree un título nuevo.  Necesitará reservar un nombre.

![](../images/getting_started/first_xbltitle_newapp.png)

A continuación, le dirigirá a la *información general de la aplicación* página de la aplicación.  Es la página principal donde estará configurando Xbox Live en los servicios -> menú Xbox Live se muestra a continuación.

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>Crear una cuenta de Xbox Live
Necesitará una cuenta de Xbox Live para iniciar sesión en Xbox Live.  Si ya tiene una que use para iniciar sesión en la consola Xbox One o en el App Xbox en Windows 10, a continuación, puede usar que uno.

En caso contrario, debe abrir el App Xbox en su PC y el inicio de sesión con su Account de Microsoft.  A continuación, se habilitará para su uso con Xbox Live.

Puede encontrar el App Xbox, vaya a la *menú Inicio* y escribiendo "Xbox" tal como se muestra a continuación.

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene un título nuevo creado, ahora puede configurar un título de Xbox Live habilitada en el motor de juego, Visual Studio o el entorno de compilación de elección.

Consulte [guía paso a paso para integrar Xbox Live](partners-step-by-step-guide.md)
