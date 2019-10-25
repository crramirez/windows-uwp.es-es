---
title: Componentes de Windows Runtime asincrónicos para una aplicación para UWP cargada en paralelo
description: En este documento se describe una característica orientada a la empresa compatible con Windows 10, que permite a las aplicaciones de .NET táctiles usar el código existente responsable de las operaciones clave críticas para la empresa.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690348"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Componentes de Windows Runtime asincrónicos para una aplicación para UWP cargada en paralelo

En este artículo se describe una característica de destino empresarial compatible con Windows 10, que permite que las aplicaciones de .NET táctiles usen el código existente responsable de las operaciones fundamentales para la empresa.

## <a name="introduction"></a>Introducción

>**Tenga en cuenta**  se puede descargar el código de ejemplo que acompaña a este documento para [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample). La plantilla de Microsoft Visual Studio para compilar componentes de Windows Runtime intermedias puede descargarse aquí: [plantilla de Visual Studio 2015 que tiene como destino aplicaciones universales de Windows para Windows 10](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents) .

Windows incluye una nueva característica denominada *componentes de Windows Runtime negociados para aplicaciones de carga lateral*. Usamos el término IPC (comunicación entre procesos) para describir la capacidad de ejecutar recursos de software de escritorio existentes en un proceso (componente de escritorio) mientras interactúa con este código en una aplicación de UWP. Este es un modelo conocido para los desarrolladores empresariales, ya que las aplicaciones de base de datos y las aplicaciones que usan servicios NT en Windows comparten una arquitectura de varios procesos similar.

La instalación de prueba de la aplicación es un componente fundamental de esta característica.
Las aplicaciones específicas de la empresa no tienen lugar en el Microsoft Store del consumidor general y las organizaciones tienen requisitos muy específicos sobre seguridad, privacidad, distribución, instalación y servicio. Como tal, el modelo de instalación de prueba es un requisito de los que usarían esta característica y un detalle de implementación crítico.

Las aplicaciones centradas en datos son un objetivo clave para esta arquitectura de aplicación. Está aprovisionado que las reglas de negocios existentes ensconced, por ejemplo, en SQL Server, serán una parte común del componente de escritorio. En realidad, no es el único tipo de funcionalidad que puede ofrecidos el componente de escritorio, pero una gran parte de la demanda de esta característica está relacionada con los datos y la lógica de negocios existentes.

Por último, dada la abrumadora penetración del entorno de tiempo de ejecución de .NET y el lenguaje de C\# en el desarrollo empresarial, esta característica se desarrolló con hincapié en el uso de .NET para la aplicación UWP y los lados del componente de escritorio. Aunque hay otros lenguajes y tiempos de ejecución posibles para la aplicación UWP, el ejemplo adjunto solo muestra C\#y está restringido al tiempo de ejecución de .NET exclusivamente.

## <a name="application-components"></a>Componentes de la aplicación

>**Tenga en cuenta**  esta característica es exclusivamente para el uso de .net. Tanto la aplicación cliente como el componente de escritorio deben crearse mediante .NET.

**Modelo de aplicación**

Esta característica se crea en torno a la arquitectura de aplicación general que se conoce como MVVM (modelo vista-modelo). Como tal, se supone que el "modelo" se hospeda completamente en el componente de escritorio. Por lo tanto, debería ser obvio de inmediato que el componente de escritorio será "sin periféricos" (es decir, no contenga ninguna interfaz de usuario). La vista estará totalmente incluida en la aplicación empresarial de carga lateral. Aunque no es necesario que esta aplicación se cree con la construcción "View-Model", se prevé que el uso de este patrón será común.

**Componente de escritorio**

El componente de escritorio de esta característica es un nuevo tipo de aplicación que se introduce como parte de esta característica. Este componente de escritorio solo se puede escribir en C\# y debe tener como destino .NET 4,6 o superior para Windows 10. El tipo de proyecto es un híbrido entre la UWP de destino de CLR, ya que el formato de comunicación entre procesos incluye tipos y clases de UWP, mientras que el componente de escritorio puede llamar a todas las partes de la biblioteca de clases en tiempo de ejecución de .NET. El impacto en el proyecto de Visual Studio se describe en detalle más adelante. Esta configuración híbrida permite calcular las referencias de los tipos UWP entre la aplicación compilada en los componentes de escritorio, a la vez que se permite llamar al código CLR de escritorio dentro de la implementación del componente de escritorio.

