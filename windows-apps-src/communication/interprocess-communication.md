---
title: Comunicación entre procesos (IPC)
description: En este tema se explican varias maneras de realizar la comunicación entre procesos (IPC) entre aplicaciones Plataforma universal de Windows (UWP) y aplicaciones Win32.
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 0aa3c62100ecbb30e136c52cee3a6862cf15bef2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175629"
---
# <a name="interprocess-communication-ipc"></a>Comunicación entre procesos (IPC)

En este tema se explican varias maneras de realizar la comunicación entre procesos (IPC) entre aplicaciones Plataforma universal de Windows (UWP) y aplicaciones Win32.

## <a name="app-services"></a>App Services

App Services permite a las aplicaciones exponer servicios que aceptan y devuelven bolsas de propiedades de primitivas ([**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)) en segundo plano. Se pueden pasar objetos enriquecidos si se [serializan](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu).

App Services puede ejecutarse [fuera de proceso](../launch-resume/how-to-create-and-consume-an-app-service.md) como una tarea en segundo plano o [en proceso](../launch-resume/convert-app-service-in-process.md) dentro de la aplicación en primer plano.

Los servicios de aplicaciones se utilizan mejor para compartir pequeñas cantidades de datos en los que no se requiere latencia en tiempo real.

## <a name="com"></a>COM

[Com](/windows/win32/com/component-object-model--com--portal) es un sistema distribuido orientado a objetos para crear componentes de software binario que pueden interactuar y comunicarse. Como desarrollador, puede usar COM para crear componentes de software reutilizables y capas de automatización para una aplicación. Los componentes COM pueden estar en proceso o fuera de proceso y pueden comunicarse a través de un modelo de [cliente y servidor](/windows/win32/com/com-clients-and-servers) . Los servidores COM fuera de proceso se han utilizado durante mucho tiempo como medio para la [comunicación entre objetos](/windows/win32/com/inter-object-communication).

Las aplicaciones empaquetadas con la funcionalidad [runFullTrust](../packaging/app-capability-declarations.md#restricted-capabilities) pueden registrar servidores com fuera de proceso para IPC mediante el [manifiesto del paquete](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension). Esto se conoce como [com empaquetado](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/).

## <a name="filesystem"></a>Sistema de archivos

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

Las aplicaciones empaquetadas pueden realizar IPC con el sistema de archivos amplio mediante la declaración de la capacidad restringida de [broadFileSystemAccess](../files/file-access-permissions.md#accessing-additional-locations) . Esta funcionalidad concede a las API de [Windows. Storage](/uwp/api/Windows.Storage) y a las API de [xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) Win32 acceso a un sistema de archivos amplio.

De forma predeterminada, IPC a través del sistema de archivos para aplicaciones empaquetadas está restringido a los otros mecanismos descritos en esta sección.

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder) permite a las aplicaciones empaquetadas declarar carpetas en su manifiesto que se pueden compartir con otros paquetes del mismo publicador.

La carpeta de almacenamiento compartido tiene los siguientes requisitos y restricciones:

* No se realiza una copia de seguridad ni se mueven los datos de la carpeta de almacenamiento compartido.
* El usuario puede borrar el contenido de la carpeta de almacenamiento compartido.
* No puede utilizar la carpeta de almacenamiento compartido para compartir datos entre aplicaciones de distintos publicadores.
* No se puede usar la carpeta de almacenamiento compartido para compartir datos entre distintos usuarios.
* La carpeta de almacenamiento compartido no tiene administración de versiones.

Si publica varias aplicaciones y está buscando un mecanismo sencillo para compartir datos entre ellas, el PublisherCacheFolder es una opción simple basada en el sistema de archivos.

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[SharedAccessStorageManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) se usa junto con App Services, las activaciones de protocolos (por ejemplo, LaunchUriForResultsAsync), etc., para compartir StorageFiles a través de tokens.

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

Con la funcionalidad [runFullTrust](../packaging/app-capability-declarations.md#restricted-capabilities) , las aplicaciones empaquetadas pueden [iniciar procesos](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher) de plena confianza en el mismo paquete.

En el caso de los escenarios en los que las restricciones de paquete son una carga, o cuando faltan opciones IPC, una aplicación podría usar un proceso de plena confianza como proxy para interactuar con el sistema y, a continuación, IPC con el propio proceso de plena confianza a través de App Services o algún otro mecanismo de IPC compatible.

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](../launch-resume/how-to-launch-an-app-for-results.md) se usa para el intercambio de datos simple ([ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)) con otras aplicaciones empaquetadas que implementan el contrato de activación [ProtocolForResults](../launch-resume/how-to-launch-an-app-for-results.md#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results) . A diferencia de App Services, que normalmente se ejecutan en segundo plano, la aplicación de destino se inicia en primer plano.

Los archivos se pueden compartir pasando tokens de [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) a la aplicación a través de la ValueSet.

## <a name="loopback"></a>Bucle invertido

Bucle invertido es el proceso de comunicación con un servidor de red que escucha en localhost (la dirección de bucle invertido).

Para mantener la seguridad y el aislamiento de red, las conexiones de bucle invertido para IPC están bloqueadas de forma predeterminada para las aplicaciones empaquetadas. Puede habilitar las conexiones de bucle invertido entre la aplicación empaquetada de confianza mediante las [funcionalidades](/previous-versions/windows/apps/hh770532(v=win.10)) y [las propiedades del manifiesto](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules).

* Todas las aplicaciones empaquetadas que participan en conexiones de bucle invertido deberán declarar la `privateNetworkClientServer` funcionalidad en sus [manifiestos de paquete](/uwp/schemas/appxpackage/uapmanifestschema/element-capability).
* Dos aplicaciones empaquetadas pueden comunicarse a través de bucle invertido declarando [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) dentro de sus manifiestos de paquete.
    * Cada aplicación debe mostrar la otra en su [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules). El cliente declara una regla de "salida" para el servidor y el servidor declara las reglas "en" para sus clientes compatibles.

> [!NOTE]
> El nombre de familia de paquete necesario para identificar una aplicación en estas reglas puede encontrarse en el editor de manifiestos de paquete de Visual Studio durante el tiempo de desarrollo, a través del [centro de Partners](../publish/view-app-identity-details.md) para aplicaciones publicadas a través del Microsoft Store o mediante el comando de PowerShell [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) para las aplicaciones que ya están instaladas.

Las aplicaciones y los servicios sin empaquetar no tienen una identidad de paquete, por lo que no se pueden declarar en [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules). Puede configurar una aplicación empaquetada para que se conecte a través de bucle invertido con aplicaciones y servicios sin empaquetar a través de [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)), pero esto solo es posible para escenarios de transferencia de prueba o depuración en los que tiene acceso local a la máquina y tiene privilegios de administrador.

* Todas las aplicaciones empaquetadas que participan en conexiones de bucle invertido deben declarar la `privateNetworkClientServer` funcionalidad en sus [manifiestos de paquete](/uwp/schemas/appxpackage/uapmanifestschema/element-capability).
* Si una aplicación empaquetada se conecta a una aplicación o un servicio no empaquetado, ejecute `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` para agregar una exención de bucle invertido para la aplicación empaquetada.
* Si una aplicación o un servicio sin empaquetar se está conectando a una aplicación empaquetada, ejecute `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` para permitir que la aplicación empaquetada reciba conexiones de bucle invertido entrantes.
    * [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) se debe ejecutar continuamente mientras la aplicación empaquetada esté escuchando conexiones.
    * La `-is` marca se presentó en Windows 10, versión 1607 (10,0; Compilación 14393).

> [!NOTE]
> El nombre de familia de paquete necesario para la `-n` marca de [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) se puede encontrar a través del editor de manifiestos de paquete de Visual Studio durante el tiempo de desarrollo, a través del [centro de Partners](../publish/view-app-identity-details.md) para las aplicaciones publicadas a través del Microsoft Store, o mediante el comando de PowerShell [Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) para las aplicaciones que ya están instaladas.

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) también es útil para [depurar problemas de aislamiento de red](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues).

