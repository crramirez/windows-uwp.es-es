---
author: jnHs
Description: You can generate promotional codes for an app or add-on that you have published in the Microsoft Store.
title: Generar códigos de promoción
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.author: wdg-dev-content
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, código de promoción, códigos de promoción, token, tokens
ms.localizationpriority: medium
ms.openlocfilehash: 37263794ffed6660f71c5e16195e992588c16d4a
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4686411"
---
# <a name="generate-promotional-codes"></a>Generar códigos de promoción


Puedes generar códigos de promoción para una aplicación o complemento que hayas publicado en la Microsoft Store. Los códigos de promoción son una forma sencilla de ofrecer a usuarios influyentes acceso gratuito a tu aplicación o complemento. También puedes usarlos en situaciones de atención al cliente y ofrecer a los usuarios acceso gratuito a tu aplicación o complemento, o para [realizar pruebas beta](beta-testing-and-targeted-distribution.md) con Windows 10. 

Cada código promocional contiene una correspondiente URL única y canjeable que un cliente puede hacer clic con el fin de canjear el código e instalar la aplicación o complemento desde Microsoft Store.  Recuerda que la aplicación debe pasar a la fase final de publicación del [proceso de certificación de aplicaciones](the-app-certification-process.md) antes de que los clientes puedan canjear un código promocional para instalarla.

Puedes generar códigos de uso único (y distribuir uno a cada cliente), o puedes elegir generar un código que puede usarse varias veces por un número especificado de los clientes.

> [!TIP]
> Puedes usar [notificaciones push dirigidas](send-push-notifications-to-your-apps-customers.md) para distribuir un código promocional a un segmento de tus clientes. Al hacerlo, asegúrate de usar un código promocional que permita a varios clientes usar el mismo código.


## <a name="promotional-code-policies"></a>Directivas de códigos promocionales

Ten en cuenta las siguientes directivas para los códigos de promoción:

-   Puedes generar códigos de promoción para cualquier aplicación o complemento (con la excepción de complementos de suscripción) que hayas publicado en la Microsoft Store. Los clientes pueden canjear los códigos en cualquier versión de Windows que sea compatible con la aplicación o complemento.
-   Los códigos de promoción expiran seis meses después de la fecha de su pedido (a menos que elijas una fecha de expiración anterior).
-   Cada 6 meses, puedes generar códigos que permiten hasta 1600 canjes para tus aplicaciones o complementos. El período de 6 meses comienza cuando se envía el primer pedido de códigos promocionales, incluso si elige una fecha de expiración anterior. El total de 1600 canjes por producto se aplica a los códigos de uso único y a los códigos que pueden usarse varias veces.
-   Debes seguir los requisitos definidos en el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), incluida la sección **3k. Códigos promocionales**.

