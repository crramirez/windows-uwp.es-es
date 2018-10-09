---
author: jnHs
Description: The first step in creating a new app in your Windows Dev Center dashboard is reserving an app name. See how to reserve app names and find suggestions for choosing a great name for your app.
title: Crear la aplicación reservando un nombre
keywords: windows 10, uwp, reserva de nombre, nombre de la aplicación, nombres de aplicaciones, nombres, nombre de producto, nomenclatura, nombre reservado, título, nombres, títulos
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 784accda4299891fa86501236d35c0828e80cf8d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "4424279"
---
# <a name="create-your-app-by-reserving-a-name"></a>Crear la aplicación reservando un nombre

El primer paso para crear una nueva aplicación en el [panel del centro de desarrollo de Windows](https://partner.microsoft.com/dashboard) es reservar un nombre de aplicación. Cada nombre reservado (a veces se denomina como *título* de la aplicación) debe ser único en todo Microsoft Store.

Puedes reservar un nombre para tu aplicación incluso si aún no has empezado a crearla. Te recomendamos hacerlo tan pronto como sea posible, para que nadie más pueda usar el nombre. Ten en cuenta que necesitarás enviar la aplicación en un plazo de tres meses para poder mantener ese nombre reservado para tu uso.

Cuando [cargas los paquetes de la aplicación](upload-app-packages.md), el valor [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) debe coincidir con el nombre que reservaste para la aplicación. Si usas Microsoft Visual Studio para crear el paquete de la aplicación, este atributo se rellenará automáticamente.

> [!IMPORTANT]
> Puedes reservar nombres adicionales para una aplicación y puedes optar por usar uno de ellos en la versión publicada de la aplicación en lugar de la que reserva al crear la aplicación en primer lugar en el panel. Sin embargo, ten en cuenta que el nombre que escribas aquí se usará en la parte de [Detalles de identidad](view-app-identity-details.md), como el **Nombre de familia de paquete (PFN) la aplicación**. Estos valores pueden ser visibles para algunos usuarios y no se puede cambiar, así que asegúrate de que el nombre que reservas es apropiado para este uso.


## <a name="create-your-app-by-reserving-a-new-name"></a>Crear la aplicación reservando un nuevo nombre

Reservar un nombre es el primer paso para crear una aplicación en el panel. 

1.  En la página **Introducción**, haz clic en **Crear una nueva aplicación**.
2.  En el cuadro de texto, escribe el nombre que quieres usar y luego selecciona **Comprobar disponibilidad**. Si el nombre está disponible, verás una marca de verificación verde. (Si el nombre que escribiste ya está reservado o lo usa otro desarrollador, verás un mensaje que indica que el nombre no está disponible).
3.  Haz clic en **Reservar nombre de producto**.

Ahora el nombre se ha reservado para ti y puedes empezar a trabajar en el [envío](app-submissions.md) cuando estés listo. 

> [!NOTE]
> Es posible que no puedas reservar un nombre, aunque no veas ninguna aplicación con ese nombre en la Microsoft Store. Por lo general, esto se debe a que otro desarrollador ha reservado el nombre para su aplicación pero aún no la ha enviado. Si no puedes reservar un nombre cuyos derechos de marca comercial u otros te corresponden, o si ves otra aplicación en la Microsoft Store que usa ese nombre, [ponte en contacto con Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777).

Después de reservar un nombre, tendrás tres meses para enviar la aplicación. Si no la envías en los tres meses, la reserva de nombre caducará y otro desarrollador podrá usar ese nombre para una aplicación. Se puede producir un error si intentas enviar una aplicación con un nombre que has dejado expirar.


## <a name="choosing-your-apps-name"></a>Elegir el nombre de la aplicación

Elegir el nombre correcto para la aplicación es una tarea importante. Elige un nombre que capte la atención de los clientes y los haga leer más sobre la aplicación. Estas son algunas sugerencias para que elijas un buen nombre de aplicación.

-   **Elige un nombre corto.** El espacio dedicado para mostrar el nombre de tu aplicación es limitado en la mayoría de los casos, por lo que sugerimos que uses el nombre más corto posible. Aunque el nombre de tu aplicación puede tener hasta 256 caracteres, puede suceder que el final de un nombre muy largo no siempre esté visible para los clientes.
    > [!NOTE]
    > El número real de caracteres mostrados puede variar en función de la longitud asignada y de los tipos de caracteres que se usen en el nombre de la aplicación. Por ejemplo, en la fuente Segoe UI que usa Windows, caben unos 30 caracteres "I" en el mismo espacio que 10 caracteres "W". Debido a esta variación, asegúrate de probar la aplicación y comprobar cómo aparece su nombre en los iconos (si eliges superponer el nombre de la aplicación), en resultados de búsqueda y dentro de la propia aplicación. Ten en cuenta también cada idioma en el que ofreces tu aplicación. Ten en cuenta que los caracteres asiáticos suelen ser más anchos que los latinos, por lo que se muestran menos caracteres.
-   **Sé original.** Procura que el nombre de tu aplicación sea lo suficientemente distintivo como para que no se confunda fácilmente con una aplicación existente.
-   **No uses nombres registrados por otros.** Asegúrate de tener los derechos de usar el nombre que reservas. Si alguien más tiene los derechos de marca comercial de ese nombre, puede denunciar una infracción y no podrás seguir usándolo. Si esto sucede después de publicada la aplicación, se quitará de la Tienda. Deberás cambiar el nombre de tu aplicación, incluyendo todas las instancias del nombre que aparecen en la aplicación y el contenido, antes de que puedas [enviar la aplicación](app-submissions.md) nuevamente para su certificación.
-   **Evita agregar información diferenciadora al final del nombre.** Si la información que distingue las distintas aplicaciones se agrega al final de un nombre, los clientes pueden no verla, especialmente si el nombre es largo y puede parecer que todas las aplicaciones tienen el mismo nombre. Si esto es inevitable, usa logotipos y diferentes imágenes de la aplicación para que sea más fácil diferenciar una aplicación de otro.
-   **No incluyas emojis en tu nombre.** No podrás reservar un nombre que incluya emojis u otros caracteres no admitidos.


## <a name="manage-additional-app-names"></a>Administrar nombres de aplicación adicionales

Puedes agregar y administrar nombres adicionales en la página **Administrar nombres de aplicaciones**, en la sección **Administración de la aplicación** de cada una de las aplicaciones que tienes en el panel de información del Centro de desarrollo de Windows.

En algunos casos, quizá quieras reservar varios nombres para la misma aplicación, por ejemplo, cuando la ofreces en distintos idiomas y quieres utilizar diferentes nombres para cada idioma. Si quieres cambiar el nombre de la aplicación completamente, deberás reservar un nombre adicional.

En esta página también puedes eliminar cualquier nombre que hayas reservado pero que ya no quieras usar.

Para obtener más información, consulta [Administrar nombres de aplicación](manage-app-names.md).

 

 