## <a name="pipes"></a>Canalizaciones

Las [canalizaciones](/windows/win32/ipc/pipes) permiten una comunicación simple entre un servidor de canalización y uno o más clientes de canalización.

Las [canalizaciones anónimas](/windows/win32/ipc/anonymous-pipes) y con [nombre](/windows/win32/ipc/named-pipes) se admiten con las siguientes restricciones:

* De forma predeterminada, las canalizaciones con nombre en aplicaciones empaquetadas solo se admiten entre procesos dentro del mismo paquete, a menos que un proceso sea de plena confianza.
* Las canalizaciones con nombre pueden compartirse entre paquetes siguiendo las instrucciones para [compartir objetos con nombre](./sharing-named-objects.md).
* Las canalizaciones con nombre en aplicaciones empaquetadas deben utilizar la sintaxis del `\\.\pipe\LOCAL\` nombre de la canalización.

## <a name="registry"></a>Registro

Por lo general, no se recomienda el uso [del registro](/windows/win32/sysinfo/registry-functions) para IPC, pero se admite para el código existente. Las aplicaciones empaquetadas solo pueden tener acceso a las claves del registro para las que tienen permiso de acceso.

[Las aplicaciones de escritorio empaquetadas como MSIX](/windows/msix/desktop/desktop-to-uwp-root) aprovechan la [virtualización del registro](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) , de modo que las escrituras globales del registro están contenidas en un subárbol privado en el paquete MSIX. Esto permite la compatibilidad del código fuente a la vez que se reduce el impacto global del registro y se puede usar para IPC entre procesos en el mismo paquete. Si debe utilizar el registro, se prefiere este modelo frente a la manipulación del registro global.

## <a name="rpc"></a>RPC

[RPC](/windows/win32/rpc/rpc-start-page) se puede usar para conectar una aplicación empaquetada a un punto de conexión RPC de Win32, siempre que la aplicación empaquetada tenga las capacidades correctas para que coincidan con las ACL del extremo de RPC.

Las funciones personalizadas permiten a los fabricantes de equipos originales y IHV [definir capacidades arbitrarias](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability), [ACL sus extremos RPC](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)y, a continuación, [conceder esas capacidades a aplicaciones cliente autorizadas](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file). Para obtener una aplicación de ejemplo completa, vea el ejemplo [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) .

Los extremos de RPC también se pueden agregan a aplicaciones empaquetadas específicas para limitar el acceso al punto de conexión solo a esas aplicaciones sin necesidad de la sobrecarga de administración de funcionalidades personalizadas. Puede usar la API [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) para derivar un SID de un nombre de familia de paquete y, a continuación, la ACL del punto de conexión RPC con el SID, tal como se muestra en el ejemplo [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp) .

## <a name="shared-memory"></a>Memoria compartida

La [asignación de archivos](/windows/win32/memory/sharing-files-and-memory) se puede usar para compartir un archivo o memoria entre dos o más procesos con las siguientes restricciones:

* De forma predeterminada, las asignaciones de archivos en aplicaciones empaquetadas solo se admiten entre procesos dentro del mismo paquete, a menos que un proceso sea de plena confianza.
* Las asignaciones de archivos se pueden compartir entre paquetes siguiendo las instrucciones para [compartir objetos con nombre](./sharing-named-objects.md).

Se recomienda la memoria compartida para compartir y manipular grandes cantidades de datos de forma eficaz.