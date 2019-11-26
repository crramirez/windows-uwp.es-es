---
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Pruebas del Kit para la certificación de aplicaciones en Windows
description: El kit para la certificación de aplicaciones de Windows contiene una serie de pruebas que pueden ayudarle a asegurarse de que la aplicación esté lista para publicarse en el Microsoft Store.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, certificación de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: 32ece54ef17c97b1cb16b3f0a706c86eb2858556
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257857"
---
# <a name="windows-app-certification-kit-tests"></a>Pruebas del Kit para la certificación de aplicaciones en Windows


El [Kit](windows-app-certification-kit.md) para la certificación de aplicaciones de Windows contiene una serie de pruebas que ayudan a garantizar que la aplicación esté lista para publicarse en el Microsoft Store. Las pruebas se enumeran a continuación con sus criterios, detalles y acciones sugeridas en caso de error.

## <a name="deployment-and-launch-tests"></a>Implementación y pruebas de inicio

Supervisa la aplicación durante la prueba de certificación para registrar cuándo se bloquea o se cuelga.

### <a name="background"></a>Background

Las aplicaciones que dejan de responder o se bloquean pueden originar pérdidas de datos al usuario y que su experiencia no sea buena.

Esperamos que las aplicaciones funcionen correctamente sin usar los modos de compatibilidad de Windows, mensajes de AppHelp o archivos de compatibilidad.

Las aplicaciones no deben enumerar los archivos DLL para cargarlos en la\-HKEY LOCAL\-MACHINE\\software\\la\\Windows NT\\CurrentVersion\\Windows\\AppInit\-dll del registro.

### <a name="test-details"></a>Detalles de la prueba

Probamos la estabilidad y resistencia de la aplicación durante las pruebas de certificación.

El Kit para la certificación de aplicaciones en Windows llama a [**IApplicationActivationManager::ActivateApplication**](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) para iniciar las aplicaciones. Para que **ActivateApplication** inicie una aplicación, el Control de cuentas de usuario (UAC) debe estar habilitado y la pantalla debe tener una resolución de al menos 1024 x 768 o 768 x 1024. Si alguna de estas condiciones no se cumple, tu aplicación no pasará esta prueba.

### <a name="corrective-actions"></a>Acciones correctivas

Asegúrate de que UAC esté habilitado en el equipo de prueba.

Asegúrate de ejecutar la prueba en un equipo que tenga una pantalla lo suficientemente amplia.

Si tu aplicación no puede iniciarse aun cuando la plataforma de prueba cumple con los requisitos de [**ActivateApplication**](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication), puedes solucionar el problema revisando el registro de eventos de activación. Para encontrar estas entradas en el registro de eventos:

1.  Abra eventvwr. exe y navegue hasta el registro de la aplicación y los servicios\\carpeta Microsoft\\Windows\\el shell envolvente.
2.  Filtra la vista para que se muestren los identificadores de eventos: 5900-6000.
3.  Revisa las entradas del registro para encontrar información que pueda explicar por qué la aplicación no se inicia.

Soluciona los problemas del archivo mediante identificación y corrección. Recompila y vuelve a probar la aplicación. También puedes comprobar si en la carpeta de registro del Kit para la certificación de aplicaciones de Windows se generó un archivo de volcado que se pueda usar para depurar la aplicación.

## <a name="platform-version-launch-test"></a>Prueba de inicio de versión de plataforma

Comprueba que la aplicación de Windows puede ejecutarse en una versión futura del sistema operativo. Esta prueba históricamente solo se ha aplicado al flujo de trabajo de la aplicación de escritorio, pero ahora también es compatible con flujos de trabajo de la Plataforma universal de Windows (UWP).

### <a name="background"></a>Background

La información de versión del sistema operativo tiene un uso restringido para el Microsoft Store. A menudo, las aplicaciones usaban esta restricción de forma incorrecta para comprobar la versión del SO para que la aplicación pudiera proporcionar a los usuarios una funcionalidad específica de una versión del sistema operativo.

### <a name="test-details"></a>Detalles de la prueba

El Kit para la certificación de aplicaciones en Windows usa HighVersionLie para detectar cómo comprueba la aplicación la versión del SO. Si la aplicación se bloquea, no pasará esta prueba.

### <a name="corrective-action"></a>Acción correctiva

Las aplicaciones deben usar las funciones auxiliares de la API de la versión para comprobar esto. Consulta [Versión del sistema operativo](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) para más información.

## <a name="background-tasks-cancellation-handler-validation"></a>Validación del controlador de cancelación de tareas en segundo plano

Comprueba que la aplicación tiene un controlador de cancelación de tareas en segundo plano declarado. Es necesario que haya una función específica a la que se llamará cuando se cancele la tarea. Esta prueba se aplica solo a aplicaciones implementadas.

### <a name="background"></a>Background

Las aplicaciones de la Tienda pueden registrar un proceso que se ejecuta en segundo plano. Por ejemplo, una aplicación de correo electrónico puede hacer ping a un servidor ocasionalmente. Sin embargo, si el sistema operativo necesita estos recursos, cancelará la tarea en segundo plano y las aplicaciones deben controlar correctamente esta cancelación. Es posible que las aplicaciones que no tengan un controlador de cancelación se bloqueen o no se cierren cuando el usuario intente cerrar la aplicación.

### <a name="test-details"></a>Detalles de la prueba

