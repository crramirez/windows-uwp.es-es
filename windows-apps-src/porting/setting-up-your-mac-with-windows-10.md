---
author: mcleblanc
description: Usa tu Mac actual para desarrollar aplicaciones para Windows.
title: "Configuración de Mac con Windows10"
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c73a1d1695e3b8a2eee8f073a5d25586a24a9d24
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="setting-up-your-mac-with-windows-10"></a>Configuración de Mac con Windows10

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa tu Mac actual para desarrollar aplicaciones para Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Ejecutar Windows en tu Mac y usar Visual Studio

¿Estás listo para empezar a desarrollar aplicaciones universales de Windows, pero no tienes un PC a mano? Eso no es ningún problema. Puedes usar tu Mac. Gracias a soluciones populares de terceros, como Apple Boot Camp, Oracle VirtualBox, VMware Fusion y Parallels Desktop, puedes instalar Windows 10 y Microsoft Visual Studio en tu equipo Apple.

**Nota** Necesitarás una imagen de arranque de Windows 10 en el disco o en la unidad flash USB. Si estás suscrito a MSDN, puedes descargar la imagen de instalación desde el Área de descarga para suscriptores de MSDN. Si no estás suscrito, puedes comprar el instalador en la [TiendaWindows](http://apps.microsoft.com/windows/app). También puedes descargarlo desde [esta ubicación](http://go.microsoft.com/fwlink/?LinkId=623906), lo que resulta útil si ya ejecutas Windows y quieres actualizar.

Cuando Windows esté en ejecución, puedes instalar Visual Studio 2015 desde el [área de descargas para desarrolladores de Windows 10](https://developer.microsoft.com/en-us/windows/downloads) y empezar a escribir aplicaciones.

**Nota** Si vas a usar los emuladores de dispositivos de Visual Studio, **debes** instalar una versión de 64bits (x64) de Windows 10 Pro o superior. Por desgracia, algunos Mac antiguos no pueden ejecutar Windows de 64 bits. Ponte en contacto con Apple en esta[página de asistencia de Apple](http://go.microsoft.com/fwlink/p/?LinkID=397959)para saber si tu hardware es compatible.

## <a name="apple-boot-camp"></a>Apple Boot Camp

La aplicación Boot Camp Assistant viene preinstalada en todos los equipos Mac recientes y si la inicias, te guiará por el proceso de instalación de Windows 10. Todo lo que necesitas es una copia de Windows (de las fuentes que se indican arriba) y al menos 30GB de espacio libre en disco. Una vez instalado, puedes arrancar en MacOSX o Windows10. Para obtener más información, consulta la [página de instrucciones de Boot Camp](http://go.microsoft.com/fwlink/?LinkId=623912) de Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Con Parallels Desktop 11, puedes ejecutar aplicaciones de Windows en paralelo con aplicaciones de Mac existentes, como Visual Studio y Cortana. Hay disponible una versión Pro que incluye funciones adicionales para desarrolladores, como por ejemplo, mejores prestaciones de depuración y compatibilidad con Docker y Jenkins. Para obtener más información y una versión de prueba gratuita, consulta [Parallels Desktop](http://go.microsoft.com/fwlink/p/?LinkId=281827).

## <a name="vmware-fusion"></a>VMWare Fusion

Fusion 8 de VMWare te permite ejecutar Visual Studio directamente en el escritorio Mac. Hay disponible una versión Pro para ofrecer a los desarrolladores funciones más avanzadas, como compatibilidad con vSphere. Para obtener más información y una versión de prueba gratuita, consulta [VMware Fusion](http://go.microsoft.com/fwlink/p/?LinkId=281826).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox es una aplicación gratuita para ejecutar máquinas virtuales en tu equipo y admite la ejecución de Windows en Mac. Es una opción sobria, pero el precio es atractivo. Para obtener más información, consulta [VirtualBox](http://go.microsoft.com/fwlink/p/?LinkId=280599).

