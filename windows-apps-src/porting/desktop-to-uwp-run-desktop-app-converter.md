---
author: normesta
Description: "Ejecuta Desktop Converter App para convertir una aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) en una aplicación de Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Puente de escritorio a UWP, Desktop App Converter
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.openlocfilehash: 4daf45a4637ac846c550430cbc7518238ebd5bd8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-desktop-app-converter"></a>Puente de escritorio a UWP: Desktop App Converter

[Obtener Desktop App Converter](https://aka.ms/converter)

Desktop App Converter (DAC) es una herramienta que te permite trasladar tus aplicaciones de escritorio existentes escritas para .NET 4.6.1 o Win32 a la Plataforma universal de Windows (UWP). Puedes ejecutar los instaladores de escritorio a través del convertidor en un modo de instalación desatendida (silenciosa) y obtener un paquete de la aplicación de Windows que puedes instalar mediante el cmdlet Add-AppxPackage de PowerShell en la máquina de desarrollo.

Desktop App Converter ahora está disponible en la [Tienda Windows](https://aka.ms/converter).

El convertidor ejecuta el instalador de escritorio en un entorno de Windows aislado con una imagen base limpia proporcionada como parte de la descarga del convertidor. Captura cualquier E/S del sistema de archivos y del registro realizada por el instalador de escritorio y la empaqueta como parte de la salida. El convertidor genera un paquete de la aplicación de Windows con la identidad del paquete y la capacidad de llamar a una gran variedad de API de WinRT.

## <a name="whats-new"></a>Novedades

La versión más reciente de DAC es v1.0.9.0. Novedades de esta actualización:

* Conversión sin instalador: si tu aplicación se instala mediante xcopy o estás familiarizado con los cambios que realiza el instalador de la aplicación en el sistema, puedes ejecutar la conversión sin instalador al establecer el parámetro Installer en el directorio raíz de los archivos de tu aplicación.
* Validación del paquete de la aplicación: usa la nueva marca `-Verify` para validar el paquete de la aplicación convertido con los requisitos de Puente de dispositivo de escritorio y la Tienda.


## <a name="system-requirements"></a>Requisitos del sistema

### <a name="operating-system"></a>Sistema operativo

+ Edición Pro o Enterprise de Actualización de aniversario de Windows 10 (10.0.14393.0 y posterior).

### <a name="hardware-configuration"></a>Configuración de hardware

+ Procesador de 64 bits (x64)
+ Virtualización asistida por hardware
+ Traducción de direcciones de segundo nivel (SLAT)

### <a name="required-resources"></a>Recursos necesarios

+ [Kit de desarrollo de software de Windows (SDK) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Instalar Desktop App Converter

*(Estos pasos no son necesarios para la conversión sin instalador)*

Desktop App Converter se basa en las características más recientes de Windows10. Asegúrate de ejecutar la Actualización de aniversario de Windows 10 (14393.0) o compilaciones posteriores.

1.    Descarga [DesktopAppConverter de la Tienda Windows](https://aka.ms/converter) y el [archivo de imagen base .wim que coincide con tu compilación](https://aka.ms/converterimages).  
2.    Ejecuta DesktopAppConverter como administrador. Puedes hacerlo desde el menú Inicio si haces clic con el botón derecho en el icono y seleccionas *Ejecutar como administrador* en *Más*, o bien desde la barra de tareas si haces clic con el botón derecho en el icono, haces clic otra vez con el botón derecho en el nombre de la aplicación que aparece y, por último, seleccionas *Ejecutar como administrador.*
3.    En la ventana de consola de la aplicación, ejecuta ```Set-ExecutionPolicy bypass```.
4.    Configura el convertidor al ejecutar ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` desde la ventana de consola de la aplicación.
5.    Si al ejecutar los comandos anteriores se te pide reiniciar, reinicia el equipo.

## <a name="run-the-desktop-app-converter"></a>Ejecutar Desktop App Converter

+ **Descarga de la Tienda**: usa ```DesktopAppConverter.exe``` para ejecutar el convertidor.

### <a name="usage"></a>Uso

```CMD
DesktopAppConverter.exe
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

### <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra cómo convertir una aplicación de escritorio llamada *MyApp* de *MyPublisher* a un paquete de la aplicación de Windows.

#### <a name="no-installer-conversion"></a>Conversión sin instalador

Con la conversión sin instalador, el parámetro `-Installer` señala al directorio raíz de los archivos de la aplicación y el parámetro `-AppExecutable` es obligatorio.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

#### <a name="installer-based-conversion"></a>Conversión basada en instalador

Con la conversión basada en instalador, el parámetro `-Installer` señala al instalador de configuración de la aplicación.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

## <a name="deploy-your-converted-windows-app-package"></a>Implementar el paquete de la aplicación de Windows convertido

Usa el cmdlet [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) en PowerShell para implementar un paquete de la aplicación firmado (.appx) en una cuenta de usuario.

Puedes usar la marca ```-Sign``` en Desktop App Converter (v0.1.24) para la firma automática de la aplicación convertida. Como alternativa, consulta [Firmar la aplicación de escritorio convertida](desktop-to-uwp-signing.md) para aprender a autofirmar paquetes AppX.

También puedes usar el parámetro ```-Register``` del cmdlet Add-AppXPackage de PowerShell para instalar desde una carpeta de archivos sin empaquetar durante el proceso de desarrollo.

Para obtener más información sobre la implementación y la depuración de la aplicación convertida, consulta [Implementar y depurar la aplicación para UWP convertida](desktop-to-uwp-deploy-and-debug.md).

## <a name="sign-your-windows-app-package"></a>Firmar el paquete de la aplicación de Windows

El cmdlet Add-AppxPackage requiere que el paquete de la aplicación de Windows (.appx) que se implemente esté firmado. Para firmar el paquete de la aplicacíon de Windows, usa la marca ```-Sign``` como parte de la línea de comandos del convertidor o SignTool.exe, que se incluye en el Microsoft Windows 10 SDK.

Para obtener detalles adicionales sobre cómo firmar el paquete de la aplicación de Windows, consulta [Firmar la aplicación de escritorio convertida](desktop-to-uwp-signing.md).

Nota: Si intentas firmar un paquete con el certificado generado automáticamente, tendrás que usar la contraseña predeterminada "123456".

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>Modificar la carpeta VFS y el subárbol del Registro (opcional)

Desktop App Converter adopta un enfoque muy conservador para filtrar el ruido del sistema y archivos del contenedor.  Esto no es necesario, pero después de la conversión, puedes:

1. Revisar la carpeta VFS y eliminar los archivos que el instalador no necesite.
2. Revisar el contenido de Reg.dat y eliminar las claves que no estén instaladas o que la aplicación no necesite.

Si realizas cambios en la aplicación convertida (incluidos los anteriores), no necesitas volver a ejecutar el convertidor; puedes volver a empaquetar manualmente tu aplicación con la herramienta MakeAppx y el archivo appxmanifest.xml que DAC genera para tu aplicación. Para obtener ayuda, consulta [Convertir manualmente la aplicación a UWP mediante el puente de escritorio](desktop-to-uwp-manual-conversion.md).

## <a name="caveats"></a>Advertencias

1. La compilación de Windows 10 de la máquina host debe coincidir con la imagen base que obtuviste como parte de la descarga de Desktop App Converter.  
2. Asegúrate de que el instalador de escritorio está en un directorio independiente, porque el convertidor copia todo el contenido del directorio en el entorno de Windows aislado.  
3. Actualmente, Desktop App Converter admite la ejecución del proceso de conversión solo en un sistema operativo de 64 bits. Puedes implementar los paquetes de aplicación de Windows convertidos solo en un sistema operativo de 64 bits (x 64).  
4. Desktop App Converter requiere el instalador de escritorio para ejecutarse en modo de instalación desatendida. Asegúrate de que pasas el indicador silent para el instalador en el convertidor mediante el parámetro *-InstallerArguments*.
5. La publicación de ensamblados SxS Fusion públicos no funcionará. Durante la instalación, una aplicación puede publicar ensamblados Fusion públicos en paralelo, accesibles para cualquier otro proceso. Durante la creación de contexto de activación de proceso, estos ensamblados se recuperan mediante un proceso del sistema denominado CSRSS.exe. Cuando esto se realiza para un proceso convertido, se produce un error en la creación de contexto de activación y en la carga del módulo de estos ensamblados. Los ensamblados de bandeja de entrada, como ComCtl, se envían con el sistema operativo, por lo que depender de ellos es seguro. Los ensamblados SxS Fusion se registrados en las siguientes ubicaciones:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de archivos: %windir%\\SideBySide

## <a name="known-issues"></a>Problemas conocidos

* Estamos investigando los siguientes errores que se producen en algunas compilaciones de SO:

    * ```E_CREATTING_ISOLATED_ENV_FAILED```
    * ```E_STARTING_ISOLATED_ENV_FAILED```

    Si se produce cualquiera de estos errores, asegúrate de que estás usando una imagen base válida del [centro de descarga](https://aka.ms/converterimages). Si estás usando un archivo .wim válido, envíanos los registros a converter@microsoft.com para ayudarnos a investigar.

* Si recibes un paquete piloto de Windows Insider en una máquina de desarrollo que anteriormente tenía Desktop App Converter instalado, puede que se muestre el error `New-ContainerNetwork: The object already exists` al instalar la nueva imagen base. Como solución, ejecuta el comando `Netsh int ipv4 reset` desde un símbolo del sistema con privilegios elevados y, después, reinicia el equipo.

* No se podrá instalar una aplicación .NET compilada con la opción de compilación "AnyCPU" si el archivo ejecutable principal o cualquiera de las dependencias están colocados en "Archivos de programa" o "Windows\System32". Como solución, usa el instalador de escritorio específico de la arquitectura (32 o 64 bits) para generar correctamente un paquete de la aplicación de Windows.

## <a name="telemetry-from-desktop-app-converter"></a>Telemetría de Desktop App Converter

Desktop App Converter puede recopilar información sobre ti y sobre el uso del software, y enviarla a Microsoft. Puedes obtener más información sobre la recopilación de datos de Microsoft y usarla en la documentación del producto y en la [Declaración de privacidad de Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Te comprometes a cumplir con todas las disposiciones aplicables de la Declaración de privacidad de Microsoft.

De manera predeterminada, la telemetría se habilitará en Desktop App Converter. Agrega la siguiente clave del registro para establecer la telemetría en una configuración deseada:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Agrega o edita el valor *DisableTelemetry* mediante un DWORD establecido en 1.
+ Para habilitar la telemetría, quita la clave o establece el valor en 0.

## <a name="desktop-app-converter-usage"></a>Uso de Desktop App Converter

A continuación, se ofrece una lista de parámetros para Desktop App Converter. También puedes ver esta lista si ejecutas:   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>Parámetros de configuración  

|Parámetro|Descripción|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Ejecuta DesktopAppConverter en modo de instalación. El modo de instalación admite la expansión de una imagen base proporcionada.|
|```-BaseImage <String>``` | Ruta de acceso completa a una imagen base no expandida. Este parámetro es necesario si se especifica el programa de instalación.|
|```-LogFile <String>``` [opcional] | Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro.|
|```-NatSubnetPrefix <String>``` [opcional] | Valor del prefijo que se usará para la instancia de Nat. Por lo general, se recomienda que lo cambies solo si el equipo host se adjunta al mismo intervalo de subred que el NetNat del convertidor. Puedes consultar la configuración actual del NetNat del convertidor mediante el cmdlet **NetNat Get**. |
|```-NoRestart [<SwitchParameter>]``` | No solicites el reinicio mientras se ejecute la instalación (es necesario reiniciar el sistema para habilitar la función de contenedor). |

### <a name="conversion-parameters"></a>Parámetros de conversión  

|Parámetro|Descripción|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | La ruta de acceso completa a la carpeta raíz de la aplicación para los archivos instalados si se hubiera instalado la aplicación (por ejemplo, "C:\Archivos de programa (x86)\MyApp").|
|```-Destination <String>``` | El destino deseado para la salida de appx del convertidor: DesktopAppConverter puede crear esta ubicación si ya no existe.|
|```-Installer <String>``` | La ruta de acceso al instalador de la aplicación se debe poder ejecutar de forma desatendida o silenciosa. Conversión sin instalador, esta es la ruta de acceso al directorio raíz de los archivos de tu aplicación. |
|```-InstallerArguments <String>``` [opcional] | Una lista separada por comas o una cadena de argumentos para forzar al instalador para que se ejecute de forma desatendida o silenciosa. Este parámetro es opcional si el instalador es un msi. Para obtener un registro a partir del instalador, proporciona el argumento de registro para el instalador aquí y usa la ruta de acceso ```<log_folder>```, que es un token que el convertidor reemplaza con la ruta de acceso apropiada. <br><br>**Nota: Los argumentos de registro y marcas de instalación desatendida o silenciosa varían entre las tecnologías de instalador.** <br><br>Un ejemplo de uso de este parámetro: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Otro ejemplo que no produzca un archivo de registro puede tener el siguiente aspecto: ```-InstallerArguments "/quiet", "/norestart"``` De nuevo, se deben dirigir literalmente todos los registros a la ruta de acceso de token ```<log_folder>``` si quieres que el convertidor lo capture y lo incluya en la carpeta de registro final.|
|```-InstallerValidExitCodes <Int32>``` [opcional] | Una lista separada por comas de códigos de salida que indica que el programa de instalación se ejecutó correctamente (por ejemplo: 0, 1234, 5678).  De forma predeterminada, es 0 para no msi y 0, 1641, 3010 para msi.|

### <a name="windows-app-package-identity-parameters"></a>Parámetros de identidad del paquete de la aplicación de Windows  

|Parámetro|Descripción|
|---------|-----------|
|```-PackageName <String>``` | El nombre del paquete de la aplicación universal de Windows
|```-Publisher <String>``` | El publicador del paquete de la aplicación universal de Windows
|```-Version <Version>``` | El número de versión del paquete de la aplicación universal de Windows

### <a name="optional-windows-app-package-manifest-parameters"></a>Parámetros opcionales de manifiesto del paquete de la aplicación de Windows  

|Parámetro|Descripción|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [opcional] | El nombre del archivo ejecutable principal de la aplicación (por ejemplo, "MyApp.exe"). Este parámetro es obligatorio para realizar una conversión sin instalador. |
|```-AppFileTypes <String>``` [opcional] | Una lista separada por comas de tipos de archivo a los que se asociará la aplicación (p.ej. ".txt, .doc", sin las comillas).|
|```-AppId <String>``` [opcional] | Especifica un valor para establecer el identificador de la aplicación en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|```-AppDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar de la aplicación en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|```-AppDescription <String>``` [opcional] | Especifica un valor para establecer la descripción de la aplicación en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|```-PackageDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar del paquete en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar del editor del paquete en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *Publisher*. |

### <a name="other-conversion-parameters"></a>Otros parámetros de conversión  

|Parámetro|Descripción|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [opcional] | Ruta de acceso completa a una imagen base ya expandida.|
|```-MakeAppx [<SwitchParameter>]``` [opcional] | Un modificador que, cuando está presente, indica a este script que llame a MakeAppx en la salida. |
|```-LogFile <String>``` [opcional] | Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro. |
| ```-Sign [<SwitchParameter>] [optional]``` | Indica a este script que se debe firmar el paquete de la aplicación de Windows de salida. Este parámetro debe aparecer junto con el modificador ```-MakeAppx```.
|```<Common parameters>``` | Este cmdlet admite los parámetros comunes: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* y *OutVariable*. Para obtener más información, consulta [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |
| ```-Verify [<SwitchParameter>] [optional]``` | Modificador que, cuando está presente, indica a DAC que se debe validar el paquete de la aplicación convertido según los requisitos de Puente de dispositivo de escritorio y la Tienda Windows. El resultado es un informe de validación "VerifyReport.xml" que se visualiza mejor en un navegador. Este parámetro debe aparecer junto con el modificador `-MakeAppx`.

### <a name="cleanup-parameters"></a>Parámetros de limpieza

|Parámetro|Descripción|
|---------|-----------|
|```Cleanup [<Option>]``` | Ejecuta la limpieza de los artefactos de DesktopAppConverter. Hay 3 opciones válidas para el modo de limpieza. |
|```Cleanup All``` | Elimina todas las imágenes base expandidas, quita todos los archivos temporales del convertidor, quita la red de contenedores y deshabilita la característica Contenedores opcional de Windows. |
|```Cleanup WorkDirectory``` | Quita todos los archivos temporales del convertidor. |
|```Cleanup ExpandedImage``` | Elimina todas las imágenes base expandidas instaladas en la máquina host. |

### <a name="package-architecture"></a>Arquitectura del paquete

Desktop App Converter ahora admite la creación de paquetes de la aplicación x86 y x64 que puedes instalar y ejecutar en máquinas x86 y amd64. Ten en cuenta que Desktop App Converter debe seguir ejecutándose en un equipo AMD64 para realizar una conversión correcta.

|Parámetro|Descripción|
|---------|-----------|
|```-PackageArch <String>``` | Genera un paquete con la arquitectura especificada. Las opciones válidas son 'x86' o 'x64'; por ejemplo, -PackageArch x86. Este parámetro es opcional. Si no se especifica, DesktopAppConverter intentará detectar automáticamente la arquitectura del paquete. Si se produce un error en la detección automática, se establece el paquete x64 de forma predeterminada.

### <a name="running-the-peheadercertfixtool"></a>Ejecutar PEHeaderCertFixTool

Durante el proceso de conversión, DesktopAppConverter ejecuta automáticamente PEHeaderCertFixTool para corregir cualquier encabezado PE dañado. Sin embargo, también puedes ejecutar PEHeaderCertFixTool en un paquete de la aplicación de Windows de UWP, en archivos sueltos o un binario concreto. Ejemplo de uso:

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>Compatibilidad con idiomas

Desktop App Converter no admite Unicode; por lo tanto, no es posible usar caracteres chinos ni caracteres que no sean ASCII con la herramienta.

## <a name="see-also"></a>Consulta también

+ [Bringing Desktop Apps to the UWP Using Desktop App Converter (Convertir las aplicaciones de escritorio a UWP mediante Desktop App Converter)](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: Bringing Existing Desktop Applications to the Universal Windows Platform (Proyecto Centennial: convertir las aplicaciones de escritorio existentes a la Plataforma universal de Windows)](https://channel9.msdn.com/events/Build/2016/B829)  
