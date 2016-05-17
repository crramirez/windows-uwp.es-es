---
author: mcleblanc
title: Inicio, reanudación y tareas en segundo plano
description: Esta sección describe lo que sucede cuando se inicia, suspende, reanuda y finaliza una aplicación de la Plataforma universal de Windows (UWP).
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
---

# Inicio, reanudación y tareas en segundo plano


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Esta sección describe lo que sucede cuando se inicia, suspende, reanuda y finaliza una aplicación de la Plataforma universal de Windows (UWP). Trata cómo activar aplicaciones mediante un contrato o una extensión, y cómo usar tareas en segundo plano que permitan que una aplicación para UWP funcione incluso cuando la aplicación no esté en primer plano. Por último, describe cómo agregar una pantalla de presentación a la aplicación.

## Ciclo de vida de la aplicación

| Tema                                            | Descripción                                                                                                     |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Ciclo de vida de la aplicación](app-lifecycle.md)               | Obtén información sobre el ciclo de vida de una aplicación para UWP y lo que sucede cuando Windows inicia, suspende y reanuda la aplicación. |
| [Administrar el inicio previo de aplicaciones](handle-app-prelaunch.md) | Aprende a administrar el inicio previo de aplicaciones.                                                                              |
| [Administrar la activación de aplicaciones](activate-an-app.md)     | Aprende a administrar la activación de aplicaciones.                                                                             |
| [Administrar la suspensión de la aplicación](suspend-an-app.md)         | Aprende a guardar datos importantes de la aplicación cuando el sistema la suspende.                                 |
| [Administrar la reanudación de la aplicación](resume-an-app.md)           | Aprende a actualizar el contenido mostrado cuando el sistema reanuda la aplicación.                                        |

 

## Iniciar aplicaciones