> [!NOTE]
> Puedes usar códigos promocionales incluso si la aplicación está disponible para los clientes (es decir, si has seleccionado **hacer este producto disponible, pero no detectable en Store** con la adquisición de detención **: los clientes con un vínculo directo podrán ver la tienda del producto Descripción, pero solo podrán descargarlo si ya tienen el producto, o tienen un código promocional y están usando un dispositivo de Windows 10** opción en la sección de [visibilidad](choose-visibility-options.md#discoverability) de tu envío). Con esta opción, los clientes deben ser en Windows 10 (incluyendo Xbox) para poder adquirir el producto con un código promocional.


## <a name="order-promotional-codes"></a>Pedir códigos promocionales

Para pedir códigos promocionales para una aplicación o complemento:

1.  En el menú de navegación izquierdo del panel de información del Panel del Centro de desarrollo de Windows, expande **Interactuar** y luego selecciona **Códigos promocionales**.

2.   En la página **Códigos promocionales**, haz clic en **Pedir códigos**.

3.  En la página **Nuevo pedido de códigos promocionales**, escribe lo siguiente:
    -   Selecciona la aplicación o complemento para el que quieras generar códigos. (Ten en cuenta que no puedes generar códigos de promoción para complementos de suscripción).
    -   Especifica un nombre para el pedido. Este nombre te servirá para diferenciar distintos pedidos de códigos cuando revises los datos de uso de los códigos promocionales.
    -   Selecciona el tipo de pedido. Puedes elegir generar un conjunto de códigos de promoción que pueden utilizarse una vez o generar un código de promoción que puede usarse varias veces.
    -   Especifica el número de códigos que quieras pedir (si se genera un conjunto de códigos) o el número de veces que se pueden canjear el código (si se genera un código que puede usarse varias veces).
    -   Especifica cuándo deben activarse los códigos promocionales. Para elegir una determinada fecha y hora de inicio, desactiva la casilla **Los códigos están activos inmediatamente**. De lo contrario, los códigos se activarán inmediatamente (aunque tu producto debe completado el proceso de publicación en orden para un cliente usar el código).
    -   Especifica cuándo deben expirar los códigos promocionales. Para elegir una fecha y hora de expiración antes de los 6 meses, desactiva la casilla **Los códigos expiran después de seis meses**.

4.  Haz clic en **Pedir códigos**. A continuación, volverás a la página **Códigos promocionales**, donde podrás ver tu nuevo pedido en la tabla de resumen de pedidos de códigos promocionales para esa aplicación.


## <a name="download-and-distribute-promotional-codes"></a>Descargar y distribuir códigos promocionales

Para descargar un pedido completado de códigos promocionales y distribuirlos a los clientes:

1.  En el menú de navegación izquierdo del panel de información del Centro de desarrollo de Windows, expande **Interactuar** y luego selecciona **Códigos promocionales**.
2.  Haz clic en el vínculo **Descargar** del pedido del código promocional y luego guarda en el equipo el archivo generado. Este archivo contiene información sobre el pedido de códigos promocionales en formato de valores separados por tabulaciones (.tsv).
3.  Abre el archivo .tsv en el editor que quieras. Para una mejor experiencia, abre el archivo .tsv en una aplicación que pueda mostrar los datos en una estructura tabular, como MicrosoftExcel. Sin embargo, puedes abrir el archivo en cualquier editor de texto.

    El archivo contiene las siguientes columnas de datos para cada código:

    -   **Nombre del producto**: es el nombre de la aplicación o complemento con el que está asociado el código.
    -   **Nombre del pedido**: es el nombre del pedido con el que se generó este código.
    -   **Código promocional**: es el propio código. Se trata de una cadena de 5 x 5 caracteres alfanuméricos separados por guiones. Por ejemplo: DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **Dirección URL canjeable**: Es la dirección URL en la que el cliente puede canjear el código e instalar la aplicación o complemento. La dirección URL tiene el siguiente formato: http://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; promotional_code >
    -   **Fecha de inicio**: La fecha en la que este código se activó.
    -   **Fecha de expiración**: La fecha de expiración de este código.
    -   **Id. de código**: Un identificador único de este código.
    -   **Id. de pedido**: Un identificador único del pedido en el que se generó este código.
    -   **Entregado a**: Un campo vacío que puedes usar para realizar un seguimiento de los clientes a los que les diste el código.
    -   **Disponible**: El número de veces que el código sigue estando disponible para canjear (en el momento en el que se generó el archivo).
    -   **Canjeado**: El número de veces que el código se ha canjeado (en el momento en el que se generó el archivo).

4.  Distribuye las direcciones URL canjeables a los clientes mediante el formato de comunicación que prefieras (por ejemplo, notificaciones dirigidas, correo electrónico, mensaje SMS o tarjetas impresas). Te recomendamos que incluyas lo siguiente en la comunicación:
    -   Una explicación de para qué aplicación o complemento es el código promocional y, de forma opcional, una descripción de por qué el cliente recibe el código.
    -   La dirección URL canjeable para el código.
    -   Instrucciones que guíen al cliente para que visite la dirección URL canjeable, inicie sesión con su cuenta Microsoft y siga las instrucciones para descargar e instalar la aplicación.


## <a name="code-redemption-user-experience"></a>Experiencia de usuario del canje de códigos

Después de distribuir un código promocional (o su dirección URL canjeable) a un cliente, pueden hacer clic en la dirección URL para obtener el producto de forma gratuita. Al hacer clic en la dirección URL canjeable, se abrirá una página **Canjear tu código** en <https://account.microsoft.com/billing/redeem>. Esta página incluye una descripción de la aplicación que el usuario va a canjear. Si el cliente no ha iniciado sesión con su cuenta Microsoft, se le pedirá que lo haga. También puedes visitar <https://account.microsoft.com/billing/redeem> y escribir el código directamente.

> [!IMPORTANT]
> Te recomendamos que no distribuyas códigos promocionales a los clientes hasta que tu producto no haya completado el proceso de publicación (incluso si seleccionaste **Make this product available but not discoverable in the Store**). Los clientes verán un error si tratan de usar el código promocional de un producto que no se ha publicado aún.

Después de que el cliente haga clic en **Canjear**, se abrirá la página de información general de la aplicación en la Microsoft Store, (si están en un dispositivo Windows 10 o Windows 8.1), donde puede hacer clic en **Instalar** para descargar e instalar la aplicación de forma gratuita. Si, por el contrario, el cliente está usando un equipo o dispositivo que no tiene la Microsoft Store instalada, el vínculo abrirá la página web de la Microsoft Store de la aplicación. El código se aplicará a la cuenta de Microsoft del cliente, para que más tarde puedan descargar la aplicación en un dispositivo Windows (que esté asociado con la misma cuenta de Microsoft) de forma gratuita.

> [!NOTE]
> En algunos casos, un cliente puede ver un botón **comprar** en lugar de **instalar**, incluso si la aplicación se canjeó correctamente mediante el código promocional. El cliente puede hacer clic en **Comprar** para instalar la aplicación de forma gratuita.


## <a name="review-your-promotional-codes"></a>Revisar los códigos promocionales

Para ver un resumen detallado de pedidos de códigos de promoción para tus aplicaciones y complementos, ve a la página **Promotional codes** (en el menú de navegación izquierdo del panel del Centro de desarrollo, amplía **Atraer** y, a continuación, selecciona **Promo codes**). Puedes revisar la siguiente información para todos tus códigos de promoción actuales e inactivos actuales:
-   Nombre del pedido
-   Aplicación o complemento
-   Fecha de inicio
-   Fecha de expiración
-   Disponible
-   Canjeado

También puedes [descargar](#download-and-distribute-promotional-codes) un pedido de esta tabla.

 
