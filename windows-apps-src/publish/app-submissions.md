---
Description: Una vez que hayas creado tu aplicación reservando un nombre, puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un envío.
title: Envíos de aplicaciones
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de comprobación, windows, uwp, envío, enviar, juego, aplicación, enviar
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7c535b77c68f4375f70fe344165f96d66a551eaf
ms.sourcegitcommit: d8ce1a25ac0373acafb394837eb5c0737f6efec8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486423"
---
# <a name="app-submissions"></a>Envíos de aplicaciones


Una vez que hayas [creado tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md), puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un **envío**.

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. Se guardan las actualizaciones que realice para su envío, por lo que puede regresar y trabajar en él cuando esté listo.

> [!NOTE]
> Debe tener activa una [cuenta de desarrollador](https://go.microsoft.com/fwlink/p/?LinkId=615100) en [centro de partners](https://partner.microsoft.com/dashboard) con el fin de enviar aplicaciones a la Microsoft Store.

Una vez publicada la aplicación, puede publicar una versión actualizada mediante la creación de otro envío en el centro de partners. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. Para crear un nuevo envío de una aplicación publicada, haga clic en **actualización** al lado de envío más reciente se muestra en su **Introducción** página. También puede [quitar una aplicación de la Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) si debe hacerlo (y, a continuación, ponerlo a disposición a intentarlo más tarde, si lo desea).

> [!NOTE]
> En esta sección de la documentación se describe cómo crear un envío de la aplicación en el centro de partners. Como alternativa, puedes usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, los productos recién creada no pueden incluir los paquetes destinados a 8.x/Windows Windows Phone 8.x o versiones anteriores. Para obtener más información, consulte este [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Lista de comprobación de envío de la aplicación

A continuación se incluyen los detalles que puedes proporcionar al crear el envío de tu aplicación, con vínculos a más información.

Los elementos que debes proporcionar o especificar se indican a continuación. Algunas áreas son opcionales o tienen valores predeterminados proporcionados que puedes cambiar según lo desees. No debe trabajar en estas secciones en el orden mostrado aquí.

### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad
| Nombre del campo                    | Notas                                       | Para obtener más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Markets**                   | Default: Todos los mercados posibles  | [Definir la selección de mercado y precios](define-pricing-and-market-selection.md)         |
| **Audiencia**                | Default: Audiencia pública | [Audiencia](choose-visibility-options.md#audience) |
| **Detectabilidad**                | Default: Hacer que esta aplicación disponible y pueda detectar en el Store | [Detectabilidad](choose-visibility-options.md#discoverability) |
| **Schedule**                  | Default: Versión tan pronto como sea posible        | [Configurar la programación de la versión exacta](configure-precise-release-scheduling.md) |
| **Precio base**                | Requerido                                    | [Establecer y programar los precios de la aplicación](set-and-schedule-app-pricing.md)              |
| **Prueba gratuita**                | Default: Sin prueba gratuita                      | [Prueba gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Precio de oferta**              | Opcional                                    | [Poner aplicaciones y complementos a la venta](put-apps-and-add-ons-on-sale.md)           |
| **Licencias para organizaciones**    | Default: Permitir la adquisición de volumen por organizaciones | [Opciones de licencia profesionales](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                                       | Para obtener más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoría y subcategoría**  | Requerido                                    | [Tabla de categoría y subcategoría](category-and-subcategory-table.md)       |
| **URL de la política de privacidad**            | Obligatorio para muchas aplicaciones. Consulta el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](store-policies.md#105-personal-information) | [URL de la política de privacidad](enter-app-properties.md#privacy-policy-url)        |
| **Sitio Web**                   | Opcional                                    | [Sitio Web](enter-app-properties.md#website)                   |
| **Información de contacto de soporte técnico**      | Obligatorio si tu producto está disponible en Xbox; de lo contrario, opcional (pero recomendado)                                   | [Información de contacto de soporte técnico](enter-app-properties.md#support-contact-info)              |
| **Configuración del juego**             | Opcional (solo aplicable a los juegos)         | [Configuración del juego](enter-app-properties.md#game-settings) |
| **Modo de presentación**             | Opcional                   | [Modo de presentación](enter-app-properties.md#display-mode) |
| **Declaraciones de producto**          | Default: Los clientes pueden instalar esta aplicación a las unidades alternativas o almacenamiento extraíble; Windows pueden incluir datos de esta aplicación en copias de seguridad automáticas en OneDrive | [Declaraciones de producto](app-declarations.md) |
| **Requisitos del sistema**      | Opcional                                    | [Requisitos del sistema](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Página de clasificación por edades

| Nombre del campo                    | Notas                                       | Para obtener más información                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Clasificaciones por edades**               | Requerido                                    | [Clasificaciones por edades](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de paquetes

| Nombre del campo                    | Notas                                  | Para obtener más información                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Control de carga de paquete**    | Obligatorio (al menos un paquete)        | [Cargar paquetes de aplicaciones](upload-app-packages.md) |
| **Disponibilidad de familia de dispositivos** | Valor predeterminado: En función de los paquetes       | [Disponibilidad de familia de dispositivos](device-family-availability.md) |
| **Implementación gradual de paquete**   | Opcional (solo para actualizaciones)            | [Implementación gradual de paquete](gradual-package-rollout.md) |
| **Actualización obligatoria**          | Opcional (solo para actualizaciones)            | [Actualización obligatoria](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descripciones de la Tienda

Necesitarás toda la información necesaria como mínimo para uno de los idiomas que admita la aplicación. Te recomendamos que proporciones [descripciones de la Tienda](create-app-store-listings.md) en todos los idiomas que admita la aplicación, y también puedes [proporcionar descripciones de la Tienda en idiomas adicionales](create-app-store-listings.md#store-listing-languages). Para que sea más fácil administrar descripciones múltiples del mismo producto, puedes [importar y exportar descripciones de Store](import-and-export-store-listings.md).

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descripción**               | Requerido                                    | [Escriba una descripción excelente aplicación](write-a-great-app-description.md) |
| **Novedades de esta versión**   | Opcional                                 | [Notas de la versión](create-app-store-listings.md#whats-new-in-this-version)       |
| **Características de la aplicación**              | Opcional                                    | [Características del producto](create-app-store-listings.md#product-features)         |
| **Capturas de pantalla**               | Obligatorio (al menos una captura de pantalla; se recomienda que sean cuatro o más)          | [Capturas de pantalla](app-screenshots-and-images.md#screenshots)          |
| **Logotipos de Store**               | Recomendado; necesario para algunas versiones de sistemas operativos | [Logotipos de Store](app-screenshots-and-images.md#store-logos)             |
| **Finalizadores**                  | Opcional                                    | [Finalizadores](app-screenshots-and-images.md#trailers)                | 
| **Imagen de Windows 10 y Xbox (art héroe de 16:9)**     | Recomendado        | [Imagen de Windows 10 y Xbox (art héroe de 16:9)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Imágenes de Xbox**     | Necesario para la visualización adecuada si publica en Xbox        | [Imágenes de Xbox](app-screenshots-and-images.md#xbox-images) |
| **Campos adicionales**  | Opcional                                    | [Campos adicionales](create-app-store-listings.md#supplemental-fields) 
| **Términos de búsqueda**              | Opcional                                    | [Términos de búsqueda](create-app-store-listings.md#search-terms)         |
| **Información de copyright y marca comercial** | Opcional                                 | [Información de copyright y marca comercial](create-app-store-listings.md#copyright-and-trademark-info) |
| **Términos de licencia adicionales**  | Opcional                                    | [Términos de licencia adicionales](create-app-store-listings.md#additional-license-terms) |
| **Desarrollado por**              | Opcional                                    | [Desarrollado por](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página de opciones de envío

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opciones de retención de publicación**     | Default: Publicar este envío tan pronto como pasa la entidad (o por fechas seleccionó en la sección de programación)      | [Opciones de retención de publicación](manage-submission-options.md#publishing-hold-options)    
| **Notas de la certificación**     | Recomendado          | [Notas de la certificación](notes-for-certification.md)             |
| **Funciones restringidas**     | Requerido si su producto declara cualquier [capacidades restringidas](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Funciones restringidas](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obtener información sobre cómo publicar aplicaciones de línea de negocio (LOB) directamente para empresas, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).
