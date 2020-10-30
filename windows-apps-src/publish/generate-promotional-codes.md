---
description: Puede generar códigos promocionales para una aplicación o un complemento que haya publicado en el Microsoft Store.
title: Generar códigos promocionales
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, código de promoción, códigos de promoción, token, tokens
ms.localizationpriority: medium
ms.openlocfilehash: b9640ad9ddc00741677b8a1341c12d0489564368
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029648"
---
# <a name="generate-promotional-codes"></a>Generar códigos promocionales


El [centro de Partners](https://partner.microsoft.com/dashboard) le permite generar códigos promocionales para una aplicación o un complemento que haya publicado en el Microsoft Store. Los códigos promocionales son una forma sencilla de ofrecer a usuarios influyentes acceso gratuito a tu aplicación o complemento. También puedes usarlos en situaciones de atención al cliente y ofrecer a los usuarios acceso gratuito a tu aplicación o complemento, o para [realizar pruebas beta](beta-testing-and-targeted-distribution.md) con Windows 10. 

Cada código promocional tiene una dirección URL única canjeable correspondiente en la que un cliente puede hacer clic para canjear el código e instalar su aplicación o complemento desde el Microsoft Store.  Tenga en cuenta que la aplicación debe pasar la fase de publicación final del [proceso de certificación](the-app-certification-process.md) de la aplicación antes de que los clientes puedan canjear un código promocional para instalarlo.

Puede generar códigos de un solo uso (y distribuir uno a cada cliente), o puede generar un código que se puede usar varias veces en un número especificado de clientes.

> [!TIP]
> Puede usar [notificaciones](send-push-notifications-to-your-apps-customers.md) de entrega de destino para distribuir un código promocional a un segmento de sus clientes. Al hacerlo, asegúrese de usar un código promocional que permita que varios clientes utilicen el mismo código.


## <a name="promotional-code-policies"></a>Directivas de códigos promocionales

Ten en cuenta las siguientes directivas para los códigos promocionales:

-   Puede generar códigos promocionales para cualquier aplicación o complemento (con la excepción de los complementos de suscripción) que publicó en el Microsoft Store. Los clientes pueden canjear los códigos en cualquier versión de Windows que sea compatible con la aplicación o el complemento.
-   Para juegos:
    - Puede generar hasta 5000 códigos promocionales por juego.
    - Los códigos promocionales generados para juegos nunca expiran.
- Para todos los demás tipos de aplicaciones o complementos:
    - En cualquier período de seis meses, puede generar hasta 1600 códigos promocionales de un solo uso, o cualquier número de varios códigos de uso, de modo que el total de canjes permitidos no supere los 1600.
    - El período de 6 meses comienza cuando se genera el primer código promocional y dura 6 meses, independientemente de si se establece una fecha de expiración anterior en los códigos.
    - Los códigos creados durante un período de seis meses existente se contabilizarán en el número de códigos generados en ese período, incluso si expirarán una vez finalizado el período (por ejemplo, si genera un código el último día de la ventana de seis meses, seguirá siendo válido durante un total de 6 meses a partir de su creación).
-   Debes seguir los requisitos definidos en el [Acuerdo para desarrolladores de aplicaciones](/legal/windows/agreements/app-developer-agreement), incluida la sección **3k. Códigos promocionales** .

> [!NOTE]
> Puede usar códigos promocionales incluso si la aplicación no está disponible para los clientes (es decir, si ha seleccionado **hacer que este producto esté disponible pero no es reconocible en el almacén** con la detención de la **adquisición: cualquier cliente con un vínculo directo puede ver la lista de la tienda del producto, pero solo puede descargarla si poseía el producto antes, o tener un código promocional y usar una opción de dispositivo de Windows 10** en la sección [detectabilidad](choose-visibility-options.md#discoverability) del envío). Con esta opción, los clientes deben estar en Windows 10 (incluido Xbox) para poder adquirir el producto con un código promocional.


## <a name="order-promotional-codes"></a>Pedir códigos promocionales

Para solicitar códigos promocionales para una aplicación o complemento:

1.  En el menú de navegación izquierdo del [centro de Partners](https://partner.microsoft.com/dashboard), expanda **atraer** y seleccione códigos de **promoción** .

2.   En la página **Códigos promocionales** , haz clic en **Pedir códigos** .

3.  En la página **Nuevo pedido de códigos promocionales** , escribe lo siguiente:
    -   Selecciona la aplicación o complemento para el que quieras generar códigos. (Tenga en cuenta que no puede generar códigos promocionales para complementos de suscripción).
    -   Especifica un nombre para el pedido. Este nombre te servirá para diferenciar distintos pedidos de códigos cuando revises los datos de uso de los códigos promocionales.
    -   Selecciona el tipo de pedido. Puede optar por generar un conjunto de códigos de promoción que se pueden usar una vez, o bien puede optar por generar un código de promoción que se puede usar varias veces.
    -   Especifique el número de códigos para ordenar (si se genera un conjunto de códigos) o el número de veces que se puede canjear el código (si se genera un código que se va a usar varias veces).
    -   Especifica cuándo deben activarse los códigos promocionales. Para elegir una determinada fecha y hora de inicio, desactiva la casilla **Los códigos están activos inmediatamente** . De lo contrario, los códigos se convertirán en activos de inmediato (aunque el producto debe haber completado el proceso de publicación para que un cliente pueda usar el código).
    -   Especifica cuándo deben expirar los códigos promocionales. Para elegir una fecha y hora de expiración específicas anteriores a 6 meses, desactive la casilla los **códigos caducan después de 6 meses** .

4.  Haz clic en **Pedir códigos** . A continuación, se le devolverá a la página **códigos promocionales** , donde podrá ver el nuevo pedido en la tabla de Resumen de pedidos de código promocional para esa aplicación.


## <a name="download-and-distribute-promotional-codes"></a>Descargar y distribuir códigos promocionales

Para descargar un pedido de código promocional entregado y distribuir los códigos a los clientes:

1.  En el menú de navegación izquierdo del [centro de Partners](https://partner.microsoft.com/dashboard), expanda **atraer** y seleccione **códigos de promoción.**
2.  Haga clic en el vínculo de **descarga** para el orden de código promocional y, a continuación, guarde el archivo generado en el equipo. Este archivo contiene información sobre el orden de los códigos promocionales en formato de valores separados por tabulaciones (. TSV).
3.  Abra el archivo. TSV en el editor de su elección. Para obtener la mejor experiencia, abra el archivo. TSV en una aplicación que pueda mostrar los datos en una estructura tabular, como Microsoft Excel. Sin embargo, puede abrir el archivo en cualquier editor de texto.

    El archivo contiene las siguientes columnas de datos para cada código:

    -   **Nombre del producto** : es el nombre de la aplicación o complemento con el que está asociado el código.
    -   **Nombre del pedido** : el nombre del orden en el que se generó este código.
    -   **Código promocional** : es el propio código. Se trata de una cadena de 5 x 5 caracteres alfanuméricos separados por guiones. Por ejemplo: DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **URL canjeable** : la dirección URL que un cliente puede usar para canjear el código e instalar la aplicación o el complemento. La dirección URL tiene el siguiente formato: https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt ;p romotional_code>
    -   **Fecha de inicio** : la fecha en que este código estuvo activo.
    -   **Fecha de expiración** : es la fecha de expiración de este código.
    -   **Id. de código** : es un identificador único de este código.
    -   **ID** . de pedido: un identificador único para el orden en el que se generó este código.
    -   **Dado a** : un campo vacío que puede usar para realizar un seguimiento del cliente al que dio el código.
    -   **Disponible** : el número de veces que el código todavía está disponible para canjear (en el momento en que se generó el archivo).
    -   **Canjeado** : el número de veces que se ha canjeado el código (en el momento en que se generó el archivo).

4.  Distribuya las direcciones URL canjeables a los clientes a través de cualquier formato de comunicación que prefiera (por ejemplo, notificaciones de destino, correo electrónico, mensajes SMS o tarjetas impresas). Te recomendamos que incluyas lo siguiente en la comunicación:
    -   Explicación de la aplicación o el complemento para el que se encuentra el código promocional y, opcionalmente, una descripción de por qué el cliente está recibiendo el código.
    -   La dirección URL canjeable para el código.
    -   Instrucciones que guían al cliente a visitar la dirección URL canjeable, iniciar sesión con su cuenta de Microsoft y seguir las instrucciones para descargar e instalar la aplicación.


## <a name="code-redemption-user-experience"></a>Experiencia de usuario del canje de códigos

Después de distribuir un código promocional (o su dirección URL canjeable) a un cliente, puede hacer clic en la dirección URL para obtener el producto de forma gratuita. Al hacer clic en la dirección URL canjeable se iniciará una página de **canje de código** autenticada en <https://account.microsoft.com/billing/redeem> . Esta página incluye una descripción de la aplicación que el usuario va a canjear. Si el cliente no ha iniciado sesión con su cuenta de Microsoft, es posible que se le pida que lo haga. El cliente también puede visitar <https://account.microsoft.com/billing/redeem> y escribir el código directamente.

> [!IMPORTANT]
> Le recomendamos que no distribuya códigos promocionales a los clientes hasta que el producto haya completado el proceso de publicación (incluso si ha seleccionado la opción **hacer que este producto esté disponible pero no es reconocible en el almacén** ). Los clientes verán un error si intentan usar un código promocional para un producto que aún no se ha publicado.

Una vez que el cliente hace clic en **canjear** , el Microsoft Store se abrirá en la página de información general de la aplicación (si se encuentran en un dispositivo Windows 10 o Windows 8.1), donde pueden hacer clic en **instalar** para descargar e instalar la aplicación de forma gratuita. Si el cliente está en un equipo o dispositivo que no tiene instalado el Microsoft Store, el vínculo iniciará la Página Web de Microsoft Store de la aplicación. El código se aplicará al cuenta de Microsoft del cliente, por lo que más adelante podrán descargar la aplicación en un dispositivo Windows (que está asociado al mismo cuenta de Microsoft) de forma gratuita.

> [!NOTE]
> En algunos casos, un cliente puede ver un botón **comprar** en lugar de **instalarlo** , aunque la aplicación se haya canjeado correctamente a través del código promocional. El cliente puede hacer clic en **comprar** para instalar la aplicación sin cargo alguno.


## <a name="review-your-promotional-codes"></a>Revisar los códigos promocionales

Para revisar un resumen detallado de los pedidos de código promocional de sus aplicaciones y complementos, vaya a la página de **códigos promocionales** (en el menú de navegación izquierdo del centro de Partners, expanda **atraer** y seleccione **códigos de promoción** ). Puede revisar los detalles siguientes para todos los códigos promocionales actuales e inactivos:
-   Nombre del pedido
-   Aplicación o complemento
-   Fecha de inicio
-   Fecha de expiración
-   Disponible
-   Canjear

También puede [Descargar](#download-and-distribute-promotional-codes) un pedido de esta tabla.

 
