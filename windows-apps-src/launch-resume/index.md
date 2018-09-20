---
author: TylerMSFT
title: Inicio, reanudación y tareas en segundo plano
description: Esta sección describe lo que sucede cuando se inicia, suspende, reanuda y finaliza una aplicación de la Plataforma universal de Windows (UWP).
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.author: twhitney
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, la tarea en segundo plano, servicio de aplicaciones, los dispositivos, sistemas remotos conectados
ms.localizationpriority: medium
ms.openlocfilehash: d4aa5a4f379e0791e9da7db4ecd2a27c09cf0a3a
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4087774"
---
# <a name="launching-resuming-and-background-tasks"></a>Inicio, reanudación y tareas en segundo plano


Esta sección incluye información sobre los siguientes asuntos:

- Qué sucede cuando se inicia, suspende, reanuda y finaliza una aplicación para la Plataforma universal de Windows (UWP).
- Cómo iniciar aplicaciones mediante un identificador URI o mediante la activación de archivos.
- Cómo usar los servicios de aplicaciones, que permiten a la aplicación para la Plataforma universal de Windows (UWP) compartir datos y funcionalidades con otras aplicaciones.
- Cómo usar tareas en segundo plano, que permite a una aplicación para UWP realizar tareas aunque dicha aplicación no esté en primer plano.
- Cómo detectar dispositivos conectados, iniciar una aplicación en otro dispositivo y comunicarse con un servicio de aplicaciones en un dispositivo remoto, de modo que puedas crear experiencias de usuario que fluyan entre distintos dispositivos.
- Cómo elegir la tecnología adecuada para ampliar y separar tu aplicación por componentes.
- Cómo agregar y configurar una pantalla de presentación para la aplicación.
- Cómo ampliar tu aplicación a través de paquetes que los usuarios puedan instalar desde la Microsoft Store.

## <a name="the-app-lifecycle"></a>El ciclo de vida de la aplicación

En esta sección se detalla el ciclo de vida de una aplicación para la Plataforma universal de Windows (UWP) desde el momento en que se activa hasta que se cierra.

| Tema | Descripción |
|-------|-------------|
| [Ciclo de vida de la aplicación](app-lifecycle.md)               | Obtén información sobre el ciclo de vida de una aplicación para UWP y lo que sucede cuando Windows inicia, suspende y reanuda la aplicación. |
| [Administrar el inicio previo de aplicaciones](handle-app-prelaunch.md) | Aprende a administrar el inicio previo de aplicaciones.                                                                              |
| [Administrar la activación de aplicaciones](activate-an-app.md)     | Aprende a administrar la activación de aplicaciones.                                                                             |
| [Administrar la suspensión de la aplicación](suspend-an-app.md)         | Aprende a guardar datos importantes de la aplicación cuando el sistema la suspende.                                 |
| [Administrar la reanudación de la aplicación](resume-an-app.md)           | Aprende a actualizar el contenido mostrado cuando el sistema reanuda la aplicación.                                        |
| [Liberar memoria cuando la aplicación pase a segundo plano](reduce-memory-usage.md) | Aprende a reducir la cantidad de memoria que usa la aplicación cuando está en el estado en segundo plano para que no se finalice.|
| [Aplazar la suspensión de la aplicación con ejecución ampliada](run-minimized-with-extended-execution.md) | Aprender a usar la ejecución extendida para mantener la aplicación en ejecución cuando está minimizada |

## <a name="launch-apps"></a>Iniciar aplicaciones

| Tema | Descripción |
|-------|-------------|
| [Crear una aplicación de consola de la Plataforma universal de Windows](console-uwp.md) | Aprende a escribir una aplicación de la Plataforma universal de Windows que se ejecuta en una ventana de consola. |
| [Crear una aplicación para UWP de instancias múltiples](multi-instance-uwp.md) | Obtén información sobre cómo escribir una aplicación de la Plataforma universal de Windows de instancias múltiples. |

La sección [Iniciar una aplicación con un URI](launch-app-with-uri.md) describe cómo usar un identificador uniforme de recursos (URI) para iniciar una aplicación.

