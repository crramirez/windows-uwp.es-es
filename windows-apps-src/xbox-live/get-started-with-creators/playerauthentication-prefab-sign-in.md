---
title: Inicie sesión con el recurso prefabricado PlayerAuthentication
description: Información general sobre el recurso prefabricado PlayerAuthentication de complemento de Unity
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605740"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>Inicio de sesión fácil con el recurso prefabricado PlayerAuthentication

El recurso prefabricado PlayerAuthentication es la manera más fácil para agregar autenticación de Xbox Live en su título. Sólo toma tres sencillos pasos para ir desde una nueva escena a una página de inicio de sesión.

1. Arrastre el recurso prefabricado PlayerAuthentication a la escena
2. Arrastre un prefabricado XboxLiveServices a la escena
3. Agregar un EventSystem a la escena (técnicamente el PlayerAuthentication creará uno automáticamente si una EventSystem no está presente, pero éste se agrega es un hábito.)

Y eso es todo. Ahora puede firmar un reproductor en XboxLive en su título haciendo clic en el prefabricado PlayerAuthentication en su escena. Probar la escena en Unity, haga clic en el botón Reproducir hará que el recurso prefabricado generar datos falsos, esto es porque el Reproductor de Unity no se puede conectar al servicio de Xbox Live. Para ver en un inicio de sesión real, deberá compilar el proyecto se ejecute localmente en Visual Studio. Si se ha configurado su título en el centro de partners y le ha autorizado una Microsoft cuenta/nombre de jugador para iniciar sesión en su título, a continuación, podrá iniciar sesión en una de las cuentas autorizadas en una compilación de Visual Studio.

Script de prefabricado de PlayerAuthentication tiene algunas opciones de configuración que se pueden manipular desde su vista en el inspector.

![Captura de pantalla de PlayerAuthentication inspector](../images/unity/playerauthentication_prefab_inspector.JPG)

* Número de Reproductor - dicta el Reproductor que esté vinculado al panel de inicio de sesión
* Tema: cambia la combinación de colores para el panel de inicio de sesión cuando un usuario se ha iniciado sesión o cerrar la sesión. Esta configuración tiene una opción clara u oscura.
* Habilitar entrada del controlador: esta casilla permite que los reproductores para utilizar un controlador de Xbox para inicio de sesión y cierre de sesión con el recurso prefabricado PlayerAuthentication.
* Número de joystick - dicta el controlador que puede iniciar sesión nuestro fuera con el recurso prefabricado.
* Inicie sesión en el botón: un menú desplegable que le permite elegir qué botón en un controlador Xbox inicia sesión un usuario.
* Botón Cerrar sesión: una lista desplegable que le permite elegir qué botón en un controlador Xbox cierra un usuario.

## <a name="multiplayer-scenario"></a>Escenario de varios jugadores

Además de inicio de sesión de un solo jugador, también puede utilizar varios prefabricados PlayerAuthentication para implementar varios jugadores local en los títulos de consola Xbox One. Adición de varias instancias de la prefabricado y cambiando el atributo de número de Reproductor de cada uno, puede iniciar sesión en varios usuarios su título.

> [!WARNING]
> No se permite iniciar sesión en varios nombres de jugadores en equipos con Windows 10. Para iniciar sesión en varios usuarios deberá probar el juego en un una consola Xbox.

Creación de una escena que permite varios jugadores solo es un poco más difícil utilizando el recurso prefabricado PlayerAuthentication.

1. Arrastre una instancia de la prefabricado PlayerAuthentication a la escena
2. Compruebe el **habilitar el controlador de entrada** cuadro en inspector de prefabricado.
3. Asegúrese de que el **Reproductor número** y **Joystick número** se establecen en 1.
4. Asignar el **botón de inicio de sesión** desde el menú desplegable.
5. Asignar el **botón Inicio de sesión** desde el menú desplegable.
6. Arrastre un *segundo* instancia de la prefabricado PlayerAuthentication a la escena.
7. Compruebe el **habilitar el controlador de entrada** cuadro en inspector de prefabricado.
8. Asegúrese de que el **Reproductor número** y **Joystick número** se establece en 2.
9. Asignar el **botón de inicio de sesión** desde el menú desplegable.
10. Asignar el **botón Inicio de sesión** desde el menú desplegable.
11. Arrastre un prefabricado XboxLiveServices a la escena
12. Agregar un EventSystem a la escena

Compruebe que el prefabricados son funciona correctamente presionando Play en el Reproductor de Unity y haga clic en el prefabricados. Se devuelven datos falsos que se espera que el Reproductor de Unity no se puede conectar a Xbox Live. Con dos instancias de la prefabricado PlayerAuthentication configurado para diferentes jugadores y joysticks está listo para crear su juego en Visual Studio, lo se puede probar correctamente en una consola Xbox. Una vez que se ha creado su juego, abra el archivo de solución en Visual Studio, deberá habilitar la compatibilidad de usuario múltiple para su juego.
Para ello:

1. Buscar en el Explorador de soluciones para el archivo package.appxmanifest.xml
2. A la derecha, haga clic en el archivo y seleccione Ver código
3. En el `<Properties><\/Properties>` sección, agregue la siguiente línea: ' < uap:SupportedUsers > varios <\/uap:SupportedUsers >.
4. Implementar el juego en Xbox iniciando una compilación de depuración remota desde Visual Studio. Puede encontrar instrucciones para configurar el título de una consola Xbox en el [configurar su UWP en el entorno de desarrollo de Xbox](../../xbox-apps/development-environment-setup.md) artículo.

> [!NOTE]
> El fragmento de configuración ha cambiado puede parecer que se está habilitando a multijugador pero sigue siendo necesario ejecutar el juego en escenarios de un solo jugador.

Cuando esté satisfecho con el PlayerAuthentication prefabricado trabajar que puede aprender más sobre un inicio de sesión scripting con el artículo [inicie sesión con el Administrador de inicio de sesión en Unity](sign-in-manager.md).