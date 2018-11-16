---
author: Mtoepke
title: Activación del modo de desarrollador de Xbox One
description: Cómo activar el modo de desarrollador para poder alternar entre el modo comercial y el modo de desarrollador.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: bf9bbc2962bc0d11f9969bf41840be515c9c0881
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7099783"
---
# <a name="xbox-one-developer-mode-activation"></a>Activación del modo de desarrollador de Xbox One

## <a name="how-developer-mode-works"></a>Cómo funciona el modo de desarrollador
Xbox One tiene dos modos, el modo *comercial* (**1**) y el modo de *desarrollador* (**2**). En el modo comercial, la consola está en el estado que cualquier cliente o usuario de una consola Xbox One usaría: puedes jugar a juegos y ejecutar aplicaciones como un usuario. En el modo de desarrollador, puedes desarrollar software para la consola, pero no puedes jugar a juegos comerciales ni ejecutar aplicaciones comerciales.

El modo de desarrollador se puede habilitar en cualquier consola Xbox One comercial. Después de habilitar el modo de desarrollador, puedes alternar rápidamente entre los modos comercial (**2a**) y de desarrollador (**2b**).

![Modos de Xbox One](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Activar el modo de desarrollador en la consola Xbox One comercial

1.  Inicia la consola Xbox One.

2.  Busca e instala la aplicación **Dev Mode Activation** desde la tienda de Xbox One.

    ![Instala la aplicación Dev Mode Activation.](images/devkit-activation-1.png)

3.  Inicia la aplicación desde la página de Store.

    ![Aplicación Dev Mode Activation](images/devkit-activation-2.png)

4.  Ten en cuenta el código que se muestra en la aplicación Dev Mode Activation.

    ![Paso de activación 5](images/activation-step-5.png)  
    
5.  Ve a [partner.microsoft.com/xboxactivate](https://partner.microsoft.com/xboxactivate).

6.  Inicia sesión con tus credenciales de cuenta en [El centro de partners](https://partner.microsoft.com/dashboard) .

7.  Escribe el código de activación que se muestra en la aplicación Dev Mode Activation. Tienes un número limitado de activaciones asociadas con tu cuenta. Una vez que se ha activado el modo de desarrollador, el centro de partners indicará que has usado una de las activaciones asociadas con tu cuenta.

    ![Paso de activación 8](images/activation-step-8-rs2.png)    
    
8.  Haz clic en **Agree and activate**. De esta manera, volverá a cargarse la página y verás que el dispositivo completa datos en la tabla. Los términos del acuerdo del Programa de activación del modo de desarrollador de Xbox One están disponibles en [Programa de activación del modo de desarrollador de Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760399).

9.  Después de escribir el código de activación, la consola mostrará una pantalla de progreso para el proceso de activación.  
    
10. Cuando finalice la activación, abre la aplicación Dev Mode Activation y haz clic en **Cambiar y reiniciar** para ir al modo de desarrollador. Ten en cuenta que este proceso tardará más de lo habitual.

    ![Paso de activación 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Alternar entre el modo comercial y el de desarrollador
Una vez que se haya habilitado el modo de desarrollador en la consola, usa **Dev Home** para cambiar entre el modo comercial y el modo de desarrollador. Para obtener más información sobre cómo iniciar y usar Dev Home, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).

* Para cambiar al modo comercial, abre **Dev Home**. En **Quick Actions**, selecciona **Leave Dev Mode**. La consola se reiniciará en modo comercial.    

  ![Paso de activación 13](images/activation-step-13-rs4.png)  
  
* Para cambiar al modo de desarrollador, usa la aplicación Dev Mode Activation. Abre la aplicación y selecciona **Switch and restart**. La consola se reiniciará en modo de desarrollador.  

  ![Paso de activación 14](images/activation-step-12.png)  

## <a name="see-also"></a>Consulta también
- [Desactivación del modo de desarrollador de Xbox One](devkit-deactivation.md)
- [UWP en Xbox One](index.md)
