---
description: Puede establecer la fecha y la hora precisas en las que la aplicación debe estar disponible en el almacén, lo que le ofrece mayor flexibilidad y la posibilidad de personalizar fechas para diferentes mercados.
title: Configurar la programación precisa del lanzamiento
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, programación, fecha de lanzamiento, fechas, Inicio
ms.localizationpriority: medium
ms.openlocfilehash: bf9fe34eb36cab57677ba8ef22397c9c345ecb40
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035178"
---
# <a name="configure-precise-release-scheduling"></a>Configurar la programación precisa del lanzamiento

La sección **programación** de la página [precios y disponibilidad](set-app-pricing-and-availability.md) le permite establecer la fecha y la hora precisas en las que la aplicación debe estar disponible en la tienda, lo que le ofrece mayor flexibilidad y la capacidad de personalizar fechas para distintos mercados.

> [!NOTE]
> Aunque este tema hace referencia a las aplicaciones, la programación de versiones de los envíos de complementos usa el mismo proceso.

También puede optar por establecer una fecha en la que el producto ya no esté disponible en el almacén. Tenga en cuenta que esto significa que el producto ya no se encuentra en el almacén a través de la búsqueda o la exploración, pero cualquier cliente con un vínculo directo puede ver la lista de la tienda del producto. Solo pueden descargarlo si ya poseen el producto o si tienen un [código promocional](generate-promotional-codes.md) y usan un dispositivo de Windows 10.

