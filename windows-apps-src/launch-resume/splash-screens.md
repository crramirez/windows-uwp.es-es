---
author: PatrickFarley
title: "Pantallas de presentación"
description: "En esta sección se describe cómo establecer y configurar la pantalla de presentación de la aplicación."
translationtype: Human Translation
ms.sourcegitcommit: 023e37bb6f9b8d8d780bb0a536cb469c87a31ed4
ms.openlocfilehash: bb0f3ff68b7dc15623e4f337e589989354e08b65

---

# <a name="splash-screens"></a>Pantallas de presentación

Todas las aplicaciones para UWP deben tener una pantalla de presentación, que es una combinación de una imagen y un color de fondo, ambos personalizables.

La pantalla de presentación se muestra de inmediato cuando el usuario inicia la aplicación. De este modo, los usuarios reciben comentarios inmediatamente mientras se inicializan los recursos de la aplicación. En cuanto la aplicación está lista para la interacción, se descarta la pantalla de presentación.

Una pantalla de presentación bien diseñada puede hacer que tu aplicación sea más atractiva. A continuación mostramos una pantalla de presentación sencilla:

![Una captura de pantalla a una escala del 75 % de la pantalla de presentación desde la muestra de pantalla de presentación.](images/regularsplashscreen.png)

Esta pantalla de presentación se crea mediante la combinación de un color de fondo verde con una imagen PNG en segundo plano transparente.

Una imagen sencilla con un color de fondo tiene un gran aspecto, independientemente del dispositivo en el que se ejecute la aplicación. Solo el tamaño del fondo cambia para compensar distintos tamaños de pantalla. Tu imagen siempre permanece intacta.

Además, puedes usar la clase [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) para personalizar la experiencia de inicio de la aplicación. Puedes colocar una pantalla de presentación extendida, creada por ti, para permitir que la aplicación tenga más tiempo para completar tareas adicionales, como preparar la interfaz de usuario de la aplicación o finalizar las operaciones de red. También puedes usar la clase **SplashScreen** para que te notifique cuando se descarte la pantalla de presentación de modo que puedas iniciar las animaciones de entrada.

| Tema | Descripción |
|-------|-------------|
| [Agregar una pantalla de presentación](add-a-splash-screen.md) | Establecer el color de imagen y de fondo de la pantalla de presentación de la aplicación. |
| [Mostrar una pantalla de presentación durante más tiempo](create-a-customized-splash-screen.md) | Muestra una pantalla de presentación más tiempo creando una pantalla de presentación extendida para tu aplicación. Esta pantalla extendida imita la pantalla de presentación cuando la aplicación se inicia, y se puede personalizar. |


<!--HONumber=Dec16_HO1-->

