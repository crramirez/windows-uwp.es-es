---
Description: Conozca las respuestas a algunas preguntas básicas de los desarrolladores sobre Windows 10 veces.
title: Preguntas más frecuentes para desarrolladores de Windows 10
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 3ba14e33c098d3515522a9a5907065751fafba87
ms.sourcegitcommit: 13bda6040988461a61b1b5561fde2f7a54835ccd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84318241"
---
# <a name="windows-10x-developer-faq"></a>Preguntas más frecuentes para desarrolladores de Windows 10

> [!IMPORTANT]
> Recientemente anunciamos algunos cambios en la priorización de Windows 10 y Windows 10 veces.
> Estos anuncios incluyen cambios en las prioridades de factor de forma 10X de Windows. [Obtenga más información aquí.](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10X es una línea de productos de la familia de Windows optimizada para su uso en dispositivos de pantalla dual. Como desarrollador, puede llegar a un público más amplio mediante la optimización de la aplicación para Windows 10 veces, aprovechando las nuevas características específicas para un público móvil y de pantalla dual, al tiempo que disfruta de la misma gama de funciones de Windows 10 y compatibilidad con escritorio enriquecida. [Anunciamos Windows 10 veces en 2019](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97), y estamos a la hora de publicarla a finales de 2020.

![Dispositivos que ejecutan Windows 10 veces](images/windows-10x-devices.png)
 
*[Producto de versión preliminar mostrado, pantallas simuladas y sujetas a cambios]*

Para obtener más información sobre cómo compilar experiencias de pantalla dual y Windows 10X, consulte las sesiones virtuales de los documentos [Microsoft 365 dev Day](https://developer.microsoft.com/microsoft-365/virtual-events)o de [desarrollador de dos pantallas](https://docs.microsoft.com/dual-screen/). Para obtener información de un vistazo, aquí encontrará respuestas a algunas preguntas que puede tener.

### <a name="how-is-this-different-from-developing-for-windows-10"></a>¿En qué se diferencia el desarrollo para Windows 10?

Para la mayoría de las aplicaciones, no es diferente. La escritura de aplicaciones 10X de Windows se admite a través del SDK de Windows 10. Como una expresión de Windows 10, Windows 10X admite API de Windows Runtime (WinRT) y ejecuta aplicaciones Win32 a través de un contenedor nativo. Después, puede llamar a nuevas API específicamente diseñadas para dispositivos de pantalla dual desde aplicaciones UWP o Win32 nuevas o existentes, lo que les permite acceder a las características y las ventajas de esta nueva plataforma.

### <a name="does-this-replace-desktop-windows-10"></a>¿Reemplaza el escritorio Windows 10?

No. Windows 10X se lanzará en paralelo a las versiones de escritorio de Windows 10. Las versiones de escritorio de Windows 10 seguirán proporcionando mejoras y mejoras en el caso de la aplicación de escritorio moderna. Windows 10X es otra plataforma optimizada para admitir plataformas de pantalla dual.

### <a name="when-will-windows-10x-be-released"></a>¿Cuándo se lanzarán 10 veces de Windows?

Windows 10 se lanzará para acompañar a la superficie neo y otros dispositivos de pantalla dual de terceros a finales de 2020.

### <a name="when-can-i-start-development-for-windows-10x"></a>¿Cuándo puedo empezar a desarrollar para Windows 10 veces?

Puede descargar el [emulador de Microsoft y la imagen del emulador 10x de Windows](https://docs.microsoft.com/dual-screen/windows/get-dev-tools) hoy mismo. Seguiremos mejorando este emulador y lo complementaremos con la compatibilidad con otros dispositivos habilitados para Windows 10. Estos emuladores, combinados con versiones preliminares de la Windows SDK, le permitirán desarrollar para Windows 10 veces antes de que el primer dispositivo de pantalla doble se publique públicamente.

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>¿Las aplicaciones de Plataforma universal de Windows (UWP) se ejecutan en Windows 10X?

La mayoría de las aplicaciones para UWP son totalmente compatibles con Windows 10X y funcionan en dispositivos que ejecutan Windows 10X sin ningún cambio. Se admiten todas las API de WinRT, al igual que la mayoría de las demás características a las que tienen acceso las aplicaciones UWP. A medida que continúa el desarrollo de versiones preliminares, se publicará documentación que detalla otras características no admitidas.

### <a name="will-my-win32-apps-run-on-windows-10x"></a>¿Las aplicaciones de Win32 se ejecutan en Windows 10 veces?

Windows 10X proporciona compatibilidad nativa para ejecutar aplicaciones Win32 en un entorno contenido. La mayoría de las aplicaciones de Win32 se pueden ejecutar y depurar en un dispositivo con Windows 10X sin incidentes, y también puede usar nuevas API de Win32 para agregar compatibilidad con pantalla dual a la aplicación.

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>¿Hay alguna característica de mi aplicación que no funcione en Windows 10X?

A medida que Windows 10X continúe su desarrollo de versión preliminar, publicaremos la documentación que resalta sus limitaciones específicas. Sin embargo, el entorno contenido usado para ejecutar aplicaciones Win32 no incluye el shell de Windows y, por tanto, no se admitirán extensiones de Shell ni características similares. De forma similar, los dispositivos con Windows 10 no admiten las API relacionadas con ciertas configuraciones del sistema, como las opciones de uso de energía.

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>Si se mejora la aplicación con las características de 10 veces, ¿se seguirá ejecutando en los dispositivos que ejecutan Windows 10 de escritorio?

Las aplicaciones diseñadas para Windows 10X siguen funcionando en los dispositivos que ejecutan la versión de escritorio de Windows 10, aunque estas nuevas API de Windows no se agregarán a las versiones de escritorio de Windows 10 hasta la siguiente actualización de la versión principal. Del mismo modo que si estuviera desarrollando una aplicación compatible con varias versiones de Windows 10 de escritorio, siga las [prácticas recomendadas de codificación adaptativa](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code) para asegurarse de que la aplicación funciona correctamente en las 10 veces y en las de escritorio. 
