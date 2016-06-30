---
author: mcleanbyron
ms.assetid: 69E05E56-B5F0-4D4C-A1FF-B6EAFF5D0E28
description: "Durante el proceso de envío, puedes configurar el comportamiento de la mediación de anuncios que desees ver. Podrás ajustar esto más adelante sin necesidad de realizar cambios en el código ni enviar paquetes nuevos."
title: "Enviar la aplicación y configurar la mediación de anuncios"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 13dd6a9c38d85ead29a43f470b7c0f63d025d612

---

# Enviar la aplicación y configurar la mediación de anuncios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Después de crear tu aplicación para que incluya todas las redes de anuncios que deseas usar y probarla para asegurarte de que todo funcione, estás listo para enviar la aplicación. Durante el proceso de envío, puedes configurar el comportamiento de la mediación de anuncios que desees ver. Podrás ajustar esto más adelante sin necesidad de realizar cambios en el código ni enviar paquetes nuevos.

## Crear una configuración de línea base para un paquete de la aplicación


Cuando cargas los paquetes, el Centro de desarrollo detecta automáticamente que usas la mediación de anuncios e identifica las redes de anuncios que usas. En la página **Paquetes**, verás un vínculo **Configurar la mediación de anuncios**. Haz clic en este vínculo para ir a la página [Rentabilizar con anuncios](https://msdn.microsoft.com/library/windows/apps/mt170658), donde tendrás que configurar la lógica de mediación en la sección **Mediación de anuncios de Windows**. Para más información acerca de las demás secciones de esta página, consulta Rentabilizar con anuncios.

La primera vez que configuras la configuración de mediación de anuncios para tu aplicación, se crea una configuración de línea base. Posteriormente, podrás agregar configuraciones específicas del mercado para aprovechar las ventajas de determinadas redes de anuncios en diferentes mercados. Si decides no configurar la configuración de mediación de anuncios antes de enviar la aplicación, el Centro de desarrollo aplica automáticamente la [configuración predeterminada](#default-ad-mediation-settings) que puedes [modificar más adelante](#optimize-ad-mediation-after-the-app-has-been-published).

En los siguientes pasos se describe cómo crear una configuración de línea base en la sección **Mediación de anuncios de Windows** de la página **Rentabilizar con anuncios**.

1.  En **Configurar mediación para**, asegúrate de que el paquete de la aplicación que deseas configurar está seleccionado.
2.  En **Destino**, asegúrate de que **Línea base** está seleccionado.
3.  En **Frecuencia de actualización**, establece la frecuencia de actualización para determinar la duración del ciclo de mediación (frecuencia de presentación de los anuncios). La duración debe ser entre 30 y 120 segundos.
  > **Nota** Si ya configuraste una frecuencia de actualización en cualquiera de los portales de redes de anuncios, procura establecer la misma frecuencia de actualización aquí.

4.  A continuación, en la sección **Mediación de anuncios de Windows** se enumeran todas las redes de anuncios usadas por la aplicación y se proporcionan dos maneras diferentes de especificar la frecuencia en que la aplicación debe usar cada red de anuncios. Elige una de estas opciones en la lista desplegable **Tipo de mediación**:

    -   **Orden por peso**. Elige esta opción para aplicar valores de porcentaje a cada red de anuncios que especifiquen la frecuencia con que la aplicación los debe usar. La suma total de los porcentajes que definas para todas las redes de anuncios debe ser del 100 % exactamente. Para obtener más información, consulta [Ordenar redes de anuncios por peso](#order-ad-networks-by-weight).
    -   **Orden por clasificación**. Elige esta opción para aplicar un número de clasificación en cada red de anuncios, de 1 a *n*, que especifique la frecuencia con que la aplicación debe usar cada red de anuncios. Para obtener más información, consulta [Ordenar redes de anuncios por clasificación](#order-ad-networkds-by-rank).

    El Centro de desarrollo aplica automáticamente la [configuración predeterminada](#default-ad-mediation-settings) que especifica la frecuencia con que la aplicación debe usar cada red de anuncios.

5.  En la lista de redes de anuncios que usa la aplicación, haz clic en la flecha desplegable de cada proveedor de red de anuncios para ver los parámetros necesarios para cada red y especifica los parámetros necesarios. Para obtener una lista de los parámetros, consulta [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md).

    En la lista ampliada de parámetros de cada red, puedes usar el campo **Tiempo de espera** para especificar el número de segundos (de 2 a 120) que la mediación de anuncios debe esperar después de solicitar un anuncio de esa red de anuncios, y antes de abandonar la solicitud y realizar una solicitud a otra red en su lugar. Si ya has [especificado este valor en el código](add-and-use-the-ad-mediator-control.md#set-timeouts), el valor especificado en el código reemplaza el valor especificado aquí.

    Si usas Microsoft Advertising, toma nota de lo siguiente:

    -   Los parámetros de **Microsoft Advertising** se rellenan automáticamente en función del contenido del paquete de la aplicación. Es opcional especificar los valores propios **Id. de la aplicación** e **Id. de unidad de anuncio** de **Microsoft Advertising**.
    -   Además de **Microsoft Advertising**, también hay una fila de **Microsoft Advertising house ads**. Los anuncios internos proporcionan una manera de crear anuncios gratuitos que promocionan una de las aplicaciones en las demás aplicaciones. Aunque los anuncios internos son gratuitos, la cobertura de anuncios internos proviene del mismo grupo que otras redes de anuncios. Por lo tanto, si asignas solicitudes de anuncios a **Anuncios internos de Microsoft Advertising**, estas optando por usar ese porcentaje del suministro de anuncios total para anuncios internos. Si asignas solicitudes de anuncio a anuncios internos, también debes crear una campaña de anuncios internos para al menos una otra aplicación registrada en tu cuenta de desarrollador. Para obtener más información, consulta [Acerca de los anuncios internos](https://msdn.microsoft.com/library/windows/apps/mt148566).

6.  Haz clic en **Guardar** para guardar la configuración de línea base.

### Ordenar redes de anuncios por peso

Elige esta opción en la lista desplegable **Mediation type** para especificar los valores de porcentaje que especifican la frecuencia con que la aplicación debe usar cada red de anuncios. La suma total de los porcentajes que definas para todas las redes de anuncios debe ser del 100 % exactamente.

Para repartir automáticamente las solicitudes de forma equitativa en todas las redes, haz clic en **Distribuir las solicitudes de anuncios de forma equitativa a través de todas las redes de anuncios activas**. Para repartir solicitudes de forma desigual, usa los controles deslizantes para indicar la frecuencia con la que la aplicación debe usar cada red de anuncios. La suma total de los porcentajes que definas para todas las redes de anuncios deben ser del 100 % exactamente. Al arrastrar los controles deslizantes, tienes varias opciones:

-   Si arrastras el control deslizante a un número, el número indica el porcentaje de tiempo que se llamará a esta red de anuncios como primera opción de la aplicación en un ciclo de mediación.
-   Si arrastras el control deslizante a **Copia de seguridad**, esto indica que debe llamarse a esta red de anuncios solo si ninguna de las redes de anuncios que tienen un porcentaje designado puede proporcionar un anuncio. Esto equivale a establecer el porcentaje en 0 %.
-   Si arrastras el control deslizante a **Inactiva**, esto indica que nunca se llamará a esta red de anuncios. Los ensamblados de la red de anuncios permanecerán en el paquete, pero el mediador no intentará invocarlos. Esta opción se puede establecer en configuraciones específicas del mercado para excluir redes de anuncios que se sabe que no funcionan correctamente o no admiten un determinado mercado.
-   Al ajustar el porcentaje de una red de anuncios, cualquier control deslizante con el que hayas seleccionado un valor distinto de **Copia de seguridad** se ajustará automáticamente para que la distribución total agregue hasta 100 %. Si seleccionas la casilla **Bloquear** para una red de anuncios, esa red de anuncios permanecerá fija en el valor seleccionado actualmente. Después puedes ajustar los valores de otras redes de anuncios sin afectar al valor de la red de anuncios bloqueada

### Ordenar redes de anuncios por clasificación

Elige esta opción en la lista desplegable **Tipo de mediación** para aplicar un número de clasificación a cada red de anuncios, de 1 a *n*, que especifique la frecuencia con que la aplicación debe usar cada red de anuncios.

Si desactivas la casilla **Activo** de una red de anuncios, nunca se llamará a esta red de anuncios. Los ensamblados de la red de anuncios permanecerán en el paquete, pero el mediador no intentará invocarlos. Esta opción se puede establecer en configuraciones específicas del mercado para excluir redes de anuncios que se sabe que no funcionan correctamente o no admiten un determinado mercado.

### Configuración de mediación de anuncios predeterminada

De manera predeterminada, el Centro de desarrollo aplica la siguiente configuración de mediación de anuncios a tu aplicación en la página **Rentabilizar con anuncios**. También se aplican los mismos valores predeterminados si decides no configurar la mediación de anuncios antes de enviar tu aplicación.

-   Si la aplicación usa Microsoft Advertising, **Microsoft Advertising** se establece en 100 (si eliges **Orden por peso**) o en 1 (si eliges **Orden por clasificación**) y todas las otras redes de anuncios se establecerán como inactivas.
-   Si la aplicación no usa Microsoft Advertising, todas las redes de anuncios se establecerán como inactivas.

## Crear una nueva configuración de destino


Después de crear una configuración de línea base de la aplicación, puedes crear configuraciones adicionales para usar en mercados específicos. Estas nuevas configuraciones heredan sus valores iniciales de la configuración de línea base, pero se pueden ajustar detalles para los mercados específicos que se admiten para la aplicación.

Para crear una nueva configuración de destino:

1.  En **Destino**, selecciona el mercado para el que deseas crear la nueva configuración.
2.  Actualiza la frecuencia de actualización y la información de distribución de solicitud de anuncios como desees.
3.  Haz clic en **Guardar** para guardar la configuración nueva.

## Optimizar la mediación de anuncios tras haber publicado la aplicación


Si quieres ajustar la mediación de anuncios para una aplicación específica, puedes hacerlo en cualquier momento sin necesidad de volver a enviar la aplicación. Esto resulta útil si ya agregaste redes de anuncios en tu aplicación para la que no habías configurado cuentas anteriormente o si notas que una red de anuncios no es capaz de rellenar anuncios de forma eficaz en mercados específicos. Puedes realizar cambios en la configuración de línea base, así como para configuraciones específicas del mercado. También puedes agregar o quitar configuraciones específicas del mercado, si lo deseas.

Para volver a la configuración de mediación de la aplicación, realiza una de las siguientes acciones:

-   En el panel de la página de información general de la cuenta, haz clic en la aplicación en la sección **Mediación de anuncios**.
-   En el panel de la página de información general de la cuenta, expande **Monetización** y haz clic en **Rentabilizar con anuncios**.

## Configuración de mediación de anuncios y actualizaciones de la aplicación


Cuando envías una actualización de aplicación, se aplica automáticamente la información de configuración de mediación de anuncios existente de la aplicación al paquete actualizado si se cumplen las condiciones siguientes:

-   El número de controles **AdMediatorControl** es el mismo que el número de controles en el paquete de la aplicación anterior y todos los controles tienen los mismos identificadores que el paquete de la aplicación anterior.
-   La lista de proveedores de redes de anuncios es la misma que la del paquete de la aplicación anterior.

Si no se cumple alguna de estas condiciones, deberás volver a crear la configuración de línea base y cualquier otra configuración objetivo específica del mercado de la aplicación.

> **Nota** El Id. de un objeto **AdMediatorControl** se genera cuando arrastras el control a una superficie de diseño de la aplicación. Este identificador no cambiará a menos que elimines el control y los reemplace arrastrando un nuevo control a la misma superficie de diseño.

 

## Revisar informes de mediación de anuncios en el panel unificado del Centro de desarrollo


Para revisar informes de mediación de anuncios, ve a la sección **Análisis** del Centro de desarrollo y haz clic en **Mediación de anuncios**. Para obtener más información sobre este informe, consulta [Informe de mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Escenarios de ejemplo


Puedes configurar las redes de anuncios de diversas maneras para admitir distintos escenarios y objetivos. En los siguientes ejemplos se muestra cómo podrías configurar tu mediación de anuncios mediante **Order by weight** después de incluir tres redes de anuncios. Aquí, usaremos Microsoft Advertising, Inneractive y AdDuplex como nuestras redes de ejemplo.

### Ejemplo 1

Quieres averiguar la velocidad de relleno y el costo por mil vistas (eCPM) de todas las redes, antes de empezar a optimizar.

Primero, establece cada red para que use una distribución equitativa del tráfico de mediación. Puedes revisar los informes de cada red de anuncios más tarde para ayudar a determinar las redes de anuncios con mayor rendimiento para cada mercado.

Después de algunos días o algunas semanas, tendrás que comprobar la velocidad de relleno y el eCPM en cada uno de los portales de redes de anuncios. Esto ayudará a determinar las mejores redes de anuncios de cada mercado. Puedes realizar ajustes para mercados específicos (o generales) sin necesidad de enviar nuevos paquetes.

### Ejemplo 2

Quieres usar Microsoft Advertising primero siempre que sea posible. Si Microsoft Advertising no puede proporcionar un anuncio, te gustará usar cualquiera de las otras redes de anuncios como copia de seguridad, sin preferencia de una sobre otra.

| Red de anuncios            | Configuración |
|-----------------------|---------------|
| Microsoft             | 100%          |
| Inneractive           | Copia de seguridad        |
| AdDuplex              | Copia de seguridad        |

### Ejemplo 3

Generalmente quieres usar Microsoft Advertising primero. Si Microsoft Advertising no puede proporcionar un anuncio, quieres asegurarte de que se llame a Inneractive antes que a AdDuplex.

| Red de anuncios            | Configuración |
|-----------------------|---------------|
| Microsoft             | 90 %           |
| Inneractive           | 10 %           |
| AdDuplex              | Copia de seguridad        |

### Ejemplo 4

Quieres usar Microsoft Advertising e Inneractive por igual, de modo que haya una mayor variedad de anuncios en la aplicación de los que verías si siempre se llamara primero a una sola red. Si ninguna red de anuncios está disponible, usarás AdDuplex como copia de seguridad.

| Red de anuncios            | Configuración |
|-----------------------|---------------|
| Microsoft             | 50 %           |
| Inneractive           | 50 %           |
| AdDuplex              | Copia de seguridad        |


## Temas relacionados

* [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md)
* [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md)
* [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)
* [Solucionar problemas de mediación de anuncios](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


