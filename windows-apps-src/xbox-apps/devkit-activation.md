---
title: Activación del modo de desarrollador de Xbox One
description: Cómo activar el modo de desarrollador para poder alternar entre el modo comercial y el modo de desarrollador.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 29bcb1b248b6b2392845962bb49eb11efed035f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174759"
---
# <a name="xbox-one-developer-mode-activation"></a>Activación del modo de desarrollador de Xbox One

## <a name="how-developer-mode-works"></a>Cómo funciona el modo de desarrollador
La consola Xbox One tiene dos modos: modo *comercial* (**1**) y modo de *desarrollador* (**2**). En el modo comercial, la consola está en el estado que cualquier cliente o usuario de una consola Xbox One usaría: puedes jugar a juegos y ejecutar aplicaciones como un usuario. En el modo de desarrollador, puedes desarrollar software para la consola, pero no puedes jugar a juegos comerciales ni ejecutar aplicaciones comerciales.

El modo de desarrollador se puede habilitar en cualquier consola Xbox One comercial. Una vez habilitado el modo de desarrollador, puede alternar entre los modos comercial (**2A**) y desarrollador (**2B**).

![Modos de Xbox One](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Activar el modo de desarrollador en la consola Xbox One comercial

1.  Inicia la consola Xbox One.

2.  Busque e instale la aplicación de **activación de modo de desarrollo** desde la tienda Xbox One.

    ![Instala la aplicación Dev Mode Activation.](images/devkit-activation-1.png)

3.  Inicie la aplicación desde la página de la tienda.

    ![Aplicación Dev Mode Activation](images/devkit-activation-2.png)

4.  Ten en cuenta el código que se muestra en la aplicación Dev Mode Activation.

    ![Paso de activación 5](images/activation-step-5.png)  
    
5.  [Registre una cuenta de desarrollador de aplicaciones en el centro de Partners](https://developer.microsoft.com/store/register).  Este es también el primer paso para publicar el juego.

6.  Inicie sesión en el [centro de Partners](https://partner.microsoft.com/dashboard) con su cuenta de desarrollador de aplicaciones del centro de Partners válida.  Si no ve varias opciones en el panel de navegación izquierdo o no ve la opción **crear una nueva aplicación** en la sección **información general** , los siguientes pasos y vínculos de activación _no funcionarán_; Asegúrese de que ha registrado completamente su cuenta de desarrollador de aplicaciones del paso anterior.

7.  Vaya a [Partner.Microsoft.com/xboxconfig/Devices](https://partner.microsoft.com/xboxconfig/devices).

8.  Escribe el código de activación que se muestra en la aplicación Dev Mode Activation. Tienes un número limitado de activaciones asociadas con tu cuenta. Una vez activado el modo de desarrollador, el centro de Partners indicará que ha usado una de las activaciones asociadas a su cuenta.

    ![Paso de activación 8](images/activation-step-8-rs2.png)    
    
9.  Haz clic en **Agree and activate**. De esta manera, volverá a cargarse la página y verás que el dispositivo completa datos en la tabla. Los términos del acuerdo del Programa de activación del modo de desarrollador de Xbox One están disponibles en [Programa de activación del modo de desarrollador de Xbox One](/legal/windows/agreements/xbox-one-developer-mode-activation).

10. Después de escribir el código de activación, la consola mostrará una pantalla de progreso para el proceso de activación.  
    
11. Cuando finalice la activación, abre la aplicación Dev Mode Activation y haz clic en **Cambiar y reiniciar** para ir al modo de desarrollador. Ten en cuenta que este proceso tardará más de lo habitual.

    ![Paso de activación 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Alternar entre el modo comercial y el de desarrollador
Una vez que se haya habilitado el modo de desarrollador en la consola, usa **Dev Home** para cambiar entre el modo comercial y el modo de desarrollador. Para obtener más información sobre cómo iniciar y usar dev Home, consulte [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).

* Para cambiar al modo de venta directa, Abra **dev Home**. En **acciones rápidas**, seleccione **abandonar el modo de desarrollo**. La consola se reiniciará en modo comercial.    

  ![Paso de activación 13](images/activation-step-13-rs4.png)  
  
* Para cambiar al modo de desarrollador, usa la aplicación Dev Mode Activation. Abra la aplicación y seleccione **cambiar y reiniciar**. La consola se reiniciará en modo de desarrollador.  

  ![Paso de activación 14](images/activation-step-12.png)  

## <a name="see-also"></a>Vea también
- [Desactivación del modo de desarrollador de Xbox One](devkit-deactivation.md)
- [UWP en Xbox One](index.md)