| Tema | Descripción |
|-------|-------------|
| [Iniciar la aplicación predeterminada de un URI](launch-default-app.md) | Aprende a iniciar la aplicación predeterminada de un identificador de recursos uniforme (URI). Los URI te permiten iniciar otra aplicación para realizar una tarea específica. En este tema también se proporciona una descripción general de los muchos esquemas de URI integrados en Windows. |
| [Administrar la activación de los identificadores URI](handle-uri-activation.md) | Aprende a registrar una aplicación para convertirla en el controlador predeterminado de un nombre de esquema de identificador uniforme de recursos (URI). |
| [Iniciar una aplicación para obtener resultados](how-to-launch-an-app-for-results.md) | Aprende a iniciar una aplicación desde otra aplicación y a intercambiar datos entre las dos. Esto se denomina iniciar una aplicación para obtener resultados. |
| [Elegir y guardar los tonos con el esquema de URI ms-tonepicker](launch-ringtone-picker.md) | En este tema se describe el esquema de URI ms-tonepicker y cómo usarlo para mostrar un selector de tono para seleccionar un tono, guardar un tono y obtener el nombre descriptivo de un tono. |
| [Cómo iniciar la aplicación Configuración de Windows](launch-settings-app.md) | Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI ms-settings. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en páginas de configuración específicas. |
| [Iniciar la aplicación Microsoft Store](launch-store-app.md) | En este tema se describe el esquema de URI ms-windows-store. La aplicación puede usar este esquema de URI para iniciar la aplicación para UWP en páginas específicas en la Store. |
| [Iniciar la aplicación Mapas de Windows](launch-maps-app.md) | Aprende a iniciar la aplicación Mapas de Windows desde la aplicación. |
| [Iniciar la aplicación Contactos](launch-people-apps.md) | En este tema se describe el esquema de URI ms-people. La aplicación puede usar este esquema de URI para iniciar la aplicación Contactos para acciones específicas. |
| [Admitir la vinculación de un sitio web con la aplicación mediante los controladores de URI de la aplicación](web-to-app-linking.md) | Controla la relación de los usuarios con tu aplicación mediante los controladores de URI de la aplicación. |

La sección [Iniciar una aplicación a través de la activación de archivos](launch-app-from-file.md) describe cómo configurar una aplicación para iniciarla cuando se abre un archivo de un tipo determinado.

| Tema | Descripción |
|-------|-------------|
| [Iniciar la aplicación predeterminada de un archivo](launch-the-default-app-for-a-file.md) | Aprende cómo iniciar la aplicación predeterminada de un archivo. |
| [Administrar la activación de archivos](handle-file-activation.md) | Aprende a registrar la aplicación para convertirla en el controlador predeterminado de un cierto tipo de archivo. |

A continuación puedes consulta otros temas relacionados con el inicio de una aplicación.

| Tema | Descripción |
|-------|-------------|
| [Continuar la actividad del usuario, incluso en diferentes dispositivos](useractivities.md) | Vuelve a captar el interés de los usuarios con tu aplicación, incluso en diferentes dispositivos, iniciando la aplicación en el punto en el que la dejó el usuario. |
| [Inicio automático con Reproducción automática](auto-launching-with-autoplay.md) | Puedes usar Reproducción automática para ofrecer tu aplicación como una opción cuando un usuario conecte un dispositivo a su PC. Esto incluye dispositivos que no son de volumen, como una cámara o un reproductor de medios, o dispositivos de volumen, como una unidad USB, una tarjeta SD o un DVD. |
| [Nombres de esquema de URI y archivo reservados](reserved-uri-scheme-names.md) | En este tema, se enumeran los nombres de esquema de URI y de archivo reservados que no están disponibles para la aplicación. |

## <a name="app-services-and-extensions"></a>Extensiones y servicios de aplicaciones

En la sección [Extensiones y servicios de aplicaciones](app-services.md) se describe cómo integrar los servicios de aplicaciones en la aplicación para UWP con el fin de permitir el uso compartido de datos y la funcionalidad entre distintas aplicaciones.

| Tema | Descripción |
|-------|-------------|
| [Crear y usar un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md) | Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo usar esos servicios. |
| [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) | Puedes convertir el código de servicio de aplicaciones que se ejecutaba en un proceso en segundo plano independiente en un código que se ejecute en el mismo proceso que el proveedor de servicios de aplicaciones. |
| [Ampliar tu aplicación con servicios de aplicaciones, extensiones y paquetes](extend-your-app-with-services-extensions-packages.md) | Determina qué tecnología usar para ampliar y separar tu aplicación por componentes y obtener una breve descripción de cada uno. |
| [Crear y usar una extensión de aplicación](how-to-create-an-extension.md) | Crea y hospeda las extensiones de aplicaciones de la Plataforma universal de Windows (UWP) para ampliar tu aplicación mediante paquetes que los usuarios pueden instalar desde la Microsoft Store. |

## <a name="background-tasks"></a>Tareas en segundo plano

La sección [Tareas en segundo plano](support-your-app-with-background-tasks.md) muestra cómo ejecutar código ligero en segundo plano como respuesta a desencadenadores.

