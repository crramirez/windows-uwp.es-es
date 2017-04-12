---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "Usa los valores del identificador de aplicación y de la unidad de anuncios de prueba de este artículo para ver cómo la aplicación representa los anuncios durante las pruebas."
title: Valores del modo de prueba
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, pruebas
ms.openlocfilehash: ec5b1c1723a7f58b20234d703fc786647268a3b3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="test-mode-values"></a>Valores del modo de prueba

Si usas un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para mostrar anuncios en tu aplicación, debes especificar un identificador de aplicación y un identificador de unidad de anuncios. Mientras desarrollas la aplicación, usa los valores de identificador de aplicación y de identificador de anuncios de prueba para ver cómo la aplicación representa los anuncios durante las pruebas.

Si intentas usar valores de prueba en tu aplicación después de publicarla, la aplicación dinámica no recibirá anuncios. Para recibir los anuncios en la aplicación publicada, debes actualizar el código para usar un identificador de aplicación y un identificador de unidad de anuncios proporcionados por el panel del Centro de desarrollo de Windows. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).
 
Estos son los valores de prueba que se usarán para los anuncios de banner e intersticiales.

* Para anuncios intersticiales:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Sistema operativo de destino</th>
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>Windows 8.x y Windows Phone 8.x</p></td>
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Para anuncios de banner:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Sistema operativo de destino</th>
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>Windows 8.x y Windows Phone 8.x</p></td>
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Importante**&nbsp;&nbsp;Para **AdControl**, el tamaño de un anuncio dinámico se define por las propiedades **Width** y **Height**. Para obtener resultados óptimos, asegúrate de que las propiedades **Width** y **Height** del código corresponden a uno de los [tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md). Las propiedades **Width** y **Height** no cambiarán en función del tamaño de un anuncio dinámico.


 

 
