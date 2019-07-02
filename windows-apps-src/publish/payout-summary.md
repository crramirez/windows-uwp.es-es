---
Description: En el apartado Resumen de pago se muestran los detalles sobre el dinero que has ganado con tus aplicaciones y complementos. También se muestra información que te permite saber cuándo recibirás los pagos y cuánto te pagarán.
title: Resumen de pago
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, resumen de pago, extracto, pagos, ganancias, pagos, pago, beneficios
ms.localizationpriority: medium
ms.openlocfilehash: 90238360ecc48beb974546dc5b49ac09c01407eb
ms.sourcegitcommit: 35a511c2b29ae3d5008612a5fc13d3eb6370d2d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67495730"
---
# <a name="payout-summary"></a>Resumen de pago

El **resumen de pagos** muestra los detalles sobre el dinero has ganado con Microsoft. También se muestra información que te permite saber cuándo recibirás los pagos y cuánto te pagarán.

Si vendes productos en Azure Marketplace, también verás información sobre los pagos efectuados en **Resumen de pago**. Para obtener más detalles sobre el pago en Azure Marketplace, consulta la página [Microsoft Azure Marketplace Participation Policies (Directivas de participación de Microsoft Azure Marketplace)](https://go.microsoft.com/fwlink/p/?LinkId=722436) y el documento [Microsoft Azure Marketplace Publisher Agreement (Acuerdo del publicador de Microsoft Azure Marketplace)](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Para ser elegible para pago, debe alcanzar la lleva a cabo la [umbral pago](payment-thresholds-methods-and-timeframes.md) de 50 dólares. Para obtener más información acerca del umbral de pago vea esta página y revise el contrato de desarrollador de la aplicación.

## <a name="access-the-payout-summary-pages"></a>Acceder a las páginas de resumen de pago

Para abrir una de las páginas de resumen de pagos:

1. Seleccione el icono de dinero en la esquina superior derecha.
2. Seleccione los pagos, historial de transacciones, o exportar datos.

## <a name="payments-page"></a>Página de pagos

Los totales en esta página representan todos los programas que participa en. Puede filtrar por Id. del participante, programa, Id. de pago y tipo Earning. Se proporcionan los importes en dólares estadounidenses. El valor de pago también se muestra en el pago a la moneda.

| Área                   | Descripción                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total de pago de este año   | El total combinado abonados, este año en dólares estadounidenses, para todos los programas.       |
| Próximo pago estimado | El único próximo pago acudir a usted (incluso si hay otros próximamente), en dólares estadounidenses. |
| Último pago           | La cantidad (en dólares estadounidenses), el nombre del programa y el programa de su pago más reciente.           |
| Pagos por origen     | Cantidad de pagos en dólares estadounidenses, representado por el programa durante los últimos 12 meses.           |
| Pagos               | Seleccione pago o pendiente y, a continuación, ordenar como desee. Para obtener más detalles de un pago específico, seleccione la vista. Para descargar una copia de la instrucción de remesa de pago, seleccione la descarga. Tenga en cuenta que los datos del historial de transacciones pueden tardar hasta 24 horas en aparecer, por lo que no puede ver inmediatamente los beneficios asociados. |

Para exportarlos datos en esta página, seleccione Exportar y, a continuación, siga las instrucciones en la página de datos de exportación.

## <a name="transaction-history-page"></a>Página Historial de transacciones

Esta página muestra todas las utilidades individuales, incluyendo la fecha, tipo y a ganar para cada uno. Puede seleccionar un período de tiempo para ver y, también puede filtrar por Id. de inscripción, programa, Id. de pago, tipo Earning, palanca y estado. Datos están disponibles para el año fiscal actual (del 1 de julio del 30 de junio) y dos años fiscales anteriores.

Para ver más detalles sobre una ganancia, seleccione la flecha hacia abajo en el lado derecho de la página. Esto mostrará la palanca, la cantidad de ingresos y el producto. Si por alguna razón cualquiera de estos datos no están disponibles, pero necesita acceso a ella, póngase en contacto con [admite](https://developer.microsoft.com/en-us/windows/support)]. Si la obtención es el resultado de un ajuste y no una transacción, no se mostrarán los campos de producto.

Para exportarlos datos de transacciones en esta página, seleccione Exportar y, a continuación, siga las instrucciones en la página de datos de exportación. Los archivos exportados desde la página de historial de transacciones mostrar datos en la moneda de la transacción, las ganancias de divisa de la transacción y dólares estadounidenses, y el valor de pago en pagar a moneda.

## <a name="payment-status"></a>Estado del pago

| Obteniendo estado           | Reason                                                                                                                                      | ¿Requerido una acción de Partner?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Sin procesar              | La obtención es apta para el pago. Permanece en este estado durante un período de enfriamiento tal como se define en la Guía de programas para el programa de incentivos. | No                                                         |
| Próximas                 | Orden de pago generada pendientes las revisiones internas antes de que se procesa el pago.                                                               | No                                                         |
| Factura de impuestos pendiente      | Su factura de impuestos es incompleto o no es válido.                                                                                                  | Debe actualizar su factura de impuestos antes de que puede ser de pago |
| Rechazado durante la revisión   | Se rechazó el pago durante la revisión.                                                                                                     | Póngase en contacto con [soporte técnico de Microsoft](https://developer.microsoft.com/en-us/windows/support) para obtener más información                      |
| Failed                   | El pago no se pudo debido a un error de sistema de Microsoft.                                                                                         | Póngase en contacto con [soporte técnico de Microsoft](https://developer.microsoft.com/en-us/windows/support) para obtener más información                      |
| En curso              | El pago está en curso.                                                                                                                 | No                                                         |
| Pago incorrecto        | El pago amortizar está en curso.                                                                                                       | No                                                         |
| Enviado                     | El pago se envió su entidad bancaria.                                                                                                     | No                                                         |
| Volver a procesar             | El pago se ha encontrado un error del sistema de Microsoft y se vuelve a procesar.                                                                  | No                                                         |
| Invertida                 | El pago se ha revertido mediante su banco y se enviará de nuevo en el siguiente ciclo de pago.                                                     | No                                                         |
| Factura de impuestos rechazado     | Se rechazó su factura de impuestos durante la revisión. Todos los pagos pendientes estará en suspensión hasta que se complete la revisión de facturas de impuestos.                 | Póngase en contacto con [soporte técnico de Microsoft](https://developer.microsoft.com/en-us/windows/support) para obtener más información                      |
| Factura de impuestos bajo revisión | Se está revisando la factura de impuestos. Una vez aprobada la factura de impuestos, se publicará el pago.                                   | No                                                         |
| Rechazado                 | El pago se rechazó su banco.                                                                                                      | Para obtener más información, póngase en contacto con su banco.                             |

## <a name="export-data-page"></a>Exportar datos (página)

Siga las instrucciones de esta página para exportar los datos que desee.

Notas:

- Cuando tenga acceso a esta página desde cualquiera de los pagos o la página Historial de transacciones, los filtros no transportan a través. Deberá rehacer en la página de datos de exportación.
- La página de datos de exportación no se actualiza por sí mismo. Es posible que deba actualizar la página manualmente para ver los datos más recientes.
- El filtro puede provocar un error No disponible en datos. Esto significa que probablemente tenga deja el valor predeterminado de período de tiempo seleccionado en tres meses y, a continuación, seleccionado un identificador de pago de una ganancia que está fuera de ese período. Expanda su período de tiempo e inténtelo de nuevo.

## <a name="payment-download-export"></a>Exportación de descarga de pago

Esta opción proporciona una descarga de los pagos recibidos en su banco para un programa determinado, los impuestos asociados y agrega la cantidad de ingresos. Este informe se usa en muchos programas de centro de partners, por lo que algunas columnas puede no aplicables a un informe. A continuación, se marcan esas columnas.

| Nombre de columna              | Descripción                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participantID            | La identidad del socio a ganar bajo el programa principal                                                                           |
| participantIDType        | Normalmente, identificador de programa para programas de incentivos y el Id. de vendedor para los programas de Store                                                              |
| participantName          | Nombre del socio con                                                                                                             |
| programName              | Nombre del programa de incentivos/store                                                                                                            |
| acumulado                   | Importe acumulado en la moneda pagar a para ese programa/participantID                                                                     |
| earnedUSD                | Importe acumulado para el identificador de participante del programa en dólares estadounidenses                                                                                    |
| withheldTax              | Importe de impuestos retenidos en la moneda pagar a para el programa/participantID                                                             |
| salesTax                 | Cantidad total de impuestos en la moneda pagar a para el programa/participantID                                                          |
| totalPayment             | Total de pago en la moneda local, excepto la retención de impuestos y los impuestos incluidos (si procede) para el programa/participantID |
| currencyCode             | Pagar para el código de divisa                                                                                                                    |
| PaymentMethod            | El método utilizado para pagar al asociado (transferencia bancaria electrónica, nota de crédito)                                                              |
| paymentID                | Identificador único para el pago. Este número es normalmente visible en la instrucción del banco.                                               |
| paymentStatus            | Estado del pago                                                                                                                          |
| paymentStatusDescription | Descripción del estado de pago                                                                                                  |
| paymentDate              | Se envió el pago de la fecha de Microsoft                                                                                                    |

## <a name="transaction-history-download-export"></a>Exportación de descarga del historial de transacciones

Esta opción proporciona una descarga de cada elemento de línea con que ve en la página de historial de transacciones, a ganar tipo, fecha, importe de transacción asociada, customer, product y otros detalles transaccionales aplicables a sus programas.

| Nombre de columna                    | Descripción                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| earningId                      | Identificador único para cada a ganar                                                                                                       |
| participantId                  | La identidad del socio a ganar bajo el programa principal                                                                            |
| participantIdType              | Id. de vendedor                                                                                                                                |
| participantName                | Nombre del socio con                                                                                                              |
| partnerCountryCode             | Ubicación o el país del socio con                                                                                                  |
| programName                    | Nombre del programa de incentivos/store                                                                                                             |
| transactionId                  | Identificador único para la transacción                                                                                                    |
| transactionCurrency            | Moneda en la que se produjo la transacción del cliente original                                                                             |
| transactionDate                | Fecha de la transacción. Útil para los programas que muchas transacciones contribuyen a ganar uno                                           |
| transactionExchangeRate        | Fecha de tasa de cambio que se usa para mostrar la cantidad de dólares correspondiente                                                                             |
| transactionAmount              | Importe de transacción en la divisa de la transacción original en función de qué a ganar se genera                                              |
| transactionAmountUSD           | Cantidad de transacciones en USD                                                                                                                |
| Palanca                          | Indica la regla de negocios para la obtención                                                                                                  |
| earningRate                    | Tipo incentivo aplicado en la cantidad de transacciones para generar una ganancia                                                                      |
| quantity                       | Varía en función de programa. Indica la cantidad facturada de programas transaccionales                                                            |
| earningType                    | Indica si se trata de cuota, el descuento, coop, venta etcetera.                                                                                          |
| earningAmount                  | Obtener el importe en la moneda de la transacción original                                                                                      |
| earningAmountUSD               | Obtener el importe en dólares estadounidenses                                                                                                                    |
| earningDate                    | Fecha de la obtención                                                                                                                      |
| calculationDate                | Fecha de que la obtención se calculó en el sistema                                                                                            |
| earningExchangeRate            | Tasa de cambio utilizada para mostrar la cantidad de dólares correspondiente                                                                                  |
| exchangeRateDate               | Fecha de tasa de cambio que se usa para calcular EarningAmount USD                                                                                   |
| claimId                        | Siempre estará en blanco                                                                                                                     |
| paymentId                      | Identificador único para el pago. Este número es normalmente visible en la instrucción de banco                                                 |
| paymentStatus                  | Estado del pago                                                                                                                           |
| paymentStatusDescription       | Descripción del estado de pago                                                                                                   |
| customerId                     | Siempre estará en blanco                                                                                                                     |
| customerName                   | Siempre estará en blanco                                                                                                                     |
| partNumber                     | Siempre estará en blanco                                                                                                                     |
| productName                    | Nombre de producto vinculado a la transacción                                                                                                       |
| productId                      | Identificador único del producto                                                                                                                |
| parentProductId                | Identificador único del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el identificador del producto principal es igual al identificador del producto. |
| parentProductName              | Nombre del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el nombre del producto principal es igual al nombre del producto.   |
| productType                    | Tipo de producto (por ejemplo aplicación, complemento, juego, etc.)                                                                                        |
| invoiceNumber                  | Siempre estará en blanco                                                                                                                     |
| subscriptionId                 | Siempre estará en blanco                                                                                                                     |
| subscriptionStartDate          | Siempre estará en blanco                                                                                                                     |
| subscriptionEndDate            | Siempre estará en blanco                                                                                                                     |
| resellerId                     | Siempre estará en blanco                                                                                                                     |
| resellerName                   | Siempre estará en blanco                                                                                                                     |
| distributorId                  | Siempre estará en blanco                                                                                                                     |
| distributorName                | Siempre estará en blanco                                                                                                                     |
| agreementNumber                | Siempre estará en blanco                                                                                                                     |
| agreementStartDate             | Siempre estará en blanco                                                                                                                     |
| agreementEndDate               | Siempre estará en blanco                                                                                                                     |
| carga de trabajo                       | Siempre estará en blanco                                                                                                                     |
| transactionType                | Tipo de transacción (por ejemplo, compra, reembolso, inversión, anulación, etc.)                                                               |
| localProviderSeller            | Proveedor o vendedor local de registro                                                                                                          |
| taxRemitted                    | Importe de impuestos enviados (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                                   |
| taxRemitModel                  | Parte responsable de la remesa de impuestos (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                    |
| storeFee                       | La cantidad que se conservan por Microsoft como una cuota para hacer que la aplicación o el complemento estén disponibles en el Store.                                           |
| transactionPaymentMethod       | Instrumento de pago del cliente usado para la transacción (por ejemplo, tarjeta, facturación del operador de telefonía móvil, PayPal, etc.)                                |
| tpan                           | Indica la red de ad de otros fabricantes                                                                                                     |
| purchaseTypeCode               | Siempre estará en blanco                                                                                                                     |
| purchaseOrderType              | Siempre estará en blanco                                                                                                                     |
| purchaseOrderCoverageStartDate | Siempre estará en blanco                                                                                                                     |
| purchaseOrderCoverageEndDate   | Siempre estará en blanco                                                                                                                     |
| externalReferenceId            | Siempre estará en blanco                                                                                                                     |
| externalReferenceIdLabel       | Siempre estará en blanco                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Exportación de descarga de instrucción de pago (heredado)

Durante un tiempo limitado en la página de resumen de pago anterior, las instrucciones de pago estará disponibles para su descarga. Este informe contiene los siguientes campos.

> [!NOTE]
> Historial de transacciones heredadas tiene una columna llamada "Reservado", que corresponde a la columna "Beneficios" en el historial de modernas, salvo que excluya todos los beneficios con estado = "Pago enviado".

| Nombre del campo              | Descripción                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fuente de ingresos          | La fuente de tus ingresos, que se basa en el lugar en el que se ha producido la transacción (por ejemplo, Microsoft Store, Windows Phone Store, Windows Store 8, publicidad, etc.)                  |
| Id. de pedido                | Un identificador de pedido exclusivo. Este id. permite identificar las transacciones de compra con sus respectivas transacciones que no son de compra (como las devoluciones, la anulaciones, etc.). Ambas tendrá el mismo id. de pedido. Además, en el caso de un cargo dividido en el que se hayan usado varios métodos de pago para una sola compra, te permitirá vincular las transacciones de compra. |
| Id. de transacción          | Identificador único de la transacción.                                                                                                                                          |
| Fecha y hora de transacción   | Fecha y hora en que se realizó la transacción (hora UTC).                                                                                                                       |
| Id. de producto principal       | Identificador único del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el identificador del producto principal es igual al identificador del producto.                                |
| Id. del producto              | Identificador único del producto.                                                                                                                                              |
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
