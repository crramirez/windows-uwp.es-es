---
author: Mtoepke
title: "Introducción a las aplicaciones multiusuario"
description: 
area: Xbox
ms.sourcegitcommit: f225811bd18be22807160e8670a1b7b8d51e4b10
ms.openlocfilehash: 20f84783131122343fd01e6cb1f5a60cf50158cd

---

# Introducción a las aplicaciones multiusuario

En este tema realizaremos una sencilla introducción de nivel superior para describir el modelo multiusuario de Xbox.

> **Nota**
            &nbsp;&nbsp;En esta versión preliminar para desarrolladores, las aplicaciones multiusuario no están habilitadas. Las habilitaremos en un futuro y para entonces publicaremos documentación, guías y muestras con información más detallada. 

El modelo de usuario de Xbox One se ajusta a los requisitos de una consola de juegos que permite que varios usuarios puedan jugar de forma cooperativa usando el mismo dispositivo. Asimismo, permite que varios usuarios, cada uno con su propio controlador, inicien sesión y usen la consola al mismo tiempo en una sola sesión interactiva. Esto es diferente en otros dispositivos Windows. Por ejemplo:
* Los **equipos de escritorio de Windows** permiten que varios usuarios usen el mismo dispositivo, pero cada uno de ellos tiene su propia sesión interactiva que, a su vez, es totalmente independiente de otras sesiones del dispositivo.
* Los **teléfonos Windows** solo permiten que un único usuario use el dispositivo. Ese usuario único se determina durante la configuración rápida y no podrá cerrar la sesión una vez la haya iniciado. De hecho, si otro usuario quiere usar el dispositivo, deberá restablecerlo. 
* La consola **Xbox One** permite que varios usuarios inicien sesión, para que así puedan usar el dispositivo a la vez en una única sesión interactiva.

Cada usuario del modelo de usuario de Xbox One cuenta con una cuenta de usuario local. Esta cuenta de usuario local se asocia con una cuenta de Xbox Live y, por lo tanto, con una cuenta de Microsoft. Esto significa que existe una asignación estricta en lo que respecta a una relación uno a uno entre una cuenta de usuario Xbox y las cuentas de Xbox Live y de Microsoft.

## Aplicaciones de usuario único
De forma predeterminada, son las aplicaciones para UWP que se ejecutan en el contexto del usuario que inició la aplicación. Estas "aplicaciones de usuario único" (SUA en sus siglas en inglés), solo tiene en cuenta a ese único usuario y se ejecutan en un modo que sea compatible con el modelo que el usuario tenga en otros dispositivos Windows. El modelo de usuario de Xbox administra qué usuario está asociado con la aplicación y garantiza que el usuario inicie sesión cuando este inicie la aplicación. En este modelo, los autores de juegos y aplicaciones para UWP no deben hacer nada especial para ejecutarlas en Xbox. 

## Aplicaciones multiusuario
Los juegos para UWP pueden formar parte del modelo multiusuario de Xbox One. Estas "aplicaciones multiusuario" (MUA), se ejecutan en el contexto de una cuenta de sistema (denominada Cuenta predeterminada), y pueden aprovechar al máximo la flexibilidad y eficacia que les ofrece el modelo de usuario de Xbox One. Para este tipo de juegos, el modelo de usuario de Xbox no se encarga de administrar qué usuario está asociado con el juego y tampoco es necesario que el usuario inicie sesión para poder ejecutar el juego. Esto significa que estos juegos deben escribirse para que quede claro cuáles son los requisitos de usuario y cómo deben administrarse; esto es, si es necesario que el usuario inicie sesión, si se debe implementar el concepto de "usuario actual", si se deben permitir entradas de contenido simultáneas de varios usuarios, etc.
   
Para formar parte del modelo multiusuario:   
1. Abre el proyecto en Visual Studio.   
2. Selecciona el archivo package.appxmanifest.xml.   
3. Haz clic con el botón secundario en Ver código y selecciónalo.   
4. Agrega la siguiente línea en la sección `<Properties></Properties>`:

`<uap:SupportedUsers>multiple</uap:SupportedUsers>`

##Orientación sobre qué modelo elegir
Todas las aplicaciones para UWP y la mayoría de los juegos de usuario único pueden escribirse para que sean de tipo SUA. Te recomendamos que uses el modelo multiusuario de Xbox One en juegos cooperativos. Te proporcionaremos documentación, guías y muestras con información más detallada en una futura vista previa de desarrollador.

## Consulta también
- [UWP on Xbox One (UWP en Xbox One)](index.md)



<!--HONumber=Jun16_HO4-->


