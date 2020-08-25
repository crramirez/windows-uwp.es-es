---
Description: Puede crear listas de tiendas para sus aplicaciones sin usar el centro de partners; para ello, exporte sus listados en un archivo. csv, escriba la información y los recursos y, a continuación, importe el archivo actualizado.
title: Importar y exportar listados de Store
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, lista de tiendas de importación, lista de tiendas de exportación, importación y exportación, almacenamiento de lista CSV
ms.localizationpriority: medium
ms.openlocfilehash: 8f2cbf0da1dfae05db428712c836835571372776
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846875"
---
# <a name="import-and-export-store-listings"></a>Importar y exportar listados de Store

En lugar de [escribir la información de las listas de tiendas directamente en el centro de Partners](create-app-store-listings.md), tiene la opción de agregar o actualizar la información mediante la exportación de los listados en un archivo. csv, la introducción de la información y los recursos, y la importación del archivo actualizado. Puede usar este método para crear listados desde cero o para actualizar listas que ya ha creado.

Esta opción es especialmente útil si desea crear o actualizar listas de tiendas para su producto en varios idiomas, ya que puede copiar y pegar la misma información en varios campos y realizar fácilmente cualquier cambio que deba aplicarse a idiomas concretos. Sin embargo, no puede usar este método para crear o actualizar [listas de tiendas específicas](create-platform-specific-store-listings.md) de la plataforma para aplicaciones publicadas previamente que admitan versiones anteriores del sistema operativo. 

