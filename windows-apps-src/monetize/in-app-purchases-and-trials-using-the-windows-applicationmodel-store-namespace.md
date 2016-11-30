---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "Obtén información sobre cómo habilitar las pruebas y compras desde la aplicación en aplicaciones para UWP orientadas a versiones anteriores a Windows 10, versión 1607."
title: "Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store"
translationtype: Human Translation
ms.sourcegitcommit: 812fa1789c5c86657b8e73e45a851c7a58a1c84e
ms.openlocfilehash: 5a4f943357660a22217351f04d735c14cab828ff

---

# Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store

Windows SDK proporciona miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) que puedes usar para agregar la funcionalidad de pruebas y compras desde la aplicación a tu aplicación para la Plataforma universal de Windows (UWP) para ayudar a rentabilizar tu aplicación y agregar nuevas funciones. Estas API también proporcionan acceso a la información de licencia de la aplicación.

En los artículos de esta sección se ofrecen instrucciones detalladas y ejemplos de código para usar los miembros del espacio de nombres **Windows.ApplicationModel.Store** para varios escenarios comunes. Para obtener una introducción a los conceptos relacionados con las compras desde la aplicación en las aplicaciones para UWP, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).

Para obtener una muestra completa que demuestre cómo implementar pruebas y compras desde la aplicación con el espacio de nombres **Windows.ApplicationModel.Store**, consulta la [muestra de la Tienda](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

>**Notas**&nbsp;&nbsp;
>
> * Si la aplicación está destinada a Windows 10, versión 1607 o una versión posterior, se recomienda usar los miembros del espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) en lugar de los de **Windows.ApplicationModel.Store**. El espacio de nombres **Windows.Services.Store** admite los tipos de complemento más recientes, como los complementos de consumibles administrados por la Tienda, y está diseñado para ser compatible con futuros tipos de productos y características compatibles con el Centro de desarrollo de Windows y la Tienda. El espacio de nombres **Windows.Services.Store** también está diseñado para ofrecer un mejor rendimiento. Para obtener más información, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md).
<br/><br/>
> * El espacio de nombres **Windows.ApplicationModel.Store** no se admite en aplicaciones de escritorio de Windows que usan el [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop). Estas aplicaciones deben usar el espacio de nombres **Windows.Services.Store** para implementar compras desde la aplicación y periodos de prueba.

## En esta sección


| Tema                                                                                                       | Descripción                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)      |  Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.  |
| [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)      | Ofrece productos consumibles desde la aplicación (artículos que se pueden comprar, usar y volver a comprar), a través de la plataforma de comercio de la Tienda para proporcionar a tus clientes una experiencia de compra sólida y de confianza. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas. |
| [Excluir o limitar las características de una versión de prueba](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Si permites que los clientes puedan usar la aplicación gratis durante un período de prueba, puedes animarles a actualizar a la versión completa de tu aplicación excluyendo o limitando algunas características durante el período de prueba. |
| [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md)      |   Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar el catálogo.    |
| [Usar recibos para comprobar compras de productos](use-receipts-to-verify-product-purchases.md)      |   Cada transacción de la Tienda Windows que tiene como resultado una compra de producto satisfactoria también puede devolver un recibo de transacción que proporcione información sobre el producto enumerado y el coste abonado por el cliente. El acceso a esta información resulta útil en aquellos casos en los que la aplicación debe comprobar que un usuario compró tu aplicación o ha realizado compras de productos desde la aplicación en la TiendaWindows. |



<!--HONumber=Nov16_HO1-->


