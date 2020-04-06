---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Kit para la certificación de aplicaciones en Windows
description: Para que la aplicación tenga posibilidades de publicarse en Microsoft Store u obtener la certificación de Windows, debes validarla y probarla localmente antes de enviarla para su certificación. En este tema explicamos cómo instalar y ejecutar el Kit para la certificación de aplicaciones en Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, certificación de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: 174ff4e588d75293ecb729312883f4792196c87a
ms.sourcegitcommit: e978fadf1aaaa8d90501f67f50e5a61f60880d00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79110105"
---
# <a name="windows-app-certification-kit"></a>Kit para la certificación de aplicaciones en Windows

Para que tu aplicación obtenga la [certificación de Windows](/windows/win32/win_cert/windows-certification-portal) o para prepararla para su [publicación en Microsoft Store](/windows/uwp/publish/app-submissions), primero debes validarla y probarla. En este tema explicamos cómo instalar y ejecutar el [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) para garantizar la seguridad y la eficiencia de la aplicación.

## <a name="prerequisites"></a>Requisitos previos

Requisitos previos para probar una aplicación universal de Windows:

- Debes instalar y ejecutar Windows 10.
- Debes instalar el [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/downloads/app-certification-kit/), que se incluye en el Kit de desarrollo de software de Windows (SDK) para Windows 10.
- Debes [habilitar el dispositivo para el desarrollo](/windows/uwp/get-started/enable-your-device-for-development).
- Debes implementar la aplicación de Windows que deseas probar en tu equipo.

> [!NOTE]
> **Actualizaciones locales:** La instalación de un [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) más reciente reemplazará cualquier versión del kit instalada anteriormente.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Validar la aplicación de Windows usando interactivamente el Kit para la certificación de aplicaciones en Windows

1. En el menú **Inicio**, ve a **Aplicaciones**, **Kits de Windows** y haz clic en **Kit para la certificación de aplicaciones en Windows**.

2. En el Kit para la certificación de aplicaciones de Windows, selecciona la categoría de validación que quieras ejecutar. Por ejemplo: si vas a validar una aplicación de Windows, selecciona **Validar una aplicación de Windows**.

    Puedes ir directamente a la aplicación que vas a probar o elegir la aplicación de una lista de la interfaz de usuario. Al ejecutar por primera vez el Kit para la certificación de aplicaciones de Windows, la interfaz de usuario enumera todas las aplicaciones de Windows instaladas en el equipo. En todas las ejecuciones posteriores, la interfaz de usuario mostrará las últimas aplicaciones de Windows validadas. Si la aplicación que quieres probar no aparece en la lista, puedes hacer clic en **Mi aplicación no está en la lista** para obtener la lista completa de todas las aplicaciones instaladas en el sistema.

3. Después de introducir o seleccionar la aplicación que deseas probar, haz clic en **Siguiente**.

4. En la siguiente pantalla, verás el flujo de trabajo de prueba que se alinea con el tipo de aplicación que se está probando. Si una prueba aparece atenuada en la lista, significa que no es aplicable a tu entorno. Por ejemplo, si estás probando una aplicación de Windows 10 en Windows 7, solo las pruebas estáticas se aplicarán al flujo de trabajo. Ten en cuenta que es posible que Microsoft Store aplique todas las pruebas desde este flujo de trabajo. Selecciona las pruebas que deseas ejecutar y haz clic en **Siguiente**.

    El Kit para la certificación de aplicaciones de Windows inicia la validación de la aplicación.

5. Cuando se pidan datos después de la prueba, escribe la ruta de acceso de la carpeta en la que quieres guardar el informe de la prueba.

    El Kit para la certificación de aplicaciones en Windows crea un archivo HTML junto a un informe XML y lo guarda en esta carpeta.

6. Abre el archivo de informe y revisa los resultados de la prueba.

> [!NOTE]
> Si usas Visual Studio, puedes ejecutar el Kit para la certificación de aplicaciones en Windows al crear el paquete de la aplicación. Consulta [Empaquetado de aplicaciones para UWP](/windows/msix/package/packaging-uwp-apps) para obtener información sobre cómo hacerlo.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Validar la aplicación de Windows usando el Kit para la certificación de aplicaciones en Windows desde una línea de comandos

> [!IMPORTANT]
> El Kit para la certificación de aplicaciones en Windows se debe ejecutar en el contexto de una sesión de usuario activa.

1. En la ventana de comandos, navega hacia el directorio que contiene el Kit para la certificación de aplicaciones en Windows.

    **Nota**   La ruta predeterminada es C:\\Archivos de programa\\Windows Kits\\10\\App certificación Kit\\.

2. Escribe los siguientes comandos en este orden para probar una aplicación que ya está instalada en el equipo de prueba:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    También puedes usar los siguientes comandos si la aplicación no está instalada. El Kit para la certificación de aplicaciones en Windows abrirá el paquete y aplicará el flujo de trabajo de prueba apropiado:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. Una vez que finalice la prueba, abre el archivo del informe `[report file name]` y revisa los resultados de la prueba.

**Nota**  El Kit para la certificación de aplicaciones en Windows se puede ejecutar desde un servicio, pero este debe iniciar el proceso del kit en una sesión de usuario activa y no se puede ejecutar en Session0.

**Nota**  Para obtener más información sobre la línea de comandos del Kit para la certificación de aplicaciones en Windows, escribe el comando `appcert.exe /?`.

## <a name="testing-with-a-low-power-computer"></a>Prueba de un equipo de bajo consumo

Los umbrales de la prueba de rendimiento del Kit para la certificación de aplicaciones en Windows se basan en el rendimiento de un equipo de bajo consumo.

Las características del equipo en el que se realiza la prueba pueden afectar a los resultados. Para determinar si el rendimiento de la aplicación cumple con las [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies), te recomendamos probarla en un equipo de bajo consumo; por ejemplo, un equipo basado en un procesador Intel Atom con una resolución de pantalla de 1366 x 768 (o superior) y un disco duro giratorio (en lugar de un disco duro de estado sólido).

A medida que evolucionan los equipos de bajo consumo, las características de rendimiento podrían cambiar con el tiempo. Consulta las [Directivas de la Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) más recientes y prueba la aplicación con la versión más reciente del Kit para la certificación de aplicaciones en Windows para asegurarte de que cumple los últimos requisitos de rendimiento.

## <a name="related-topics"></a>Temas relacionados

- [Pruebas del Kit para la certificación de aplicaciones en Windows](windows-app-certification-kit-tests.md)
- [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies)
