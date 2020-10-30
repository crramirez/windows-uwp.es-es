---
description: Descubra las diferentes opciones que tienen las aplicaciones de escritorio para enviar notificaciones del sistema
title: Notificaciones del sistema de aplicaciones de escritorio
label: Toast notifications from desktop apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Win32, escritorio, notificaciones del sistema, puente de escritorio, msix, paquete disperso, opciones para enviar notificaciones del sistema, servidor com, activador com, com, com falsificado, sin com, sin com, enviar notificaciones de envío
ms.localizationpriority: medium
ms.openlocfilehash: 9cdd8e57311400c8603f4eb99e9bfd1a2230f2ce
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033098"
---
# <a name="toast-notifications-from-desktop-apps"></a>Notificaciones del sistema de aplicaciones de escritorio

Las aplicaciones de escritorio (incluidas las aplicaciones empaquetadas de [MSIX](/windows/msix/desktop/source-code-overview) , las aplicaciones que usan [paquetes dispersos](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obtener la identidad del paquete y las aplicaciones de escritorio clásicas no empaquetadas) pueden enviar notificaciones del sistema interactivas, al igual que las aplicaciones de Windows. Sin embargo, hay varias opciones diferentes para las aplicaciones de escritorio debido a los diferentes esquemas de activación.

En este artículo, se enumeran las opciones que tiene para enviar una notificación del sistema en Windows 10. Cada opción es totalmente compatible...

* Persistencia en el centro de actividades
* Ser activable desde el elemento emergente y dentro del centro de actividades
* Se está activando cuando el archivo EXE no se está ejecutando

## <a name="all-options"></a>Todas las opciones

En la tabla siguiente se muestran las opciones para admitir las notificaciones del sistema dentro de la aplicación de escritorio y las características admitidas correspondientes. Puede usar la tabla para seleccionar la mejor opción para su escenario.<br/><br/>

| Opción | Objetos visuales | Acciones | Entradas | Activa en proceso |
| -- | -- | -- | -- | -- |
| [Activador COM](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Sin CLSID de COM/stub](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Opción preferida: COM Activator

Esta es la opción preferida que funciona con las aplicaciones de escritorio y es compatible con todas las características de notificación. No tenga miedo del "activador COM"; tenemos una biblioteca para las aplicaciones [de C#](send-local-toast-desktop.md) y [C++](send-local-toast-desktop-cpp-wrl.md) que hace que resulte muy sencillo, incluso si nunca ha escrito un servidor com antes.<br/><br/>

| Objetos visuales | Acciones | Entradas | Activa en proceso |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Con la opción Activator de COM, puede usar las siguientes plantillas de notificación y tipos de activación de la aplicación.<br/><br/>

| Tipo de plantilla y activación | MSIX/paquete disperso | Escritorio clásico |
| -- | -- | -- |
| Primer plano ToastGeneric | ✔️ | ✔️ |
| Fondo de ToastGeneric | ✔️ | ✔️ |
| Protocolo ToastGeneric | ✔️ | ✔️ |
| Plantillas heredadas | ✔️ | ❌ |

> [!NOTE]
> Si agrega el activador COM a la aplicación de paquete MSIX/disperso existente, las activaciones de la notificación de primer o segundo plano y las de notificaciones heredadas activarán ahora su activador de COM en lugar de la línea de comandos.

Para obtener información sobre cómo usar esta opción, consulte [enviar una notificación del sistema local desde las aplicaciones de C# de escritorio](send-local-toast-desktop.md) o [enviar una notificación del sistema local desde las aplicaciones de WRL de Win32 C++](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Opción alternativa: ningún CLSID de COM/stub

Se trata de una opción alternativa si no puede implementar un activador COM. Sin embargo, sacrificará algunas características, como la compatibilidad de entrada (cuadros de texto en la notificación del sistema) y la activación en proceso.<br/><br/>

| Objetos visuales | Acciones | Entradas | Activa en proceso |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Con esta opción, si admite el escritorio clásico, está mucho más limitado en las plantillas de notificación y los tipos de activación que puede usar, tal como se muestra a continuación.<br/><br/>

| Tipo de plantilla y activación | MSIX/paquete disperso | Escritorio clásico |
| -- | -- | -- |
| Primer plano ToastGeneric | ✔️ | ❌ |
| Fondo de ToastGeneric | ✔️ | ❌ |
| Protocolo ToastGeneric | ✔️ | ✔️ |
| Plantillas heredadas | ✔️ | ❌ |

En el caso de las aplicaciones empaquetadas [MSIX](/windows/msix/desktop/source-code-overview) y las aplicaciones que usan [paquetes dispersos](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps), solo tiene que enviar notificaciones del sistema como si fuera una aplicación de UWP. Cuando el usuario haga clic en la notificación del sistema, la aplicación se iniciará con los argumentos de inicio que especificó en la notificación del sistema.

En el caso de las aplicaciones de escritorio clásicas, configure el AUMID para que pueda enviar notificaciones del sistema y, a continuación, especifique también un CLSID en el acceso directo. Puede ser cualquier GUID aleatorio. No agregue el servidor COM o el activador. Está agregando un "stub" COM CLSID, que hará que el centro de actividades conserve la notificación. Tenga en cuenta que solo puede usar notificaciones de activación de protocolo, ya que el CLSID de código auxiliar interrumpirá la activación de cualquier otra activación del sistema. Por lo tanto, debe actualizar la aplicación para admitir la activación del protocolo y hacer que el protocolo del sistema Active su propia aplicación.


## <a name="resources"></a>Recursos

* [Enviar una notificaciones del sistema local desde aplicaciones de C# de escritorio](send-local-toast-desktop.md)
* [Enviar una notificación del sistema local desde las aplicaciones de WRL de Win32 C++](send-local-toast-desktop-cpp-wrl.md)
* [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
