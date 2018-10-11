---
author: PatrickFarley
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: Pruebas de aplicación Puente de dispositivo de escritorio de Windows
description: Usa las pruebas integradas del puente de escritorio para garantizar que la aplicación de escritorio está optimizada para la conversión a una aplicación para UWP.
ms.author: pafarley
ms.date: 12/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, certificación de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: 96087d2a41eb443374d8cd9bda5608d6156f9173
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "4506783"
---
# <a name="windows-desktop-bridge-app-tests"></a>Pruebas de aplicación Puente de dispositivo de escritorio de Windows

[Las aplicaciones de puente de dispositivo de escritorio](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root) son aplicaciones de escritorio de Windows convertidas a las aplicaciones de la plataforma Universal de Windows (UWP) mediante el [Puente de escritorio](https://developer.microsoft.com/en-us/windows/bridges/desktop). Después de la conversión, la aplicación de escritorio de Windows se empaqueta, se le realiza un mantenimiento y se implementa en forma de un paquete de la aplicación para UWP (un archivo .appx o .appxbundle) destinado a escritorio de Windows10.

## <a name="required-versus-optional-tests"></a>Pruebas obligatorias vs. pruebas opcionales
Pruebas opcionales para las aplicaciones de puente de escritorio de Windows solo son informativas y no se usará para evaluar la aplicación durante la incorporación de Microsoft Store. Es recomendable investigar estos resultados para crear aplicaciones de mejor calidad de la prueba. Los criterios de aprobación o no aprobación general para la incorporación de la tienda se determinan según las pruebas obligatorias y no según estas pruebas opcionales.

## <a name="current-optional-tests"></a>Pruebas opcionales actuales

### <a name="1-digitally-signed-file-test"></a>1. Prueba de archivo firmado digitalmente 
**Antecedentes**  
Esta prueba verifica que todos los archivos portables ejecutables (PE) contienen una firma válida. La presencia de archivos firmados digitalmente permite a los usuarios saber que el software es original.

**Detalles de la prueba**  
La prueba examina todos los archivos portables ejecutables en el paquete y comprueba sus encabezados en busca de una firma. Se recomiendan que todos los archivos PE estén firmados digitalmente. Si alguno de los archivos PE no está firmado, se generará una advertencia.
 
**Acciones correctivas**  
Siempre se recomienda tener archivos firmados digitalmente. Para más información, consulta [Introducción a la firma de código](https://msdn.microsoft.com/en-us/library/ms537361(v=vs.85).aspx).

### <a name="2-file-association-verbs"></a>2. Verbos de asociación de archivos 
**Antecedentes**  
Esta prueba examina el Registro de paquete para comprobar si los verbos de asociación de archivo están registrados. 

**Detalles de la prueba**  
Las aplicaciones de escritorio convertidas pueden mejorarse con una amplia gama de API para la Plataforma universal de Windows. Esta prueba verifique que los archivos binarios para UWP en la aplicación no llaman a las API que no sean para UWP. Los archivos binarios para UWP tienen el indicador **AppContainer** establecido.

**Acciones correctivas**  
Consulta [Puente de dispositivo de escritorio a UWP: extensiones de aplicación](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-extensions) para ver una explicación de estas extensiones y cómo usarlas correctamente. 

### <a name="3-debug-configuration-test"></a>3. Prueba de configuración de depuración
Esta prueba verifica que la appx no es una versión de depuración.
 
**Antecedentes**  
Para lograr la certificación de Microsoft Store, las aplicaciones no deben compilarse para depuración y no deben hacer referencia a las versiones de depuración de un archivo ejecutable. Además, debes crear tu propio código según lo optimice tu aplicación para pasar esta prueba.
 
**Detalles de la prueba**  
Prueba la aplicación para asegurarte de que no sea una versión de depuración y no esté vinculada con ningún marco de depuración.
 
**Acciones correctivas**  
* Compila la aplicación como una versión de lanzamiento antes de enviarla a Microsoft Store.
* Asegúrate de tener instalada la versión correcta de .NET Framework.
* Asegúrate de que la aplicación no esté vinculada a versiones de depuración de un marco de trabajo y que esté creada con una versión de lanzamiento. Si la aplicación incluye componentes .NET, comprueba si has instalado la versión correcta de .NET Framework.

### <a name="4-package-sanity-test"></a>4. Pruebas de integridad del paquete
#### <a name="41-archive-files-usage"></a>4.1 Uso de los archivos de almacenamiento

**Antecedentes**  
Esta prueba te ayuda a crear aplicaciones mejoradas del Puente de dispositivo de escritorio para ejecutarse en equipos con [Windows 10 S](https://www.microsoft.com/windows/windows-10-s).

**Detalles de la prueba**  
Esta prueba busca todos los archivos ejecutables dentro de los archivos almacenados o con contenido autoextraíble. Como los archivos ejecutables dentro de este tipo de contenido no están firmados durante la incorporación a la Tienda Windows, es posible que la aplicación no funcione como está previsto en sistemas con Windows 10 S.
 
**Acciones correctivas**
* Evalúa los archivos marcados por la prueba para determinar si afecta a la aplicación que se ejecuta en un entorno con Windows 10 S.
* Si tu aplicación se viera afectada, quita los archivos ejecutables de los archivos almacenados y no uses los archivos autoextraíbles para colocar archivos ejecutables en disco. Esto debería evitar la pérdida de funcionalidad de la aplicación.

#### <a name="42-blocked-executables"></a>4.2 Ejecutables bloqueados

**Antecedentes**  
Esta prueba te ayuda a crear aplicaciones mejoradas del Puente de dispositivo de escritorio para ejecutarse en equipos con [Windows 10 S](https://www.microsoft.com/windows/windows-10-s). 

**Detalles de la prueba**  
Esta prueba comprueba si la aplicación está intentando iniciar archivos ejecutables, ya que esta acción está restringida en sistemas con Windows 10 S. Es posible que las aplicaciones que dependan de esta funcionalidad no funcionen como está previsto en los sistemas con Windows 10 S. 

**Acciones correctivas**  
* Identifica cuál de las entradas marcadas de la prueba representan una llamada para iniciar un archivo ejecutable que no forme parte de la aplicación y elimina estas llamadas. 
* Si los archivos marcados forman parte de la aplicación, puedes omitir la advertencia.


## <a name="current-required-tests"></a>Pruebas obligatorias actuales

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. Prueba de funcionalidades de la aplicación (funcionalidades de uso especial)

**Antecedentes**  
Las funcionalidades de uso especial están indicadas para escenarios muy específicos. Estas funcionalidades solo pueden usarse en cuentas de empresa. 

**Detalles de la prueba**  
Examinan si la aplicación declara alguna de las siguientes funcionalidades: 
* EnterpriseAuthentication
* SharedUserCertificates
* DocumentsLibrary

Si se declara alguna de estas funcionalidades, la prueba muestra una advertencia al usuario. 

**Acciones correctivas**  
Piensa en la posibilidad de quitar la función de uso especial si la aplicación no la necesita. Además, el uso de estas funcionalidades está sujeto a otras revisiones de las directivas de incorporación.

### <a name="2-app-manifest-resources-tests"></a>2. Pruebas de recursos del manifiesto de la aplicación 
#### <a name="21-app-resources-validation"></a>2.1 Validación de recursos de la aplicación
Es posible que la aplicación no se instale correctamente si las cadenas o imágenes declaradas en el manifiesto de la aplicación son incorrectas. Si se instala con estos errores, es posible que el logotipo de la aplicación u otras imágenes no se muestren correctamente.    

**Detalles de la prueba**  
Inspecciona los recursos definidos en el manifiesto de la aplicación para asegurarse de que estén presentes y sean válidos.

**Acción correctiva**  
Usa la siguiente tabla como guía.

Mensaje de error | Comentarios
--------------|---------
La imagen {image name} define ambos calificadores Scale y TargetSize; puedes definir un calificador por vez. | Puedes personalizar imágenes para distintas resoluciones. En el mensaje en sí, {image name}contiene el nombre de la imagen que presenta el error. Asegúrate de que cada imagen defina Scale o TargetSize como calificador. 
La imagen {image name} no cumple con las restricciones de tamaño.  | Asegúrate de que todas las imágenes de la aplicación cumplan con las restricciones de tamaño apropiado. En el mensaje en sí, {image name} contiene el nombre de la imagen que presenta el error. 
La imagen {image name} no se encuentra en el paquete.  | Falta una imagen obligatoria. En el mensaje en sí, {image name} contiene el nombre de la imagen que falta. 
La imagen {image name} no es un archivo de imagen válido.  | Asegúrate de que todas las imágenes de la aplicación cumplan con las restricciones de tipo de formato de archivo apropiado. En el mensaje en sí, {image name}contiene el nombre de la imagen que no es válida. 
La imagen “BadgeLogo” tiene un valor ABGR {value} en la posición (x, y) que no es válido. El píxel debe ser blanco (##FFFFFF) o transparente (00######).  | El logotipo del distintivo es una imagen que se muestra junto a la notificación del distintivo para identificar la aplicación en la pantalla de bloqueo. Esta imagen debe ser monocromática (puede tener únicamente píxeles blancos o transparentes). En el mensaje en sí, {value} contiene un valor de color en la imagen que no es válido. 
La imagen "BadgeLogo" tiene un valor ABGR {value} en la posición (x, y) que no es válido para una imagen con blanco en contraste alto. El píxel debe ser (##2A2A2A) o más oscuro, o transparente (00######).  | El logotipo del distintivo es una imagen que se muestra junto a la notificación del distintivo para identificar la aplicación en la pantalla de bloqueo. Dado que el logotipo del distintivo aparece en un fondo blanco cuando se usa blanco en contraste alto, debe ser una versión oscura del logotipo del distintivo normal. En blanco de alto contraste, el logotipo del distintivo solo puede contener píxeles que son más oscuros que (##2A2A2A) o transparentes. En el mensaje en sí, {value} contiene un valor de color en la imagen que no es válido. 
La imagen debe definir al menos una variante sin un calificador TargetSize. Debe definir un calificador Scale o dejar Scale y TargetSize sin especificar, para que de manera predeterminada se establezca Scale-100.  | Para más información, consulta las guías sobre el [diseño adaptativo](https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx) y los [recursos de la aplicación](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data). 
Al paquete le falta un archivo "resources.pri".  | Si tienes contenido localizable en el manifiesto de la aplicación, asegúrate de que el paquete de la aplicación incluya un archivo resources.pri válido. 
El archivo "resources.pri" debe tener un mapa de recursos cuyo nombre coincida con el nombre del paquete {package full name}.  | Este error puede obtenerse si el manifiesto cambió y el nombre del mapa de recursos del archivo resources.pri deja de coincidir con el nombre del paquete en el manifiesto. En el mensaje en sí, {package full name} contiene el nombre del paquete que resources.pri debe tener. Para corregir esto, debes reconstruir resources.pri y la forma más fácil de hacerlo es reconstruyendo el paquete de la aplicación. 
El archivo "resources.pri" no debe tener habilitado Combinar automáticamente.  | MakePRI.exe admite una opción denominada Combinar automáticamente. El valor predeterminado de Combinar automáticamente es desactivado. Cuando está habilitado, Combinar automáticamente combina los recursos del paquete de idioma de una aplicación en un solo resources.pri en tiempo de ejecución. No recomendamos esto para las aplicaciones que intentas distribuir a través de Microsoft Store. El archivo resources.pri de una aplicación que se distribuye a través de Microsoft Store debe estar en la raíz del paquete de la aplicación y contener todas las referencias de idioma que admita la aplicación. 
La cadena {string} no cumple la restricción de longitud máxima de {number} caracteres.  | Consulta los [requisitos de metadatos del paquete](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). En el mensaje en sí, {string} se reemplaza por la cadena que tiene el error y {number} contiene la longitud máxima. 
La cadena {string} no debe tener espacios en blanco iniciales ni finales.  | El esquema de los elementos en el manifiesto de la aplicación no permite caracteres de espacio en blanco inicial ni final. En el mensaje en sí, {string} se reemplaza por la cadena que tiene el error. Asegúrate de que ninguno de los valores localizados en los campos del manifiesto en resources.pri tenga caracteres de espacio en blanco inicial o final. 
La cadena debe ser no vacía (mayor que cero en longitud).  | Consulta el tema sobre los [requisitos del paquete de la aplicación](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements) para más información. 
No hay un recurso predeterminado especificado en el archivo "resources.pri".  | Para más información, consulta la guía sobre [recursos de la aplicación](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data). En la configuración de compilación predeterminada, Visual Studio solo incluye los recursos de imagen con una escala de 200 en el paquete de la aplicación al generar paquetes y coloca otros recursos en el paquete de recursos. Asegúrate de incluir recursos de imagen con una escala de 200 o de configurar el proyecto para incluir los recursos que tienes. 
No hay un valor de recurso especificado en el archivo "resources.pri".  | Asegúrate de que el manifiesto de la aplicación tenga recursos definidos en dicho archivo. 
El archivo de imagen {filename} debe tener un tamaño inferior a 204800bytes.  | Reduce el tamaño de las imágenes especificadas. 
El archivo {filename} no debe contener una sección de mapa inverso.  | Aunque el mapa inverso se genera durante la tarea "F5 debugging" de Visual Studio al llamar a makepri.exe, se puede quitar ejecutando makepri.exe sin el parámetro /m al generar un archivo pri. 



#### <a name="22-branding-validation"></a>2.2 Validación de la personalización de marca
**Antecedentes**  
Se espera que las aplicaciones Puente de dispositivo de escritorio estén completas y sean absolutamente funcionales. Las aplicaciones que usan imágenes predeterminadas (de plantillas o muestras del SDK) ofrecen una experiencia mediocre al usuario y son difíciles de identificar en el catálogo de la tienda.

**Detalles de la prueba**  
La prueba corrobora que las imágenes usadas por la aplicación no sean imágenes predeterminadas de muestras de SDK ni de Visual Studio. 

**Acciones correctivas**  
Reemplaza las imágenes predeterminadas con otras más distintivas y representativas de tu aplicación.

### <a name="3-package-compliance-tests"></a>3. Pruebas de cumplimiento del paquete
#### <a name="31-app-manifest"></a>3.1 Manifiesto de la aplicación
Prueba el contenido del manifiesto de la aplicación para asegurarte de que su contenido es correcto.

**Antecedentes**  
Las aplicaciones deben tener un manifiesto de la aplicación con el formato correcto.

**Detalles de la prueba**  
Examina el manifiesto de la aplicación para comprobar que el contenido sea correcto, según se describe en los [Requisitos del paquete de la aplicación](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). En esta prueba se realizan las siguientes comprobaciones:
* **Extensiones de archivo y protocolos**  
La aplicación puede declarar los tipos de archivo a los que se puede asociar. Una declaración de un gran número de tipos de archivo poco comunes provoca una experiencia de usuario deficiente. Esta prueba limita el número de extensiones de archivo con las que puede asociarse una aplicación.
* **Regla de dependencia de marco**  
Esta prueba aplica el requisito de que las aplicaciones declaren las dependencias adecuadas en UWP. Si se encuentra una dependencia inadecuada, la aplicación no superará la prueba. Si hay un error de coincidencia entre la versión del sistema operativo a la que se dirige la aplicación y las dependencias de marco establecidas, la aplicación no superará la prueba. Tampoco lo hará si hace referencia a alguna "versión preliminar " de los archivos dll del marco.
* **Comprobación de la comunicación entre procesos (IPC)**  
Esta prueba impone el requisito de que las aplicaciones Puente de dispositivo de escritorio no se comunican fuera del contenedor de la aplicación con componentes del escritorio. La comunicación entre procesos está pensada exclusivamente para las aplicaciones de prueba. Las aplicaciones en las que el nombre especificado en [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) sea `DesktopApplicationPath` no superarán esta prueba.  

**Acción correctiva**  
Compara el manifiesto de la aplicación con los requisitos descritos en la página sobre los [Requisitos del paquete de la aplicación](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements).


#### <a name="32-application-count"></a>3.2 Recuento de aplicaciones
Esta prueba verifica que el paquete de aplicaciones (.appx, lote de aplicaciones) contiene una aplicación. 

**Antecedentes**  
Esta prueba se implementa según la directiva de la Tienda. 

**Detalles de la prueba**  
Esta prueba verifica que el número total de paquetes .appx del lote es menor que 512 y que hay un único paquete "principal" en el lote. También comprueba que el número de revisión de la versión del lote se estableció en 0. 

**Acciones correctivas**  
Asegúrese de que el paquete de la aplicación y el lote cumplen con requisitos que se indican en la sección **Detalles de la prueba**.


#### <a name="33-registry-checks"></a>3.3 Comprobaciones del Registro
**Antecedentes**  
Esta prueba verifica si la aplicación instala o actualiza nuevos servicios o controladores.

**Detalles de la prueba**  
La prueba busca dentro del archivo registry.dat actualizaciones a ubicaciones específicas del Registro que indican un nuevo servicio o controlador que se va a registrar. Si la aplicación intenta instalar un controlador o servicio, no se superará la prueba.  

**Acciones correctivas**  
Revisa los errores y quita los servicios o controladores en cuestión, si no son necesarios. Si la aplicación depende de estos, tendrás que modificar la aplicación si quieres incorporarla a la tienda.


### <a name="4-platform-appropriate-files-test"></a>4. Prueba de archivos apropiados para la plataforma
Las aplicaciones que instalan binarios mixtos pueden bloquearse o no ejecutarse correctamente en la arquitectura del procesador que tenga el usuario. 

**Antecedentes**  
Esta prueba examina los binarios de un paquete de la aplicación para ver si existen conflictos de arquitectura. Un paquete de la aplicación no debe contener binarios que no se puedan usar en la arquitectura de procesador especificada en el manifiesto. De haberlos, existe la posibilidad de que la aplicación se bloquee o de que el tamaño del paquete de la aplicación se incremente sin necesidad. 

**Detalles de la prueba**  
Valida que el "valor de bits" de cada archivo en el encabezado portable ejecutable sea el apropiado al hacer referencia cruzada con la declaración de arquitectura de procesador en el paquete de la aplicación. 

**Acciones correctivas**  
Sigue estas directrices para que el paquete de la aplicación contenga únicamente archivos admitidos por la arquitectura especificada en el manifiesto de la aplicación: 
* Si la arquitectura de procesador de destino de la aplicación es de tipo procesador neutral, el paquete de la aplicación no puede contener archivos de imagen o binarios de x86, x64 o ARM.
* Si la arquitectura de procesador de destino de la aplicación es de tipo x86, el paquete de la aplicación solo debe contener archivos de imagen o binarios de x86. Si el paquete contiene archivos de imagen o binarios de x64 o ARM, la prueba no se superará.
* Si la arquitectura de procesador de destino de la aplicación es de tipo x64, el paquete de la aplicación debe contener archivos de imagen o binarios de x64. Ten en cuenta que, en este caso, también puede contener archivos de x86, pero la experiencia de aplicación principal debe usar el binario de x64. La prueba no se superará si el paquete contiene archivos de imagen o binarios de ARM o si *solo* contiene archivos de imagen o binarios de x86.
* Si la arquitectura de procesador de destino de la aplicación es de tipo ARM, el paquete de la aplicación solo debe contener archivos de imagen o binarios de ARM. Si el paquete contiene archivos de imagen o binarios de x64 o x86, la prueba no se superará. 

### <a name="5-supported-api-test"></a>5. Prueba de API admitidas
Comprueba la aplicación para detectar el uso de cualquier API no compatible. 

**Antecedentes**  
Las aplicaciones Puente de dispositivo de escritorio pueden aprovechar algunas API de Win32 heredadas junto con API modernas (componentes para UWP). Esta prueba identifica los archivos binarios administrados que usan API no compatibles.
 
**Detalles de la prueba**  
Esta prueba verifica todos los componentes para UWP en la aplicación:
* Comprueba que cada archivo binario administrado dentro del paquete de la aplicación no tiene una dependencia en una API de Win32 que no se admite para el desarrollo de aplicaciones para UWP mediante la tabla de direcciones de importación del binario.
* Comprueba que cada binario administrado en el paquete de la aplicación no toma una dependencia de una función fuera del perfil aprobado. 

**Acciones correctivas**  
Esto puede corregirse al asegurarte de que la aplicación se haya compilado como una versión de lanzamiento y no como una versión de depuración. 

> [!NOTE]
> La versión de depuración de una aplicación pasará esta prueba, incluso si la aplicación usa solo [las API para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx). Revisa los mensajes de error para identificar la API presentan que no es una API permitida para aplicaciones para UWP. 

> [!NOTE]
> Las aplicaciones C++ creadas en una configuración de depuración pasará esta prueba, incluso si la configuración use solo las API del Windows SDK para aplicaciones para UWP. Para obtener más información, consulta [alternativas a las API de Windows en aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx) .

### <a name="6-user-account-control-uac-test"></a>6. Prueba de control de cuentas de usuario (UAC)  

**Antecedentes**  
Asegura que la aplicación no solicita el control de cuentas de usuario en tiempo de ejecución.

**Detalles de la prueba**  
Una aplicación no puede solicitar la elevación de administrador o UIAccess por la directiva de Microsoft Store. No se admiten permisos de seguridad con privilegios elevados. 

**Acciones correctivas**  
Las aplicaciones deben ejecutarse como usuario interactivo. Para más información, consulta [UI Automation Security Overview (Introducción a la seguridad de la automatización de la interfaz de usuario](https://go.microsoft.com/fwlink/?linkid=839440).

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Validación de metadatos de Windows Runtime
**Antecedentes**  
Comprueba que los componentes suministrados en una aplicación corresponden al sistema de tipo de UWP.

**Detalles de la prueba**  
Esta prueba produce un número de indicadores relacionados al uso de tipo adecuado.

**Acciones correctivas**  
* **Atributo ExclusiveTo**  
Asegúrate de que las clases UWP no implementen interfaces marcadas como ExclusiveTo para otra clase.
* **Exactitud de metadatos general**  
Comprueba que el compilador que usas para generar los tipos esté actualizado con las especificaciones de UWP.
* **Propiedades**  
Asegúrate de que todas las propiedades de una clase de UWP tengan un método `get` (los métodos `set` son opcionales). Para todas las propiedades, asegúrate de que el tipo que el método `get`devuelve coincide con el tipo del parámetro de entrada del método `set`.
* **Ubicación del tipo**  
Comprueba que los metadatos de todos los tipos de UWP estén ubicados en el archivo .winmd cuyo nombre coincida en longitud con el espacio de nombres del paquete de la aplicación.
* **Distinción entre mayúsculas y minúsculas del nombre de tipo**  
Comprueba que todos los tipos de UWP tengan nombres únicos con distinción entre mayúsculas y minúsculas en el paquete de la aplicación. Asegúrate también de que ningún nombre de tipo UWP se use además como espacio de nombres de tu paquete de la aplicación.
* **Exactitud del nombre de tipo**  
Comprueba que no hay tipos de UWP en el espacio de nombres global ni en el espacio de nombres de nivel superior de Windows.
 

### <a name="8-windows-security-features-tests"></a>8. Pruebas de características de seguridad de Windows
Cambiar las protecciones de seguridad de Windows predeterminadas puede causar grandes riesgos a los clientes. 

#### <a name="81-banned-file-analyzer"></a>8.1 Analizador de archivos prohibidos
**Antecedentes**  
Algunos archivos se actualizaron con importantes mejoras de seguridad, confiabilidad y otras. Las aplicaciones Puente de dispositivo de escritorio de Windows deben contener las versiones más recientes estos archivos, dado que las versiones obsoletas presentan un riesgo. El Kit para la certificación de aplicaciones en Windows bloquea estos archivos con el fin de garantizar que todas las aplicaciones usen una versión actual.

**Detalles de la prueba**  
La comprobación de archivos prohibidos del Kit para la certificación de aplicaciones en Windows comprueba actualmente los siguientes archivos:
* *Bing.Maps.JavaScript\js\veapicore.js*  
Esta comprobación suele producir un error cuando una aplicación usa una versión preliminar del archivo en lugar de la versión oficial más reciente. 

**Acciones correctivas**  
Para corregir esto, usa la versión más reciente del [SDK de mapas de Bing](http://go.microsoft.com/fwlink/p/?linkid=614880) para aplicaciones para UWP.

#### <a name="82-private-code-signing"></a>8.2 Firma de código privado
Prueba la existencia de archivos binarios de firma de código privado en el paquete de la aplicación. 

**Antecedentes**  
Los archivos de firma de código privado se deben conservar de forma privada, ya que podrían usarse de forma malintencionada en caso de que se pongan en riesgo. 

**Detalles de la prueba**  
Busca archivos dentro del paquete de la aplicación que tengan la extensión .pfx o.snk, lo que indicaría que se incluyen claves de firma privadas. 

**Acciones correctivas**  
Quita todas las claves de firma de código privado (por ejemplo, archivos .pfx y .snk) del paquete.


## <a name="related-topics"></a>Temas relacionados

* [Directivas de Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Dn764944)
