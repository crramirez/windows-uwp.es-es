---
title: Modo de ventana frente a modo de pantalla completa
description: 'Las aplicaciones de Direct3D pueden ejecutarse en cualquiera de los dos modos de pantalla: completa o de ventana.'
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords: Modo de ventana frente a modo de pantalla completa
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 91b488dce85ed173584fdb9873884f9f36689858
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>Modo de ventana frente a modo de pantalla completa


Las aplicaciones de Direct3D pueden ejecutarse en cualquiera de los dos modos de pantalla: completa o de ventana. En el *modo de ventana*, la aplicación comparte el espacio de pantalla del escritorio disponible con todas las aplicaciones en ejecución. En el *modo de pantalla completa*, la ventana en la que la aplicación se ejecuta abarca todo el escritorio, ocultando todas las aplicaciones en ejecución (incluido el entorno de desarrollo). Por lo general, los juegos están de manera predeterminada en modo de pantalla completa para sumergir al usuario completamente en el juego ocultando todas las aplicaciones en ejecución.

Las diferencias en el código entre el modo de pantalla completa y el modo de ventana son muy pequeñas.

Dado que una aplicación que se ejecuta en modo de pantalla completa usa toda la pantalla, para depurar la aplicación se requiere un monitor independiente o el uso de un depurador remoto. Una de las ventajas de una aplicación de modo de ventana es que puede pasar el código en un depurador sin necesidad de varios monitores o un depurador remoto.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

 

 




