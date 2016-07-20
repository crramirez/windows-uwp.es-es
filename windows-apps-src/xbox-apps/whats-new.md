---
author: v-angraf
title: Novedades para UWP en Xbox One
description: "Destaca las nuevas características para la UWP en aplicaciones para Xbox One."
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: edc9a914f200c643b1133cf07778e2ca3931d0d9

---

# Novedades en la Developer Preview para la UWP en Xbox One de julio de 2016

La versión de Developer Preview de julio de 2016 de la Plataforma universal de Windows (UWP) en Xbox One contiene las características nuevas, las actualizaciones para funciones existentes y las correcciones de errores siguientes.

## El acceso a redes con sockets TCP/UDP ya está disponible  
Ya está disponible el acceso a redes entrantes y salientes desde una consola que use sockets tradicionales TCP/UDP (WinSock, Windows.Networking.Sockets).

## Compatibilidad con Fiddler
Ahora puedes habilitar a Fiddler como proxy para una consola que tenga habilitada la Plataforma universal de Windows (UWP) en Xbox One. Fiddler te permite registrar e inspeccionar todo el tráfico HTTP/HTTPS hacia y desde servicios de Xbox y servicios web de terceros de confianza. Para obtener más información, consulta [Cómo usar Fiddler con Xbox One al desarrollar para la UWP](uwp-fiddler.md).

## Ahora el modo de mouse está habilitado de manera predeterminada
Ahora el modo de mouse está habilitado de manera predeterminada para XAML y las aplicaciones web hospedadas.
Recomendamos encarecidamente desactivarlo y optimizar para la navegación con el mando de dirección.
Para obtener información sobre cómo desactivar el modo de mouse, consulta [Cómo deshabilitar el modo de mouse](how-to-disable-mouse-mode.md).
Para obtener más información sobre cómo compilar aplicaciones fabulosas para Xbox, consulta [Diseño para Xbox y televisión](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode).

## El área de superficie de API para UWP extendida ahora es funcional en la consola
Ahora hay más API para UWP que son funcionales en la consola Xbox. Para obtener más información sobre la compatibilidad de las API para la UWP, consulta [UWP features that aren’t yet supported on Xbox](http://go.microsoft.com/fwlink/?LinkID=760755) (Características UWP que aún no se admiten en Xbox One). 

## Funcionalidades de música y audio en segundo plano
Ahora es posible reproducir música y audio desde una aplicación que se ejecute en segundo plano.

## Mejoras de XAML
Se han realizado las siguientes mejoras en la plataforma XAML:
-   El rectángulo de foco ahora se ha adaptado para una experiencia de 10pies de televisión.
-   Los sonidos de Xbox ahora están incrustados en la plataforma XAML.
-   Se ha mejorado la navegación con foco XY entre elementos de la interfaz de usuario. 

## Ahora puedes cambiar el tamaño de almacenamiento de desarrollador asignado en la consola
Una nueva opción de configuración de la aplicación Dev Home te permite aumentar o disminuir el tamaño de almacenamiento de desarrollador asignado en la consola. Para obtener más información acerca de cómo cambiar el tamaño de almacenamiento de desarrollador asignado, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).

## Mejoras en las herramientas WDP
Se han realizado las siguientes mejoras a la herramienta Windows Device Portal (WDP) para Xbox:
 - La herramienta incluye opciones de configuración de la consola adicionales. Para obtener más información sobre la configuración de la consola, consulta el tema de referencia [/ext/settings](wdp-xboxsettings-api.md). 
 - Los usuarios pueden tener una sesión iniciada o cerrada en la consola. Para obtener más información acerca de los usuarios, consulta el tema de referencia [usuario/ext](wdp-user-management.md).
 - Ya puedes obtener una captura de pantalla de la consola. Para obtener más información acerca de cómo hacer una captura de pantalla, consulta el tema de referencia [/ext/screenshot](wdp-media-capture-api.md).
 - La herramienta puede implementar una compilación de archivos sueltos de la aplicación. Para obtener más información sobre compilaciones de archivos sueltos, consulta el tema de referencia [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Puedes acceder a los archivos de desarrollador de la consola desde el Explorador de archivos del equipo de desarrollo. Para obtener más información sobre cómo acceder a archivos mediante el Explorador de archivos, consulta el tema de referencia [/ext/smb/developerfolder](wdp-smb-api.md).

## Consulta también
- [UWP en Xbox One](index.md)
- [Problemas conocidos](known-issues.md)



<!--HONumber=Jul16_HO2-->


