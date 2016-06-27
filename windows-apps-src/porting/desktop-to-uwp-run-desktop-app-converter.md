---
author: awkoren
Description: "Ejecuta Desktop Converter App para convertir una aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) en una aplicación de la Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Vista previa de Desktop App Converter (Project Centennial)
ms.sourcegitcommit: 6d1c6e836d666972641320c73896459490f45924
ms.openlocfilehash: 874b6452386526d66062a27a5b520cb1a232ae64

---

# Vista previa de Desktop App Converter (Project Centennial)

\[Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.\]

[Obtener Desktop App Converter.](http://go.microsoft.com/fwlink/?LinkId=785437)

Desktop App Converter es una herramienta de versión preliminar que te permite llevar tus aplicaciones de escritorio existentes que se escribieron para .NET 4.6.1 o Win32 a la Plataforma universal de Windows (UWP). Puedes ejecutar los instaladores de escritorio a través del convertidor en un modo de instalación desatendida (silenciosa) y obtener el paquete AppX que puedes instalar mediante el cmdlet de PowerShell Add-AppxPackage en el equipo de desarrollo.

El convertidor ejecuta al instalador de escritorio en un entorno de Windows aislado con una imagen de base limpia proporcionada como parte de la descarga del convertidor. Captura cualquier E/S del sistema de archivos y del registro realizada por el instalador de escritorio y la empaqueta como parte de la salida. El convertidor genera un AppX con la identidad del paquete y la capacidad de llamar a una gran variedad de API de WinRT.

## Novedades

En esta sección se describen los cambios entre las versiones de Desktop App Converter. 

### 8/6/2016

* Se agregó soporte técnico para generar paquetes appx x86 en equipos host AMD64 que ejecutan el convertidor.
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
- Se actualizó la versión mínima de escritorio admitida (10.0.14342.0) para Project Centennial en el manifiesto.

## Requisitos del sistema

### Sistema operativo compatible
+ Versión preliminar de la Actualización de aniversario de Windows 10 versión Enterprise (compilación 10.0.14342.0 y posterior)

### Configuración del hardware necesario

El equipo debe tener las siguientes funcionalidades mínimas:
+ procesador de 64 bits (x64)
+ Virtualización asistida por hardware
+ Traducción de direcciones de segundo nivel (SLAT)

### Recursos recomendados
+ [Kit de desarrollo de software de Windows (SDK) para Windows 10](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)

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
Usa el cmdlet [Add-AppxPackage](https://technet.microsoft.com/en-us/library/hh856048.aspx) en PowerShell para implementar un paquete de la aplicación firmado (.appx) en una cuenta de usuario. Para firmar el paquete .appx, consulta la siguiente sección, "Firma el paquete AppX". Además, puedes incluir el parámetro *Register* del cmdlet para instalar desde una carpeta de archivos desempaquetados durante el proceso de desarrollo. Para obtener más información, consulta [Implementa y depura tu aplicación para UWP convertida](desktop-to-uwp-deploy-and-debug.md).

## Firma el paquete .Appx

El cmdlet Add-AppxPackage requiere que el paquete de la aplicación (.appx) que se implemente esté firmado. Usa SignTool.exe, que se incluye en el SDK de Microsoft Windows 10, para firmar el paquete .appx.

### Ejemplo
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```
**Nota:** Cuando ejecutes MakeCert.exe y se te pida que escribas una contraseña, selecciona **Ninguno**.

Para obtener más información sobre los certificados y las firmas, consulta:

+ [Cómo crear certificados temporales para su uso durante el desarrollo](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### Advertencias
1. La compilación de Windows 10 en el equipo host debe coincidir con la imagen base que obtuviste como parte de la descarga de Desktop App Converter.  
2. Asegúrate de que el instalador de escritorio está en un directorio independiente, porque el convertidor copia todo el contenido del directorio en el entorno de Windows aislado.  
3. Actualmente, Desktop App Converter admite la ejecución del proceso de conversión solo en un sistema operativo de 64 bits. Puedes implementar los paquetes .appx convertidos solo en un sistema operativo de 64 bits (x 64).  
4. Desktop App Converter requiere el instalador de escritorio para ejecutarse en modo de instalación desatendida. Asegúrate de que pasas el indicador silent para el instalador en el convertidor mediante el parámetro *-InstallerArguments*.
5. La publicación de ensamblados SxS Fusion públicos no funcionará. Durante la instalación, una aplicación puede publicar ensamblados Fusion públicos en paralelo, accesibles para cualquier otro proceso. Durante la creación de contexto de activación de proceso, estos ensamblados se recuperan mediante un proceso del sistema denominado CSRSS.exe. Cuando esto se hace en un proceso Centennial, se producirá un error en la creación de contexto de activación y en la carga del módulo de estos ensamblados. Los ensamblados de bandeja de entrada, como ComCtl, se envían con el sistema operativo, por lo que llevar una dependencia a estos desde procesos de Centennial es seguro. Los ensamblados SxS Fusion están registrados en las siguientes ubicaciones:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de archivos: %windir%\\SideBySide

## Problemas conocidos

+ Si recibes un paquete piloto de Windows Insider en un equipo de desarrollo que anteriormente tenía la vista previa de Desktop App Converter instalada, puede que recibas el error `New-ContainerNetwork: The object already exists` al instalar la nueva imagen base. Como solución, ejecuta el comando `Netsh int ipv4 reset` desde un símbolo del sistema con privilegios elevados y, después, reinicia el equipo. 
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
|```-AppExecutable <String>``` [opcional] | La ruta de acceso completa del archivo ejecutable principal de la aplicación si tuviera que instalarse (no es obligatorio que así sea), por ejemplo, "C:\Archivos de programa (x86)\MyApp\MyApp.exe".|
|```-AppFileTypes <String>``` [opcional] | Una lista separada por comas de tipos de archivo a los que la aplicación se asociará (por ej. ".txt, .doc", sin las comillas).|
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
|```<Common parameters>``` | Este cmdlet admite los parámetros comunes: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* y *OutVariable*. Para obtener más información, consulta [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### Parámetros de limpieza
|Parámetro|Descripción|
|---------|-----------|
|```Cleanup [<Option>]``` | Ejecuta la limpieza de los artefactos de DesktopAppConverter. Hay 3 opciones válidas para el modo de limpieza. |
|```Cleanup All``` | Elimina todas las imágenes base expandidas, quita todos los archivos temporales del convertidor, quita la red de contenedores y deshabilita la característica Contenedores opcional de Windows. |
|```Cleanup WorkDirectory``` | Quita todos los archivos temporales del convertidor. |
|```Cleanup ExpandedImages``` | Elimina todas las imágenes base expandidas instaladas en el equipo host. |

### Parámetros de paquetes x86
La vista previa de Desktop App Converter admite ahora la creación de paquetes de la aplicación x86 que puedes instalar y ejecutar en equipos x86 y amd64. Ten en cuenta que Desktop App Converter debe seguir ejecutándose en un equipo AMD64 para realizar una conversión correcta.

|Parámetro|Descripción|
|---------|-----------|
|```-CreateX86Package[<SwitchParameter>]``` | Genera un paquete de 32 bits que se pueda instalar y ejecutar en ambos sistemas operativos host de 32 bits y 64 bits. El convertidor intenta detectar arquitectura del paquete del archivo ejecutable de la aplicación o usa de manera predeterminada el valor 64 bits si no se encuentra ningún archivo .exe. |

## Consulta también
+ [Obtener Desktop App Converter](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [Lleva tu aplicación de escritorio a la Plataforma universal de Windows](https://developer.microsoft.com/en-us/windows/bridges/desktop)
+ [Traer las aplicaciones de escritorio a UWP mediante Desktop App Converter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: llevar las aplicaciones de escritorio existentes a la Plataforma universal de Windows](https://channel9.msdn.com/events/Build/2016/B829)  
+ [UserVoice para Puente de escritorio (Project Centennial)](http://aka.ms/UserVoiceDesktopToUwp)
+ [Puente de la aplicación de escritorio a ejemplos de código de UWP en GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Jun16_HO3-->


