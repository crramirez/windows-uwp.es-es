---
author: awkoren
Description: "En este artículo se describen los problemas conocidos del Puente de dispositivo de escritorio a UWP."
Search.Product: eADQiWindows 10XVcnh
title: Problemas conocidos del Puente de dispositivo de escritorio
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: d7f7cfef59c02c3a8930c1bbc2c87cc40675ca22
ms.lasthandoff: 02/08/2017

---
# <a name="known-issues-with-the-desktop-bridge"></a>Problemas conocidos del Puente de dispositivo de escritorio

En este artículo se describen los problemas conocidos del Puente de dispositivo de escritorio a UWP.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Pantalla azul con el código de error 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Después de instalar o iniciar determinadas aplicaciones desde la Tienda Windows, la máquina se reinicia inesperadamente con el error: **0x139 (KERNEL\_SECURITY\_CHECK\_FAILURE)**.

Las aplicaciones afectadas conocidas incluyen Kodi, JT2Go, Ear Trumpet y Teslagrad, entre otras.

Se publicó una [actualización de Windows (versión 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) el 27/10/16 que incluye correcciones importantes que abordan este problema. Si se produce este problema, actualiza la máquina. Si no puedes actualizar el equipo porque la máquina se reinicia antes de poder iniciar sesión, debes usar la función Restaurar sistema para recuperar el sistema a un punto anterior a la instalación de una de las aplicaciones afectadas. Para obtener información sobre cómo usar Restaurar sistema, consulta [Opciones de recuperación de Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options). 

Si la actualización no soluciona el problema o no estás seguro de cómo recuperar tu equipo, ponte en contacto con el [Soporte técnico de Microsoft](https://support.microsoft.com/contactus/). 

Si eres un desarrollador, quizá quieras impedir la instalación de tus aplicaciones de puente de escritorio en las versiones de Windows que no incluyen esta actualización. Ten en cuenta que, al hacerlo, la aplicación no estará disponible para los usuarios que aún no han instalado la actualización. Para limitar la disponibilidad de la aplicación a los usuarios que han instalado esta actualización, modifica el archivo AppxManifest.xml de la siguiente manera:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Para conocer más detalles sobre Windows Update, consulta: 
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history
