---
author: jnHs
Description: "Administrar tus IAP en masa te permite realizar cambios en varias IAP al mismo tiempo, en lugar de enviar cada actualización individualmente."
title: Administrar IAP en masa
translationtype: Human Translation
ms.sourcegitcommit: 475371dd55aa111f3743c03dc1600e8cfdbeb5b0
ms.openlocfilehash: ae4d4ed33b9bd10a2b01b336c942ad3212de6533


---

# Administrar IAP en masa

> 
            **Importante** Esta característica está disponible actualmente solo para las cuentas de desarrollador que se hayan unido al [Programa Insider del Centro de desarrollo](dev-center-insider-program.md). La implementación de esta característica puede cambiar antes de que esté disponible para todos los desarrolladores. Esta documentación preliminar proporciona información básica sobre cómo funciona la característica.

Administrar tus IAP en masa te permite realizar cambios en varias IAP al mismo tiempo, en lugar de enviar cada actualización individualmente. Puedes acceder a esta funcionalidad desde la página de introducción de la aplicación haciendo clic en **Administrar IAP en masa**.

## Exportar información de IAP actual

Para empezar, primero tendrás que descargar un archivo de plantilla .csv. Si ya has creado IAP, este archivo incluye información sobre ellos. De lo contrario, será un archivo en blanco que puedes usar para especificar información para nuevos IAP. 

Para generar y descargar este archivo de plantilla, haz clic en **Exportar IAP** y guarda el archivo .csv en el equipo.

El archivo .csv contiene las siguientes columnas. 

| Nombre de columna               | Descripción                            | ¿Requerido?      |
|---------------------------|----------------------------------|----------------------|
| Id. del producto    |  El [Id. de producto](set-your-iap-product-id.md#product-id) único del IAP.  | Sí. No se puede cambiar después de que se publique el IAP. |
| Acción |La acción que se va a aplicar al importar la plantilla. Los valores admitidos son **Enviar** (para enviar un nuevo IAP o actualizar un IAP publicado anteriormente) y **CreateDraft** (para guardar los cambios sin enviarlos a la Tienda). |    Sí |
| Tipo de producto  | El [tipo de producto](set-your-iap-product-id.md#product-type) del IAP. Los valores admitidos son **Consumible** o **Duradero**. | Sí. No se puede cambiar después de que se publique el IAP. |
| Duración del producto  | Para un IAP duradero es **Para siempre** (para un producto que no expira nunca) o una duración establecida. Los valores de duración aceptables son: **1 día, 3días, 5días, 7días, 14días, 30días, 60días, 90días, 180días, 365días**   | Sí (si el tipo de producto es Duradero) |
| Tipo de contenido  | El [tipo de contenido](enter-iap-properties.md#content-type) del IAP. Para la mayoría de IAP debería ser **ElectronicSoftwareDownload**. Otros valores aceptables son: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService** | Sí |
| Etiqueta   | Información de [etiqueta](enter-iap-properties.md#tag) opcional que se usa en la implementación de la aplicación. | No |
| Precio base    | La [franja de precios](set-iap-pricing-and-availability.md#base-price) en la que quieres ofrecer el IAP. Debe estar entre **Gratis** o una franja de precios válida en el formato **0.99USD**. |   Sí |
| Fecha de lanzamiento  | La fecha en la que quieres publicar el IAP. Los valores aceptables son **Inmediato**, **Manual** o una cadena de fecha que cumpla con el [estándar ISO 8601](http://go.microsoft.com/fwlink/p/?LinkId=817237). | Sí |
| Títulos    | El nombre que verán los clientes del IAP, precedido por el código de idioma y un punto y coma. Por ejemplo, para usar el título "Título de ejemplo" en inglés (Estados Unidos) tendrías que *escribir en-us;Título de ejemplo*. Los títulos adicionales para otros idiomas se pueden separar por punto y coma. Cada título debe tener 100 caracteres como máximo.     | Sí |
|Descripciones   | Información adicional opcional para mostrar a los clientes, precedida por el código de idioma y configuración regional y un punto y coma. Por ejemplo, para usar la descripción "Esto es un ejemplo" en inglés (Estados Unidos), tendrías que escribir *en-us;Esto es un ejemplo*. Los títulos adicionales para otros idiomas se pueden separar por punto y coma. Cada descripción debe tener 200 caracteres como máximo.    | No |
| Mercados | Uno o más [mercados](define-pricing-and-market-selection.md#windows-store-consumer-markets) en los que quieres ofrecer el IAP. Separa cada mercado por un punto y coma. | Sí |
|Palabras clave | Información de las [palabras clave](enter-iap-properties.md#keywords) opcionales que se usan en la implementación de la aplicación. | No |

## Importar IAP

Antes de importar los cambios, debes actualizar el archivo .csv descargado con los cambios que quieras realizar.

Para realizar cambios en los IAP que ya has publicado, actualiza los valores que desees cambiar en la copia de la hoja de cálculo. Puedes quitar cualquier fila del IAP que no quieras actualizar o dejarlas tal cual. Ten en cuenta que si ya hay un envío en curso para ese IAP, no podrás realizar cambios con el archivo .csv.

> 
            **Importante** Al enviar actualizaciones de IAP que ya hayas publicado, no puedes cambiar los campos **Id. del producto** y **Tipo de producto**.

Para enviar un nuevo IAP, agrega una nueva fila y escribe la información de tu nuevo IAP. Asegúrate de escribir toda la información necesaria. 

Cuando hayas realizado todos los cambios, guarda el archivo .csv (con el mismo nombre de archivo) y después carga el archivo arrastrándolo al campo especificado (o haz clic en **Examinar los archivos**). Se mostrará un resumen de los cambios junto con todos los errores que deben corregirse antes de su envío. Después de comprobar que la información es correcta, puedes hacer clic en **Enviar a la Tienda**. Cada IAP pasará por el proceso de envío con la información que hayas proporcionado.




<!--HONumber=Jul16_HO1-->


