---
Description: Si se producen errores después de enviar la aplicación a la Tienda, tienes que resolverlos para poder continuar el proceso de certificación.
title: Resolver errores de envío
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
---

# Resolver errores de envío


Si se producen errores después de enviar la aplicación a la Tienda, tienes que resolverlos para poder continuar el [proceso de certificación](the-app-certification-process.md). El mensaje de error indicará cuál es el problema y lo que probablemente tendrás que hacer para corregirlo. Aquí encontrarás información adicional que puede ayudarte a resolver estos errores.

## Aplicaciones para UWP


Si envías una aplicación para UWP, es posible que veas un error durante el procesamiento previo si el archivo de paquete no es un archivo .appxupload generado por Visual Studio para la Tienda. Asegúrate de seguir los pasos de [Empaquetado de aplicaciones universales de Windows para Windows 10](../packaging/packaging-uwp-apps.md) al crear el archivo de paquete de la aplicación y carga solo el archivo .appxupload en la página [Paquetes](upload-app-packages.md) del envío, no un appx o .appxbundle.

Si se muestra un error de compilación, asegúrate de que eres capaz de generar correctamente la aplicación en modo de lanzamiento. Para obtener más información, consulta [Errores de compilador interno nativo .NET](http://go.microsoft.com/fwlink/p/?LinkID=613098).

## Aplicaciones de Windows Phone


Es posible que veas **error 2001** cuando se detectan problemas con los paquetes de Windows Phone durante el procesamiento previo. En la mayoría de los casos, necesitarás volver a compilar el paquete de la aplicación para corregir el error. Una vez que lo hayas hecho, reemplaza el paquete antiguo por el nuevo en la página [Paquetes](upload-app-packages.md) del envío antes de hacer clic de nuevo en **Enviar a la Tienda**.

Hay una serie de problemas que pueden causar este error. Revisa la lista siguiente para determinar lo que es posible que se aplique a los paquetes.

-   **Uno o más ensamblados del paquete se han ofuscado incorrectamente:** usa una herramienta diferente para realizar la ofuscación o quitarla. El proceso de compilación optimiza los ensamblados ofuscados pero, en ocasiones, algunos ensamblados se ofuscan con una herramienta que modifica el MSIL de una forma no compatible que provoca un error.
-   **El tamaño de uno o más métodos de la aplicación supera los 256 KB de IL.:** refactoriza el método incorrecto en funciones más pequeñas. El tamaño de MSIL para métodos de un ensamblado se puede determinar mediante la herramienta ILDASM.
-   **Error en la validación de la firma de nombre seguro para uno o más ensamblados:** este error suele ocurrir cuando la firma de nombre seguro se realizó con una clave diferente a la esperada en los metadatos del ensamblado. Firma con la clave correcta o quita la firma de nombre seguro.
-   **El paquete contiene ensamblados en modo mixto (con código administrado y nativo):** los ensamblados en modo mixto no son compatibles con Windows Phone. Quita los ensamblados de modo mixto del paquete y vuelve a enviar la aplicación.
-   **Un ensamblado de Windows Phone 8.1 XAP o appx/appxbundle no es válido:** asegúrate de que el archivo .winmd tiene al menos un punto de entrada público. Si es necesario, puedes usar cualquier aplicación descompilador para revisar el código y comprobar si hay puntos de entrada públicos.

Otro error que puede aparecer después de enviar la aplicación es el **error 1300**. Esto ocurre cuando ya se han precompilado uno o varios ensamblados (o todo el paquete). Para solucionar este problema, recompila el paquete de la aplicación en Microsoft Visual Studio y, a continuación, envía el paquete recién generado.

 

 






<!--HONumber=Mar16_HO1-->


