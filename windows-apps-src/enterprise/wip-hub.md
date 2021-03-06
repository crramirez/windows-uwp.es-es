---
description: En este tema del centro se describe un panorama completo de desarrollador sobre cómo Windows Information Protection (WIP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows Information Protection (WIP)
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Windows Information Protection, datos empresariales, protección de datos empresariales,
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.localizationpriority: medium
ms.openlocfilehash: cb3006541a5e14936035bc7b9d3b6493df031823
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032018"
---
# <a name="windows-information-protection-wip"></a>Windows Information Protection (WIP)

__Nota__ La directiva de Windows Information Protection (WIP) se puede aplicar a Windows 10, versión 1607.

WIP protege los datos que pertenecen a una organización aplicando las directivas definidas por esta. Si la aplicación se incluye en las directivas, todos los datos generados por la aplicación están sujetos a las restricciones de la directiva. Este tema te ayuda a crear aplicaciones que aplican estas directivas de manera correcta sin que esto afecte para nada los datos personales del usuario.
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>En primer lugar, ¿qué es WIP?

WIP es un conjunto de características en equipos de escritorio, portátiles, tabletas y teléfonos que son compatibles con los sistemas Administración de dispositivos móviles (MDM) y Administración de aplicaciones móviles (MAM) de la organización.

WIP, junto con MDM, proporciona a la organización un mayor poder sobre cómo se controlan los datos en los dispositivos que administra la organización. En ocasiones, los usuarios llevan dispositivos al trabajo, pero no los inscriben el sistema MDM de la organización.  En esos casos, las organizaciones pueden usar MAM para lograr un mayor dominio sobre cómo se controlan sus datos en una línea específica de aplicaciones empresariales que los usuarios instalen en su dispositivo.

Con MDM y MAM, los administradores pueden identificar qué aplicaciones pueden acceder a los archivos que pertenecen a la organización y si los usuarios pueden copiar los datos de estos archivos y luego pegarlos en documentos personales.

Aquí le mostramos cómo funciona: Los usuarios inscriben sus dispositivos en el sistema de administración de dispositivos móviles (MDM). Un administrador de la organización administrativa usa Microsoft Intune o System Center Configuration Manager (SCCM) para definir y luego implementar una directiva en los dispositivos inscritos.

Si no es necesario que los usuarios inscriban sus dispositivos, los administradores usarán su sistema MAM para definir e implementar una directiva que se aplique a aplicaciones específicas. Cuando los usuarios instalen cualquiera de esas aplicaciones, recibirán la directiva asociada.

Esa directiva identifica las aplicaciones que pueden acceder a los datos empresariales (conocida como la *lista de permitidos* de la directiva). Estas aplicaciones pueden acceder a archivos de empresa protegidos, redes privadas virtuales (VPN) y datos de empresa en el Portapapeles o través de un contrato para contenido compartido. La directiva también define las reglas que controlan los datos. Por ejemplo, si los datos pueden copiarse desde los archivos que sean propiedad de la empresa y, a continuación, pegarse en archivos que no pertenezcan a esta.

Si los usuarios anulan la inscripción de su dispositivo del sistema MDM de la organización o desinstalan las aplicaciones identificadas por el sistema MAM de la organización, los administradores pueden borrar de forma remota los datos de la empresa de dicho dispositivo.

![Ciclo de vida de WIP](images/wip-lifecycle.png)

> **Más información sobre WIP** <br>
* [Introducción a Windows Information Protection](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/bg-p/Windows10Blog)
* [Protege los datos de tu empresa con Windows Information Protection (WIP)](/windows/whats-new/edp-whats-new-overview)

Si la aplicación está en la lista de aplicaciones permitidas, todos los datos generados por la aplicación están sujetos a las restricciones de la directiva. Esto significa que si los administradores revocan el acceso del usuario a los datos de empresa, dichos usuarios perderán el acceso a todos los datos de la aplicación que has creado.

Esto está bien si tu aplicación está diseñada solo para el uso de la empresa. Pero si tu aplicación crea datos que los usuarios consideran personales, querrás *habilitar* tu aplicación para que distinga de manera inteligente entre los datos personales y de empresa. Este tipo de aplicaciones se denominan *habilitadas para empresas* porque pueden aplicar la directiva mientras que conservan la integridad de los datos personales del usuario.

## <a name="create-an-enterprise-enlightened-app"></a>Crear una aplicación habilitada para empresas

Usa API de WIP para optimizar la aplicación y, a continuación, declarar tu aplicación como habilitada para empresas.

Optimiza tu aplicación si se va a utilizar para fines de empresa y personales.

Optimizar la aplicación si desea controlar correctamente el cumplimiento de los elementos de directiva.

Por ejemplo, si la directiva permite a los usuarios pegar datos de empresa en un documento personal, puedes evitar que los usuarios tengan que responder a un cuadro de diálogo de consentimiento antes de pegar los datos. Del mismo modo, puedes presentar cuadros de diálogo informativos personalizados para responder a estos tipos de eventos.

Si estás listo para optimizar tu aplicación, consulta una de estas guías:

**Para las aplicaciones Plataforma universal de Windows (UWP) que se compilan con C #**

[Guía del desarrollador de Windows Information Protection (WIP)](wip-dev-guide.md).

**Para aplicaciones de escritorio creadas con C++**

[Guía del desarrollador de Windows Information Protection (WIP) (C++)](/previous-versions/windows/desktop/EDP/wip-developer-guide).


## <a name="create-non-enlightened-enterprise-app"></a>Crear una aplicación empresarial no habilitada

Si vas a crear una aplicación de línea de negocio (LOB) que no esté destinada a uso personal, tal vez no tengas que habilitarla.

### <a name="windows-desktop-apps"></a>Aplicaciones de escritorio para Windows
No es necesario que habilites una aplicación de escritorio de Windows, pero debes probarla para garantizar que funciona correctamente en la directiva. Por ejemplo, inicia la aplicación, úsala y, después, anula la inscripción del dispositivo del sistema MDM. A continuación, asegúrate de que la aplicación puede iniciarse de nuevo. Si los archivos fundamentales para el funcionamiento de la aplicación están cifrados, la aplicación podría no iniciarse. Además, revisa los archivos con los que interactúa la aplicación para garantizar que la aplicación no cifrará accidentalmente archivos personales del usuario. Esto podría incluir archivos de metadatos, imágenes y otras cosas.

Después de haber probado la aplicación, agrega esta marca para el archivo de recursos o el proyecto y, luego, vuelve a compilar la aplicación.

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
Si bien las directivas de MDM no requieren la marca, las directivas de MAM sí lo hacen.

### <a name="uwp-apps"></a>Aplicaciones para UWP

Si tienes previsto incluir la aplicación en una directiva de MAM, deberás habilitarla. Las directivas implementadas en dispositivos sujetos a MDM no lo requieren, pero si distribuyes la aplicación a los consumidores de la organización, resulta difícil, si no imposible, determinar qué tipo de sistema de administración de directivas usará. Para garantizar que tu aplicación funcionará en ambos tipos de sistemas de administración de directivas (MDM y MAM), debes habilitarla.






 
