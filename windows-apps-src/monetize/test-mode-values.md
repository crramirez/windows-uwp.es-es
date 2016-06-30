---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "Usa los valores de identificador de aplicación y de unidad de anuncios de prueba de este artículo para ver cómo la aplicación representa los anuncios durante las pruebas."
title: Valores del modo de prueba
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 93b20954ba82b613bde96db30a000902dec3b844

---

# Valores del modo de prueba


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Si usas un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para mostrar anuncios en tu aplicación, debes especificar un identificador de aplicación y un identificador de unidad de anuncios. Mientras desarrollas la aplicación, usa los valores de identificador de aplicación y de identificador de anuncios de prueba para ver cómo la aplicación representa los anuncios durante las pruebas.

> **Importante** Si la aplicación usa la mediación de anuncios (es decir, usa un objeto **AdMediatorControl**), no es necesario especificar unidades de anuncios. En este escenario, las unidades de anuncios se generan automáticamente. Para obtener más información, consulta [Cuál es la diferencia: AdMediator o AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

Si intentas usar valores de prueba en tu aplicación después de publicarla, la aplicación dinámica no recibirá anuncios. Para recibir los anuncios en la aplicación publicada, debes actualizar el código para usar un identificador de aplicación y un identificador de unidad de anuncios proporcionados por el panel del Centro de desarrollo de Windows. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).
 

Estos son los valores de prueba que se usarán para los anuncios de banner e intersticiales en vídeo.

* Para los anuncios intersticiales en vídeo:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Para los anuncios de banner:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Importante** El tamaño de un anuncio dinámico está definido por las propiedades **Width** y **Height** de la clase **AdControl**. Para obtener resultados óptimos, asegúrate de que las propiedades **Width** y **Height** del código corresponden a uno de los [tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md). Las propiedades **Width** y **Height** no cambiarán en función del tamaño de un anuncio dinámico.



 

 



<!--HONumber=Jun16_HO4-->


