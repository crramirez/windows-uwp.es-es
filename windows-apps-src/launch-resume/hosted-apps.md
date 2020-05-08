---
Description: Obtenga información sobre cómo compilar una aplicación hospedada que hereda los atributos de tiempo de ejecución, punto de entrada y archivo ejecutable de una aplicación host.
title: Crear aplicaciones hospedadas
ms.date: 04/23/2020
ms.topic: article
keywords: Windows 10, escritorio, paquete, identidad, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ed4356513e406c7c787ec111d32560ac08d293f1
ms.sourcegitcommit: f26d0b22a70b05679fc7089e11d639ba1a4a23af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "82107728"
---
# <a name="create-hosted-apps"></a>Crear aplicaciones hospedadas

A partir de Windows 10, versión 2004, puede crear *aplicaciones hospedadas*. Una aplicación hospedada comparte el mismo archivo ejecutable y la misma definición que una aplicación *host* principal, pero parece y se comporta como una aplicación independiente en el sistema.

Las aplicaciones hospedadas son útiles para los escenarios en los que desea que un componente (como un archivo ejecutable o un archivo de script) se comporte como una aplicación de Windows 10 independiente, pero el componente requiere un proceso de host para poder ejecutarlo. Por ejemplo, se puede entregar un script de PowerShell o de Python como una aplicación hospedada que requiere la instalación de un host para poder ejecutarse. Una aplicación hospedada puede tener su propio icono de inicio, su identidad y una profunda integración con las características de Windows 10, como tareas en segundo plano, notificaciones, mosaicos y destinos de los recursos compartidos.

La característica de aplicaciones hospedadas es compatible con varios elementos y atributos del manifiesto de paquete que permiten a una aplicación hospedada usar un ejecutable y una definición en un paquete de aplicación host. Cuando un usuario ejecuta la aplicación hospedada, el sistema operativo inicia automáticamente el archivo ejecutable del host en la identidad de la aplicación hospedada. Después, el host puede cargar activos visuales, contenido o llamar a las API como la aplicación hospedada. La aplicación hospedada obtiene la intersección de las funciones declaradas entre el host y la aplicación hospedada. Esto significa que una aplicación hospedada no puede solicitar más capacidades de las que proporciona el host.

## <a name="define-a-host"></a>Definir un host

El *host* es el ejecutable principal o el proceso en tiempo de ejecución de la aplicación hospedada. Actualmente, los únicos hosts admitidos son las aplicaciones de escritorio (.NET o C++/Win32) que tienen la *identidad del paquete*. Las aplicaciones UWP no se admiten como hosts en este momento. Hay varias maneras de que una aplicación de escritorio tenga la identidad del paquete:

