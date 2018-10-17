---
author: jwmsft
title: Aplicaciones adaptables para versiones
description: Aprende a sacar partido de las nuevas API mientras mantienes la compatibilidad con versiones anteriores
ms.author: jimwalk
ms.date: 09/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2485eab4b192fe4a65c68d957de1ec9192f8c20
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "4741515"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>Aplicaciones adaptables para versiones: Usa las nuevas API mientras mantienes la compatibilidad con versiones anteriores

Cada versión del SDK de Windows10 agrega nuevas y emocionantes funcionalidades, y querrás aprovecharlas. Sin embargo, no todos los clientes actualizarán sus dispositivos a la última versión de Windows10 al mismo tiempo y quieres asegurarte de que tu aplicación funciona en la gama más amplia posible de dispositivos. Aquí te mostramos cómo diseñar la aplicación para que se ejecute en versiones anteriores de Windows10, pero que también aproveche las ventajas de las nuevas funciones cuando la aplicación se ejecute en un dispositivo que tenga instalada la actualización más reciente.

Es necesario llevar a cabo 3 pasos para asegurarse de que la aplicación admite la gama más amplia posible de dispositivos Windows10.

- En primer lugar, configura el proyecto de Visual Studio para orientarlo a las API más recientes. Esto afecta a lo que sucede cuando se compila la aplicación.
- En segundo lugar, realiza comprobaciones en tiempo de ejecución para asegurarte de llamar únicamente a las API que estén presentes en el dispositivo en el que se ejecute la aplicación.
- Por último, prueba la aplicación en la versión mínima y la versión de destino de Windows 10.

## <a name="configure-your-visual-studio-project"></a>Configura el proyecto de Visual Studio.

El primer paso para admitir varias versiones de Windows 10 es especificar las versiones *de destino* y *mínimas* del sistema operativo/SDK que se admiten en el proyecto de Visual Studio.

- *Destino*: es la versión del SDK en la que Visual Studio compila el código de la aplicación y ejecuta todas las herramientas. Todas las API y todos los recursos de esta versión del SDK están disponibles en el código de la aplicación en el momento de la compilación.
- *Mínima*: es la versión del SDK que admite la versión más antigua del sistema operativo en la que se puede ejecutar la aplicación (y que la Tienda implementará) y la versión para la que Visual Studio compila el código de marcado de la aplicación. 

Durante el tiempo de ejecución, la aplicación se ejecutará para la versión del sistema operativo para la que se implemente, por lo que la aplicación generará excepciones si usas recursos o llamas a API que no estén disponibles en esa versión. Más adelante en este artículo te mostramos cómo usar las comprobaciones en tiempo de ejecución para llamar a las API correctas.

Los valores Destino y Mínima especifican los extremos de un intervalo de versiones del sistema operativo/SDK. Sin embargo, si la aplicación se prueba en la versión mínima, tendrás la seguridad de que se ejecutará en todas las versiones entre la mínima y la de destino.

> [!TIP]
> Visual Studio no avisa sobre la compatibilidad de las API. Es tu responsabilidad probar y garantizar que la aplicación funciona según lo esperado en todas las versiones del sistema operativo entre la mínima y la de destino, ambas incluidas.

Cuando crees un nuevo proyecto en Visual Studio 2015, actualización 2 o posterior, se te pedirá que establezcas las versiones de destino y mínima que admite tu aplicación. De forma predeterminada, la versión de destino es la versión superior instalada del SDK, mientras que la versión mínima es la versión más baja instalada del SDK. Puedes elegir las versiones de destino y mínima solo entre las versiones del SDK que estén instaladas en el equipo. 

![Establecer el SDK de destino en Visual Studio](images/vs-target-sdk-1.png)

Por lo general, recomendamos dejar los valores predeterminados. Sin embargo, si tienes instalada una versión preliminar del SDK y estás escribiendo código de producción, deberías cambiar la versión de destino de la versión del Preview SDK a la última versión oficial del SDK. 

Para cambiar la versión mínima y de destino de un proyecto que ya se haya creado en Visual Studio, ve a Proyecto -> Propiedades -> ficha Aplicación -> Destino.

![Cambiar el SDK de destino en Visual Studio](images/vs-target-sdk-2.png)

Como referencia, la siguiente tabla muestra los números de compilación de cada SDK.

| Nombre descriptivo | Versión | Sistema operativo/compilación de SDK |
| ---- | ---- | ---- |
| RTM | 1507 | 10240 |
| Actualización de noviembre | 1511 | 10586 |
| Actualización de aniversario | 1607 | 14393 |
| Creators Update | 1703 | 15063 |
| Fall Creators Update | 1709 | 16299 |
| Actualización de abril de 2018 | 1803 | 17134 |
| Actualización de octubre de 2018 | 1809 | _Insider Preview_ |

