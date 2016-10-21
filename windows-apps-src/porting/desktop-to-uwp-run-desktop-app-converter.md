---
author: awkoren
Description: "Ejecuta Desktop Converter App para convertir una aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) en una aplicación de Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
translationtype: Human Translation
ms.sourcegitcommit: ed4da94f2732c279c3071135168da9e4b18953cb
ms.openlocfilehash: c0ed8386cb823ea83e5b1f80cd584f370a85f278

---

# Desktop App Converter

[Obtener Desktop App Converter](https://aka.ms/converter)

Desktop App Converter (DAC) es una herramienta que te permite trasladar tus aplicaciones de escritorio existentes escritas para .NET 4.6.1 o Win32 a la Plataforma universal de Windows (UWP). Puedes ejecutar los instaladores de escritorio a través del convertidor en un modo de instalación desatendida (silenciosa) y obtener el paquete AppX que puedes instalar mediante el cmdlet Add-AppxPackage de PowerShell en la máquina de desarrollo.

Desktop App Converter ahora está disponible en la [Tienda Windows](https://aka.ms/converter).

El convertidor ejecuta el instalador de escritorio en un entorno de Windows aislado con una imagen base limpia proporcionada como parte de la descarga del convertidor. Captura cualquier E/S del sistema de archivos y del registro realizada por el instalador de escritorio y la empaqueta como parte de la salida. El convertidor genera un AppX con la identidad del paquete y la capacidad de llamar a una gran variedad de API de WinRT.

## Novedades

En esta sección se describen los cambios entre las versiones de Desktop App Converter. 

### 14/09/2016 (v1.0)

* Desktop App Converter ahora está disponible para descargar en la [Tienda Windows](https://aka.ms/converter). 
* Consigue las imágenes base de Windows 10 (.wim) más recientes en el [Centro de descarga](https://www.microsoft.com/download/details.aspx?id=53833) para su uso con DAC.
* Con la aplicación de la Tienda, ahora puedes usar el nuevo punto de entrada *DesktopAppConverter.exe <arguments>* para ejecutar el convertidor desde cualquier lugar en un símbolo del sistema con privilegios elevados o una ventana de PowerShell.  

### 02/09/2016 (v0.1.25)

* Se integró el paquete de NuGet dotnet-computervirtualization más reciente.
* Se agregaron las dependencias incorporadas recientemente en common.dll.
* Varias correcciones de errores.

### 04/08/2016 (v0.1.24)

* Se agregó compatibilidad para la firma automática de las aplicaciones convertidas generadas por DAC con fines de prueba. Echa un vistazo a la marca ```–Sign``` para probarla. 
* Se agregaron advertencias si cualquiera de los registros COM del subárbol de Registro virtual no se admite en el paquete AppX.  
* Se agregó compatibilidad para la detección automática de dependencias de aplicación en bibliotecas de VC ++ y su posterior conversión a dependencias del manifiesto AppX. Ten en cuenta que para transferir localmente y probar las aplicaciones con VC ++ en tiempo de ejecución, tendrás que descargar los paquetes de marcos VCLib según se describe en la entrada de blog [Using Visual C++ Runtime in a Centennial project (Uso de Visual C++ en tiempo de ejecución en un proyecto Centennial)](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project). Busca los paquetes en la carpeta ```Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop``` en tu máquina, navega a la versión correspondiente para ti (por ejemplo, 11.0, 12.0, 14.0) y haz doble clic en el paquete de la arquitectura adecuada (x64, x86) para instalarla.
* Se actualizó el esquema del manifiesto para alinearlo con la Actualización de aniversario de Windows 10 (10.0.14393.0). 
* Varias correcciones de errores y diseño de salida mejorado. 

### 07/07/2016 (v0.1.22)

* Se agregó compatibilidad para detectar automáticamente las extensiones de shell de la aplicación de escritorio y declararlas en AppXManifest del paquete de UWP. Para obtener más información sobre las extensiones de escritorio, consulte [**Extensiones para aplicaciones de escritorio convertidas**](desktop-to-uwp-extensions.md). 
* Detección mejorada de AppExecutable para un gran conjunto de aplicaciones. 

### 16/06/2016 (v0.1.20)

* Corrige problemas que impedían las conversiones correctas en las últimas compilaciones de Windows 10 Insider Preview. 
* Se reemplazó ```–CreateX86Package``` por ```–PackageArch```, lo que permite especificar la arquitectura del paquete generado. 

### 8/6/2016

* Se agregó soporte técnico para generar paquetes .appx x86 en equipos host AMD64 que ejecutan el convertidor.
* Se redujo el uso del espacio en disco mediante la eliminación de las imágenes base expandidas anteriormente.
* Se agregó soporte técnico para limpiar los archivos temporales y las imágenes base innecesarias.
* Se mejoró el soporte técnico para detectar asociaciones de protocolos y tipos de archivo.
* Se mejoró la lógica para detectar la propiedad AppExecutable en un gran conjunto de aplicaciones.
* Se agregó soporte técnico para proporcionar parámetros -InstallerArguments adicionales para instaladores basados en MSI.
* Correcciones de errores para todos los errores PathTooLongException durante el proceso de conversión.

### 12/5/2016

- Se restauró la compatibilidad de la edición profesional de Windows. 
- La marca ```-Setup``` habilita ahora la característica Contenedores de Windows y controla la expansión de la imagen base. Ejecuta lo siguiente desde un símbolo del sistema de PowerShell con privilegios elevados para realizar una instalación única: ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- Se agregó la autodetección de la ruta de instalación y el movimiento de la raíz de la aplicación fuera de VFS para reducir los redireccionamientos del sistema de archivos innecesarios en tiempo de ejecución.
- Se agregó la autodetección de la imagen base expandida como parte del proceso de conversión.
- Se agregó la detección automática de asociaciones de tipos de archivo y protocolos.
- Se mejoró la lógica para detectar el acceso directo del menú Inicio.
- Se mejoró el filtrado del sistema de archivos para conservar los archivos MUI instalados con la aplicación.
- Se actualizó la versión de escritorio mínima admitida (10.0.14342.0) en el manifiesto.

## Requisitos del sistema

### Sistema operativo compatible
+ Versión preliminar de la Actualización de aniversario de Windows 10 versión Enterprise (compilación 10.0.14342.0 y posterior)

### Configuración del hardware necesario

El equipo debe tener las siguientes funcionalidades mínimas:
+ procesador de 64 bits (x64)
+ Virtualización asistida por hardware
+ Traducción de direcciones de segundo nivel (SLAT)

### Recursos necesarios

+ [Kit de desarrollo de software de Windows (SDK) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375)

## Instalar Desktop App Converter   

Desktop App Converter se basa en las características de Windows 10 que se envían como parte de las compilaciones de Windows Insider Preview. Asegúrate de que estás en la última compilación para usar el convertidor.

1. Asegúrate de tener el último sistema operativo Windows 10 Insider Preview, edición Enterprise o Pro (http://insider.windows.com). 
2. Descarga el archivo DesktopAppConverter.zip y el archivo .wim de la imagen base correspondientes a tu versión de Insider Preview (http://aka.ms/converter). 
3. Extrae DesktopAppConverter.zip en una carpeta local.
4. Desde una ventana de administración de PowerShell:  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. Ejecuta el siguiente comando desde una ventana de administración de PowerShell para configurar el convertidor:
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose
```
6. Si al ejecutar los comandos anteriores se te pide reiniciar, reinicia el equipo y vuelve a ejecutar el comando.

## Ejecutar Desktop App Converter
Desktop App Converter tiene dos puntos de entrada: PowerShell y Shell de comandos. Puedes usar cualquiera de estos puntos de entrada para iniciar el proceso de conversión.

### Uso
```CMD
DesktopAppConverter.ps1
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]  
```

### Ejemplo
El siguiente ejemplo muestra cómo convertir una aplicación de escritorio denominada *MyApp* de *&lt;publisher_name&gt;* a un paquete de UWP (AppX).

+ Desde una ventana de administración de PowerShell, ejecuta el siguiente comando:
```CMD
PS C:\>.\DesktopAppConverter.ps1 -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## Implementa la AppX convertida

Usa el cmdlet [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) en PowerShell para implementar un paquete de la aplicación firmado (.appx) en una cuenta de usuario. 

Puedes usar la marca ```-Sign``` en Desktop App Converter (v0.1.24) para la firma automática de la aplicación convertida. Como alternativa, consulta [Firmar la aplicación de escritorio convertida](desktop-to-uwp-signing.md) para aprender a autofirmar paquetes AppX.

También puedes usar el parámetro ```-Register``` del cmdlet Add-AppXPackage de PowerShell para instalar desde una carpeta de archivos sin empaquetar durante el proceso de desarrollo. 

Para obtener más información sobre la implementación y la depuración de la aplicación convertida, consulta [Implementar y depurar la aplicación para UWP convertida](desktop-to-uwp-deploy-and-debug.md). 

## Firmar el paquete .appx

El cmdlet Add-AppxPackage requiere que el paquete de la aplicación (.appx) que se implemente esté firmado. Para firmar el paquete .appx, usa la marca ```-Sign``` como parte de la línea de comandos del convertidor o SignTool.exe, que se incluye en Microsoft Windows 10 SDK.

Para obtener detalles adicionales sobre cómo firmar el paquete .appx, consulta [Firmar la aplicación de escritorio convertida](desktop-to-uwp-signing.md). 

## Advertencias

1. La compilación de Windows 10 de la máquina host debe coincidir con la imagen base que obtuviste como parte de la descarga de Desktop App Converter.  
2. Asegúrate de que el instalador de escritorio está en un directorio independiente, porque el convertidor copia todo el contenido del directorio en el entorno de Windows aislado.  
3. Actualmente, Desktop App Converter admite la ejecución del proceso de conversión solo en un sistema operativo de 64 bits. Puedes implementar los paquetes .appx convertidos solo en un sistema operativo de 64 bits (x 64).  
4. Desktop App Converter requiere el instalador de escritorio para ejecutarse en modo de instalación desatendida. Asegúrate de que pasas el indicador silent para el instalador en el convertidor mediante el parámetro *-InstallerArguments*.
5. La publicación de ensamblados SxS Fusion públicos no funcionará. Durante la instalación, una aplicación puede publicar ensamblados Fusion públicos en paralelo, accesibles para cualquier otro proceso. Durante la creación de contexto de activación de proceso, estos ensamblados se recuperan mediante un proceso del sistema denominado CSRSS.exe. Cuando esto se realiza para un proceso convertido, se produce un error en la creación de contexto de activación y en la carga del módulo de estos ensamblados. Los ensamblados de bandeja de entrada, como ComCtl, se envían con el sistema operativo, por lo que depender de ellos es seguro. Los ensamblados SxS Fusion se registrados en las siguientes ubicaciones:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de archivos: %windir%\\SideBySide

## Problemas conocidos

+ Si recibes un paquete piloto de Windows Insider en una máquina de desarrollo que anteriormente tenía Desktop App Converter instalado, puede que se muestre el error `New-ContainerNetwork: The object already exists` al instalar la nueva imagen base. Como solución, ejecuta el comando `Netsh int ipv4 reset` desde un símbolo del sistema con privilegios elevados y, después, reinicia el equipo. 
+ No se podrá instalar una aplicación .NET compilada con la opción de compilación "AnyCPU" si el archivo ejecutable principal o cualquiera de las dependencias están colocados en "Archivos de programa" o "Windows\System32". Como solución, usa el instalador de escritorio específico de la arquitectura (32 o 64 bits) para generar correctamente un paquete AppX.

## Telemetría de Desktop App Converter  
Desktop App Converter puede recopilar información sobre ti y sobre el uso del software, y enviarla a Microsoft. Puedes obtener más información sobre la recopilación de datos de Microsoft y usarla en la documentación del producto y en la [Declaración de privacidad de Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Te comprometes a cumplir con todas las disposiciones aplicables de la Declaración de privacidad de Microsoft.

De manera predeterminada, la telemetría se habilitará en Desktop App Converter. Agrega la siguiente clave del registro para establecer la telemetría en una configuración deseada:  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Agrega o edita el valor *DisableTelemetry* mediante un DWORD establecido en 1.
+ Para habilitar la telemetría, quita la clave o establece el valor en 0.

## Uso de Desktop App Converter
Esta es una lista de parámetros para Desktop App Converter. También puedes ver esta lista en la ventana de Windows Powershell si ejecutas el siguiente comando:  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### Parámetros de configuración  
|Parámetro|Descripción|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Ejecuta DesktopAppConverter en modo de instalación. El modo de instalación admite la expansión de una imagen base proporcionada.|
|```-BaseImage <String>``` | Ruta de acceso completa a una imagen base no expandida. Este parámetro es necesario si se especifica el programa de instalación.|
|```-LogFile <String>``` [opcional] | Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro.|
|```-NatSubnetPrefix <String>``` [opcional] | Valor del prefijo que se usará para la instancia de Nat. Por lo general, se recomienda que lo cambies solo si el equipo host se adjunta al mismo intervalo de subred que el NetNat del convertidor. Puedes consultar la configuración actual del NetNat del convertidor mediante el cmdlet **NetNat Get**. |
|```-NoRestart [<SwitchParameter>]``` | No solicites el reinicio mientras se ejecute la instalación (es necesario reiniciar el sistema para habilitar la función de contenedor). |

### Parámetros de conversión  
|Parámetro|Descripción|
|---------|-----------|
|```-Installer <String>``` | La ruta de acceso al instalador de la aplicación se debe poder ejecutar de forma desatendida o silenciosa.|
|```-InstallerArguments <String>``` [opcional] | Una lista separada por comas o una cadena de argumentos para forzar al instalador para que se ejecute de forma desatendida o silenciosa. Este parámetro es opcional si el instalador es un msi. Para obtener un registro a partir del instalador, proporciona el argumento de registro para el instalador aquí y usa la ruta de acceso ```<log_folder>```, que es un token que el convertidor reemplaza con la ruta de acceso apropiada. <br><br>**Nota: Los argumentos de registro y marcas de instalación desatendida o silenciosa varían entre las tecnologías de instalador.** <br><br>Un ejemplo de uso de este parámetro: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Otro ejemplo que no produzca un archivo de registro puede tener el siguiente aspecto: ```-InstallerArguments "/quiet", "/norestart"``` De nuevo, se deben dirigir literalmente todos los registros a la ruta de acceso de token ```<log_folder>``` si quieres que el convertidor lo capture y lo incluya en la carpeta de registro final.|
|```-InstallerValidExitCodes <Int32>``` [opcional] | Una lista separada por comas de códigos de salida que indica que el programa de instalación se ejecutó correctamente (por ejemplo: 0, 1234, 5678).  De forma predeterminada, es 0 para no msi y 0, 1641, 3010 para msi.|
|```-Destination <String>``` | El destino deseado para la salida de appx del convertidor: DesktopAppConverter puede crear esta ubicación si ya no existe.|

### Parámetros de identidad AppX  
|Parámetro|Descripción|
|---------|-----------|
|```-PackageName <String>``` | El nombre del paquete de la aplicación universal de Windows
|```-Publisher <String>``` | El publicador del paquete de la aplicación universal de Windows
|```-Version <Version>``` | El número de versión del paquete de la aplicación universal de Windows

### Parámetros opcionales de manifiesto de Appx  
|Parámetro|Descripción|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [opcional] | El nombre del archivo ejecutable principal de la aplicación (por ejemplo, "MyApp.exe"). |
|```-AppFileTypes <String>``` [opcional] | Una lista separada por comas de tipos de archivo a los que se asociará la aplicación (p.ej. ".txt, .doc", sin las comillas).|
|```-AppId <String>``` [opcional] | Especifica un valor para establecer el id. de la aplicación en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|```-AppDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar de la aplicación en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|```-AppDescription <String>``` [opcional] | Especifica un valor para establecer la descripción de la aplicación en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|```-PackageDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar del paquete en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar del publicador del paquete en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *Publisher*. |

### Otros parámetros de conversión  
|Parámetro|Descripción|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [opcional] | Ruta de acceso completa a una imagen base ya expandida.|
|```-MakeAppx [<SwitchParameter>]``` [opcional] | Un modificador que, cuando está presente, indica a este script que llame a MakeAppx en la salida. |
|```-LogFile <String>``` [opcional] | Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro. |
| ```Sign [<SwitchParameter>] [optional]``` | Indica a este script que firme el paquete AppX de salida. Este parámetro debe aparecer junto con el modificador ```-MakeAppx```. 
|```<Common parameters>``` | Este cmdlet admite los parámetros comunes: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* y *OutVariable*. Para obtener más información, consulta [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### Parámetros de limpieza
|Parámetro|Descripción|
|---------|-----------|
|```Cleanup [<Option>]``` | Ejecuta la limpieza de los artefactos de DesktopAppConverter. Hay 3 opciones válidas para el modo de limpieza. |
|```Cleanup All``` | Elimina todas las imágenes base expandidas, quita todos los archivos temporales del convertidor, quita la red de contenedores y deshabilita la característica Contenedores opcional de Windows. |
|```Cleanup WorkDirectory``` | Quita todos los archivos temporales del convertidor. |
|```Cleanup ExpandedImage``` | Elimina todas las imágenes base expandidas instaladas en la máquina host. |

### Arquitectura del paquete
Desktop App Converter ahora admite la creación de paquetes de la aplicación x86 y x64 que puedes instalar y ejecutar en máquinas x86 y amd64. Ten en cuenta que Desktop App Converter debe seguir ejecutándose en un equipo AMD64 para realizar una conversión correcta.

|Parámetro|Descripción|
|---------|-----------|
|```-PackageArch <String>``` | Genera un paquete con la arquitectura especificada. Las opciones válidas son 'x86' o 'x64'; por ejemplo, -PackageArch x86. Este parámetro es opcional. Si no se especifica, DesktopAppConverter intentará detectar automáticamente la arquitectura del paquete. Si se produce un error en la detección automática, se establece el paquete x64 de forma predeterminada. 

### Ejecutar PEHeaderCertFixTool

Durante el proceso de conversión, DesktopAppConverter ejecuta automáticamente PEHeaderCertFixTool para corregir cualquier encabezado PE dañado. Sin embargo, también puedes ejecutar PEHeaderCertFixTool en un appx de UWP, en archivos sueltos o un binario concreto. 

PEHeaderCertFixTool se incluye como parte de DesktopAppConverter.zip. Ejemplo de uso: 

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## Compatibilidad con idiomas

Desktop App Converter no admita Unicode; por lo tanto, no es posible usar caracteres chinos ni caracteres que no sean ASCII con la herramienta.

## Consulta también
+ [Obtener Desktop App Converter](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [Lleva tu aplicación de escritorio a la Plataforma universal de Windows](https://developer.microsoft.com/windows/bridges/desktop)
+ [Traer las aplicaciones de escritorio a UWP mediante Desktop App Converter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: Bringing Existing Desktop Applications to the Universal Windows Platform (Proyecto Centennial: llevar las aplicaciones de escritorio existentes a la Plataforma universal de Windows)](https://channel9.msdn.com/events/Build/2016/B829)  
+ [UserVoice para puente de escritorio](http://aka.ms/UserVoiceDesktopToUwp)
+ [Ejemplos de código de puente de aplicación de escritorio a UWP en GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Sep16_HO5-->