**DataContract**

El contrato entre la aplicación de carga lateral y el componente de escritorio se describe en términos del sistema de tipos de UWP. Esto implica declarar una o varias clases de C\# que pueden representar una UWP. Vea el tema de MSDN [crear Windows Runtime componentes en C\# y Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) para obtener un requisito específico de la creación de Windows Runtime clase con\#de c.

>**Tenga en cuenta**  las enumeraciones no se admiten en el contrato de componentes de Windows Runtime entre el componente de escritorio y la aplicación de carga en este momento.

**Aplicación de carga lateral**

La aplicación de carga en paralelo es una aplicación de UWP normal en todos los aspectos excepto en una: se carga en paralelo en lugar de instalarse a través del Microsoft Store. La mayoría de los mecanismos de instalación son idénticos: el manifiesto y el empaquetado de la aplicación son similares (una adición al manifiesto se describe en detalle más adelante). Una vez habilitada la instalación de prueba, un sencillo script de PowerShell puede instalar los certificados necesarios y la propia aplicación. Es el procedimiento recomendado normal que la aplicación de carga de prueba pasa la prueba de certificación WACK que se incluye en el menú proyecto o almacén de Visual Studio.

>**Nota:** La instalación en paralelo se puede activar en configuración&gt; actualización & seguridad&gt; para los desarrolladores.

Un punto importante a tener en cuenta es que el mecanismo de agente de aplicación incluido como parte de Windows 10 es solo de 32 bits. El componente de escritorio debe ser de 32 bits.
Las aplicaciones de carga lateral pueden ser de 64 bits (siempre y cuando se registren proxies de 64 bits y 32 bits), pero esto no será atípico. La creación de la aplicación de instalación de prueba en C\# mediante la configuración normal "neutra" y el valor predeterminado "Prefer 32-bit" crea de forma natural aplicaciones de 32 bits cargadas en paralelo.

**Creación de instancias de servidor y AppDomains**

Cada aplicación de carga cara recibe su propia instancia de un servidor de agente de aplicación (denominado "múltiples instancias"). El código de servidor se ejecuta dentro de un solo AppDomain. Esto permite tener varias versiones de las bibliotecas que se ejecutan en instancias independientes. Por ejemplo, la aplicación A necesita V 1.1 de un componente y la aplicación B necesita V2. Estos se separan de forma limpia con los componentes V 1.1 y V2 en directorios de servidor independientes y apuntan a la aplicación a cualquier servidor que admita la versión correcta que se desea.

La implementación del código de servidor se puede compartir entre varias instancias del servidor de agente de aplicación seleccionando varias aplicaciones en el mismo directorio del servidor. Todavía habrá varias instancias del servidor de agente de aplicaciones, pero ejecutarán un código idéntico. Todos los componentes de implementación que se usan en una sola aplicación deben estar presentes en la misma ruta de acceso.

## <a name="defining-the-contract"></a>Definir el contrato

El primer paso para crear una aplicación con esta característica es crear el contrato entre la aplicación de prueba y el componente de escritorio. Esto debe realizarse exclusivamente con tipos de Windows Runtime.
Afortunadamente, son fáciles de declarar con clases de C\#. Sin embargo, hay consideraciones importantes sobre el rendimiento al definir estas conversaciones, que se trata en una sección posterior.

La secuencia para definir el contrato se presenta de la siguiente manera:

**Paso 1:** Cree una nueva biblioteca de clases en Visual Studio. Asegúrese de crear el proyecto mediante la plantilla de **biblioteca de clases** y no la **Windows Runtime** plantilla de componentes.

Una implementación obviamente sigue, pero esta sección solo cubre la definición del contrato entre procesos. El ejemplo que lo acompaña incluye la siguiente clase (EnterpriseServer.cs), cuya forma inicial tiene este aspecto:

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

