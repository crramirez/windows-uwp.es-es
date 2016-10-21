---
author: JnHs
Description: "Si la aplicación usa los elementos AdMediatorControl o AdControl para mostrar anuncios de banner, puedes aumentar la velocidad de relleno de los anuncios y tus ingresos mostrando anuncios de filiales de Microsoft en la aplicación."
title: Lanzamiento gradual del paquete
translationtype: Human Translation
ms.sourcegitcommit: ac9eed95edba99cdba914ff21b25383f35a20012
ms.openlocfilehash: 3ec642ef0a21d06b20cabb12a47d2abcfd19fe59

---

# Lanzamiento gradual del paquete

Al publicar una actualización para un envío, puedes elegir lanzar gradualmente los paquetes actualizados para un porcentaje de los clientes de la aplicación en Windows 10. Esto te permite supervisar los comentarios y los datos analíticos de los paquetes específicos para asegurarte de que estás seguro sobre la actualización antes de hacer un lanzamiento más amplio. Asimismo, puedes incrementar el porcentaje (o detener la actualización) en cualquier momento, sin tener que crear un nuevo envío. 

> **Importante** Tus selecciones de lanzamiento se aplican a todos los paquetes, pero solo se aplicarán a los clientes que ejecuten versiones de sistema operativo compatibles con los paquetes piloto (Windows.Desktop compilación 10586 o posterior; Windows.Mobile compilación 10586.63 o posterior y Xbox), incluidos los clientes que obtienen la aplicación a través de una [licencia administrada desde la Tienda (en línea)](organizational-licensing.md) mediante la [Tienda Windows para empresas](https://www.microsoft.com/business-store). Al usar el lanzamiento de paquete gradual, los clientes con versiones anteriores del sistema operativo no recibirán paquetes del envío más reciente hasta que finalice el lanzamiento del paquete, como se describe a continuación.

Ten en cuenta que todos los clientes verán los detalles de la descripción de la Tienda que especifiques con el envío más reciente. La configuración de lanzamiento solo se aplica a los paquetes que reciben los clientes, tanto en las adquisiciones nuevas como en las actualizaciones para clientes existentes.

> **Sugerencia** El lanzamiento del paquete distribuye paquetes a una selección aleatoria de clientes en los porcentajes que especifiques. Para distribuir paquetes específicos a los clientes seleccionados que indiques, puedes usar paquetes piloto.  También puedes combinar el lanzamiento con tus paquetes piloto si quieres distribuir una actualización de forma gradual a uno de tus grupos piloto.

## Establecer el porcentaje de lanzamiento

Puedes seleccionar lanzar la actualización desde la página **Paquetes** de un envío actualizado. Para hacerlo, activa la casilla que indica **Los lanzamientos se actualizan gradualmente una vez publicado el envío (solo a clientes de Windows 10)**. A continuación, escribe el porcentaje de clientes que deben recibir la actualización cuando el envío se publique por primera vez. Por ejemplo, puedes escribir 5 si quieres empezar por lanzar la actualización a solo un pequeño porcentaje de clientes de la aplicación.

Haz clic en **Actualizar** para guardar tus selecciones. Cuando la aplicación complete el proceso de certificación, los paquetes se distribuirán a los clientes según el porcentaje que hayas especificado, tanto para adquisiciones nuevas como para actualizaciones para clientes existentes.

## Ajustar el lanzamiento después de publicar el envío

Para ajustar el lanzamiento después de publicar el envío, ve a la página Información general de la aplicación. Puedes arrastrar el selector para cambiar el porcentaje de clientes que reciben los paquetes de tu envío más reciente. Haz clic en **Actualizar** para guardar tus selecciones. Después, los paquetes empezarán a distribuirse a los clientes en función del porcentaje que hayas especificado, tanto para adquisiciones nuevas como para actualizaciones para clientes existentes.

## Completar el lanzamiento

Para poder crear un nuevo envío, debes completar el lanzamiento del paquete. Puedes **finalizar** el lanzamiento y distribuir los paquetes más recientes para todos los clientes, o **detener** el lanzamiento para dejar de distribuir los paquetes más recientes.

Si tienes confianza en la actualización y quieres ponerla a disposición de todos los clientes, haz clic en **Finalizar lanzamiento de paquete** para distribuir los paquetes más recientes a todos los clientes.

> **Sugerencia** Cambiar el porcentaje de lanzamiento al 100% no garantiza que todos los clientes reciban los paquetes de los últimos envíos, ya que puede que algunos clientes usen versiones de sistema operativo incompatibles con el lanzamiento. Debes finalizar el lanzamiento para dejar de distribuir los paquetes anteriores y actualizar a los clientes existentes con los más recientes.

Si observas que hay problemas con la actualización y no quieres distribuirla más, puedes hacer clic en **Detener lanzamiento de paquete** para detener la distribución de paquetes del envío más reciente. Una vez detenido el lanzamiento del paquete, estos paquetes ya no se distribuirán a los clientes; solo los paquetes del envío anterior se usarán para los clientes nuevos o la actualización de clientes. Sin embargo, los clientes que ya tengan los paquetes nuevos, conservarán esos paquetes. Su versión no se revertirá a la anterior. Para proporcionar una actualización para estos clientes, debes crear un nuevo envío con los paquetes que quieres que reciban. Ten en cuenta que, si usas un lanzamiento gradual en el próximo envío, se ofrecerá a los clientes que tenían el paquete que detuviste la nueva actualización en el mismo orden en que se les ofreció el paquete que se detuvo. El nuevo lanzamiento se producirá entre el último envío finalizado y el envío más reciente. Una vez detenido un lanzamiento de paquete, dichos paquetes ya no se podrán distribuir a ningún cliente.



<!--HONumber=Aug16_HO5-->