La aplicación se inicia, se suspende, y la parte que no está en segundo plano de la aplicación se finaliza. A continuación, las tareas en segundo plano asociadas con esta aplicación se cancelan. Se comprueba el estado de la aplicación y si la aplicación sigue ejecutándose, no pasará esta prueba.

### <a name="corrective-action"></a>Acción correctiva

Agrega el controlador de cancelación a la aplicación. Para más información, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## <a name="app-count"></a>Recuento de aplicaciones

Comprueba que el paquete de la aplicación (APPX, lote de aplicaciones) contiene una aplicación. Esto se ha cambiado en el kit para ser una prueba independiente.

### <a name="background"></a>Background

Esta prueba se ha implementado según la directiva de la Tienda.

### <a name="test-details"></a>Detalles de la prueba

Para las aplicaciones de Windows Phone 8.1, se comprueba que el número total de paquetes appx del lote es &lt; 512, que hay un solo paquete principal en el lote y que la arquitectura del paquete principal del lote se ha marcado como ARM o neutro.

Para las aplicaciones de Windows 10, la prueba comprueba que el número de revisión de la versión del lote se establece en 0.

### <a name="corrective-action"></a>Acción correctiva

Asegúrese de que el paquete de la aplicación y el lote cumplen con requisitos anteriores que se indican en la sección Detalles de la prueba.

## <a name="app-manifest-compliance-test"></a>Prueba de cumplimiento del manifiesto de la aplicación

Prueba el contenido del manifiesto de la aplicación para asegurarte de que su contenido es correcto.

### <a name="background"></a>Background

Las aplicaciones deben tener un manifiesto de la aplicación con el formato correcto.

### <a name="test-details"></a>Detalles de la prueba

