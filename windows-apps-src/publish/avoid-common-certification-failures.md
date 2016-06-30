---
author: jnHs
Description: "Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado."
title: "Evitar errores de certificación comunes"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6af2842eeb5eeebfc9ffc5a7a3ec98fdcebddb74

---

# Evitar errores de certificación comunes


Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado.

> **Note**  También debes revisar las [Directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944) para asegurarte de que la aplicación cumple los requisitos establecidos en ellas.

 

-   Envía la aplicación solamente cuando esté terminada. Puedes usar la descripción de la aplicación para mencionar las próximas funciones, pero asegúrate de que no contenga secciones incompletas, vínculos a páginas web en construcción o cualquier elemento que pueda dar al cliente la impresión de que la aplicación está incompleta.

-   [Prueba tu aplicación con el Kit para la certificación de aplicaciones de Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) antes de que envíes tu aplicación.

-   Prueba tu aplicación en diversas configuraciones para asegurarte de que sea lo más estable posible.

-   Asegúrate de que tu aplicación no se bloquee, si no existe conectividad de red. Aun si se requiere una conexión para usar realmente la aplicación, es necesario que tenga un rendimiento adecuado cuando no haya conexión.
-   Asegúrate de que la descripción de la aplicación refleja fielmente lo que hace la aplicación. Para obtener ayuda, consulta nuestras directrices sobre cómo [escribir una buena descripción de la aplicación](write-a-great-app-description.md).

-   Asegúrate de proporcionar respuestas completas y precisas a todas las preguntas de la sección [Clasificaciones por edades](age-ratings.md).

-   Si tu aplicación usa las API de comercio de la Tienda Windows desde el espacio de nombres [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197), asegúrate de probar la aplicación y comprobar que administre excepciones típicas. Asimismo, asegúrate de que la aplicación use la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (no la clase [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), que está destinada a fines de prueba solamente).

-   No [declares tu aplicación como accesible](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que hayas realizado en ella ingeniería específica y la hayas probado en escenarios de accesibilidad.

-   Asegúrate de [proporcionar toda la información necesaria](notes-for-certification.md) que se requiere para usar tu aplicación, como el nombre de usuario y contraseña para una cuenta de prueba en el caso de que tu aplicación necesite que los usuarios se registren en un servicio, o todos los pasos necesarios para acceder a funciones ocultas o bloqueadas.

 

 







<!--HONumber=Jun16_HO4-->


