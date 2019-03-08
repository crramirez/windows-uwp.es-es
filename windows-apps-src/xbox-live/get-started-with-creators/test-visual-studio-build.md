---
title: Probar el juego de Unity en Visual Studio
description: Lista de comprobación para la prueba correcta de Unity se compila en Visual Studio.
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589880"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>Probar la compilación de juegos Unity en Visual Studio

Para probar la funcionalidad de Xbox Live de los juegos de Unity con datos reales, debe crear el juego como se describe en el **compilación y prueba del proyecto** sección de la [configurar Xbox Live en el artículo de Unity](configure-xbox-live-in-unity.md). El siguiente artículo le proporcionará una lista de comprobación de los elementos para ayudar a garantizar una prueba correcta de su juego Unity en Visual Studio.

1. **Compruebe que tiene un título configurado correctamente asociado con el juego de Unity.**
    Si habilitó Xbox Live mediante el Asistente para asociación de Xbox Live, desea familiarizarse con [centro de partners](https://partner.microsoft.com/dashboard). Centro de partners le permite configurar la configuración de Xbox Live para el título y deben estar configurados correctamente en el orden del título para comunicarse con Xbox Live. El artículo [crear un nuevo título de programa de creadores de Xbox Live y publicar en el entorno de prueba](create-and-test-a-new-creators-title.md) le guiará por el proceso de instalación del centro de partners. Si ya ha configurado su juego a través de la **Asistente para configuración de Xbox** en Unity, puede ir a la sección [configuración del servicio de prueba de Xbox Live en su juego](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game). En el centro de partners, asegúrese de comprobar que la información de la configuración de Xbox Live para su juego Unity coincide con la configuración del centro de partners para su juego.

2. **Asegúrese de que el título tiene un Account(with gamertag) Microsoft autorizados que pueden iniciar sesión en su título.**
    Sin una cuenta autorizada no podrá completa iniciar sesión en durante la comprobación de su título, ni tampoco podrá utilizar otras características de Xbox Live. Para asegurarse de que tiene un Account de Microsoft y el nombre de jugador autorizados lee [autorizar Xbox Live cuentas para realizar pruebas en su entorno](authorize-xbox-live-accounts.md).

3. **Asegúrese de que se ha publicado el título de la prueba.**
    Al realizar cambios en el título, esos cambios debe publicarse en el cuadro de arena antes de que se pueden usar. Asegúrese de que se inserta el **prueba** botón para publicar los cambios a la configuración.

    ![Publicar para la imagen de prueba](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    El **prueba** botón se encuentra en [centro de partners](https://partner.microsoft.com/dashboard) configuración del servicio en Xbox Live el título de la. Si ha realizado algún cambio en la configuración, como agregar una nueva cuenta de pruebas autorizado quiere insertar el **probar** botón para publicar la nueva configuración en el servicio Xbox Live.

4. **Compruebe para asegurarse de que su equipo está en modo de programador y usa el recinto adecuado de Xbox Live.**
    Cuando se publica el título para las pruebas se publican en un entorno específico que se llama a un espacio aislado. Si el equipo de desarrollo no está configurado para usar ese espacio aislado no podrá probar las características de Xbox Live. Obtenga información sobre cómo comprobar y cambiar el espacio aislado de su PC con el [introducción de espacios aislados de Xbox Live](xbox-live-sandboxes-creators.md).

5. **Asegúrese de que se compila el proyecto como un x64 destinadas a la máquina local para crear en su equipo de compilación**

    ![configuración de compilación](../images/unity/get-started-with-creators/vsBuildSettings.JPG)