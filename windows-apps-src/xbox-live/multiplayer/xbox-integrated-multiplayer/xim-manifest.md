---
title: Configuración del proyecto XIM
description: Describe cómo configurar los manifiestos de aplicación para UWP para trabajar con varios jugadores integrada de Xbox (XIM).
ms.assetid: 5d0ed7bd-9527-42a3-ae09-8cc9012c1111
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, xbox integrado participan varios jugadores, manifiesto
ms.localizationpriority: medium
ms.openlocfilehash: 926cc4495236fc4762f9b3176a5d6a4a579e2e2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613430"
---
# <a name="xim-project-configuration"></a>Configuración del proyecto XIM

## <a name="required-appxmanifest-capability-content"></a>Contenido de capacidad AppXManifest requerido

Una aplicación mediante XIM inherentemente requiere conexión a y Aceptar conexiones de los recursos de red tanto a través de Internet y la red local. También requiere acceso a los dispositivos de micrófono para admitir el chat de voz. Como resultado, la aplicación debe declarar el "internetClientServer" y "privateNetworkClientServer" capacidades y la funcionalidad del dispositivo "micrófono" en su AppXManifest. Consulte la documentación de plataforma para obtener más detalles sobre cada uno. El fragmento de código siguiente muestra los nodos que deben existir en el nodo de paquete/capacidades, o bien se bloquearán conectividad y chat:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <Capability Name="internetClientServer" />
     <Capability Name="privateNetworkClientServer" />
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="universal-windows-application-xbox-live-multiplayer-invite-protocol-activation-extension"></a>Aplicación universal de Windows, Xbox Live multijugador invitar a la extensión del protocolo de activación

Aplicaciones para varios jugadores de Xbox Live se esperan que admiten invitaciones al juego. Llegan invitaciones aceptados por los usuarios locales en el formulario de activación de protocolos de la aplicación (consulte la documentación de la plataforma para obtener más información acerca de la activación de protocolo). Las aplicaciones que usan XIM distinto para la invocación de redes XIM administradas a través de las reservas de direcciones fuera de banda la `xim::extract_protocol_activation_information()` estáticamente también debe declarar el método para ayudar en la activación de protocolos en tiempo de ejecución, pero las aplicaciones de la plataforma Universal de Windows (UWP) a través de un Extensión de "windows.protocol" AppXManifest que admiten dichas activaciones "ms-xbl multijugador" por el sistema. Esto puede hacerse tal como se muestra en el fragmento de código siguiente:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" IgnorableNamespaces="uap">
   <Applications>
     <Application ...>
       <Extensions>
         ...
          <uap:Extension Category="windows.protocol">
             <uap:Protocol Name="ms-xbl-multiplayer"/>
          </uap:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

Esto solo es necesario para aplicaciones universales de Windows. Aplicaciones Xbox One de recurso exclusivo ("XDK") no declaran explícitamente una extensión del protocolo de activación que se registran automáticamente mediante la declaración de su extensión de Id. de título de Xbox Live.

## <a name="xbox-one-exclusive-resource-application-network-manifest-templates"></a>Plantillas de manifiesto de red de aplicación de recurso exclusivo Xbox One

Cualquier aplicación de recurso exclusivo Xbox One que utilice XIM debe asegurarse de cierta descripción de socket y contenido de la plantilla se incluye en su extensión "manifiesto de la red" del AppXManifest (consulte la documentación de la plataforma para obtener más detalles sobre los manifiestos de red). La aplicación puede tener otras plantillas, pero el fragmento de código siguiente debe estar presente; de lo contrario se producirá un error al mover a una red XIM:

```xml

 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/2010/manifest" xmlns:mx="http://schemas.microsoft.com/appx/2013/xbox/manifest" IgnorableNamespaces="mx">
   <Applications>
     <Application ...>
       <Extensions>
         ...
         <mx:Extension Category="windows.xbox.networking">
           <mx:XboxNetworkingManifest>
             <mx:SocketDescriptions>
               <mx:SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Initiate" />
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
               <mx:SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
             </mx:SocketDescriptions>
             <mx:SecureDeviceAssociationTemplates>
               <mx:SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
               <mx:SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
             </mx:SecureDeviceAssociationTemplates>
           </mx:XboxNetworkingManifest>
         </mx:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

## <a name="universal-windows-application-uwp-network-manifest-templates"></a>Windows (UWP) de la aplicación red manifiesto plantillas universales

Cualquier aplicación universal de Windows que usa XIM debe asegurarse de cierta descripción de socket y contenido de la plantilla se incluye en su archivo de "red manifiesto" (archivo networkmanifest.xml en el directorio raíz del paquete, consulte la documentación de plataforma para obtener más información sobre manifiestos de red). La aplicación puede tener otras plantillas, pero el fragmento de código siguiente debe estar presente; de lo contrario se producirá un error al mover a una red XIM:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <NetworkManifest xmlns="http://schemas.microsoft.com/xbox/2012/networkmanifest">
   <SocketDescriptions>
     <SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Initiate" />
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
     <SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
   </SocketDescriptions>
   <SecureDeviceAssociationTemplates>
     <SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
     <SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
   </SecureDeviceAssociationTemplates>
 </NetworkManifest>
```
