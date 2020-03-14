---
Description: Puedes establecer la fecha precisa y la hora en que la aplicación debe estar disponible en la Tienda, para ofrecerte más flexibilidad y la capacidad de personalizar las fechas para los diferentes mercados.
title: Configurar la programación precisa del lanzamiento
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, programación, fecha de lanzamiento, fechas, inicio
ms.localizationpriority: medium
ms.openlocfilehash: eebd98d8e1ce39ef8d9876ab4749bcc76012f9fa
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210391"
---
# <a name="configure-precise-release-scheduling"></a>Configurar la programación precisa del lanzamiento

La sección **Programación** en la página [Precios y disponibilidad](set-app-pricing-and-availability.md) te permite establecer la fecha y hora precisas a la que la aplicación debe estar disponible en la Store, ofreciéndote más flexibilidad y la capacidad de personalizar fechas para los diferentes mercados.

> [!NOTE]
> Aunque este tema hace referencia a las aplicaciones, la programación de lanzamiento de envíos de complementos utiliza el mismo proceso.

Además, puedes establecer una fecha en la que el producto dejará de estar disponible en la Tienda. Recuerda que esto significa que el producto ya no podrá encontrarse en la Store mediante búsqueda o exploración, pero los clientes con un vínculo directo podrán ver la descripción del producto en la Store. Solo pueden descargarlo si ya tienen el producto o si tienen un [código promocional](generate-promotional-codes.md) y estás usando un dispositivo Windows 10.

