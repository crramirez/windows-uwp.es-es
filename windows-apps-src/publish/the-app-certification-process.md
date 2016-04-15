---
Description: Cuando termines de crear el envío de la aplicación y hagas clic en Enviar a la Tienda, entra en el paso de certificación.
title: Proceso de certificación de la aplicación
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
---

# Proceso de certificación de la aplicación


Cuando termines de crear el envío de la aplicación y hagas clic en **Enviar a la Tienda**, entra en el paso de certificación. Normalmente, este proceso se completa en unas horas, aunque en algunos casos puede tardar más. A continuación se muestra un vistazo de lo que ocurre durante el paso de certificación.

## Preprocesamiento

Después de cargar los paquetes de la aplicación correctamente y enviar la aplicación para su certificación, los paquetes se ponen en cola para someterse a pruebas. Se mostrará un mensaje si se detectan los errores durante el preprocesamiento. Para obtener más información sobre posibles errores, consulta [Resolver errores de envío](resolve-submission-errors.md).

## Certificación

Durante esta fase, se llevan a cabo varias pruebas:

-   **Pruebas de seguridad:** La primera prueba comprueba si hay virus y malware en los paquetes de la aplicación. Si la aplicación no pasa esta prueba, necesitarás comprobar el sistema de desarrollo ejecutando el software antivirus más reciente y luego recompilar el paquete de la aplicación en un sistema limpio.
-   **Pruebas de cumplimiento técnico:** El cumplimiento de los requisitos técnicos se prueba con el Kit para la certificación de aplicaciones en Windows. (Debes asegurarte siempre de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviarla a la Tienda).
-   **Cumplimiento de requisitos de contenido:** el tiempo que lleva esto depende de la complejidad de la aplicación, la cantidad de contenido visual que tenga y la cantidad de aplicaciones que se hayan enviado recientemente. Asegúrate de proporcionar toda la información que los evaluadores deben conocer en la página [Notas para la certificación](notes-for-certification.md).

Una vez completado el proceso de certificación, obtendrás un informe donde se indicará si la aplicación pasó o no la certificación. Si no la pasó, el informe indicará en qué prueba produjo errores o qué [directiva](https://msdn.microsoft.com/library/windows/apps/dn764944) no se cumplió. Después de solucionar el problema, puedes crear un nuevo envío de la aplicación para volver a iniciar el proceso de certificación.

## Versión

Cuando la aplicación aprueba la certificación, está lista para pasar al proceso de **Publicación**. Si has indicado que el envío se debe publicar lo antes posible, ocurrirá inmediatamente. Si has indicado que no se debe lanzar hasta una fecha determinada, esperaremos hasta esa fecha, a menos que hagas clic en el vínculo a **Cambiar fecha de lanzamiento**. Si has indicado que deseas publicar el envío manualmente, no la publicaremos hasta que indiques que debemos hacer clic en el botón **Publicar ahora** o si haces clic en el vínculo **Cambiar fecha de lanzamiento** y seleccionas una fecha específica.

## Publicación

Los paquetes de la aplicación se firman digitalmente para protegerlos de alteraciones después de su lanzamiento. Una vez iniciada esta fase, ya no puedes cancelar el envío ni cambiar la fecha de lanzamiento.

Mientras la aplicación se encuentra en la fase de publicación, el vínculo **Mostrar detalles** de la columna Estado para el envío de la aplicación te avisará cuando se pongan a disposición de los clientes en cada una de tus versiones de sistema operativo compatibles los nuevos paquetes y los detalles del listado de la Tienda. La aplicación permanece en la fase de publicación hasta que los nuevos paquetes y los detalles del listado estén disponibles para todos los clientes potenciales de la aplicación. Cuando esto ocurra, el estado cambiará de **Publicación** a **En la Tienda**. 

## En la Tienda 

Tras completar los pasos anteriores, tu envío estará disponible en la Tienda Windows para que lo descarguen los clientes (a menos que haya elegido otra opción [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility)).

**Nota**  También realizamos comprobaciones puntuales de aplicaciones después de que se hayan publicado para poder identificar posibles problemas y asegurarnos de que la aplicación cumple con todas las [directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944) Si encontramos algún problema, se te notificará al respecto y sobre cómo solucionarlo, si corresponde, o bien sobre si la aplicación se ha quitado de la Tienda.

 

 

 






<!--HONumber=Mar16_HO5-->


