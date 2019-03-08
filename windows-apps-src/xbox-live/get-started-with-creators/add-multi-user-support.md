---
title: Agregar compatibilidad con varios usuario a su juego Unity
description: Agregar compatibilidad con varios usuarios a su juego Unity mediante el complemento Xbox Live Unity
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity, multiusuario
ms.localizationpriority: medium
ms.openlocfilehash: 483a0e966be756de483bf7e2ab8458647397687b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613740"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>Agregar compatibilidad con varios usuario a su juego Unity
> [!IMPORTANT]
> El complemento de Xbox Live Unity no es compatible con logros o juego multijugador en línea y solo se recomienda para [programa de creadores de Xbox Live](../developer-program-overview.md) miembros.

Compatibilidad con varios usuario ahora es compatible con la Xbox Live de complemento de Unity para escenarios que implican a varios reproductores locales. Cada jugador puede usar su propia cuenta de Xbox Live para estadísticas e inicio de sesión. El juego también puede habilitar a los invitados para los usuarios que no tienen cuentas de Xbox Live para reproducir. Esta característica solo se admite en las consolas Xbox.

Este tutorial le guiará a través de cómo agregar compatibilidad con varios usuario a la prefabricados diferentes de Xbox Live.

> [!IMPORTANT]
> Escenarios de varios usuarios solo se admiten en las consolas Xbox. Esta funcionalidad no funciona en PC.

## <a name="prerequisites"></a>Requisitos previos
Tendrá que ha seguido el [Introducción](configure-xbox-live-in-unity.md) tutorial para habilitar y configurar Xbox Live.

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>Agregar e inicio de sesión de varios usuarios de Xbox Live

1. Desde el **activos** > **Xbox Live** > **prefabricados** carpeta, arrastre a la escena tantos **XboxLiveUser** porque no hay instancias prefabricadas serán los jugadores. Para este tutorial, habrá dos jugadores dos **XboxLiveUser** instancias se agregarán a la escena. Nos referiremos a ellos como **XboxLiveUser** y **XboxLiveUser2** para mayor comodidad.

2. Para agregar los usuarios y firmarlos en correctamente, agregue las dos instancias de la **UserProfile** prefabricado a la escena, uno para cada **XboxLiveUser**. Asegúrese de que tiene una instancia de la `XboxLiveServices` prefabricado en la escena. Además, asegúrese de mover los dos **UserProfile** objetos separados en la escena para distinguirlos. Dado que estos prefabricados usan el Eventsystem Unity, asegúrese de que tiene una instancia de la `EventSystem` en la escena.

    ![Jerarquía de la compatibilidad con varios usuario en Xbox Live proyecto de complemento Tutorial de Unity](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Juego escena de la compatibilidad con varios usuario en el proyecto de complemento Tutorial de Unity en vivo de Xbox](../images/unity/MUA-Tutorial-GameScene.png)

3. Asignar una instancia de la **XboxLiveUser** prefabricados que haya en la escena para cada uno de los **UserProfile** objetos.

    ![UserProfile prefabricado para compatibilidad con varios usuario](../images/unity/user-profile-for-mua.png)

4. Dado que solo se admiten las aplicaciones de varios usuarios en dispositivos Xbox One, adición de compatibilidad de controlador para el **UserProfile** objetos es necesario. En cada **UserProfile** objeto, hay un campo denominado `InputControllerButton` donde puede especificar el joystick y botón numera cada **UserProfile** debe escuchar.

Para este tutorial, vamos a usar `joystick 1 button 0` para el **UserProfile** asignado al jugador 1 y `joystick 2 button 0` para Reproductor 2 y el segundo **UserProfile** objeto de juego. Esto asignará la `A` botón para interactuar con **UserProfile** para los dos controladores.

> [!Note]
> Para obtener más detalles sobre la compatibilidad de controlador de Xbox en el complemento de Unity de Xbox Live, consulte: [Agregar compatibilidad con controladores para Xbox Live prefabricados](add-controller-support-to-xbox-live-prefabs.md)

5. Ejecute la escena en el editor y posicionamiento que se ejecute en cada uno de los botones para asegurarse de que todo está configurado correctamente.

    ![Compatibilidad con varios usuario de las pruebas en el Editor de Unity](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>Compilar y probar la UWP

1. Después de seguir los pasos descritos en la [títulos de creadores de desarrollar con Unity](configure-xbox-live-in-unity.md) tutorial, abra la solución exportada de UWP en Visual Studio.

2. En el proyecto UWP de su juego, haga clic en el `package.appxmanifest.xml` de archivo y elija **ver código**.

3. En la `<Properties></Properties>` sección, agregue lo siguiente que permite la entrada de varios usuario de la aplicación: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. Para realizar pruebas en Xbox, siga la documentación de la [configurar su UWP en el entorno de desarrollo de Xbox](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) tutorial.

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>Uso de los otros Xbox Live prefabricados con varios usuarios

En el **ejemplos** carpeta del complemento, hay un **MultiUserExample** escena que muestra cómo pueden usar los diferentes prefabricados el **XboxLiveUser** instancias para mostrar específicos información para cada usuario.
