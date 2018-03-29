---
author: jnHs
Description: The first step in creating a new app in your Windows Dev Center dashboard is reserving an app name. See how to reserve app names and find suggestions for choosing a great name for your app.
title: Crear la aplicación reservando un nombre
keywords: windows 10, uwp, reserva de nombre, nombre de la aplicación, nombres de aplicación, nombres, nombre del producto, nomenclatura
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.author: wdg-dev-content
ms.date: 01/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 960acceb9f665b9d2f2d1def680626876c3aa29b
ms.sourcegitcommit: b6915c7fa2c7292e9b4e3d3e9927dc8746ec1ffb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
# <a name="create-your-app-by-reserving-a-name"></a>Crear la aplicación reservando un nombre


El primer paso para crear una nueva aplicación en el panel del Centro de desarrollo de Windows es reservar un nombre de aplicación. Aprende a reservar nombres de aplicación y buscar sugerencias para [elegir un buen nombre de aplicación](#choosing-your-apps-name). Cada nombre reservado debe ser único en toda la Microsoft Store.

Cuando [cargas los paquetes de la aplicación](upload-app-packages.md), el valor [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-displayname) debe coincidir con el nombre que reservaste para la aplicación. Si usas Microsoft Visual Studio para crear el paquete de la aplicación, este atributo se rellenará automáticamente.

## <a name="create-your-app-by-reserving-a-new-name"></a>Crear la aplicación reservando un nuevo nombre

Reservar un nombre es el primer paso para crear una aplicación en el panel. Puedes hacerlo incluso si aún no has empezado a compilar la aplicación. Recomendamos hacerlo lo antes posible para que nadie más pueda usar el nombre.

1.  En la página **Introducción**, haz clic en **Crear una nueva aplicación**.
2.  En el cuadro de texto, escribe el nombre que quieres usar y luego selecciona **Comprobar disponibilidad**. Si el nombre está disponible, verás una marca de verificación verde. (Si el nombre que escribiste ya está reservado o lo usa otro desarrollador, verás un mensaje que indica que el nombre no está disponible).
3.  Haz clic en **Reservar nombre de producto**.

Ahora el nombre se ha reservado para ti y puedes empezar a trabajar en el [envío](app-submissions.md) cuando estés listo.

> [!NOTE]
> Es posible que no puedas reservar un nombre, aunque no veas ninguna aplicación con ese nombre en la Microsoft Store. Por lo general, esto se debe a que otro desarrollador ha reservado el nombre para su aplicación pero aún no la ha enviado. Si no puedes reservar un nombre cuyos derechos de marca comercial u otros te corresponden, o si ves otra aplicación en la Microsoft Store que usa ese nombre, [ponte en contacto con Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777).

Después de reservar un nombre, tendrás un año para enviar la aplicación. Si no la envías en el plazo del año, la reserva de nombre caducará y otro desarrollador podrá usarlo para una aplicación. Se puede producir un error si intentas enviar una aplicación con un nombre que has dejado expirar.

> [!NOTE]
> Si tienes una aplicación WindowsPhone que has creado en el panel de información anterior de WindowsPhone y nunca le has reservado un nombre, tendrás que hacerlo para cargar paquetes .appx o [ver los detalles de la identidad de la aplicación](view-app-identity-details.md) específicos de los paquetes .appx. Reservar un nombre exclusivo también evita que cualquier otra persona reserve ese nombre. Sin embargo, si no reservas un nombre, puedes seguir administrando y enviando la aplicación a los clientes de Windows Phone 8.x.


## <a name="choosing-your-apps-name"></a>Elegir el nombre de la aplicación

Elegir el nombre correcto para la aplicación es una tarea importante. Elige un nombre que capte la atención de los clientes y los haga leer más sobre la aplicación. Estas son algunas sugerencias para que elijas un buen nombre de aplicación.

-   **Elige un nombre corto.** El espacio dedicado para mostrar el nombre de tu aplicación es limitado en la mayoría de los casos, por lo que sugerimos que uses el nombre más corto posible. Aunque el nombre de tu aplicación puede tener hasta 256 caracteres, puede suceder que el final de un nombre muy largo no siempre esté visible para los clientes.

   > [!NOTE]
   > El número real de caracteres mostrados puede variar en función de la longitud asignada y de los tipos de caracteres que se usen en el nombre de la aplicación. Por ejemplo, en la fuente Segoe UI que usa Windows, caben unos 30 caracteres "I" en el mismo espacio que 10 caracteres "W". Debido a esta variación, asegúrate de probar la aplicación y comprobar cómo aparece su nombre en los iconos (si eliges superponer el nombre de la aplicación), en resultados de búsqueda y dentro de la aplicación en sí antes de enviarla. Ten en cuenta también cada idioma en el que ofreces tu aplicación. Ten en cuenta que los caracteres asiáticos suelen ser más anchos que los latinos, por lo que se muestran menos caracteres.

-   **Sé original.** Procura que el nombre de tu aplicación sea lo suficientemente distintivo como para que no se confunda fácilmente con una aplicación existente.
-   **No uses nombres registrados por otros.** Asegúrate de tener los derechos de usar el nombre que reservas. Si alguien más tiene los derechos de marca comercial de ese nombre, puede denunciar una infracción y no podrás seguir usándolo. Si esto sucede después de publicada la aplicación, se quitará de la Tienda. Deberás cambiar el nombre de tu aplicación, incluyendo todas las instancias del nombre que aparecen en la aplicación y el contenido, antes de que puedas [enviar la aplicación](app-submissions.md) nuevamente para su certificación.
-   **Evita agregar información diferenciadora al final del nombre.** Si la información que distingue las distintas aplicaciones se agrega al final de un nombre, los clientes pueden no verla, especialmente si el nombre es largo y puede parecer que todas las aplicaciones tienen el mismo nombre. Si esto es inevitable, usa logotipos e imágenes de aplicación diferentes, de forma que sea más fácil diferenciar una aplicación de otra.
-   **No incluyas emojis en tu nombre.** No podrás reservar un nombre que incluya emojis u otros caracteres no admitidos.


## <a name="manage-additional-app-names"></a>Administrar nombres de aplicación adicionales

Puedes agregar y administrar nombres adicionales en la página **Administrar nombres de aplicaciones**, en la sección **Administración de la aplicación** de cada una de las aplicaciones que tienes en el panel de información del Centro de desarrollo de Windows.

En algunos casos, quizá quieras reservar varios nombres para la misma aplicación, por ejemplo, cuando la ofreces en distintos idiomas y quieres utilizar diferentes nombres para cada idioma. Si quieres cambiar el nombre de la aplicación completamente, deberás reservar un nombre adicional.

En esta página también puedes eliminar cualquier nombre que hayas reservado pero que ya no quieras usar.

Para obtener más información, consulta [Administrar nombres de aplicación](manage-app-names.md).

 

 




