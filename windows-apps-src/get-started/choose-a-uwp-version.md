---
author: QuinnRadich
title: "Elegir una versión de UWP"
description: "Al escribir una aplicación para UWP en Microsoft Visual Studio, puedes elegir la versión de destino. Obtén información sobre la diferencia entre las diferentes versiones de UWP y sobre cómo configurar las opciones en proyectos nuevos y existentes."
redirect_url: ../updates-and-versions/choose-a-uwp-version/
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="choose-a-uwp-version"></a>Elegir una versión de UWP

**La ubicación de esta página se ha cambiado a ../updates-and-versions/choose-a-uwp-version/**

Al escribir una aplicación para UWP en Microsoft Visual Studio, puedes elegir la versión de destino. Obtén información sobre la diferencia entre las diferentes versiones de UWP y sobre cómo configurar las opciones en proyectos nuevos y existentes.

## <a name="whats-different-in-each-uwp-version"></a>¿Qué es diferente en cada versión de UWP?

En cada versión sucesiva de Windows10 están disponibles API nuevas y modificadas para UWP. Para obtener información específica sobre qué funciones se han agregado en qué versión, consulta [Novedades para desarrolladores de Windows10](../whats-new/windows-10-version-1607.md).

Para ver los temas de consulta que enumeran todas las familias de dispositivos y sus versiones y todos los contratos de API y sus versiones, consulta [Familias de dispositivos](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) y [Contratos de API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="choose-which-version-to-use-for-your-app"></a>Elegir la versión que usarás para la aplicación

En el diálogo **Nuevo proyecto de Windows universal** de Visual Studio, puedes elegir una versión para la **Versión de destino** y otra para la **Versión mínima**.

* **Versión de destino**. Esto establece el ajuste *TargetPlatformVersion* en el archivo del proyecto. También determina el valor del atributo *TargetDeviceFamily@MaxVersionTested* en el manifiesto del paquete de la aplicación. El valor que elijas especificará la versión de la plataforma UWP a la que está destinada tu proyecto (y, por lo tanto, el conjunto de API disponibles para tu aplicación), por lo que recomendamos que elijas la versión más reciente que sea posible. Para obtener más información sobre el manifiesto del paquete de la aplicación y algunas directrices sobre cómo configurar TargetDeviceFamily manualmente, consulta [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Versión mínima**. Esto establece el ajuste *TargetPlatformMinVersion* en el archivo del proyecto. También determina el valor del atributo *TargetDeviceFamily@MinVersion* en el manifiesto del paquete de la aplicación. El valor que elijas especificará la versión mínima de la plataforma UWP con la que puede funcionar tu proyecto.

Ten en cuenta que vas a declarar que tu aplicación funciona en cualquier versión de Windows en el rango desde la **Versión mínima** a la **Versión de destino**. Si las dos son la misma versión, no necesitas hacer nada especial. Si son diferentes, estas son algunas cosas que debes tener en cuenta.

* En el código, puedes llamar libremente (es decir, sin comprobaciones condicionales) a las API que existen en la versión especificada por la **Versión mínima**.
* El valor de **Versión de destino** se usa para identificar todas las referencias (winmds del contrato) para compilar el proyecto. Pero estas referencias te permitirán compilar el código con llamadas a API que no tienen por qué existir en los dispositivos que hayas declarado que admites (a través de **Versión mínima**). Por lo tanto, cualquier API que se haya introducido después la **Versión mínima** deberá llamarse a través de código adaptativo. Para obtener más información acerca del código adaptativo, consulta la [Guía de aplicaciones de la Plataforma universal de Windows (UWP)](universal-application-platform-guide.md).