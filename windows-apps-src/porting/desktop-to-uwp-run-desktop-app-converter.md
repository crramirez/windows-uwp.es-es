---
author: normesta
Description: Run the Desktop Converter App to package a Windows desktop application (like Win32, WPF, and Windows Forms).
Search.Product: eADQiWindows 10XVcnh
title: Empaquetar una aplicación con Desktop App Converter (Puente de dispositivo de escritorio)
ms.author: normesta
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: bde2e58934e24df7db2cb77fb793106aa65e3834
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4619898"
---
# <a name="package-a-desktop-application-using-the-desktop-app-converter"></a>Empaquetar una aplicación de escritorio con Desktop App Converter

[Obtener Desktop App Converter](https://aka.ms/converter)

Puedes usar Desktop App Converter (DAC) para llevar las aplicaciones de escritorio a la Plataforma universal de Windows (UWP). Esto incluye aplicaciones de Win32 y aplicaciones que hayas creado mediante .NET 4.6.1.

![Icono de DAC](images/desktop-to-uwp/dac.png)

Aunque el término "Converter" aparece en el nombre de esta herramienta, realmente no convierte la aplicación. La aplicación no se modifica. Sin embargo, estar herramienta genera un paquete de la aplicación de Windows con una identidad del paquete y la capacidad de llamar a una gran variedad de API de WinRT.

Puedes instalar dicho paquete usando el cmdlet de PowerShell Add-AppxPackage en el equipo de desarrollo.

El convertidor ejecuta el instalador de escritorio en un entorno de Windows aislado mediante una imagen base limpia proporcionada como parte de la descarga del convertidor. Captura cualquier E/S del sistema de archivos y del registro realizada por el instalador de escritorio y la empaqueta como parte de la salida.

>[!IMPORTANT]
>La capacidad para crear un paquete de aplicación de Windows para la aplicación de escritorio (de lo contrario se conoce como el puente de escritorio, se introdujo en Windows 10, versión 1607, y solo se puede usar en proyectos destinados a la actualización de aniversario de Windows 10 (10.0; Compilación 14393) o una versión posterior de Visual Studio.

> [!NOTE]
> Consulta <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">esta serie</a> de vídeos cortos que publicó Microsoft Virtual Academy. Estos vídeos explican algunas de las formas más habituales de usar Desktop App Converter.

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>DAC hace más que simplemente generar un paquete para ti

Aquí te mostramos algunas de las cosas que puede hacer.

**Windows 10 Creators Update**

:heavy_check_mark: registra automáticamente los controladores de vista previa, los de miniatura, los de propiedad, las reglas del firewall y los marcadores de dirección URL.

:heavy_check_mark: registra automáticamente las asignaciones de tipo de archivo que permiten a los usuarios agrupar archivos mediante la columna **Kind** del Explorador de archivos.

:heavy_check_mark: registra los servidores COM públicos.

**Actualización de aniversario de Windows 10 o posterior**

:heavy_check_mark: firma el paquete automáticamente para que puedas probar la aplicación.

: heavy_check_mark: valida la aplicación con la aplicación empaquetada y requisitos de Microsoft Store.

Para obtener una lista completa de estas opciones, consulta la sección [Parámetros](#command-reference) de esta guía.

Si estás listo para crear tu paquete, comencemos.

## <a name="first-prepare-your-application"></a>Primero, prepara tu aplicación

Revisar esta guía antes de empezar a crear un paquete de la aplicación: [Preparar para empaquetar una aplicación de escritorio](desktop-to-uwp-prepare.md).

## <a name="make-sure-that-your-system-can-run-the-converter"></a>Asegúrate de que el sistema puede ejecutar el convertidor

Asegúrate de que el sistema cumple los requisitos siguientes:

* Edición Pro o Enterprise de Actualización de aniversario de Windows 10 (10.0.14393.0 y posterior).
* Procesador de 64 bits (x64)
* Virtualización asistida por hardware
* Traducción de direcciones de segundo nivel (SLAT)
* [Kit de desarrollo de software de Windows (SDK) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375).

## <a name="start-the-desktop-app-converter"></a>Inicia Desktop App Converter

1. Descarga e instala [Desktop App Converter](https://aka.ms/converter).

2. Ejecuta Desktop App Converter como administrador.

    ![ejecuta DAC como administrador](images/desktop-to-uwp/run-converter.png)

   Aparecerá una ventana de consola. Usarás esa ventana de consola para ejecutar comandos.

## <a name="set-a-few-things-up-apps-with-installers-only"></a>Configura algunas cosas (solo para aplicaciones que tengan instaladores)

Puedes omitir pasar a la siguiente sección si la aplicación no tiene un instalador.

1. Identifica el número de versión del sistema operativo.

   Para ello, escribe **winver** en el cuadro de diálogo **Ejecutar** y luego elige el botón **Aceptar**.

   ![winver](images/desktop-to-uwp/winver.png)

   Encontrarás la versión de la compilación de Windows en el cuadro de diálogo **Acerca de Windows**.

   ![Versión de Windows 10](images/desktop-to-uwp/win-10-version.png)

2. Descarga la [imagen base de Desktop App Converter](https://aka.ms/converterimages) adecuada.

   Asegúrate de que el número de versión que aparece el nombre del archivo coincide con el número de versión de la compilación de Windows.

   >[!IMPORTANT]
   > Si usas el número de compilación **15063**, y la versión secundaria de esa compilación es mayor o igual a **.483** (por ejemplo: **15063.540**), asegúrate de descargar el archivo **BaseImage-15063-UPDATE.wim**. Si la versión secundaria de esa compilación es menor que **.483**, descarga el archivo **BaseImage 15063.wim**. Si ya has instalado una versión compatible de este archivo base, puedes corregirlo. En esta [entrada de blog](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/) se explica cómo hacerlo.

3. Coloca el archivo descargado en algún lugar del equipo donde puedas encontrarlo más tarde.

4. En la ventana de consola que apareció cuando iniciaste Desktop App Converter, ejecuta el siguiente comando: ```Set-ExecutionPolicy bypass```.
5. Configura el convertidor mediante la ejecución de este comando: ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```.
6. Si se te solicita, reinicia el equipo.

   Aparecerán mensajes de estado en la ventana de consola a medida que el convertidor amplía la imagen base. Si no ves los mensajes de estado, presiona cualquier tecla. Es posible que esta acción cause que el contenido de la ventana de consola se actualice.

   ![mensajes de estado en la ventana de consola](images/desktop-to-uwp/bas-image-setup.png)

   Cuando la imagen base se amplíe totalmente, ve a la siguiente sección.

## <a name="package-an-app"></a>Empaquetar una aplicación

Para empaquetar la aplicación, ejecuta el comando ``DesktopAppConverter.exe`` en la ventana de consola que se abrió cuando iniciaste Desktop App Converter.  

Especificará el número de nombre, el publicador y la versión del paquete de la aplicación mediante el uso de parámetros.

> [!NOTE]
> Si has reservado el nombre de la aplicación en la Store Windows, puedes obtener los nombres del paquete y el publicador mediante el panel del Centro de desarrollo de Windows. Si vas a transferir localmente la aplicación a otros sistemas, puedes proporcionar tus propios nombres a estos elementos, siempre y cuando el nombre del publicador que elijas coincida con el nombre que aparece en el certificado que uses para firmar la aplicación.

### <a name="a-quick-look-at-command-parameters"></a>Un vistazo rápido a los parámetros de comando

Estos son los parámetros necesarios.

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
Puedes leer acerca de cada uno de ellos [aquí](#command-reference).

### <a name="examples"></a>Ejemplos

Aquí te mostramos algunas de las formas más habituales de empaquetar la aplicación.

* [Empaquetar una aplicación que tiene un archivo de instalador (.msi)](#installer-conversion)
* [Empaquetar una aplicación que tiene un archivo ejecutable de instalación](#setup-conversion)
* [Empaquetar una aplicación que no tiene un instalador](#no-installer-conversion)
* [Empaquetar una aplicación, firmarla y prepararla para su envío a la Store.](#optional-parameters)

<a id="installer-conversion" />

#### <a name="package-an-application-that-has-an-installer-msi-file"></a>Empaquetar una aplicación que tiene un archivo de instalador (.msi)

Selecciona el archivo instalador mediante el parámetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> Hay dos cosas importantes que hay que tener en cuenta aquí. En primer lugar, asegúrate de que el programa de instalación se encuentra en una carpeta independiente y que solo aquellos archivos relacionados con el instalador están en esa carpeta. El convertidor copiará todo el contenido de esa carpeta en el entorno aislado de Windows. <br> En segundo lugar, si el centro de desarrollo asigna una identidad al paquete que comienza por un número, asegúrate también de pasar el parámetro <i>- AppId</i> y usa solo el sufijo de cadena (después del separador de punto) como el valor de dicho parámetro.  

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

Si el instalador incluye instaladores para marcos o bibliotecas dependientes, es posible que debas organizar las cosas de una manera un poco diferente. Consulta [Encadenamiento de varios instaladores con el Puente de dispositivo de escritorio](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/).

<a id="setup-conversion" />

#### <a name="package-an-application-that-has-a-setup-executable-file"></a>Empaquetar una aplicación que tiene un archivo ejecutable de instalación

Selecciona el archivo ejecutable de instalación mediante el parámetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```
>[!IMPORTANT]
>Si el centro de desarrollo asigna una identidad al paquete que comienza por un número, asegúrate también de pasar el parámetro <i>- AppId</i> y usa solo el sufijo de cadena (después del separador de punto) como el valor de dicho parámetro.

El parámetro ``InstallerArguments`` es opcional. Sin embargo, dado que Desktop App Converter necesita el programa de instalación se ejecute en modo de instalación desatendida, es posible que tienes que usarlo si la aplicación necesita marcas silenciosas para ejecutar de forma silenciosa. La marca ``/S`` es un indicador "Silent" muy común, pero es posible que la marca que uses sea diferente en función de qué tecnología de instalador usaste para crear el archivo de instalación.

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="no-installer-conversion" />

#### <a name="package-an-application-that-doesnt-have-an-installer"></a>Empaquetar una aplicación que no tiene un instalador

En este ejemplo, usa el ``Installer`` parámetro para que apunte a la carpeta raíz de los archivos de la aplicación.

Usa el parámetro `AppExecutable` para seleccionar el archivo ejecutable de la aplicación.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>Si el centro de desarrollo asigna una identidad al paquete que comienza por un número, asegúrate también de pasar el parámetro <i>- AppId</i> y usa solo el sufijo de cadena (después del separador de punto) como el valor de dicho parámetro.

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="optional-parameters" />

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>Empaquetar una aplicación, firmarla y ejecutar comprobaciones de validación en el paquete

En este ejemplo es similar al primero, salvo que aquí se muestra cómo firmar la aplicación para realizar la prueba local y validarla en la aplicación con la aplicación empaquetada y requisitos de Microsoft Store.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>Si el centro de desarrollo asigna una identidad al paquete que comienza por un número, asegúrate también de pasar el parámetro <i>- AppId</i> y usa solo el sufijo de cadena (después del separador de punto) como el valor de dicho parámetro.

El ``Sign`` parámetro genera un certificado y, a continuación, inicia la aplicación con él. Para ejecutar la aplicación, tendrás que instalar el certificado generado. Para obtener información sobre cómo hacerlo, consulta la sección [Ejecutar la aplicación empaquetada](#run-app) de esta guía.

Puedes validar la aplicación mediante el uso de la ``Verify`` parámetro.

### <a name="a-quick-look-at-optional-parameters"></a>Un vistazo rápido a los parámetros opcionales

Los parámetros ``Sign`` y ``Verify`` son opcionales. Hay muchos más parámetros opcionales.  Estos son algunos de los parámetros opcionales que se usan con mayor frecuencia.

```CMD
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
Puedes leer acerca de todos ellos en la siguiente sección.

<a id="command-reference" />

### <a name="parameter-reference"></a>Referencia del parámetro

Esta es la lista completa de los parámetros (organizada por categorías) que puedes usar al ejecutar Desktop App Converter.

* [Programa de instalación](#setup-params)
* [Conversión](#conversion-params)
* [Identidad del paquete](#identity-params)
* [Manifiesto de paquete](#manifest-params)
* [Limpieza](#cleanup-params)
* [Arquitectura](#architecture-params)
* [Varios](#other-params)

También puedes ver toda la lista ejecutando el comando ``Get-Help`` en la ventana de consola de la aplicación.   

||||
|-------------|-----------|-------------|
|<a id="setup-params" /> <strong>Parámetros de configuración</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |Necesario |Ejecuta DesktopAppConverter en modo de instalación. El modo de instalación admite la expansión de una imagen base proporcionada.|
|-BaseImage &lt;String&gt; | Necesario |Ruta de acceso completa a una imagen base no expandida. Este parámetro es necesario si se especifica el programa de instalación.|
| -LogFile &lt;String&gt; |Opcional |Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro.|
|-NatSubnetPrefix &lt;String&gt; |Opcional |Valor del prefijo que se usará para la instancia de Nat. Por lo general, se recomienda que lo cambies solo si el equipo host se adjunta al mismo intervalo de subred que el NetNat del convertidor. Puedes consultar la configuración NetNat actual del convertidor mediante el cmdlet **Get-NetNat**. |
|-NoRestart [&lt;SwitchParameter&gt;] |Necesario |No solicites el reinicio mientras se ejecuta la instalación (es necesario reiniciar el sistema para habilitar la función de contenedor). |
|<a id="conversion-params" /> <strong>Parámetros de conversión</strong>|||
|-AppInstallPath &lt;String&gt;  |Opcional |Ruta de acceso completa a la carpeta raíz de la aplicación para los archivos instalados si se hubiera instalado la aplicación (por ejemplo, "C:\Archivos de programa (x86)\MyApp").|
|-Destination &lt;String&gt; |Necesario |El destino deseado para la salida de appx del convertidor: DesktopAppConverter puede crear esta ubicación si ya no existe.|
|-Installer &lt;String&gt; |Necesario |La ruta de acceso al instalador de la aplicación se debe poder ejecutar de forma desatendida o silenciosa. Conversión sin instalador, se trata de la ruta de acceso al directorio raíz de los archivos de la aplicación. |
|-InstallerArguments &lt;String&gt; |Opcional |Es una lista separada por comas o una cadena de argumentos para forzar al instalador a que se ejecute de forma desatendida o silenciosa. Este parámetro es opcional si el instalador es un msi. Para obtener un registro a partir del instalador, proporciona aquí el argumento de registro del instalador y usa la ruta de acceso &lt;log_folder&gt;, que es un token que el convertidor reemplaza con la ruta de acceso apropiada. <br><br>**NOTA**: los argumentos de registro y marcas de instalación desatendida o silenciosa varían entre las tecnologías de instalador. <br><br>Un ejemplo de uso de este parámetro: -InstallerArguments "/silent /log &lt;log_folder&gt;\install.log" Otro ejemplo que no produzca un archivo de registro puede tener el siguiente aspecto: ```-InstallerArguments "/quiet", "/norestart"``` De nuevo, se deben dirigir literalmente todos los registros a la ruta de acceso del token &lt;log_folder&gt; si quieres que el convertidor lo capture y lo incluya en la carpeta de registro final.|
|-InstallerValidExitCodes &lt;Int32&gt; |Opcional |Es una lista separada por comas de códigos de salida que indica que el programa de instalación se ejecutó correctamente (por ejemplo: 0, 1234, 5678).  De forma predeterminada, es 0 si no hay archivos msi y 0, 1641, 3010 si hay archivos msi.|
|-MakeAppx [&lt;SwitchParameter&gt;]  |Opcional |Es un modificador que, cuando está presente, indica a este script que llame a MakeAppx en la salida. |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |Opcional |Modificador que, cuando está presente, indica a este script para empaquetar la salida como un paquete de MSIX. |
|<a id="identity-params" /><strong>Parámetros de identidad de paquete</strong>||
|-PackageName &lt;String&gt; |Requerido |El nombre del paquete de la aplicación universal de Windows. Si el centro de desarrollo asigna una identidad al paquete que comienza por un número, asegúrate también de pasar el parámetro <i>- AppId</i> y usa solo el sufijo de cadena (después del separador de punto) como el valor de dicho parámetro. |
|-Publisher &lt;String&gt; |Necesario |El publicador del paquete de la aplicación universal de Windows |
|-Version &lt;Version&gt; |Necesario |El número de versión del paquete de la aplicación universal de Windows |
|<a id="manifest-params" /><strong>Parámetros del manifiesto de paquete</strong>||
|-AppExecutable &lt;String&gt; |Opcional |El nombre del archivo ejecutable principal de la aplicación (por ejemplo, "MyApp.exe"). Este parámetro es obligatorio para realizar una conversión sin instalador. |
|-AppFileTypes &lt;String&gt;|Opcional |Una lista separada por comas de tipos de archivo a los que se asociará la aplicación. Ejemplo de uso: -AppFileTypes "'.md', '.markdown'".|
|-AppId &lt;String&gt; |Opcional |Especifica un valor para establecer el identificador de la aplicación en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*. En muchos casos, usar *PackageName* está bien. No obstante, si el centro de desarrollo asigna una identidad al paquete que comienza por un número, asegúrate también de pasar el parámetro <i>- AppId</i> y usa solo el sufijo de cadena (después del separador de punto) como el valor de dicho parámetro. |
|-AppDisplayName &lt;String&gt;  |Opcional |Especifica un valor para establecer el nombre para mostrar de la aplicación en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|-AppDescription &lt;String&gt; |Opcional |Especifica un valor para establecer la descripción de la aplicación en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*.|
|-PackageDisplayName &lt;String&gt; |Opcional |Especifica un valor para establecer el nombre para mostrar del paquete en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *PackageName*. |
|-PackagePublisherDisplayName &lt;String&gt; |Opcional |Especifica un valor para establecer el nombre para mostrar del editor del paquete en el manifiesto del paquete de la aplicación de Windows. Si no se especifica, se establecerá en el valor pasado para *Publisher*. |
|<a id="cleanup-params" /><strong>Parámetros de limpieza</strong>|||
|-Cleanup [&lt;Option&gt;] |Necesario |Ejecuta la limpieza de los artefactos de DesktopAppConverter. Hay 3 opciones válidas para el modo de limpieza. |
|-Cleanup All | |Elimina todas las imágenes base expandidas, quita todos los archivos temporales del convertidor, quita la red de contenedores y deshabilita la característica Contenedores opcional de Windows. |
|-Cleanup WorkDirectory |Necesario |Quita todos los archivos temporales del convertidor. |
|-Cleanup ExpandedImage |Necesario |Elimina todas las imágenes base expandidas instaladas en la máquina host. |
|<a id="architecture-params" /><strong>Parámetros de arquitectura del paquete</strong>|||
|-PackageArch &lt;String&gt; |Necesario |Genera un paquete con la arquitectura especificada. Las opciones válidas son 'x86' o 'x64'; por ejemplo, -PackageArch x86. Este parámetro es opcional. Si no se especifica, DesktopAppConverter intentará detectar automáticamente la arquitectura del paquete. Si se produce un error en la detección automática, se establece el paquete x64 de forma predeterminada. |
|<a id="other-params" /><strong>Varios parámetros</strong>|||
|-ExpandedBaseImage &lt;String&gt;  |Opcional |Ruta de acceso completa a una imagen base ya expandida.|
|-LogFile &lt;String&gt;  |Opcional |Especifica un archivo de registro. Si se omite, se creará una ubicación temporal del archivo de registro. |
| -Sign [&lt;SwitchParameter&gt;] |Opcional |Indica a este script que debe firmar la salida del paquete de la aplicación de Windows mediante un certificado generado con fines de prueba. Este parámetro debe aparecer junto con el modificador ```-MakeAppx```. |
|&lt;Parámetros comunes&gt; |Necesario |Este cmdlet admite los parámetros comunes: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* y *OutVariable*. Para obtener más información, consulta [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |
| -Verify [&lt;SwitchParameter&gt;] |Opcional |Modificador que, cuando está presente, indica a DAC para validar el paquete de la aplicación con la aplicación empaquetada y requisitos de Microsoft Store. El resultado es un informe de validación "VerifyReport.xml" que se visualiza mejor en un navegador. Este parámetro debe aparecer junto con el modificador `-MakeAppx`. |
|-PublishComRegistrations| Opcional| Analiza todos los registros COM públicos que realizó el instalador y publica aquellos que son válidos en el manifiesto. Usa esta marca solo si quieres que estos registros estén disponibles en otras aplicaciones. No debes usar este indicador si solo la aplicación usa estos registros. <br><br>Lee [este artículo](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97) para asegurarte de que los registros COM se comportan tal y como esperas después de empaquetar la aplicación.

<a id="run-app" />

## <a name="run-the-packaged-app"></a>Ejecutar la aplicación empaquetada

Hay dos formas de ejecutar la aplicación.

Una manera es abrir un símbolo del sistema de PowerShell y escribir el siguiente comando: ```Add-AppxPackage –Register AppxManifest.xml```. Es probablemente la forma más sencilla de ejecutar la aplicación porque no tienes que iniciar sesión.

Otra manera es firmar la aplicación con un certificado. Si usas el ```sign``` parámetro, Desktop App Converter generará uno automáticamente y, a continuación, firmar la aplicación con él. Este archivo se llama **auto-generated.cer** y lo encontrarás en la carpeta raíz de la aplicación empaquetada.

Sigue estos pasos para instalar el certificado generado y así ejecutar la aplicación.

1. Haz doble clic en el archivo **auto-generated.cer** para instalar el certificado.

   ![archivo de certificado generado](images/desktop-to-uwp/generated-cert-file.png)

   > [!NOTE]
   > Si se te pide una contraseña, usa la contraseña predeterminada "123456".

2. En el cuadro de diálogo **Certificado**, haz clic en el botón **Instalar certificado**.
3. En el **Asistente para la importación de certificados**, instala el certificado en la **máquina local** y colócalo en el almacén de certificados **Personas de confianza**.

   ![Almacén Personas de confianza](images/desktop-to-uwp/trusted-people-store.png)

5. En la carpeta raíz de la aplicación empaquetada, haz doble clic en el archivo de paquete de la aplicación de Windows.

   ![Archivo del paquete de la aplicación de Windows](images/desktop-to-uwp/windows-app-package.png)

6. Instala la aplicación para ello, haz clic en el botón **Instalar**.

   ![botón Instalar](images/desktop-to-uwp/install.png)


## <a name="modify-the-packaged-app"></a>Modificar la aplicación empaquetada

Es probable que realizaremos cambios en la aplicación empaquetada para solucionar errores, agregar activos visuales o mejorar tu aplicación experiencias tan modernas como iconos dinámicos.

Después de realizar los cambios, no tienes que volver a ejecutar el convertidor. En la mayoría de los casos, volver a empaquetar la aplicación mediante la herramienta MakeAppx y el archivo appxmanifest.xml que DAC genera para tu aplicación. Consulta [Generar un paquete de aplicación de Windows](desktop-to-uwp-manual-conversion.md#make-appx).

* Si modificas cualquiera de los activos visuales de la aplicación, genera un nuevo archivo de índice de recursos del paquete y después ejecuta la herramienta MakeAppx para generar un nuevo paquete. Consulta [Generar un archivo de índice de recursos del paquete (PRI)](desktop-to-uwp-manual-conversion.md#make-pri).

* Si quieres agregar iconos o ventanas dinámicas para que aparezcan en la barra de tareas de Windows, la vista de tareas, ALT+TAB, el asistente para el espacio restante y la esquina inferior derecha de las ventana de Inicio, consulta [(Opcional) Agregar los activos sin placa basados en destino](desktop-to-uwp-manual-conversion.md#target-based-assets).

> [!NOTE]
> Si realizas cambios en la configuración del registro que creó el instalador, tendrás que volver a ejecutar Desktop App Converter para detectar esos cambios.

**Vídeos**

|Modificar y volver a empaquetar la salida |Demo: modificar y volver a empaquetar la salida|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

Las dos siguientes secciones describen un par de correcciones opcionales para la aplicación empaquetada que podría tener en cuenta.

### <a name="delete-unnecessary-files-and-registry-keys"></a>Eliminar los archivos innecesarios y las claves de registro

Desktop App Converter adopta un enfoque muy conservador para filtrar el ruido del sistema y los archivos del contenedor.

Si quieres, puedes revisar la carpeta VFS y eliminar los archivos que no necesite el programa de instalación.  Asimismo, puedes revisar el contenido de Reg.dat y eliminar las claves que no estén instaladas o que la aplicación no necesite.

### <a name="fix-corrupted-pe-headers"></a>Corregir encabezados PE dañados

Durante el proceso de conversión, DesktopAppConverter ejecuta automáticamente PEHeaderCertFixTool para corregir cualquier encabezado PE dañado. Sin embargo, también puedes ejecutar PEHeaderCertFixTool en un paquete de la aplicación de Windows de UWP, en archivos sueltos o en un binario concreto. Aquí tienes un ejemplo.

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="telemetry-from-desktop-app-converter"></a>Telemetría de Desktop App Converter

Desktop App Converter puede recopilar información sobre ti y sobre el uso del software, y enviarla a Microsoft. Puedes obtener más información sobre la recopilación de datos de Microsoft y usarla en la documentación del producto y en la [Declaración de privacidad de Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Te comprometes a cumplir con todas las disposiciones aplicables de la Declaración de privacidad de Microsoft.

De manera predeterminada, la telemetría se habilitará en Desktop App Converter. Agrega la siguiente clave del registro para establecer la telemetría en una configuración deseada:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Agrega o edita el valor *DisableTelemetry* mediante un DWORD establecido en 1.
+ Para habilitar la telemetría, quita la clave o establece el valor en 0.

### <a name="language-support"></a>Compatibilidad con idiomas

Desktop App Converter no admite Unicode; por lo tanto, no es posible usar caracteres chinos ni caracteres que no sean ASCII con la herramienta.

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

También puedes consultar [esta](desktop-to-uwp-known-issues.md#app-converter) lista de problemas conocidos.

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Ejecutar la aplicación o buscar y corregir problemas**

Consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](desktop-to-uwp-debug.md)

**Distribuir la aplicación**

Consulta [distribuir una aplicación de escritorio empaquetada](desktop-to-uwp-distribute.md)
