---
ms.assetid: 55315F38-6EC5-4889-A14E-7D8EC282FE98
description: Usa este método en la API de envío de Microsoft Store para obtener el estado de un envío de complemento.
title: Obtener el estado de un envío de complemento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, add-on submission, envío de complemento, status, estado
ms.localizationpriority: medium
ms.openlocfilehash: 1bfec8232fe8e410e65997098954e35d3f5fdc1b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590620"
---
# <a name="get-the-status-of-an-add-on-submission"></a>Obtener el estado de un envío de complemento

Usa este método en la API de envío de Microsoft Store para obtener el estado de un complemento (también conocido como producto desde la aplicación o IAP) existente. Para obtener más información sobre el proceso de creación del envío de un complemento mediante la API de envío de Microsoft Store, consulta [Administrar envíos de complemento](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Creación de un envío de complemento de uno de sus aplicaciones. Puede hacerlo en el centro de partners, o puede hacerlo mediante el uso de la [crear una presentación de complemento](create-an-add-on-submission.md) método.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/status``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obligatorio. El Id. de la Tienda del complemento que contiene el envío cuyo estado quieres obtener. El identificador de Store está disponible en el centro de partners.  |
| submissionId | string | Obligatorio. El identificador del envío para el que quieres obtener el estado. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de complemento](create-an-add-on-submission.md). Para un envío que se creó en el centro de partners, este identificador también está disponible en la dirección URL de la página de envío en el centro de partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo obtener el estado de un envío de complemento.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el envío especificado. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Publicación</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores. Para obtener más información, consulta [Recurso de detalles de estado](manage-add-on-submissions.md#status-details-object). |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío. |
| 409  | El complemento utiliza una característica de centro de partners que es [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener la presentación de un complemento](get-an-add-on-submission.md)
* [Crear una presentación de complemento](create-an-add-on-submission.md)
* [Confirmar la presentación de un complemento](commit-an-add-on-submission.md)
* [Actualizar un complemento de envío](update-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
