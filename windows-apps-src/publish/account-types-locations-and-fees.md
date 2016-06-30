---
author: jnHs
ms.assetid: C2415466-EC59-416E-B6AE-7DA5ED82DCE6
title: Tipos de cuenta, ubicaciones y precios
description: "Microsoft ofrece cuentas de desarrollador individuales y corporativas en muchos países y regiones."
translationtype: Human Translation
ms.sourcegitcommit: 93b6d952e42949917a0fff5b39f3f194f49531d5
ms.openlocfilehash: b4707f53aa1a1bd7bd7c03c74f2d18a0f9f55b10

---

# Tipos de cuenta, ubicaciones y precios

Microsoft ofrece dos tipos de cuentas de desarrollador: cuentas individuales y cuentas de empresa. Las cuentas de desarrollador se ofrecen en [varios países y regiones](#developer-account-and-app-submission-markets). Cualquier tipo de cuenta proporciona acceso para publicar aplicaciones en la Tienda y participar en otros programas de Microsoft para desarrolladores.

> **Nota**: Al crear una cuenta individual o corporativa, puedes asociar una sola cuenta de Microsoft con la cuenta de desarrollador. Al suscribirte a una cuenta, asegúrate de que inicias sesión con la cuenta Microsoft que quieres usar para tu cuenta de desarrollador.

Cuando [comiences el proceso de suscripción](http://go.microsoft.com/fwlink/p/?LinkId=615100), deberás elegir si quieres crear una cuenta individual o de empresa. Las cuentas individuales están pensadas para un único desarrollador que trabaja por su cuenta. Las cuentas de empresa están pensadas para empresas y organizaciones. Las cuentas de empresa dan acceso a algunas funcionalidades adicionales de la aplicación. También debemos comprobar con más atención las cuentas de empresa después de que te registres, para así confirmar que tienes autorización para configurar la cuenta de tu empresa. Esta comprobación puede tardar entre unos días y un par de semanas y suele incluir una llamada de teléfono a tu empresa. Ambos tipos de cuenta te permitirán enviar aplicaciones, complementos y servicios.

> **Importante**: No puedes cambiar el tipo de cuenta después haberlo seleccionado, así que asegúrate de elegir el tipo adecuado.

Estas son las diferencias fundamentales entre los dos tipos de cuentas.

| Cuenta individual | Cuenta de empresa |
|--------------------|-----------------|
| <ul><li>No puede usar funcionalidades específicas de las aplicaciones</li><li>Cuesta aproximadamente 19 USD (tarifa de registro única; el precio exacto varía en función del país o la región)</li></ul> | <ul><li>Requiere verificación mediante Symantec o Dun & Bradstreet</li><li>Más acceso a las funcionalidades de las aplicaciones</li><li>Requiere que la empresa esté reconocida como tal en el país o la región en la que se ubica</li><li>Cuesta aproximadamente 99 USD (tarifa de registro única; el precio exacto varía en función del país o la región)</li></ul> |

> **Importante**: Si decides crear una cuenta de empresa, asegúrate de leer nuestras [directrices para cuentas de empresa](opening-a-developer-account.md#additional-guidelines-for-company-accounts)

Las cuentas de empresa son un poco más caras, principalmente porque es necesario realizar pasos adicionales para asegurarnos de que tengas autorización para representar a tu empresa al configurar la cuenta. Una de las principales ventajas de tener una cuenta de empresa es la capacidad de enviar aplicaciones que declaren [declaraciones de funcionalidades de las aplicaciones](https://msdn.microsoft.com/library/windows/apps/Mt270968) adicionales.

Concretamente, debes tener una cuenta de empresa para poder publicar aplicaciones que usen estas tres funcionalidades:

-   **enterpriseAuthentication**: usa las credenciales de Windows para acceder a una intranet corporativa. Normalmente se utiliza en aplicaciones de línea de negocio que se conectan a los servidores de una empresa. (No necesitas esta funcionalidad para la comunicación genérica a través de Internet).
-   **sharedUserCertificates**: permite el acceso de una aplicación a certificados de software y hardware como, por ejemplo, los certificados almacenados en una tarjeta inteligente. Suele usarse para aplicaciones empresariales o financieras que requieren una tarjeta inteligente para la autenticación.
-   **documentsLibrary**: proporciona acceso mediante programación a los documentos del usuario, filtrados por las asociaciones de tipo de archivo declaradas en el manifiesto del paquete. (No necesitas declarar esta funcionalidad para acceder a los Documentos de un usuario con el [selector de archivos](https://msdn.microsoft.com/library/windows/apps/Hh465174)). Ten en cuenta que las aplicaciones destinadas a Windows Phone 8.1 o versiones anteriores no pueden usar la funcionalidad documentsLibrary.

> **Nota**: Además, debes ponerte en contacto con el soporte técnico y obtener su aprobación para poder enviar aplicaciones que declaren la funcionalidad de **documentsLibrary**.

Tener una cuenta de la empresa no garantiza que las aplicaciones que usan estas funcionalidades aprobarán la certificación. Estas funcionalidades están destinadas a escenarios muy específicos, y la mayoría de las aplicaciones no deberían declararlas. Para obtener más información, consulta [Declaraciones de funcionalidad de las aplicaciones](https://msdn.microsoft.com/library/windows/apps/Mt270968).

En las cuentas de la compañía, usamos Symantec o Dun & Bradstreet para comprobar que tienes autorización para crear una cuenta para la compañía a la que representas. Este proceso puede tardar entre un par de días y un par de semanas y suele incluir una llamada telefónica a la empresa (de modo que deberás asegurarte de que la información de contacto esté actualizada cuando rellenes los formularios de registro). No podrás enviar aplicaciones desde una cuenta de la empresa hasta que esta no esté comprobada pero, mientras esperas, puedes [reservar un nombre de aplicación](https://msdn.microsoft.com/library/windows/apps/JJ657967), acceder al panel del Centro de desarrollo de Windows y seguir creando y probando la aplicación.

## Cuenta de desarrollador y mercados de envío de aplicaciones

Puedes registrarte con una cuenta de desarrollador y enviar aplicaciones si resides o diriges un negocio en uno de los países o regiones indicados a continuación.

En la tabla siguiente, la **Tarifa de registro** es lo que cuesta actualmente el registro de la cuenta de desarrollador. Los precios están sujetos a cambios.

> **Nota**: Aplicaremos cualquier impuesto o tasa aplicables a la tarifa de registro cuando te suscribas. Además, cuando te paguemos, es posible que el banco te cobre un cargo por recibir el pago.

La columna **Pago** describe cómo te pagaremos el dinero que ganes con tus aplicaciones. Podrás elegir la [cuenta de pago](setting-up-your-payout-account-and-tax-forms.md) en la que te depositaremos este dinero.

> **Nota**: Algunos mercados no admiten aplicaciones de **pago**. En esos mercados, solo se pueden enviar aplicaciones gratuitas.

La columna **PayPal** indica si PayPal es un método de pago admitido que puede usarse para tu [cuenta de pago](https://msdn.microsoft.com/library/windows/apps/Bg124529) (y, salvo que se indique lo contrario, para la tarifa de registro de la cuenta de desarrollador).

| País o región | Tarifa de reg. individual | Tarifa de reg. corporativa | Pago | PayPal |
|----------------|---------------------|------------------|--------|--------|
| Afganistán | 19 USD | 99 USD | USD pagados al banco | No |
| Albania | 19 USD | 99 USD | USD pagados al banco | No |
| Argelia | 1535 DZD | 7000 DZD | USD pagados al banco | No | 
|  Andorra | 14 EUR | 75 EUR | No pago | No | 
|  Angola | 19 USD | 99 USD | USD pagados al banco | No | 
|  Antigua y Barbuda | 19 USD | 99 USD | XCD pagados al banco | No | 
|  Argentina | 106 ARS | 420 ARS | USD pagados al banco | No | 
|  Armenia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Australia | 21 AUD | 110 AUD | AUD pagados al banco | Sí | 
|  Austria | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Azerbaiyán | 19 USD | 99 USD | AZN pagados al banco | No | 
|  Bahamas | 19 USD | 99 USD | No pago | No | 
|  Baréin | 7 BHD | 38 BHD | BHD pagados al banco | No | 
|  Bangladesh | 1468 BDT | 7600 BDT | BDT pagados al banco | No | 
|  Barbados | 19 USD | 99 USD | No pago | No | 
|  Belarús | 19 USD | 99 USD | USD pagados al banco | No | 
|  Bélgica | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Belice | 19 USD | 99 USD | No pago | No | 
|  Benín | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Bután | 19 USD | 99 USD | No pago | No | 
|  Bolivia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Bosnia y Herzegovina | 19 USD | 99 USD | USD pagados al banco | No | 
|  Botsuana | 19 USD | 99 USD | BWP pagados al banco | No | 
|  Brasil | 46 BRL | 160 BRL | USD pagados al banco | No | 
|  Brunéi | 19 USD | 99 USD | No pago | No | 
|  Bulgaria | 28 BGN | 160 BGN | BGN pagados al banco | No | 
|  Burkina Faso | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Burundi | 19 USD | 99 USD | USD pagados al banco | No | 
|  Cabo Verde | 19 USD | 99 USD | No pago | No | 
|  Camboya | 19 USD | 99 USD | USD pagados al banco | No | 
|  Camerún | 19 USD | 99 USD | XAF pagados al banco | No | 
|  Canadá | 20 CAD | 99 CAD | CAD pagados al banco | Sí | 
|  República Centroafricana | 19 USD | 99 USD | XAF pagados al banco | No | 
|  Chad | 19 USD | 99 USD | XAF pagados al banco | No | 
|  Chile | 9776 CLP | 46 000 CLP | USD pagados al banco | No | 
|  China | 116 CNY | 600 CNY | USD pagados al banco | Sí* | 
|  Colombia | 36 543 COP | 180 000 COP | USD pagados al banco | No | 
|  Comoras | 19 USD | 99 USD | USD pagados al banco | No | 
|  Congo | 19 USD | 99 USD | XAF pagados al banco | No | 
|  Congo (RDC) | 19 USD | 99 USD | USD pagados al banco | No | 
|  Costa Rica | 9578 CRC | 49725 CRC | CRC pagados al banco | No | 
|  Côte d'Ivoire | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Croacia | 107 HRK | 500 HRK | HRK pagados al banco | No | 
|  Chipre | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  República Checa | 365 CZK | 1720 CZK | CZK pagadas al banco | Sí | 
|  Dinamarca | 106 DKK | 530 DKK | DKK pagadas al banco | Sí | 
|  Yibuti | 19 USD | 99 USD | No pago | No | 
|  Dominica | 19 USD | 99 USD | XCD pagados al banco | No | 
|  República Dominicana | 19 USD | 99 USD | DOP pagados al banco | No | 
|  Ecuador | 19 USD | 99 USD | USD pagados al banco | No | 
|  Egipto | 133 EGP | 600 EGP | EGP pagadas al banco | No | 
|  El Salvador | 19 USD | 99 USD | USD pagados al banco | No | 
|  Guinea Ecuatorial | 19 USD | 99 USD | No pago | No | 
|  Eritrea | 19 USD | 99 USD | USD pagados al banco | No | 
|  Estonia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Etiopía | 19 USD | 99 USD | ETB pagados al banco | No | 
|  Islas Feroe | 19 USD | 99 USD | No pago | No | 
|  Fiyi | 19 USD | 99 USD | FJD pagados al banco | No | 
|  Finlandia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Francia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Gabón | 19 USD | 99 USD | No pago | No | 
|  Gambia | 19 USD | 99 USD | No pago | No | 
|  Georgia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Alemania | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Ghana | 19 USD | 99 USD | GHS pagados al banco | No | 
|  Grecia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Groenlandia | 19 USD | 99 USD | Sin pago | No | 
|  Granada | 19 USD | 99 USD | Sin pago | No | 
|  Guatemala | 145 GTQ | 750 GTQ | GTQ pagados al banco | No | 
|  Guinea | 19 USD | 99 USD | USD pagados al banco | No | 
|  Guinea-Bisáu | 19 USD | 99 USD | No pago | No | 
|  Guyana | 19 USD | 99 USD | No pago | No | 
|  Haití | 19 USD | 99 USD | USD pagados al banco | No | 
|  Honduras | 19 USD | 99 USD | USD pagados al banco | No | 
|  RAE de Hong Kong | 147 HKD | 760 HKD | HKD pagados al banco | Sí | 
|  Hungría | 4237 HUF | 18 800 HUF | HUF pagados al banco | Sí* | 
|  Islandia | 2319 ISK | 11929 ISK | USD pagados al banco | No | 
|  India | 1201 INR | 4500 INR | INR pagadas al banco | No | 
|  Indonesia | 203015 IDR | 1000000 IDR | IDR pagados al banco | No | 
|  Irak | 22078 IQD | 120000 IQD | USD pagados al banco | No | 
|  Irlanda | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Israel | 67 ILS | 350 ILS | ILS pagados al banco | Sí | 
|  Italia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Jamaica | 19 USD | 99 USD | JMD pagados al banco | No | 
|  Japón | 1847 JPY | 9800 JPY | JPY pagados al banco | Sí | 
|  Jordania | 13 JOD | 70 JOD | JOD pagados al banco | No | 
|  Kazajistán | 2897 KZT | 15038 KZT | KZT pagados al banco | No | 
|  Kenia | 1900 KES | 9999 KES | KES pagados al banco | No | 
|  Kiribati | 19 USD | 99 USD | Sin pago | No | 
|  Corea del Sur | 21216 KRW | 108000 KRW | USD pagados al banco | No | 
|  Kuwait | 5 KWD | 28 KWD | KWD pagados al banco | No | 
|  Kirguistán | 19 USD | 99 USD | No pago | No | 
|  Laos | 19 USD | 99 USD | USD pagados al banco | No | 
|  Letonia | 14 EUR | 75 EUR | EUR pagados al banco | No | 
|  Líbano | 28690 LBP | 149686 LBP | LBP pagadas al banco | No | 
|  Lesoto | 19 USD | 99 USD | No pago | No | 
|  Liberia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Libia | 19 USD | 99 USD | No pago | No | 
|  Liechtenstein | 17 CHF | 93 CHF | CHF pagados al banco | Sí | 
|  Lituania | 14 EUR | 75 EUR | EUR pagados al banco | No | 
|  Luxemburgo | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  RAE de Macao | 19 USD | 99 USD | No pago | No | 
|  ERY de Macedonia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Madagascar | 19 USD | 99 USD | USD pagados al banco | No | 
|  Malawi | 19 USD | 99 USD | MWK pagados al banco | No | 
|  Malasia | 62 MYR | 300 MYR | USD pagados al banco | No | 
|  Maldivas | 19 USD | 99 USD | No pago | No | 
|  Malí | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Malta | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Islas Marshall | 19 USD | 99 USD | No pago | No | 
|  Mauritania | 5681 MRO | 30046 MRO | No pago | No | 
|  Mauricio | 19 USD | 99 USD | MUR pagados al banco | No | 
|  México | 247 MXN | 1140 MXN | MXN pagados al banco | Sí | 
|  Micronesia | 19 USD | 99 USD | No pago | No | 
|  Mónaco | 14 EUR | 75 EUR | EUR pagados al banco | No | 
|  Mongolia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Montenegro | 14 EUR | 75 EUR | EUR pagados al banco | No | 
|  Marruecos | 158 MAD | 800 MAD | MAD pagados al banco | No | 
|  Mozambique | 19 USD | 99 USD | USD pagados al banco | No | 
|  Myanmar | 19 USD | 99 USD | No pago | No | 
|  Namibia | 19 USD | 99 USD | No pago | No | 
|  Nauru | 19 USD | 99 USD | No pago | No | 
|  Nepal | 19 USD | 99 USD | USD pagados al banco | No | 
|  Países Bajos | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Nueva Zelanda | 24 NZD | 140 NZD | NZD pagados al banco | Sí | 
|  Nicaragua | 19 USD | 99 USD | USD pagados al banco | No | 
|  Níger | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Nigeria | 3700 NGN | 19500 NGN | NGN pagados al banco | No | 
|  Noruega | 113 NOK | 580 NOK | NOK pagadas al banco | Sí | 
|  Omán | 7 OMR | 40 OMR | OMR pagados al banco | No | 
|  Pakistán | 1959 PKR | 9000 PKR | PKR pagados al banco | No | 
|  Palaos | 19 USD | 99 USD | No pago | No | 
|  Panamá | 19 USD | 99 USD | USD pagados al banco | No | 
|  Papúa Nueva Guinea | 19 USD | 99 USD | No pago | No | 
|  Paraguay | 19 USD | 99 USD | USD pagados al banco | No | 
|  Perú | 54 PEN | 280 PEN | PEN pagados al banco | No | 
|  Filipinas | 832 PHP | 4400 PHP | PHP pagados al banco | Sí | 
|  Polonia | 59 PLN | 280 PLN | PLN pagados al banco | Sí | 
|  Portugal | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Catar | 69 QAR | 360 QAR | QAR pagados al banco | No | 
|  Rumania | 14 EUR | 75 EUR | USD pagados al banco | No | 
|  Rusia | 626 RUB | 3000 RUB | RUB pagados al banco | Sí* | 
|  Ruanda | 19 USD | 99 USD | RWF pagados al banco | No | 
|  San Cristóbal y Nieves | 19 USD | 99 USD | No pago | No | 
|  Santa Lucía | 19 USD | 99 USD | No pago | No | 
|  San Vicente y las Granadinas | 19 USD | 99 USD | XCD pagados al banco | No | 
|  Samoa | 19 USD | 99 USD | No pago | No | 
|  San Marino |  14 EUR | 75 EUR | No pago | No | 
|  Santo Tomé y Príncipe | 19 USD | 99 USD | No pago | No | 
|  Arabia Saudí | 71 SAR | 380 SAR | SAR pagados al banco | No | 
|  Senegal | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Serbia | 1619 RSD | 7000 RSD | USD pagados al banco | No | 
|  Seychelles | 19 USD | 99 USD | No pago | No | 
|  Sierra Leona | 19 USD | 99 USD | USD pagados al banco | No | 
|  Singapur | 24 SGD | 120 SGD | SGD pagados al banco | Sí | 
|  Eslovaquia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Eslovenia | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Islas Salomón | 19 USD | 99 USD | No pago | No | 
|  Somalia | 19 USD | 99 USD | USD pagados al banco | No | 
|  Sudáfrica | 193 ZAR | 700 ZAR | ZAR pagados al banco | No | 
|  España | 14 EUR | 75 EUR | EUR pagados al banco | Sí | 
|  Sri Lanka | 19 USD | 99 USD | LKR pagados al banco | No | 
|  Surinam | 19 USD | 99 USD | No pago | No | 
|  Suazilandia | 19 USD | 99 USD | No pago | No | 
|  Suecia | 123 SEK | 700 SEK | SEK pagadas al banco | Sí | 
|  Suiza | 17 CHF | 90 CHF | CHF pagados al banco | Sí | 
|  Taiwán | 568 TWD | 2840 TWD | USD pagados al banco | Sí | 
|  Tayikistán | 19 USD | 99 USD | USD pagados al banco | No | 
|  Tanzania | 19 USD | 99 USD | TZS pagados al banco | No | 
|  Tailandia | 601 THB | 3000 THB | THB pagados al banco | Sí | 
|  Timor-Leste | 19 USD | 99 USD | USD pagados al banco | No | 
|  Togo | 19 USD | 99 USD | XOF pagados al banco | No | 
|  Tonga | 19 USD | 99 USD | TOP pagados al banco | No | 
|  Trinidad y Tobago | 122 TTD | 636 TTD | TTD pagados al banco | No | 
|  Túnez | 31 TND | 140 TND | TND pagados al banco | No | 
|  Turquía | 37 TRY | 160 TRY | TRY pagadas al banco | No | 
|  Turkmenistán | 19 USD | 99 USD | USD pagados al banco | No | 
|  Tuvalu | 19 USD | 99 USD | No pago | No | 
|  Uganda | 19 USD | 99 USD | UGX pagados al banco | No | 
|  Ucrania | 156 UAH | 800 UAH | USD pagados al banco | No | 
|  Emiratos Árabes Unidos | 19 USD | 99 USD | EUR pagados al banco | Sí | 
|  Reino Unido | 12 GBP | 65 GBP | GBP pagadas al banco | Sí | 
|  Estados Unidos | 19 USD | 99 USD | USD pagados al banco | Sí | 
|  Uruguay | 19 USD | 99 USD | UYU pagados al banco | No | 
|  Uzbekistán | 19 USD | 99 USD | USD pagados al banco | No | 
|  Vanuatu | 19 USD | 99 USD | No pago | No | 
|  Venezuela | 119 VEF | 420 VEF | USD pagados al banco | No | 
|  Vietnam | 400425 VND | 2000000 VND | VND pagados al banco | No | 
|  Yemen | 4080 YER | 21245 YER | No pago | No | 
|  Zambia | 19 USD | 99 USD | ZMK pagados al banco | No | 
|  Zimbabue | 19 USD | 99 USD | USD pagados al banco | No |

\* PayPal puede usarse como método de pago para cuentas de pago en este mercado, pero no puede usarse para pagar la tarifa de registro de la cuenta de desarrollador.

## Temas relacionados

* [Abrir una cuenta de desarrollador](opening-a-developer-account.md)
* [Configurar formularios fiscales y cuentas de pago](setting-up-your-payout-account-and-tax-forms.md)
* [Recibir pagos](getting-paid-apps.md)
 



<!--HONumber=Jun16_HO4-->


