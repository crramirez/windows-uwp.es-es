---
Description: Una vez que hayas creado tu aplicación reservando un nombre, puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un envío.
title: Envíos de aplicaciones
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de comprobación, windows, uwp, envío, enviar, juego, aplicación, enviar
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bed7f232c8ec59771c6ae80a48b12bab1307ad68
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260024"
---
# <a name="app-submissions"></a>Envíos de aplicaciones


Una vez que hayas [creado tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md), puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un **envío**.

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. Updates you make to your submission are saved, so you can come back and work on it whenever you're ready.

> [!NOTE]
> You must have an active [developer account](https://developer.microsoft.com/store/register) in [Partner Center](https://partner.microsoft.com/dashboard) in order to submit apps to the Microsoft Store.

After your app is published, you can publish an updated version by creating another submission in Partner Center. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. To create a new submission for a published app, click **Update** next to the most recent submission shown on its **Overview** page. You can also [remove an app from the Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) if you need to do so (and then make it available again later, if you'd like).

> [!NOTE]
> This section of the documentation describes how to create an app submission in Partner Center. Como alternativa, puedes usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

> [!IMPORTANT]
> As of October 31, 2018, newly-created products cannot include packages targeting Windows 8.x/Windows Phone 8.x or earlier. For more info, see this [blog post](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Lista de comprobación de envío de aplicaciones

A continuación se incluyen los detalles que puedes proporcionar al crear el envío de tu aplicación, con vínculos a más información.

Los elementos que debes proporcionar o especificar se indican a continuación. Algunas áreas son opcionales o tienen valores predeterminados proporcionados que puedes cambiar según lo desees. You don't have to work on these sections in the order listed here.

### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad
| Nombre del campo                    | Notas                                       | Más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Markets**                   | Opción predeterminada: Todos los mercados posibles  | [Define pricing and market selection](define-pricing-and-market-selection.md)         |
| **Audience**                | Valor predeterminado: Audiencia pública | [Audience](choose-visibility-options.md#audience) |
| **Discoverability**                | Valor predeterminado: Hacer esta aplicación disponible y detectable en Store | [Discoverability](choose-visibility-options.md#discoverability) |
| **Schedule**                  | Opción predeterminada: Se lanzará lo antes posible        | [Configure precise release scheduling](configure-precise-release-scheduling.md) |
| **Base price**                | Necesario                                    | [Set and schedule app pricing](set-and-schedule-app-pricing.md)              |
| **Prueba gratuita**                | Predeterminado: sin prueba gratuita                      | [Prueba gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Precio de oferta**              | Opcional                                    | [Poner aplicaciones y complementos a la venta](put-apps-and-add-ons-on-sale.md)           |
| **Organizational licensing**    | Valor predeterminado: Permitir que las organizaciones adquieran licencias por volumen | [Organizational licensing options](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                                       | Más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Category and subcategory**  | Necesario                                    | [Category and subcategory table](category-and-subcategory-table.md)       |
| **Privacy policy URL**            | Obligatorio para muchas aplicaciones. Consulta el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](store-policies.md#105-personal-information) | [Privacy policy URL](enter-app-properties.md#privacy-policy-url)        |
| **Website**                   | Opcional                                    | [Website](enter-app-properties.md#website)                   |
| **Support contact info**      | Obligatorio si tu producto está disponible en Xbox; de lo contrario, opcional (pero recomendado)                                   | [Support contact info](enter-app-properties.md#support-contact-info)              |
| **Game settings**             | Opcional (solo aplicable a los juegos)         | [Game settings](enter-app-properties.md#game-settings) |
| **Display mode**             | Opcional                   | [Display mode](enter-app-properties.md#display-mode) |
| **Product declarations**          | Predeterminado: Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble; Windows puede incluir datos de la aplicación en las copias de seguridad automáticas en OneDrive | [Product declarations](app-declarations.md) |
| **Requisitos del sistema**      | Opcional                                    | [Requisitos del sistema](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Página de clasificación por edades

| Nombre del campo                    | Notas                                       | Más información                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Age ratings**               | Necesario                                    | [Age ratings](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de paquetes

| Nombre del campo                    | Notas                                  | Más información                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package upload control**    | Obligatorio (al menos un paquete)        | [Upload app packages](upload-app-packages.md) |
| **Device family availability** | Valor predeterminado: En función de los paquetes       | [Device family availability](device-family-availability.md) |
| **Gradual package rollout**   | Opcional (solo para actualizaciones)            | [Gradual package rollout](gradual-package-rollout.md) |
| **Mandatory update**          | Opcional (solo para actualizaciones)            | [Mandatory update](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descripciones de Store

Necesitarás toda la información necesaria como mínimo para uno de los idiomas que admita la aplicación. Te recomendamos que proporciones [descripciones de Store](create-app-store-listings.md) en todos los idiomas que admita la aplicación, y también puedes [proporcionar descripciones de Store en idiomas adicionales](create-app-store-listings.md#store-listing-languages). Para que sea más fácil administrar descripciones múltiples del mismo producto, puedes [importar y exportar descripciones de Store](import-and-export-store-listings.md).

| Nombre del campo                    | Notas                                       | Más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descripción**               | Necesario                                    | [Write a great app description](write-a-great-app-description.md) |
| **What's new in this version**   | Opcional                                 | [Notas de la versión](create-app-store-listings.md#whats-new-in-this-version)       |
| **App features**              | Opcional                                    | [Product features](create-app-store-listings.md#product-features)         |
| **Screenshots**               | Obligatorio (al menos una captura de pantalla; se recomienda que sean cuatro o más)          | [Screenshots](app-screenshots-and-images.md#screenshots)          |
| **Store logos**               | Recomendado; necesario para algunas versiones de sistemas operativos | [Store logos](app-screenshots-and-images.md#store-logos)             |
| **Trailers**                  | Opcional                                    | [Trailers](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 and Xbox image (16:9 Super hero art)**     | Recomendaciones        | [Windows 10 and Xbox image (16:9 Super hero art)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox images**     | Required for proper display if you publish to Xbox        | [Xbox images](app-screenshots-and-images.md#xbox-images) |
| **Supplemental fields**  | Opcional                                    | [Supplemental fields](create-app-store-listings.md#supplemental-fields) 
| **Search terms**              | Opcional                                    | [Search terms](create-app-store-listings.md#search-terms)         |
| **Copyright and trademark info** | Opcional                                 | [Copyright and trademark info](create-app-store-listings.md#copyright-and-trademark-info) |
| **Additional license terms**  | Opcional                                    | [Additional license terms](create-app-store-listings.md#additional-license-terms) |
| **Developed by**              | Opcional                                    | [Developed by](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página de opciones de envío

| Nombre del campo                    | Notas                                       | Más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Publishing hold options**     | Valor predeterminado: Publicar este envío tan pronto como supere la certificación (o siguiendo las fechas seleccionadas en la sección Programación)      | [Publishing hold options](manage-submission-options.md#publishing-hold-options)    
| **Notes for certification**     | Recomendaciones          | [Notes for certification](notes-for-certification.md)             |
| **Restricted capabilities**     | Required if your product declares any [restricted capabilities](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Restricted capabilities](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obtener información sobre cómo publicar aplicaciones de línea de negocio (LOB) directamente para empresas, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).