Esto define una clase "EnterpriseServer" de la que se pueden crear instancias desde la aplicación de carga lateral. Esta clase proporciona la funcionalidad prometida en RuntimeClass. RuntimeClass se puede usar para generar el archivo winmd de referencia que se incluirá en la aplicación de carga.

**Paso 2:** Edite el archivo de proyecto manualmente para cambiar el tipo de salida de proyecto a **Windows Runtime componente**.

Para hacer esto en Visual Studio, haga clic con el botón derecho en el proyecto recién creado y seleccione "descargar proyecto", luego haga clic con el botón derecho de nuevo y seleccione "editar EnterpriseServer. csproj" para abrir el archivo de proyecto, un archivo XML, para su edición.

En el archivo abierto, busque la etiqueta \<OutputType\> y cambie su valor a "winmdobj".

**Paso 3:** Cree una regla de compilación que cree un archivo de metadatos de Windows de "referencia" (archivo. winmd). es decir, no tiene ninguna implementación.

**Paso 4:** Cree una regla de compilación que cree un archivo de metadatos de Windows de "implementación", es decir, que tenga la misma información de metadatos, pero también incluye la implementación.

Esto se realiza mediante los siguientes scripts. Agregue los scripts a la línea de comandos del evento posterior a la compilación, en **las propiedades** del proyecto > **eventos de compilación**.

> **Tenga en cuenta** que el script es diferente en función de la versión de Windows de destino (Windows 10) y la versión de Visual Studio en uso.

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

Una vez que se crea el archivo **winmd** de referencia (en la carpeta "referencia" en la carpeta de destino del proyecto), se lleva a cabo la entrega (copia) a cada proyecto de aplicación de carga en paralelo y al que se hace referencia. Esto se describe con más detalle en la sección siguiente. La estructura del proyecto que se incorpora en las reglas de compilación anteriores garantiza que la implementación y el **winmd** de referencia se encuentran en directorios claramente segregados en la jerarquía de compilación para evitar confusiones.

## <a name="side-loaded-applications-in-detail"></a>Aplicaciones de carga en detalle
Como se indicó anteriormente, la aplicación de carga de prueba se crea como cualquier otra aplicación de UWP, pero hay un detalle adicional: declarar la disponibilidad de RuntimeClass (es) en el manifiesto de la aplicación de carga lateral. Esto permite que la aplicación simplemente escriba nuevo para tener acceso a la funcionalidad del componente de escritorio. Una nueva entrada de manifiesto en la sección <Extension> describe el RuntimeClass implementado en el componente Desktop e información sobre dónde se encuentra. El contenido de la declaración en el manifiesto de la aplicación es el mismo para las aplicaciones que tienen como destino Windows 10. Por ejemplo:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

La categoría es inProcessServer porque hay varias entradas en la categoría outOfProcessServer que no son aplicables a esta configuración de aplicación. Tenga en cuenta que el componente de <Path> debe contener siempre clrhost. dll (pero esto no se aplica y especificar un valor diferente producirá un error de manera **no** definida).

La sección <ActivatableClass> es igual que una verdadera RuntimeClass en proceso preferida por un componente de Windows Runtime en el paquete de la aplicación. <ActivatableClassAttribute> es un nuevo elemento y los atributos name = "DesktopApplicationPath" y Type = "String" son obligatorios e invariables. El atributo de valor apunta a la ubicación en la que reside el archivo winmd de implementación del componente de escritorio (más información sobre esto en la sección siguiente). Cada RuntimeClass preferida por el componente de escritorio debe tener su propio árbol de elementos <ActivatableClass>. El ActivatableClassId debe coincidir con el nombre completo del espacio de nombres de RuntimeClass.

Como se mencionó en la sección "definir el contrato", se debe realizar una referencia de proyecto al archivo winmd de referencia del componente de escritorio. Normalmente, el sistema de proyectos de Visual Studio crea una estructura de directorios de dos niveles con el mismo nombre. En el ejemplo, es EnterpriseIPCApplication\\EnterpriseIPCApplication. El archivo **winmd** de referencia se copia manualmente en este directorio de segundo nivel y, a continuación, se usa el cuadro de diálogo referencias del proyecto (haga clic en el botón **examinar.** ) para buscar y hacer referencia a este archivo **winmd**. Después, el espacio de nombres de nivel superior del componente de escritorio (por ejemplo, Fabrikam) debe aparecer como un nodo de nivel superior en la parte referencias del proyecto.

