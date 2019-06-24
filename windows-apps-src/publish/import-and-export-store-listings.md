---
Description: Puede crear listas de Store para sus aplicaciones sin usar el centro de partners mediante la exportación de los anuncios en un archivo .csv, escribir la información y recursos y, a continuación, importar el archivo actualizado.
title: Importar y exportar descripciones de la Tienda
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, importar descripciones de store, exportar descripciones de store, importar exportar, descripción de store csv
ms.localizationpriority: medium
ms.openlocfilehash: b45f12a9ef7212ddb23b647894189491bfa73549
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320634"
---
# <a name="import-and-export-store-listings"></a>Importar y exportar descripciones de la Tienda

En lugar de [escribir información de sus publicaciones Store directamente en el centro de partners](create-app-store-listings.md), tiene la opción para agregar o actualizar la información mediante la exportación de los anuncios en un archivo .csv, escribir la información y recursos y, a continuación, importar el archivo actualizado. Puedes usar este método para crear descripciones desde cero o actualizar descripciones que ya has creado.

Esta opción es especialmente útil si quieres crear o actualizar descripciones de Store para el producto en varios idiomas, ya que puedes copiar y pegar la misma información en varios campos y realizar cualquier cambio fácilmente que se deben aplicar a un idioma concreto. Sin embargo, no se puede utilizar este método para crear o actualizar [anuncios de Store específicos de la plataforma](create-platform-specific-store-listings.md) para las aplicaciones publicadas anteriormente que compatibilidad con versiones anteriores de sistema operativo. 

