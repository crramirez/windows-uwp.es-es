---
author: andrewleader
Description: Discover the different options desktop Win32 apps have for sending toast notifications
title: Notificaciones del sistema desde aplicaciones de escritorio
label: Toast notifications from desktop apps
template: detail.hbs
ms.author: mijacobs
ms.date: 05/01/2018
ms.topic: article
keywords: windows 10, uwp, win32, escritorio, notificaciones del sistema, puente de dispositivo de escritorio, opciones para enviar notificaciones del sistema, servidor com, activador com, com, falso com, no com, sin com, enviar notificaciones del sistema
ms.localizationpriority: medium
ms.openlocfilehash: 2ee44d990ef29fa8281a14c8ad03b6c6ff16a4cf
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6672690"
---
# <a name="toast-notifications-from-desktop-apps"></a>Notificaciones del sistema desde aplicaciones de escritorio

Las aplicaciones de escritorio (Puente de dispositivo de escritorio y Win32 clásico) pueden enviar notificaciones del sistema interactivas igual de la misma manera que las aplicaciones de la Plataforma universal de Windows (UWP). Sin embargo, hay varias opciones distintas para aplicaciones de escritorio debido a los diferentes esquemas de activación.

En este artículo, enumeramos las opciones que tienes para enviar una notificación del sistema en Windows 10. Cada opción admite totalmente...

* Persistir en el Centro de actividades.
* Ser activable tanto desde el elemento emergente como desde dentro del Centro de actividades.
* Ser activable mientras tu EXE no se está ejecutando.

## <a name="all-options"></a>Todas las opciones

En la tabla siguiente se muestra las opciones de compatibilidad con las notificaciones del sistema dentro de tu aplicación de escritorio y las características compatibles correspondientes. Puedes usar la tabla para seleccionar la mejor opción para tu escenario.<br/><br/>

| Opción | Elementos visuales | Acciones | Entradas | Activa en proceso |
| -- | -- | -- | -- | -- |
| [Activador COM](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Ningún COM/CLSID de rutas internas](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Opción preferida: activador COM

Esta es la opción preferida que funciona tanto para Puente de dispositivo de escritorio como para Win32 clásico y es compatible con todas las características de notificación. No tengas miedo del "activador COM"; tenemos una biblioteca [para C#](send-local-toast-desktop.md) y [aplicaciones de C++](send-local-toast-desktop-cpp-wrl.md) que hace que resulte muy sencillo, incluso si nunca antes has escrito un servidor COM.<br/><br/>

| Elementos visuales | Acciones | Entradas | Activa en proceso |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Con la opción de activador COM, puedes usar las siguientes plantillas de notificación y tipos de activación en tu aplicación.<br/><br/>

| Tipo de activación y plantilla | Puente de dispositivo de escritorio | Win32 clásico |
| -- | -- | -- |
| ToastGeneric en primer plano | ✔️ | ✔️ |
| Segundo plano de ToastGeneric | ✔️ | ✔️ |
| Protocolo de ToastGeneric | ✔️ | ✔️ |
| Plantillas heredadas | ✔️ | ❌ |

> [!NOTE]
> Si agregas el activador COM a tu aplicación existente del Puente de dispositivo de escritorio, las activaciones de notificaciones heredadas de primer y segundo plano activarán ahora el activador COM en lugar de la línea de comandos.

Para obtener información acerca de cómo usar esta opción, consulta [Enviar una notificación del sistema local desde aplicaciones de C# de escritorio](send-local-toast-desktop.md) o [Enviar una notificación del sistema local desde aplicaciones de C++ (WRL) de escritorio](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Opción alternativa: Ningún COM/CLSID de rutas internas

Esta es una opción alternativa si no puedes implementar un activador COM. Sin embargo, sacrificarás algunas características, como la compatibilidad de entrada (cuadros de texto en las notificaciones del sistema) y la activación en proceso.<br/><br/>

| Elementos visuales | Acciones | Entradas | Activa en proceso |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Con esta opción, si admites Win32 clásico, tu limitación será mucho mayor en los tipos de activación y plantillas de notificación que puedes usar, como se muestra a continuación.<br/><br/>

| Tipo de activación y plantilla | Puente de dispositivo de escritorio | Win32 clásico |
| -- | -- | -- |
| ToastGeneric en primer plano | ✔️ | ❌ |
| Segundo plano de ToastGeneric | ✔️ | ❌ |
| Protocolo de ToastGeneric | ✔️ | ✔️ |
| Plantillas heredadas | ✔️ | ❌ |

Publicarás documentos en el futuro en los que se muestre cómo usar esta opción. Básicamente, para las aplicaciones del Puente de dispositivo de escritorio, solo tienes que enviar notificaciones del sistema como lo haría una aplicación para UWP. Cuando el usuario haga clic en la notificación del sistema, la aplicación se iniciará en la línea de comandos con los argumentos de inicio que especificaste en la notificación del sistema.

Para aplicaciones clásicas de Win32, configura el AUMID para que puedas enviar notificaciones del sistema y también especificar a continuación un CLSID en el acceso directo. Puede ser cualquier GUID aleatorio. No agregues el activador o servidor COM. Agregas un CLSID COM "auxiliar", lo que hará que el Centro de actividades conservar la notificación. Ten en cuenta que solo puedes usar notificaciones del sistema de activación de protocolo ya que el CLSID auxiliar interrumpirá la activación de cualquier otra activaciones de notificación del sistema. Por lo tanto, tienes que actualizar tu aplicación para admitir la activación de protocolo y que el protocolo de notificaciones del sistema activen tu propia aplicación.


## <a name="resources"></a>Recursos

* [Enviar una notificación del sistema local desde aplicaciones de C# de escritorio](send-local-toast-desktop.md)
* [Enviar una notificación del sistema local desde aplicaciones de C++ WRL de escritorio](send-local-toast-desktop-cpp-wrl.md)
* [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)