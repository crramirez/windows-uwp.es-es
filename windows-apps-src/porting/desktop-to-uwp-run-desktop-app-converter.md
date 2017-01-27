---
author: awkoren
Description: "Ejecuta Desktop Converter App para convertir una aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) en una aplicación de Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
translationtype: Human Translation
ms.sourcegitcommit: bf6da2f4d780774819fe7a4abf6367345304767c
ms.openlocfilehash: 3ffd664892fe5ee589d3bf5704e2eeed178bf5f3

---

# <a name="desktop-app-converter"></a>Desktop App Converter

[Obtener Desktop App Converter](https://aka.ms/converter)

Desktop App Converter (DAC) es una herramienta que te permite trasladar tus aplicaciones de escritorio existentes escritas para .NET 4.6.1 o Win32 a la Plataforma universal de Windows (UWP). Puedes ejecutar los instaladores de escritorio a través del convertidor en un modo de instalación desatendida (silenciosa) y obtener el paquete AppX que puedes instalar mediante el cmdlet Add-AppxPackage de PowerShell en la máquina de desarrollo.

Desktop App Converter ahora está disponible en la [Tienda Windows](https://aka.ms/converter).

El convertidor ejecuta el instalador de escritorio en un entorno de Windows aislado con una imagen base limpia proporcionada como parte de la descarga del convertidor. Captura cualquier E/S del sistema de archivos y del registro realizada por el instalador de escritorio y la empaqueta como parte de la salida. El convertidor genera un AppX con la identidad del paquete y la capacidad de llamar a una gran variedad de API de WinRT.

## <a name="whats-new"></a>Novedades

En esta sección se describen los cambios entre las versiones de Desktop App Converter. 

### <a name="12142016-v104"></a>14/12/2016 (v1.0.4)

* Mejora de la validación de la imagen base para buscar archivos .wim no válidos. 
* Corrección de errores de caracteres especiales en el parámetro ```-Publisher```. 
* Recursos actualizados.

### <a name="1122016-v101"></a>02/11/2016 (v1.0.1)

* Mejora de la validación de esquemas de manifiesto. 
* Mejora de los mensajes de error. 
* Se ha agregado la validación de las versiones compatibles de Windows. 
* Correcciones de errores para pruebas del filtro del Registro.

### <a name="9142016-v10"></a>14/09/2016 (v1.0)

* Desktop App Converter ahora está disponible para su descarga en la [Tienda Windows](https://aka.ms/converter). 
* Consigue las imágenes base de Windows 10 (.wim) más recientes en el [Centro de descarga](https://aka.ms/converterimages) para su uso con DAC.
* Con la aplicación de la Tienda, ahora puedes usar el nuevo punto de entrada *DesktopAppConverter.exe <arguments>* para ejecutar el convertidor desde cualquier lugar en un símbolo del sistema con privilegios elevados o una ventana de PowerShell.  


### <a name="922016-v0125"></a>02/09/2016 (v0.1.25)

* Se integró el paquete de NuGet dotnet-computervirtualization más reciente.
* Se agregaron las dependencias incorporadas recientemente en common.dll.
* Varias correcciones de errores.

### <a name="842016-v0124"></a>04/08/2016 (v0.1.24)

* Se agregó compatibilidad para la firma automática de las aplicaciones convertidas generadas por DAC con fines de prueba. Echa un vistazo a la marca ```–Sign``` para probarla. 
* Se agregaron advertencias si cualquiera de los registros COM del subárbol de Registro virtual no se admite en el paquete AppX.  
* Se agregó compatibilidad para la detección automática de dependencias de aplicación en bibliotecas de VC ++ y su posterior conversión a dependencias del manifiesto de AppX. Ten en cuenta que para transferir localmente y probar las aplicaciones con VC ++ en tiempo de ejecución, tendrás que descargar los paquetes de marcos VCLib según se describe en la entrada de blog [Using Visual C++ Runtime in a Centennial project (Uso de Visual C++ en tiempo de ejecución en un proyecto Centennial)](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project). Busca los paquetes en la carpeta ```Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop``` en tu máquina, navega a la versión correspondiente para ti (por ejemplo, 11.0, 12.0, 14.0) y haz doble clic en el paquete de la arquitectura adecuada (x64, x86) para instalarla.
* Se actualizó el esquema del manifiesto para alinearlo con la Actualización de aniversario de Windows 10 (10.0.14393.0). 
* Varias correcciones de errores y diseño de salida mejorado. 

### <a name="772016-v0122"></a>07/07/2016 (v0.1.22)

* Se agregó compatibilidad para detectar automáticamente las extensiones de shell de la aplicación de escritorio y declararlas en AppXManifest del paquete de UWP. Para obtener más información sobre las extensiones de escritorio, consulte [**Extensiones para aplicaciones de escritorio convertidas**](desktop-to-uwp-extensions.md). 
* Detección mejorada de AppExecutable para un gran conjunto de aplicaciones. 

### <a name="6162016-v0120"></a>16/06/2016 (v0.1.20)

* Corrige problemas que impedían las conversiones correctas en las últimas compilaciones de Windows 10 Insider Preview. 
* Se reemplazó ```–CreateX86Package``` por ```–PackageArch```, lo que permite especificar la arquitectura del paquete generado. 

### <a name="682016"></a>8/6/2016

* Se agregó soporte técnico para generar paquetes .appx x86 en equipos host AMD64 que ejecutan el convertidor.
* Se redujo el uso del espacio en disco mediante la eliminación de las imágenes base expandidas anteriormente.
* Se agregó soporte técnico para limpiar los archivos temporales y las imágenes base innecesarias.
* Se mejoró el soporte técnico para detectar asociaciones de protocolos y tipos de archivo.
* Se mejoró la lógica para detectar la propiedad AppExecutable en un gran conjunto de aplicaciones.
* Se agregó soporte técnico para proporcionar parámetros -InstallerArguments adicionales para instaladores basados en MSI.
* Correcciones de errores para todos los errores PathTooLongException durante el proceso de conversión.

### <a name="5122016"></a>12/5/2016

- Se restauró la compatibilidad de la edición profesional de Windows. 
- La marca ```-Setup``` habilita ahora la característica Contenedores de Windows y controla la expansión de la imagen base. Ejecuta lo siguiente desde un símbolo del sistema de PowerShell con privilegios elevados para realizar una instalación única: ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- Se agregó la autodetección de la ruta de instalación y el movimiento de la raíz de la aplicación fuera de VFS para reducir los redireccionamientos del sistema de archivos innecesarios en tiempo de ejecución.
- Se agregó la autodetección de la imagen base expandida como parte del proceso de conversión.
- Se agregó la detección automática de asociaciones de tipos de archivo y protocolos.
- Se mejoró la lógica para detectar el acceso directo del menú Inicio.
- Se mejoró el filtrado del sistema de archivos para conservar los archivos MUI instalados con la aplicación.
- Se actualizó la versión de escritorio mínima admitida (10.0.14342.0) en el manifiesto.

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

Desktop App Converter se basa en las características más recientes de Windows 10. Asegúrate de ejecutar la Actualización de aniversario de Windows 10 (14393.0) o compilaciones posteriores.

1.  Descarga [DesktopAppConverter de la Tienda Windows](https://aka.ms/converter) y el [archivo de imagen base .wim que coincide con tu compilación](https://aka.ms/converterimages).  
2.  Ejecuta DesktopAppConverter como administrador. Puedes hacerlo desde el menú Inicio si haces clic con el botón derecho en el icono y seleccionas *Ejecutar como administrador* en *Más*, o bien desde la barra de tareas si haces clic con el botón derecho en el icono, haces clic otra vez con el botón derecho en el nombre de la aplicación que aparece y, por último, seleccionas *Ejecutar como administrador.*
3.  En la ventana de consola de la aplicación, ejecuta ```Set-ExecutionPolicy bypass```.
4.  Configura el convertidor al ejecutar ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` desde la ventana de consola de la aplicación.
5.  Si al ejecutar los comandos anteriores se te pide reiniciar, reinicia el equipo.

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

### <a name="example"></a>Ejemplo

En el siguiente ejemplo se muestra cómo convertir una aplicación de escritorio denominada *MyApp* de *&lt;nombre_editor&gt;* a un paquete de UWP (AppX).

```CMD
DesktopAppConverter.exe -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## <a name="deploy-your-converted-appx"></a>Implementar la AppX convertida

Usa el cmdlet [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) en PowerShell para implementar un paquete de la aplicación firmado (.appx) en una cuenta de usuario. 

Puedes usar la marca ```-Sign``` en Desktop App Converter (v0.1.24) para la firma automática de la aplicación convertida. Como alternativa, consulta [Firmar la aplicación de escritorio convertida](desktop-to-uwp-signing.md) para aprender a autofirmar paquetes AppX.

También puedes usar el parámetro ```-Register``` del cmdlet Add-AppXPackage de PowerShell para instalar desde una carpeta de archivos sin empaquetar durante el proceso de desarrollo. 

Para obtener más información sobre la implementación y la depuración de la aplicación convertida, consulta [Implementar y depurar la aplicación para UWP convertida](desktop-to-uwp-deploy-and-debug.md). 

## <a name="sign-your-appx-package"></a>Firmar el paquete .appx

El cmdlet Add-AppxPackage requiere que el paquete de la aplicación (.appx) que se implemente esté firmado. Para firmar el paquete .appx, usa la marca ```-Sign``` como parte de la línea de comandos del convertidor o SignTool.exe, que se incluye en Microsoft Windows 10 SDK.

Para obtener detalles adicionales sobre cómo firmar el paquete .appx, consulta [Firmar la aplicación de escritorio convertida](desktop-to-uwp-signing.md). 

Nota: Si intentas firmar un paquete con el certificado generado automáticamente, tendrás que usar la contraseña predeterminada "123456".

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>Modificar carpeta VFS y subárbol del Registro (opcional)

Desktop App Converter adopta un enfoque muy conservador para filtrar el ruido del sistema y archivos del contenedor.  Esto no es necesario, pero después de la conversión, puedes:

1. Revisar la carpeta VFS y eliminar los archivos que el instalador no necesite.
2. Revisar el contenido de Reg.dat y eliminar las claves que no estén instaladas o que la aplicación no necesite.

Si realizas cambios en la aplicación convertida (incluidos los anteriores), no necesitas volver a ejecutar el convertidor; puedes volver a empaquetar manualmente tu aplicación con la herramienta MakeAppx y el archivo appxmanifest.xml que DAC genera para tu aplicación. Para obtener ayuda, consulta [Convertir manualmente la aplicación a UWP mediante el puente de escritorio](desktop-to-uwp-manual-conversion.md).

## <a name="caveats"></a>Advertencias

1. La compilación de Windows 10 de la máquina host debe coincidir con la imagen base que obtuviste como parte de la descarga de Desktop App Converter.  
2. Asegúrate de que el instalador de escritorio está en un directorio independiente, porque el convertidor copia todo el contenido del directorio en el entorno de Windows aislado.  
3. Actualmente, Desktop App Converter admite la ejecución del proceso de conversión solo en un sistema operativo de 64 bits. Puedes implementar los paquetes .appx convertidos solo en un sistema operativo de 64 bits (x 64).  
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

* No se podrá instalar una aplicación .NET compilada con la opción de compilación "AnyCPU" si el archivo ejecutable principal o cualquiera de las dependencias están colocados en "Archivos de programa" o "Windows\System32". Como solución, usa el instalador de escritorio específico de la arquitectura (32 o 64 bits) para generar correctamente un paquete AppX.

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
|```-Installer <String>``` | La ruta de acceso al instalador de la aplicación se debe poder ejecutar de forma desatendida o silenciosa.|
|```-InstallerArguments <String>``` [opcional] | Una lista separada por comas o una cadena de argumentos para forzar al instalador para que se ejecute de forma desatendida o silenciosa. Este parámetro es opcional si el instalador es un msi. Para obtener un registro a partir del instalador, proporciona el argumento de registro para el instalador aquí y usa la ruta de acceso ```<log_folder>```, que es un token que el convertidor reemplaza con la ruta de acceso apropiada. <br><br>**Nota: Los argumentos de registro y marcas de instalación desatendida o silenciosa varían entre las tecnologías de instalador.** <br><br>Un ejemplo de uso de este parámetro: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Otro ejemplo que no produzca un archivo de registro puede tener el siguiente aspecto: ```-InstallerArguments "/quiet", "/norestart"``` De nuevo, se deben dirigir literalmente todos los registros a la ruta de acceso de token ```<log_folder>``` si quieres que el convertidor lo capture y lo incluya en la carpeta de registro final.|
|```-InstallerValidExitCodes <Int32>``` [opcional] | Una lista separada por comas de códigos de salida que indica que el programa de instalación se ejecutó correctamente (por ejemplo: 0, 1234, 5678).  De forma predeterminada, es 0 para no msi y 0, 1641, 3010 para msi.|

### <a name="appx-identity-parameters"></a>Parámetros de identidad de AppX  

|Parámetro|Descripción|
|---------|-----------|
|```-PackageName <String>``` | El nombre del paquete de la aplicación universal de Windows
|```-Publisher <String>``` | El publicador del paquete de la aplicación universal de Windows
|```-Version <Version>``` | El número de versión del paquete de la aplicación universal de Windows

### <a name="optional-appx-manifest-parameters"></a>Parámetros opcionales de manifiesto de Appx  

|Parámetro|Descripción|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [opcional] | El nombre del archivo ejecutable principal de la aplicación (por ejemplo, "MyApp.exe"). |
|```-AppFileTypes <String>``` [opcional] | Una lista separada por comas de tipos de archivo a los que se asociará la aplicación (p. ej. ".txt, .doc", sin las comillas).|
|```-AppId <String>``` [opcional] | Especifica un valor para establecer el id. de la aplicación en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|```-AppDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar de la aplicación en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|```-AppDescription <String>``` [opcional] | Especifica un valor para establecer la descripción de la aplicación en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|```-PackageDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar del paquete en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [opcional] | Especifica un valor para establecer el nombre para mostrar del publicador del paquete en el manifiesto de appx. Si no se especifica, se establecerá en el valor pasado para *Publisher*. |

### <a name="other-conversion-parameters"></a>Otros parámetros de conversión  

|Parámetro|Descripción|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [opcional] | Ruta de acceso completa a una imagen base ya expandida.|
|```-MakeAppx [<SwitchParameter>]``` [opcional] | Un modificador que, cuando está presente, indica a este script que llame a MakeAppx en la salida. |
|```-LogFile <String>``` [opcional] | Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro. |
| ```Sign [<SwitchParameter>] [optional]``` | Indica a este script que firme el paquete AppX de salida. Este parámetro debe aparecer junto con el modificador ```-MakeAppx```. 
|```<Common parameters>``` | Este cmdlet admite los parámetros comunes: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* y *OutVariable*. Para obtener más información, consulta [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

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

Durante el proceso de conversión, DesktopAppConverter ejecuta automáticamente PEHeaderCertFixTool para corregir cualquier encabezado PE dañado. Sin embargo, también puedes ejecutar PEHeaderCertFixTool en un appx de UWP, en archivos sueltos o un binario concreto. Ejemplo de uso: 

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


<!--HONumber=Dec16_HO3-->


