---
description: Para recibir dinero de las ventas de las aplicaciones en el Microsoft Store, debe configurar la cuenta de pago y rellenar los formularios fiscales necesarios.
title: Configuración de la cuenta de pago y los formularios de impuestos
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 1/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 563e8e5df010d869183fb0a3c734eaae521a759e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030278"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>Configuración de la cuenta de pago y los formularios de impuestos

> [!NOTE]
> Si busca soporte técnico en relación con los pagos, incluidos la configuración de cuentas de pago, los pagos que faltan, la puesta en espera de los pagos o cualquier otra cosa, póngase en contacto con el servicio de soporte técnico [aquí](https://partner.microsoft.com/support).

Para recibir dinero de las ventas de las aplicaciones en el Microsoft Store, debe configurar la cuenta de pago y rellenar los formularios fiscales necesarios en el [centro de Partners](https://partner.microsoft.com/dashboard).

Si solo tienes pensado anunciar aplicaciones gratuitas (y no piensas ofrecer compras desde la aplicación ni usar Microsoft Advertising), no necesitas configurar una cuenta de pago ni rellenar ningún formulario fiscal. Si cambia de opinión más adelante y decide que desea vender aplicaciones (o complementos), puede configurar la cuenta de pago y rellenar los formularios fiscales en ese momento. No podrá enviar ningún complemento o aplicación de pago hasta que se haya completado su cuenta de pago y el perfil fiscal.

> [!NOTE]
> En [determinados mercados](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), los desarrolladores solo pueden enviar aplicaciones gratuitas. Si su cuenta está registrada en uno de estos mercados, no tendrá la opción de configurar una cuenta de pago.

Una vez que haya [configurado la cuenta de desarrollador](opening-a-developer-account.md), debe hacer dos cosas antes de poder vender aplicaciones (o complementos) en el Microsoft Store:

- [Rellenado de los formularios de impuestos](#tax-forms)
- [Configuración de la cuenta de pago](#payout-account)

> [!NOTE]
> Para obtener más información sobre cómo y cuándo se pagará por el dinero que realizan las aplicaciones, consulte el artículo sobre el [pago](getting-paid-apps.md).

## <a name="tax-forms"></a>Formularios de impuestos

### <a name="filling-out-your-tax-forms"></a>Rellenado de los formularios de impuestos

En primer lugar, deberá crear un perfil fiscal y asignarlo a los programas en los que participe. Puede crear su *perfil fiscal* para el Microsoft Store completando los pasos siguientes:

- Especifique el país de residencia y la nacionalidad.
- Rellene los formularios fiscales adecuados.

Puede completar y enviar los formularios fiscales electrónicamente desde el Centro de partners. En la mayoría de los casos, no es necesario imprimir y enviar por correo ningún formulario.

> [!IMPORTANT]
> Los distintos países y regiones tienen requisitos fiscales diferentes. El importe exacto que debes pagar en impuestos depende de los países y las regiones donde vendas tus aplicaciones. Consulta el [Acuerdo de desarrollador de aplicaciones](/legal/windows/agreements/app-developer-agreement) para conocer en qué países o regiones Microsoft remite ventas e IVA de importación en tu nombre. En otros países o regiones, según el lugar donde te hayas registrado, es posible que debas remitir las ventas y el IVA de importación de tus ventas de aplicaciones directamente a la autoridad fiscal local. Además, los ingresos por ventas de la aplicación que recibe pueden estar sujetos a impuestos como ingresos. Le recomendamos encarecidamente que se ponga en contacto con la autoridad correspondiente de su país o región, que puede ayudarle a determinar la información fiscal adecuada para sus actividades de desarrollador de Microsoft Store.

1. En el [centro de Partners](https://partner.microsoft.com/dashboard), seleccione el icono de configuración de la **cuenta** en la esquina superior derecha y, a continuación, seleccione Configuración del **desarrollador** .
2. En el menú de navegación izquierdo, seleccione **Pago e impuestos** y, a continuación, seleccione **Asignaciones de perfiles fiscales y de pago** .

    ![Asignación de Perfil de pago y de impuestos](images/payout-tax-profile-assignment.png)

3. Seleccione el programa y la combinación de ID. de vendedor para los que desea configurar la información fiscal.

    ![Pago-seleccionar ID. de vendedor](images/payout-select-seller-id.png)

4. Si desea utilizar un perfil fiscal existente, selecciónelo en la lista desplegable. En caso contrario, seleccione **Crear nuevo perfil** y presione **Enviar** . Accederá a la página de perfiles fiscales.
5. Haga clic en el botón **Editar** para editar la información fiscal.
6. Seleccione el botón de radio adecuado, así como su país si se le solicita. Este paso determina la entidad empresarial de Microsoft que se usará para realizar pagos en su cuenta.

    ![País de selección fiscal de pago](images/payout-select-tax-country.png)

7. En función de las selecciones realizadas el paso 6, se le pedirá que proporcione la información fiscal necesaria para su país.

> [!NOTE]
> Sea cual sea su país de residencia o nacionalidad, debe rellenar Estados Unidos formularios fiscales para vender aplicaciones o complementos a través de la Microsoft Store. Los desarrolladores que cumplan con ciertos requisitos de residencia de los Estados Unidos deben completar un formulario IRS W-9. Otros desarrolladores que se encuentran fuera de los Estados Unidos deben completar un formulario IRS W-8. Puede rellenar estos formularios en línea a medida que completa su perfil fiscal.

### <a name="withholding-rates"></a>Tasas de retención

La información que envíe en las declaraciones de impuestos determinará la tasa de retención fiscal adecuada. La tasa de retención se aplica solo a las ventas que se realizan en Estados Unidos. Las ventas realizadas en ubicaciones fuera de Estados Unidos no están sujetas a retenciones. Las tasas de retención varían, pero para la mayoría de los desarrolladores que están registrados fuera de Estados Unidos, la tasa predeterminada es del 30 %. Podrá reducir esta tasa si su país ha firmado con Estados Unidos un tratado tributario por ingresos especial.

### <a name="tax-treaty-benefits"></a>Beneficios por tratado tributario

Si reside fuera de Estados Unidos, puede aprovechar los beneficios por tratados tributarios. Estas ventajas varían de un país a un país y pueden permitirle reducir la cantidad de impuestos que el Microsoft Store retiene. Puede reclamar los beneficios por tratado tributario rellenando la Parte II del formulario W-8BEN. Es recomendable que se ponga en contacto con los recursos adecuados de su país o región para determinar si estos beneficios se le pueden aplicar.

> [!NOTE]
> No es necesario tener el número de identificación fiscal individual (ITIN) de Estados Unidos para recibir pagos de Microsoft o para reclamar beneficios del tratado fiscal.

## <a name="payout-account"></a>Cuenta de pago

Una cuenta de pago es la cuenta bancaria a la que se envían los ingresos de sus ventas. Puede ver todas las cuentas de pago que ha especificado en la página Perfil.

> [!NOTE]
> En algunos mercados, se puede usar PayPal para su cuenta de pago. Consulte [umbrales de pago, métodos y períodos de tiempo](payment-thresholds-methods-and-timeframes.md) para averiguar si se admite PayPal para un mercado específico y lea la [información de PayPal](#paypal-info) siguiente para obtener más información.

### <a name="create-a-payment-profile"></a>Creación de un perfil de pago

1. En el [centro de Partners](https://partner.microsoft.com/dashboard), seleccione el icono de engranaje **configuración** en la esquina superior derecha y, a continuación, seleccione Configuración del **desarrollador** .
2. Debajo del encabezado *Pago e impuestos* fiscal, seleccione **Asignación de perfiles fiscales y de pago** .

    > [!NOTE]
    > Dado que esta información es confidencial, es posible que se le pida que inicie sesión de nuevo.

3. Seleccione el método de pago que quiere configurar.

    ![Selección de tipo de cuenta de pago](images/payout-account-type-selection.png)

4. Seleccione un perfil de pago existente o haga clic en **Crear un perfil de pago** para crear un nuevo perfil para el método de pago elegido.

> [!NOTE]
> Si, por alguna razón, su cuenta no está lista para recibir fondos de Microsoft, puede activar la casilla **suspender mi pago** . Continuará ganando a partir de las ventas, pero los pagos no se distribuirán hasta que deshabilite **la retención del pago.**

### <a name="create-a-bank-based-payment-profile"></a>Creación de un perfil de pago basado en el banco

Si ha elegido usar una cuenta bancaria para recibir pagos, deberá completar el siguiente proceso para configurar su cuenta.

1. En la página *Bank Profile* (Perfil de banco), proporcione la información necesaria sobre su banco.
2. Especifique la información de su cuenta bancaria.

    > [!NOTE]
    > Los campos que se usan para proporcionar la información de la cuenta aceptan solo caracteres alfanuméricos.

    ![Información del Banco de pago](images/payout-bank-info.png)

3. Proporcione los detalles del beneficiario.
4. De nuevo en la página de *asignación de perfil* , seleccione la moneda que desea usar cuando emita sus pagos.

    > [!WARNING]
    > Asegúrese de que el banco acepte la moneda de pago que seleccione.

5. Tendrá que seleccionar un perfil de pago para cada programa en el que participe, aunque puede usar el mismo perfil para varios programas.

    ![Perfil de banco de uso de pago](images/payout-use-bank-profile.png)

6. Haga clic en Enviar para guardar los cambios.

> [!NOTE]
> Microsoft puede tardar hasta 48 horas en validar la información de su perfil. Cuando se complete este proceso, *Estado de la comprobación* mostrará **Completado** .

Para garantizar que el pago se realiza correctamente, ten en cuenta los siguientes puntos:

- El **nombre del titular de la cuenta** especificado para su cuenta de pago en el Centro de partners debe ser exactamente el mismo nombre asociado a su cuenta bancaria. Por ejemplo, si el nombre de la cuenta bancaria contiene un segundo nombre, agregue este segundo nombre al **nombre del titular de la cuenta** .
- Los pagos se transfieren directamente de Microsoft a su cuenta bancaria en moneda USD.
- La información bancaria introducida en el Centro de partners en caracteres latinos se traduce en caracteres cirílicos.

### <a name="editing-existing-payment-profiles"></a>Edición de perfiles de pago existentes

Puede editar los perfiles de pago existentes si necesita realizar cambios o corregir cualquier información incorrecta.

1. En el [centro de Partners](https://partner.microsoft.com/dashboard), seleccione el icono de engranaje **configuración** en la esquina superior derecha y, a continuación, seleccione Configuración del **desarrollador** .
2. Debajo del encabezado *Pago e impuestos* fiscal, seleccione **Perfiles fiscales y de pago** .
3. Los perfiles de pago se mostrarán junto con su estado. Busque el perfil que desea editar y haga clic en **Editar** en el extremo derecho.

> [!IMPORTANT]
> El hecho de cambiar la cuenta de pago puede dar lugar a retrasos en los pagos de hasta un ciclo completo. Este retraso se produce porque es necesario comprobar el cambio de la cuenta, al igual que hicimos cuando configuró por primera vez la cuenta de pago. Aunque se le seguirá pagando el importe completo después de que se haya comprobado la cuenta, los pagos pendientes del ciclo actual se agregarán al siguiente. Consulte [Recepción del pago](getting-paid-apps.md) para más información.

### <a name="paypal-info"></a>Información de PayPal

En la selección de países y regiones, puede crear una cuenta de pago indicando su información de PayPal. Sin embargo, antes de elegir PayPal como opción de cuenta de pago:

- Consulte [Umbrales, métodos y períodos de tiempo de pago](payment-thresholds-methods-and-timeframes.md) para confirmar si PayPal es un método de pago admitido en su país o región.
- Consulte las preguntas frecuentes siguientes. En función de su situación, es posible que PayPal no sea la mejor opción de la cuenta de pago y que se prefiera una cuenta bancaria.

Preguntas comunes sobre el uso de PayPal como método de pago:

- **¿Qué configuración de PayPal necesito tener para recibir pagos?** Debes asegurarte de que tu cuenta de PayPal no bloquea los pagos con cheque electrónico (eCheck). Esta configuración se administra en la página de preferencias de recepción de pagos de PayPal. Consulte la [página de configuración de la cuenta de PayPal](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/) para más información.
- **¿Se admite mi país o región?** Consulte [Umbrales, métodos y períodos de tiempo de pago](payment-thresholds-methods-and-timeframes.md) para averiguar dónde se admite PayPal como método de pago.
- **¿Mi cuenta de PayPal tiene que estar registrada en el mismo país o región que mi cuenta del Centro de partners?** No. Al configurar una cuenta de PayPal, puede aceptar la configuración predeterminada. No debe tener ningún problema con otros países o regiones y monedas a menos que haya bloqueado el pago en algunas monedas. Esta configuración se administra en la página de preferencias de recepción de pagos de PayPal.
- **¿Tengo que aceptar los pagos de PayPal manualmente?** No. De forma predeterminada, las cuentas de PayPal se establecen para requerir que los usuarios acepten los pagos manualmente, lo que significa que, si no acepta el pago en un plazo de 30 días, se devuelve. Para cambiar esta configuración, desactive "Ask Me" (Preguntar) en la página More Settings (Más opciones) de PayPal.
- **¿Qué monedas admite PayPal?** Consulte la [Página de soporte de PayPal](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) para ver la lista actual.

### <a name="specific-requirements-for-certain-countriesregions"></a>Requisitos específicos para determinados países o regiones

En algunos países y regiones, se deben seguir los requisitos adicionales para las cuentas de pago. Si resides en Pakistán, Rusia o Ucrania, ten en cuenta los requisitos siguientes.

#### <a name="pakistan"></a>Pakistán

El formulario Form-R es un requisito de cumplimiento normativo de Pakistán. Se utiliza para indicar el propósito y el motivo de la recepción de fondos del extranjero. Por lo tanto, siempre que sea válido para un pago mensual de Microsoft, tendrá que enviar un formulario Form-R al banco antes de que se pueda publicar el pago en su cuenta. Póngase en contacto con la sucursal de su banco local para obtener instrucciones sobre cómo obtener una copia de Form-R.

Tendrá que enviar un formulario Form-R al banco cada mes que sea elegible para un pago. Por ejemplo, si espera recibir un pago cada mes del año, tendrá que enviar un formulario Form-R doce veces (una vez al mes).

Una vez enviado el pago a su banco, tiene 30 días para enviar un formulario Form-R. Si no se envía en un plazo de 30 días, los fondos se devolverán a Microsoft.

#### <a name="russia"></a>Rusia

Si eres un desarrollador que vive en Rusia, puede que tengas que proporcionar documentación a tu banco antes de que este ingrese un importe en tu cuenta. Cuando sea posible el pago, le proporcionaremos la siguiente documentación en un mensaje de correo electrónico:

1. Certificado de recepción (AC): contiene la cantidad de pagos que se transfieren a su cuenta.
2. Contrato de desarrollador de aplicaciones: una copia firmada del acuerdo para desarrolladores que debe estar refrendada.

Para garantizar que el pago se realiza correctamente, ten en cuenta los siguientes puntos:

- El **nombre del titular de la cuenta** especificado para su cuenta de pago en el Centro de partners debe ser exactamente el mismo nombre asociado a su cuenta bancaria. Por ejemplo, si el nombre de la cuenta bancaria contiene un segundo nombre, agregue este segundo nombre al **nombre del titular de la cuenta** .
- Los pagos se transfieren directamente de Microsoft a su cuenta bancaria en rublos (RUB).
- La información bancaria introducida en el Centro de partners en caracteres latinos se traduce en caracteres cirílicos.
- Los pagos deben realizarse en una cuenta bancaria y no en una tarjeta bancaria.

#### <a name="ukraine"></a>Ucrania

Si eres un desarrollador que vive en Ucrania, puede que tengas que proporcionar documentación a tu banco antes de que este ingrese un importe en tu cuenta. Cuando sea posible el pago, le proporcionaremos la siguiente documentación en un mensaje de correo electrónico:

1. Certificado de recepción (AC): contiene la cantidad de pagos que se transfieren a su cuenta.
2. Contrato de desarrollador de aplicaciones: una copia firmada del acuerdo para desarrolladores que debe estar refrendada.
3. Acuerdo rectificativo (AA): este documento lo puede usar el banco para ayudar a identificar los fondos de pago.

Microsoft proporciona los tres documentos cuando se va a realizar el primer pago. Para los pagos posteriores, solo recibirá el documento AC. Guarda los documentos ADA y AA. Podrías necesitarlos para recibir pagos de tu banco en el futuro.

### <a name="create-a-paypal-payment-profile"></a>Creación de un perfil de pago de PayPal

Si ha elegido usar una cuenta bancaria para recibir pagos, deberá completar el siguiente proceso para configurar su cuenta.

1. En la página de *PayPal* , proporcione la información necesaria sobre su cuenta de PayPal.
2. Proporcione los detalles de su cuenta de PayPal.

    > [!NOTE]
    > Los campos que se usan para proporcionar la información de la cuenta aceptan solo caracteres alfanuméricos.

    ![Información de pago de PayPal](images/payout-paypal-info.png)

3. Proporcione los detalles del beneficiario.
4. De nuevo en la página de *asignación de perfil* , seleccione la moneda que desea usar cuando emita sus pagos.
5. Tendrá que seleccionar un perfil de pago para cada programa en el que participe, aunque puede usar el mismo perfil para varios programas.
6. Haga clic en Enviar para guardar los cambios.