| Activación de archivos y URI                                                                         | Descripción                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Iniciar una aplicación para obtener resultados](how-to-launch-an-app-for-results.md)                               | Aprende a iniciar una aplicación desde otra aplicación e intercambiar datos entre las dos.                                                                                             |
| [Iniciar la aplicación predeterminada de un URI](launch-default-app.md)                                      | Aprende a iniciar la aplicación predeterminada de un identificador de recursos uniforme (URI).                                                                                               |
| [Administración de la activación de URI](handle-uri-activation.md)                                              | Aprende a registrar una aplicación para convertirla en el controlador predeterminado de un nombre de esquema de URI.                                                                                          |
| [Iniciar la aplicación predeterminada de un archivo](launch-the-default-app-for-a-file.md)                      | Aprende a iniciar la aplicación predeterminada de un tipo de archivo.                                                                                                                       |
| [Administrar la activación de archivos](handle-file-activation.md)                                            | Aprende a registrar la aplicación para convertirla en el controlador predeterminado de un tipo de archivo.                                                                                                  |
| [Directrices sobre tipos de archivo y URI](https://msdn.microsoft.com/library/windows/apps/hh700321) | Si comprendes la relación entre las aplicaciones para UWP y los protocolos y tipos de archivo que estas admiten, podrás proporcionar una experiencia más coherente y elegante a los usuarios. |
| [Nombres de esquema de URI y archivo reservados](reserved-uri-scheme-names.md)                             | Este tema enumera los nombres de esquema de URI y de archivo reservados que no están disponibles para la aplicación.                                                                                |
| Activar aplicaciones integradas                                                                          | Descripción                                                                                                                                                                |
| [Iniciar la aplicación Configuración de Windows](launch-settings-app.md)                                      | Aprende a iniciar la aplicación Configuración de Windows.                                                                                                                              |
| [Iniciar la aplicación de la Tienda Windows](launch-store-app.md)                                            | Aprende a iniciar la aplicación de la Tienda Windows.                                                                                                                                 |
| [Iniciar la aplicación Mapas de Windows](launch-maps-app.md)                                              | Aprende a iniciar la aplicación Mapas de Windows.                                                                                                                                  |

 

## Servicios y tareas en segundo plano



| Tema                                                                                                            | Descripción                                                                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md)                             | Los temas de esta sección muestran cómo ejecutar tu propio código ligero en segundo plano al responder a los desencadenadores con tareas en segundo plano.                                                       |
| [Acceder a sensores y dispositivos desde una tarea en segundo plano](access-sensors-and-devices-from-a-background-task.md)       | [
              **DeviceUseTrigger**
            ](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que tu aplicación universal de Windows acceda a dispositivos periféricos y sensores en segundo plano, incluso cuando la aplicación en primer plano esté suspendida. |
| [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)                                           | Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano.                                                                                                                          |
| [Crear y usar un servicio de aplicación](how-to-create-and-consume-an-app-service.md)                                | Aprende a escribir una aplicación para UWP que puede proporcionar servicios a otras aplicaciones para UWP y a usar esos servicios.                                                                                  |
| [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md)                               | Crea una tarea en segundo plano y regístrala para ejecutarla cuando tu aplicación no esté en primer plano.                                                                                                 |
| [Depurar una tarea en segundo plano](debug-a-background-task.md)                                                           | Aprende a depurar una tarea en segundo plano, incluida la activación y el seguimiento de depuración de la tarea en segundo plano en el registro de eventos de Windows.                                                                        |
| [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md) | Habilita el uso de tareas en segundo plano declarándolas como extensiones en el manifiesto de la aplicación.                                                                                                       |
| [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)                                     | Aprende a crear una tarea en segundo plano que reconozca solicitudes de cancelación y detenga el trabajo, y que informe de la cancelación a la aplicación a través del almacenamiento persistente.                                     |
| [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)           | Aprende a diseñar tu aplicación para que reconozca el progreso y la finalización de una tarea en segundo plano.                                                                                                                     |
| [Registrar una tarea en segundo plano](register-a-background-task.md)                                                     | Aprende a crear una función que pueda reutilizarse para registrar de forma segura la mayoría de las tareas en segundo plano.                                                                                                  |
| [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)             | Aprende a crear una tarea en segundo plano que responda a eventos [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).                                                                         |
| [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)                                        | Aprende a programar una tarea en segundo plano única o ejecutar una tarea en segundo plano periódica.                                                                                                          |
| [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)                 | Aprende a establecer condiciones que controlan cuándo se ejecutará tu tarea en segundo plano.                                                                                                                  |
| [Transferir datos en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | Usa la API de transferencia en segundo plano para copiar archivos en segundo plano.                                                                                                                              |
| [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)                       | Usa una tarea en segundo plano para actualizar el icono dinámico de tu aplicación con contenido actualizado.                                                                                                                      |
| [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)                                                       | Aprende a usar la clase [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para ejecutar código ligero en segundo plano mientras el dispositivo está enchufado.                             |

 

## Agregar una pantalla de presentación


Todas las aplicaciones para UWP deben tener una pantalla de presentación, que es una combinación de imagen de pantalla de presentación y un color de fondo, ambos personalizables.

La pantalla de presentación se muestra de inmediato cuando el usuario inicia la aplicación. De este modo, los usuarios reciben comentarios inmediatamente mientras se inicializan los recursos de la aplicación. En cuanto la aplicación está lista para la interacción, se descarta la pantalla de presentación.

Una pantalla de presentación bien diseñada puede hacer que tu aplicación sea más atractiva. A continuación mostramos una pantalla de presentación sencilla:

![Una captura de pantalla a una escala del 75 % de la pantalla de presentación desde la muestra de pantalla de presentación.](images/regularsplashscreen.png)

Esta pantalla de presentación se crea combinando un color de fondo verde con un PNG transparente.

Combinar una imagen y un color de fondo para formar la pantalla de presentación ayuda a que esta tenga una buena apariencia sin importar el dispositivo en el que esté instalada tu aplicación. Cuando se muestra la pantalla de presentación, solamente cambia el tamaño del fondo para compensar los distintos tamaños de pantalla. Tu imagen siempre permanece intacta.

Además, puedes usar la clase [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) para personalizar la experiencia de inicio de la aplicación. Puedes colocar una pantalla de presentación extendida, creada por ti, para permitir que la aplicación tenga más tiempo para completar tareas adicionales, como preparar la interfaz de usuario de la aplicación o finalizar las operaciones de red. También puedes usar la clase **SplashScreen** para que te notifique cuando se descarte la pantalla de presentación de modo que puedas iniciar las animaciones de entrada.

| Tema                                                                          | Descripción                                                                                                                                                                                       |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Agregar una pantalla de presentación](add-a-splash-screen.md)                                 | Establecer el color de imagen y de fondo de la pantalla de presentación de la aplicación.                                                                                                                                          |
| [Mostrar una pantalla de presentación durante más tiempo](create-a-customized-splash-screen.md) | Muestra una pantalla de presentación más tiempo creando una pantalla de presentación extendida para tu aplicación. Esta pantalla extendida imita la pantalla de presentación cuando la aplicación se inicia, y se puede personalizar. |

 

 

 





<!--HONumber=May16_HO2-->


