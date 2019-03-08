---
title: Cuentas de prueba de Xbox Live
description: Obtenga información sobre cómo crear cuentas de prueba para pruebas de Xbox Live habilitado juego durante el desarrollo.
ms.assetid: e8076233-c93c-4961-86ac-27ec74917ebc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, cuenta de prueba
ms.localizationpriority: medium
ms.openlocfilehash: 14313b6121cabf82762b5e3e862c73a9d3ec05cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594630"
---
# <a name="xbox-live-test-accounts"></a>Cuentas de prueba de Xbox Live

Al probar la funcionalidad en su título durante el desarrollo, puede ser útil crear cuentas adicionales de Xbox Live.  Por ejemplo, es posible que desee una nueva cuenta con ningún logros.  O bien, puede realizar varias cuentas y las haga amigas entre sí para probar escenarios de redes sociales.

Puede ser lento para crear cuentas múltiples de Microsoft (MSA), por lo que se proporciona una manera fácil de crear varias cuentas de prueba a la vez.

Cuentas de prueba tienen también algunas otras ventajas.  Puede iniciar sesión en su *espacio aislado de desarrollo*, mientras que una MSA normal no debido a restricciones de seguridad.  Si no sabe qué un *espacio aislado de desarrollo* es, a continuación, consulte [espacios aislados de Xbox Live](xbox-live-sandboxes.md)

## <a name="types-of-test-accounts"></a>Tipos de cuentas de prueba

Hay dos opciones de cuentas de prueba.  MSAs regulares que se aprovisionan para que funcione en su espacio aislado de desarrollo, o cuentas de prueba que solo funcionan en un espacio aislado de desarrollo.

Si está desarrollando un título con el programa de creadores, solo se pueden usar MSAs regulares que se ha aprovisionado para su espacio aislado de desarrollo.

A continuación, veremos cómo crear ambos tipos.

## <a name="provisioning-regular-msas"></a>MSAs Regular de aprovisionamiento

Si tiene una cuenta existente de Xbox Live, sería un buen punto de partida para aprovisionarla para su uso con su espacio aislado de desarrollo.

Si no tiene una cuenta existente de Xbox Live o requerir MSAs adicionales, puede crear algunas en [ https://account.microsoft.com/account ](https://account.microsoft.com/account).

## <a name="creating-test-accounts"></a>Creación de cuentas de prueba

Si eres un ID@Xbox para desarrolladores, entonces también puede crear cuentas de prueba exclusivamente para su uso en los espacios aislados de desarrollo.  También puede crear varias cuentas de prueba a la vez.

Para ir a la página de administración de la cuenta de prueba en el centro de partners.
1. Vaya a [centro de partners](https://partner.microsoft.com/dashboard)
2. Haga clic en el icono de engranaje en la parte superior derecha para ir a la configuración de cuenta
3. Haga clic en "Cuentas de prueba".

Vea a continuación una captura de pantalla que muestra dónde encontrarlo

![](images/getting_started/devcenter_testaccount_nav.png)

Al hacer clic en "Cuentas de prueba", verá un resumen de cualquier existente cuentas de prueba si tiene alguno.  También tiene la opción para crear nuevas cuentas de prueba.

![](images/getting_started/devcenter_testaccount_summary.png)

Puede hacer clic en "Nueva cuenta de prueba" y aparecerá un formulario que puede usar para crear cuentas de prueba.

![](images/getting_started/devcenter_testaccount_new.png)

Las cuentas de prueba que cree tendrá el prefijo con el nombre de su espacio aislado de desarrollo y automáticamente tendrán acceso a su espacio aislado de desarrollo.