>**Nota:** Es muy importante usar el archivo **winmd de referencia** en la aplicación de prueba. Si realiza accidentalmente el archivo **winmd de implementación** en el directorio de la aplicación de carga lateral y hace referencia a él, es probable que reciba un error relacionado con "no se encuentra IStringable". Se trata de una firma de que se ha hecho referencia a un archivo **winmd** equivocado. Las reglas posteriores a la compilación de la aplicación del servidor IPC (que se detallan en la siguiente sección) segregan cuidadosamente estos dos **winmd** en directorios independientes.

Variables de entorno (especialmente% ProgramFiles%) se puede usar en <ActivatableClassAttribute Value="path">. Como se indicó anteriormente, el agente de aplicación solo admite 32 bits, por lo que% ProgramFiles% se resolverá en C:\\archivos de programa (x86) si la aplicación se ejecuta en un sistema operativo de 64 bits.

## <a name="desktop-ipc-server-detail"></a>Detalle del servidor IPC de escritorio

En las dos secciones anteriores se describe la declaración de la clase y la mecánica de transportar el archivo **winmd** de referencia al proyecto de aplicación de carga lateral. La mayor parte del trabajo restante en el componente de escritorio implica la implementación de. Dado que el punto entero del componente de escritorio es poder llamar al código de escritorio (normalmente para volver a usar los recursos de código existentes), el proyecto se debe configurar de una manera especial.
Normalmente, un proyecto de Visual Studio que usa .NET usa uno de los dos "perfiles".
Uno es para escritorio (". NetFramework ") y otro está destinado a la parte de la aplicación de UWP de CLR (". NetCore "). Un componente de escritorio de esta característica es un híbrido entre estos dos. Como resultado, la sección de referencias se construye con sumo cuidado para mezclar estos dos perfiles.

Un proyecto de aplicación de UWP normal no contiene referencias de proyecto explícitas, ya que la totalidad de la superficie de API de Windows Runtime se incluye implícitamente.
Normalmente solo se realizan otras referencias entre proyectos. Sin embargo, un proyecto de componente de escritorio tiene un conjunto de referencias muy especial. Inicia la vida como un proyecto de "biblioteca de clases de escritorio clásico\\" y, por lo tanto, es un proyecto de escritorio. Por tanto, es necesario realizar referencias explícitas a la API de Windows Runtime (a través de referencias a archivos **winmd** ). Agregue las referencias apropiadas como se muestra a continuación.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

Las referencias anteriores son una combinación cuidadosa de eferences que son fundamentales para el funcionamiento correcto de este servidor híbrido. El protocolo es abrir el archivo. csproj (tal y como se describe en cómo editar el proyecto OutputType) y agregar estas referencias según sea necesario.