> [!TIP]
> También puedes usar esta característica para importar y exportar descripciones de la Tienda de un complemento. En el caso de los complementos, el proceso funciona igual, salvo que se incluyen [únicamente los campos relevantes de los complementos](#add-ons).

Tenga en cuenta que siempre puede crear o actualizar programas directamente en el centro de partners (incluso si anteriormente ha usado el método de importación/exportación). La actualización directamente en el centro de partners puede ser más fácil cuando simplemente realiza un cambio sencillo, pero puede usar cualquiera de estos métodos en cualquier momento.

## <a name="export-listings"></a>Exportar descripciones

En la página de información general de envíos, haz clic en **Exportar descripción** (en la sección **Descripciones de la Tienda**) para generar un archivo .csv codificado en UTF-8. Guarda este archivo en tu equipo.

Puedes usar Microsoft Excel u otro editor para editar este archivo. Ten en cuenta que las versiones de Excel de Office 365 te permiten guardar un archivo .csv como **CSV UTF-8 (delimitado por comas) (*.csv)** , pero otras versiones puede que no lo admitan. Puedes encontrar información sobre las versiones de Excel compatibles con esta característica en [Boletín de nuevas características de Excel 2016](https://support.office.com/article/what-s-new-in-excel-for-office-365-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73?ui=en-US&rs=en-001&ad=US), así como más información sobre la codificación de UTF-8 en diversos editores [aquí](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Si aún no has creado las descripciones para tu producto, el archivo .csv que exportaste no incluirá los datos personalizados. Aparecerán las columnas **Campo**, **ID**, **Tipo** y **predeterminado**, así como las filas que corresponden a cada elemento que puede aparecer en una descripción de la Tienda.

Si ya has creado descripciones (o has cargado paquetes), también aparecerán columnas etiquetadas con códigos de idioma y configuración regional que corresponden al idioma de cada descripción que has creado (o que hemos detectado en tus paquetes), así como la información de descripción que hayas proporcionado anteriormente.
     
Aquí tienes información general del contenido de cada una de las columnas en el archivo .csv exportado:
- La columna **Campo** incluye un nombre que está asociado a cada parte de una descripción de la Tienda. Estos se corresponden con los mismos elementos que puede proporcionar al crear anuncios Store en el centro de partners, aunque algunos de los nombres son ligeramente diferentes. En los elementos en los que puedes escribir más de un mismo tipo de elemento, aparecerán varias filas, hasta el número máximo que puedas especificar. Por ejemplo, en la opción **Características de la aplicación**, aparecerán las opciones **Feature1**, **Feature2**, etc., hasta un máximo de **Feature20** (ya que has especificado un máximo de 20 características de la aplicación).
- El **ID** columna contiene un número que el centro de partners se asocia a cada campo. 
- El **tipo** columna proporciona instrucciones generales acerca de qué tipo de información para proporcionar para ese campo, como **texto** o **ruta de acceso relativa (o dirección URL al archivo en el centro de partners)** . 
- La columna **predeterminado** (y cualquier otra columna etiquetada con códigos de idioma y configuración regional) representa el texto o los activos asociados a cada parte de la descripción de la Tienda. Puedes editar los campos de estas columnas para realizar actualizaciones en la descripciones de la Tienda.

>[!IMPORTANT]
> No cambies nada de la información de las columnas **Campo**, **ID** o **Tipo**. La información de estas columnas debe permanecer sin cambios para que el archivo importado pueda procesarse.

## <a name="update-listing-info"></a>Actualizar información de las descripciones

Una vez que hayas exportado las descripciones y hayas guardado el archivo .csv, puedes editar la información de las descripciones directamente en el archivo .csv. 

Junto con la columna **predeterminado**, cada idioma para el que has creado una descripción tiene su propia columna. Los cambios que realices en una columna se aplicarán a la descripción en dicho idioma. Puedes crear descripciones para nuevos idiomas agregando el código de idioma y configuración regional en la siguiente columna vacía ubicada en la fila superior. Para obtener una lista de códigos válidos de idioma y configuración regional, consulta [Idiomas admitidos](supported-languages.md).

Puedes usar la columna **predeterminado** para introducir información que quieras compartir entre todas las descripciones de la aplicación. Si el campo de un idioma determinado se queda en blanco, se usará la información de la columna predeterminada para dicho idioma. Puedes anular dicho campo para un idioma en concreto, introduciendo información diferente para ese idioma.

La mayoría de los campos de las descripciones de la Tienda son opcionales. En cada descripción son necesarias la **Descripción** y una captura de pantalla; en el caso de idiomas que no tengan paquetes asociados, también tendrás que especificar un **Título** para indicar los nombres reservados de las aplicaciones que deben usarse para dicha descripción. Para el resto de los campos, puedes dejar el campo en blanco si no quieres incluirlo en la descripción. Recuerda que si dejas en blanco un campo para un idioma determinado, comprobaremos si hay información de ese campo en la columna predeterminada. De ser así, se utilizará dicha información. 

Por ejemplo, considera la siguiente situación: 

![Ejemplo de descripciones exportadas](images/listingimport.png)
     
- El texto “Descripción predeterminada” se usará en el campo **Descripción** en las descripciones en en-us y fr-fr. Sin embargo, el campo **Descripción** de la descripción en es-es usaría el texto "Descripción en español". 
- En el campo **ReleaseNotes**, se usará el texto “Versiones de notas en inglés” para en-us y el texto “Versiones de notas en francés” para fr-fr. Sin embargo, no se mostrarán notas de versiones para es-es.

Si no quieres realizar modificaciones en un campo específico, puedes eliminar toda la fila de la hoja de cálculo, **a excepción de las filas de tráileres y sus miniaturas y títulos asociados**. Excepto en el caso de estos elementos, eliminar una fila no afectará a los datos asociados a ese campo en tus descripciones. Esto te permite quitar todas las filas que no vayas a editar para que puedas centrarte en los campos en los que estás realizando cambios.

Eliminar la información de un campo para un idioma, sin quitar toda la fila, funciona de manera diferente, en función del campo. En el caso de los campos en los que la opción **Tipo** es **Texto**, eliminar la información de un campo simplemente quitará dicha entrada de la descripción en ese idioma.  Sin embargo, al eliminar la información en un campo para una imagen, como una captura de pantalla o el logotipo, no tendrá ningún efecto; la imagen anterior aún se usará a menos que quite editando directamente en el centro de partners. Eliminando la información de un campo de finalizador se realmente quitar ese finalizador desde el centro de partners, así que asegúrese de que tener una copia de los archivos necesarios antes de hacerlo.

Muchos de los campos de tus descripciones exportadas requieren la entrada de texto, como por ejemplo, los que aparecen en el ejemplo anterior, **Descripción** y **ReleaseNotes**. En estos tipos de campos, simplemente escribe el texto correspondiente en el campo de cada idioma. Asegúrate de seguir la longitud y otros requisitos establecidos para cada campo. Para obtener más información sobre estos requisitos, consulta [Crear descripciones de la Tienda de aplicaciones](create-app-store-listings.md).

Indicar información para los campos que corresponden a los activos, como por ejemplo, imágenes y tráileres, resulta un poco más complicado. En lugar de **texto**, **tipo** para estos recursos es **ruta de acceso relativa (o dirección URL al archivo en el centro de partners)** . 
     
Si ya has cargado activos para tus descripciones de la Tienda, estos activos se representarán mediante una dirección URL. Estas direcciones URL pueden reutilizarse en varias descripciones de un producto, o incluso en diferentes productos de la misma cuenta de desarrollador, de modo que puedes copiarlas para volver a utilizarlas en un campo diferente si lo deseas.

> [!TIP]
> Para confirmar el activo que corresponde a una dirección URL, puedes escribir la dirección URL en un navegador para ver la imagen (o descargar el tráiler).  Debe iniciar sesión con su cuenta de centro de partners en orden para esta dirección URL para que funcione.

Si desea usar un nuevo recurso que aún no ha agregado anteriormente al centro de partners, puede hacerlo mediante la importación de sus publicaciones como una carpeta, en lugar de un archivo .csv único. Tendrás que crear una carpeta que incluya el archivo .csv. Después agrega las imágenes de esa misma carpeta a la carpeta raíz o a una subcarpeta. Tendrás que especificar la ruta de acceso completa, incluido el nombre de carpeta raíz, en el campo.

> [!TIP]
> Para obtener mejores resultados al importar tus descripciones como una carpeta, asegúrate de usar la versión más reciente de Microsoft Edge, Chrome o Firefox.

Por ejemplo, si la carpeta raíz se denomina **my_folder** y quieres usar una imagen denominada **screenshot1.png** para **DesktopScreenshot1**, puedes agregar screenshot1.png a la raíz de dicha carpeta y luego escribir **my_folder/screenshot1.png** en el campo **DesktopScreenshot1**. Si has creado una carpeta de imágenes en la carpeta raíz y has colocado ahí screenshot.1jpg, deberías escribir **my_folder/images/screenshot1.png**. Tenga en cuenta que después de importar los anuncios mediante una carpeta, rutas de acceso a las imágenes se convertirá a direcciones URL a los archivos en el centro de partners de la próxima vez que exportar los anuncios. Puedes copiar y pegar estas direcciones URL para usarlas de nuevo (por ejemplo, para usar los mismos activos en varios idiomas de descripciones). 

> [!IMPORTANT]
> Si el anuncio exportado incluye finalizadores, tenga en cuenta que al eliminar la dirección URL para el finalizador o su imagen en miniatura del archivo .csv quitará por completo el archivo eliminado del centro de partners, y ya no podrá obtener acceso a él existe (a menos que también se usa en el ano Ther listado donde no se ha borrado). 

## <a name="import-listings"></a>Importar descripciones

Una vez que hayas introducido todos los cambios en el archivo .csv (y hayas incluido los activos que quieras cargar), tendrás que guardar el archivo antes de cargarlo. Si usas una versión de Microsoft Excel compatible con la codificación UTF-8, asegúrate de seleccionar **Guardar como** y usar el formato **CSV UTF-8 (delimitado por comas) (*.csv)** . Si usas un editor diferente para ver y editar el archivo .csv, asegúrate de que el archivo se codifique en UTF-8 antes de cargarlo.

Cuando estés listo para cargar el archivo .csv actualizado e importar los datos de la descripción, selecciona **Importar descripciones** en la página de información general de envíos. Si solo vas a importar un archivo .csv, elige **Importar .csv**, busca el archivo y haz clic en **Abrir**. Si vas a importar una carpeta con archivos de imagen, elige Importar carpeta, busca la carpeta y haz clic en **Seleccionar carpeta**. Asegúrate de que hay un solo archivo .csv en la carpeta, junto con todos los activos que estás cargando. 

A medida que procesamos el archivo .csv importado, aparecerá una barra de progreso que refleja el estado de la importación y validación. Esto puede tardar algún tiempo, especialmente si tienes muchas descripciones o archivos de imagen. 

Si se detectan problemas, aparecerá una nota que indica que tendrás que realizar las actualizaciones necesarias y volver a intentarlo. Selecciona el vínculo **Ver errores** para ver los campos que no son válidos y por qué. Tendrás que corregir estos problemas en el archivo .csv (o reemplazar cualquier activo no válido) y luego volver a importar las descripciones.

> [!TIP]
> Puedes volver a acceder a esta información más adelante a través del vínculo **Ver errores de la última importación**.

Ninguna de la información del archivo .csv se guardará en el centro de partners hasta que todos los errores en el archivo se ha resuelto, incluso para los campos sin errores. Después de importar un archivo .csv que no tiene ningún error, la información de la lista que has proporcionado se guardará en el centro de partners y se usará para esa presentación.

Puede seguir realizar actualizaciones en sus publicaciones mediante la importación de otro archivo CSV actualizado, o mediante la realización de cambios directamente en el centro de partners.

## <a name="add-ons"></a>Complementos

Para complementos, importar y exportar listas Store utiliza el mismo proceso descrito anteriormente, salvo que solo verá los tres campos relevantes para [anuncios de complemento Store](create-add-on-store-listings.md): **Descripción**, **título**, y **StoreLogo300x300** (denominados **icono** en la página de listado de Store en el centro de partners). El campo **Título** es necesario y los otros dos campos son opcionales.

Ten en cuenta que debes importar y exportar descripciones de la Tienda por separado para cada complemento de tu aplicación navegando a la página de información general de envíos del complemento.