* La forma más común de conceder la identidad del paquete a una aplicación [de escritorio es empaquetarla en un paquete MSIX](https://docs.microsoft.com/windows/msix).
* En algunos casos, también puede optar por conceder la identidad del paquete mediante la creación de un [paquete disperso](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps). Esta opción es útil si no puede adoptar el empaquetado de MSIX para implementar la aplicación de escritorio.

La extensión [**uap10: HostRuntime**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) declara el host en el manifiesto del paquete. Esta extensión tiene un atributo de **identificador** al que se debe asignar un valor al que también se hace referencia en el manifiesto del paquete de la aplicación hospedada. Cuando se activa la aplicación hospedada, el host se inicia en la identidad de la aplicación hospedada y puede cargar contenido o archivos binarios desde el paquete de la aplicación hospedada.

En el ejemplo siguiente se muestra cómo definir un host en un manifiesto del paquete. La extensión **uap10: HostRuntime** está en todo el paquete y, por lo tanto, se declara como un elemento secundario del elemento del [**paquete**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-package) .

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

Tome nota de estos detalles importantes sobre los siguientes elementos.

| Elemento              | Detalles |
|----------------------|-------|
| [**uap10:Extension**](https://docs.microsoft.com/wp/schemas/appxpackage/uapmanifestschema/element-uap10-extension) | La `windows.hostRuntime` categoría declara una extensión de todo el paquete que define la información en tiempo de ejecución que se va a usar al activar una aplicación hospedada. Una aplicación hospedada se ejecutará con las definiciones declaradas en la extensión. Al usar la aplicación host declarada en el ejemplo anterior, una aplicación hospedada se ejecutará como el archivo ejecutable **PyScriptEngine. exe** en el nivel de confianza **mediumIL** .<br/><br/>Los atributos **Executable**, **uap10: RuntimeBehavior**y **uap10: trustLevel** especifican el nombre del archivo binario del proceso de host en el paquete y cómo se ejecutarán las aplicaciones hospedadas. Por ejemplo, una aplicación hospedada que use los atributos del ejemplo anterior se ejecutará como el archivo ejecutable PyScriptEngine. exe en el nivel de confianza mediumIL. |
| [**uap10:HostRuntime**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) | El atributo **ID** declara el identificador único de esta aplicación host específica en el paquete. Un paquete puede tener varias aplicaciones host y cada una debe tener un elemento **uap10: HostRuntime** con un **identificador**único.

## <a name="declare-a-hosted-app"></a>Declarar una aplicación hospedada

Una *aplicación hospedada* declara una dependencia del paquete en un *host*. La aplicación hospedada aprovecha el identificador del host (es decir, el atributo **ID** de la extensión **uap10: HostRuntime** en el paquete host) para la activación en lugar de especificar un ejecutable de punto de entrada en su propio paquete. Normalmente, la aplicación hospedada contiene contenido, recursos visuales, scripts o archivos binarios a los que puede acceder el host. El valor de [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) del paquete de la aplicación hospedada debe tener como destino el mismo valor que el host.

Los paquetes de aplicaciones hospedadas se pueden firmar o sin firmar:

* Los paquetes firmados pueden contener archivos ejecutables. Esto resulta útil en escenarios que tienen un mecanismo de extensión binaria, que permite al host cargar un archivo DLL o un componente registrado en el paquete de la aplicación hospedada.
* Los paquetes sin firmar solo pueden contener archivos no ejecutables. Esto resulta útil en escenarios en los que el host solo necesita cargar imágenes, activos, contenido o archivos de scripts. Los paquetes sin firmar deben incluir un valor `OID` especial en su elemento [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) o no podrán registrarse. Esto evita que los paquetes sin firmar entren en conflicto con o suplantar la identidad de un paquete firmado.

Para definir una aplicación hospedada, declare los siguientes elementos en el manifiesto del paquete:

* El elemento [**uap10: HostRuntimeDependency**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency) . Se trata de un elemento secundario del elemento [dependencies](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) .
* El atributo **uap10: HostId** del elemento [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) (para una aplicación) o un elemento [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) (para una extensión activable).

En el ejemplo siguiente se muestran las secciones pertinentes de un manifiesto del paquete para una aplicación hospedada sin firmar.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

Tome nota de estos detalles importantes sobre los siguientes elementos.

| Elemento              | Detalles |
|----------------------|-------|
| [**Identidad**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | Dado que el paquete de aplicación hospedada en este ejemplo está sin signo, el atributo de **publicador** debe incluir la `OID.2.25.311729368913984317654407730594956997722=1` cadena. Esto garantiza que el paquete sin firmar no puede suplantar la identidad de un paquete firmado. |
| [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | El atributo **MinVersion** debe especificar 10.0.19041.0 o una versión posterior del sistema operativo. |
| [**uap10:HostRuntimeDependency**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)  | Este elemento de elemento declara una dependencia en el paquete de la aplicación host. Consta del **nombre** y el **publicador** del paquete de host y el **MinVersion** del que depende. Estos valores se pueden encontrar en el elemento [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) del paquete host. |
| [**Aplicación**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) | El atributo **uap10: HostId** expresa la dependencia en el host. El paquete de la aplicación hospedada debe declarar este atributo en lugar de los atributos de **archivo ejecutable** y de **punto** de entrada habituales para una [**aplicación**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) o un elemento de [**extensión**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) . Como resultado, la aplicación hospedada hereda los atributos **Executable**, **EntryPoint** y Runtime del host con el valor **HostId** correspondiente.<br/><br/>El atributo **uap10: Parameters** especifica los parámetros que se pasan a la función de punto de entrada del ejecutable del host. Dado que el host necesita saber qué hacer con estos parámetros, hay un contrato implícito entre el host y la aplicación hospedada. |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>Registrar un paquete de aplicación hospedada sin firmar en tiempo de ejecución

Una ventaja de la extensión **uap10: HostRuntime** es que permite a un host generar dinámicamente un paquete de aplicación hospedada en tiempo de ejecución y registrarlo mediante la API de [**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) , sin necesidad de firmarlo. Esto permite a un host generar dinámicamente el contenido y el manifiesto para el paquete de la aplicación hospedada y, a continuación, registrarlo.

Use los métodos siguientes de la clase [**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) para registrar un paquete de aplicación hospedada sin firmar. Estos métodos están disponibles a partir de Windows 10, versión 2004.

* **AddPackageByUriAsync**: registra un paquete MSIX sin firmar mediante la propiedad **AllowUnsigned** del parámetro *Options* .
* **RegisterPackageByUriAsync**: realiza un registro de archivo de manifiesto del paquete suelto. Si el paquete está firmado, la carpeta que contiene el manifiesto debe incluir un [archivo. p7x](https://docs.microsoft.com/windows/msix/overview#inside-an-msix-package) y un catálogo. Si es sin signo, se debe establecer la propiedad **AllowUnsigned** del parámetro *Options* .

### <a name="requirements-for-unsigned-hosted-apps"></a>Requisitos para las aplicaciones hospedadas sin firmar

* Los elementos de la [**aplicación**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) o la [**extensión**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) del manifiesto del paquete no pueden contener datos de activación, como los atributos **ejecutable**, **EntryPoint**o **trustLevel** . En su lugar, estos elementos solo pueden contener un atributo **uap10: HostId** que exprese la dependencia en el host y un atributo **Uap10: Parameters** .
* El paquete debe ser un paquete principal. No puede ser una agrupación, paquete de marco, recurso o paquete opcional.

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>Requisitos para un host que instala y registra un paquete de aplicación hospedada sin firmar

* El host debe tener la [identidad del paquete](#define-a-host).
* El host debe tener la **packageManagement** [funcionalidad restringida](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)packageManagement.
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](https://docs.microsoft.com/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>Muestra

Para una aplicación de ejemplo totalmente funcional que se declare como un host y, a continuación, registre dinámicamente un paquete de aplicación hospedada en tiempo de ejecución, consulte el [ejemplo de aplicación hospedada](https://github.com/microsoft/AppModelSamples/tree/master/Samples/HostedApps).

### <a name="the-host"></a>El host

El host se denomina **PyScriptEngine**. Se trata de un contenedor escrito en C# que ejecuta scripts de Python. Cuando se ejecuta con `-Register` el parámetro, el motor de scripts instala una aplicación hospedada que contiene un script de Python. Cuando un usuario intenta iniciar la aplicación hospedada recién instalada, el host se inicia y ejecuta el script de Python de **NumberGuesser** .

El manifiesto del paquete para la aplicación host (el archivo package. appxmanifest de la carpeta PyScriptEnginePackage) contiene una extensión **uap10: HostRuntime** que declara la aplicación como un host con el identificador **PythonHost** y el archivo ejecutable **PyScriptEngine. exe**.  

> [!NOTE]
> En este ejemplo, el manifiesto del paquete se denomina package. appxmanifest y forma parte de un [proyecto](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)de paquete de aplicación de Windows. Cuando se compila este proyecto, se [genera un manifiesto denominado AppxManifest. XML](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) y se compila el paquete MSIX para la aplicación host.

### <a name="the-hosted-app"></a>La aplicación hospedada

La aplicación hospedada consta de un script de Python y artefactos de paquete, como el manifiesto del paquete. No contiene ningún archivo PE.

El manifiesto del paquete para la aplicación hospedada (el archivo NumberGuesser/AppxManifest. xml) contiene los siguientes elementos:

* El atributo **Publisher** del elemento [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) contiene el `OID.2.25.311729368913984317654407730594956997722=1` identificador, que es necesario para un paquete sin firmar.
* El atributo **uap10: HostId** del elemento [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) identifica **PythonHost** como su host.

### <a name="run-the-sample"></a>Ejecución del ejemplo

El ejemplo requiere la versión 10.0.19041.0 o posterior de Windows 10 y el Windows SDK.

1. Descargue el [ejemplo](https://aka.ms/hostedappsample) en una carpeta en el equipo de desarrollo.
2. Abra la solución PyScriptEngine. sln en Visual Studio y establezca el proyecto **PyScriptEnginePackage** como proyecto de inicio.
3. Compile el proyecto **PyScriptEnginePackage** .
4. En Explorador de soluciones, haga clic con el botón derecho en el proyecto **PyScriptEnginePackage** y elija **implementar**.
5. Abra una ventana del símbolo del sistema en el directorio donde copió los archivos de ejemplo y ejecute el siguiente comando para registrar la aplicación de **NumberGuesser** de ejemplo (la aplicación hospedada). Cambie `D:\repos\HostedApps` a la ruta de acceso donde copió los archivos de ejemplo.

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > Puede ejecutar `pyscriptengine` en la línea de comandos porque el host del ejemplo declara un [**AppExecutionAlias**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias).

6. Abra el menú **Inicio** y haga clic en **NumberGuesser** para ejecutar la aplicación hospedada.
