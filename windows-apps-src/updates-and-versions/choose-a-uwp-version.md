---
author: QuinnRadich
title: "Elegir una versión de UWP"
description: "Al escribir una aplicación para UWP en Microsoft Visual Studio, puedes elegir la versión de destino. Obtén información sobre la diferencia entre las diferentes versiones de UWP y sobre cómo configurar las opciones en proyectos nuevos y existentes."
translationtype: Human Translation
ms.sourcegitcommit: 04e96e38887116fe71504e7b6cb9a275e4dd30f0
ms.openlocfilehash: fb03ae3b4f234dc61ec6918df06c15bc3c00df09

---

# Elegir una versión de UWP

Al escribir una aplicación para UWP en Microsoft Visual Studio, puedes elegir la versión de destino. Actualmente, solo hay tres versiones posibles.

| Versión | Descripción |
| --- | --- |
| Compilación 14393 (edición de Aniversario) | Esta es la versión más reciente de Windows10, publicada en julio de 2016. Algunas características destacadas de esta versión incluyen: </br> \* **Windows Ink:** Nuevos controles InkCanvas e InkToolbar. </br> \* **API de Cortana:** Usa nuevas acciones de Cortana para integrar la compatibilidad de Cortana con funciones específicas de tu aplicación. </br> \* **Windows Hello:** Microsoft Edge ahora admite Windows Hello, lo que proporciona a los desarrolladores web acceso a la autenticación biométrica. </br> Para obtener información sobre estas y muchas otras funciones agregadas en esta versión de Windows, visita [el Centro de desarrollo](https://developer.microsoft.com/en-us/windows/windows-10-for-developers) o la página de información detallada en [Novedades para desarrolladores en Windows 10](../whats-new/windows-10-version-1607.md).  |
| Compilación 10586 | Esta versión de Windows10 se publicó en noviembre de 2015. Las características destacadas incluyen la introducción de las API ORTC (Comunicaciones en tiempo real mediante objetos) para la comunicación de vídeo en Microsoft Edge y las API de proveedores para permitir a las aplicaciones usar la autenticación de rostro de Windows Hello. [Más información sobre las características introducidas en esta compilación.](../whats-new/windows-10-version-1511.md) |
| Compilación 10240 | Esta es la versión inicial de Windows 10, publicada en julio de 2015. [Más información sobre las características introducidas en esta compilación.](../whats-new/windows-10-version-1507.md) |

Es muy recomendable que los nuevos desarrolladores y los desarrolladores que escriban código para un público general usen siempre la compilación más reciente de Windows (14393). Los desarrolladores que escriban aplicaciones de empresa deberían pensar seriamente en ofrecer compatibilidad para una **versión mínima** más antigua.

## ¿Qué es diferente en cada versión de UWP?

En cada versión sucesiva de Windows10 están disponibles API nuevas y modificadas para UWP. Para obtener información específica sobre qué funciones se han agregado en qué versión, consulta [Novedades para desarrolladores de Windows10](../whats-new/windows-10-version-1607.md).

Para ver los temas de consulta que enumeran todas las familias de dispositivos y sus versiones y todos los contratos de API y sus versiones, consulta [Familias de dispositivos](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) y [Contratos de API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## Elegir la versión que usarás para la aplicación

En el diálogo **Nuevo proyecto de Windows universal** de Visual Studio, puedes elegir una versión para la **Versión de destino** y otra para la **Versión mínima**.

* **Versión de destino**. Esto establece el ajuste *TargetPlatformVersion* en el archivo del proyecto. También determina el valor del atributo *TargetDeviceFamily@MaxVersionTested* en el manifiesto del paquete de la aplicación. El valor que elijas especificará la versión de la plataforma UWP a la que está destinada tu proyecto (y, por lo tanto, el conjunto de API disponibles para tu aplicación), por lo que recomendamos que elijas la versión más reciente que sea posible. Para obtener más información sobre el manifiesto del paquete de la aplicación y algunas directrices sobre cómo configurar TargetDeviceFamily manualmente, consulta [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Versión mínima**. Esto establece el ajuste *TargetPlatformMinVersion* en el archivo del proyecto. También determina el valor del atributo *TargetDeviceFamily@MinVersion* en el manifiesto del paquete de la aplicación. El valor que elijas especificará la versión mínima de la plataforma UWP con la que puede funcionar tu proyecto.

Ten en cuenta que vas a declarar que tu aplicación funciona en cualquier versión de Windows en el rango desde la **Versión mínima** a la **Versión de destino**. Si las dos son la misma versión, no necesitas hacer nada especial. Si son diferentes, estas son algunas cosas que debes tener en cuenta.

* En el código, puedes llamar libremente (es decir, sin comprobaciones condicionales) a las API que existen en la versión especificada por la **Versión mínima**.
* Asegúrate de probar tu código en la **Versión mínima**, para asegurarte de que funciona sin necesidad de API que solo están presentes en la **Versión de destino**.
* El valor de **Versión de destino** se usa para identificar todas las referencias (winmds del contrato) para compilar el proyecto. Pero estas referencias te permitirán compilar el código con llamadas a API que no tienen por qué existir en los dispositivos que hayas declarado que admites (a través de **Versión mínima**). Por lo tanto, cualquier API que se haya introducido después la **Versión mínima** deberá llamarse a través de código adaptativo. Para obtener más información acerca del código adaptativo, consulta la [Guía de aplicaciones de la Plataforma universal de Windows (UWP)](../get-started/universal-application-platform-guide.md).



<!--HONumber=Nov16_HO1-->