> [!TIP]
> También puede usar esta característica para importar y exportar los detalles de la lista de tiendas de un complemento. En el caso de los complementos, el proceso funciona igual, salvo que solo se incluyen [los campos relevantes para los complementos](#add-ons) .

Tenga en cuenta que siempre puede crear o actualizar listas de programas directamente en el centro de Partners (incluso si ya ha usado el método Import/Export). La actualización directamente en el centro de Partners puede ser más fácil cuando se realiza un cambio sencillo, pero puede usar cualquiera de los métodos en cualquier momento.

## <a name="export-listings"></a>Exportar listados

En la página de información general del envío de una aplicación, haga clic en **exportar** lista (en la sección **almacenes** ) para generar un archivo. csv codificado en UTF-8. Guarde este archivo en una ubicación del equipo.

Puede usar Microsoft Excel u otro editor para editar este archivo. Tenga en cuenta que Microsoft 365 versiones de Excel le permitirá guardar un archivo. csv como **CSV UTF-8 (delimitado por comas) (*. csv)**, pero es posible que otras versiones no lo admitan. Puede encontrar detalles sobre las versiones de Excel que admiten esta característica en el [boletín de características nuevas de excel 2016](https://support.office.com/article/what-s-new-in-excel-for-office-365-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73?ui=en-US&rs=en-001&ad=US)y más información sobre la codificación como UTF-8 en varios editores [aquí](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Si aún no ha creado ninguna lista para el producto, el archivo. csv que exportó no contendrá ningún dato personalizado. Verá las columnas para **campo**, **identificador**, **tipo**y **valor predeterminado**, y filas que corresponden a todos los elementos que pueden aparecer en una lista de tiendas.

Si ya ha creado listas (o ha cargado paquetes), también verá las columnas etiquetadas con códigos de configuración regional de idioma que corresponden al idioma de cada una de las listas que ha creado (o que hemos detectado en los paquetes), así como a cualquier información de lista que haya proporcionado previamente.
     
Esta es una introducción a lo que contiene cada una de las columnas del archivo. csv exportado:
- La columna **campo** contiene un nombre que está asociado a cada parte de una lista de tiendas. Estos se corresponden con los mismos elementos que puede proporcionar al crear listas de tiendas en el centro de Partners, aunque algunos de los nombres son ligeramente diferentes. En el caso de los elementos en los que puede especificar más de un tipo de elemento, verá varias filas, hasta el número máximo que puede proporcionar. Por ejemplo, en el caso de **las características** de la aplicación, verá **Feature1**, **característica2**, etc., que se dirige a **Feature20** (ya que puede proporcionar hasta 20 características de aplicación).
- La columna **ID** contiene un número que el centro de Partners asocia a cada campo. 
- La columna **tipo** proporciona instrucciones generales sobre el tipo de información que se debe proporcionar para ese campo, como **texto** o **ruta de acceso relativa (o dirección URL al archivo en el centro de Partners)**. 
- La columna **predeterminada** (y las columnas etiquetadas con códigos de idioma y configuración regional) representan el texto o los recursos asociados a cada parte de la lista de tiendas. Puede editar los campos de estas columnas para hacer actualizaciones en los listados de la tienda.

>[!IMPORTANT]
> No cambie ninguna información de las columnas **campo**, **identificador**o **tipo** . La información de estas columnas debe permanecer sin cambios para que se pueda procesar el archivo importado.

## <a name="update-listing-info"></a>Actualizar información de lista

Una vez que haya exportado sus listas y guardado el archivo. csv, puede editar la información de la lista directamente en el archivo. csv. 

Junto con la columna **predeterminada** , cada idioma para el que haya creado una lista tiene su propia columna. Los cambios que realice en una columna se aplicarán a la descripción en ese idioma. Puede crear listas para los nuevos idiomas agregando el código de configuración regional de idioma en la siguiente columna vacía de la fila superior. Para obtener una lista de los códigos de idioma válidos, consulte [idiomas admitidos](supported-languages.md).

Puede usar la columna **predeterminada** para escribir la información que desea compartir entre todas las descripciones de su aplicación. Si el campo de un idioma determinado se deja en blanco, se usará la información de la columna predeterminada para ese idioma. Puede invalidar ese campo para un idioma determinado escribiendo información diferente para ese idioma.

La mayoría de los campos de la lista de tiendas son opcionales. La **Descripción** y una captura de pantalla son necesarias para cada lista. en el caso de los idiomas que no tienen paquetes asociados, también debe proporcionar un **título** para indicar cuál de los nombres de aplicación reservados deben usarse para esa lista. En el caso de todos los demás campos, puede dejar el campo vacío si no desea incluirlo en la lista. Recuerde que si deja en blanco un campo para un idioma determinado, se comprobará para ver si hay información en ese campo en la columna predeterminada. En ese caso, se usará esa información. 

Por ejemplo, considere el siguiente ejemplo: 

![Ejemplo de lista exportada](images/listingimport.png)
     
- El texto "Descripción predeterminada" se usará para el campo **Descripción** de las listas en-US y fr-fr. Sin embargo, el campo de **Descripción** de la lista de es-es usaría el texto "Spanish Description". 
- En el caso del campo **releasenotes** , el texto "Notas de la versión en inglés" se usará para en-US y se usará el texto "Notas de la versión francés" para fr-fr. Sin embargo, no se mostrarán notas de la versión para es-es.

Si no desea que se realicen modificaciones en un campo determinado, puede eliminar toda la fila de la hoja de cálculo, **con la excepción de las filas de los finalizadores y sus títulos y miniaturas asociados**. Aparte de para estos elementos, la eliminación de una fila no afectará a los datos asociados a ese campo en las listas. Esto le permite quitar las filas que no quiera editar, de modo que pueda centrarse en los campos en los que está realizando cambios.

La eliminación de la información de un campo para un idioma, sin quitar toda la fila, funciona de forma diferente, dependiendo del campo. En el caso de los campos cuyo **tipo** sea **texto**, si se elimina la información de un campo, simplemente se quitará esa entrada de la lista en ese idioma.  Sin embargo, la eliminación de la información en un campo de una imagen, como una captura de pantalla o un logotipo, no tendrá ningún efecto; la imagen anterior se seguirá usando a menos que la edite directamente en el centro de Partners. Al eliminar la información de un campo de finalizador, se quitará realmente ese finalizador del centro de Partners, por lo que debe asegurarse de que tiene una copia de los archivos necesarios antes de hacerlo.

Muchos de los campos de los listados exportados requieren la entrada de texto, como los del ejemplo anterior, **Description** y **releasenotes**. Para estos tipos de campos, basta con escribir el texto adecuado en el campo para cada idioma. Asegúrese de seguir la longitud y otros requisitos para cada campo. Para obtener más información sobre estos requisitos, consulte [crear listas](create-app-store-listings.md)de la tienda de aplicaciones.

Proporcionar información para los campos que se corresponden con los recursos, como las imágenes y los finalizadores, es un poco más complicado. En lugar de **texto**, el **tipo** de estos recursos es **ruta de acceso relativa (o dirección URL del archivo en el centro de Partners)**. 
     
Si ya ha cargado recursos para los listados de la tienda, estos recursos se representarán mediante una dirección URL. Estas direcciones URL se pueden reutilizar en varias descripciones de un producto, o incluso en diferentes productos dentro de la misma cuenta de desarrollador, por lo que puede copiar estas direcciones URL para reutilizarlas en un campo diferente si lo desea.

> [!TIP]
> Para confirmar qué recurso corresponde a una dirección URL, puede escribir la dirección URL en un explorador para ver la imagen (o descargar el vídeo del finalizador).  Debe haber iniciado sesión en su cuenta del centro de partners para que esta dirección URL funcione.

Si desea usar un nuevo recurso que no ha agregado previamente al centro de Partners, puede importar los listados como una carpeta, en lugar de hacerlo como un solo archivo. csv. Deberá crear una carpeta que contenga el archivo. csv. A continuación, agregue las imágenes de la misma carpeta, ya sea en la carpeta raíz o en una subcarpeta. Tendrá que escribir la ruta de acceso completa, incluido el nombre de la carpeta raíz, en el campo.

> [!TIP]
> Para obtener los mejores resultados al importar las listas como una carpeta, asegúrese de que está usando la versión más reciente de Microsoft Edge, Chrome o Firefox.

Por ejemplo, si la carpeta raíz se denomina **my_folder**y desea usar una imagen llamada **screenshot1.png** para **DesktopScreenshot1**, puede Agregar screenshot1.png a la raíz de esa carpeta y, a continuación, escribir **my_folder/screenshot1.png** en el campo **DesktopScreenshot1** . Si ha creado una carpeta images dentro de la carpeta raíz y, a continuación, se ha colocado screenshot1.jpg allí, debe escribir **my_folder screenshot1.png/images/ **. Tenga en cuenta que después de importar las listas mediante una carpeta, las rutas de acceso a las imágenes se convertirán en direcciones URL en los archivos del centro de Partners la próxima vez que exporte las listas. Puede copiar y pegar estas direcciones URL para usarlas de nuevo (por ejemplo, para usar los mismos recursos en varios lenguajes de lista). 

> [!IMPORTANT]
> Si la lista exportada incluye finalizadores, tenga en cuenta que al eliminar la dirección URL del finalizador o su imagen en miniatura del archivo. csv, se quitará por completo el archivo eliminado del centro de Partners y ya no podrá tener acceso a él (a menos que también se use en otro listado en el que no se haya eliminado). 

## <a name="import-listings"></a>Importar listas de programas

Una vez que haya especificado todos los cambios en el archivo. csv (e incluido los recursos que desea cargar), deberá guardar el archivo antes de cargarlo. Si utiliza una versión de Microsoft Excel que admite la codificación UTF-8, asegúrese de seleccionar **Guardar como** y use el formato **CSV UTF-8 (delimitado por comas) (*. csv)** . Si usa un editor diferente para ver y editar el archivo. csv, asegúrese de que el archivo. csv está codificado en UTF-8 antes de cargar.

Cuando esté listo para cargar el archivo. csv actualizado e importar los datos de la lista, seleccione **importar listas de programas** en la página de información general del envío. Si solo está importando un archivo. csv, elija **Importar. csv**, busque el archivo y haga clic en **abrir**. Si va a importar una carpeta con archivos de imagen, elija Importar carpeta, vaya a la carpeta y haga clic en **Seleccionar carpeta**. Asegúrese de que solo hay un archivo. csv en la carpeta, junto con los recursos que está cargando. 

A medida que se procesa el archivo. csv importado, verá una barra de progreso que refleja el estado de la importación y la validación. Esto puede tardar algún tiempo, especialmente si tiene muchas listas o archivos de imagen. 

Si se detectan problemas, verá una nota que indica que deberá realizar las actualizaciones necesarias y volver a intentarlo. Seleccione el vínculo **Ver errores** para ver los campos que no son válidos y por qué. Deberá corregir estos problemas en el archivo. csv (o reemplazar los recursos no válidos) y, a continuación, volver a importar los listados.

> [!TIP]
> Puede tener acceso a esta información más adelante a través del vínculo **ver los errores de la última importación** .

Ninguna información del archivo. csv se guardará en el centro de Partners hasta que se resuelvan todos los errores del archivo, incluso en el caso de los campos sin errores. Una vez que haya importado un archivo. csv que no tenga errores, la información de la lista que ha proporcionado se guardará en el centro de Partners y se usará para ese envío.

Puede continuar realizando actualizaciones en las listas importando otro archivo. csv actualizado o realizando cambios directamente en el centro de Partners.

## <a name="add-ons"></a>Complementos

En el caso de los complementos, la importación y exportación de listas de tiendas usa el mismo proceso descrito anteriormente, salvo que solo verá los tres campos relevantes para los [listados de tiendas de complementos](create-add-on-store-listings.md): **Descripción**, **título**y **StoreLogo300x300** (denominado **icono** en la página de la lista de tiendas del centro de Partners). El campo **title** es obligatorio y los otros dos campos son opcionales.

Tenga en cuenta que debe importar y exportar listas de tiendas por separado para cada complemento de la aplicación. para ello, vaya a la página de información general del envío del complemento.


