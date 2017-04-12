---
author: Mtoepke
title: "Introducción a las aplicaciones multiusuario"
description: "Una introducción sencilla de nivel superior que describe el modelo multiusuario de Xbox."
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
ms.openlocfilehash: b150b50c1072a96ae0017bae848eeff94bb07ce0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="introduction-to-multi-user-applications"></a>Introducción a las aplicaciones multiusuario

En este tema realizaremos una sencilla introducción de nivel superior para describir el modelo multiusuario de Xbox.

El modelo de usuario de Xbox One se ajusta a los requisitos de una consola de juegos que permite que varios usuarios puedan jugar de forma cooperativa en el mismo dispositivo. Asimismo, permite que varios usuarios, cada uno con su propio controlador, inicien sesión y usen la consola al mismo tiempo en una sola sesión interactiva. Esto es diferente en otros dispositivos Windows. Por ejemplo:
* Los **equipos de escritorio de Windows** permiten que varios usuarios usen el mismo dispositivo, pero cada uno de ellos tiene su propia sesión interactiva que, a su vez, es totalmente independiente de otras sesiones del dispositivo.
* Los **teléfonos Windows** solo permiten que un único usuario use el dispositivo. Ese usuario único se determina durante la configuración rápida y no podrá cerrar la sesión una vez la haya iniciado. De hecho, si otro usuario quiere usar el dispositivo, deberá restablecerlo. 
* La consola **Xbox One** permite que varios usuarios inicien sesión y usen el dispositivo simultáneamente en una única sesión interactiva.

Cada usuario del modelo de usuario de Xbox One cuenta con una cuenta de usuario local. Esta cuenta de usuario local se asocia con una cuenta de Xbox Live y, por lo tanto, con una cuenta de Microsoft. Esto significa que existe una asignación estricta en lo que respecta a una relación uno a uno entre una cuenta de usuario Xbox y las cuentas de Xbox Live y de Microsoft.

## <a name="single-user-applications"></a>Aplicaciones de usuario único
De forma predeterminada, las aplicaciones para Plataforma universal de Windows (UWP) se ejecutan en el contexto del usuario que inició la aplicación. Estas *aplicaciones de usuario único* (SUA) solo tienen en cuenta a ese único usuario y se ejecutan en un modo que sea compatible con el modelo que el usuario tenga en otros dispositivos Windows. El modelo de usuario de Xbox administra qué usuario está asociado con la aplicación y garantiza que el usuario inicie sesión cuando este inicie la aplicación. En este modelo, los creadores de juegos y aplicaciones para UWP no deben hacer nada especial para ejecutarlas en Xbox. 

## <a name="multi-user-applications"></a>Aplicaciones multiusuario
Los juegos para UWP pueden formar parte del modelo multiusuario de Xbox One. Estas *aplicaciones multiusuario* (MUA) se ejecutan en el contexto de una cuenta de sistema (denominada Cuenta predeterminada) y pueden aprovechar al máximo la flexibilidad y eficacia que les ofrece el modelo de usuario de XboxOne. Para este tipo de juegos, el modelo de usuario de Xbox no se encarga de administrar qué usuario está asociado con el juego y tampoco es necesario que el usuario inicie sesión para poder ejecutar el juego. Esto significa que estos juegos deben escribirse para que quede claro cuáles son los requisitos de usuario y cómo deben administrarse; esto es, si es necesario que el usuario inicie sesión, si se debe implementar el concepto de "usuario actual", si se deben permitir entradas de contenido simultáneas de varios usuarios, etc.
   
Para formar parte del modelo multiusuario:   
1. Abre el proyecto en Visual Studio.   
2. Selecciona el archivo package.appxmanifest.xml.   
3. Haz clic con el botón derecho en **Ver código** y selecciónalo.   
4. Agrega la siguiente línea en la sección `<Properties></Properties>`:

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>Identificación de usuarios y entradas
Los desarrolladores pueden usar KeyRoutedEventArgs.DeviceId, que se usa en lo eventos enrutados KeyUp y KeyDown, para diferenciar los eventos generados desde diferentes modos de entrada.
Con el método Windows.System.UserDeviceAssociation.FindUserFromDeviceId, la identificación del usuario asociado a una entrada específica resultará más sencilla.

Consulta el tema [KeyRoutedEventArgs.DeviceId](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.keyroutedeventargs.deviceid) para obtener más información.


## <a name="guidance-on-which-model-to-choose"></a>Orientación sobre qué modelo elegir
Todas las aplicaciones para UWP y la mayoría de los juegos de usuario único pueden escribirse para que sean de tipo SUA. Te recomendamos que uses el modelo multiusuario de Xbox One en juegos multijugador.

## <a name="see-also"></a>Consulta también
- [UWP en Xbox One](index.md)
