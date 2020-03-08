---
Description: Los informes de pago muestran los detalles sobre el dinero que ha obtenido con sus aplicaciones y complementos. También se muestra información que te permite saber cuándo recibirás los pagos y cuánto te pagarán.
title: Informes de pago
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, resumen de pago, extracto, pagos, ganancias, pagos, pago, beneficios
ms.localizationpriority: medium
ms.openlocfilehash: f4d8727a48cd68b304d515fe34082b4c4f632b4b
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853462"
---
# <a name="payout-reports"></a>Informes de pago

El **Resumen de pagos** muestra detalles sobre el dinero que ha obtenido con Microsoft. También se muestra información que te permite saber cuándo recibirás los pagos y cuánto te pagarán.

Si vendes productos en Azure Marketplace, también verás información sobre los pagos efectuados en **Resumen de pago**. Para obtener más detalles sobre el pago en Azure Marketplace, consulta la página [Microsoft Azure Marketplace Participation Policies (Directivas de participación de Microsoft Azure Marketplace)](https://docs.microsoft.com/legal/marketplace/participation-policy) y el documento [Microsoft Azure Marketplace Publisher Agreement (Acuerdo del publicador de Microsoft Azure Marketplace)](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Para ser válidos para el pago, el avance debe alcanzar el [umbral de pago](payment-thresholds-methods-and-timeframes.md) de $50. Para obtener más información sobre el umbral de pago, consulte esta página y revise el contrato para desarrolladores de aplicaciones.

> [!NOTE]
> Si busca soporte técnico relacionado con los pagos, incluida la configuración de las cuentas de pago, los pagos que faltan, la colocación de los pagos en espera o cualquier otra cosa, póngase en contacto con el soporte técnico [aquí](https://developer.microsoft.com/windows/support).

## <a name="access-the-payout-summary-pages"></a>Acceder a las páginas de Resumen de pago

Para abrir una de las páginas de Resumen de pago:

1. Seleccione el icono de pago en la esquina superior derecha.
2. Seleccione historial de transacciones, pagos o exportar datos.

## <a name="transaction-history-page"></a>Página historial de transacciones

En esta página se muestran todos los beneficios individuales, incluida la fecha, el tipo y la obtención de cada uno de ellos. Puede seleccionar un período de tiempo para verlo y también puede filtrar por ID. de inscripción, programa, ID. de pago, tipo de ganancia, palanca y estado. Los datos están disponibles para el año fiscal actual (1 de Julio – 30 de junio) y los dos años fiscales anteriores.

Para ver más detalles sobre una ganancia, seleccione la flecha hacia abajo que se encuentra en el lado derecho de la página. Esto mostrará la palanca, el importe de los ingresos y el producto. Si, por alguna razón, alguno de estos datos no está disponible, pero necesita acceder a él, póngase en contacto [con el soporte técnico](https://developer.microsoft.com/windows/support)]. Si la ganancia es el resultado de un ajuste, y no una transacción, no se mostrarán los campos del producto.

Para exportar cualquiera de los datos de la transacción en esta página, use la página **exportar datos** .

## <a name="payments-page"></a>Página pagos

Los totales de esta página representan todos los programas en los que participa. Puede filtrar por ID. de participante, programa, ID. de pago y tipo de ganancia. Los importes se indican en dólares estadounidenses. El valor de pago también se muestra en pago a moneda.

| Área                   | Descripción                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Pago total de este año   | El total combinado abonado a su año, en dólares estadounidenses, para todos sus programas.       |
| Siguiente pago estimado | El siguiente pago que le llega (incluso si hay otros que próximamente), en dólares estadounidenses. |
| Último pago           | La cantidad (en dólares estadounidenses), el nombre del programa y el programa de su pago más reciente.           |
| Pagos por origen     | Cantidad de pagos, en dólares estadounidenses, representados por el programa en los últimos 12 meses.           |
| Pagos               | Seleccione pago o pendiente y, a continuación, ordenar como desee. Para obtener detalles adicionales de una vista de pago específica, seleccione Vista. Para descargar una copia de la instrucción de remesa de pago, seleccione Descargar. Tenga en cuenta que los datos del historial de transacciones pueden tardar hasta 24 horas en aparecer, por lo que es posible que no vea las ganancias asociadas de inmediato. |

Para exportar cualquiera de los datos de esta página, seleccione Exportar y, a continuación, siga las instrucciones de la página exportar datos.

## <a name="transaction-history-page"></a>Página historial de transacciones

En esta página se muestran todos los beneficios individuales, incluida la fecha, el tipo y la obtención de cada uno de ellos. Puede seleccionar un período de tiempo para verlo y también puede filtrar por ID. de inscripción, programa, ID. de pago, tipo de ganancia, palanca y estado. Los datos están disponibles para el año fiscal actual (1 de Julio – 30 de junio) y los dos años fiscales anteriores.

Para ver más detalles sobre una ganancia, seleccione la flecha hacia abajo que se encuentra en el lado derecho de la página. Esto mostrará la palanca, el importe de los ingresos y el producto. Si, por alguna razón, alguno de estos datos no está disponible, pero necesita acceder a él, póngase en contacto [con el soporte técnico](https://developer.microsoft.com/windows/support)]. Si la ganancia es el resultado de un ajuste, y no una transacción, no se mostrarán los campos del producto.

Para exportar cualquiera de los datos de la transacción en esta página, seleccione Exportar y, a continuación, siga las instrucciones de la página exportar datos. Los archivos exportados desde la página historial de transacciones muestran datos en la moneda de la transacción, ganancias en la moneda de la transacción y en dólares estadounidenses, y el valor de pago en pago por moneda.

## <a name="payment-status"></a>Estado del pago

| Obteniendo estado           | Razón                                                                                                                                      | ¿Es necesaria la acción del asociado?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Sin procesar              | La ganancia es válida para el pago. Permanece en este estado durante un período de enfriamiento, tal como se define en la guía de programas para el programa de incentivos. | No                                                         |
| Próximo                 | El pedido de pago generó revisiones internas pendientes antes de que se procese el pago.                                                               | No                                                         |
| Factura de impuestos pendiente      | La factura de impuestos está incompleta o no es válida.                                                                                                  | Debe actualizar la factura de impuestos antes de que se le pague |
| Rechazado durante la revisión   | Se rechazó el pago durante la revisión.                                                                                                     | Póngase en contacto [con el soporte técnico de Microsoft](https://developer.microsoft.com/windows/support) para obtener detalles                      |
| No superadas                   | No se pudo realizar el pago debido a un error del sistema de Microsoft.                                                                                         | Póngase en contacto [con el soporte técnico de Microsoft](https://developer.microsoft.com/windows/support) para obtener detalles                      |
| En curso              | El pago está en curso.                                                                                                                 | No                                                         |
| Pago incorrecto        | La regresión del pago está en curso.                                                                                                       | No                                                         |
| Sent                     | Se ha enviado el pago al Banco.                                                                                                     | No                                                         |
| Reprocesamiento             | El pago encontró un error del sistema de Microsoft y se está reprocesando.                                                                  | No                                                         |
| Invertido                 | El banco invirtió el pago y se enviará de nuevo en el siguiente ciclo de pago.                                                     | No                                                         |
| Factura de impuestos rechazada     | La factura de impuestos se rechazó durante la revisión. Todos los pagos pendientes estarán en espera hasta que se complete la revisión de la factura de impuestos.                 | Póngase en contacto [con el soporte técnico de Microsoft](https://developer.microsoft.com/windows/support) para obtener detalles                      |
| Factura de impuestos en revisión | Se está revisando la factura de impuestos. El pago se publicará una vez que se haya aprobado la factura de impuestos.                                   | No                                                         |
| Rechazado                 | El Banco rechazó el pago.                                                                                                      | Póngase en contacto con su banco para obtener más información.                             |

## <a name="export-data-page"></a>Página exportar datos

Siga las instrucciones de esta página para exportar los datos que desee.

Notas:

- La página exportar datos no se actualiza por sí misma. Es posible que tenga que actualizar la página manualmente para ver los datos más recientes.
- El filtro puede producir un error no hay datos disponibles. Probablemente significa que ha dejado el período de tiempo predeterminado seleccionado en tres meses y, a continuación, ha seleccionado un ID. de pago de una ganancia que está fuera de ese período. Expanda el período de tiempo y vuelva a intentarlo.

## <a name="payments"></a>Pagos

![Exportar pagos](images/pc-export-payments.png)

Esta opción proporciona una descarga de los pagos que ha recibido en el Banco para un programa determinado, los impuestos asociados y el importe de la ganancia agregada. Este informe se usa para muchos programas del centro de Partners, por lo que es posible que algunas columnas no sean aplicables a su informe. Estas columnas se marcan a continuación.

| Nombre de columna              | Descripción                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | La identidad principal del socio que gana en el programa                                                                             |
| participantIDType        | Por lo general, identificador de programa para programas de incentivos e identificador de vendedor para programas de tienda                                                                |
| participantName          | Nombre del asociado que gana                                                                                                               |
| nombreprograma              | Nombre del programa de incentivos/tienda                                                                                                              |
| recibido                   | Cantidad acumulada en el pago por moneda para ese programa/participantID                                                                       |
| earnedUSD                | Cantidad ganada para el ID. de programa/participante, en USD                                                                                      |
| withheldTax              | Cantidad de impuestos retenidos en el pago por moneda para el programa/participantID                                                               |
| salesTax                 | Importe total del impuesto de ventas en el pago por moneda para el programa/participantID (aplicable solo para programas de incentivos)                   |
| serviceFeeTax            | Cantidad total de serviceFeeTax de pago a divisa para el programa/participantID (aplicable solo para programas de tienda y Azure Marketplace) |
| totalPayment             | Pago total en moneda local excluyendo el impuesto de retención e incluyendo el impuesto de venta (si procede) para el programa/participantID   |
| currencyCode             | Pago a código de divisa                                                                                                                      |
| paymentMethod            | El método que se usa para pagar al socio comercial, por ejemplo, transferencia electrónica bancaria, nota de crédito                                                     |
| paymentID                | Identificador único para el pago. Este número normalmente es visible en la instrucción Bank. (aplicable solo para pagos de SAP)              |
| paymentStatus            | Estado del pago                                                                                                                            |
| paymentStatusDescription | Descripción detallada del estado de pago                                                                                                    |
| paymentDate              | Fecha de envío del pago de Microsoft                                                                                                      |

## <a name="transaction-history"></a>Historial de transacciones

![Exportar historial de transacciones](images/pc-export-transaction.png)

Esta opción proporciona una descarga de cada elemento de línea que se ve en la página historial de transacciones, ganando el tipo, la fecha, el importe de la transacción asociada, el cliente, el producto y otros detalles transaccionales aplicables a los programas.

| Nombre de columna                    | Descripción                                                                                                                              | Aplicabilidad de incentivos/tienda/Azure Marketplace           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | Identificador único para cada obtención                                                                                                       | Todos                                                            |
| participantId                  | La identidad principal del socio que gana en el programa                                                                            | Todos                                                            |
| participantIdType              | Principalmente el identificador de programa para programas de incentivos y el vendedor si tiene programas de tienda y Azure Marketplace                                          | Todos                                                            |
| participantName                | Nombre del asociado que gana                                                                                                              | Todos                                                            |
| partnerCountryCode             | Ubicación/país del asociado que gana                                                                                                  | Todos                                                            |
| nombreprograma                    | Nombre del programa de incentivos/tienda                                                                                                             | Todos                                                            |
| transactionId                  | Identificador único de la transacción                                                                                                    | Todos                                                            |
| transactionCurrency            | Moneda en la que se produjo la transacción de cliente original (no es una moneda de ubicación de socio comercial)                                     | Todos                                                            |
| transactionDate                | Fecha de la transacción. Útil para programas en los que muchas transacciones contribuyen a una ganancia                                           | Todos                                                            |
| transactionExchangeRate        | Fecha de la tasa de cambio usada para mostrar la cantidad de transacción USD correspondiente                                                                 | Todos                                                            |
| transactionAmount              | Importe de la transacción en la moneda de la transacción original en función de la obtención generada                                              | Todos                                                            |
| transactionAmountUSD           | Importe de la transacción en USD                                                                                                                | Todos                                                            |
| Leverling                          | Indica la regla de negocios para la obtención                                                                                                  | Todos                                                            |
| earningRate                    | Tasa de incentivos aplicada en el importe de la transacción para generar una ganancia                                                                      | Todos                                                            |
| quantity                       | Varía en función del programa. Indica la cantidad facturada de programas transaccionales                                                            | Todos                                                            |
| quantityType                   | Indica el tipo de cantidad, por ejemplo, cantidad facturada, MAU                                                                             | Todos                                                            |
| earningType                    | Indica si es tarifa, descuento, Coop, venta, etc.                                                                                          | Todos                                                            |
| earningAmount                  | Ganar importe en la moneda de la transacción original                                                                                      | Todos                                                            |
| earningAmountUSD               | Importe en USD                                                                                                                    | Todos                                                            |
| earningDate                    | Fecha de la obtención                                                                                                                      | Todos                                                            |
| calculationDate                | Fecha en que se calculó la ganancia en el sistema                                                                                            | Todos                                                            |
| earningExchangeRate            | Tasa de cambio usada para mostrar la cantidad de USD correspondiente                                                                                  | Todos                                                            |
| exchangeRateDate               | Fecha de la tasa de cambio usada para calcular EarningAmount USD                                                                                   | Todos                                                            |
| paymentAmountWOTax             | Importe de la ganancia (sin impuestos) en pago a moneda por pagos "enviados"                                                                 | Todos                                                            |
| paymentCurrency                | Pague a la moneda seleccionada por el socio en el perfil de pago. Solo se muestra para los pagos enviados                                                   | Todos                                                            |
| paymentExchangeRate            | Tasa de cambio usada para calcular paymentAmountWOTax en moneda de pago mediante ExchangeRateDate                                            | Todos                                                            |
| claimId                        | Identificador único para la demanda                                                                                                              | Incentivos: solo algunos programas                                |
| planId                         | Identificador único para el plan                                                                                                               | Incentivos: solo algunos programas                                |
| paymentId                      | Identificador único para el pago. Este número suele estar visible en la instrucción Bank.                                                 | Solo pagos de SAP                                              |
| paymentStatus                  | Estado del pago                                                                                                                           | Todos                                                            |
| paymentStatusDescription       | Descripción detallada del estado de pago                                                                                                   | Todos                                                            |
| Identificador                     | Siempre estará en blanco                                                                                                                     | Solo programas de incentivos (excepción: OEM) y Azure Marketplace |
| customerName                   | Siempre estará en blanco                                                                                                                     | Solo programas de incentivos (excepción: OEM) y Azure Marketplace |
| partNumber                     | Siempre estará en blanco                                                                                                                     | Algunos incentivos y programas de tienda y Azure Marketplace        |
| productName                    | Nombre del producto vinculado a la transacción                                                                                                       | Todos                                                            |
| productId                      | Identificador único del producto                                                                                                                | Tienda y Azure Marketplace                                    |
| parentProductId                | Identificador único del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el identificador del producto principal es igual al identificador del producto. | Tienda y Azure Marketplace                                    |
| parentProductName              | Nombre del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el nombre del producto principal es igual al nombre del producto.   | Tienda y Azure Marketplace                                    |
| productType                    | Tipo de producto (por ejemplo aplicación, complemento, juego, etc.)                                                                                        | Tienda y Azure Marketplace                                    |
| invoiceNumber                  | Número de factura (aplicable solo para EA)                                                                                                  | Incentivos y Azure Marketplace: solo algunos programas           |
| subscriptionId                 | Identificador de suscripción asociado al cliente                                                                                         | Incentivos: solo algunos programas                                 |
| subscriptionStartDate          | Fecha de inicio de suscripción                                                                                                                  | Incentivos: solo algunos programas                                 |
| subscriptionEndDate            | Fecha de finalización de suscripción                                                                                                                    | Incentivos: solo algunos programas                                 |
| resellerId                     | Identificador reseller                                                                                                                      | Incentivos: solo algunos programas                                 |
| resellerName                   | Nombre del distribuidor                                                                                                                            | Incentivos: solo algunos programas                                 |
| distributorId                  | Identificador del distribuidor                                                                                                                   | Incentivos: solo algunos programas                                 |
| distributorName                | Nombre del distribuidor                                                                                                                         | Incentivos: solo algunos programas                                 |
| agreementNumber                | Número de contrato                                                                                                                         | Incentivos: solo algunos programas                                 |
| agreementStartDate             | Fecha de inicio de contrato                                                                                                                     | Incentivos: solo algunos programas                                 |
| agreementEndDate               | Fecha de finalización de contrato                                                                                                                       | Incentivos: solo algunos programas                                 |
| duplica                       | Carga de trabajo                                                                                                                                 | Incentivos: solo algunos programas                                 |
| transactionType                | Tipo de transacción (por ejemplo, compra, reembolso, inversión, anulación, etc.)                                                               | Tienda y Azure Marketplace                                    |
| localProviderSeller            | Proveedor o vendedor local de registro                                                                                                          | Solo almacenamiento                                                     |
| taxRemitted                    | Importe de impuestos enviados (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                                   | Tienda y Azure Marketplace                                    |
| taxRemitModel                  | Parte responsable de la remesa de impuestos (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                    | Solo almacenamiento                                                     |
| storeFee                       | La cantidad retenida por Microsoft como una cuota para que la aplicación o el complemento estén disponibles en la tienda.                                           | Solo almacenamiento                                                     |
| transactionPaymentMethod       | Instrumento de pago del cliente usado para la transacción (por ejemplo, tarjeta, facturación del operador de telefonía móvil, PayPal, etc.)                                | Tienda y Azure Marketplace                                    |
| tpan                           | Indica la red de ad de terceros                                                                                                     | Tienda: solo anuncios                                               |
| customerCountry                | País del cliente                                                                                                                         | Tienda y Azure Marketplace                                    |
| customerCity                   | Ciudad del cliente                                                                                                                            | Tienda y Azure Marketplace                                    |
| customerState                  | Estado del cliente                                                                                                                           | Tienda y Azure Marketplace                                    |
| customerZip                    | Código postal del cliente                                                                                                                 | Tienda y Azure Marketplace                                    |
| purchaseTypeCode               | Siempre estará en blanco                                                                                                                     | Programa de incentivos: CRI                                        |
| purchaseOrderType              | Siempre estará en blanco                                                                                                                     | Programa de incentivos: CRI                                        |
| purchaseOrderCoverageStartDate | Siempre estará en blanco                                                                                                                     | Programa de incentivos: CRI                                        |
| purchaseOrderCoverageEndDate   | Siempre estará en blanco                                                                                                                     | Programa de incentivos: CRI                                        |
| programOfferingLevel           |                                                                                                                                          | Programa de incentivos: CRI                                        |
| TenantID                       |                                                                                                                                          | Programas de incentivos                                             |
| externalReferenceId            | Identificador único para el programa                                                                                                        | Programas de pago directo (incentivos y tienda)                      |
| externalReferenceIdLabel       | Etiqueta de identificador único                                                                                                                  | Programas de pago directo (incentivos y tienda)                      |

## <a name="historical-statements"></a>Instrucciones históricas

![Exportar instrucciones históricas](images/pc-export-statements.png)

El historial de transacciones desde antes del 1 2019 de julio se trata por separado. Las instrucciones utilizarán los campos siguientes en lugar de los actuales.

> [!NOTE]
> El historial de transacciones heredadas tiene una columna denominada "reservada" que corresponde a la columna "beneficios" en el historial moderno, con la salvedad de que excluye todas las ganancias con estado = "pago enviado".

> [!NOTE]
> Los filtros (3M, 6 M, 12M, etc.) no se aplicarán a la sección de **instrucciones históricas** .

| Nombre del campo              | Descripción                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fuente de ingresos          | La fuente de tus ingresos, que se basa en el lugar en el que se ha producido la transacción (por ejemplo, Microsoft Store, Windows Phone Store, Windows Store 8, publicidad, etc.)                  |
| Order ID                | Un identificador de pedido exclusivo. Este id. permite identificar las transacciones de compra con sus respectivas transacciones que no son de compra (como las devoluciones, la anulaciones, etc.). Ambas tendrá el mismo id. de pedido. Además, en el caso de un cargo dividido en el que se hayan usado varios métodos de pago para una sola compra, te permitirá vincular las transacciones de compra. |
| Transaction ID          | Identificador único de la transacción.                                                                                                                                          |
| Fecha y hora de transacción   | Fecha y hora en que se realizó la transacción (hora UTC).                                                                                                                       |
| Id. de producto principal       | Identificador único del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el identificador del producto principal es igual al identificador del producto.                                |
| Product ID              | Identificador único del producto.                                                                                                                                              |
| Nombre del producto principal     | Nombre del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el nombre del producto principal es igual al nombre del producto.                                  |
| Nombre del producto            | Nombre del producto.                                                                                                                                                    |
| Tipo de producto            | Tipo de producto (por ejemplo aplicación, complemento, juego, etc.)                                                                                                                       |
| Cantidad                | Si el valor de Fuente de ingresos es la Microsoft Store para Empresas, Cantidad representa el número de licencias adquiridas. Para otras fuentes de ingresos, el valor de Cantidad siempre será 1. Nota: Aunque una misma transacción esté dividida en dos artículos de línea debido al uso de dos métodos de pago distintos, cada artículo de línea mostrará un valor de 1 en Cantidad. |
| Tipo de transacción        | Tipo de transacción (por ejemplo, compra, reembolso, inversión, anulación, etc.)                                                                                              |
| Método de pago          | Instrumento de pago del cliente usado para la transacción (por ejemplo, tarjeta, facturación del operador de telefonía móvil, PayPal, etc.)                                                               |
| País o región        | País o región donde se realizó la transacción.                                                                                                                          |
| Proveedor o vendedor local | Proveedor o vendedor local del registro.                                                                                                                                        |
| Moneda de transacción    | Moneda de la transacción.                                                                                                                                            |
| Importe de la transacción      | Importe de la transacción.                                                                                                                                              |
| Impuestos enviados            | Importe de impuestos enviados (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                                                                  |
| Ingresos netos            | Importe de la transacción menos impuestos.                                                                                                                                   |
| Honorarios de la Tienda               | Porcentaje de los ingresos netos retenido por Microsoft en concepto de honorarios por hacer que la aplicación o el complemento esté disponible en la Tienda.                                                      |
| Ganancias por la aplicación            | Ingresos netos menos los honorarios de la Tienda.                                                                                                                                       |
| Impuestos retenidos          | Importe de impuestos retenido. (No se incluye en el archivo .csv **Reservado**).                                                                                                |
| Pago                 | Ganancias por la aplicación menos la retención de impuestos aplicables (es el importe que aparece en Moneda de transacción). (No se incluye en el archivo .csv **Reservado**).                               |
| Tipo de cambio                 | Tipo de cambio usado para convertir la moneda de la transacción a la moneda de pago.                                                                                         |
| Moneda de pago        | Moneda en que se realiza el pago.                                                                                                                                       |
| Pago convertido       | Importe del pago convertido a la moneda de pago con el tipo de cambio.                                                                                                         |
| Modelo de remesa de impuestos         | Parte responsable de la remesa de impuestos (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                                                   |
| Fecha y hora de elegibilidad   | Fecha y hora en las que las ganancias por las transacciones sean aptas para el pago (hora UTC). Cuando se crea un pago, incluye las ganancias de las transacciones que tienen una fecha y hora de elegibilidad anterior a la fecha de creación del pago. (Solo se incluye en el archivo .csv **Reservado**). |
| Cargos                 | Muestra un desglose de todos los detalles de los cargos agregados en la columna Importe de la transacción. (Solo se incluye para Azure Marketplace; no se incluye en el archivo .csv **Reservado**). |