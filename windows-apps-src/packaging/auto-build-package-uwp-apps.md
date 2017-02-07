---
author: rmpablos
title: "Configurar compilaciones automatizadas para la aplicación para UWP"
description: "Cómo configurar las compilaciones automatizadas para producir paquetes de instalaciones de prueba y/o paquetes de la Tienda."
translationtype: Human Translation
ms.sourcegitcommit: 1e30c20b38b9e6b00cd4f56bf9ffb3752ceed6a9
ms.openlocfilehash: 681019d708b859d117992cde6d2ad3d37fa4833a

---
# Configurar compilaciones automatizadas para la aplicación para UWP

Puedes usar Visual Studio Team Services (VSTS) para crear compilaciones automatizadas para proyectos de UWP. En este artículo, analizaremos diferentes formas de hacerlo.  También te mostraremos cómo realizar estas tareas mediante el uso de la línea de comandos para que puedas integrar con otros sistemas de compilación como AppVeyor. 

## Seleccionar el tipo correcto de agente de compilación

Elige el tipo de agente de compilación que quieres que VSTS use cuando ejecuta el proceso de compilación. Un agente de compilación hospedado se implementa con las herramientas y SDK más comunes, y funcionará para la mayoría de los escenarios, consulta el artículo [Software en el servidor de compilación hospedado](https://www.visualstudio.com/en-us/docs/build/admin/agents/hosted-pool#software). Sin embargo, puedes crear a un agente de compilación personalizado si necesitas tener más control sobre los pasos de la compilación. Puedes usar la siguiente tabla para ayudarte a tomar esa decisión.

|**Escenario**|**Agente personalizado**|**Agente de compilación hospedado**|
-------------|----------------|----------------------|
|Generación de UWP básica (incluye .NET Native)|: white_check_mark:|: white_check_mark:|
|Generar paquetes de instalación de prueba|: white_check_mark:|: white_check_mark:|
|Generar paquetes para envío a la Tienda|: white_check_mark:|: white_check_mark:|
|Usar certificados personalizados|: white_check_mark:||
|Compilación destinada a un Windows SDK personalizado|: white_check_mark:||
|Ejecutar pruebas unitarias|: white_check_mark:||
|Usar compilaciones incrementales|: white_check_mark:||

>Nota: Si tienes previsto seleccionar como destino el SDK de la Actualización de aniversario de Windows (compilación 14393) tendrás que configurar el agente de compilación personalizado, ya que el grupo de compilación hospedado solo admite el SDK 10586 y 10240. Más información para [elegir una versión de UWP](https://msdn.microsoft.com/en-us/windows/uwp/updates-and-versions/choose-a-uwp-version)

#### Crear un agente de compilación personalizado (opcional)

Si decides crear a un agente de compilación personalizado, necesitarás las herramientas de la plataforma universal de Windows. Estas herramientas forman parte de Visual Studio. Puedes usar la edición de la comunidad de Visual Studio.

Para obtener más información, consulta [Implementar un agente en Windows.](https://www.visualstudio.com/en-us/docs/build/admin/agents/v2-windows) 

Para ejecutar pruebas unitarias de UWP, tendrás que hacer lo siguiente: •   Implementar e iniciar la aplicación. •   Ejecutar el agente VSTS en modo interactivo. •   Configurar el agente para que inicie sesión automáticamente después de un reinicio.

Ahora hablaremos sobre cómo configurar una compilación automatizada.

## Configurar una compilación automatizada
Comenzaremos con la definición de compilación de UWP predeterminada que está disponible en VSTS y, a continuación, te mostraremos cómo configurar esa definición, de modo que puedas realizar tareas más avanzadas de compilación.

**Agregar el certificado de tu proyecto a un repositorio de código fuente**

VSTS funciona con repositorios de código basados en TFS y GIT.  
Si usas un repositorio de Git, agrega el archivo de certificado del proyecto al repositorio para que el agente de compilación pueda firmar el paquete appx. Si no haces esto, el repositorio de Git omitirá el archivo de certificado. Para agregar el archivo de certificado a tu repositorio, haz clic con el botón derecho en el archivo de certificado en el Explorador de soluciones y, después, en el menú contextual, elige el comando Agregar archivo omitido a control de origen. 

![cómo incluir un certificado](images/building-screen1.png)

Analizaremos la [administración avanzada de certificados](#certificates-best-practices) más adelante en esta guía. 

Para crear tu primera definición de compilación en VSTS, navega hasta la pestaña Compilaciones y selecciona el botón +.

![crear la definición de compilación](images/building-screen2.png)

En la lista de plantillas de definición de compilación, elige la plantilla *Plataforma universal de Windows*.

![seleccionar la compilación de uwp](images/building-screen3.png)

Esta definición de compilación contiene las siguientes tareas de compilación:

- Restauración de NuGet **\*.sln
- Compilar solución **\*.sln
- Publicar símbolos
- Publicar artefacto: drop

#### Configurar la tarea de compilación de restauración de NuGet

Esta tarea restaura los paquetes de NuGet definidos en el proyecto. Algunos paquetes requieren una versión personalizada de NuGet.exe. Si estás usando un paquete que requiera una, haz referencia a esa versión de NuGet.exe en tu repositorio y, a continuación, haz referencia a ella en la propiedad avanzada *Ruta de acceso a NuGet.exe*.

![definición de compilación predeterminada](images/building-screen4.png)

#### Configurar la tarea de compilación de compilación de soluciones

Esta tarea compila cualquier solución que se encuentre en la carpeta de trabajo en archivos binarios y produce el archivo AppX de salida. Esta tarea utiliza argumentos de MSBuild.  Tendrás que especificar el valor de los argumentos. Usa la siguiente tabla como guía. 

|**Argumento de MSBuild**|**Valor**|**Descripción**|
|--------------------|---------|---------------|
|AppxPackageDir|$(Build.ArtifactStagingDirectory)\AppxPackages|Define la carpeta en la que almacenar los artefactos generados.|
|AppxBundlePlatforms|$(Build.BuildPlatform)|Te permite definir las plataformas que incluir en el lote.|
|AppxBundle|Always|Crea un appxbundle con los archivos de appx para la plataforma especificada.|
|**UapAppxPackageBuildMode**|StoreUpload|Define el tipo de paquete de appx que generar. (No se incluye de manera predeterminada)|


Si quieres compilar tu solución usando la línea de comandos o cualquier otro sistema de compilación, ejecuta msbuild con estos argumentos.

```
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"  
/p:UapAppxPackageBuildMode=StoreUpload 
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Los parámetros definidos con la sintaxis $() son variables definidas en la definición de compilación y cambiarán en otros sistemas de compilación.

![variables predeterminadas](images/building-screen5.png)

Para ver todas las variables predefinidas, consulta [Usar variables de compilación.](https://www.visualstudio.com/docs/build/define/variables)

#### Configurar la tarea de compilación Publicar artefacto 
Esta tarea almacena los artefactos generados en VSTS. Puedes verlos en la pestaña Artefactos de la página de resultados de la compilación. VSTS usa la carpeta `$Build.ArtifactStagingDirectory)\AppxPackages` que hemos definido anteriormente.

![artefactos](images/building-screen6.png)

Como hemos establecido la propiedad `UapAppxPackageBuildMode` en `StoreUpload`, la carpeta artefactos incluye el paquete que cargaste en la Tienda (appxupload), así como los paquetes que habilitan la instalación de prueba (appxbundle).


>Nota: De manera predeterminada, el agente VSTS mantiene los paquetes appx generados más recientes. Si deseas almacenar solo los artefactos de la compilación actual, configura la compilación para limpiar el directorio de archivos binarios. Para ello, agrega una variable llamada `Build.Clean` y, a continuación, establécela en el valor `all`. Para obtener más información, consulta [Especificar el repositorio.](https://www.visualstudio.com/en-us/docs/build/define/repository#how-can-i-clean-the-repository-in-a-different-way)

#### Los tipos de compilaciones automatizadas
A continuación, usarás tu definición de compilación para crear una compilación automatizada. La siguiente tabla describe cada tipo de compilación automatizada que puedes crear. 

|**Tipo de compilación**|**Artefactos**|**Frecuencia recomendada**|**Descripción**|
|-----------------|------------|-------------------------|---------------|
|Integración continua|Registro de la compilación, Resultados de la prueba|Cada confirmación|Este tipo de compilación es rápido y se ejecuta varias veces al día.|
|Compilación de implementación continua para instalación de prueba|Paquetes de implementación|Diariamente |Este tipo de compilación puede incluir pruebas unitarias, pero tarda un poco más. Permite pruebas manuales y puedes integrarlo con otras herramientas como HockeyApp.|
|Compilación de implementación continua que envía un paquete a la Tienda|Publicación de paquetes|A petición|Este tipo de compilación crea un paquete que se puede publicar en la Tienda.|

Echemos un vistazo a cómo configurar cada uno de ellos.


## Configurar una compilación de integración continua (CI) 
Este tipo de una compilación te ayuda a diagnosticar rápidamente problemas relacionados con el código. Por lo general se ejecutan para una única plataforma y no necesitan ser procesado por la cadena de herramientas de .NET Native. Además, con las compilaciones de CI, puedes ejecutar pruebas unitarias que producen un informe de resultados de prueba.  

Si quieres ejecutar pruebas unitarias de UWP como parte de la compilación de CI, deberás usar a un agente de compilación personalizado en lugar de un agente de compilación hospedado.

>Nota: Si empaquetas más de una aplicación en la misma solución, es posible que recibas un error. Consulta el tema siguiente de la ayuda para solucionar dicho error: [Resolver los errores que aparecen cuando se empaqueta más de una aplicación en la misma solución.](#bundle-errors) 


### Configurar una definición de la compilación de CI
Usa la plantilla de UWP predeterminada para crear una definición de compilación. A continuación, configura el desencadenador para ejecutarla en cada comprobación.  

![desencadenador de CI](images/building-screen7.png)

Dado que la compilación de CI no se implementará en los usuarios, es una buena idea mantener diferentes números de control de versiones para evitar confusiones con las compilaciones de CD. Por ejemplo: `$(BuildDefinitionName)_0.0.$(DayOfYear)$(Rev:.r)`.


#### Configurar a un agente de compilación personalizado para las pruebas unitarias

1. En primer lugar, habilita el modo de desarrollador en tu PC. Consulta Habilitar el dispositivo para el desarrollo. 2. Habilitar el servicio para que se ejecute como un proceso interactivo. Consulta Implementar un agente en Windows. 3.Implementar el certificado de firma al agente.

Para ello, haz doble clic en el archivo .cer, elige Equipo Local y, a continuación, elige el almacén de personas de confianza.

<span id="uwp-unit-tests" />
### Configurar la definición de compilación para ejecutar pruebas unitarias de UWP
Para ejecutar una prueba unitaria, utiliza el paso de compilación de prueba de Visual Studio.


![agregar pruebas unitarias](images/building-screen8.png)

Las pruebas unitarias de UWP se ejecutan en el contexto de un archivo appx determinado, por lo que no puedes usar el paquete generado. Además, tendrás que especificar la ruta de acceso a un archivo appx de plataforma concreto. Por ejemplo:

```
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp.UnitTest\x86\MyUWPApp.UnitTest_$(AppxVersion)_x86.appx
```

>Nota: Usa el siguiente comando para ejecutar las pruebas unitarias localmente desde la línea de comandos:
`"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe"`

#### Acceder a los resultados de la prueba
En VSTS, la página de resumen de la compilación muestra los resultados de la prueba de cada compilación que ejecuta las pruebas unitarias.  Desde ahí, puedes abrir la página Resultados de pruebas para ver más detalles sobre los resultados de la prueba. 

![resultados de la prueba](images/building-screen9.png)

#### Mejorar la velocidad de una compilación de CI
Si quieres usar la compilación de CI únicamente para supervisar la calidad de los registros, puedes reducir los tiempos de compilación.

#### Para mejorar la velocidad de una compilación de CI
1.  Realiza la compilación para una única plataforma.
2.  Edita la variable BuildPlatform para usar solo x86. ![configurar ci](images/building-screen10.png) 
3.  En el paso de la compilación, agrega /p:AppxBundle=Never a la propiedad Argumentos de MSBuild y, a continuación, establece la propiedad Plataforma. ![configurar plataforma](images/building-screen11.png)
4.  En el proyecto de pruebas unitarias, deshabilita .NET Native. 

Para ello, abre el archivo de proyecto y, en las propiedades del proyecto, establece la propiedad `UseDotNetNativeToolchain` en `false`.

>Nota. El uso de la cadena de herramientas de .NET Native es aún una parte importante del flujo de trabajo de forma que debes usarla para probar las compilaciones de versiones. 

<span id="bundle-errors" />
#### Resolver los errores que aparecen cuando se empaqueta más de una aplicación en la misma solución 
Si agregas más de un proyecto de UWP a tu solución y, después, vuelves a intentar crear un lote, es posible que recibas un error como este: 

```
MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle
```

Este error aparece porque en el nivel de la solución, no está claro qué aplicación debería aparecer en el lote. Para resolver este problema, abre cada archivo de proyecto y agrega las siguientes propiedades al final del primer elemento `<PropertyGroup>`:

|**Proyecto**|**Propiedades**|
|-------|----------|
|Aplicación|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

A continuación, quite el argumento de MSBuild `AppxBundle` desde el paso de compilación.

## Configurar una compilación de implementación continua para una instalación de prueba
Cuando se completa este tipo de compilación, los usuarios pueden descargar el archivo appxbundle desde la sección de artefactos de la página de resultados de la compilación. Si quieres realizar una prueba beta de la aplicación mediante la creación de una distribución más completa, puedes usar el servicio HockeyApp. Este servicio ofrece funcionalidades avanzadas para las pruebas beta, análisis de usuario y diagnósticos de bloqueos.


### Aplicación de números de versión a las compilaciones

El archivo de manifiesto contiene el número de versión de la aplicación.  Actualiza el archivo de manifiesto en el repositorio de control de código fuente para cambiar el número de versión. Otra forma para actualizar el número de versión de la aplicación es usar el número de compilación generado por VSTS y, a continuación, modificar el manifiesto de la aplicación justo antes de compilar la aplicación. Simplemente no confirmes los cambios en el repositorio de código fuente.

Tendrás que definir el formato del número de compilación de control de versiones en la definición de la compilación y, a continuación, usa el número de versión resultante para actualizar el AppxManifest y, opcionalmente, los archivos AssemblyInfo.cs, antes de compilar.

Define el formato de número de compilación en la ficha *General* de la definición de la compilación:

![versión de compilación](images/building-screen12.png) 

Por ejemplo, si estableces el formato de número de compilación en el siguiente valor:  
``` 
$(BuildDefinitionName)_1.1.$(DayOfYear)$(Rev:r).0 
```

VSTS genera un número de versión como:
```
CI_MyUWPApp_1.1.2501.0
```

>Nota: La Tienda requerirá que el último número en la versión sea 0.

Para que puedas extraer el número de versión y aplicarla al manifiesto y/o a los archivos `AssemblyInfo`, usa un script de PowerShell personalizado (disponible [aquí](https://go.microsoft.com/fwlink/?prd=12560&pver=14&plcid=0x409&clcid=0x9&ar=DevCenter&sar=docs)). Ese script lee el número de versión de la variable de entorno `BUILD_BUILDNUMBER` y, a continuación, modifica los archivos AssemblyInfo y AppxManifest. Asegúrate de agregar este script al repositorio de origen y, a continuación, configura una tarea de compilación de PowerShell, tal como se muestra aquí:


![versión de actualización](images/building-screen13.png) 

La variable `$(AppxVersion)` contiene el número de versión. Puedes usar ese número en otros pasos de la compilación. 


#### Opcional: Integrar con HockeyApp
En primer lugar, instala la extensión de Visual Studio [HockeyApp](https://marketplace.visualstudio.com/items?itemName=ms.hockeyapp). 

>Nota: Tendrás que instalar esta extensión como un administrador de VSTS. 


![aplicación hockey](images/building-screen14.png) 

A continuación, configura la conexión de HockeyApp usando esta guía: [Cómo usar HockeyApp con Visual Studio Team Services (VSTS) o Team Foundation Server (TFS).](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs) Puedes usar tu cuenta de Microsoft, cuenta de medios sociales o solo una dirección de correo electrónico para configurar tu cuenta de HockeyApp. El plan gratuito incluye dos aplicaciones, un propietario y ninguna restricción de datos.

A continuación, puedes crear una aplicación HockeyApp manualmente o cargando un archivo de paquete appx existente. Para obtener más información, consulta [Cómo crear una nueva aplicación.](https://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app)  

Para usar un archivo de paquete appx existente, agrega un paso de compilación y establece el parámetro de ruta de acceso de archivo binario del paso de compilación. 

![configurar la aplicación de hockey](images/building-screen15.png) 

Para establecer este parámetro, combina el nombre de aplicación, la variable AppxVersion y las plataformas compatibles en una cadena como esta:

``` 
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp_$(AppxVersion)_Test\MyUWPApp_$(AppxVersion)_x86_x64_ARM.appxbundle
```

>Nota: Aunque la tarea HockeyApp te permite especificar la ruta de acceso al archivo de símbolos, un procedimiento recomendado consiste en incluir los símbolos (archivos appxsym) con el paquete.

Te ayudaremos a instalar y ejecutar un paquete de instalación de prueba [más adelante](#sideloading-best-practices) en esta guía. 

## Configurar una compilación de implementación continua que envía un paquete a la Tienda 

Para generar paquetes de envío a la Tienda, asocia tu aplicación con la Tienda mediante el asistente de asociación con la Tienda en Visual Studio.

![asociar con la Tienda](images/building-screen16.png) 

>Nota: Este asistente genera un archivo denominado Package.StoreAssociation.xml que contiene la información de asociación con la Tienda. Si almacenas el código fuente en un repositorio público como GitHub, este archivo contendrá todos los nombres reservados de aplicación para esa cuenta. Puedes excluir o eliminar este archivo antes de hacerlo público.

Si no tienes acceso a la cuenta del Centro de desarrollo que se usó para publicar la aplicación, puedes seguir las instrucciones de este documento: [¿Crear una aplicación para terceros? Cómo empaquetar su aplicación de la Tienda.](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/#e35YzR5aRG6uaBqK.97) 

A continuación, debes comprobar que el paso de compilación incluye el parámetro siguiente:

```
/p:UapAppxPackageBuildMode=StoreUpload 
```

Esto generará el archivo appxupload que se puede enviar a la Tienda.


#### Configurar el envío automático a la Tienda

Usa la extensión de Visual Studio Team Services para la Tienda Windows para realizar la integración con la API de la Tienda y enviar el paquete appxupload a la Tienda.

Debes conectar tu cuenta del Centro de desarrollo con Azure Active Directory (AD) y, a continuación, crear una aplicación en tu AD para autenticar las solicitudes. Puedes seguir las instrucciones de la página de extensión para lograrlo. 

Una vez que has configurado la extensión, puedes agregar la tarea de compilación y configurarla con el identificador de tu aplicación y la ubicación del archivo appxupload.

![configurar el centro de desarrollo](images/building-screen17.png) 

Donde el valor del parámetro `Package File` será:

```
$(Build.ArtifactStagingDirectory)\
AppxPackages\MyUWPApp__$(AppxVersion)_x86_x64_ARM_bundle.appxupload
```

>Nota. Tienes que activar manualmente esta compilación. Puedes usarla para actualizar las aplicaciones existentes, pero no podrás usarla para el primer envío a la Tienda. Para obtener más información, consulta [Crear y administrar envíos a la Tienda mediante el uso de servicios de la Tienda Windows](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services).

## Procedimientos recomendados

<span id="sideloading-best-practices"/>
### Procedimientos recomendados para aplicaciones de instalaciones de prueba

Si quieres distribuir tu aplicación sin publicarla en la Tienda, puedes realizar instalaciones de prueba de la aplicación directamente en dispositivos, siempre que dichos dispositivos confíen en el certificado que se usó para firmar el paquete de la aplicación. 

Usa el script de PowerShell `Add-AppDevPackage.ps1` para instalar aplicaciones. Este script agregará el certificado a la sección de certificación raíz de confianza para el equipo local y, a continuación, instalará o actualizará el archivo appx.

#### Realizar una instalación de prueba de la aplicación con la Actualización de aniversario de Windows 10
En la Actualización de aniversario de Windows 10, puedes hacer doble clic en el archivo appxbundle e instalar la aplicación seleccionando el botón de instalación en un cuadro de diálogo. 


![instalación de prueba en rs1](images/building-screen18.png) 

>Nota: Este método no instala el certificado ni las dependencias asociadas.

Si deseas distribuir los paquetes appx desde un sitio web como VSTS o HockeyApp, debes agregar ese sitio a la lista de sitios de confianza en el explorador. De lo contrario, Windows marca el archivo como bloqueado. 

<span id="certificates-best-practices"/>
### Procedimientos recomendados para certificados de firma 
Visual Studio genera un certificado para cada proyecto. Esto hace que sea difícil mantener una lista protegida de certificados válidos. Si tienes previsto crear varias aplicaciones, puedes crear un único certificado para firmar todas las aplicaciones. A continuación, cada dispositivo que confíe en tu certificado podrá realizar instalaciones de prueba de cualquiera de las aplicaciones sin necesidad de instalar otro certificado. Para obtener más información, consulta [Cómo crear un certificado de firma del paquete de la aplicación.](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)


#### Crear un certificado de firma
Usa la herramienta [MakeCert.exe](https://msdn.microsoft.com/en-us/library/windows/desktop/ff548309(%09v=vs.85).aspx) para crear un certificado. El siguiente ejemplo crea un certificado con la herramienta MakeCert.exe.

```
MakeCert /n publisherName /r /h 0 /eku "1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.10.3.13" /e expirationDate /sv MyKey.pvk MyKey.cer
```

A continuación, puedes usar la herramienta Pvk2Pfx para generar un archivo PFX que contenga la clave privada protegida con una contraseña.

Proporciona estos certificados a cada rol del equipo:

|**Equipo**|**Uso**|**Certificado**|**Almacén de certificados**|
|-----------|---------|---------------|---------------------|
|Equipo de compilación o desarrollador|Firmar compilaciones|MyCert.PFX|Usuario actual/Personal|
|Equipo de compilación o desarrollador|Ejecutar|MyCert.cer|Equipo local/Personas de confianza|
|Usuario|Ejecutar|MyCert.cer|Equipo local/Personas de confianza|

>Nota: También puedes usar un certificado de empresa que ya sea de confianza para los usuarios.

#### Firmar tu aplicación de UWP
Visual Studio y MSBuild ofrece distintas opciones para administrar el certificado que usas para firmar la aplicación:

Una opción consiste en incluir el certificado con la clave privada (normalmente en forma de un archivo .PFX) en tu solución y, a continuación, hacer referencia al pfx en el archivo del proyecto. Esto lo puedes administrar mediante la pestaña Paquete del editor de manifiestos.


![crear certificado](images/building-screen19.png) 

Otra opción es instalar el certificado en el equipo de compilación (Usuario actual/Personal) y, a continuación, usar la opción Elegir del almacén de certificados. Esto especifica la huella digital del certificado en el archivo de proyecto, de forma que se deberá instalar el certificado en todos los equipos que se usen para compilar el proyecto.

#### Confiar en el certificado de firma en los dispositivos de destino
Un dispositivo de destino debe confiar en el certificado antes de poder instalar la aplicación en él. 

Registra la clave pública del certificado en la ubicación de raíz de confianza o de personas de confianza en el almacén de certificados del equipo Local.

La forma más sencilla de registrar el certificado es hacer doble clic en el archivo .cer y, a continuación, seguir los pasos en el asistente para guardar el certificado en el almacén de personas de confianza y el equipo local.

## Temas relacionados
* [Compilar la aplicación de .NET para Windows](https://www.visualstudio.com/en-us/docs/build/get-started/dot-net) 
* [Empaquetado de aplicaciones para UWP](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
* [Realizar la instalación de prueba de aplicaciones de línea de negocio en Windows 10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
* [Cómo crear un certificado de firma del paquete de la aplicación](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)



<!--HONumber=Nov16_HO1-->

