---
author: DelfCo
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: Tecnologías para tener acceso a la red y servicios web.
title: Redes y servicios web
---

# Redes y servicios web

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Las siguientes tecnologías de red y servicios web están disponibles para los desarrolladores de la Plataforma universal de Windows (UWP).

| Tema                                                                                   | Descripción                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [Conceptos básicos de redes](networking-basics.md)                                               | Cosas que debes hacer en cualquier aplicación habilitada para la red                     |
| [¿Qué tecnología de red?](which-networking-technology.md)                          | Una introducción rápida de las tecnologías de redes disponibles para un desarrollador UWP, con sugerencias sobre cómo elegir las tecnologías que son adecuadas para la aplicación.               |
| [Comunicaciones de red en segundo plano](network-communications-in-the-background.md) | Las aplicaciones usan tareas en segundo plano y dos mecanismos principales para mantener las comunicaciones cuando no están en primer plano: el agente de sockets y los desencadenadores del canal de control.                  |
| [Sockets](sockets.md)                                                                   | Puedes usar [Windows.Networking.Sockets](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.networking.sockets.aspx) y [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) para comunicarte con otros dispositivos como un desarrollador de aplicaciones de la aplicación para UWP. Este tema proporciona instrucciones detalladas sobre el uso del espacio de nombres de Windows.Networking.Sockets para llevar a cabo operaciones de red. |
| [WebSockets](websockets.md)                                                             | WebSocket ofrece un mecanismo para una comunicación bidireccional, rápida y segura entre un cliente y un servidor a través de Internet mediante HTTP(S).                 |
| [HttpClient](httpclient.md)                                                             | Usa la API del espacio de nombres [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) para enviar y recibir información mediante los protocolos HTTP 2.0 y HTTP 1.1.             |
| [Fuentes RSS y Atom](web-feeds.md)                                                          | Recupera o crea el contenido web más reciente o popular usando fuentes sindicadas generadas a partir de los estándares de RSS y Atom mediante características del espacio de nombres [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632).                   |
| [Transferencias en segundo plano](background-transfers.md)                                         | Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red.           |
| [Etiquetado de conexiones de red con identidad EDP](tagging_network_connections_with_edp_identity.md) | En este tema se muestra cómo crear un contexto de subproceso protegido antes de crear conexiones de red en un escenario de Enterprise Data Protection (EDP). |


<!--HONumber=May16_HO2-->


