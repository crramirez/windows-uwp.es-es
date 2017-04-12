---
author: Mtoepke
title: "Desactivación del modo de desarrollador de Xbox One"
description: "Cómo desactivar el modo de desarrollador."
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 244124dd-d80a-4a72-91db-1c9c2fbc7c3c
ms.openlocfilehash: 46e9e013336f19aedafdb21e0417c878c7c7e8a1
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xbox-one-developer-mode-deactivation"></a>Desactivación del modo de desarrollador de Xbox One

* [Cambiar al modo comercial](#switch-to-retail-mode)
* [Desactivar la consola con la aplicación Dev Mode Activation](#deactivate-your-console-using-the-dev-mode-activation-app)  
* [Restablecer la consola](#reset-your-console)
* [Desactivar la consola con el Centro de desarrollo de Windows](#deactivate-your-console-using-windows-dev-center)

Si decides que ya no quieres usar la consola para el desarrollo, sigue estos pasos para desactivar el modo de desarrollador.

## <a name="switch-to-retail-mode"></a>Cambiar al modo comercial
En primer lugar, vuelve a definir el modo comercial a la consola Xbox One.

1. Abre **Dev Home**.
2. Haz clic en **Leave developer mode**.  La consola se reiniciará en modo comercial.  

   ![Salir del modo de desarrollador](images/deactivation-leave-dev-mode.png)

Ahora puedes desactivar la consola mediante uno de los siguientes métodos.

## <a name="deactivate-your-console-using-the-dev-mode-activation-app"></a>Desactivar la consola con la aplicación Dev Mode Activation

El método preferido de desactivar el modo de desarrollador en la consola es usar la aplicación Dev Mode Activation. 

1. Navega a **Mis juegos y aplicaciones** > **Aplicaciones**.
  
   ![Paso de activación 3](images/activation-step-3.png)    
   
2.  Abre la aplicación Dev Mode Activation.    
3.  Haz clic en **Desactivar**.
  
![Desactivar la consola](images/deactivation-app.png)

## <a name="reset-your-console"></a>Restablecer la consola

También puedes desactivar el modo de desarrollador restableciendo la consola.  

> [!NOTE]
> Al restablecer la consola, se perderán todos los datos guardados a nivel local.

Para restablecer la consola, realiza los siguientes pasos:

1.  Ve a **Mis juegos y aplicaciones**.  
2.  Selecciona **Aplicaciones** y, a continuación, selecciona **Configuración**.  
3.  Ve a **Sistema** en el panel izquierdo y, a continuación, selecciona **Console info & updates** en el panel derecho.  
4.  Ve a **Console info & updates**.  
   
    ![Información y actualizaciones de la consola](images/deactivation-console-info-updates.png)  
    
5.  Haz clic en **Reset console**.
    
    ![Restablecer la consola](images/deactivation-reset-console.png)
    
6.  A continuación, haz clic en **Reset and remove everything**. Esta opción restablece la consola a su estado comercial original.  Se eliminarán todas tus aplicaciones, juegos y datos guardados a nivel local. Ten en cuenta que, si eliges la otra opción, **Reset and keep my games & apps**, la consola no se quitará del programa para desarrolladores.  
   
    ![Restablecer y quitar todo](images/deactivation-reset-remove.png)

## <a name="deactivate-your-console-using-windows-dev-center"></a>Desactivar la consola con el Centro de desarrollo de Windows

Si no puedes acceder a la consola por cualquier motivo, también puedes desactivar el modo de desarrollador en la consola mediante el Centro de desarrollo de Windows.

1. Ve a [developer.microsoft.com/xboxdevices](https://developer.microsoft.com/xboxdevices).    
2. Inicia sesión con tu cuenta del Centro de desarrollo en el Centro de desarrollo.    
3. Busca la consola que quieras desactivar en la lista de consolas, comparando el número de serie, el identificador de la consola o el identificador de dispositivo.  
4. Haz clic en **Desactivar**.  
  
![Desactivar con el Centro de desarrollo](images/deactivation-devcenter.png)

Si anteriormente no has vuelto a definir el modo comercial a la consola Xbox One, hazlo ahora.

1. Inicia **Dev Home**.
2. Haz clic en **Leave developer mode**.  La consola se reiniciará en modo comercial.

![Paso de activación 13](images/deactivation-leave-dev-mode.png)

## <a name="see-also"></a>Consulta también
- [Activación del modo de desarrollador de Xbox One](devkit-activation.md)
- [UWP en Xbox One](index.md)