Examina el manifiesto de la aplicación para comprobar que el contenido sea correcto, según se describe en los [Requisitos del paquete de la aplicación](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

-   **Extensiones de archivo y protocolos**

    La aplicación puede declarar las extensiones de archivo con los que quiere asociarse. Si no se usa correctamente, la aplicación puede declarar una gran cantidad de extensiones de archivo, la mayoría de las cuales quizás ni use, lo que provocaría una experiencia del usuario deficiente. Esta prueba agrega una comprobación para limitar la cantidad de extensiones de archivo con las que puede asociarse una aplicación.

-   **Regla de dependencia de marco**

    Esta prueba aplica el requisito de que las aplicaciones adopten las dependencias adecuadas en UWP. Si se encuentra una dependencia inadecuada, la aplicación no pasará la prueba.

    Si hay un error de coincidencia entre la versión del sistema operativo a la que se aplica la aplicación y las dependencias de marco establecidas, la aplicación no pasará la prueba. Tampoco lo hará si hace referencia a alguna versión anterior de los dll del marco.

-   **Comprobación de la comunicación entre procesos (IPC)**

    Esta prueba exige el requisito de que las aplicaciones UWP no se comunican fuera del contenedor de la aplicación con los componentes de escritorio. La comunicación entre procesos está pensada exclusivamente para las aplicaciones de prueba. Las aplicaciones en las que el nombre especificado en [**ActivatableClassAttribute**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-activatableclassattribute) sea "DesktopApplicationPath" no superarán esta prueba.

### <a name="corrective-action"></a>Acción correctiva

Compara el manifiesto de la aplicación con los requisitos descritos en la página sobre los [Requisitos del paquete de la aplicación](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

## <a name="windows-security-features-test"></a>Prueba de características de seguridad de Windows

### <a name="background"></a>Background

Cambiar las protecciones de seguridad de Windows predeterminadas puede causar grandes riesgos a los clientes.

### <a name="test-details"></a>Detalles de la prueba

Prueba la seguridad de la aplicación ejecutando el [analizador binario BinScope](#binscope-binary-analyzer-tests).

Las pruebas del analizador binario BinScope examinan los archivos binarios de la aplicación para comprobar las prácticas de codificación y diseño que hacen que la aplicación sea menos vulnerable a los ataques o que se use como vector de ataque.

Las pruebas del analizador binario BinScope comprueban el uso correcto de las siguientes características relacionadas con la seguridad:

-   Pruebas de analizador binario BinScope
-   Firma de código privado

### <a name="binscope-binary-analyzer-tests"></a>Pruebas de analizador binario BinScope

Las pruebas del [analizador binario BinScope](https://www.microsoft.com/en-us/download/details.aspx?id=44995) examinan los archivos binarios de la aplicación para comprobar las prácticas de codificación y diseño que hacen que la aplicación sea menos vulnerable a los ataques o que se use como vector de ataque.

Las pruebas del analizador binario BinScope comprueban el uso correcto de estas características relacionadas con la seguridad:

-   [Atributo](#binscope-1)
-   [Protección de control de excepciones de/SafeSEH](#binscope-2)
-   [Prevención de ejecución de datos](#binscope-3)
-   [Selección aleatoria del diseño del espacio de direcciones](#binscope-4)
-   [Sección de PE compartida de lectura/escritura](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>Atributo

**Mensaje de error del Kit para la certificación de aplicaciones en Windows :** error en la prueba APTCACheck

El atributo AllowPartiallyTrustedCallersAttribute (APTCA) habilita el acceso a los códigos de plena confianza desde los códigos de confianza parcial en los ensamblados con signo. Cuando apliques el atributo APTCA en un ensamblado, los llamadores de confianza parcial pueden obtener acceso a ese ensamblado durante su vigencia, lo que puede comprometer la seguridad.

**Qué hacer si se produce un error en la aplicación en esta prueba**

No uses el atributo APTCA en ensamblados con nombre seguro a menos que lo requiera tu proyecto y comprendas cuáles son los riesgos. En los casos en los que sea obligatorio, asegúrate de que todas las API estén protegidas con las peticiones de seguridad de acceso al código apropiadas. APTCA no tiene efectos cuando el ensamblado forma parte de una aplicación para la Plataforma universal de Windows (UWP).

**Sección**

Esta prueba solo se realiza en código administrado (C#, .NET, etc.).

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>Protección de control de excepciones de/SafeSEH

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** Error en la prueba SafeSEHCheck

Un controlador de excepciones se ejecuta cuando la aplicación encuentra una condición de excepción, como un error de división entre cero. Dado que la dirección del controlador de excepciones se almacena en la pila cuando se llama una función, podría quedar expuesta a un atacante de desbordamiento de búfer si algún software malintencionado quisiera sobrescribir la pila.

**Qué hacer si se produce un error en la aplicación en esta prueba**

Habilita la opción /SAFESEH en el comando enlazador cuando diseñes tu aplicación. Esta opción está activada de manera predeterminada en la configuración de lanzamiento de Visual Studio. Comprueba que esta opción esté habilitada en las instrucciones de compilación de todos los módulos ejecutables de la aplicación.

**Sección**

La prueba no se realiza en binarios de 64 bits ni en binarios de conjunto de chips ARM porque no almacenan direcciones de controladores de excepciones en la pila.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>Prevención de ejecución de datos

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** error en la prueba NXCheck

Esta prueba comprueba que la aplicación no ejecute el código que se almacena en el segmento de datos.

**Qué hacer si se produce un error en la aplicación en esta prueba**

Habilita la opción /NXCOMPAT en el comando enlazador cuando diseñes tu aplicación. Esta opción está activada de manera predeterminada en las versiones del enlazador que admiten la Prevención de ejecución de datos (DEP).

**Sección**

Te recomendamos que pruebes tus aplicaciones en una CPU con funcionalidad DEP y repares cualquier error que encuentres en los resultados de DEP.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>Selección aleatoria del diseño del espacio de direcciones

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** Error en la prueba DBCheck.

La selección aleatoria del diseño del espacio de direcciones (ASLR) carga imágenes ejecutables en ubicaciones impredecibles de la memoria, lo que dificulta la tarea del software malintencionado La aplicación y todos los componentes que usa la aplicación deben admitir ASLR.

**Qué hacer si se produce un error en la aplicación en esta prueba**

Habilita la opción /DYNAMICBASE en el comando enlazador cuando diseñes tu aplicación. Comprueba que todos los módulos que usa la aplicación también usen esta opción de enlazador.

**Sección**

Normalmente, ASLR no afecta al rendimiento. Pero en algunos escenarios, hay una pequeña mejora de rendimiento en sistemas de 32 bits. Es posible que el rendimiento pueda afectar a un sistema muy congestionado con imágenes cargadas en muchas ubicaciones diferentes de la memoria.

Esta prueba se realiza solamente en aplicaciones escritas en lenguajes no administrados, como C o C++.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>Sección de PE compartida de lectura/escritura

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** Error en la prueba SharedSectionsCheck.

Los archivos binarios con secciones grabables marcadas como compartidas son una amenaza de seguridad. No compiles aplicaciones con secciones compartidas grabables a menos que sea necesario. Usa [**CreateFileMapping**](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-createfilemappinga) o [**MapViewOfFile**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffile) para crear un objeto de memoria compartida debidamente asegurado.

**Qué hacer si se produce un error en la aplicación en esta prueba**

Elimina todas las secciones compartidas de la aplicación y crea objetos de memoria compartidos invocando [**CreateFileMapping**](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-createfilemappinga) o [**MapViewOfFile**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffile) con los atributos de seguridad apropiados y vuelve a compilar la aplicación.

**Sección**

Esta prueba se realiza solamente en aplicaciones escritas en lenguajes no administrados, como C o C++.

### <a name="appcontainercheck"></a>AppContainerCheck

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** Error en la prueba AppContainerCheck.

AppContainerCheck comprueba que esté establecido el bit **appcontainer** en el encabezado portable ejecutable (PE) de un binario ejecutable. Las aplicaciones deben tener el bit **appcontainer** establecido en todos los archivos .exe y en todas las DLL no administradas para ejecutarse correctamente.

**Qué hacer si se produce un error en la aplicación en esta prueba**

Si un archivo ejecutable nativo no pasa la prueba, asegúrate de haber usado los compiladores y enlazadores más recientes para compilarlo y de haber usado la marca */appcontainer* en el enlazador.

Si un ejecutable administrado no supera la prueba, asegúrese de que usó el compilador y el enlazador más recientes, como Microsoft Visual Studio, para compilar la aplicación para UWP.

**Sección**

Esta prueba se realiza en todos los archivos .exe y en DLL no administrados.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** Error en la prueba ExecutableImportsCheck.

Una imagen portable ejecutable (PE) no pasa esta prueba si su tabla de importación se colocó en una sección de código ejecutable. Esto puede producirse si habilitaste la combinación de .rdata para la imagen PE estableciendo la marca */merge* del enlazador de Visual C++ en */merge:.rdata=.text*.

**Qué hacer si se produce un error en la aplicación en esta prueba**

No combines la tabla de importación en una sección de código ejecutable. Asegúrate de que la marca */merge* del enlazador de Visual C++ no esté configurada para combinar la sección ".rdata" en una sección de código.

**Sección**

Esta prueba se realiza en todo el código binario excepto en los ensamblados puramente administrados.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Mensaje de error del Kit para la certificación de aplicaciones en Windows:** Error en la prueba WXCheck.

La comprobación ayuda a garantizar que un binario no tenga ninguna página asignada como de escritura y ejecutable. Esto puede ocurrir si el binario tiene una sección grabable y ejecutable, o si el *alineación* del archivo binario es menor que el tamaño de la *Página\-* .

**Qué hacer si se produce un error en la aplicación en esta prueba**

Asegúrese de que el archivo binario no tiene una sección grabable o ejecutable y que el valor de *alineación* del archivo binario es al menos igual que su *Página\-tamaño*.

**Sección**

Esta prueba se realiza en todos los archivos .exe y en DLL nativos no administrados.

Un ejecutable puede tener una sección de escritura y ejecutable si se compiló con Editar y continuar habilitado (/ZI). Deshabilitar Editar y continuar hará que la sección no válida no esté presente.

El *tamaño de\-de página* es el valor predeterminado de *alineación* para los ejecutables.

### <a name="private-code-signing"></a>Firma de código privado

Prueba la existencia de archivos binarios de firma de código privado en el paquete de la aplicación.

### <a name="background"></a>Background

Los archivos de firma de código privado se deben conservar de forma privada, ya que podrían usarse de forma malintencionada en caso de que se pongan en riesgo.

### <a name="test-details"></a>Detalles de la prueba

Prueba para detectar archivos dentro del paquete de la aplicación que tengan la extensión .pfx o.snk, lo que indicaría que se incluyeron claves de firma privadas.

### <a name="corrective-actions"></a>Acciones correctivas

Quite todas las claves de firma de código privado (por ejemplo, archivos. pfx y. snk) del paquete.

## <a name="supported-api-test"></a>Prueba de API admitidas

Prueba la aplicación para detectar el uso de cualquier API no compatible.

### <a name="background"></a>Background

Las aplicaciones deben usar las API para que las aplicaciones UWP (Windows Runtime o las API Win32 compatibles) estén certificadas para la Microsoft Store. Esta prueba también identifica situaciones en las que un binario administrado toma una dependencia de una función fuera del perfil aprobado.

### <a name="test-details"></a>Detalles de la prueba

-   Comprueba que cada binario del paquete de la aplicación no tiene una dependencia en una API de Win32 que no se admite para el desarrollo de aplicaciones para UWP comprobando la tabla de direcciones de importación del archivo binario.
-   Comprueba que cada binario administrado en el paquete de la aplicación no toma una dependencia de una función fuera del perfil aprobado.

### <a name="corrective-actions"></a>Acciones correctivas

Asegúrate de que la aplicación se haya compilado como una versión de lanzamiento y no con una versión de depuración.

> **Tenga en cuenta**  la compilación de depuración de una aplicación producirá un error en esta prueba, aunque la aplicación solo use las [API para las aplicaciones UWP](https://docs.microsoft.com/uwp/).

Revise los mensajes de error para identificar la API que usa la aplicación y que no es una [API para aplicaciones UWP](https://docs.microsoft.com/uwp/).

> **Tenga** en C++ cuenta  las aplicaciones que se compilan en una configuración de depuración producirán un error en esta prueba aunque la configuración solo use las API del Windows SDK para las aplicaciones para UWP. Consulte [alternativas a las API de Windows en aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx) para obtener más información.

## <a name="performance-tests"></a>Pruebas de rendimiento

La aplicación debe responder con rapidez a la interacción del usuario y los comandos del sistema para que la experiencia de usuario sea rápida y fluida.

Las características del equipo en el que se realiza la prueba pueden afectar a los resultados. Los umbrales de la prueba de rendimiento para la certificación de aplicaciones están establecidos de tal manera que los equipos de bajo consumo cumplan con las expectativas del cliente en cuanto a una experiencia rápida y fluida. Para determinar el rendimiento de la aplicación, te recomendamos que la pruebes en un equipo de bajo consumo, por ejemplo, un equipo basado en un procesador Intel Atom con una resolución de pantalla de 1366 x 768 (o superior) y un disco duro giratorio (en lugar de un disco duro de estado sólido).

### <a name="bytecode-generation"></a>Generación de códigos de bytes

Como optimización del rendimiento para acelerar el tiempo de ejecución de JavaScript, los archivos de JavaScript con la extensión .js generan código de bytes cuando se implementa la aplicación. Esto mejora el inicio y el tiempo de ejecución en curso para las operaciones de JavaScript.

### <a name="test-details"></a>Detalles de la prueba

Examina la implementación de la aplicación para asegurarse de que todos los archivos .js se hayan convertido a código de bytes.

### <a name="corrective-action"></a>Acción correctiva

Si la prueba da error, considera lo siguiente para abordar el problema:

-   Comprueba que el registro de eventos esté habilitado.
-   Comprueba que todos los archivos JavaScript sean sintácticamente válidos.
-   Confirma que todas las versiones anteriores de la aplicación estén desinstaladas.
-   Excluye los archivos identificados del paquete de la aplicación.

### <a name="optimized-binding-references"></a>Referencias de enlace optimizadas

Cuando uses enlaces, WinJS.Binding.optimizeBindingReferences debe establecerse como verdadero para optimizar el uso de la memoria.

### <a name="test-details"></a>Detalles de la prueba

Comprueba el valor de WinJS.Binding.optimizeBindingReferences.

### <a name="corrective-action"></a>Acción correctiva

Establece WinJS.Binding.optimizeBindingReferences en **true** en el JavaScript de la aplicación.

## <a name="app-manifest-resources-test"></a>Prueba de recursos del manifiesto de la aplicación

### <a name="app-resources-validation"></a>Validación de recursos de la aplicación

La aplicación probablemente no se instale si las cadenas o imágenes declaradas en el manifiesto de la aplicación son incorrectas. Si se instala con estos errores, es probable que el logotipo de la aplicación u otras imágenes usadas por esta no se muestren correctamente.

### <a name="test-details"></a>Detalles de la prueba

Inspecciona los recursos definidos en el manifiesto de la aplicación para asegurarse de que estén presentes y sean válidos.

### <a name="corrective-action"></a>Acción correctiva

Usa esta tabla como guía.

<table>
<tr><th>Mensaje de error</th><th>Observaciones</th></tr>
<tr><td>
<p>La imagen {image name} define ambos calificadores Scale y TargetSize; puedes definir un calificador por vez.</p>
</td><td>
<p>Puedes personalizar imágenes para distintas resoluciones.</p>
<p>En el mensaje en sí, {image name}contiene el nombre de la imagen que presenta el error.</p>
<p> Asegúrate de que cada imagen defina Scale o TargetSize como calificador.</p>
</td></tr>
<tr><td>
<p>La imagen {image name} no cumple con las restricciones de tamaño.</p>
</td><td>
<p>Asegúrate de que todas las imágenes de la aplicación cumplan con las restricciones de tamaño apropiado.</p>
<p>En el mensaje en sí, {image name}contiene el nombre de la imagen que presenta el error.</p>
</td></tr>
<tr><td>
<p>La imagen {image name} no se encuentra en el paquete.</p>
</td><td>
<p>Falta una imagen obligatoria.</p>
<p>En el mensaje en sí, {image name} contiene el nombre de la imagen que falta.</p>
</td></tr>
<tr><td>
<p>La imagen {image name} no es un archivo de imagen válido.</p>
</td><td>
<p>Asegúrate de que todas las imágenes de la aplicación cumplan con las restricciones de tipo de formato de archivo apropiado.</p>
<p>En el mensaje en sí, {image name}contiene el nombre de la imagen que no es válida.</p>
</td></tr>
<tr><td>
<p>La imagen “BadgeLogo” tiene un valor ABGR {value} en la posición (x, y) que no es válido. El píxel debe ser blanco (##FFFFFF) o transparente (00######).</p>
</td><td>
<p>El logotipo del distintivo es una imagen que se muestra junto a la notificación del distintivo para identificar la aplicación en la pantalla de bloqueo. Esta imagen debe ser monocromática (puede tener únicamente píxeles blancos o transparentes).</p>
<p>En el mensaje en sí, {value} contiene un valor de color en la imagen que no es válido.</p>
</td></tr>
<tr><td>
<p>La imagen "BadgeLogo" tiene un valor ABGR {value} en la posición (x, y) que no es válido para una imagen con blanco en contraste alto. El píxel debe ser (##2A2A2A) o más oscuro, o transparente (00######).</p>
</td><td>
<p>El logotipo del distintivo es una imagen que se muestra junto a la notificación del distintivo para identificar la aplicación en la pantalla de bloqueo.   Dado que el logotipo del distintivo aparece en un fondo blanco cuando se usa blanco en contraste alto, debe ser una versión oscura del logotipo del distintivo normal. En blanco de alto contraste, el logotipo del distintivo solo puede contener píxeles que son más oscuros que (##2A2A2A) o transparentes.</p>
<p>En el mensaje en sí, {value} contiene un valor de color en la imagen que no es válido.</p>
</td></tr>
<tr><td>
<p>La imagen debe definir al menos una variante sin un calificador TargetSize. Debe definir un calificador Scale o dejar Scale y TargetSize sin especificar, para que de manera predeterminada se establezca Scale-100.</p>
</td><td>
<p>Para más información, consulta <a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Diseño con capacidad de respuesta 101 para aplicaciones de la Plataforma universal de Windows (UWP)</a> y <a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">Directrices sobre recursos de la aplicación</a>.</p>
</td></tr>
<tr><td>
<p>Al paquete le falta un archivo "resources.pri".</p>
</td><td>
<p>Si tienes contenido localizable en el manifiesto de la aplicación, asegúrate de que el paquete de la aplicación incluya un archivo resources.pri válido.</p>
</td></tr>
<tr><td>
<p>El archivo "resources.pri" debe tener un mapa de recursos cuyo nombre coincida con el nombre del paquete {package full name}.</p>
</td><td>
<p>Este error puede obtenerse si el manifiesto cambió y el nombre del mapa de recursos del archivo resources.pri deja de coincidir con el nombre del paquete en el manifiesto.</p>
<p>En el mensaje en sí, {package full name} contiene el nombre del paquete que resources.pri debe tener.</p>
<p>Para corregir esto, debes reconstruir resources.pri y la forma más fácil de hacerlo es reconstruyendo el paquete de la aplicación.</p>
</td></tr>
<tr><td>
<p>El archivo "resources.pri" no debe tener habilitado Combinar automáticamente.</p>
</td><td>
<p>MakePRI.exe admite una opción denominada <strong>Combinar automáticamente</strong>. El valor predeterminado de <strong>Combinar automáticamente</strong> es <strong>desactivado</strong>. Cuando está habilitado, <strong>Combinar automáticamente</strong> combina los recursos del paquete de idioma de una aplicación en un solo resources.pri en tiempo de ejecución. No se recomienda para las aplicaciones que desea distribuir a través de la Microsoft Store. Los recursos. PRI de una aplicación distribuida a través de la Microsoft Store deben estar en la raíz del paquete de la aplicación y contener todas las referencias de lenguaje que admite la aplicación.</p>
</td></tr>
<tr><td>
<p>La cadena {string} no cumple la restricción de longitud máxima de {number} caracteres.</p>
</td><td>
<p>Consulta los <a href="https://docs.microsoft.com/windows/uwp/publish/app-package-requirements">requisitos de metadatos del paquete</a>.</p>
<p>En el mensaje en sí, {string} se reemplaza por la cadena que tiene el error y {number} contiene la longitud máxima.</p>
</td></tr>
<tr><td>
<p>La cadena {string} no debe tener espacios en blanco iniciales ni finales.</p>
</td><td>
<p>El esquema de los elementos en el manifiesto de la aplicación no permite caracteres de espacio en blanco inicial ni final.</p>
<p>En el mensaje en sí, {string} se reemplaza por la cadena que tiene el error.</p>
<p>Asegúrate de que ninguno de los valores localizados en los campos del manifiesto en resources.pri tenga caracteres de espacio en blanco inicial o final.</p>
</td></tr>
<tr><td>
<p>La cadena debe ser no vacía (mayor que cero en longitud).</p>
</td><td>
<p>Consulta el tema sobre los <a href="https://docs.microsoft.com/windows/uwp/publish/app-package-requirements">requisitos del paquete de la aplicación</a> para más información.</p>
</td></tr>
<tr><td>
<p>No hay un recurso predeterminado especificado en el archivo "resources.pri".</p>
</td><td>
<p>Para más información, consulta <a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">Directrices sobre recursos de la aplicación</a>.</p>
<p>En la configuración de compilación predeterminada, Visual Studio solo incluye los recursos de imagen con una escala de 200 en el paquete de la aplicación al generar paquetes y coloca otros recursos en el paquete de recursos. Asegúrate de incluir recursos de imagen con una escala de 200 o de configurar el proyecto para incluir los recursos que tienes.</p>
</td></tr>
<tr><td>
<p>No hay un valor de recurso especificado en el archivo "resources.pri".</p>
</td><td>
<p>Asegúrate de que el manifiesto de la aplicación tenga recursos definidos en dicho archivo.</p>
</td></tr>
<tr><td>
<p>El archivo de imagen {FILENAME} debe ser inferior a 204800 bytes.\*\*</p>
</td><td>
<p>Reduce el tamaño de las imágenes especificadas.</p>
</td></tr>
<tr><td>
<p>El archivo {filename} no debe contener una sección de asignación inversa.\*\*</p>
</td><td>
<p>Aunque el mapa inverso se genera durante la tarea "F5 debugging" de Visual Studio al llamar a makepri.exe, se puede quitar ejecutando makepri.exe sin el parámetro /m al generar un archivo pri.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* indica que se ha agregado una prueba en el kit de certificación de aplicaciones de Windows 3,3 para Windows 8.1 y solo es aplicable cuando se usa esa versión del kit o posterior.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>Validación de la personalización de marca

Se espera que las aplicaciones UWP estén completas y sean totalmente funcionales. Las aplicaciones que usan imágenes predeterminadas (de plantillas o muestras del SDK) ofrecen una experiencia mediocre al usuario y son difíciles de identificar en el catálogo de la tienda.

### <a name="test-details"></a>Detalles de la prueba

La prueba corrobora que las imágenes usadas por la aplicación no sean imágenes predeterminadas de muestras de SDK ni de Visual Studio.

### <a name="corrective-actions"></a>Acciones correctivas

Reemplaza las imágenes predeterminadas con otras más distintivas y representativas de tu aplicación.

## <a name="debug-configuration-test"></a>Prueba de configuración de depuración

Prueba la aplicación para asegurarte de que no sea una versión de depuración.

### <a name="background"></a>Background

Para que se certifique el Microsoft Store, las aplicaciones no se deben compilar para depurar y no deben hacer referencia a las versiones de depuración de un archivo ejecutable. Además, debes crear tu propio código según lo optimice tu aplicación para pasar esta prueba.

### <a name="test-details"></a>Detalles de la prueba

Prueba la aplicación para asegurarte de que no sea una versión de depuración y no esté vinculada con ningún marco de depuración.

### <a name="corrective-actions"></a>Acciones correctivas

-   Compile la aplicación como una compilación de versión antes de enviarla a la Microsoft Store.
-   Asegúrate de tener instalada la versión correcta de .NET Framework.
-   Asegúrate de que la aplicación no esté vinculada a versiones de depuración de un marco de trabajo y que esté creada con una versión de lanzamiento. Si la aplicación incluye componentes .NET, comprueba si has instalado la versión correcta de .NET Framework.

## <a name="file-encoding-test"></a>Prueba de codificación de archivos

### <a name="utf-8-file-encoding"></a>Codificación de archivos UTF-8

### <a name="background"></a>Background

Los archivos HTML, CSS y JavaScript deben codificarse en formato UTF-8 con una correspondiente marca de orden de bytes (BOM), para que se puedan obtener los beneficios del almacenamiento en caché del código de bytes y se eviten ciertas condiciones de error en tiempo de ejecución.

### <a name="test-details"></a>Detalles de la prueba

Prueba el contenido de los paquetes de la aplicación para asegurarte de que usen la codificación de archivo correcta.

### <a name="corrective-action"></a>Acción correctiva

Abre el archivo afectado y selecciona **Guardar como** en el menú **Archivo** de Visual Studio. Selecciona el control desplegable ubicado junto al botón **Save** y selecciona **Guardar con codificación**. En el cuadro de diálogo de opciones de guardado **Advanced**, elige la opción Unicode (UTF-8 con firma) y haz clic en **Aceptar**.

## <a name="direct3d-feature-level-test"></a>Prueba de nivel de función de Direct3D

### <a name="direct3d-feature-level-support"></a>Compatibilidad del nivel de función de Direct3D

Prueba las aplicaciones de Microsoft Direct3D para garantizar que no se bloquearán en los dispositivos con hardware gráfico más antiguo.

### <a name="background"></a>Background

Microsoft Store requiere que todas las aplicaciones que usan Direct3D se representen correctamente o con errores en las tarjetas gráficas de nivel de característica 9\-1.

Dado que los usuarios pueden cambiar el hardware gráfico en el dispositivo después de instalar la aplicación, si elige un nivel de característica mínimo superior a 9\-1, la aplicación debe detectar en el inicio si el hardware actual cumple o no los requisitos mínimos. Si no se cumplen los requisitos mínimos, la aplicación debe mostrar un mensaje al usuario en el que se detallan todos los requisitos de Direct3D. Asimismo, si se descarga una aplicación en un dispositivo con el que no es compatible, debe detectarlo en el inicio y mostrar al cliente un mensaje en el que se detallan los requisitos.

### <a name="test-details"></a>Detalles de la prueba

La prueba se validará si las aplicaciones se representan con precisión en el nivel de características 9\-1.

### <a name="corrective-action"></a>Acción correctiva

Asegúrese de que la aplicación se representa correctamente en el nivel de características de Direct3D 9\-1, incluso si espera que se ejecute en un nivel de características superior. Consulta [Desarrollar para distintos niveles de funciones de Direct3D](https://msdn.microsoft.com/library/windows/apps/hh994923.aspx) para obtener más información.

### <a name="direct3d-trim-after-suspend"></a>Recorte Direct3D tras suspensión

> **Tenga en cuenta**  esta prueba solo se aplica a las aplicaciones UWP desarrolladas para Windows 8.1 y versiones posteriores.

### <a name="background"></a>Background

Si la aplicación no llama a [**Trim**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) en su dispositivo Direct3D, no podrá liberar la memoria asignada a trabajos 3D anteriores. Esto aumenta el riesgo de que la aplicación finalice debido a la presión de memoria del sistema.

### <a name="test-details"></a>Detalles de la prueba

Comprueba si las aplicaciones cumplen los requisitos d3d y garantiza que las aplicaciones llamen a una nueva API [**Trim**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) en la devolución de llamada de Suspend.

### <a name="corrective-action"></a>Acción correctiva

La aplicación debe llamar a la API [**Trim**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) en su interfaz de [**IDXGIDevice3**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3) siempre que esté por suspenderse.

## <a name="app-capabilities-test"></a>Prueba de funcionalidades de la aplicación

### <a name="special-use-capabilities"></a>Funcionalidades de uso especial

### <a name="background"></a>Background

Las funcionalidades de uso especial están indicadas para escenarios muy específicos. Estas funcionalidades solo pueden usarse en cuentas de empresa.

### <a name="test-details"></a>Detalles de la prueba

Examina si la aplicación declara alguna de las siguientes funcionalidades:

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

Si se declara alguna de estas funcionalidades, la prueba muestra una advertencia al usuario.

### <a name="corrective-actions"></a>Acciones correctivas

Piensa en la posibilidad de quitar la función de uso especial si la aplicación no la necesita. Además, el uso de estas funcionalidades está sujeto a otras revisiones de las directivas de incorporación.

## <a name="windows-runtime-metadata-validation"></a>Validación de metadatos de Windows Runtime

### <a name="background"></a>Background

Comprueba que los componentes suministrados en una aplicación corresponden al sistema de tipo de UWP.

### <a name="test-details"></a>Detalles de la prueba

Comprueba que los archivos **.winmd** del paquete cumplen las reglas de UWP.

### <a name="corrective-actions"></a>Acciones correctivas

-   **Prueba del atributo ExclusiveTo:** comprueba que las clases de UWP no implementen interfaces de otra clase marcadas como exclusivas (ExclusiveTo).
-   **Prueba de ubicación de tipo:** comprueba que los metadatos de todos los tipos de UWP estén ubicados en el archivo .winmd cuyo nombre en longitud presente la mayor coincidencia con el espacio de nombres en el paquete de la aplicación.
-   **Prueba de distinción entre mayúsculas y minúsculas del nombre de tipo:** comprueba que todos los tipos de UWP tengan nombres únicos sin distinción de mayúsculas y minúsculas en el paquete de la aplicación. Asegúrate también de que ningún nombre de tipo UWP se use además como espacio de nombres de tu paquete de la aplicación.
-   **Prueba de exactitud del nombre de tipo:** comprueba que no haya tipos de UWP en el espacio de nombres global ni en el espacio de nombres de nivel superior de Windows.
-   **Prueba de exactitud de metadatos general:** comprueba que el compilador que usas para generar los tipos de UWP cumpla con las especificaciones actuales de Windows Runtime.
-   **Prueba de propiedades:** comprueba que las propiedades de una clase de UWP tengan un método get (los métodos set son opcionales). Asegúrate de que el método get devuelva un tipo de valor que coincida con el tipo del parámetro de entrada del método set, para todas las propiedades de los tipos de UWP.

## <a name="package-sanity-tests"></a>Pruebas de integridad del paquete

### <a name="platform-appropriate-files-test"></a>Prueba de archivos apropiados para la plataforma

Las aplicaciones que instalan binarios mixtos pueden bloquearse o no ejecutarse correctamente según la arquitectura del procesador que tenga el usuario.

### <a name="background"></a>Background

Esta prueba valida los binarios de un paquete de la aplicación para evitar conflictos de arquitectura. Un paquete de la aplicación no debe contener binarios que no se puedan usar en la arquitectura de procesador especificada en el manifiesto. De haberlos, existe la posibilidad de que la aplicación se bloquee o de que el tamaño del paquete de la aplicación se incremente sin necesidad.

### <a name="test-details"></a>Detalles de la prueba

Examina que el "valor de bits" de cada archivo en el encabezado PE sea el apropiado al hacer referencia cruzada con la declaración de arquitectura de procesador en el paquete de la aplicación.

### <a name="corrective-action"></a>Acción correctiva

Sigue estas directrices para que el paquete de la aplicación contenga únicamente archivos admitidos por la arquitectura especificada en el manifiesto de la aplicación:

-   Si la arquitectura de procesador de destino de la aplicación es de tipo neutral, el paquete de la aplicación no puede contener archivos de imagen o binarios de x86, x64 o ARM.

-   Si la arquitectura de procesador de destino de la aplicación es de tipo x86, el paquete de la aplicación solo debe contener archivos de imagen o binarios de x86. Si el paquete contiene archivos de imagen o binarios de x64 o ARM, la prueba no se superará.

-   Si la arquitectura de procesador de destino de la aplicación es de tipo x64, el paquete de la aplicación debe contener archivos de imagen o binarios de x64. Ten en cuenta que, en este caso, también puede contener archivos de x86, pero la experiencia de aplicación principal debe usar el binario de x64.

    La prueba no se superará, sin embargo, si el paquete contiene archivos de imagen o binarios de ARM o si solo contiene archivos de imagen o binarios de x86.

-   Si la arquitectura de procesador de destino de la aplicación es de tipo ARM, el paquete de la aplicación solo debe contener archivos de imagen o binarios de ARM. Si el paquete contiene archivos de imagen o binarios de x64 o x86, la prueba no se superará.

### <a name="supported-directory-structure-test"></a>Prueba de estructura de directorios admitida

Valida que las aplicaciones no crean subdirectorios como parte de la instalación que superan el máximo\-ruta de acceso.

### <a name="background"></a>Background

Los componentes del sistema operativo (incluido Trident, WWAHost, etc.) se limitan internamente a la ruta de acceso máxima\-para las rutas del sistema de archivos y no funcionarán correctamente para rutas de acceso más largas.

### <a name="test-details"></a>Detalles de la prueba

Comprueba que ninguna ruta de acceso dentro del directorio de instalación de la aplicación supera el máximo\-ruta de acceso.

### <a name="corrective-action"></a>Acción correctiva

Usa una estructura de directorio o nombre de archivo más cortos.

## <a name="resource-usage-test"></a>Prueba de uso de recursos

### <a name="winjs-background-task-test"></a>Prueba de tareas en segundo plano de WinJS

Esta prueba garantiza que las aplicaciones de JavaScript tengan las instrucciones de cierre adecuadas para que no consuman batería.

### <a name="background"></a>Background

Las aplicaciones que tienen tareas en segundo plano de JavaScript necesitan llamar a Close() en la última declaración de la tarea. De lo contrario, podrían impedir que el sistema vuelva al modo de espera conectado y agoten la batería.

### <a name="test-details"></a>Detalles de la prueba

Si la aplicación no cuenta con un archivo de tareas en segundo plano especificado en el manifiesto, pasará la prueba. De lo contrario, la prueba analizará el archivo de tareas en segundo plano de JavaScript especificado en el manifiesto de la aplicación y buscará la declaración Close(). En función de si encuentra o no esta declaración, se pasará o no la prueba.

### <a name="corrective-action"></a>Acción correctiva

Actualiza el código JavaScript en segundo plano para que llame a Close() correctamente.


## <a name="related-topics"></a>Temas relacionados

* [Pruebas de aplicaciones de puente de escritorio de Windows](windows-desktop-bridge-app-tests.md)
* [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 