Una vez que las referencias están configuradas correctamente, la siguiente tarea consiste en implementar la funcionalidad del servidor. Vea el tema [prácticas recomendadas para la interoperabilidad con componentes de Windows Runtime (aplicaciones paraC++ UWP con C\#/VB/y XAML)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10)).
La tarea consiste en crear una DLL de componentes de Windows Runtime que pueda llamar al código de escritorio como parte de su implementación. El ejemplo que lo acompaña incluye los patrones principales utilizados en Windows Runtime:

-   Llamadas a métodos

-   Windows Runtime orígenes de eventos por parte del componente de escritorio

-   Windows Runtime operaciones asincrónicas

-   Devolver matrices de tipos básicos

**Instalación**

Para instalar la aplicación, copie el archivo **winmd** de implementación en el directorio correcto especificado en el manifiesto de la aplicación de carga lateral asociado: valor del <ActivatableClassAttribute>= "ruta de acceso". Copie también los archivos de compatibilidad asociados y el archivo dll de proxy/código auxiliar (este último detalle se trata a continuación). Si no se puede copiar el archivo **winmd** de implementación en la ubicación del directorio del servidor, todas las llamadas de la aplicación que se cargan en paralelo a las nuevas en el RuntimeClass producirán un error "clase no registrada". Si no se instala el proxy o el código auxiliar (o si no se registra), se producirá un error en todas las llamadas sin valores devueltos. Este último error **no** suele estar asociado a excepciones visibles.
Si se observan excepciones debido a este error de configuración, pueden hacer referencia a "conversión no válida".

**Consideraciones sobre la implementación del servidor**

El servidor de Windows Runtime de escritorio se puede considerar como "trabajo" o "tarea" en función de. Cada llamada al servidor funciona en un subproceso que no es de interfaz de usuario y todo el código debe ser compatible con varios subprocesos y ser seguro. La parte de la aplicación de carga en paralelo que llama a la funcionalidad del servidor también es importante. Es fundamental evitar siempre llamar a código de ejecución prolongada desde cualquier subproceso de la interfaz de usuario en la aplicación de carga. Hay dos formas principales de hacerlo:

1.  Si se llama a la funcionalidad de servidor desde un subproceso de interfaz de usuario, use siempre un patrón Async en el área de la superficie pública del servidor y la implementación.

2.  Llame a la funcionalidad del servidor desde un subproceso en segundo plano en la aplicación de prueba.

**Windows Runtime Async en el servidor**

Dada la naturaleza entre procesos del modelo de aplicación, las llamadas al servidor tienen más sobrecarga que el código que se ejecuta exclusivamente en proceso. Normalmente, es seguro llamar a una propiedad simple que devuelve un valor en memoria, ya que se ejecutará lo suficientemente rápido como para que el bloqueo del subproceso de la interfaz de usuario no sea un problema. Sin embargo, cualquier llamada que implique e/s de cualquier orden (esto incluye todo el control de archivos y las recuperaciones de bases de datos) podría bloquear el subproceso de la interfaz de usuario de llamada y hacer que la aplicación se termine debido a la falta de respuesta. Además, las llamadas de propiedad en objetos no se recomiendan en esta arquitectura de aplicación por motivos de rendimiento.
Esto se describe con más detalle en la sección siguiente.

Un servidor implementado correctamente implementará normalmente llamadas realizadas directamente desde subprocesos de interfaz de usuario a través del patrón Async de Windows Runtime. Esto se puede implementar siguiendo este patrón. En primer lugar, la declaración (de nuevo, en el ejemplo que lo acompaña):

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Esto declara una Windows Runtime operación asincrónica que devuelve un entero.
Normalmente, la implementación de la operación asincrónica adopta la forma:

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Nota:** Es habitual esperar otras operaciones de ejecución potencialmente prolongada mientras se escribe la implementación. En ese caso, debe declararse el código de la **tarea. Run** :

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Los clientes de este método asincrónico pueden esperar esta operación como cualquier otra Windows Runtime operación aysnc.

**Llamar a la funcionalidad del servidor desde un subproceso en segundo plano de la aplicación**

Dado que es habitual que el cliente y el servidor se escriban en la misma organización, se puede adoptar una práctica de programación en la que se realizarán todas las llamadas al servidor mediante un subproceso en segundo plano en la aplicación de carga. Una llamada directa que recopila uno o más lotes de datos del servidor se puede realizar desde un subproceso en segundo plano. Cuando los resultados se recuperan por completo, el lote de datos que se encuentra en memoria en el proceso de la aplicación normalmente se puede recuperar directamente desde el subproceso de la interfaz de usuario. Los objetos de C @ no__t_0_ son ágiles entre subprocesos en segundo plano y subprocesos de interfaz de usuario, por lo que son especialmente útiles para este tipo de patrón de llamada.\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Creación e implementación del proxy de Windows Runtime

Dado que el enfoque IPC implica el cálculo de referencias de Windows Runtime interfaces entre dos procesos, se debe usar un proxy Windows Runtime y un código auxiliar registrados globalmente.

**Crear el proxy en Visual Studio**

El proceso de creación y registro de servidores proxy y códigos auxiliares para su uso dentro de un paquete de aplicación de UWP normal se describe en el tema [generar eventos en Windows Runtime componentes](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140)).
Los pasos descritos en este artículo son más complicados que el proceso que se describe a continuación, ya que implica registrar el proxy o el código auxiliar dentro del paquete de aplicación (en lugar de registrarlo globalmente).

**Paso 1:** Mediante la solución para el proyecto de componente de escritorio, cree un proyecto de proxy o código auxiliar en Visual Studio:

**Solución > Agregar > Proyecto > Visual C++ > consola Win32 seleccione la opción dll.**

En los pasos siguientes, se supone que el componente de servidor se denomina **MyWinRTComponent**.

**Paso 3:** Elimine todos los archivos CPP/H del proyecto.

**Paso 4:** La sección anterior "definir el contrato" contiene un comando posterior a la compilación que ejecuta **winmdidl. exe**, **MIDL. exe**, **mdmerge. exe**, etc. Una de las salidas del paso MIDL de este comando posterior a la compilación genera cuatro salidas importantes:

a) dlldata. c

