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

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. Las actualizaciones que realice en el envío se guardarán, por lo que puede volver y trabajar en ella siempre que esté listo.

> [!NOTE]
> Debe tener una cuenta de [desarrollador](https://developer.microsoft.com/store/register) activa en el [centro de Partners](https://partner.microsoft.com/dashboard) para poder enviar aplicaciones a la Microsoft Store.

Una vez publicada la aplicación, puede publicar una versión actualizada mediante la creación de otro envío en el centro de Partners. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. Para crear un nuevo envío para una aplicación publicada, haga clic en **Actualizar** junto al envío más reciente que se muestra en su página de **información general** . También puede [quitar una aplicación de la tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store) si tiene que hacerlo (y, a continuación, volver a estar disponible más adelante, si lo desea).

> [!NOTE]
> En esta sección de la documentación se describe cómo crear un envío de aplicación en el centro de Partners. Como alternativa, puedes usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, los productos recién creados no pueden incluir paquetes destinados a Windows 8. x/Windows Phone 8. x o una versión anterior. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Lista de comprobación de envío de la aplicación

A continuación se incluyen los detalles que puedes proporcionar al crear el envío de tu aplicación, con vínculos a más información.

Los elementos que debes proporcionar o especificar se indican a continuación. Algunas áreas son opcionales o tienen valores predeterminados proporcionados que puedes cambiar según lo desees. No tiene que trabajar en estas secciones en el orden que se muestra aquí.

### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad
| Nombre del campo                    | Notas                                       | Para obtener más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercado**                   | Opción predeterminada: Todos los mercados posibles  | [Definir precios y selección de mercado](define-pricing-and-market-selection.md)         |
| **Dirigido**                | Valor predeterminado: Audiencia pública | [Dirigido](choose-visibility-options.md#audience) |
| **Detectabilidad**                | Valor predeterminado: Hacer esta aplicación disponible y detectable en Store | [Detectabilidad](choose-visibility-options.md#discoverability) |
| **Planea**                  | Opción predeterminada: Se lanzará lo antes posible        | [Configuración de la programación de versiones precisa](configure-precise-release-scheduling.md) |
| **Precio base**                | Requerido                                    | [Establecimiento y programación de precios de aplicaciones](set-and-schedule-app-pricing.md)              |
| **Prueba gratuita**                | Valor predeterminado: sin prueba gratuita                      | [Prueba gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Precio de oferta**              | Opcional                                    | [Poner aplicaciones y complementos a la venta](put-apps-and-add-ons-on-sale.md)           |
| **Licencias organizativas**    | Valor predeterminado: Permitir que las organizaciones adquieran licencias por volumen | [Opciones de licencia de la organización](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                                       | Para obtener más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoría y Subcategoría**  | Requerido                                    | [Tabla de categoría y Subcategoría](category-and-subcategory-table.md)       |
| **Dirección URL de la Directiva de privacidad**            | Obligatorio para muchas aplicaciones. Consulta el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](store-policies.md#105-personal-information) | [Dirección URL de la Directiva de privacidad](enter-app-properties.md#privacy-policy-url)        |
| **Bsitio**                   | Opcional                                    | [Bsitio](enter-app-properties.md#website)                   |
| **Información de contacto de soporte técnico**      | Obligatorio si tu producto está disponible en Xbox; de lo contrario, opcional (pero recomendado)                                   | [Información de contacto de soporte técnico](enter-app-properties.md#support-contact-info)              |
| **Configuración del juego**             | Opcional (solo aplicable a los juegos)         | [Configuración del juego](enter-app-properties.md#game-settings) |
| **Modo de presentación**             | Opcional                   | [Modo de presentación](enter-app-properties.md#display-mode) |
| **Declaraciones de productos**          | Valor predeterminado: Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble; Windows puede incluir datos de la aplicación en las copias de seguridad automáticas en OneDrive | [Declaraciones de productos](app-declarations.md) |
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
| **Control de carga de paquetes**    | Obligatorio (al menos un paquete)        | [Cargar paquetes de aplicaciones](upload-app-packages.md) |
| **Disponibilidad de la familia de dispositivos** | Valor predeterminado: En función de los paquetes       | [Disponibilidad de la familia de dispositivos](device-family-availability.md) |
| **Implementación gradual de paquetes**   | Opcional (solo para actualizaciones)            | [Implementación gradual de paquetes](gradual-package-rollout.md) |
| **Actualización obligatoria**          | Opcional (solo para actualizaciones)            | [Actualización obligatoria](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descripciones de la Tienda

Necesitarás toda la información necesaria como mínimo para uno de los idiomas que admita la aplicación. Te recomendamos que proporciones [descripciones de la Tienda](create-app-store-listings.md) en todos los idiomas que admita la aplicación, y también puedes [proporcionar descripciones de la Tienda en idiomas adicionales](create-app-store-listings.md#store-listing-languages). Para que sea más fácil administrar descripciones múltiples del mismo producto, puedes [importar y exportar descripciones de Store](import-and-export-store-listings.md).

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descripción**               | Requerido                                    | [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md) |
| **Novedades de esta versión**   | Opcional                                 | [Notas de la versión](create-app-store-listings.md#whats-new-in-this-version)       |
| **Características de la aplicación**              | Opcional                                    | [Características del producto](create-app-store-listings.md#product-features)         |
| **Ventanas**               | Obligatorio (al menos una captura de pantalla; se recomienda que sean cuatro o más)          | [Ventanas](app-screenshots-and-images.md#screenshots)          |
| **Logotipos de la tienda**               | Recomendado; necesario para algunas versiones de sistemas operativos | [Logotipos de la tienda](app-screenshots-and-images.md#store-logos)             |
| **Clip**                  | Opcional                                    | [Clip](app-screenshots-and-images.md#trailers)                | 
| **Imagen de Windows 10 y Xbox (ilustración de 16:9 Super Hero)**     | Recomendado        | [Imagen de Windows 10 y Xbox (ilustración de 16:9 Super Hero)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Imágenes de Xbox**     | Necesario para la presentación adecuada si se publica en Xbox        | [Imágenes de Xbox](app-screenshots-and-images.md#xbox-images) |
| **Campos adicionales**  | Opcional                                    | [Campos adicionales](create-app-store-listings.md#supplemental-fields) 
| **Términos de búsqueda**              | Opcional                                    | [Términos de búsqueda](create-app-store-listings.md#search-terms)         |
| **Información de copyright y marca comercial** | Opcional                                 | [Información de copyright y marca comercial](create-app-store-listings.md#copyright-and-trademark-info) |
| **Términos de licencia adicionales**  | Opcional                                    | [Términos de licencia adicionales](create-app-store-listings.md#additional-license-terms) |
| **Desarrollado por**              | Opcional                                    | [Desarrollado por](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página de opciones de envío

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opciones de retención de publicación**     | Valor predeterminado: Publicar este envío tan pronto como supere la certificación (o siguiendo las fechas seleccionadas en la sección Programación)      | [Opciones de retención de publicación](manage-submission-options.md#publishing-hold-options)    
| **Notas para la certificación**     | Recomendado          | [Notas para la certificación](notes-for-certification.md)             |
| **Funcionalidades restringidas**     | Obligatorio si el producto declara cualquier [funcionalidad restringida](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Funcionalidades restringidas](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obtener información sobre cómo publicar aplicaciones de línea de negocio (LOB) directamente para empresas, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).