Puedes descargar cualquier versión publicada del SDK desde [Windows SDK y el archivo del emulador](https://developer.microsoft.com/downloads/sdk-archive). Puedes descargar el Windows Insider Preview SDK más reciente desde la sección para desarrolladores del sitio [Windows Insider](https://insider.windows.com/Home/BuildWithWindows).

 Para obtener más información acerca de las actualizaciones de Windows 10, consulta la [información de versión de Windows 10](https://technet.microsoft.com/windows/release-info). Para obtener información importante acerca de Windows 10 admiten el ciclo de vida, consulte la [hoja de datos del ciclo de vida de Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

## <a name="perform-api-checks"></a>Realizar comprobaciones de API

La clave para las aplicaciones adaptables para versiones es la combinación de contratos de API y la clase [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation). Esta clase permite detectar si un contrato de API especificado, tipo o miembro está presente para que puedas realizar de forma segura llamadas de API en una gran variedad de dispositivos y de versiones del sistema operativo.

### <a name="api-contracts"></a>Contratos de API

El conjunto de las API de una familia de dispositivos se desglosa en subdivisiones conocidas como contratos de API. Puedes usar el método **ApiInformation.IsApiContractPresent** para probar la presencia de un contrato de API. Esto es útil si quieres probar la presencia de muchas API que estén en la misma versión de un contrato de API.

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

¿Qué es un contrato de API? Básicamente, un contrato de API representa una característica: un conjunto de API relacionadas que suministran juntas algunas funciones particulares. Un contrato de API hipotético puede representar un conjunto de API que contenga dos clases, cinco interfaces, una estructura, dos enumeraciones y así sucesivamente.

Los tipos relacionados lógicamente se agrupan en un contrato de API y, a partir de Windows 10, cada API de Windows Runtime es un miembro de algunos contrato de API. Con contratos de API, se comprueba la disponibilidad de una API o una característica específica en el dispositivo, comprobando de manera eficaz las funcionalidades de un dispositivo, en vez de comprobar un dispositivo específico o un sistema operativo. Se necesita una plataforma que implementa las API en un contrato de API para implementar cada API de ese contrato de API. Esto significa que puedes probar si el sistema operativo es compatible con un contrato de API particular y, si lo es, llamar a cualquiera de las API de ese contrato de API sin comprobar cada uno de ellos individualmente.

El contrato de API más grande y más comúnmente usado es el **Windows.Foundation.UniversalApiContract**. Contiene la mayoría de las API en la Plataforma universal de Windows. La documentación de los [contratos de API y el SDK de extensión de la familia de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/) describe la variedad de contratos de API disponibles. Verás que la mayoría de ellos representa un conjunto de API relacionados funcionalmente.

> [!NOTE]
> Si tienes una vista previa del Kit de desarrollo de software de Windows (SDK) que no está documentada todavía, también podrás encontrar información sobre el soporte de contrato de API en el archivo de 'Platform.xml' ubicado en la carpeta de instalación de SDK en ‘\(Program Files (x86))\Windows Kits\10\Platforms\<platform>\<SDK version>\Platform.xml’.

### <a name="version-adaptive-code-and-conditional-xaml"></a>Código adaptativo para versiones y XAML condicional

En todas las versiones de Windows 10, puedes usar la clase ApiInformation en una condición del código para probar la presencia de la API que se desee llamar. En el código adaptable, puedes usar distintos métodos de la clase, como IsTypePresent, IsEventPresent, IsMethodPresent y IsPropertyPresent, para probar las API en el nivel de detalle que necesitas.

Para obtener más información y ejemplos, consulta **[Código adaptativo para versiones](version-adaptive-code.md)**.

Si la versión mínima de tu aplicación es la compilación 15063 (Creators Update) o posterior, puedes usar *XAML condicional* para establecer propiedades y crear instancias de objetos en el marcado sin tener que usar código subyacente. El XAML condicional proporciona una forma de usar el método ApiInformation.IsApiContractPresent en el marcado.

Para obtener más información y ejemplos, consulta **[Conditional XAML](conditional-xaml.md)**.

## <a name="test-your-version-adaptive-app"></a>Probar la versión de la aplicación adaptable

Cuando usas código adaptable para versiones o XAML condicional para escribir una versión de la aplicación adaptable, deberás probarla en un dispositivo que ejecuta la versión mínima y en un dispositivo que ejecute la versión de destino de Windows 10.

No se pueden probar todas las rutas de código condicionales en un solo dispositivo. Para garantizar que todas las rutas de código se prueban, debes implementar y probar la aplicación en un dispositivo remoto (o máquina virtual) ejecutando la versión mínima admitida del sistema operativo.
Para obtener más información sobre la depuración remota, consulta [Implementación y depuración de aplicaciones para UWP](deploying-and-debugging-uwp-apps.md).

## <a name="related-articles"></a>Artículos relacionados

- [Qué es una aplicación para UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contratos de API](https://channel9.msdn.com/Events/Build/2015/3-733) (Vídeo de compilación de 2015)