---
title: Solucionar problemas de instalación de Xbox Live en PC Windows
description: Obtenga información sobre cómo solucionar problemas de su entorno de desarrollo de Xbox Live en un equipo con Windows.
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647740"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Solucionar problemas de instalación de Xbox Live en PC Windows

En el equipo de Windows 10, puede asegurarse de que su equipo está configurado correctamente con estos pasos:

1. Cambiar el equipo para que apunte al espacio aislado XDKS.1 donde los ejemplos están diseñados para ejecutarse.  Puede hacerlo mediante la ejecución de esta secuencia de comandos:

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. Extraiga el contenido del archivo zip "SourcesAndSamples.zip" se encuentra en el SDK.
1. Abra una solución de ejemplo:
    1. Para la API de C++: {*raíz del SDK de código fuente*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. Para las API de WinRT con C#: {*raíz del SDK de código fuente*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. Para la API de WinRT con C++ / c++ / CX: {*raíz del SDK de código fuente*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. Cambiar la plataforma de destino de compilación "Win32" o "x64".
1. A la derecha, haga clic en la solución y volver a generar todo.
1. Inicie la aplicación en el depurador.
1. Inicie sesión con la cuenta de desarrollo que ha creado en el [Portal para desarrolladores de Xbox](https://xdp.xboxlive.com), o con una cuenta de desarrollador comercial autorizado en [centro de partners](https://partner.microsoft.com/dashboard).
1. Conceder el permiso de aplicación para acceder a la información de Xbox Live.
1. Compruebe que la aplicación puede recuperar la información y puede ver el nombre de jugador.