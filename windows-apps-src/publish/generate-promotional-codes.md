---
Description: Puedes generar códigos de promoción para una aplicación o complemento que hayas publicado en la Microsoft Store.
title: Generar códigos promocionales
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, código de promoción, códigos de promoción, token, tokens
ms.localizationpriority: medium
ms.openlocfilehash: 931b3abfe13a3834d991ee1a0a38c752b9e3f719
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64745028"
---
# <a name="generate-promotional-codes"></a>Generar códigos promocionales


[Centro de partners](https://partner.microsoft.com/dashboard) le permite generar códigos promocionales para una aplicación o un complemento que se ha publicado en la Microsoft Store. Los códigos promocionales son una forma sencilla de ofrecer a usuarios influyentes acceso gratuito a tu aplicación o complemento. También puede usar los códigos promocionales para escenarios de servicio al cliente proporcionando a los usuarios acceso gratuito a la aplicación o el complemento, o para [pruebas beta](beta-testing-and-targeted-distribution.md) con Windows 10. 

Cada código de promoción tiene una única pueden canjear dirección URL correspondiente que un cliente puede hacer clic con el fin de canjear el código e instalar la aplicación o el complemento desde la Microsoft Store.  Recuerda que la aplicación debe pasar a la fase final de publicación del [proceso de certificación de aplicaciones](the-app-certification-process.md) antes de que los clientes puedan canjear un código promocional para instalarla.

Puede generar códigos de uso único (y distribuir uno para cada cliente), o bien puede generar un código que se puede usar varias veces por un número especificado de los clientes.

> [!TIP]
> Puedes usar [notificaciones push dirigidas](send-push-notifications-to-your-apps-customers.md) para distribuir un código promocional a un segmento de tus clientes. Al hacerlo, asegúrate de usar un código promocional que permita a varios clientes usar el mismo código.


## <a name="promotional-code-policies"></a>Directivas de códigos promocionales

Ten en cuenta las siguientes directivas para los códigos promocionales:

-   Puedes generar códigos de promoción para cualquier aplicación o complemento (con la excepción de complementos de suscripción) que hayas publicado en la Microsoft Store. Los clientes pueden canjear los códigos en cualquier versión de Windows que sea compatible con la aplicación o complemento.
-   Para los juegos:
    - Puede generar hasta 5000 códigos promocionales por juego.
    - Los códigos promocionales generados para juegos no expiren nunca.
- Para los demás tipos de aplicaciones o complementos:
    - En cualquier período de seis meses, puede generar hasta 1600 códigos promocionales de uso único o cualquier número de usar varios códigos de forma que el total había permitida canjes no exceda de 1600.
    - El período de 6 meses comienza al generar el primero se crea el código de promoción y tiene una duración de seis meses, independientemente de si se establece una fecha de expiración anterior sobre los códigos.
    - Los códigos creados durante un período de seis meses existente será contarán para el número de códigos generados dentro de ese período, incluso si expira una vez finalizado el período (por ejemplo, si genera un código en el último día de la ventana de seis meses, será will ser todavía  ser válidos durante un total de 6 meses desde su creación).
-   Debe seguir los requisitos definidos en el [acuerdo del desarrollador de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), incluida la sección **3 k. Los códigos promocionales**.

> [!NOTE]
> Puede usar los códigos promocionales incluso si la aplicación no está disponible para los clientes (es decir, si ha seleccionado **hacen que este producto disponible pero no detectables en el Store** con el **detener adquisición: Cualquier cliente con un vínculo directo puede ver la lista de Store del producto, pero solo puede descargar si pertenece el producto antes de, o tienen un código promocional y está usando un dispositivo Windows 10** opción en su envío [detectabilidad ](choose-visibility-options.md#discoverability) sección). Con esta opción, los clientes deben estar en Windows 10 (incluidos Xbox) con el fin de adquirir el producto con un código promocional.


## <a name="order-promotional-codes"></a>Pedir códigos promocionales

Para los códigos promocionales de orden para una aplicación o complemento:

1.  En el menú de navegación izquierdo de [centro de partners](https://partner.microsoft.com/dashboard), expanda **atraiga** y, a continuación, seleccione **códigos de promoción**.

2.   En la página **Códigos promocionales**, haz clic en **Pedir códigos**.

3.  En la página **Nuevo pedido de códigos promocionales**, escribe lo siguiente:
    -   Selecciona la aplicación o complemento para el que quieras generar códigos. (Ten en cuenta que no puedes generar códigos de promoción para complementos de suscripción).
    -   Especifica un nombre para el pedido. Este nombre te servirá para diferenciar distintos pedidos de códigos cuando revises los datos de uso de los códigos promocionales.
    -   Selecciona el tipo de pedido. Puedes elegir generar un conjunto de códigos de promoción que pueden utilizarse una vez o generar un código de promoción que puede usarse varias veces.
    -   Especifica el número de códigos que quieras pedir (si se genera un conjunto de códigos) o el número de veces que se pueden canjear el código (si se genera un código que puede usarse varias veces).
    -   Especifica cuándo deben activarse los códigos promocionales. Para elegir una determinada fecha y hora de inicio, desactiva la casilla **Los códigos están activos inmediatamente**. En caso contrario, los códigos se activan inmediatamente (aunque el producto debe haber completado el proceso de publicación de pedido para un cliente utilizar el código).
    -   Especifica cuándo deben expirar los códigos promocionales. Para elegir una fecha y hora de expiración antes de los 6 meses, desactiva la casilla **Los códigos expiran después de seis meses**.

4.  Haz clic en **Pedir códigos**. A continuación, volverás a la página **Códigos promocionales**, donde podrás ver tu nuevo pedido en la tabla de resumen de pedidos de códigos promocionales para esa aplicación.


## <a name="download-and-distribute-promotional-codes"></a>Descargar y distribuir códigos promocionales

Para descargar un pedido completado de códigos promocionales y distribuirlos a los clientes:

1.  En el menú de navegación izquierdo de [centro de partners](https://partner.microsoft.com/dashboard), expanda **atraiga** y, a continuación, seleccione **códigos de promoción.**
2.  Haz clic en el vínculo **Descargar** del pedido del código promocional y luego guarda en el equipo el archivo generado. Este archivo contiene información sobre el pedido de códigos promocionales en formato de valores separados por tabulaciones (.tsv).
3.  Abre el archivo .tsv en el editor que quieras. Para una mejor experiencia, abre el archivo .tsv en una aplicación que pueda mostrar los datos en una estructura tabular, como Microsoft Excel. Sin embargo, puedes abrir el archivo en cualquier editor de texto.

    El archivo contiene las siguientes columnas de datos para cada código:

    -   **Nombre de producto**: El nombre de la aplicación o el complemento que está asociado el código.
    -   **Nombre del pedido**: El nombre de la orden en que se generó este código.
    -   **El código de promoción**: El propio código. Se trata de una cadena de 5 x 5 caracteres alfanuméricos separados por guiones. Por ejemplo: DM3GY M2GYM 6YMW6 4QHHT 23W2Z
    -   **Dirección URL puede canjear**: La dirección URL que un cliente puede utilizar para canjear el código e instalar la aplicación o complemento. La dirección URL tiene el formato siguiente: https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; promotional_code >
    -   **Fecha de inicio**: La fecha que se activaron este código.
    -   **Punto de expirar fecha**: La fecha de que expiración de este código.
    -   **Id. de código**: Un identificador único para este código.
    -   **Id. de pedido**: Un identificador único para el orden en el que se generó este código.
    -   **Dado a**: Un campo vacío que puede usar para realizar un seguimiento de qué cliente dio el código.
    -   **Disponible**: El número de veces que el código sigue estando disponible para canjear (en el momento en que se generó el archivo).
    -   **Redeemed**: El número de veces que se ha canjeado el código (en el momento en que se generó el archivo).

4.  Distribuye las direcciones URL canjeables a los clientes mediante el formato de comunicación que prefieras (por ejemplo, notificaciones dirigidas, correo electrónico, mensaje SMS o tarjetas impresas). Te recomendamos que incluyas lo siguiente en la comunicación:
    -   Una explicación de para qué aplicación o complemento es el código promocional y, de forma opcional, una descripción de por qué el cliente recibe el código.
    -   La dirección URL canjeable para el código.
    -   Instrucciones que guíen al cliente para que visite la dirección URL canjeable, inicie sesión con su cuenta Microsoft y siga las instrucciones para descargar e instalar la aplicación.


## <a name="code-redemption-user-experience"></a>Experiencia de usuario del canje de códigos

Después de distribuir un código promocional (o su dirección URL puede canjear) a un cliente, puede haga clic en la dirección URL para obtener el producto de forma gratuita. Al hacer clic en la dirección URL canjeable, se abrirá una página **Canjear tu código** en <https://account.microsoft.com/billing/redeem>. Esta página incluye una descripción de la aplicación que el usuario va a canjear. Si el cliente no ha iniciado sesión con su cuenta Microsoft, se le pedirá que lo haga. También puedes visitar <https://account.microsoft.com/billing/redeem> y escribir el código directamente.

> [!IMPORTANT]
> Te recomendamos que no distribuyas códigos promocionales a los clientes hasta que tu producto no haya completado el proceso de publicación (incluso si seleccionaste **Make this product available but not discoverable in the Store**). Los clientes verán un error si tratan de usar el código promocional de un producto que no se ha publicado aún.

Después de que el cliente haga clic en **Canjear**, se abrirá la página de información general de la aplicación en la Microsoft Store, (si están en un dispositivo Windows 10 o Windows 8.1), donde puede hacer clic en **Instalar** para descargar e instalar la aplicación de forma gratuita. Si, por el contrario, el cliente está usando un equipo o dispositivo que no tiene la Microsoft Store instalada, el vínculo abrirá la página web de la Microsoft Store de la aplicación. El código se aplicará a la cuenta de Microsoft del cliente, para que más tarde puedan descargar la aplicación en un dispositivo Windows (que esté asociado con la misma cuenta de Microsoft) de forma gratuita.

> [!NOTE]
> En algunos casos, es posible que vea un cliente un **comprar** botón en lugar de **instalar**, aunque la aplicación se ha canjeado correctamente mediante el código promocional. El cliente puede hacer clic en **Comprar** para instalar la aplicación de forma gratuita.


## <a name="review-your-promotional-codes"></a>Revisar los códigos promocionales

Para revisar un resumen detallado de los pedidos de código de promoción para sus aplicaciones y complementos, navegue hasta la **códigos promocionales** página (en el menú de navegación izquierdo del centro de partners, expanda **atraiga** y, a continuación, seleccione **Códigos de promoción**). Puedes revisar la siguiente información para todos tus códigos de promoción actuales e inactivos actuales:
-   Nombre del pedido
-   Aplicación o complemento
-   Fecha de inicio
-   Fecha de expiración
-   Disponible
-   Canjeado

También puedes [descargar](#download-and-distribute-promotional-codes) un pedido de esta tabla.

 
