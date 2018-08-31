---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: Envíos de aplicaciones
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de comprobación, windows, uwp, envío, enviar, juego, aplicación, enviar
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9802577f9252b590657406bcb59b0c28adeb4781
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "3238295"
---
# <a name="app-submissions"></a>Envíos de aplicaciones


Una vez que hayas [creado tu aplicación reservando un nombre](create-your-app-by-reserving-a-name.md), puedes empezar a trabajar en conseguir que se publique. El primer paso es crear un **envío**.

Puedes iniciar el envío cuando la aplicación está completa y lista para publicar o puedes empezar a escribir información incluso antes de que hayas escrito una sola línea de código. Las actualizaciones que realice el envío se guardan, por lo que puede volver y trabajar en él cuando estés listo.

> [!NOTE]
> Debes tener una [cuenta de desarrollador](http://go.microsoft.com/fwlink/p/?LinkId=615100) para tener acceso al [Centro de desarrollo de Windows](https://partner.microsoft.com/dashboard) y enviar aplicaciones a la Microsoft Store.

Después de publicar la aplicación, puedes publicar una versión actualizada creando otro envío en el panel. Crear un nuevo envío permite hacer y publicar los cambios que son necesarios, tanto si cargas nuevos paquetes como si tan solo cambias detalles como el precio o la categoría. Para crear un nuevo envío de una aplicación publicada, haz clic en **Actualizar** junto al envío más reciente que se muestre en la página de información general de la aplicación. También puedes [quitar una aplicación de la tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store) si es necesario hacerlo (y, a continuación, hacer que esté disponible más adelante, si lo deseas).

> [!NOTE]
> En esta sección de la documentación se describe cómo crear un envío de aplicación en el panel del Centro de desarrollo. Como alternativa, puedes usar la [API de envío de MicrosoftStore](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar los envíos de aplicaciones.

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
| **Funciones de la aplicación**              | Opcional                                    | [Funciones de la aplicación](create-app-store-listings.md#app-features)         |
| **Capturas de pantalla**               | Obligatorio (al menos una captura de pantalla; se recomienda que sean cuatro o más)          | [Capturas de pantalla](app-screenshots-and-images.md#screenshots)          |
| **Logotipos de Store**               | Recomendado; necesario para algunas versiones de sistemas operativos | [Logotipos de Store](app-screenshots-and-images.md#store-logos)             |
| **Activos gráficos adicionales**     | Recomendado (especialmente para algunas versiones de sistemas operativos)         | [Activos de imágenes adicionales](app-screenshots-and-images.md#additional-art-assets) |
| **Tráileres**                  | Opcional                                    | [Tráileres](app-screenshots-and-images.md#trailers)                | 
| **Campos adicionales**  | Opcional                                    | [Información complementaria](create-app-store-listings.md#supplemental-fields) 
| **Términos de búsqueda**              | Opcional                                    | [Términos de búsqueda](create-app-store-listings.md#search-terms)         |
| **Información de copyright y marca comercial** | Opcional                                 | [Información de copyright y marca comercial](create-app-store-listings.md#copyright-and-trademark-info) |
| **Términos de licencia adicionales**  | Opcional                                    | [Términos de licencia adicionales](create-app-store-listings.md#additional-license-terms) |
| **Desarrollado por**              | Opcional                                    | [Desarrollado por](create-app-store-listings.md#developed-by)                   |
| **Descripciones de Store específicas de la plataforma** | Opcional                               | [Creación de descripciones de Store específicas de la plataforma](create-platform-specific-store-listings.md)  |

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
