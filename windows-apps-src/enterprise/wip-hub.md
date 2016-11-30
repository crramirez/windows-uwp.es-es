---
author: normesta
Description: "En este tema del centro se describe un panorama completo de desarrollador sobre cómo Windows Information Protection (WIP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada."
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows Information Protection (WIP)
translationtype: Human Translation
ms.sourcegitcommit: 724d9edf67d0f73ceb3eb2ac323e0a0f42f2dd0d
ms.openlocfilehash: f9cfa8d1d7ea4e78208a4fb3fc853884a13a676c

---

# Windows Information Protection (WIP)

__Nota__ La directiva de Windows Information Protection (WIP) se puede aplicar a Windows 10, versión 1607.

WIP protege los datos que pertenecen a una organización aplicando las directivas definidas por esta. Si la aplicación se incluye en las directivas, todos los datos generados por la aplicación están sujetos a las restricciones de la directiva. Este tema te ayuda a crear aplicaciones que aplican estas directivas de manera correcta sin que esto afecte para nada los datos personales del usuario.

## En primer lugar, ¿qué es WIP?

WIP es un conjunto de características en equipos de escritorio, portátiles, tabletas y teléfonos que son compatibles con el sistema de administración de dispositivos móviles (MDM). WIP te proporciona un mayor control sobre cómo se gestionan los datos en los dispositivos que administra la organización. Por ejemplo, los administradores pueden identificar qué aplicaciones pueden acceder a los archivos que pertenecen a la organización y si los usuarios pueden copiar los datos de estos archivos y, a continuación, pegarlos en documentos personales.

Así es como funciona. Los usuarios inscriben sus dispositivos en el sistema de administración de dispositivos móviles (MDM). Un administrador de la organización administrativa usa Microsoft Intune o System Center Configuration Manager (SCCM) para definir y, a continuación, implementar una directiva en los dispositivos inscritos.

Esa directiva identifica las aplicaciones que pueden acceder a los datos de empresa (conocida como la *lista permitida* de la directiva). Estas aplicaciones pueden acceder a archivos de empresa protegidos, redes privadas virtuales (VPN) y datos de empresa en el Portapapeles o través de un contrato para contenido compartido. La directiva también define las reglas que controlan los datos. Por ejemplo, si los datos pueden copiarse desde los archivos que sean propiedad de la empresa y, a continuación, pegarse en archivos que no pertenezcan a esta.

Si los usuarios anulan la inscripción de su dispositivo del sistema MDM de la organización, los administradores pueden borrar de forma remota los datos de empresa desde este dispositivo.

![Ciclo de vida de WIP](images/wip-lifecycle.png)

> **Más información sobre WIP** <br>
* [Introducción a Windows Information Protection](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [Protege los datos de tu empresa con Windows Information Protection (WIP)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

Si la aplicación está en la lista de aplicaciones permitidas, todos los datos generados por la aplicación están sujetos a las restricciones de la directiva. Esto significa que si los administradores revocan el acceso del usuario a los datos de empresa, dichos usuarios perderán el acceso a todos los datos de la aplicación que has creado.

Esto está bien si tu aplicación está diseñada solo para el uso de la empresa. Pero si tu aplicación crea datos que los usuarios consideran personales, querrás *habilitar* tu aplicación para que distinga de manera inteligente entre los datos personales y de empresa. Este tipo de aplicaciones se denominan *habilitadas para empresas* porque pueden aplicar la directiva mientras que conservan la integridad de los datos personales del usuario.

## Crear una aplicación habilitada para empresas

Usa API de WIP para optimizar la aplicación y, a continuación, declarar tu aplicación como habilitada para empresas.

Optimiza tu aplicación si se va a utilizar para fines de empresa y personales.

Optimizar la aplicación si desea controlar correctamente el cumplimiento de los elementos de directiva.

Por ejemplo, si la directiva permite a los usuarios pegar datos de empresa en un documento personal, puedes evitar que los usuarios tengan que responder a un cuadro de diálogo de consentimiento antes de pegar los datos. Del mismo modo, puedes presentar cuadros de diálogo informativos personalizados para responder a estos tipos de eventos.

Si estás listo para optimizar tu aplicación, consulta una de estas guías:

**Para las aplicaciones de la Plataforma Universal de Windows (UWP) creadas con C.#**

[Crear una aplicación habilitada que consume datos de empresa y personales (C++)](wip-dev-guide.md).

**Para aplicaciones de escritorio creadas con C++**

[Crear una aplicación habilitada que consume datos de empresa y personales (C++)](http://go.microsoft.com/fwlink/?LinkId=822192).

Las aplicaciones habilitadas para empresas comparten, entre otras, estas cualidades:

* Mantienen los datos de empresa protegidos, ya estén en reposo, en uso o en desarrollo.
* Reconocen los datos personales y evita que los datos estén sujetos a restricciones de directiva.
* Reconocen los datos de empresa y los protegen cuando llegan a la aplicación.
* Protegen los datos de empresa que salen de la aplicación.

  Por ejemplo, no permite que el contenido vaya a un punto de conexión de red no empresarial, encapsula los datos en un formulario cifrado portátil antes de permitir que utilicen un perfil móvil y, posiblemente (según la configuración de directiva), pide confirmación al usuario antes de pegar datos de empresa en una aplicación que no está en la lista de aplicaciones permitidas.







 



<!--HONumber=Nov16_HO1-->