| Tema | Descripción |
|-------|-------------|
| [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)                                       | Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano. |
| [Acceder a sensores y dispositivos desde una tarea en segundo plano](access-sensors-and-devices-from-a-background-task.md)   | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que la aplicación universal de Windows acceda a dispositivos periféricos y sensores en segundo plano, aunque la aplicación en primer plano esté suspendida. |
| [Crear y registrar una tarea en segundo plano en el proceso](create-and-register-an-inproc-background-task.md)       | Crea y registra una tarea en segundo plano que se ejecute en el mismo proceso que la aplicación en primer plano. |
| [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md)           | Crea y registra una tarea en segundo plano que se ejecute en un proceso independiente de la aplicación y regístralo para que se ejecute cuando tu aplicación no esté en primer plano. |
| [Migrar una tarea en segundo plano fuera de proceso en una tarea en segundo plano en proceso](convert-out-of-process-background-task.md) | Obtén información sobre cómo migrar una tarea en segundo plano fuera de proceso en una tarea en segundo plano en proceso que se ejecuta en el mismo proceso que la aplicación en primer plano.|
| [Depurar una tarea en segundo plano](debug-a-background-task.md)                                                       | Aprende a depurar una tarea en segundo plano, incluida la activación y el seguimiento de depuración de la tarea en segundo plano en el registro de eventos de Windows. |
| [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md) | Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación. |
| [Agrupar registro de tarea en segundo plano](group-background-tasks.md)                                             | Aísla el registro de tareas en segundo plano con grupos. |
| [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)                                 | Aprende a crear una tarea en segundo plano que reconozca solicitudes de cancelación y detenga el trabajo, y que informe de la cancelación a la aplicación a través del almacenamiento persistente. |
| [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)       | Aprende a diseñar tu aplicación para que reconozca el progreso y la finalización de una tarea en segundo plano. |
| [Optimizar la actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) |Aprende a reducir la energía usada en segundo plano e interactuar con la configuración de usuario en relación con la actividad en segundo plano. |
| [Registrar una tarea en segundo plano](register-a-background-task.md)                                                 | Aprende a crear una función que pueda reutilizarse para registrar de forma segura la mayoría de las tareas en segundo plano. |
| [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)         | Aprende a crear una tarea en segundo plano que responda a eventos [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). |
| [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)                                    | Aprende a programar una tarea en segundo plano única o a ejecutar una tarea en segundo plano periódica. |
| [Ejecutar en segundo plano de manera indefinida](run-in-the-background-indefinetly.md)                                    | Usa la funcionalidad para ejecutar una tarea en segundo plano o una sesión de ejecución extendida en segundo plano de manera indefinida. |
| [Desencadenar una tarea en segundo plano desde dentro de la aplicación](trigger-background-task-from-app.md) | Aprende a usar el [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para activar una tarea en segundo plano desde dentro de la aplicación.|
| [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)             | Aprende a establecer condiciones que controlan cuándo se ejecutará tu tarea en segundo plano. |
| [Transferir datos en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt280377)                 | Usa la API de transferencia en segundo plano para copiar archivos en segundo plano. |
| [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)                   | Usa una tarea en segundo plano para actualizar el icono dinámico de tu aplicación con contenido actualizado. |
| [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)                                                   | Aprende a usar la clase [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para ejecutar código ligero en segundo plano mientras el dispositivo está enchufado. |

## <a name="remote-systems"></a>Sistemas remotos

En la sección [Aplicaciones y dispositivos conectados (Project Rome)](connected-apps-and-devices.md) se describe cómo usar la plataforma de sistemas remotos para detectar dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.

| Tema | Descripción |
|-------|-------------|
| [Detectar dispositivos remotos](discover-remote-devices.md)  | Obtén información sobre cómo detectar dispositivos a los que te puedas conectar. |
| [Iniciar una aplicación en un dispositivo remoto](launch-a-remote-app.md) | Aprende a iniciar una aplicación en un dispositivo remoto.  |
| [Comunicarse con un servicio de aplicaciones remoto](communicate-with-a-remote-app-service.md) | Obtén información sobre cómo interactuar con una aplicación en un dispositivo remoto. |
| [Conectar dispositivos a través de sesiones remotas](remote-sessions.md) | Cree experiencias compartidas en varios dispositivos uniéndolos en una sesión remota. |

## <a name="splash-screens"></a>Pantallas de presentación

La sección [Pantallas de presentación](splash-screens.md) describe cómo establecer y configurar la pantalla de presentación de la aplicación.

| Tema | Descripción |
|-------|-------------|
| [Agregar una pantalla de presentación](add-a-splash-screen.md) | Establecer el color de imagen y de fondo de la pantalla de presentación de la aplicación. |
| [Mostrar una pantalla de presentación durante más tiempo](create-a-customized-splash-screen.md) | Muestra una pantalla de presentación más tiempo creando una pantalla de presentación extendida para tu aplicación. Esta pantalla extendida imita la pantalla de presentación cuando la aplicación se inicia, y se puede personalizar. |
