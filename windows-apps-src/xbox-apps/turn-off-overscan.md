---
author: payzer
title: "Cómo desactivar el sobrebarrido"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# Cómo dibujar la interfaz de usuario hasta el borde de la pantalla   
De manera predeterminada, las aplicaciones tendrán bordes situados en los límites de la ventanilla. Esto es para tener en cuenta el área segura del televisor. Para obtener más información, consulta [Diseño para Xbox y televisión](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area).  Te recomendamos desactivar esta función y dibujar hasta el borde de la pantalla. Puedes dibujar hasta el borde de la pantalla si agregas el siguiente código cuando se inicia la aplicación:
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
Nota: Las aplicaciones de C++/DirectX no tienen que preocuparse de esto. El sistema siempre representará la aplicación hasta el borde de la pantalla.



<!--HONumber=Jun16_HO5-->


