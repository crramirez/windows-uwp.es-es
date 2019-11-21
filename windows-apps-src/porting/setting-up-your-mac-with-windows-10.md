---
description: Usa tu Mac actual para desarrollar aplicaciones para Windows.
title: Configuración de Mac con Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55165e0369c6bda64c19dc384c5c2addf224b8ba
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259119"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Configuración de Mac con Windows 10


Usa tu Mac actual para desarrollar aplicaciones para Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Ejecutar Windows en tu Mac y usar Visual Studio

¿Estás listo para empezar a desarrollar aplicaciones universales de Windows, pero no tienes un PC a mano? Eso no es ningún problema. Puedes usar tu Mac. Con soluciones de terceros populares como Apple Boot Camp, Oracle VirtualBox, VMware Fusion y Parallels Desktop, puede instalar Windows 10 y Microsoft Visual Studio en el equipo Apple.

**Tenga en cuenta**  necesitará una imagen de arranque de Windows 10 en disco o en una unidad flash USB. Si estás suscrito a MSDN, puedes descargar la imagen de instalación desde el Área de descarga para suscriptores de MSDN. Si no es suscriptor, el instalador se puede adquirir en el [Microsoft Store](https://www.microsoft.com/store/apps). También puedes descargarlo desde [esta ubicación](https://www.microsoft.com/software-download/windows10), lo que resulta útil si ya ejecutas Windows y quieres actualizar.

Una vez que tenga Windows en ejecución, puede instalar la versión más reciente de Visual Studio desde [descargas para desarrolladores para Windows 10](https://developer.microsoft.com/en-us/windows/downloads) y empezar a escribir aplicaciones.

**Tenga en cuenta**  si tiene previsto usar los emuladores de dispositivos de Visual Studio, **debe** instalar una versión de 64 bits (x64) de Windows 10 Pro o superior. Por desgracia, algunos Mac antiguos no pueden ejecutar Windows de 64 bits. Ponte en contacto con Apple en esta[página de asistencia de Apple](https://support.apple.com/kb/HT5634)para saber si tu hardware es compatible.

## <a name="apple-boot-camp"></a>Apple Boot Camp

La aplicación de asistente de inicio de campamento se instala previamente en cada Mac reciente y, al iniciarla, se le guiará por el proceso de instalación de Windows 10. Todo lo que necesitas es una copia de Windows (de las fuentes que se indican arriba) y al menos 30 GB de espacio libre en disco. Una vez instalado, puedes arrancar en Mac OSX o Windows 10. Para obtener más información, consulta la [página de instrucciones de Boot Camp](https://support.apple.com/HT201468) de Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Con Parallels Desktop 11, puedes ejecutar aplicaciones de Windows en paralelo con aplicaciones de Mac existentes, como Visual Studio y Cortana. Hay disponible una versión Pro que incluye funciones adicionales para desarrolladores, como por ejemplo, mejores prestaciones de depuración y compatibilidad con Docker y Jenkins. Para obtener más información y una versión de prueba gratuita, consulta [Parallels Desktop](https://www.parallels.com/download/desktop/).

## <a name="vmware-fusion"></a>VMWare Fusion

Fusion 8 de VMWare te permite ejecutar Visual Studio directamente en el escritorio Mac. Hay disponible una versión Pro para ofrecer a los desarrolladores funciones más avanzadas, como compatibilidad con vSphere. Para obtener más información y una versión de prueba gratuita, consulta [VMware Fusion](http://www.vmware.com/products/fusion/).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox es una aplicación gratuita para ejecutar máquinas virtuales en tu equipo y admite la ejecución de Windows en Mac. Es una opción sobria, pero el precio es atractivo. Para obtener más información, consulta [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

