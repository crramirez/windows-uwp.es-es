---
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: Utilice este método en la API de envío de Microsoft Store para confirmar el envío de un complemento nuevo o actualizado al centro de partners.
title: Commit an add-on submission (Confirmar un envío de complemento)
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, commit add-on submission, confirmar envío de complemento, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: efab4412486566ae817eb66e78f5407533a30d5b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608220"
---
# <a name="commit-an-add-on-submission"></a>Commit an add-on submission (Confirmar un envío de complemento)

Utilice este método en la API de envío de Microsoft Store para confirmar el envío de un complemento nuevo o actualizado (también conocido como aplicación producto o IAP) al centro de partners. El centro de partners alertas de confirmación acción se que se ha cargado los datos de envío (incluidos los iconos relacionados). En respuesta, el centro de partners confirma los cambios a los datos de envío para ingesta y la publicación. Una vez finalizada correctamente la operación de confirmación, los cambios realizados en el envío se muestran en el centro de partners.

Para obtener más información sobre cómo se ajusta la operación de confirmación en el proceso de envío de un complemento mediante la API de envío de Microsoft Store, consulta [Administrar envíos de complementos](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* [Crea un envío de complemento](create-an-add-on-submission.md) y posteriormente [actualiza el envío](update-an-add-on-submission.md) con cualquier modificación necesaria en los datos de envío.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obligatorio. El Id. de la Tienda del complemento que contiene el envío que deseas confirmar. El identificador de Store está disponible en el centro de partners, y se incluye en los datos de respuesta para las solicitudes a [obtener todos los complementos](get-all-add-ons.md) y [crear un complemento](create-an-add-on.md). |
| submissionId | string | Obligatorio. El identificador del envío que deseas confirmar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de complemento](create-an-add-on-submission.md). Para un envío que se creó en el centro de partners, este identificador también está disponible en la dirección URL de la página de envío en el centro de partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo confirmar un envío de complemento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Publicación</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el envío especificado. |
| 409  | Se encontró el envío especificado, pero no pudo confirmarse en su estado actual o el complemento utiliza una característica de centro de partners que es [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener la presentación de un complemento](get-an-add-on-submission.md)
* [Crear una presentación de complemento](create-an-add-on-submission.md)
* [Actualizar un complemento de envío](update-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
* [Obtener el estado del envío de un complemento](get-status-for-an-add-on-submission.md)
