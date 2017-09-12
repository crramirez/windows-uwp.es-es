---
author: jnHs
Description: "Una vez que hayas creado tu aplicación reservando un nombre, puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un envío."
title: "Envíos de aplicaciones"
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "lista de comprobación"
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: fdef30d07386a1c5ab7dc6bd62b9507ff5852194
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2017
---
# <a name="app-submissions"></a>Envíos de aplicaciones


Una vez que hayas [creado tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md), puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un **envío**.

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. El envío se guardará en el panel para que puedas trabajar en él cuando estés listo.

Después de publicar la aplicación, puedes publicar una versión actualizada creando otro envío en el panel. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. Para crear un nuevo envío de una aplicación publicada, haz clic en **Actualizar** junto al envío más reciente que se muestre en la página de información general de la aplicación.

> [!NOTE]
> En esta sección de la documentación se describe cómo crear el envío de una aplicación en el panel del Centro de desarrollo. Como alternativa, puedes usar la [API de envío de la Tienda Windows](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

## <a name="app-submission-checklist"></a>Lista de comprobación de envío de la aplicación

A continuación se incluyen los detalles que puedes proporcionar al crear el envío de tu aplicación, con vínculos a más información.

Los elementos que debes proporcionar o especificar se indican a continuación. Algunas áreas son opcionales o tienen valores predeterminados proporcionados que puedes cambiar según lo desees.

### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad
| Nombre del campo                    | Notas                                       | Más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercados**                   | Predeterminado: Todos los mercados posibles.  | [Definir los precios y la selección del mercado](define-pricing-and-market-selection.md)         |
| **Visibilidad**                | Predeterminado: Make this app available and discoverable in the Store | [Visibilidad](set-app-pricing-and-availability.md#visibility) |
| **Programación**                  | Predeterminado: Se lanzará lo antes posible        | [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md) |
| **Precio base**                | Requerido                                    | [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md)              |
| **Prueba gratuita**                | Predeterminado: sin prueba gratuita                      | [Prueba gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Precio de oferta**              | Opcional                                    | [Poner aplicaciones y complementos a la venta](put-apps-and-add-ons-on-sale.md)           |
| **Licencias organizativas**    | Valor predeterminado: Permitir que las organizaciones adquieran licencias por volumen | [Opciones de licencias organizativas](organizational-licensing.md)        |
| **Fecha de publicación**                | Valor predeterminado: publicar tan pronto como sea posible      | [Fecha de publicación](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                                       | Más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoría y subcategoría**  | Obligatorio                                    | [Tabla de categoría y subcategoría](category-and-subcategory-table.md)       |
| **Requisitos del sistema**      | Opcional                                    | [Requisitos del sistema](enter-app-properties.md#system-requirements)      |
| **Declaraciones de producto**          | Predeterminado: Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble; Windows puede incluir datos de la aplicación en las copias de seguridad automáticas en OneDrive | [Declaraciones de producto](app-declarations.md) |

<span/>

### <a name="age-ratings-page"></a>Página de clasificación por edades

| Nombre del campo                    | Notas                                       | Más información                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Clasificaciones por edades**               | Obligatorio                                    | [Clasificaciones por edades](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de paquetes

| Nombre del campo                    | Notas                                  | Más información                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Control de carga de paquetes**    | Obligatorio (al menos un paquete)        | [Cargar paquetes de la aplicación](upload-app-packages.md) |
| **Disponibilidad de familias de dispositivos** | Valor predeterminado: En función de los paquetes       | [Disponibilidad de familias de dispositivos](upload-app-packages.md#device-family-availability) |
| **Lanzamiento de paquete gradual**   | Opcional (solo para actualizaciones)            | [Lanzamiento de paquete gradual](gradual-package-rollout.md) |
| **Actualización obligatoria**          | Opcional (solo para actualizaciones)            | [Actualización obligatoria](upload-app-packages.md#mandatory-update)

<span/>

### <a name="store-listings"></a>Descripciones de la Tienda

Necesitarás toda la información necesaria como mínimo para uno de los idiomas que admita la aplicación. Te recomendamos que proporciones [descripciones de la Tienda](create-app-store-listings.md) en todos los idiomas que admita la aplicación, y también puedes [proporcionar descripciones de la Tienda en idiomas adicionales](create-app-store-listings.md#store-listing-languages).

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descripción**               | Obligatorio                                    | [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md) |
| **Notas de la versión**             | Opcional                                    | [Notas de la versión](create-app-store-listings.md#release-notes)       |
| **Capturas de pantalla**               | Obligatorio (al menos una captura de pantalla)          | [Capturas de pantalla](app-screenshots-and-images.md#screenshots)          |
| **Logotipos de la Tienda**               | Opcional, pero muy recomendado para Windows Phone 8.1 y versiones anteriores | [Logotipos de la Tienda](app-screenshots-and-images.md#store-logos)             |
| **Imágenes promocionales**        | Opcional                                    | [Imágenes promocionales](app-screenshots-and-images.md#promotional-images) |
| **Imágenes de Xbox**               | Opcional                                    | [Imágenes de Xbox](app-screenshots-and-images.md#xbox-images)              |
| **Imágenes promocionales opcionales**       | Opcional                            | [Imágenes promocionales opcionales](app-screenshots-and-images.md#optional-promotional-images)       |
| **Tráileres**                  | Opcional                                    | [Tráileres](app-screenshots-and-images.md#trailers)                | 
| **Funciones de la aplicación**              | Opcional                                    | [Características](create-app-store-listings.md#app-features)             |
| **Requisitos adicionales del sistema**      | Opcional                                    | [Requisitos adicionales del sistema](create-app-store-listings.md#additional-system-requirements) 
| **Términos de búsqueda**              | Opcional                                    | [Términos de búsqueda](create-app-store-listings.md#search-terms)         |
| **Directiva de privacidad**            | Obligatorio para algunas aplicaciones. Consulta el [Acuerdo para desarrolladores de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058) y las [Directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [Directiva de privacidad](create-app-store-listings.md#privacy-policy)        |
| **Información de copyright y marca comercial** | Opcional                                 | [Información de copyright y marca comercial](create-app-store-listings.md#copyright-and-trademark-info) |
| **Términos de licencia adicionales**  | Opcional                                    | [Términos de licencia adicionales](create-app-store-listings.md#additional-license-terms) |
| **Sitio web**                   | Opcional                                    | [Sitio web](create-app-store-listings.md#website)                   |
| **Información de contacto de soporte técnico**      | Opcional                                    | [Información de contacto de soporte técnico](create-app-store-listings.md)              |
| **Descripciones de la Tienda específicas de la plataforma** | Opcional                               | [Crear descripciones de la Tienda específicas de la plataforma](create-platform-specific-store-listings.md)  |

<span/>

### <a name="notes-for-certification-page"></a>Notas para la página de certificación

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Notas**                     | Opcional                                    | [Notas para la certificación](notes-for-certification.md)             |

<span/>

**Note**&nbsp;&nbsp;Para obtener información sobre cómo publicar aplicaciones de línea de negocio (LOB) directamente en las empresas, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).