b) un archivo de encabezado (por ejemplo, MyWinRTComponent. h)

c) \*\_archivo i. c (por ejemplo, MyWinRTComponent\_i. c)

d) \*\_archivo p. c (por ejemplo, MyWinRTComponent\_p. c)

**Paso 5:** Agregue estos cuatro archivos generados al proyecto "MyWinRTProxy".

**Paso 6:** Agregue un archivo Def al proyecto "MyWinRTProxy" **(proyecto > agregar nuevo elemento > código > archivo de definición de módulo**) y actualice el contenido para que sea:

Biblioteca MyWinRTComponent. Proxies. dll

Port

DllCanUnloadNow privado

DllGetClassObject privado

DllRegisterServer privado

DllUnregisterServer privado

**Paso 7:** Abra las propiedades del proyecto "MyWinRTProxy":

**> Propiedades de Comfiguration general > nombre de destino:**

MyWinRTComponent. Proxies

**Definiciones delC++ preprocesador de C/> > Agregar**

32\_WINDOWS; REGISTRAR\_DLL de\_del PROXY "

**C/C++ > encabezado precompilado: seleccione "no usar encabezado precompilado"**

**Vinculador > General > omitir biblioteca de importación: seleccione "sí".**

**Entrada de > del vinculador > dependencias adicionales: agregar rpcrt4. lib; runtimeobject. lib**

**Enlazador > metadatos de Windows > generar metadatos de Windows: seleccione "no".**

**Paso 8:** Compile el proyecto "MyWinRTProxy".

**Implementación del proxy**

El proxy debe estar registrado globalmente. La manera más sencilla de hacerlo es hacer que el proceso de instalación llame a DllRegisterServer en el archivo DLL del proxy. Tenga en cuenta que, puesto que la característica solo admite servidores creados para x86 (es decir, no compatibilidad con 64 bits), la configuración más sencilla consiste en usar un servidor de 32 bits, un proxy de 32 bits y una aplicación de 32 bits. Normalmente, el proxy se encuentra junto a la implementación de **winmd** para el componente de escritorio.

Se debe realizar un paso de configuración adicional. Para que el proceso de carga en paralelo cargue y ejecute el proxy, el directorio debe estar marcado como "Read/Execute" para ALL_APPLICATION_PACKAGES. Esto se hace a través de la herramienta de línea de comandos **icacls. exe** . Este comando debe ejecutarse en el directorio donde residen los archivos **winmd** de implementación y proxy/stub:

*icacls. /T/Grant \*S-1-15-2-1: RX*

## <a name="patterns-and-performance"></a>Patrones y rendimiento

Es muy importante que el rendimiento del transporte entre procesos se supervise cuidadosamente. Una llamada entre procesos es al menos dos veces más costosa que una llamada en proceso. Crear conversaciones de "chat" entre procesos o realizar transferencias repetidas de objetos grandes, como imágenes de mapa de bits, puede provocar un rendimiento inesperado y no deseable de la aplicación.

Esta es una lista no exhaustiva de aspectos que se deben tener en cuenta:

-   Siempre se deben evitar las llamadas de métodos sincrónicos del subproceso de interfaz de usuario de la aplicación al servidor. Llame al método desde un subproceso en segundo plano de la aplicación y, a continuación, use CoreWindowDispatcher para obtener los resultados en el subproceso de la interfaz de usuario si es necesario.

