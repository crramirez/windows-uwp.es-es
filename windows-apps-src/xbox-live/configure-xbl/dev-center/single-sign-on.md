---
title: Configurar el inicio de sesión único en el centro de partners
description: Se describe cómo configurar el inicio de sesión único en el centro de partners para permitir un título al firmar un usuario en los servicios mediante su identificador de Xbox Live.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, udc, Centro para desarrolladores universal, inicio de sesión único
ms.openlocfilehash: 32f06edd407d8c1fa74795d0a230c7d56ba8e838
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632880"
---
# <a name="configure-single-sign-on-in-partner-center"></a>Configurar el inicio de sesión único en el centro de partners

Inicio de sesión único permite que un reproductor con el título para iniciar sesión en sus servicios a través de su inicio de sesión de Xbox Live. Esto permite que un reproductor que se inicien sesión en Xbox Live ejecutar una aplicación o juego para el servicio sin tener que iniciar sesión una segunda vez con una credencial de cuenta diferente específica del servicio.

> [!NOTE]
> En este tema no se aplica a los títulos en el programa de creadores de Xbox Live.

Por ejemplo, el título podría ser una aplicación que permite que el servicio para transmitir contenido (como vídeos, música, etc.) a su dispositivo, siempre que tienen una cuenta válida con el servicio. Si el usuario está registrado en su cuenta de Xbox Live, debe ser capaz de transmitir el contenido sin tener que iniciar sesión en su servicio cada vez.

Además, si su aplicación envía datos de Kinect para un servicio externo, puede configurar en aquí.

Si el servicio requiere que el usuario crea una cuenta independiente de Xbox Live, debe proporcionar una forma para el usuario vincular su cuenta de Xbox Live con su cuenta en el servicio como una acción puntual.

Al configurar el inicio de sesión único, puede especificar direcciones URL y su usuario de confianza. Cada vez que la aplicación llama a cualquiera de las direcciones URL especificadas, Xbox Live adjuntará automáticamente un token de Xbox Secure Token Service (XSTS). El servicio que recibe la clave se conoce como un *confianza*, y debe configurar [confianza](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index) antes de configurar el inicio de sesión único. Cada configuración de la entidad de usuario de confianza especifica qué información se encuentra en el token XSTS, así como una clave de cifrado única que puede usar el usuario de confianza para descodificar el token XSTS.

Agregar configuración haciendo lo siguiente:

1. Después de seleccionar el título en [centro de partners](https://partner.microsoft.com/dashboard), vaya a **servicios** > **Xbox Live**.

2. Haga clic en el vínculo a **Xbox Live de sesión único**.

3. Haga clic en el **agregar dirección URL** botón para crear una nueva sesión entrada individual. Esto agregará una nueva fila a la parte inferior de la lista de configuraciones.

4. En el cuadro de dirección URL, escriba la dirección URL para el servicio con un nombre de dominio completo. Puede reemplazar el subdominio de nivel inferior con un carácter comodín ('\*'). Esto coincidirá con cualquier dirección URL que tiene los mismos dominios de nivel superior. Por ejemplo, "*. ejemplo.com&quot; coincidirá con"bar.example.com"o"foo.bar.example.com".

5. En el cuadro de entidad de usuario de confianza, seleccione la configuración del usuario de confianza que especifica cómo se codifica el token XSTS.

6. Haga clic en el **guardar** botón para guardar los cambios.

![Captura de pantalla de la página de configuración del inicio de sesión único](../../images/dev-center/single-signon.png)
