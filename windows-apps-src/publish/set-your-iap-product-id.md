---
author: jnHs
Description: El primer paso para enviar un IAP (producto desde la aplicación) en el panel del Centro de desarrollo de Windows.
title: Establecer el id. de producto de IAP
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
---

# Establecer el id. de producto de IAP


El primer paso para enviar un IAP (producto desde la aplicación) en el panel del Centro de desarrollo de Windows. Este es el mismo identificador al que debes hacer referencia en [el código de la aplicación para llamar al IAP](https://msdn.microsoft.com/library/windows/apps/mt219684).

Un IAP debe estar asociado a una aplicación que ya hayas creado en el panel (incluso si todavía no la has enviado). Puedes encontrar el botón para **Crear un IAP nuevo** en la página **Introducción** de la aplicación o en su página **IAP**.

Una vez que hagas clic en el botón, verás la página **Crear IAP**. A continuación, escribe el id. de producto único que usarás para hacer referencia a la oferta en el código.

A continuación se detallan algunos aspectos que se deben tener en cuenta al elegir un id. de producto:

-   Los clientes no verán este id. de producto. (Más adelante, puedes escribir un [título y descripción](create-iap-descriptions.md) que se muestre a los clientes.)
-   No se puede cambiar ni eliminar el id. de producto del IAP una vez que se haya publicado.
-   Un id. de producto no puede tener más de 100 caracteres de longitud.
-   Un id. de producto no puede incluir ninguno de los caracteres siguientes: **&lt;&gt; \* % & : \\ ? + ,**
-   Para ofrecer el IAP en todos los dispositivos, debes usar solo caracteres alfanuméricos, puntos o guiones bajos. Si usas otros tipos de caracteres, el IAP no estará disponible para su compra para clientes que ejecutan Windows Phone 8.1 o versiones anteriores.
-   Un id. de producto no tiene por qué ser único en la Tienda Windows, pero debe ser exclusivo para tu cuenta de desarrollador.

 

 






<!--HONumber=May16_HO2-->