-   Llamar a operaciones asincrónicas desde un subproceso de interfaz de usuario de aplicación es seguro, pero tenga en cuenta los problemas de rendimiento que se describen a continuación.

-   La transferencia masiva de resultados reduce la charla entre procesos. Normalmente, esto se realiza mediante la construcción de matriz Windows Runtime.

-   Devolver una *lista<T>* donde *t* es un objeto de una operación asincrónica o de una captura de propiedad, provocará una gran cantidad de conversaciones entre procesos. Por ejemplo, suponga que devuelve una*lista&lt;* objetos de&gt;de personas. Cada paso de la iteración será una llamada entre procesos. Cada objeto *People* devuelto se representa mediante un proxy y cada llamada a un método o propiedad en ese objeto individual producirá una llamada entre procesos. Por lo tanto, una lista "inocente" *&lt;&gt;* objeto donde el *recuento* es grande producirá un gran número de llamadas lentas. Mejores resultados de rendimiento de la transferencia masiva de Structs del contenido de una matriz. Por ejemplo:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

A continuación, devuelva * PersonStruct\[\]* en lugar de la *lista&lt;PersonObject&gt;* .
Esto obtiene todos los datos en un "salto" entre procesos

Como con todas las consideraciones de rendimiento, la medición y las pruebas son críticas. Idealmente, la telemetría se debe insertar en las distintas operaciones para determinar cuánto tiempo tardan. Es importante medir a lo largo de un intervalo: por ejemplo, ¿cuánto tiempo se tarda en consumir todos los objetos de *personas* para una consulta determinada en la aplicación de carga lateral?

Otra técnica es la prueba de carga variable. Esto se puede hacer colocando enlaces de prueba de rendimiento en la aplicación que introducen cargas de retraso variables en el procesamiento del servidor. Esto puede simular distintos tipos de carga y la reacción de la aplicación para el rendimiento del servidor variable.
En el ejemplo se muestra cómo colocar retrasos de tiempo en el código mediante técnicas asincrónicas adecuadas. La cantidad exacta de retraso que se va a insertar y el intervalo de selección aleatoria que se van a colocar en esa carga artificial variarán según el diseño de la aplicación y el entorno previsto en el que se ejecutará la aplicación.

## <a name="development-process"></a>Proceso de desarrollo

Cuando realice cambios en el servidor, es necesario asegurarse de que las instancias que se estén ejecutando previamente ya no se estén ejecutando. COM eliminará finalmente el proceso, pero el temporizador de detención tarda más tiempo del que es eficaz para el desarrollo iterativo. Por lo tanto, el sacrificio de una instancia de ejecución anterior es un paso normal durante el desarrollo. Esto requiere que el desarrollador realice un seguimiento de qué instancia de Dllhost hospeda el servidor.

El proceso de servidor puede encontrarse y eliminarse mediante el administrador de tareas u otras aplicaciones de terceros. La herramienta de línea de comandos * * TaskList. exe * * también está incluida y tiene una sintaxis flexible, por ejemplo:

  
 | **Comando** | **Acción** |
 | ------------| ---------- |
 | TaskList | Enumera todos los procesos en ejecución en orden aproximado de tiempo de creación, con los procesos creados más recientemente cerca de la parte inferior. |
 | TaskList/FI "IMAGENAME EQ Dllhost. exe"/M | Muestra información sobre todas las instancias de dllhost. exe. El modificador/M muestra los módulos que se han cargado. |
 | TaskList/FI "PID EQ 12564"/M | Puede utilizar esta opción para consultar el archivo Dllhost. exe si conoce su PID. |

La lista de módulos de un servidor de agente debe mostrar *clrhost. dll* en la lista de módulos cargados.

## <a name="resources"></a>Recursos

-   [Plantillas de proyecto de componentes de WinRT negociados para Windows 10 y VS 2015](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [NorthwindRT ejemplo de componente WinRT asincrónico](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Entrega de aplicaciones Microsoft Store confiables y confiables](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [Contratos y extensiones de aplicaciones (aplicaciones de la tienda Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Cómo transferir localmente aplicaciones en Windows 10](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [Implementación de aplicaciones para UWP en empresas](https://go.microsoft.com/fwlink/p/?LinkID=264770)

