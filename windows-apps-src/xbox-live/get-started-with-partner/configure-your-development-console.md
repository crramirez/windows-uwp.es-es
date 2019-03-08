---
title: Configurar la consola de desarrollo de Xbox
description: Obtenga información sobre cómo configurar la consola de desarrollo de Xbox para admitir el desarrollo de Xbox Live.
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 479be2401e0c54801645ad1c0d91b11b7ffb6869
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649020"
---
# <a name="configure-your-xbox-development-console"></a>Configurar la consola de desarrollo de Xbox

Para configurar la consola de desarrollo:
- Obtener los identificadores
- Establecer el espacio aislado en los kits de desarrollo
- Inicie sesión con una cuenta de desarrollo

## <a name="get-your-ids"></a>Obtener los identificadores
Para habilitar los espacios aislados y servicios de Xbox Live, deberá obtener varios identificadores para configurar el kit de desarrollo y su título. Es posible con el mismo proceso.

Siga [configuración del servicio de Xbox Live](../xbox-live-service-configuration.md) para obtener los identificadores

## <a name="set-your-sandbox-on-your-development-kits"></a>Establecer el espacio aislado en los kits de desarrollo
No podrá arrancar el kit de desarrollo sin tener que establecer el identificador de espacio aislado. Para ello, puede usar la "Xbox un administrador" que está instalado en su PC por el XDK, o puede abrir una ventana de comandos XDK y use el comando de configuración (xbconfig.exe) como sigue:

Compruebe su recinto de seguridad actual. En el símbolo del sistema, escriba xbconfig sandboxid.

Si es así no es el esperado, cambie el identificador de espacio aislado. Escriba xbconfig sandboxid =<your sandbox id> en el símbolo del sistema.

Reinicie la consola mediante el reinicio (xbreboot.exe) en el símbolo del sistema.

Compruebe que su espacio aislado se ha restablecido correctamente. En el símbolo del sistema, escriba xbconfig sandboxid.

## <a name="sign-in-with-a-development-account"></a>Inicie sesión con una cuenta de desarrollo

Puede crear cuentas de desarrollo que se usa para iniciar sesión en [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) o [centro de partners](https://partner.microsoft.com/dashboard)
