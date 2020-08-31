---
Description: Una vez que hayas creado tu aplicación reservando un nombre, puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un envío.
title: Envíos de aplicaciones
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de comprobación, Windows, UWP, envío, envío, juego, aplicación, envío
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1359fb530dec1a35b2ab2994442b65ec441cc0ac
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158069"
---
# <a name="app-submissions"></a>Envíos de aplicaciones


Una vez que hayas [creado tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md), puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un **envío**.

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. Las actualizaciones que realice en el envío se guardarán, por lo que puede volver y trabajar en ella siempre que esté listo.

> [!NOTE]
> Debe tener una cuenta de [desarrollador](https://developer.microsoft.com/store/register) activa en el [centro de Partners](https://partner.microsoft.com/dashboard) para poder enviar aplicaciones a la Microsoft Store.

Una vez publicada la aplicación, puede publicar una versión actualizada mediante la creación de otro envío en el centro de Partners. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. Para crear un nuevo envío para una aplicación publicada, haga clic en **Actualizar** junto al envío más reciente que se muestra en su página de **información general** . También puede [quitar una aplicación de la tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store) si tiene que hacerlo (y, a continuación, volver a estar disponible más adelante, si lo desea).

> [!NOTE]
> En esta sección de la documentación se describe cómo crear un envío de aplicación en el centro de Partners. Como alternativa, puede usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

> [!IMPORTANT]
> Ya no se pueden cargar nuevos paquetes XAP compilados con los SDK de Windows Phone 8. x. Las aplicaciones que ya se encuentran en el almacén con paquetes XAP seguirán funcionando en dispositivos Windows 10 Mobile. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Lista de comprobación de envío de la aplicación

A continuación se incluyen los detalles que puedes proporcionar al crear el envío de tu aplicación, con vínculos a más información.

Los elementos que debes proporcionar o especificar se indican a continuación. Algunas áreas son opcionales o tienen valores predeterminados proporcionados que puedes cambiar según lo desees. No es necesario que trabaje en estas secciones en el orden que se muestra aquí.

### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad
| Nombre de campo                    | Notas                                       | Para obtener más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercados**                   | Predeterminado: todos los mercados posibles  | [Definir los precios y la selección del mercado](./define-market-selection.md)         |
| **Audiencia**                | Valor predeterminado: público público | [Audiencia](choose-visibility-options.md#audience) |
| **Detectabilidad**                | Valor predeterminado: hacer que esta aplicación esté disponible y reconocible en el almacén | [Detectabilidad](choose-visibility-options.md#discoverability) |
| **Programación**                  | Valor predeterminado: liberar lo antes posible        | [Configurar la programación precisa del lanzamiento](configure-precise-release-scheduling.md) |
| **Precio base**                | Obligatorio                                    | [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md)              |
| **Evaluación gratuita**                | Valor predeterminado: sin prueba gratuita                      | [Evaluación gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Precio de oferta**              | Opcional                                    | [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md)           |
| **Licencias organizativas**    | Valor predeterminado: Permitir que las organizaciones adquieran licencias por volumen | [Opciones de licencia organizativas](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página de propiedades

| Nombre de campo                    | Notas                                       | Para obtener más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoría y subcategoría**  | Obligatorio                                    | [Tabla de categoría y subcategoría](category-and-subcategory-table.md)       |
| **Dirección URL de la directiva de privacidad**            | Necesario para muchas aplicaciones. Consulta el [contrato de desarrollador de aplicaciones](/legal/windows/agreements/app-developer-agreement) y las [directivas de Microsoft Store](store-policies.md#105-personal-information) | [Dirección URL de la directiva de privacidad](enter-app-properties.md#privacy-policy-url)        |
| **Sitio web**                   | Opcional                                    | [Sitio web](enter-app-properties.md#website)                   |
| **Información de contacto de soporte técnico**      | Obligatorio si el producto está disponible en Xbox; de lo contrario, opcional (pero recomendado)                                   | [Información de contacto de soporte técnico](enter-app-properties.md#support-contact-info)              |
| **Configuración del juego**             | Opcional (solo es aplicable a juegos)         | [Configuración del juego](enter-app-properties.md#game-settings) |
| **Modo de presentación**             | Opcional                   | [Modo de presentación](enter-app-properties.md#display-mode) |
| **Declaraciones de producto**          | Valor predeterminado: Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble; Windows puede incluir datos de la aplicación en las copias de seguridad automáticas en OneDrive | [Declaraciones de producto](./product-declarations.md) |
| **Requisitos del sistema**      | Opcional                                    | [Requisitos del sistema](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Página de clasificación por edades

| Nombre de campo                    | Notas                                       | Para obtener más información                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Clasificaciones por edades**               | Obligatorio                                    | [Clasificaciones por edades](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de paquetes

| Nombre de campo                    | Notas                                  | Para obtener más información                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Control de carga de paquetes**    | Obligatorio (al menos un paquete)        | [Cargar paquetes de la aplicación](upload-app-packages.md) |
| **Disponibilidad de familias de dispositivos** | Valor predeterminado: En función de los paquetes       | [Disponibilidad de familias de dispositivos](device-family-availability.md) |
| **Lanzamiento gradual del paquete**   | Opcional (solo para actualizaciones)            | [Lanzamiento gradual del paquete](gradual-package-rollout.md) |
| **Actualización obligatoria**          | Opcional (solo para actualizaciones)            | [Actualización obligatoria](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descripciones de la Tienda

Necesitarás toda la información necesaria como mínimo para uno de los idiomas que admita la aplicación. Te recomendamos que proporciones [descripciones de la Tienda](create-app-store-listings.md) en todos los idiomas que admita la aplicación, y también puedes [proporcionar descripciones de la Tienda en idiomas adicionales](create-app-store-listings.md#store-listing-languages). Para facilitar la administración de varias listas para el mismo producto, puede [importar y exportar listas](import-and-export-store-listings.md)de la tienda.

| Nombre de campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descripción**               | Obligatorio                                    | [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md) |
| **Novedades de esta versión**   | Opcional                                 | [Notas de la versión](create-app-store-listings.md#whats-new-in-this-version)       |
| **Funciones de la aplicación**              | Opcional                                    | [Características del producto](create-app-store-listings.md#product-features)         |
| **Capturas de pantalla**               | Obligatorio (al menos una captura de pantalla; se recomiendan cuatro o más)          | [Capturas de pantalla](app-screenshots-and-images.md#screenshots)          |
| **Logotipos de Store**               | Recomendar necesario para algunas versiones del sistema operativo | [Logotipos de Store](app-screenshots-and-images.md#store-logos)             |
| **Clips finales**                  | Opcional                                    | [Clips finales](app-screenshots-and-images.md#trailers)                | 
| **Imagen de Windows 10 y Xbox (ilustración de 16:9 Super Hero)**     | Recomendado        | [Imagen de Windows 10 y Xbox (ilustración de 16:9 Super Hero)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Imágenes de Xbox**     | Necesario para la presentación adecuada si se publica en Xbox        | [Imágenes de Xbox](app-screenshots-and-images.md#xbox-images) |
| **Campos adicionales**  | Opcional                                    | [Campos adicionales](create-app-store-listings.md#supplemental-fields) 
| **Términos de búsqueda**              | Opcional                                    | [Términos de búsqueda](create-app-store-listings.md#search-terms)         |
| **Información de copyright y marca comercial** | Opcional                                 | [Información de copyright y marca comercial](create-app-store-listings.md#copyright-and-trademark-info) |
| **Términos de licencia adicionales**  | Opcional                                    | [Términos de licencia adicionales](create-app-store-listings.md#additional-license-terms) |
| **Desarrollado por**              | Opcional                                    | [Desarrollado por](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página opciones de envío

| Nombre de campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opciones de retención de publicación**     | Valor predeterminado: publicar este envío en cuanto pase la certificación (o por las fechas seleccionadas en la sección programación)      | [Opciones de retención de publicación](manage-submission-options.md#publishing-hold-options)    
| **Notas para certificación**     | Recomendado          | [Notas para certificación](notes-for-certification.md)             |
| **Funcionalidades restringidas**     | Obligatorio si el producto declara cualquier [funcionalidad restringida](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Funcionalidades restringidas](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obtener información sobre cómo publicar aplicaciones de línea de negocio (LOB) directamente en empresas, consulte [distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).