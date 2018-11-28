---
Description: If you encounter errors after submitting your app to the Store, you must resolve them in order to continue the certification process.
title: Resolver errores de envío
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9911323010f691d1fa59c35306a7173cd08a0faa
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7832724"
---
# <a name="resolve-submission-errors"></a>Resolver errores de envío

Si se producen errores después de enviar la aplicación a la Tienda, tienes que resolverlos para poder continuar el [proceso de certificación](the-app-certification-process.md). El mensaje de error indicará cuál es el problema y lo que probablemente tendrás que hacer para corregirlo. Aquí encontrarás información adicional que puede ayudarte a resolver estos errores.

## <a name="uwp-apps"></a>Aplicaciones para UWP

Si envías una aplicación para UWP, es posible que veas un error durante el procesamiento previo si el archivo de paquete no es un archivo .msixupload o .appxupload generado por Visual Studio para la tienda. Asegúrate de que puedes seguir los pasos de [paquete de una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md) al crear el archivo de paquete de la aplicación y solo cargan el archivo .msixupload o .appxupload en la página [paquetes](upload-app-packages.md) del envío, no un appx/.msix o .msixbundle/appxbundle .

Si se muestra un error de compilación, asegúrate de que eres capaz de generar correctamente la aplicación en modo de lanzamiento. Para obtener más información, consulta [Errores de compilador interno nativo .NET](http://go.microsoft.com/fwlink/p/?LinkID=613098).

## <a name="desktop-application"></a>Aplicación de escritorio

Si vas a enviar un paquete que contiene los archivos binarios de Win32 y el UWP, asegúrate de que crees que el paquete utilizando el proyecto de empaquetado de Windows que está disponible en Visual Studio 2017 Update 4. Si creas el paquete mediante el uso de una plantilla de proyecto UWP, es posible que no podrás enviar que empaquetarla a la tienda o la instalación de prueba en otros equipos. Incluso si el paquete se publica correctamente, es posible que se comportan de forma inesperada en el equipo del usuario. Para obtener más información, consulta el [paquete de una aplicación con Visual Studio (puente de escritorio)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8.x y versiones anteriores

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, recién creado productos no pueden incluir paquetes destinados a Windows Phone 8.x o versiones anteriores. Para obtener más información, consulta este [blog publicar](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Es posible que veas **error 2001** cuando se detectan problemas con los paquetes de Windows Phone durante el procesamiento previo. En la mayoría de los casos, necesitarás volver a compilar el paquete de la aplicación para corregir el error. Una vez que lo hayas hecho, reemplaza el paquete antiguo por el nuevo en la página [Paquetes](upload-app-packages.md) del envío antes de hacer clic de nuevo en **Enviar a la Tienda**.

Hay una serie de problemas que pueden causar este error. Revisa la lista siguiente para determinar lo que es posible que se aplique a los paquetes.

-   **Uno o varios ensamblados del paquete se han ofuscado incorrectamente:** usa una herramienta diferente para realizar la ofuscación o quita la ofuscación. El proceso de compilación optimiza los ensamblados ofuscados pero, en ocasiones, algunos ensamblados se ofuscan con una herramienta que modifica el MSIL de una forma no compatible que provoca un error.
-   **El tamaño de uno o varios métodos de la aplicación supera los 256KB de nivel de integridad:** refactoriza el método incorrecto en funciones más pequeñas. El tamaño de MSIL para métodos de un ensamblado se puede determinar mediante la herramienta ILDASM.
-   **Error en la validación de la firma de nombre seguro para uno o varios ensamblados:** este error suele ocurrir cuando la firma de nombre seguro se realizó con una clave diferente a la esperada en los metadatos del ensamblado. Firma con la clave correcta o quita la firma de nombre seguro.
-   **El paquete contiene ensamblados en modo mixto (con código administrado y nativo):** los ensamblados en modo mixto no son compatibles con WindowsPhone. Quita los ensamblados de modo mixto del paquete y vuelve a enviar la aplicación.
-   **Un ensamblado de Windows Phone 8.1 XAP o appx/appxbundle no es válido:** asegúrate de que el archivo .winmd tenga al menos un punto de entrada público. Si es necesario, puedes usar cualquier aplicación descompilador para revisar el código y comprobar si hay puntos de entrada públicos.

Otro error que puede aparecer después de enviar la aplicación es el **error 1300**. Esto ocurre cuando ya se han precompilado uno o varios ensamblados (o todo el paquete). Para solucionar este problema, recompila el paquete de la aplicación en Microsoft Visual Studio y, a continuación, envía el paquete recién generado.

## <a name="nameidentity-errors"></a>Errores de nombre/identidad

Si aparece un error que dice **El nombre encontrado en el paquete no es uno de los nombres de aplicación reservados. Reserva el nombre de la aplicación o actualiza el paquete con el nombre de la aplicación correcto para este idioma** puede deberse a que escribiste un nombre incorrecto en el paquete. Este error también puede producirse si estás usando un nombre de aplicación que no has reservado en el centro de partners. Normalmente se puede resolver el error siguiendo estos pasos:

- Ve a la página de la aplicación [Identidad de la aplicación](view-app-identity-details.md) (en **Administración de aplicaciones**) para confirmar si la aplicación tiene una identidad asignada. Si no la tiene, verás una opción para crear una. Debes reservar un nombre para tu aplicación con el fin de crear la identidad. Asegúrate de que este sea el nombre que usaste en el paquete.
- Si la aplicación ya tiene una identidad, puede significar que aún tienes que reservar el nombre que quieras usar en el paquete. En **Administración de aplicaciones**, haz clic en [Administrar nombres de la aplicación](manage-app-names.md). Escribe el nombre que te gustaría usar y haz clic en **Reservar nombre de aplicación**.

> [!IMPORTANT]
>  Si el nombre que quieres usar no está disponible, otra aplicación es posible que ya haya reservado ese nombre. Si la aplicación ya está publicada con ese nombre, o si crees que tienes derecho a utilizar, [Ponte en contacto con soporte técnico](https://go.microsoft.com/fwlink/p/?LinkId=331509).  

 

 




