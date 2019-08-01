---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Kit para la certificación de aplicaciones en Windows
description: Para que la aplicación tenga posibilidades de publicarse en Microsoft Store u obtener la certificación de Windows, debes validarla y probarla localmente antes de enviarla para su certificación. En este tema explicamos cómo instalar y ejecutar el Kit para la certificación de aplicaciones en Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, certificación de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: d6be00fdbfc7ef5950f6cc35ba35d4afd0dc2791
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682454"
---
# <a name="windows-app-certification-kit"></a>Kit para la certificación de aplicaciones en Windows



Para certificar la aplicación de [Windows](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) o prepararla para su [publicación en el Microsoft Store](https://docs.microsoft.com/windows/uwp/publish/app-submissions), debe validarla y probarla en primer lugar localmente. En este tema se muestra cómo instalar y ejecutar el [Kit de certificación de aplicaciones de Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) para asegurarse de que la aplicación sea segura y eficaz.

## <a name="prerequisites"></a>Requisitos previos

Requisitos previos para probar una aplicación universal de Windows:

-   Debe instalar y ejecutar Windows 10.
-   Debe instalar la [versión 10 del kit]( https://go.microsoft.com/fwlink/p/?LinkID=309666)para la certificación de aplicaciones de Windows, que se incluye en el kit de desarrollo de software (SDK) de Windows para Windows 10.
-   Debes [habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
-   Debes implementar la aplicación de Windows que deseas probar en tu equipo.

**Una nota acerca de las actualizaciones en contexto**

La instalación de un [Kit para la certificación de aplicaciones en Windows]( https://go.microsoft.com/fwlink/p/?LinkID=309666) más reciente reemplazará las versiones anteriores que estén instaladas en el equipo.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Validar la aplicación de Windows usando interactivamente el Kit para la certificación de aplicaciones en Windows

1.  En el menú **Inicio**, ve a **Aplicaciones**, **Kits de Windows** y haz clic en **Kit para la certificación de aplicaciones en Windows**.

2.  En el Kit para la certificación de aplicaciones de Windows, selecciona la categoría de validación que quieras ejecutar. Por ejemplo: Si va a validar una aplicación de Windows, seleccione **validar una aplicación de Windows**.

    Puedes ir directamente a la aplicación que vas a probar o elegir la aplicación de una lista de la interfaz de usuario. Al ejecutar por primera vez el Kit para la certificación de aplicaciones de Windows, la interfaz de usuario enumera todas las aplicaciones de Windows instaladas en el equipo. En todas las ejecuciones posteriores, la interfaz de usuario mostrará las últimas aplicaciones de Windows validadas. Si la aplicación que quieres probar no aparece en la lista, puedes hacer clic en **Mi aplicación no está en la lista** para obtener la lista completa de todas las aplicaciones instaladas en el sistema.

3.  Después de introducir o seleccionar la aplicación que deseas probar, haz clic en **Siguiente**.

4.  En la siguiente pantalla, verás el flujo de trabajo de prueba que se alinea con el tipo de aplicación que se está probando. Si una prueba aparece atenuada en la lista, significa que no es aplicable a tu entorno. Por ejemplo, si estás probando una aplicación de Windows 10 en Windows 7, solo las pruebas estáticas se aplicarán al flujo de trabajo. Tenga en cuenta que el Microsoft Store puede aplicar todas las pruebas de este flujo de trabajo. Selecciona las pruebas que deseas ejecutar y haz clic en **Siguiente**.

    El Kit para la certificación de aplicaciones de Windows inicia la validación de la aplicación.

5.  Cuando se pidan datos después de la prueba, escribe la ruta de acceso de la carpeta en la que quieres guardar el informe de la prueba.

    El Kit para la certificación de aplicaciones en Windows crea un archivo HTML junto a un informe XML y lo guarda en esta carpeta.

6.  Abre el archivo de informe y revisa los resultados de la prueba.

> [!NOTE]
> Si usa Visual Studio, puede ejecutar el kit de certificación de aplicaciones de Windows al crear el paquete de la aplicación. Consulta [Empaquetado de aplicaciones para UWP](/windows/msix/package/packaging-uwp-apps) para obtener información sobre cómo hacerlo.

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Validar la aplicación de Windows usando el Kit para la certificación de aplicaciones en Windows desde una línea de comandos

> [!IMPORTANT]
> El kit para la certificación de aplicaciones de Windows debe ejecutarse en el contexto de una sesión de usuario activa.

1.  En la ventana de comandos, navega hacia el directorio que contiene el Kit para la certificación de aplicaciones en Windows.

    **Tenga en cuenta**    que la ruta de\\acceso predeterminada es C:\\archivos\\de programa\\Windows kits\\de certificación de aplicaciones 10.

2.  Escribe los siguientes comandos en este orden para probar una aplicación que ya está instalada en el equipo de prueba:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    También puedes usar los siguientes comandos si la aplicación no está instalada. El Kit para la certificación de aplicaciones en Windows abrirá el paquete y aplicará el flujo de trabajo de prueba apropiado:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  Una vez que finalice la prueba, abre el archivo del informe `[report file name]` y revisa los resultados de la prueba.

**Tenga en cuenta**  que el kit para la certificación de aplicaciones de Windows se puede ejecutar desde un servicio, pero el servicio debe iniciar el proceso del kit en una sesión de usuario activa y no se puede ejecutar en Session0.

**Nota:**    para obtener más información sobre la línea de comandos del kit para la certificación de aplicaciones de Windows, escriba el comando.`appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Prueba de un equipo de bajo consumo

Los umbrales de la prueba de rendimiento del Kit para la certificación de aplicaciones en Windows se basan en el rendimiento de un equipo de bajo consumo.

Las características del equipo en el que se realiza la prueba pueden afectar a los resultados. Para determinar si el rendimiento de la aplicación cumple las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies), se recomienda que pruebe la aplicación en un equipo de bajo consumo, como un equipo basado en procesador Intel Atom con una resolución de pantalla de 1366x768 (o superior) y un disco duro de rotación. unidad (en lugar de una unidad de disco duro de estado sólido).

A medida que evolucionan los equipos de bajo consumo, las características de rendimiento podrían cambiar con el tiempo. Consulte las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) más recientes y pruebe la aplicación con la versión más reciente del kit de certificación de aplicaciones de Windows para asegurarse de que la aplicación cumple los requisitos de rendimiento más recientes.

## <a name="related-topics"></a>Temas relacionados

* [Pruebas del kit de certificación de aplicaciones de Windows](windows-app-certification-kit-tests.md)
* [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 

 