De forma predeterminada (a menos que haya seleccionado una de las opciones **hacer que esta aplicación esté disponible pero no sea reconocible en las** opciones del almacén en la sección [visibilidad](choose-visibility-options.md#discoverability) ), la aplicación estará disponible para los clientes en cuanto pase la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir esta sección.

Tenga en cuenta que no podrá configurar fechas en la sección **programación** si ha seleccionado una de las opciones de **hacer que esta aplicación esté disponible pero no se puede detectar en las** opciones del almacén en la sección de [visibilidad](choose-visibility-options.md#discoverability) , ya que la aplicación no se lanzará a los clientes, por lo que no hay ninguna fecha de lanzamiento para configurar.

> [!IMPORTANT]
> Las fechas que especifique en la sección Programación solo se aplican a los clientes de Windows 10.
>
>Si la aplicación publicada anteriormente es compatible con versiones anteriores del sistema operativo, cualquier fecha de detención de la **adquisición** que seleccione no se aplicará a los clientes. todavía podrán adquirir la aplicación (a menos que envíe una actualización con una nueva selección en la sección de [visibilidad](choose-visibility-options.md#discoverability) o si selecciona **crear aplicación no disponible** en la página de **información general** de la aplicación).

## <a name="base-schedule"></a>Programación base

Las selecciones que realice para la programación base se aplicarán a todos los mercados en los que esté disponible la aplicación, a menos que agregue posteriormente fechas para mercados específicos (o grupos de marketing) seleccionando [personalizar para mercados específicos](#customize-the-schedule-for-specific-markets).

Aquí verá dos opciones: **liberar** y detener la **adquisición** .

## <a name="release"></a>Release

En el menú desplegable **versión** , puede establecer cuándo desea que la aplicación esté disponible en la tienda. Esto significa que la aplicación es reconocible en el almacén a través de la búsqueda o la exploración, y que los clientes pueden ver su lista de tiendas y adquirir la aplicación.

>[!NOTE]
> Después de publicar la aplicación y de que esté disponible en el almacén, ya no podrá seleccionar una fecha de **lanzamiento** (ya que la aplicación ya se habrá lanzado).

Estas son las opciones que puede configurar para la programación de **versión** de un producto:
- lo **antes posible** : el producto se lanzará en cuanto se certifique y publique. Ésta es la opción predeterminada.
- **en** : el producto se publicará en la fecha y hora que seleccione. Además, tiene dos opciones:
   - **UTC** : la hora que seleccione será la hora universal coordinada (UTC), de modo que la aplicación se publique al mismo tiempo en todo el mundo.
   - **Local** : la hora que seleccione será la usada en cada zona horaria asociada a un mercado. (Tenga en cuenta que para los mercados que incluyen más de una zona horaria, solo se utilizará una zona horaria en ese mercado. En el Estados Unidos, se usa la zona horaria oriental. En esta página se muestra una lista completa de zonas horarias.)
- **no programado** : la aplicación no estará disponible en el almacén. Si elige esta opción, puede hacer que la aplicación esté disponible en la tienda más adelante creando un nuevo envío y eligiendo una de las otras opciones.

## <a name="stop-acquisition"></a>Detener adquisición

En la lista desplegable **detener adquisición** , puede establecer una fecha y una hora en las que desea dejar de permitir que los clientes nuevos lo adquieran de la tienda o detectar su lista. Esto puede ser útil si desea controlar de forma precisa Cuándo una aplicación ya no se ofrecerá a los nuevos clientes, como cuando está coordinando la disponibilidad entre más de una de sus aplicaciones.

De forma predeterminada, la opción **detener la adquisición** está establecida en nunca. Para cambiar esto **, seleccione en en la** lista desplegable y especifique una fecha y una hora, como se describió anteriormente. En la fecha y hora que seleccione, los clientes ya no podrán adquirir la aplicación.

Es importante comprender que esta opción tiene el mismo impacto que seleccionar **hacer que esta aplicación se pueda detectar pero no estar disponible** en la sección de [visibilidad](choose-visibility-options.md#discoverability) y elegir **detener la adquisición: cualquier cliente con un vínculo directo puede ver la lista de la tienda del producto, pero solo puede descargarla si poseía el producto antes o tiene un código promocional y usa un dispositivo de Windows 10.** Para dejar de ofrecer una aplicación completamente a los clientes nuevos, haga clic en **crear aplicación no disponible** en la página de información general de la aplicación. Para obtener más información, consulta [Quitar una aplicación de la Tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Si selecciona una fecha para **detener la adquisición** y más adelante decide que quiere que la aplicación vuelva a estar disponible, puede crear un nuevo envío y volver a cambiar la **adquisición de detenerla** a **nunca** . La aplicación volverá a estar disponible después de publicar el envío actualizado.

## <a name="customize-the-schedule-for-specific-markets"></a>Personalización de la programación para mercados específicos

De forma predeterminada, las opciones seleccionadas anteriormente se aplicarán a todos los mercados en los que se ofrece la aplicación. Para personalizar el precio de mercados específicos, haga clic en **personalizar para mercados específicos** . Aparecerá la ventana emergente de **selección de mercado** , donde se muestran todos los mercados en los que ha elegido hacer que la aplicación esté disponible. Si ha excluido los mercados de la sección [mercados](./define-market-selection.md) , no se mostrarán esos mercados.

Para agregar una programación para un mercado, selecciónela y haga clic en **Guardar** . Verá las mismas opciones de **versión** y **detención de adquisición** descritas anteriormente, pero las selecciones que realice solo se aplicarán a ese mercado.

Para agregar una programación que se aplicará a varios mercados, creará un *grupo de marketing* . Para ello, seleccione los mercados que quiera incluir y, a continuación, escriba un nombre para el grupo. (Este nombre es solo para su referencia y no será visible para ningún cliente). Por ejemplo, si desea crear un grupo de marketing para Norteamérica, puede seleccionar **Canada** , **México** y **Estados Unidos** , y asignarle el nombre **Norteamérica** u otro nombre que elija. Cuando haya terminado de crear el grupo de marketing, haga clic en **Guardar** . Verá las mismas opciones de **versión** y **detención de adquisición** descritas anteriormente, pero las selecciones que realice solo se aplicarán a ese grupo de marketing.

Para agregar una programación personalizada para un mercado adicional o un grupo de mercado adicional, solo tiene que hacer clic en **personalizar para mercados específicos** de nuevo y repetir estos pasos. Para cambiar los mercados incluidos en un grupo de mercado, seleccione su nombre. Para quitar la programación personalizada para un grupo de marketing (o mercado individual), haga clic en **quitar** .

> [!NOTE]
> Un mercado no puede pertenecer a más de uno de los grupos de marketing que se usan en la sección **programación** .

## <a name="global-time-zones"></a>Zonas horarias globales

A continuación se muestra una tabla en la que se muestran las zonas horarias específicas que se usan en cada mercado, por lo que cuando el envío utiliza la hora local (por ejemplo, el lanzamiento a las 9:00 de la versión local), puede averiguar el tiempo que se publicará en cada mercado, especialmente útil para los mercados que tienen más de una zona horaria, como Canadá.

| Mercado | Zona horaria |
|--------|-----------|
| Afganistán  |  (UTC+04:30) Kabul |
| Albania  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Argelia  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Samoa Americana  |  (UTC+13:00) Samoa |
| Andorra  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Angola  |  (UTC+01:00) África Central Occidental |
| Anguila  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Antártida  |  (UTC+12:00) Auckland, Wellington |
| Antigua y Barbuda  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Argentina  |  (UTC-03:00) Ciudad Autónoma de Buenos Aires |
| Armenia  |  (UTC+04:00) Abu Dhabi, Mascate |
| Aruba  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Australia  |  (UTC+10:00) Canberra, Melbourne, Sídney |
| Austria  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Azerbaiyán  |  (UTC+04:00) Bakú |
| Bahamas  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Baréin  |  (UTC+04:00) Abu Dhabi, Mascate |
| Bangladés  |  (UTC+06:00) Dacca |
| Barbados  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Belarús  |  (UTC+03:00) Minsk |
| Bélgica  |  (UTC+01:00) Bruselas, Copenhague, Madrid, París |
| Belice  |  (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Benín  |  (UTC+01:00) África Central Occidental |
| Bermudas  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Bután  |  (UTC+06:00) Dacca |
| República Bolivariana de Venezuela  |  (UTC-04:00) Caracas |
| Bolivia  |  (UTC-04:00) Georgetown, La Paz, Manaos, San Juan |
| Bonaire, San Eustaquio y Saba  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Bosnia y Herzegovina  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Botsuana  |  (UTC+01:00) África Central Occidental |
| Isla Bouvet  |  (UTC+00:00) Monrovia, Reykjavik |
| Brasil  |  (UTC-03:00) Brasilia |
| Territorio Británico del Océano Índico  |  (UTC+06:00) Dacca |
| Islas Vírgenes Británicas  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Brunéi  |  (UTC+08:00) Irkutsk |
| Bulgaria  |  (UTC+02:00) Chisinau |
| Burkina Faso  |  (UTC+00:00) Monrovia, Reykjavik |
| Burundi  |  (UTC+02:00) Harare, Pretoria |
| CÃ ́te d'Ivoire  |  (UTC+00:00) Monrovia, Reykjavik |
| Camboya  |  (UTC+07:00) Bangkok, Hanói, Yakarta |
| Camerún  |  (UTC+01:00) África Central Occidental |
| Canadá  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Cabo Verde  |  (UTC-01:00) Isla de Cabo Verde |
| Islas Caimán  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| República Centroafricana  |  (UTC+01:00) África Central Occidental |
| Chad  |  (UTC+01:00) África Central Occidental |
| Chile  |  (UTC-04:00) Santiago |
| China  |  (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Isla de Navidad  |  (UTC+07:00) Krasnoyarsk |
| Islas Cocos  |  (UTC+06:30) Yangón (Rangún) |
| Colombia  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Comoras  |  (UTC+03:00) Nairobi |
| Congo  |  (UTC+01:00) África Central Occidental |
| Congo (RDC)  |  (UTC+01:00) África Central Occidental |
| Islas Cook  |  (UTC-10:00) Hawái |
| Costa Rica  |  (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Croacia  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| CuraÃ § AO  |  (UTC-04:00) Cuiabá |
| Chipre  |  (UTC+02:00) Chisinau |
| Chequia  |  (UTC+01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Dinamarca  |  (UTC+01:00) Bruselas, Copenhague, Madrid, París |
| Yibuti  |  (UTC+03:00) Nairobi |
| Dominica  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| República Dominicana  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Ecuador  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Egipto  |  (UTC+02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Guinea Ecuatorial  |  (UTC+01:00) África Central Occidental |
| Eritrea  |  (UTC+03:00) Nairobi |
| Estonia  |  (UTC+02:00) Chisinau |
| Etiopía  |  (UTC+03:00) Nairobi |
| Islas Malvinas  |  (UTC-04:00) Santiago |
| Islas Feroe  |  (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| Fiyi  |  (UTC+12:00) Fiyi |
| Finlandia  |  (UTC+02:00) Helsinki, Kiev, Riga, Sofia, Tallin, Vilna |
| Francia  |  (UTC+01:00) Bruselas, Copenhague, Madrid, París |
| Guayana Francesa  |  (UTC-03:00) Cayena, Fortaleza |
| Polinesia Francesa  |  (UTC-10:00) Hawái |
| Tierras Australes y Antárticas Francesas  |  (UTC+05:00) Asjabad, Tashkent |
| Gabón  |  (UTC+01:00) África Central Occidental |
| Gambia  |  (UTC+00:00) Monrovia, Reykjavik |
| Georgia  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Alemania  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Ghana  |  (UTC+00:00) Monrovia, Reykjavik |
| Gibraltar  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Grecia  |  (UTC+02:00) Atenas, Bucarest |
| Groenlandia  |  (UTC+00:00) Monrovia, Reykjavik |
| Granada  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Guadalupe  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Guam  |  (UTC+10:00) Guam, Port Moresby |
| Guatemala  |  (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Guernsey  |  (UTC+00:00) Monrovia, Reykjavik |
| Guinea  |  (UTC+00:00) Monrovia, Reykjavik |
| Guinea-Bissau  |  (UTC+00:00) Monrovia, Reykjavik |
| Guyana  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Haití  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Isla Heard e Islas McDonald  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Santa Sede (Ciudad del Vaticano)  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Honduras  |  (UTC-06:00) Hora central (EE.UU. y Canadá) |
| RAE de Hong Kong  |  (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Hungría  |  (UTC+01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Islandia  |  (UTC+00:00) Monrovia, Reykjavik |
| India  |  (UTC+05:30) Chennai, Kolkata, Mumbai, Nueva Delhi |
| Indonesia  |  (UTC+07:00) Bangkok, Hanói, Yakarta |
| Iraq  |  (UTC+04:00) Abu Dhabi, Mascate |
| Irlanda  |  (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| Israel  |  (UTC+02:00) Jerusalén |
| Italia  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Jamaica  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Japón  |  (UTC+09:00) Osaka, Sapporo, Tokio |
| Jersey  |  (UTC+00:00) Monrovia, Reykjavik |
| Jordania  |  (UTC+02:00) Chisinau |
| Kazajistán  |  (UTC+05:00) Asjabad, Tashkent |
| Kenia  |  (UTC+03:00) Nairobi |
| Kiribati  |  (UTC+14:00) Isla de Kiritimati |
| Corea  |  (UTC+09:00) Seúl |
| Kuwait  |  (UTC+04:00) Abu Dhabi, Mascate |
| Kirguistán  |  (UTC+06:00) Astaná |
| Laos  |  (UTC+07:00) Bangkok, Hanói, Yakarta |
| Letonia  |  (UTC+02:00) Chisinau |
| Líbano  |  (UTC+02:00) Chisinau |
| Lesoto  |  (UTC+02:00) Harare, Pretoria |
| Liberia  |  (UTC+00:00) Monrovia, Reykjavik |
| Libia  |  (UTC+02:00) Chisinau |
| Liechtenstein  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Lituania  |  (UTC+02:00) Chisinau |
| Luxemburgo  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| RAE de Macao  |  (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Madagascar  |  (UTC+03:00) Nairobi |
| Malawi  |  (UTC+02:00) Harare, Pretoria |
| Malasia  |  (UTC+08:00) Kuala Lumpur, Singapur |
| Maldivas  |  (UTC+05:00) Asjabad, Tashkent |
| Mali  |  (UTC+00:00) Monrovia, Reykjavik |
| Malta  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Hombre, isla de  |  (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| Islas Marshall  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Antiguo |
| Martinica  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Mauritania  |  (UTC+00:00) Monrovia, Reykjavik |
| Mauricio  |  (UTC+04:00) Port Louis |
| Mayotte  |  (UTC+03:00) Nairobi |
| México  |  (UTC-06:00) Guadalajara, Ciudad de México, Monterrey |
| Micronesia  |  (UTC+10:00) Guam, Port Moresby |
| Moldova  |  (UTC+02:00) Chisinau |
| Mónaco  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Mongolia  |  (UTC+07:00) Krasnoyarsk |
| Montenegro  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Montserrat  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Marruecos  |  (UTC+01:00) Casablanca |
| Mozambique  |  (UTC+02:00) Harare, Pretoria |
| Myanmar  |  (UTC+06:30) Yangón (Rangún) |
| Namibia  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Nauru  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Antiguo |
| Nepal  |  (UTC+05:45) Katmandú |
| Países Bajos  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Nueva Caledonia  |  (UTC+11:00) Islas Solomon, Nueva Caledonia |
| Nueva Zelanda  |  (UTC+12:00) Auckland, Wellington |
| Nicaragua  |  (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Níger  |  (UTC+01:00) África Central Occidental |
| Nigeria  |  (UTC+01:00) África Central Occidental |
| Niue  |  (UTC+13:00) Samoa |
| Isla Norfolk  |  (UTC+11:00) Islas Solomon, Nueva Caledonia |
| Macedonia del Norte  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Islas Marianas del Norte  |  (UTC+10:00) Guam, Port Moresby |
| Noruega  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Omán  |  (UTC+04:00) Abu Dhabi, Mascate |
| Pakistán  |  (UTC+05:00) Islamabad, Karachi |
| Palau  |  (UTC+09:00) Osaka, Sapporo, Tokio |
| Autoridad palestina  |  (UTC+02:00) Chisinau |
| Panamá  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Papúa Nueva Guinea  |  (UTC+10:00) Vladivostok |
| Paraguay  |  (UTC-04:00) Asunción |
| Perú  |  (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Filipinas  |  (UTC+08:00) Kuala Lumpur, Singapur |
| Islas Pitcairn  |  (UTC-08:00) Hora del Pacífico (EE.UU. y Canadá) |
| Polonia  |  (UTC+01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Portugal  |  (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| Qatar  |  (UTC+04:00) Abu Dhabi, Mascate |
| Reunión  |  (UTC+04:00) Port Louis |
| Rumanía  |  (UTC+02:00) Chisinau |
| ROW  |  (UTC-07:00) Hora de las Montañas Rocosas (EE. UU. y Canadá) |
| Rusia  |  (UTC+03:00) Moscú, San Petersburgo |
| Ruanda  |  (UTC+02:00) Harare, Pretoria |
| SÃ £ o TomÃ © y PrÃncipe  |  (UTC+00:00) Monrovia, Reykjavik |
| San BarthÃ © LEMY  |  (UTC+04:00) Ereván |
| Santa Elena, Ascensión y Tristán da Cunha  |  (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| San Cristóbal y Nieves  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Santa Lucía  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| San Martín (zona francesa)  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| San Pedro y Miquelón  |  (UTC-02:00) Atlántico central - Antiguo |
| San Vicente y las Granadinas  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Samoa  |  (UTC+13:00) Samoa |
| San Marino  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Arabia Saudí  |  (UTC+03:00) Kuwait, Riad |
| Senegal  |  (UTC+00:00) Monrovia, Reykjavik |
| Serbia  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Seychelles  |  (UTC+04:00) Abu Dhabi, Mascate |
| Sierra Leona  |  (UTC+00:00) Monrovia, Reykjavik |
| Singapur  |  (UTC+08:00) Kuala Lumpur, Singapur |
| Sint Maarten (zona neerlandesa)  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Eslovaquia  |  (UTC+01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Eslovenia  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Islas Salomón  |  (UTC+11:00) Islas Solomon, Nueva Caledonia |
| Somalia  |  (UTC+03:00) Nairobi |
| Sudáfrica  |  (UTC+02:00) Harare, Pretoria |
| Islas Georgia del Sur y Sandwich del Sur  |  (UTC-02:00) Atlántico central - Antiguo |
| España  |  (UTC+01:00) Bruselas, Copenhague, Madrid, París |
| Sri Lanka  |  (UTC+05:30) Chennai, Kolkata, Mumbai, Nueva Delhi |
| Surinam  |  (UTC-03:00) Cayena, Fortaleza |
| Svalbard y Jan Mayen  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Suazilandia  |  (UTC+02:00) Harare, Pretoria |
| Suecia  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Suiza  |  (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Taiwán  |  (UTC+08:00) Taipéi |
| Tayikistán  |  (UTC+05:00) Asjabad, Tashkent |
| Tanzania  |  (UTC+03:00) Nairobi |
| Tailandia  |  (UTC+07:00) Bangkok, Hanói, Yakarta |
| Timor-Leste  |  (UTC+09:00) Seúl |
| Togo  |  (UTC+00:00) Monrovia, Reykjavik |
| Tokelau  |  (UTC+13:00) Nukualofa |
| Tonga  |  (UTC+13:00) Nukualofa |
| Trinidad y Tobago  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Túnez  |  (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Turquía  |  (UTC+03:00) Estambul |
| Turkmenistán  |  (UTC+05:00) Asjabad, Tashkent |
| Islas Turcas y Caicos  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Tuvalu  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Antiguo |
| EE. UU. Islas menores alejadas de EE. UU.  |  (UTC+13:00) Samoa |
| EE. UU. Vírgenes de EE. UU.  |  (UTC-04:00) Hora del Atlántico (Canadá) |
| Uganda  |  (UTC+03:00) Nairobi |
| Ucrania  |  (UTC+02:00) Chisinau |
| Emiratos Árabes Unidos  |  (UTC+04:00) Abu Dhabi, Mascate |
| Reino Unido  |  (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| Estados Unidos  |  (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Uruguay  |  (UTC-03:00) Brasilia |
| Uzbekistán  |  (UTC+05:00) Asjabad, Tashkent |
| Vanuatu  |  (UTC+11:00) Islas Solomon, Nueva Caledonia |
| Vietnam  |  (UTC+07:00) Bangkok, Hanói, Yakarta |
| Wallis y Futuna  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Antiguo |
| Yemen  |  (UTC+04:00) Abu Dhabi, Mascate |
| Zambia  |  (UTC+02:00) Harare, Pretoria |
| Zimbabue  |  (UTC+02:00) Harare, Pretoria |
