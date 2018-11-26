---
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: Envíos de aplicaciones
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de comprobación, windows, uwp, envío, enviar, juego, aplicación, enviar
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 444243bdb1d50146ba54af4f1417103566f97f93
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7694221"
---
# <a name="app-submissions"></a>Envíos de aplicaciones


Una vez que hayas [creado tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md), puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un **envío**.

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. Las actualizaciones que realice el envío se guardan, para que puedas volver y trabajar en él cuando estés listo.

> [!NOTE]
> Debes tener una [cuenta de desarrollador](http://go.microsoft.com/fwlink/p/?LinkId=615100) de activa en [El centro de partners](https://partner.microsoft.com/dashboard) para poder enviar aplicaciones a la Microsoft Store.

Después de publica la aplicación, puedes publicar una versión actualizada creando otro envío en el centro de partners. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. Para crear un nuevo envío para una aplicación publicada, haz clic en la **actualización** junto al envío más reciente que se muestra en su página de **información general** . También puedes [quitar una aplicación de la tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store) si es necesario hacerlo (y, a continuación, hacer que esté disponible más tarde, si lo deseas).

> [!NOTE]
> En esta sección de la documentación se describe cómo crear un envío de aplicación en el centro de partners. Como alternativa, puedes usar la [API de envío de MicrosoftStore](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, recién creado productos no pueden incluir paquetes destinados a 8.x/Windows de Windows Phone 8.x o versiones anteriores. Para obtener más información, consulta este [blog publicar](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

## <a name="app-submission-checklist"></a>Lista de comprobación de envío de aplicaciones

A continuación se incluyen los detalles que puedes proporcionar al crear el envío de tu aplicación, con vínculos a más información.

Los elementos que debes proporcionar o especificar se indican a continuación. Algunas áreas son opcionales o tienen valores predeterminados proporcionados que puedes cambiar según lo desees. No tienes que funcionan en estas secciones en el orden en que se muestran aquí.

### <a name="pricing-and-availability-page"></a>Página Precios y disponibilidad
| Nombre del campo                    | Notas                                       | Más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercados**                   | Opción predeterminada: Todos los mercados posibles  | [Definir los precios y la selección del mercado](define-pricing-and-market-selection.md)         |
| **Audiencia**                | Valor predeterminado: Audiencia pública | [Audiencia](choose-visibility-options.md#audience) |
| **Detectabilidad**                | Valor predeterminado: Hacer esta aplicación disponible y detectable en Store | [Detectabilidad](choose-visibility-options.md#discoverability) |
| **Programación**                  | Predeterminado: Se lanzará lo antes posible        | [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md) |
| **Precio base**                | Requerido                                    | [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md)              |
| **Prueba gratuita**                | Predeterminado: sin prueba gratuita                      | [Prueba gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Precio de oferta**              | Opcional                                    | [Poner aplicaciones y complementos a la venta](put-apps-and-add-ons-on-sale.md)           |
| **Licencias organizativas**    | Valor predeterminado: Permitir que las organizaciones adquieran licencias por volumen | [Opciones de licencias de organización](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página Propiedades

| Nombre del campo                    | Notas                                       | Más información                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoría y subcategoría**  | Obligatorio                                    | [Tabla de categorías y subcategorías](category-and-subcategory-table.md)       |
| **Dirección URL de la directiva de privacidad**            | Obligatorio para muchas aplicaciones. Consulta el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [Dirección URL de la directiva de privacidad](enter-app-properties.md#privacy-policy-url)        |
| **Sitio web**                   | Opcional                                    | [Sitio web](enter-app-properties.md#website)                   |
| **Información de contacto de soporte técnico**      | Obligatorio si tu producto está disponible en Xbox; de lo contrario, opcional (pero recomendado)                                   | [Información de contacto de soporte técnico](enter-app-properties.md#support-contact-info)              |
| **Configuración del juego**             | Opcional (solo aplicable a los juegos)         | [Configuración del juego](enter-app-properties.md#game-settings) |
| **Modo de pantalla**             | Opcional                   | [Modo de pantalla](enter-app-properties.md#display-mode) |
| **Declaraciones de producto**          | Predeterminado: Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble; Windows puede incluir datos de la aplicación en las copias de seguridad automáticas en OneDrive | [Declaraciones de producto](app-declarations.md) |
| **Requisitos del sistema**      | Opcional                                    | [Requisitos del sistema](enter-app-properties.md#system-requirements)      |

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
| **Disponibilidad de familias de dispositivos** | Valor predeterminado: En función de los paquetes       | [Disponibilidad de familias de dispositivos](device-family-availability.md) |
| **Lanzamiento de paquete gradual**   | Opcional (solo para actualizaciones)            | [Lanzamiento de paquete gradual](gradual-package-rollout.md) |
| **Actualización obligatoria**          | Opcional (solo para actualizaciones)            | [Actualización obligatoria](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descripciones de la Tienda

Necesitarás toda la información necesaria como mínimo para uno de los idiomas que admita la aplicación. Te recomendamos que proporciones [descripciones de Store](create-app-store-listings.md) en todos los idiomas que admita la aplicación, y también puedes [proporcionar descripciones de Store en idiomas adicionales](create-app-store-listings.md#store-listing-languages). Para que sea más fácil administrar descripciones múltiples del mismo producto, puedes [importar y exportar descripciones de Store](import-and-export-store-listings.md).

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descripción**               | Obligatorio                                    | [Escribir una excelente descripción de la aplicación](write-a-great-app-description.md) |
| **Novedades de esta versión**   | Opcional                                 | [Notas de la versión](create-app-store-listings.md#whats-new-in-this-version)       |
| **Funciones de la aplicación**              | Opcional                                    | [Características del producto](create-app-store-listings.md#product-features)         |
| **Capturas de pantalla**               | Obligatorio (al menos una captura de pantalla; se recomienda que sean cuatro o más)          | [Capturas de pantalla](app-screenshots-and-images.md#screenshots)          |
| **Logotipos de Store**               | Recomendado; necesario para algunas versiones de sistemas operativos | [Logotipos de Store](app-screenshots-and-images.md#store-logos)             |
| **Tráileres**                  | Opcional                                    | [Tráileres](app-screenshots-and-images.md#trailers)                | 
| **Imagen de Windows 10 y Xbox (imagen principal súper 16:9)**     | Recomendaciones        | [Windows 10 y Xbox imagen (imagen 16:9 ilustración principal súper)
] (app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Imágenes de Xbox**     | Obligatorio para una correcta visualización Si publicas en Xbox        | [Imágenes de Xbox
] (imágenes de xbox de aplicación capturas de pantalla y images.md #) |
| **Campos adicionales**  | Opcional                                    | [Campos adicionales](create-app-store-listings.md#supplemental-fields) 
| **Términos de búsqueda**              | Opcional                                    | [Términos de búsqueda](create-app-store-listings.md#search-terms)         |
| **Información de copyright y marca comercial** | Opcional                                 | [Información de copyright y marca comercial](create-app-store-listings.md#copyright-and-trademark-info) |
| **Términos de licencia adicionales**  | Opcional                                    | [Términos de licencia adicionales](create-app-store-listings.md#additional-license-terms) |
| **Desarrollado por**              | Opcional                                    | [Desarrollado por](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página de opciones de envío

| Nombre del campo                    | Notas                                       | Para obtener más información                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opciones de suspensión de publicación**     | Valor predeterminado: Publicar este envío tan pronto como supere la certificación (o siguiendo las fechas seleccionadas en la sección Programación)      | [Opciones de suspensión de publicación](manage-submission-options.md#publishing-hold-options)    
| **Notas para la certificación**     | Recomendado          | [Notas para la certificación](notes-for-certification.md)             |
| **Funcionalidades restringidas**     | Necesario si tu producto declara las [funcionalidades restringidas](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Funcionalidades restringidas](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obtener información sobre cómo publicar aplicaciones de línea de negocio (LOB) directamente para empresas, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).