De manera predeterminada (a menos que hayas seleccionado uno de la opciones **Make this app available but not discoverable in the Store** en la sección [Visibilidad](choose-visibility-options.md#discoverability), la aplicación estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir la sección.

Ten en cuenta que no podrás configurar las fechas en la sección **Programación** si has seleccionado una de las opciones **Make this app available but not discoverable in the Store**, en la sección [Visibilidad](choose-visibility-options.md#discoverability), porque la aplicación no se lanzará a los clientes, así que no hay ninguna fecha de lanzamiento que configurar.

> [!IMPORTANT]
> Las fechas que especifiques en la sección Programación solo se aplican a los clientes de Windows 10.
>
>Si la aplicación publicada anteriormente es compatible con versiones anteriores del sistema operativo, cualquier fecha de detención de la **adquisición** que seleccione no se aplicará a los clientes. todavía podrán adquirir la aplicación (a menos que envíe una actualización con una nueva selección en la sección de [visibilidad](choose-visibility-options.md#discoverability) o si selecciona **crear aplicación no disponible** en la página de **información general** de la aplicación).

## <a name="base-schedule"></a>Programación de base

Las opciones seleccionadas para la programación de base se aplicarán a todos los mercados en los que la aplicación esté disponible, a menos que agregues fechas posteriores para mercados específicos (o grupos de mercado) seleccionando [Personalización para mercados específicos](#customize-the-schedule-for-specific-markets).

Verás dos opciones aquí: **Lanzamiento** y **Detener la compra**. 

## <a name="release"></a>Publicación

En la lista desplegable de **Lanzamiento**, puedes establecer cuando quieres que tu aplicación esté disponible en la Store. Esto significa que la aplicación es reconocible en la Store mediante búsqueda o exploración, y que los clientes pueden ver su descripción en la Store y comprar la aplicación.

>[!NOTE]
> Una vez que la aplicación se ha publicado y está disponible en la Store, ya no podrás seleccionar una fecha de **lanzamiento** (ya que la aplicación se habrá publicado).

Estas son las opciones que se puede configurar para la programación del **lanzamiento** de un producto:
- **Tan pronto como sea posible**: El producto se publicará tan pronto como se certifique y se publique. Esta es la opción predeterminada.
- **En**: El producto se publicará en la fecha y hora que selecciones. Tienes dos opciones adicionales:
   - **UTC**: El tiempo que selecciones será el Horario universal coordinado (UTC), para que la aplicación se lance al mismo tiempo en todos los lugares.
   - **Local**: El tiempo que selecciones se usará en cada zona horaria asociada con un mercado. (Ten en cuenta para los mercados que tengan más de una zona horaria, se usará solo una zona horaria para ese mercado. En el Estados Unidos, se usa la zona horaria oriental. En esta página se muestra una lista completa de zonas horarias.)
- **No programada**: La aplicación no estará disponible en la Store. Si eliges esta opción, puede realizar la aplicación disponible en la Store creando más tarde un nuevo envío y eligiendo una de las otras opciones.

## <a name="stop-acquisition"></a>Detener la compra

En el menú desplegable **Detener la compra**, puedes establecer una fecha y hora en la que quieras establecer que los clientes nuevos ya no compren la aplicación o descubran su descripción. Esto puede ser útil si desea controlar con precisión cuando una aplicación ya no se ofrecerán a los clientes nuevos, como cuando se coordinar disponibilidad entre más de uno de tus aplicaciones.

De manera predeterminada, **Detener la compra** siempre está establecida en "Nunca". Para cambiar esta opción, selecciona **En** en la lista desplegable y especifica una fecha y hora, como se ha descrito anteriormente. En la fecha y hora que selecciones, los clientes ya no podrán comprar la aplicación.

Es importante comprender que esta opción tiene el mismo impacto que seleccionar **hacer que esta aplicación se pueda detectar pero no estar disponible** en la sección de [visibilidad](choose-visibility-options.md#discoverability) y elegir **detener la adquisición: cualquier cliente con un vínculo directo puede ver la lista de la tienda del producto, pero solo puede descargarla si poseía el producto antes o tiene un código promocional y usa un dispositivo de Windows 10.** Para dejar de ofrecer completamente una aplicación a los clientes nuevos, haz clic en **Make app unavailable** desde la página Información general de la aplicación. Para obtener más información, consulta [Quitar una aplicación de la Tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Si seleccionas una fecha para **detener la compra** y, más adelante decides que te gustaría que la aplicación estuviera disponible de nuevo, puedes crear un nuevo envío y cambiar la opción **impedir la compra** a **Nunca**. La aplicación volverá a estar disponible una vez que tu envío actualizado se publique.

## <a name="customize-the-schedule-for-specific-markets"></a>Personalizar la programación para mercados concretos 

De manera predeterminada, las opciones que selecciones anteriormente, se aplicarán a todos los mercados en los que se ofrece la aplicación. Para personalizar los precios para mercados específicos, haz clic en **Personalización para mercados específicos**. La ventana emergente de **Selección de mercado** aparecerá en la lista de todos los mercados en los que hayas elegido que la aplicación estará disponible. Si has excluido algún mercado en la sección [Mercados](define-pricing-and-market-selection.md), no se mostrarán aquí. 

Para agregar un programa para un mercado, selecciónalo y haz clic en **Guardar**. A continuación, verás las mismas opciones **Lanzamiento** y **Detener la compra** descritas anteriormente, pero las selecciones que realice solo se aplicarán a ese mercado.

Para agregar una programación que se aplique a varios mercados, se creará un *grupo de mercado*. Para ello, selecciona los mercados que quieras incluir y escribe un nombre del grupo. (Este nombre solo es para tu propia referencia y no será visible para los clientes). Por ejemplo, si quieres crear un grupo de mercado para Norteamérica, puedes seleccionar **Canadá**, **México**, y **Estados Unidos**y asígnale el nombre **Norteamérica** u otro nombre que elijas. Cuando hayas terminado de crear el grupo de mercado, haz clic en **Guardar**. A continuación, verás las mismas opciones **Lanzamiento** y **Detener la compra** descritas anteriormente, pero las selecciones que realice solo se aplicarán a ese grupo de mercado.

Para agregar una programación personalizada para un mercado adicional, o un grupo de mercados adicionales, haz clic en **Personalización para mercados específicos** de nuevo y repite estos pasos. Para cambiar los mercados que se incluyen en un grupo de mercado, selecciona su nombre. Para quitar la programación personalizada para un grupo de mercado (o mercado individual), haz clic en **Quitar**.

> [!NOTE]
> Un mercado no puede pertenecer a más de un grupo de mercado que usas en la sección **Programación**. 

## <a name="global-time-zones"></a>Zonas horarias globales

A continuación se muestra una tabla en la que se muestran las zonas horarias específicas que se usan en cada mercado, por lo que cuando el envío utiliza la hora local (por ejemplo, el lanzamiento a las 9:00 de la versión local), puede averiguar el tiempo que se publicará en cada mercado, en especial útil con los mercados que tienen más de una hora z. una, como Canadá.

| Mercado | Zona horaria |
|--------|-----------|
| Afganistán  |  (UTC + 04:30) Kabul |
| Albania  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Argelia  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Samoa Americana  |  (UTC + 13:00) Samoa |
| Andorra  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Angola  |  (UTC + 01:00) África central occidental |
| Anguila  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Antártida  |  (UTC + 12:00) Auckland, Wellington |
| Antigua y Barbuda  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Argentina  |  (UTC-03:00) Ciudad de Buenos Aires |
| Armenia  |  (UTC + 04:00) Abu Dabi, Muscat |
| Aruba  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Australia  |  (UTC + 10:00) Canberra, Melbourne, Sydney |
| Austria  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Azerbaiyán  |  (UTC + 04:00) Bakú |
| Bahamas  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Baréin  |  (UTC + 04:00) Abu Dabi, Muscat |
| Bangladés  |  (UTC + 06:00) Dacca |
| Barbados  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Belarús  |  (UTC + 03:00) Minsk |
| Bélgica  |  (UTC + 01:00) Bruselas, Copenhague, Madrid, París |
| Belice  |  (UTC-06:00) Hora central (EE. UU. & Canadá) |
| Benín  |  (UTC + 01:00) África central occidental |
| Bermudas  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Bután  |  (UTC + 06:00) Dacca |
| República Bolivariana de Venezuela  |  (UTC-04:00) Caracas |
| Bolivia  |  (UTC-04:00) Georgetown, la paz, Manaos, San Juan |
| Bonaire, San Eustaquio y Saba  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Bosnia y Herzegovina  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Botsuana  |  (UTC + 01:00) África central occidental |
| Isla Bouvet  |  (UTC + 00:00) Monrovia, Reykjavik |
| Brasil  |  (UTC-03:00) Brasilia |
| Territorio Británico del Océano Índico  |  (UTC + 06:00) Dacca |
| Islas Vírgenes Británicas  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Brunéi  |  (UTC + 08:00) Irkutsk |
| Bulgaria  |  (UTC + 02:00) Chisinau |
| Burkina Faso  |  (UTC + 00:00) Monrovia, Reykjavik |
| Burundi  |  (UTC + 02:00) Harare, Pretoria |
| CÃ ́te d'Ivoire  |  (UTC + 00:00) Monrovia, Reykjavik |
| Camboya  |  (UTC + 07:00) Bangkok, Hanoi, Yakarta |
| Camerún  |  (UTC + 01:00) África central occidental |
| Canadá  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Cabo Verde  |  (UTC-01:00) Islas de cabo verde |
| Islas Caimán  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| República Centroafricana  |  (UTC + 01:00) África central occidental |
| Chad  |  (UTC + 01:00) África central occidental |
| Chile  |  (UTC-04:00) Santiago |
| China  |  (UTC + 08:00) Pekín, Chongqing, Hong Kong, Urumqi |
| Isla de Navidad  |  (UTC + 07:00) Krasnoiarsk |
| Islas Cocos  |  (UTC + 06:30) Yangón (Rangún) |
| Colombia  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Comoras  |  (UTC + 03:00) Nairobi |
| Congo  |  (UTC + 01:00) África central occidental |
| Congo (RDC)  |  (UTC + 01:00) África central occidental |
| Islas Cook  |  (UTC-10:00) Hawái |
| Costa Rica  |  (UTC-06:00) Hora central (EE. UU. & Canadá) |
| Croacia  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| CuraÃ § AO  |  (UTC-04:00) Cuiabá |
| Chipre  |  (UTC + 02:00) Chisinau |
| República Checa  |  (UTC + 01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Dinamarca  |  (UTC + 01:00) Bruselas, Copenhague, Madrid, París |
| Yibuti  |  (UTC + 03:00) Nairobi |
| Dominica  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| República Dominicana  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Ecuador  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Egipto  |  (UTC + 02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Hora central (EE. UU. & Canadá) |
| Guinea Ecuatorial  |  (UTC + 01:00) África central occidental |
| Eritrea  |  (UTC + 03:00) Nairobi |
| Estonia  |  (UTC + 02:00) Chisinau |
| Etiopía  |  (UTC + 03:00) Nairobi |
| Islas Malvinas  |  (UTC-04:00) Santiago |
| Islas Feroe  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| Fiyi  |  (UTC + 12:00) Fiyi |
| Finlandia  |  (UTC + 02:00) Helsinki, Kiev, Riga, Sofía, Tallin, Vilnius |
| Francia  |  (UTC + 01:00) Bruselas, Copenhague, Madrid, París |
| Guayana Francesa  |  (UTC-03:00) Cayena, fortaleza |
| Polinesia Francesa  |  (UTC-10:00) Hawái |
| Tierras Australes y Antárticas Francesas  |  (UTC + 05:00) Asjabad, Tashkent |
| Gabón  |  (UTC + 01:00) África central occidental |
| Gambia  |  (UTC + 00:00) Monrovia, Reykjavik |
| Georgia  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Alemania  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Ghana  |  (UTC + 00:00) Monrovia, Reykjavik |
| Gibraltar  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Grecia  |  (UTC + 02:00) Atenas, Bucarest |
| Groenlandia  |  (UTC + 00:00) Monrovia, Reykjavik |
| Granada  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Guadalupe  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Guam  |  (UTC + 10:00) Guam, Puerto Moresby |
| Guatemala  |  (UTC-06:00) Hora central (EE. UU. & Canadá) |
| Guernsey  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guinea  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guinea-Bisáu  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guyana  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Haití  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Islas Heard y McDonald  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Santa Sede (Ciudad del Vaticano)  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Honduras  |  (UTC-06:00) Hora central (EE. UU. & Canadá) |
| Región Administrativa Especial de Hong Kong  |  (UTC + 08:00) Pekín, Chongqing, Hong Kong, Urumqi |
| Hungría  |  (UTC + 01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Islandia  |  (UTC + 00:00) Monrovia, Reykjavik |
| India  |  (UTC + 05:30) Chennai, Calcuta, Mumbai, Nueva Delhi |
| Indonesia  |  (UTC + 07:00) Bangkok, Hanoi, Yakarta |
| Irak  |  (UTC + 04:00) Abu Dabi, Muscat |
| Irlanda  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| Israel  |  (UTC + 02:00) Jerusalén |
| Italia  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Jamaica  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Japón  |  (UTC + 09:00) Osaka, Sapporo, Tokio |
| Jersey  |  (UTC + 00:00) Monrovia, Reykjavik |
| Jordania  |  (UTC + 02:00) Chisinau |
| Kazajistán  |  (UTC + 05:00) Asjabad, Tashkent |
| Kenia  |  (UTC + 03:00) Nairobi |
| Kiribati  |  (UTC + 14:00) Isla Kiritimati |
| Corea del Sur  |  (UTC + 09:00) Seúl |
| Kuwait  |  (UTC + 04:00) Abu Dabi, Muscat |
| Kirguistán  |  (UTC + 06:00) Astaná |
| Laos  |  (UTC + 07:00) Bangkok, Hanoi, Yakarta |
| Letonia  |  (UTC + 02:00) Chisinau |
| Líbano  |  (UTC + 02:00) Chisinau |
| Lesoto  |  (UTC + 02:00) Harare, Pretoria |
| Liberia  |  (UTC + 00:00) Monrovia, Reykjavik |
| Libia  |  (UTC + 02:00) Chisinau |
| Liechtenstein  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Lituania  |  (UTC + 02:00) Chisinau |
| Luxemburgo  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| RAE de Macao  |  (UTC + 08:00) Pekín, Chongqing, Hong Kong, Urumqi |
| Macedonia (Ex-República Yugoslava de Macedonia)  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Madagascar  |  (UTC + 03:00) Nairobi |
| Malawi  |  (UTC + 02:00) Harare, Pretoria |
| Malasia  |  (UTC + 08:00) Kuala Lumpur, Singapur |
| Maldivas  |  (UTC + 05:00) Asjabad, Tashkent |
| Malí  |  (UTC + 00:00) Monrovia, Reykjavik |
| Malta  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Hombre, isla de  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| Islas Marshall  |  (UTC + 12:00) Petropávlovsk-Kamchatski-Old |
| Martinica  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Mauritania  |  (UTC + 00:00) Monrovia, Reykjavik |
| Mauricio  |  (UTC + 04:00) Puerto Louis |
| Mayotte  |  (UTC + 03:00) Nairobi |
| México  |  (UTC-06:00) Guadalajara, ciudad de México, Monterrey |
| Micronesia  |  (UTC + 10:00) Guam, Puerto Moresby |
| Moldova  |  (UTC + 02:00) Chisinau |
| Mónaco  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Mongolia  |  (UTC + 07:00) Krasnoiarsk |
| Montenegro  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Montserrat  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Marruecos  |  (UTC + 01:00) Casablanca |
| Mozambique  |  (UTC + 02:00) Harare, Pretoria |
| Myanmar  |  (UTC + 06:30) Yangón (Rangún) |
| Namibia  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Nauru  |  (UTC + 12:00) Petropávlovsk-Kamchatski-Old |
| Nepal  |  (UTC + 05:45) Katmandú |
| Países Bajos  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Nueva Caledonia  |  (UTC + 11:00) Islas Salomón, Nueva Caledonia |
| Nueva Zelanda  |  (UTC + 12:00) Auckland, Wellington |
| Nicaragua  |  (UTC-06:00) Hora central (EE. UU. & Canadá) |
| Níger  |  (UTC + 01:00) África central occidental |
| Nigeria  |  (UTC + 01:00) África central occidental |
| Niue  |  (UTC + 13:00) Samoa |
| Isla Norfolk  |  (UTC + 11:00) Islas Salomón, Nueva Caledonia |
| Islas Marianas del Norte  |  (UTC + 10:00) Guam, Puerto Moresby |
| Noruega  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Omán  |  (UTC + 04:00) Abu Dabi, Muscat |
| Pakistán  |  (UTC + 05:00) Islamabad, Karachi |
| Palaos  |  (UTC + 09:00) Osaka, Sapporo, Tokio |
| Autoridad Palestina  |  (UTC + 02:00) Chisinau |
| Panamá  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Papúa Nueva Guinea  |  (UTC + 10:00) Vladivostok |
| Paraguay  |  (UTC-04:00) Asunción |
| Perú  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Filipinas  |  (UTC + 08:00) Kuala Lumpur, Singapur |
| Islas Pitcairn  |  (UTC-08:00) Hora del Pacífico (EE. UU. & Canadá) |
| Polonia  |  (UTC + 01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Portugal  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| Catar  |  (UTC + 04:00) Abu Dabi, Muscat |
| Reunión  |  (UTC + 04:00) Puerto Louis |
| Rumanía  |  (UTC + 02:00) Chisinau |
| COLUMNA  |  (UTC-07:00) Hora de las montañas rocosas (EE. UU. & Canadá) |
| Rusia  |  (UTC + 03:00) Moscú, San Petersburgo |
| Ruanda  |  (UTC + 02:00) Harare, Pretoria |
| SÃ £ o TomÃ © y PrÃncipe  |  (UTC + 00:00) Monrovia, Reykjavik |
| San BarthÃ © LEMY  |  (UTC + 04:00) Ereván |
| Santa Elena, Ascensión y Tristán de Acuña  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| San Cristóbal y Nieves  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Santa Lucía  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| San Martín (zona francesa)  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| San Pedro y Miquelón  |  (UTC-02:00) Atlántico central-antiguo |
| San Vicente y las Granadinas  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Samoa  |  (UTC + 13:00) Samoa |
| San Marino  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Arabia Saudí  |  (UTC + 03:00) Kuwait, Riad |
| Senegal  |  (UTC + 00:00) Monrovia, Reykjavik |
| Serbia  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Seychelles  |  (UTC + 04:00) Abu Dabi, Muscat |
| Sierra Leona  |  (UTC + 00:00) Monrovia, Reykjavik |
| Singapur  |  (UTC + 08:00) Kuala Lumpur, Singapur |
| Sint Maarten (zona neerlandesa)  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Eslovaquia  |  (UTC + 01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Eslovenia  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Islas Salomón  |  (UTC + 11:00) Islas Salomón, Nueva Caledonia |
| Somalia  |  (UTC + 03:00) Nairobi |
| Sudáfrica  |  (UTC + 02:00) Harare, Pretoria |
| Georgia del Sur e Islas Sandwich del Sur  |  (UTC-02:00) Atlántico central-antiguo |
| España  |  (UTC + 01:00) Bruselas, Copenhague, Madrid, París |
| Sri Lanka  |  (UTC + 05:30) Chennai, Calcuta, Mumbai, Nueva Delhi |
| Surinam  |  (UTC-03:00) Cayena, fortaleza |
| Svalbard y Jan Mayen  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Suazilandia  |  (UTC + 02:00) Harare, Pretoria |
| Suecia  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Suiza  |  (UTC + 01:00) Amsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Taiwán  |  (UTC + 08:00) Taipei |
| Tayikistán  |  (UTC + 05:00) Asjabad, Tashkent |
| Tanzania  |  (UTC + 03:00) Nairobi |
| Tailandia  |  (UTC + 07:00) Bangkok, Hanoi, Yakarta |
| Timor-Leste  |  (UTC + 09:00) Seúl |
| Togo  |  (UTC + 00:00) Monrovia, Reykjavik |
| Tokelau  |  (UTC + 13:00) Nuku'alofa |
| Tonga  |  (UTC + 13:00) Nuku'alofa |
| Trinidad y Tobago  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Túnez  |  (UTC + 01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Turquía  |  (UTC + 03:00) Estambul |
| Turkmenistán  |  (UTC + 05:00) Asjabad, Tashkent |
| Islas Turcas y Caicos  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Tuvalu  |  (UTC + 12:00) Petropávlovsk-Kamchatski-Old |
| Islas menores alejadas de los EE. UU.  |  (UTC + 13:00) Samoa |
| Islas Vírgenes de los Estados Unidos de América  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Uganda  |  (UTC + 03:00) Nairobi |
| Ucrania  |  (UTC + 02:00) Chisinau |
| Emiratos Árabes Unidos  |  (UTC + 04:00) Abu Dabi, Muscat |
| Reino Unido  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| Estados Unidos  |  (UTC-05:00) Hora del este (EE. UU. & Canadá) |
| Uruguay  |  (UTC-03:00) Brasilia |
| Uzbekistán  |  (UTC + 05:00) Asjabad, Tashkent |
| Vanuatu  |  (UTC + 11:00) Islas Salomón, Nueva Caledonia |
| Vietnam  |  (UTC + 07:00) Bangkok, Hanoi, Yakarta |
| Wallis y Futuna  |  (UTC + 12:00) Petropávlovsk-Kamchatski-Old |
| Sáhara Occidental (en conflicto)  |  (UTC + 00:00) Dublín, Edimburgo, Lisboa, Londres |
| Yemen  |  (UTC + 04:00) Abu Dabi, Muscat |
| Zambia  |  (UTC + 02:00) Harare, Pretoria |
| Zimbabue  |  (UTC + 02:00) Harare, Pretoria |
