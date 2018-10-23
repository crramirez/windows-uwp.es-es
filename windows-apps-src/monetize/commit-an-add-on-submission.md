---
author: Xansky
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: Usa este método en la API de envío de Microsoft Store para confirmar un envío de complemento nuevo o actualizado al Centro de desarrollo de Windows.
title: Confirmar un envío de complemento
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, commit add-on submission, confirmar envío de complemento, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: b9d20bffe2be163db568af0b16bdfef8cd600271
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "5430826"
---
# <a name="commit-an-add-on-submission"></a>Confirmar un envío de complemento

Usa este método en la API de envío de Microsoft Store para confirmar un envío de complementos (también conocidos como productos desde la aplicación o IAP) nuevo o actualizado al Centro de desarrollo de Windows. La acción de confirmación avisa al Centro de desarrollo de que los datos del envío se han cargado (incluidos los iconos relacionados). En respuesta, el Centro de desarrollo confirma los cambios en los datos de envío para la recopilación y la publicación. Después de que la operación de confirmación se realice correctamente, los cambios en el envío se muestran en el panel del Centro de desarrollo.

Para obtener más información sobre cómo se ajusta la operación de confirmación en el proceso de envío de un complemento mediante la API de envío de Microsoft Store, consulta [Administrar envíos de complementos](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* [Crea un envío de complemento](create-an-add-on-submission.md) y posteriormente [actualiza el envío](update-an-add-on-submission.md) con cualquier modificación necesaria en los datos de envío.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadena | Obligatorio. El Id. de la Store del complemento que contiene el envío que deseas confirmar. El Id. de la Store está disponible en el panel del Centro de desarrollo, y se incluye en los datos de respuesta para las solicitudes de [Obtención de todos los complementos](get-all-add-ons.md) y [Creación de un complemento](create-an-add-on.md). |
| submissionId | cadena | Obligatorio. El identificador del envío que deseas confirmar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de complemento](create-an-add-on-submission.md). Para un envío creado en el panel del Centro de desarrollo, este id. también está disponible en la URL de la página de envío del panel.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo confirmar un envío de complemento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicación</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>Error de lanz.</li></ul>  |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el envío especificado. |
| 409  | Se encontró el envío especificado, pero no se ha podido confirmar en su estado actual o el complemento usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Artículos relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener un envío de complemento](get-an-add-on-submission.md)
* [Creación de un envío de complemento](create-an-add-on-submission.md)
* [Actualizar un envío de complemento](update-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
* [Get the status of an add-on submission (Obtener el estado de un envío de complemento)](get-status-for-an-add-on-submission.md)
