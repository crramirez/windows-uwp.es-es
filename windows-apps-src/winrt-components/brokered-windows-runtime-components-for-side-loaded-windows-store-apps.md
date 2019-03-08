---
title: Componentes negociados de Windows Runtime para una aplicación de prueba para UWP
description: Este documento describe una característica empresarial de destino compatible con Windows 10, que permite que las aplicaciones de .NET táctiles usar el código existente responsable de las operaciones empresariales clave.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: 9ebac70d56fcdf1bf717d763daf4ac1bd9c06d4b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640090"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Componentes negociados de Windows Runtime para una aplicación de prueba para UWP

Este artículo describe una característica empresarial de destino compatible con Windows 10, que permite que las aplicaciones de .NET táctiles usar el código existente responsable de las operaciones empresariales clave.

## <a name="introduction"></a>Introducción

>**Tenga en cuenta**  el código de ejemplo que acompaña a este documento se puede descargar para [Visual Studio 2015 y 2017](https://aka.ms/brokeredsample). La plantilla de Microsoft Visual Studio para compilar asíncrona de componentes de Windows en tiempo de ejecución puede descargarse aquí: [Plantilla de Visual Studio 2015 como destino las aplicaciones de Windows Universal para Windows 10](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

Windows incluyen una nueva característica denominada *asíncrona de componentes de Windows en tiempo de ejecución para las aplicaciones de carga lateral*. Usamos el término IPC (comunicación entre procesos) para describir la capacidad de ejecutar activos de software de escritorio existentes en un proceso (componente de escritorio) mientras se interactúa con este código en una aplicación para UWP. Este es un modelo que resultará familiar para los desarrolladores empresariales porque las aplicaciones de base de datos y las aplicaciones que usan servicios NT en Windows comparten una arquitectura multiproceso similar.

La instalación de prueba de la aplicación es un componente fundamental de esta característica.
Las aplicaciones específicas de empresa no tienen cabida en la Microsoft Store para el público general y las corporaciones tienen requisitos muy específicos de seguridad, privacidad, distribución, instalación y servicio. Por lo tanto, el modelo de prueba es tanto un requisito de aquellos que usen esta característica como un aspecto fundamental de la implementación.

Las aplicaciones centradas en datos son un objetivo clave de esta arquitectura de aplicación. Se sabe que las reglas empresariales existentes que encontramos, por ejemplo, en SQL Server serán una parte común del componente de escritorio. Por supuesto, este no es el único tipo de funcionalidad que el componente de escritorio propone, pero una gran parte de la demanda de esta característica está relacionada con los datos y la lógica empresarial existentes.

Por último, dada la presencia de una tarea abrumadora de runtime de .NET y C\# lenguaje en el desarrollo de la empresa, esta característica se desarrolló con un énfasis en el uso de .NET para la aplicación para UWP y los lados del componente de escritorio. Aunque hay otros lenguajes y tiempos de ejecución posibles para la aplicación para UWP, que lo acompaña solo ilustra C\#y está restringido exclusivamente al tiempo de ejecución. NET.

## <a name="application-components"></a>Componentes de aplicación

>**Tenga en cuenta**  esta característica es exclusivamente para el uso de. NET. La aplicación cliente y el componente de escritorio deben estar creados con .NET.

**Modelo de aplicación**

Esta característica se creó sobre la arquitectura de aplicación general conocida como MVVM (Model View View-Model). Como tal, se da por hecho que el "modelo" se aloja por completo en el componente de escritorio. Por lo tanto, queda inmediatamente claro que el componente de escritorio será "desatendido" (es decir, no contiene ninguna interfaz de usuario). La vista estará contenida totalmente en la aplicación empresarial de prueba. Aunque no es un requisito que esta aplicación se cree con la construcción "modelo de vista", será común el uso de este patrón.

**Componente de escritorio**

El componente de escritorio es un nuevo tipo de aplicación que se incorpora como parte de esta característica. Solo se puede escribir este componente de escritorio en C\# y debe tener como destino .NET 4.6 o posterior para Windows 10. El tipo de proyecto es un híbrido entre CLR destinado a UWP, ya que el formato de comunicación entre procesos incluye tipos UWP y clases, mientras que el componente de escritorio puede llamar a todas las partes de la biblioteca de clases de .NET en tiempo de ejecución. Más adelante se describirá con detalle el impacto sobre el proyecto de Visual Studio. Esta configuración híbrida permite calcular las referencias a los tipos de UWP entre la aplicación creada sobre los componentes de escritorio y, al mismo tiempo, llamar al código de CLR de escritorio dentro de la implementación del componente de escritorio.

**contrato**

El contrato entre la aplicación de prueba y el componente de escritorio se describe en términos del sistema de tipos de UWP. Esto implica declarar uno o más C\# clases que pueden representar una UWP. Vea el tema de MSDN [crear componentes de Windows en tiempo de ejecución de C\# y Visual Basic](https://msdn.microsoft.com/library/br230301.aspx) para un requisito concreto de la creación de la clase en tiempo de ejecución de Windows mediante C\#.

>**Tenga en cuenta**  las enumeraciones no se admiten en el contrato de los componentes de Windows en tiempo de ejecución entre el componente de escritorio y de carga lateral de la aplicación en este momento.

**Aplicación de carga lateral**

La aplicación de prueba es una aplicación para UWP normal en todos los aspectos excepto en uno: se instala mediante una aplicación de prueba en lugar de mediante la Microsoft Store. La mayoría de los mecanismos de instalación son idénticos: el manifiesto y el paquete de la aplicación son similares (más adelante se describe una incorporación al manifiesto). Cuando se habilita la instalación de prueba, un sencillo script de PowerShell puede instalar los certificados necesarios y la propia aplicación. El procedimiento recomendado es que la aplicación de prueba pase la prueba de certificación del WACK que se incluye en el menú Proyecto/Tienda de Visual Studio

>**Tenga en cuenta** -instalación de prueba puede activarse en configuración -&gt; actualización y seguridad -&gt; para desarrolladores.

Un aspecto importante a tener en cuenta es que el mecanismo Agente de aplicación que se incluye en Windows 10 es solo de 32 bits. El componente de escritorio debe ser de 32 bits.
Las aplicaciones de prueba pueden ser de 64 bits (siempre que haya registrados tanto proxies de 64 bits como de 32 bits), pero no será lo normal. Creación de la aplicación carga lateral en C\# utilizando la configuración de "neutral" normal y el valor predeterminado "preferencia de 32 bits" naturalmente crea aplicaciones de carga lateral de 32 bits.

**Creación de instancias de servidor y dominios de aplicación**

Cada aplicación de prueba recibe su propia instancia de un servidor de Agente de aplicación (denominado "multinstancias"). El código de servidor se ejecuta dentro de un único AppDomain. Esto permite tener varias versiones de bibliotecas ejecutándose en diferentes instancias. Por ejemplo, la aplicación A necesita la versión V1.1 de un componente y la aplicación B necesita la versión V2. Para separarlos limpiamente, los componentes V1.1 y V2 se guardan en directorios de servidor diferentes y la aplicación apunta al servidor que admita la versión correcta deseada.

Varias instancias del servidor de Agente de aplicación pueden compartir la implementación del código de servidor haciendo que varias aplicaciones apunten al mismo directorio de servidor. Seguirá habiendo varias instancias del servidor de Agente de aplicación, pero ejecutarán un código idéntico. Todos los componentes de la implementación que se usan en una sola aplicación deben existir en la misma ruta de acceso.

## <a name="defining-the-contract"></a>Definición del contrato

El primer paso para crear una aplicación usando esta característica es crear el contrato entre la aplicación de prueba y el componente de escritorio. Esto debe hacerse exclusivamente con tipos Windows Runtime.
Afortunadamente, estos son fáciles de declarar mediante C\# clases. Sin embargo, hay consideraciones de rendimiento importantes cuando se definen estas conversaciones (se trata en una sección posterior).

La secuencia para definir el contrato se presenta de este modo:

**Paso 1:** Crear una nueva biblioteca de clases en Visual Studio. Asegúrate de crear el proyecto con la plantilla Biblioteca de clase y no con la plantilla componente de Windows Runtime

Obviamente a continuación hay una implementación, pero esta sección solo abarca la definición del contrato entre procesos. La muestra correspondiente incluye la siguiente clase (EnterpriseServer.cs), cuya forma inicial tiene este aspecto:

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

Esto define una clase "EnterpriseServer" de la que se puede crear instancias desde la aplicación de prueba. Esta clase proporciona la funcionalidad prometida en la clase RuntimeClass. La clase RuntimeClass puede usarse para generar el archivo winmd de referencia que se incluirá en la aplicación de prueba.

**Paso 2:** Edite el archivo de proyecto manualmente para cambiar el tipo de salida del proyecto de componente de Windows en tiempo de ejecución

Para hacer esto en Visual Studio, haz clic con el botón derecho en el proyecto recién creado y selecciona "Descargar el proyecto", a continuación, haz clic otra vez con el botón derecho y selecciona "Editar EnterpriseServer.csproj" para abrir el archivo de proyecto, un archivo XML, para editarlo.

En el archivo abierto, busque el \<OutputType\> etiquetar y cambie su valor a "winmdobj".

**Paso 3:** Crear una regla de compilación que crea una "referencia" Windows metadatos (archivo .winmd). es decir, no tiene ninguna implementación.

**Paso 4:** Crear una regla de compilación que crea un archivo de metadatos de Windows "implementación", es decir, tiene la misma información de metadatos, pero también incluye la implementación.

Esto se realiza con los siguientes scripts. Agrega los scripts a la línea de comandos de evento posterior a la compilación, en Proyecto **Propiedades** > **Eventos de compilación**.

> **Nota** El script es diferente en función de la versión de Windows de destino (Windows 10) y la versión de Visual Studio en uso.

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

Una vez que la referencia **winmd** se crea (en la carpeta "referencia" bajo la carpeta de destino del proyecto), resulta mano pertenecientes a (copiar) para cada proyecto de aplicación de carga lateral consumo y al que hace referencia. Esto se describirá con más detalle en la siguiente sección. La estructura del proyecto incluida en las reglas de compilación anteriores Asegúrese de que la implementación y la referencia **winmd** están en directorios claramente separados en la jerarquía de compilación para evitar confusiones.

## <a name="side-loaded-applications-in-detail"></a>Aplicaciones de prueba en detalle
Como se indicó anteriormente, la aplicación de prueba se crea como cualquier otra aplicación para UWP, pero hay un detalle adicional: hay que declarar la disponibilidad de las clases RuntimeClass en el manifiesto de la aplicación de prueba. Esto permite a la aplicación simplemente escribir new para acceder a la funcionalidad del componente de escritorio. Un nuevo manifiesto en la sección <Extension> describe la clase RuntimeClass implementada en el componente de escritorio e incluye información sobre dónde está ubicada. Este contenido de la declaración en el manifiesto de la aplicación es el mismo para las aplicaciones destinadas a Windows 10. Por ejemplo:

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

La categoría es inProcessServer porque hay varias entradas en la categoría outOfProcessServer que no son aplicables a esta configuración de la aplicación. Tenga en cuenta que el <Path> componente siempre debe contener clrhost.dll (sin embargo esto es **no** aplica y especificando un valor diferente se producirá un error de maneras no definido).

La sección <ActivatableClass> es la misma que una RuntimeClass que está realmente dentro del proceso, preferida por un componente de Windows Runtime en el paquete de la aplicación. <ActivatableClassAttribute> es un nuevo elemento y los atributos nombre = "DesktopApplicationPath" y el tipo = "string" es obligatorios y todos los idiomas. El atributo Value apunta a la ubicación donde se encuentra el archivo winmd de implementación del componente de escritorio (encontrarás más información sobre esto en la siguiente sección). Cada clase RuntimeClass preferida por el componente de escritorio debe tener su propio árbol de elementos <ActivatableClass>. ActivatableClassId debe coincidir con el nombre completo en el espacio de nombres de la RuntimeClass.

Como se indicó en la sección "Definición del contrato", se debe crear una referencia de proyecto al archivo winmd de referencia del componente de escritorio. El sistema de proyectos de Visual Studio normalmente crea una estructura de directorios de dos niveles con el mismo nombre. En el ejemplo es EnterpriseIPCApplication\\EnterpriseIPCApplication. La referencia **winmd** se copian manualmente en este segundo directorio de nivel y, a continuación, se usa el cuadro de diálogo las referencias del proyecto (haga clic en el **Examinar... **  botón) para buscar y hacer referencia a esto **winmd**. Después, el espacio de nombres de nivel superior del componente de escritorio (por ejemplo, Fabrikam) debe aparecer como nodo de nivel superior en la parte Referencias del proyecto.

>**Tenga en cuenta** es muy importante usar el **referencia winmd** en la aplicación de carga lateral. Si accidentalmente transmitirán el **implementación winmd** al directorio de aplicación de carga lateral y referencia, es probable que recibirá un error relacionado con "no se encuentra IStringable". Esta es una sesión seguro que equivocados **winmd** se ha hecho referencia a. Las reglas posteriores a la compilación de la IPC aplicación de servidor (que se detallan en la siguiente sección) cuidadosamente separar estos dos **winmd** en directorios independientes.

Variables de entorno (especialmente % ProgramFiles %) se pueden usar en <ActivatableClassAttribute Value="path"> . Como se indicó anteriormente, el agente de la aplicación solo admite 32 bits, por lo % ProgramFiles % se resuelve como C:\\(x86) si la aplicación se ejecuta en un sistema operativo de 64 bits de los archivos de programa.

## <a name="desktop-ipc-server-detail"></a>Detalles sobre el servidor IPC de escritorio

Las dos secciones anteriores describen la declaración de la clase y los mecanismos de transporte de la referencia **winmd** al proyecto de aplicación de carga lateral. Gran parte del trabajo restante en el componente de escritorio está relacionado con la implementación. Como el objetivo del componente de escritorio es poder llamar al código de escritorio (normalmente para reutilizar activos de código existentes), el proyecto debe configurarse de una manera especial.
Normalmente, un proyecto de Visual Studio con .NET usa uno de dos "perfiles".
Uno es para el escritorio (".NetFramework") y otro es para dirigirse a la parte de la aplicación para UWP de CLR (".NetCore"). En esta característica, un componente de escritorio es un híbrido entre estos dos. Como resultado, la sección de referencias se construye de forma muy cuidadosa para mezclar estos dos perfiles.

Un proyecto de aplicación para UWP normal no contiene referencias de proyecto explícitas porque toda la superficie de la API de Windows Runtime está incluida de forma implícita.
Normalmente solo se realizan otras referencias entre proyectos. Sin embargo, un proyecto de componente de escritorio tiene un conjunto muy especial de referencias. Se inicia su vida útil como una "escritorio clásico\\biblioteca de clases" del proyecto y, por tanto, es un proyecto de escritorio. Las referencias explícitas así a la API de Windows en tiempo de ejecución (a través de las referencias a **winmd** archivos) se deben realizar. Agregar referencias adecuadas como se muestra a continuación.

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

Las referencias anteriores son una mezcla cuidadosa de referencias fundamentales para el correcto funcionamiento de este servidor híbrido. El protocolo es abrir el archivo .csproj (como se describe en cómo editar el proyecto OutputType) y agregar las referencias según sea necesario.

Cuando las referencias están correctamente configuradas, la siguiente tarea es implementar la funcionalidad del servidor. Vea el tema MSDN [procedimientos recomendados para la interoperabilidad con componentes de Windows en tiempo de ejecución (aplicaciones para UWP mediante C\#/VB/C++ y XAML)](https://msdn.microsoft.com/library/windows/apps/hh750311.aspx).
La tarea consiste en crear un archivo DLL del componente de Windows Runtime que pueda llamar al código de escritorio como parte de la implementación. La muestra correspondiente incluye los principales patrones que se usan en Windows en tiempo de ejecución:

-   Llamadas a métodos

-   Orígenes de eventos de Windows en tiempo de ejecución poro componente de escritorio

-   Operaciones asincrónicas de Windows en tiempo de ejecución

-   Devolución de matrices de tipos básicos

**Instalar**

Para instalar la aplicación, copie la implementación **winmd** al directorio especificado en el asociado carga lateral del manifiesto de aplicación correcto: <ActivatableClassAttribute>del valor = "path". Copia también los archivos auxiliares asociados y el archivo DLL de proxy/código auxiliar (este punto se trata con más detalle más adelante). Error al copiar la implementación **winmd** al servidor de ubicación del directorio hará que todas las carga lateral llamadas de la aplicación a nuevos en el RuntimeClass producirá un error "clase no registrada". Si no se instala o no se registra el proxy o código auxiliar, todas las llamadas producirán un error sin valores de retorno. Este error último suele **no** asociados con las excepciones visibles.
Si se observan excepciones debidas a este error de configuración, pueden hacer referencia a "conversión no válida".

**Consideraciones de implementación de servidor**

El servidor de Windows en tiempo de ejecución de escritorio se puede considerar como basado en "trabajos" o "tareas". Todas las llamadas al servidor funcionan en un subproceso sin interfaz de usuario, y todo el código debe ser seguro y compatible con multiproceso. También es importante qué parte de la aplicación de prueba llama a la funcionalidad del servidor. Es fundamental evitar siempre llamar a código de ejecución larga desde cualquier subproceso de interfaz de usuario en la aplicación de prueba. Hay dos maneras de realizar esto:

1.  Si llamas a la funcionalidad del servidor desde un subproceso de interfaz de usuario, usa siempre un patrón asincrónico en la implementación y en el área de la superficie pública del servidor.

2.  Llama a la funcionalidad del servidor desde un subproceso en segundo plano en la aplicación de prueba.

**Async en tiempo de ejecución de Windows en el servidor**

Dada la naturaleza entre procesos del modelo de aplicación, las llamadas al servidor tienen más sobrecarga que el código que se ejecuta exclusivamente dentro del proceso. Normalmente es seguro llamar a una propiedad simple que devuelve un valor en memoria, porque se ejecutará lo suficientemente rápido como para que no resulte preocupante un posible bloqueo del subproceso de interfaz de usuario. Sin embargo, cualquier llamada que implica una E/S de cualquier tipo (incluida la manipulación de archivos y recuperaciones de bases de datos) podría bloquear el subproceso de interfaz de usuario que llama y hacer que la aplicación finalice debido a la falta de respuesta. Además, se desaconseja realizar llamadas a propiedades de objetos en esta arquitectura de aplicación por motivos de rendimiento.
Esto se explica con más detalle en la siguiente sección.

Normalmente, un servidor correctamente implementado implementará las llamadas realizadas directamente desde subprocesos de interfaz de usuario mediante el patrón de Windows en tiempo de ejecución asincrónico. Esto se puede implementar siguiendo este patrón. Primero, la declaración (de nuevo, de la muestra correspondiente):

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Esto declara una operación de Windows en tiempo de ejecución asincrónica que devuelve un entero.
La implementación de la operación asincrónica normalmente toma esta forma:

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Nota** Es habitual esperar otras posibles operaciones de ejecución larga mientras se escribe la implementación. Si es así, el **Task.Run** debe declararse el código:

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Los clientes de este método asincrónico pueden esperar esta operación como cualquier otra operación asincrónica de Windows Runtime.

**Llamar a funciones de servidor desde un subproceso en segundo plano de la aplicación**

Como es típico que la misma organización escriba tanto el cliente como el servidor, se puede adoptar un procedimiento de programación para que todas las llamadas al servidor las realice un subproceso en segundo plano en la aplicación de prueba. Desde un subproceso en segundo plano se puede realizar una llamada directa que recopila uno o varios lotes de datos del servidor. Cuando los resultados se recuperan por completo, el lote de datos que está en memoria en el proceso de la aplicación normalmente se puede recuperar directamente del subproceso de interfaz de usuario. C\# objetos son naturalmente ágiles entre subprocesos en segundo plano y subprocesos de interfaz de usuario, por lo que son especialmente útiles para este tipo de patrón de llamada.

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Crear e implementar el proxy de Windows en tiempo de ejecución

Como el enfoque de IPC implica calcular referencias a las interfaces de Windows en tiempo de ejecución entre dos procesos, se debe usar un proxy y código auxiliar de Windows en tiempo de ejecución registrados globalmente.

**Creación del proxy en Visual Studio**

El proceso para crear y registrar los servidores proxy ni códigos auxiliares para su uso dentro de un paquete de aplicación para UWP normal se describen en el tema [provocar eventos en componentes de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/dn169426.aspx).
Los pasos que se describen en este artículo son más complicados que el proceso que se describe a continuación, porque implican registrar el proxy o el código auxiliar dentro del paquete de la aplicación (en lugar de registrarlo globalmente).

**Paso 1:** Con la solución para el proyecto de componente de escritorio, cree un proyecto de servidor Proxy/código auxiliar en Visual Studio:

**Solución > Agregar > proyecto > Visual C++ > opción consola seleccionar DLL de Win32.**

Para los pasos siguientes, se supone que el componente de servidor se denomina **MyWinRTComponent**.

**Paso 3:** Elimine todos los archivos CPP/H del proyecto.

**Paso 4:** La sección anterior "Define el contrato" contiene un comando posterior a la compilación que se ejecuta **winmdidl.exe**, **midl.exe**, **mdmerge.exe**, y así sucesivamente. Una de las salidas del paso midl de este comando posterior a la compilación genera cuatro salidas importantes:

a) Dlldata.c

b) Un archivo de encabezado (por ejemplo, MyWinRTComponent.h)

(c) A \* \_i.c archivo (por ejemplo, MyWinRTComponent\_i.c)

(d) A \* \_p.c archivo (por ejemplo, MyWinRTComponent\_p.c)

**Paso 5:** Agregue estos cuatro archivos generados al proyecto "MyWinRTProxy".

**Paso 6:** Agregue un archivo de definición al proyecto "MyWinRTProxy" **(proyecto > Agregar nuevo elemento > código > archivo de definición de módulo**) y actualiza el contenido para que sea:

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**Paso 7:** Abra las propiedades del proyecto "MyWinRTProxy":

**Propiedades de configuración > General > nombre de destino:**

MyWinRTComponent.Proxies

**C/C ++ > definiciones del preprocesador > Agregar**

"WIN32; \_WINDOWS; REGISTRAR\_PROXY\_DLL "

**C/C ++ > encabezado precompilado: Seleccione "No se usa como encabezado precompilado"**

**Enlazador > General > Omitir biblioteca de importación: Seleccione "Sí"**

**Enlazador > entrada > dependencias adicionales: Agregar rpcrt4.lib;runtimeobject.lib**

**Enlazador > metadatos de Windows > generar metadatos de Windows: Seleccione "No"**

**Paso 8:** Compile el proyecto "MyWinRTProxy".

**Implementación del proxy**

El proxy debe registrarse globalmente. La manera más sencilla de hacerlo es hacer que tu proceso de instalación llame a DllRegisterServer en el archivo dll del proxy. Ten en cuenta que como la característica solo admite servidores compilados para x86 (es decir, no admite 64 bits), la configuración más sencilla es usar un servidor de 32 bits, un proxy de 32 bits y una aplicación de prueba de 32 bits. El proxy normalmente se encuentra junto a la implementación **winmd** para el componente de escritorio.

Se debe realizar otro paso de configuración adicional. Para que el proceso de prueba cargue y ejecute el proxy, el directorio debe estar marcado como "read / execute" para ALL_APPLICATION_PACKAGES. Esto se realiza mediante el **icacls.exe** herramienta de línea de comandos. Este comando debe ejecutarse en el directorio donde la implementación **winmd** y se encuentra el archivo dll de proxy/código auxiliar:

*icacls . /T /grant \*S-1-15-2-1:RX*

## <a name="patterns-and-performance"></a>Patrones y rendimiento

Es muy importante supervisar detenidamente el rendimiento del transporte entre procesos. Una llamada entre procesos cuesta el doble que una llamada dentro del proceso. Crear conversaciones "banales" entre procesos o realizar transferencias repetidas de objetos grandes, como imágenes de mapa de bits, puede provocar un rendimiento inesperado y no deseado de la aplicación.

Esta es una lista no exhaustiva de cosas para tener en cuenta:

-   Deben evitarse siempre las llamadas a métodos sincrónicas desde subprocesos de interfaz de usuario de la aplicación al servidor. Llama al método desde un subproceso en segundo plano en la aplicación y, después, usa CoreWindowDispatcher para obtener los resultados en el subproceso de interfaz de usuario, si es necesario.

-   Llamar a operaciones asincrónicas desde un subproceso de interfaz de usuario de la aplicación es seguro, pero ten en cuenta los problemas de rendimiento que se indican más adelante.

-   La transferencia masiva de resultados reduce las conversaciones entre procesos. Normalmente esto se realiza mediante la construcción Array de Windows Runtime.

-   Devolver *lista<T>* donde *T* es un objeto de una captura de funcionamiento o la propiedad async, hará que un lote de intercambio de mensajes entre procesos. Por ejemplo, suponga que devolver un*lista&lt;personas&gt;* objetos. Cada pase de iteración será una llamada entre procesos. Cada *personas* objeto devuelto se representa mediante un servidor proxy y de cada llamada a un método o propiedad en ese objeto individual dará como resultado una llamada entre procesos. Por lo que una "inocentes" *lista&lt;personas&gt;* objeto donde *recuento* es grande hará que un gran número de llamadas lentas. La transferencia masiva de estructuras del contenido de una matriz ofrece mejor rendimiento. Por ejemplo:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

A continuación, devolver* PersonStruct\[\]* en lugar de *lista&lt;PersonObject&gt;*.
Así se obtienen todos los datos en un "salto" entre procesos.

Al igual que en todas las consideraciones de rendimiento, medir y probar es fundamental. Lo ideal es insertar telemetría en las diversas operaciones para determinar cuánto tardan. Es importante medir a través de un intervalo: por ejemplo, ¿cuánto tiempo se realmente tarda se utilizan todos los *personas* objetos para una consulta determinada en la aplicación de carga lateral?

Otra técnica es las pruebas de carga variable. Se pueden hacer poniendo enlaces a pruebas de rendimiento en la aplicación, que introducen cargas retrasadas variables en el procesamiento del servidor. Así se pueden simular diversos tipos de cargas y la reacción de la aplicación al rendimiento variable del servidor.
La muestra ilustra cómo insertar retrasos de tiempo en el código usando las técnicas asincrónicas adecuadas. La cantidad exacta de retraso que se inserta y el intervalo de aleatorización que se asigna a esa carga artificial variará según el diseño de cada aplicación y del entorno anticipado en el que se ejecute la aplicación.

## <a name="development-process"></a>Proceso de desarrollo

Cuando se realizan cambios en el servidor, hay que procurar que cualquier instancia que se ejecutara anteriormente ya no se ejecuta. En última instancia, COM se encargará de dar con esto en el proceso, pero el temporizador de resumen tardará más tiempo y reducirá la eficacia del desarrollo iterativo. En consecuencia, eliminar una instancia que se ejecutara anteriormente constituye un paso normal durante el desarrollo. Esto conlleva que el desarrollador lleve un seguimiento de la instancia de dllhost que hospeda el servidor.

El proceso de servidor se puede detectar y eliminar mediante el Administrador de tareas o cualquier otra aplicación externa. La herramienta de línea de comandos **TaskList.exe **también se incluye y tiene una sintaxis flexible, por ejemplo:

  
 | **Command** | **Acción** |
 | ------------| ---------- |
 | tasklist | Genera una lista con todos los procesos en ejecución en orden aproximado de hora de creación, con los procesos más recientes al final. |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | Genera una lista de todas las instancias de dllhost.exe. El conmutador /M enumera los módulos que se han cargado. |
 | tasklist /FI "PID eq 12564" /M | Esta opción te puede servir para realizar una consulta a dllhost.exe si conoces su PID. |

Debe enumerar la lista de módulos para un servidor de agente *clrhost.dll* en su lista de los módulos cargados.

## <a name="resources"></a>Recursos

-   [Plantillas de proyecto de componente de WinRT asíncrona para Windows 10 y VS 2015](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

-   [Ejemplo de componente de WinRT asíncrona NorthwindRT](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Entrega de aplicaciones Microsoft Store de confianza](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [Contratos de aplicación y las extensiones (aplicaciones de Windows Store)](https://msdn.microsoft.com/library/windows/apps/hh464906.aspx)

-   [Cómo agregar o quitar aplicaciones en Windows 10](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#GroupPolicy)

-   [Implementación de aplicaciones UWP para empresas](https://go.microsoft.com/fwlink/p/?LinkID=264770)

