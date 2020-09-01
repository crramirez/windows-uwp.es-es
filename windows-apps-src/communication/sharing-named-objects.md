---
title: Compartición de objetos con nombre
description: En este tema se explica cómo compartir objetos con nombre entre aplicaciones de Plataforma universal de Windows (UWP) y aplicaciones Win32.
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 38d08e71c44945a7b22f124d15507c7889f8589d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175619"
---
# <a name="sharing-named-objects"></a>Compartición de objetos con nombre

En este tema se explica cómo compartir objetos con nombre entre aplicaciones de Plataforma universal de Windows (UWP) y aplicaciones Win32.

## <a name="named-objects-in-packaged-applications"></a>Objetos con nombre en aplicaciones empaquetadas

Los [objetos con nombre](/windows/win32/sync/object-names) proporcionan una manera sencilla de que los procesos compartan los identificadores de objetos. Después de que un proceso haya creado un objeto con nombre, otros procesos pueden usar el nombre para llamar a la función adecuada para abrir un identificador del objeto. Los objetos con nombre se utilizan normalmente para la [sincronización](/windows/win32/sync/interprocess-synchronization) de subprocesos y la [comunicación entre procesos](./interprocess-communication.md).

De forma predeterminada, las aplicaciones empaquetadas solo pueden tener acceso a los objetos con nombre que han creado. Para compartir objetos con nombre con aplicaciones empaquetadas, se deben establecer permisos al crear objetos y los nombres se deben calificar cuando se abren objetos.

## <a name="creating-named-objects"></a>Crear objetos con nombre

Los objetos con nombre se crean con una `Create` API correspondiente:

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [Createsemaphore (](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

Todas estas API comparten un `LPSECURITY_ATTRIBUTES` parámetro que permite al llamador especificar listas de [control de acceso (ACL)](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) para controlar qué procesos pueden tener acceso al objeto. Para compartir objetos con nombre con aplicaciones empaquetadas, se debe conceder el permiso dentro de las ACL cuando se creen los objetos con nombre.

Los identificadores de seguridad (SID) representan identidades dentro de las ACL. Cada aplicación empaquetada tiene su propio SID basado en el nombre de familia del paquete. Puede generar el SID para una aplicación empaquetada pasando su nombre de familia de paquete a [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername).

> [!NOTE]
> El nombre de familia de paquete puede encontrarse en el editor de manifiestos de paquete de Visual Studio durante el tiempo de desarrollo, a través del [centro de Partners](../publish/view-app-identity-details.md) para las aplicaciones publicadas a través del Microsoft Store o mediante el comando de PowerShell [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) para las aplicaciones que ya están instaladas.

[Este ejemplo](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples) muestra el patrón básico necesario para la ACL de un objeto con nombre. Para compartir objetos con nombre con aplicaciones empaquetadas, cree una estructura de [EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w) para cada aplicación:

* `grfAccessMode = GRANT_ACCESS`
* `grfAccessPermissions =` permisos adecuados basados en el objeto y uso previsto
    * [Derechos de acceso genéricos](/windows/win32/secauthz/generic-access-rights)
    * [Derechos de acceso y seguridad de objetos de sincronización](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [Derechos de acceso y seguridad de asignación de archivos](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =` el SID adquirido de [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)

Al rellenar el `LPSECURITY_ATTRIBUTES` parámetro en `Create` llamadas con `EXPLICIT_ACCESS` reglas para aplicaciones empaquetadas, puede conceder acceso a esas aplicaciones para abrir el objeto con nombre.

> [!NOTE]
> Las aplicaciones Win32 pueden tener acceso a todos los objetos con nombre creados por aplicaciones empaquetadas siempre que califiquen los nombres de objeto al [abrirlos](#opening-named-objects). No es necesario que se les conceda acceso.

## <a name="opening-named-objects"></a>Abrir objetos con nombre

Los objetos con nombre se abren pasando un nombre a una `Open` API correspondiente:

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

Los objetos con nombre creados por una aplicación empaquetada se crean en el espacio de nombres de la aplicación, también conocido como la ruta de acceso del objeto con nombre. Al abrir los objetos con nombre creados por una aplicación empaquetada, los nombres de objeto deben ir precedidos de la ruta de acceso del objeto con nombre de la aplicación que se crea.

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath) devolverá la ruta de acceso del objeto con nombre para una aplicación empaquetada en función de su SID. Puede generar el SID para una aplicación empaquetada pasando su nombre de familia de paquete a [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername).

> [!NOTE]
> El nombre de familia de paquete puede encontrarse en el editor de manifiestos de paquete de Visual Studio durante el tiempo de desarrollo, a través del [centro de Partners](../publish/view-app-identity-details.md) para las aplicaciones publicadas a través del Microsoft Store o mediante el comando de PowerShell [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) para las aplicaciones que ya están instaladas.

Al abrir los objetos con nombre creados por una aplicación empaquetada, use el formato `<PATH>\<NAME>` :

* Reemplazar `<PATH>` por la ruta de acceso del objeto con nombre de la aplicación de creación.
* Reemplazar `<NAME>` por el nombre del objeto.

> [!NOTE]
> El prefijo de los nombres de objeto con `<PATH>` solo es necesario si una aplicación empaquetada creó el objeto. No es necesario calificar los objetos con nombre creados por las aplicaciones Win32, aunque se debe conceder acceso al [crear](#creating-named-objects)los objetos.

## <a name="remarks"></a>Observaciones

Los objetos con nombre en aplicaciones empaquetadas están aislados de forma predeterminada para preservar la seguridad y garantizar la compatibilidad con eventos de ciclo de vida de la aplicación, como la suspensión y la terminación. El uso compartido de objetos con nombre entre aplicaciones presenta restricciones estrictas de enlace y control de versiones, y requiere que cada aplicación sea resistente al ciclo de vida de los demás. Por estos motivos, se recomienda compartir solo los objetos con nombre entre las aplicaciones del mismo publicador.