---
author: jnHs
Description: "Puedes generar códigos promocionales para una aplicación o un producto desde la aplicación (IAP) que hayas publicado en la Tienda Windows."
title: "Genera códigos promocionales"
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4001f374a80cd7c62df6230a9429dd5b5a19d2b2

---

# Genera códigos promocionales


Puedes generar códigos promocionales para una aplicación o un producto desde la aplicación (IAP) que hayas publicado en la Tienda Windows. Los códigos promocionales son una forma sencilla de ofrecer a usuarios influyentes acceso gratuito a tu aplicación o IAP. También puedes usarlos en situaciones de atención al cliente y ofrecer a los usuarios acceso gratuito a tu aplicación o IAP, o para [realizar pruebas beta](beta-testing-and-targeted-distribution.md) con Windows 10.

Cada código promocional contiene una dirección URL única y canjeable que puedes distribuir a un usuario. El usuario solo tiene que hacer clic en la dirección URL para canjear el código e instalar la aplicación o IAP desde la Tienda Windows.

En el panel del Centro de desarrollo de Windows, puedes:

-   Realizar un pedido de códigos promocionales para tu aplicación.
-   Descargar un pedido completado de códigos promocionales.
-   Revisa el uso de los códigos promocionales para las aplicaciones, como los siguientes:
    -   Resúmenes de pedidos de códigos promocionales de todas las aplicaciones en general (en la página **Información general del panel**) y de cada aplicación en particular (en la página **Información general de la aplicación** de cada aplicación).
    -   Un resumen detallado de los pedidos de códigos promocionales de cada aplicación (en la página **Códigos promocionales** de cada aplicación).

> **Nota**  Puedes generar códigos de promoción incluso si seleccionaste la opción **Ocultar esta aplicación en la tienda e impedir la compra. Los clientes con un código promocional aún pueden descargarla en dispositivos con Windows 10** en la página del panel [Precios y disponibilidad](set-app-pricing-and-availability.md) de tu aplicación. La aplicación debe pasar a la fase final de publicación del [proceso de certificación de aplicaciones](the-app-certification-process.md) antes de que los usuarios puedan canjear un código promocional para instalarla.

## Directivas de códigos promocionales


Ten en cuenta las siguientes directivas para los códigos promocionales:

-   Puedes generar códigos promocionales para cualquier aplicación o IAP que hayas publicado en la Tienda Windows. Los usuarios pueden canjear los códigos en cualquier versión de Windows compatible con la aplicación o con la IAP.
-   Los códigos promocionales expiran seis meses después de la fecha en que los pediste.
-   Cada 6 meses puedes generar hasta 250 códigos promocionales para cada una de tus aplicaciones o IAP. El período de 6 meses comienza en la fecha en que se envía el primer pedido de códigos promocionales.
-   Debes seguir los requisitos definidos en el [Acuerdo para desarrolladores de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058), incluida la sección **3k. Códigos promocionales**.

## Pedido de códigos promocionales


Para pedir códigos promocionales para una aplicación o IAP que has publicado en la Tienda Windows:

1.  En el panel del Centro de desarrollo de Windows, realiza una de las siguientes acciones:
    -   En la página de tu aplicación **Introducción general de la aplicación**, busca la sección **Códigos promocionales** y haz clic en **Pedir códigos**.
    -   En cualquier página del panel de la aplicación, en el menú de navegación izquierdo, expande **Rentabilidad** y haz clic en **Códigos promocionales**. En la página **Códigos promocionales**, haz clic en **Pedir códigos**.

2.  En la página **Nuevo pedido de códigos promocionales**, escribe lo siguiente:
    -   Selecciona la aplicación o IAP para la que quieres generar códigos.
    -   Especifica un nombre para el pedido. Este nombre te servirá para diferenciar los diferentes pedidos de códigos cuando revises los datos de uso de los códigos promocionales.
    -   Especifica cuántos códigos quieres pedir.

3.  Haz clic en **Pedir códigos**. El pedido se envía y el panel te lleva a la página **Código promocionales**, donde el nuevo pedido aparece como **Pendiente** en la tabla resumen de pedidos de códigos promocionales.

Los códigos promocionales suelen estar listos para su descarga menos de una hora después de realizar el pedido, aunque algunos pedidos tardan más en procesarse. Cuando los códigos están listos para su descarga, el estado del pedido cambia a **Disponible**.

## Descargar y distribuir códigos promocionales


Para descargar un pedido completado y distribuir los códigos promocionales a los usuarios de la aplicación:

1.  En el panel del Centro de desarrollo de Windows, vuelve a la página **Códigos promocionales** de la aplicación (para ello, expande **Monetización** y haz clic en **Códigos promocionales**).
2.  Confirma que es el estado del pedido es **Disponible**. Haz clic en el vínculo **Descargar** del pedido y guarda en el equipo el archivo proporcionado. Este archivo contiene información sobre el pedido de códigos promocionales en formato de valores separados por tabulaciones (TSV).
3.  Abre el archivo TSV en el editor que quieras. Para una mejor experiencia, abre el archivo TSV en una aplicación que pueda mostrar los datos en una estructura tabular, como Microsoft Excel. Sin embargo, también puedes abrir el archivo en cualquier editor de texto.

    El archivo contiene las siguientes columnas de datos para cada código:

    -   **Nombre del producto**: es el nombre de la aplicación o la IAP con el que está asociado el código.
    -   **Nombre del pedido**: es el nombre del pedido con el que se suministró este código.
    -   **Código promocional**: es el propio código. Se trata de una cadena de 5 x 5 caracteres alfanuméricos separados por guiones. Por ejemplo:

        DM3GY M2GYM 6YMW6 4QHHT 23W2Z

    -   **Dirección URL canjeable**: es la dirección URL en la que el usuario puede canjear el código e instalar la aplicación o la IAP. La dirección URL tiene el siguiente formato:

        https://account.microsoft.com/billing/redeem?mstoken=&lt;promotional_code>

    -   **Fecha del pedido**: es la fecha en la que se hizo el pedido de este código.
    -   **Fecha de expiración**: es la fecha de expiración de este código.
    -   **Id. de código**: es un identificador único de este código.
    -   **Id. de pedido**: es un identificador único del pedido en el que se suministró este código.
    -   **Entregado a**: es un campo vacío que puedes rellenar con un valor que identifique al usuario al que se le dio el código.

4.  Distribuye las direcciones URL canjeables a los usuarios mediante el formato de comunicación que prefieras (por ejemplo, correo electrónico, mensaje SMS o tarjetas impresas). Te recomendamos que incluyas lo siguiente en la comunicación:
    -   Una explicación de para qué aplicación o IAP es el código promocional y, de forma opcional, una descripción de por qué el usuario recibe el código.
    -   La dirección URL canjeable para el código.
    -   Instrucciones que guíen al usuario para que visite la dirección URL canjeable, inicie sesión con su cuenta Microsoft y siga las instrucciones para descargar e instalar la aplicación.

## Experiencia de usuario del canje de códigos


Ya has distribuido a un usuario una dirección URL canjeable; los siguientes pasos describen la experiencia de canjear el código por la aplicación.

1.  El usuario hace clic en la dirección URL canjeable.

    El explorador se abre en una página autenticada de **Canjea tu código** en <https://account.microsoft.com/billing/redeem>. Esta página incluye una descripción de la aplicación que el usuario va a canjear.

2.  El usuario hace clic en **Canjear.**

    El explorador abre una página de **agradecimiento** con un vínculo denominado **Obtén*****&lt;nombre de la aplicación&gt;***.

    > **Nota**  Los usuarios recibirán un error en este paso si la aplicación no está publicada aún.

3.  El usuario debe hacer clic en **Obtén*****&lt;nombre de la aplicación&gt;***.

4.  Si el usuario está usando un equipo que tiene instalada la Tienda Windows para Windows 10 o para Windows 8.1, se abrirá la Tienda Windows en la página de información general de la aplicación. El usuario puede hacer clic en **Instalar** para instalar la aplicación de forma gratuita.

    Si, por el contrario, el usuario está usando un equipo o dispositivo que no tiene la Tienda de Windows instalada, el explorador se abre en la página web de la Tienda Windows de la aplicación. El usuario puede hacer clic en **Instalar** para instalar la aplicación de forma gratuita.

    > **Nota**  En algunos casos la página de la aplicación podría mostrar un botón **Comprar** en lugar de **Instalar**, incluso si la aplicación se canjeó correctamente mediante el código promocional. El usuario puede hacer clic en **Comprar** para instalar la aplicación de forma gratuita.

## Revisar los códigos promocionales


Hay varias formas de revisar el uso de los códigos promocionales.

-   Para ver un resumen de los pedidos de códigos promocionales de todas tus aplicaciones, visita la página **Información general del panel** y busca la sección **Códigos promocionales**. Esta sección muestra los códigos promocionales activos restantes para todas las aplicaciones, el número total de códigos promocionales canjeados para todas las aplicaciones y el número total de pedidos de códigos promocionales que has realizado para todas las aplicaciones.
-   Para ver un resumen de los pedidos de códigos promocionales de una aplicación específica, ve a la página **Información general de la aplicación** y localiza la sección **Códigos promocionales**. Esta sección muestra los códigos promocionales activos restantes para la aplicación, el número total de códigos promocionales canjeados para la aplicación y el número total de pedidos de códigos promocionales que has realizado para la aplicación.
-   Para ver un resumen detallado de los pedidos de códigos promocionales de una aplicación específica, ve a la página **Códigos promocionales** de la aplicación (para ello, expande **Monetización** y haz clic en **Códigos promocionales**). Puedes revisar la siguiente información sobre todos los códigos promocionales actuales e inactivos de la aplicación:
    -   Nombre del pedido
    -   Nombre de la aplicación o IAP
    -   Fecha del pedido
    -   Fecha de expiración
    -   Estado

También puedes descargar un pedido activo desde esta tabla.

 

 







<!--HONumber=Jun16_HO3-->


