---
author: Mtoepke
title: Desactivación del modo de desarrollador de Xbox One
description: Cómo desactivar el modo de desarrollador.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 244124dd-d80a-4a72-91db-1c9c2fbc7c3c
ms.localizationpriority: medium
ms.openlocfilehash: d1df8d475ec27ff3de7e3d6599e53ec3035cffc6
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "4462506"
---
# <a name="xbox-one-developer-mode-deactivation"></a>Desactivación del modo de desarrollador de Xbox One

Si decides que ya no quieres usar la consola para el desarrollo, sigue estos pasos para desactivar el modo de desarrollador.

## <a name="switch-to-retail-mode"></a>Cambiar al modo comercial

En primer lugar, vuelve a definir el modo comercial a la consola Xbox One.

1. Abre **Dev Home**.

2. Selecciona **Leave Dev Mode**.  La consola se reiniciará en modo comercial.  

   ![Salir del modo de desarrollador](images/devkit-deactivation-1.png)

Ahora puedes desactivar la consola mediante uno de los siguientes métodos.

## <a name="deactivate-your-console-using-the-dev-mode-activation-app"></a>Desactivar la consola con la aplicación Dev Mode Activation

El método preferido de desactivar el modo de desarrollador en la consola es usar la aplicación **Dev Mode Activation**. 

1. Desplázate a **Juegos y aplicaciones** > **Aplicaciones**.
  
   ![Paso de activación 3](images/devkit-deactivation-5.png)    
   
2.  Abre la aplicación Dev Mode Activation.

3.  Selecciona **Desactivar**.
  
    ![Desactivar la consola](images/deactivation-app.png)

Consulta [Activación del modo de desarrollador de Xbox One](devkit-activation.md) para obtener más información sobre la aplicación **Dev Mode Activation**. 

## <a name="reset-your-console"></a>Restablecer la consola

También puedes desactivar el modo de desarrollador restableciendo la consola.  

> [!NOTE]
> Al restablecer la consola, se perderán todos los datos guardados a nivel local.

Para restablecer la consola, realiza los siguientes pasos:

1.  Ve a **Mis juegos y aplicaciones**.

2.  Selecciona **Aplicaciones** y, a continuación, selecciona **Configuración**.

3.  Ve a **Sistema** en el panel izquierdo y, a continuación, selecciona **Información de la consola** en el panel derecho.   
   
    ![Información y actualizaciones de la consola](images/devkit-deactivation-2.png)  
    
4.  Selecciona **Restablecer la consola**.
    
    ![Restablecer la consola](images/devkit-deactivation-3.png)
    
5.  A continuación, selecciona **Restablecer y quitar todo**. Esta opción restablece la consola a su estado comercial original.  Se eliminarán todas tus aplicaciones, juegos y datos guardados a nivel local. Ten en cuenta que, si eliges la otra opción, **Reset and keep my games & apps**, la consola no se quitará del programa para desarrolladores.  
   
    ![Restablecer y quitar todo](images/devkit-deactivation-4.png)

## <a name="deactivate-your-console-using-windows-dev-center"></a>Desactivar la consola con el Centro de desarrollo de Windows

Si no puedes acceder a la consola por cualquier motivo, también puedes desactivar el modo de desarrollador en la consola mediante el Centro de desarrollo de Windows.

1. Desplázate a la página [Administrar consolas Xbox One](https://partner.microsoft.com/xboxdevices) en el Centro de desarrollo. Es posible que se te pida que inicies sesión con la cuenta del Centro de desarrollo.

2. Busca la consola que quieras desactivar en la lista de consolas, comparando el número de serie, el identificador de la consola o el identificador de dispositivo.  

3. Haz clic en **Desactivar**.  
  
![Desactivar con el Centro de desarrollo](images/devkit-deactivation-6.png)

Si anteriormente no has vuelto a definir el modo comercial a la consola Xbox One, hazlo ahora como se describe en [Cambiar al modo comercial](#switch-to-retail-mode).

## <a name="see-also"></a>Ver también
- [Activación del modo de desarrollador de Xbox One](devkit-activation.md)
- [UWP en Xbox One](index.md)
