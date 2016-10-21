---
author: jnHs
Description: "Administrar tus complementos en masa te permite realizar cambios en varios complementos al mismo tiempo, en lugar de enviar cada actualización individualmente."
title: Administrar complementos en masa
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: 9d387cf3a7850301660a672e3255a762ecd3bd4a


---

# Administrar complementos en masa

> **Importante** Esta característica está disponible actualmente solo para las cuentas de desarrollador que se hayan unido al [Programa Insider del Centro de desarrollo](dev-center-insider-program.md). La implementación de esta característica puede cambiar antes de que esté disponible para todos los desarrolladores. Esta documentación preliminar proporciona información básica sobre cómo funciona la característica.

Administrar tus complementos en masa te permite realizar cambios en varios complementos al mismo tiempo, en lugar de enviar cada actualización individualmente. Puedes acceder a esta funcionalidad desde la página de introducción de la aplicación haciendo clic en **Administrar complementos en masa**.

## Exportar información de complementos actual

Para empezar, primero tendrás que descargar un archivo de plantilla .csv. Si ya creaste complementos, este archivo incluye información sobre ellos. De lo contrario, será un archivo en blanco que puedes usar para especificar información para nuevos complementos.

Para generar y descargar este archivo de plantilla, haz clic en **Exportar complementos** y guarda el archivo .csv en el equipo.

El archivo .csv contiene las siguientes columnas. 

| Nombre de columna               | Descripción                            | ¿Requerido?      |
|---------------------------|----------------------------------|----------------------|
| Id. del producto    |  [Id. del producto](set-your-add-on-product-id.md#product-id) único del complemento.  | Sí. No se puede cambiar después de publicar el complemento. |
| Acción |Acción que se va a aplicar al importar la plantilla. Los valores admitidos son **Enviar** (para enviar un nuevo complemento o actualizar un complemento publicado anteriormente) y **CreateDraft** (para guardar los cambios sin enviarlos a la Tienda). |  Sí |
| Tipo de producto  | [Tipo de producto](set-your-add-on-product-id.md#product-type) del complemento. Los valores admitidos son **Consumible** o **Duradero**. |   Sí. No se puede cambiar después de publicar el complemento. |
| Duración del producto  | Para un complemento de tipo Duradero es **Para siempre** (para un producto que no expira nunca) o una duración establecida. Los valores de duración aceptables son: **1 día, 3días, 5días, 7días, 14días, 30días, 60días, 90días, 180días, 365días**    | Sí (si el tipo de producto es Duradero) |
| Tipo de contenido  | [Tipo de contenido](enter-add-on-properties.md#content-type) del complemento. Para la mayoría de los complementos debería ser **ElectronicSoftwareDownload**. Otros valores aceptables son: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService** |    Sí |
| Etiqueta   | Información de [etiqueta](enter-add-on-properties.md#custom-developer-data) (también conocida como **datos de desarrollador personalizados**) opcional que se usa en la implementación de la aplicación. | No |
| Precio base    | [Franja de precios](set-add-on-pricing-and-availability.md#base-price) en la que quieres ofrecer el complemento. Debe estar entre **Free** o una franja de precios válida en el formato **0,99USD**. | Sí |
| Fecha de lanzamiento  | La fecha en la que quieres publicar el complemento. Los valores aceptables son **Inmediato**, **Manual** o una cadena de fecha que cumpla con el [estándar ISO 8601](http://go.microsoft.com/fwlink/p/?LinkId=817237). | Sí |
| Títulos    | Nombre que verán los clientes del complemento, precedido por el código de idioma y un punto y coma. Por ejemplo, para usar el título "Título de ejemplo" en inglés (Estados Unidos) tendrías que *escribir en-us;Título de ejemplo*. Los títulos adicionales para otros idiomas se pueden separar por punto y coma. Cada título debe tener 100 caracteres como máximo.  | Sí |
|Descripciones   | Información adicional opcional para mostrar a los clientes, precedida por el código de idioma y configuración regional y un punto y coma. Por ejemplo, para usar la descripción "Esto es un ejemplo" en inglés (Estados Unidos), tendrías que escribir *en-us;Esto es un ejemplo*. Los títulos adicionales para otros idiomas se pueden separar por punto y coma. Cada descripción debe tener 200 caracteres como máximo.    | No |
| Mercados | Uno o más [mercados](define-pricing-and-market-selection.md#windows-store-consumer-markets) en los que quieres ofrecer el complemento. Separa cada mercado por un punto y coma. |  Sí |
|Palabras clave | [Palabras clave](enter-add-on-properties.md#keywords) opcionales que se usan en la implementación de la aplicación. | No |

## Importar complementos

Antes de importar los cambios, debes actualizar el archivo .csv descargado con los cambios que quieras realizar.

Para realizar cambios en los complementos que ya has publicado, actualiza los valores que quieres cambiar en la copia de la hoja de cálculo. Puedes quitar las filas de los complementos que no quieras actualizar o dejarlas tal cual. Ten en cuenta que si ya hay un envío en curso para ese complemento, no podrás realizar cambios con el archivo .csv.

> **Importante** Al enviar actualizaciones de complementos que ya has publicado, no puedes cambiar los campos **Id. del producto** y **Tipo de producto**.

Para enviar un nuevo complemento, agrega una nueva fila y escribe la información de tu nuevo complemento. Asegúrate de escribir toda la información necesaria. 

Cuando hayas realizado todos los cambios, guarda el archivo .csv (con el mismo nombre de archivo) y después carga el archivo arrastrándolo al campo especificado (o haz clic en **Examinar los archivos**). Se mostrará un resumen de los cambios junto con todos los errores que deben corregirse antes de su envío. Después de comprobar que la información es correcta, puedes hacer clic en **Enviar a la Tienda**. Cada complemento pasará por el proceso de envío con la información que hayas proporcionado.




<!--HONumber=Aug16_HO3-->


