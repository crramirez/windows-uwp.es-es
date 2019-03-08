---
title: Autorizar a cuentas de Xbox Live en el entorno de pruebas
description: Obtenga información sobre cómo autorizar a una cuenta de Xbox Live pública para su uso en las pruebas en el entorno de desarrollo.
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, cuentas, cuentas de prueba
ms.localizationpriority: medium
ms.openlocfilehash: 662f85f985baf58eef050060f8132f5a4b92444d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663860"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>Autorización de cuentas de Xbox Live para pruebas en su entorno

En este tema recorrerá el proceso de configuración de una cuenta de Xbox Live con el entorno de prueba de publicadores

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente para autorizar a una cuenta de prueba de Xbox Live:

* Un [cuenta de Xbox Live](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>Vaya a la página cuenta de prueba de Xbox

Esto se encuentra en la sección de configuración de la cuenta del centro de partners

Puede llegar a esta sección en uno de dos maneras

1. En [centro de partners](https://partner.microsoft.com/dashboard/windows/overview), haga clic en el ⚙️ de engranaje de configuración en la esquina superior derecha del panel y seleccione **opciones del desarrollador** en la lista desplegable. Se abrirá un menú de navegación en el lado izquierdo de la pantalla donde seleccionará la lista desplegable de Xbox Live y, a continuación, el **cuentas de prueba de Xbox** vínculo.
2. Desde la página de configuración de creadores de Xbox Live, busque la sección de prueba y haga clic en el vínculo titulado **autorizar Xbox Live cuentas para su entorno de prueba**

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>Autorizar a una cuenta de Xbox Live para su entorno de prueba

* Una vez dentro de la página de cuentas de prueba de Xbox debería ver una lista de todas las cuentas autorizadas actualmente. Para autorizar a una cuenta nueva, haga clic en el botón Agregar cuenta

![Agregar cuentas de Xbox Live](../images/creators_udc/add_test_account.png)

* Modal debería aparecer en la pantalla con un cuadro de texto donde puede escribir la dirección de correo electrónico de la cuenta deseada

![Adición de Xbox Live cuentas Modal](../images/creators_udc/add_test_account_modal.png)

* Haga clic en el botón Agregar para validar que la dirección de correo electrónico existe y tiene una cuenta asociada de Xbox Live. Si se superan las comprobaciones modal desaparecerá y podrá ver la nueva cuenta en la tabla indica ya está autorizada correctamente para su entorno de prueba

![Agregar éxito en vivo de las cuentas de Xbox](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>Solución de problemas

El correo electrónico que escribió en el estado modal se ejecuta a través de algunas comprobaciones, incluida una búsqueda para asegurarse de que está asociada una cuenta de Xbox Live con él. Si se produce un error en cualquiera de estas comprobaciones, la cuenta no se agregan a la tabla y, por tanto, no autorizada y obtendrá un error de "Lo sentimos, hubo un problema al agregar la dirección de correo electrónico".

Es una buena prueba si tiene problemas intentar iniciar sesión con la cuenta [Xbox.com](https://www.xbox.com/live/). Si no puede iniciar sesión, a continuación, la cuenta no es una cuenta de Xbox Live.
