---
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: API de JavaScript "Hacer un examen".
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: Windows 10, uwp, educación
ms.localizationpriority: medium
ms.openlocfilehash: bee8a04e3b4d57caf7da3e21f2be3c789d83be90
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9049372"
---
# <a name="take-a-test-javascript-api"></a>API de JavaScript "Hacer un examen"

[Hacer un examen](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) es una aplicación para UWP basada en explorador que representa el bloqueo de evaluaciones en línea para las pruebas determinantes, lo que permite educadores centrarse en la evaluación de contenido en lugar de cómo proporcionar un entorno de prueba seguro. Para ello, usa una API de JavaScript que cualquier aplicación web puede usar. La API de Hacer un examen admite la [API estándar del explorador de SBAC](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) para las principales pruebas determinantes que suelen llevarse a cabo.

Si quieres obtener información acerca de la propia aplicación, consulta [Referencia técnica de la aplicación Hacer un examen)](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). Para solucionar cualquier problema, consulta [Solucionar problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos](troubleshooting.md).

## <a name="reference-documentation"></a>Documentación de referencia
Las APIs de Hacer un examen existen en los siguientes espacios de nombres. Ten en cuenta que todas las API dependen de un objeto `SecureBrowser` global.

| Espacio de nombres | Descripción |
|-----------|-------------|
|[espacio de nombres de seguridad](#security-namespace)|Contiene API que te permiten bloquear el dispositivo para pruebas y aplicar un entorno de pruebas. |

### <a name="security-namespace"></a>Espacio de nombres de seguridad

El espacio de nombres de seguridad te permite bloquear el dispositivo, comprobar la lista de procesos de usuario y del sistema, obtener direcciones IP y MAC y borrar los recursos web guardados en caché.

| Método | Descripción   |
|--------|---------------|
|[lockDown](#lockDown) | Bloquea el dispositivo para pruebas. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Determina si el contexto de bloqueo aún se aplica al dispositivo. |
|[getDeviceInfo](#getDeviceInfo) | Obtén información acerca de la plataforma en el que se ejecuta la aplicación de prueba. |
|[examineProcessList](#examineProcessList)|Obtiene la lista de procesos del usuario y del sistema en ejecución.|
|[cerrar](#close) | Cierra el explorador y se desbloquea el dispositivo. |
|[getPermissiveMode](#getPermissiveMode)|Comprueba si el modo permisivo está activado o desactivado.|
|[setPermissiveMode](#setPermissiveMode)|Activa o desactiva el modo permisivo.|
|[emptyClipBoard](#emptyClipBoard)|Borra el portapapeles del sistema.|
|[getMACAddress](#getMACAddress)|Obtiene la lista de direcciones MAC del dispositivo.|
|[getStartTime](#getStartTime) | Obtiene la hora en que se inició la aplicación de prueba. |
|[getCapability](#getCapability) | Realiza una consulta de si una funcionalidad está habilitada o deshabilitada. |
|[setCapability](#setCapability)|Habilita o deshabilita la funcionalidad especificada.| 
|[isRemoteSession](#isRemoteSession) | Comprueba si la sesión actual se registra de manera remota. |
|[isVMSession](#isVMSession) | Comprueba si la sesión actual se ejecuta en una máquina virtual. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
Bloquea el dispositivo. También se usa para desbloquear el dispositivo. La aplicación web de pruebas invocará a esta llamada antes de permitir a los estudiantes que inicien las pruebas. El implementador es necesario para realizar todas las acciones necesarias para proteger el entorno de pruebas. Los pasos realizados para proteger el entorno son específicos de dispositivo y, por ejemplo, incluyen aspectos como deshabilitar capturas de pantalla, deshabilitar el chat de voz cuando se está en modo seguro, borrar el Portapapeles del sistema, entrar en un modo de pantalla completa, espacios en dispositivos OSX 10.7, etc. La aplicación de prueba habilitará el bloqueo antes de que comience una evaluación y deshabilitará el bloqueo cuando el estudiante haya completado la evaluación y se encuentre fuera de la prueba segura.

**Sintaxis**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Parámetros**  
* `enable` - **true** para ejecutar la aplicación Hacer un examen pasando por alto la pantalla de bloqueo y aplicar las directivas descritas en este [documento](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** detiene la ejecución de Hacer un examen pasando por alto la pantalla de bloqueo y se cierra a menos que la aplicación no esté bloqueada, en cuyo caso el parámetro no tendrá ningún efecto.  
* `onSuccess` - [opcional] Se ha habilitado o deshabilitado correctamente la función para llamar después del bloqueo. Debe tener la forma `Function(Boolean currentlockdownstate)`.  
* `onError` - [opcional] La función para llamar si la operación de bloqueo generó error. Debe tener la forma `Function(Boolean currentlockdownstate)`.  

**Requisitos**  
Windows 10, versión 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Determina si el contexto de bloqueo aún se aplica al dispositivo. La aplicación web de pruebas invocará esto antes de permitir a los estudiantes que inicien las pruebas y periódicamente, cuando se esté dentro de la prueba.

**Sintaxis**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Parámetros**  
* `callback` - La función para llamar cuando esta función finaliza. Debe tener la forma `Function(String state)` donde `state` es una cadena JSON que contiene dos campos. El primero es el campo `secure`, que solo mostrará `true` si se han habilitado todos los bloqueos necesarios (o se han deshabilitado las características) para habilitar un entorno de prueba seguro y ninguno de estos se ha puesto en peligro puesto que la aplicación entró en el modo de bloqueo. El otro campo, `messageKey`, incluye otros detalles o información que son específicos del proveedor. El objetivo aquí es permitir que los proveedores coloquen información adicional que aumenta la marca `secure` booleana:

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Requisitos**  
Windows 10, versión 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
Obtén información acerca de la plataforma en la que se ejecuta la aplicación de prueba. Esto se utiliza para ampliar cualquier información que sea apreciable desde el agente de usuario.

**Sintaxis**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Parámetros**  
* `callback` - La función para llamar cuando esta función finaliza. Debe tener la forma `Function(String infoObj)` donde `infoObj` es una cadena JSON que contiene varios campos. Se deben admitir los siguientes campos:
    * `os` representa el tipo de sistema operativo (por ejemplo: Windows, macOS, Linux, iOS, Android, etc.)
    * `name` representa el nombre de la versión del sistema operativo, si la hay (por ejemplo: Sierra, Ubuntu).
    * `version` representa la versión del sistema operativo (por ejemplo: 10,1, 10 Pro, etc.)
    * `brand` representa la personalización de marca de explorador seguro (por ejemplo: OAKS, CA, SmarterApp, etc.)
    * `model` representa el modelo de dispositivo para dispositivos móviles. nulo o no se usa para exploradores de escritorio.

**Requisitos**  
Windows 10, versión 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Obtiene la lista de todos los procesos que se ejecutan en el equipo cliente propiedad del usuario. La aplicación de prueba invocará esto para examinar la lista y compararla con una lista de procesos que se han considerado de la lista negra durante el ciclo de pruebas. Esta llamada se debe invocar tanto al inicio de una evaluación como periódicamente mientras el alumno realiza la evaluación. Si se detecta un proceso incluido en la lista negra, se deberá detener la evaluación para conservar la integridad de la prueba.

**Sintaxis**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Parámetros**  
* `blacklistedProcessList` - La lista de procesos que la aplicación de prueba ha incluido en la lista negra.  
`callback` - La función que se invocará una vez que se han encontrado los procesos activos. Debe tener el formato: `Function(String foundBlacklistedProcesses)` donde `foundBlacklistedProcesses` tiene el formato: `"['process1.exe','process2.exe','processEtc.exe']"`. Estará vacío si no se encontró ningún proceso de la lista negra. Si es nulo, esto indica que se ha producido un error en la llamada de función original.

**Observaciones** La lista no incluye los procesos del sistema.

**Requisitos**  
Windows 10, versión 1709

---

<span id="close"/>

### <a name="close"></a>cerrar
Cierra el explorador y se desbloquea el dispositivo. La aplicación de prueba debe invocar esto cuando el usuario elige salir del explorador.

**Sintaxis**  
`void SecureBrowser.security.close(restart);`

**Parámetros**  
* `restart` - Este parámetro se pasa por alto, pero debe proporcionarse.

**Comentarios** En Windows 10, versión 1607, el dispositivo debe bloquearse inicialmente. En versiones posteriores, este método cierra el explorador, independientemente de si el dispositivo está bloqueado.

**Requisitos**  
Windows 10, versión 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
La aplicación web de prueba debe invocar esto para determinar si el modo permisivo está activado o desactivado. En el modo permisivo, se espera que los exploradores relajen algunos de sus ganchos de seguridad más estrictos para permitir que la tecnología de asistencia funcione con el explorador seguro. Por ejemplo, es posible que los exploradores que impiden de manera agresiva que otras interfaces de usuario de aplicación se presenten encima de ellos quieran relajar esto cuando se encuentren en el modo permisivo. 

**Sintaxis**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Parámetros**  
* `callback` - La función para invocar cuando esta llamada finalice. Debe tener el formato: `Function(Boolean permissiveMode)` donde `permissiveMode` indica si el explorador se encuentra actualmente en el modo permisivo. Si no está definido o es nulo, se produjo un error en la operación get.

**Requisitos**  
Windows 10, versión 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
La aplicación web de prueba debe invocar esto para pasar de la activación a la desactivación, o viceversa, del modo permisivo. En el modo permisivo, se espera que los exploradores relajen algunos de sus ganchos de seguridad más estrictos para permitir que la tecnología de asistencia funcione con el explorador seguro. Por ejemplo, es posible que los exploradores que impiden de manera agresiva que otras interfaces de usuario de aplicación se presenten encima de ellos quieran relajar esto cuando se encuentren en el modo permisivo. 

**Sintaxis**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Parámetros**  
* `enable` - El valor booleano que indica el estado del modo permisivo previsto.  
* `callback` - La función para invocar cuando esta llamada finalice. Debe tener el formato: `Function(Boolean permissiveMode)` donde `permissiveMode` indica si el explorador se encuentra actualmente en el modo permisivo. Si no está definido o es nulo, se produjo un error en la operación set.

**Requisitos**  
Windows 10, versión 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Borra el portapapeles del sistema. La aplicación de prueba debe invocar esto para forzar el borrado de datos que se pueden guardar en el Portapapeles del sistema. La función **[lockDown](#lockDown)** también realiza esta operación.

**Sintaxis**  
`void SecureBrowser.security.emptyClipBoard();`

**Requisitos**  
Windows 10, versión 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Obtiene la lista de direcciones MAC del dispositivo. L aplicación de prueba debe invocar esto para ayudar en el diagnóstico. 

**Sintaxis**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Parámetros**  
* `callback` - La función para invocar cuando esta llamada finalice. Debe tener el formato: `Function(String addressArray)` donde `addressArray` tiene el formato: `"['00:11:22:33:44:55','etc']"`.

**Observaciones**  
Es difícil depender de las direcciones IP de origen para distinguir entre los equipos de usuario final dentro de los servidores de pruebas porque los firewalls/NAT/servidores proxy se usan habitualmente en las escuelas. Las direcciones MAC permiten que la aplicación distinga entre equipos de cliente final detrás de un firewall común para fines de diagnóstico.

**Requisitos**  
Windows 10, versión 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Obtiene la hora en que se inició la aplicación de prueba.

**Sintaxis**  
`DateTime SecureBrowser.settings.getStartTime();`

**Return**  
Un objeto DateTime que indica la hora en que se inició la aplicación de prueba.

**Requisitos**  
Windows 10, versión 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Realiza una consulta de si una funcionalidad está habilitada o deshabilitada. 

**Sintaxis**  
`Object SecureBrowser.security.getCapability(String feature)`

**Parámetros**  
`feature` - La cadena para determinar qué capacidad se debe consultar. Las cadenas de funcionalidad válidas son "screenMonitoring", "printing" y "textSuggestions" (no distingue mayúsculas de minúsculas).

**Valor devuelto**  
Esta función devuelve un objeto JavaScript o literal con el formato: `{<feature>:true|false}`. **true** si la funcionalidad consultada está habilitada, **false** si la capacidad no está habilitada o la cadena de funcionalidad no es válida.

**Requisitos** Windows 10, versión 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Habilita o deshabilita una funcionalidad específica del explorador.

**Sintaxis**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Parámetros**  
* `feature` - La cadena para determinar qué capacidad se debe establecer. Las cadenas de funcionalidad válidas son `"screenMonitoring"`, `"printing"` y `"textSuggestions"` (no distingue mayúsculas de minúsculas).  
* `value` - La configuración prevista para la característica. Debe ser `"true"` o `"false"`.  
* `onSuccess` - [opcional] La función para llamar después de la operación set ha finalizado correctamente. Debe tener el formato `Function(String jsonValue)` donde *jsonValue* tiene el formato: `{<feature>:true|false|undefined}`.  
* `onError` - [opcional] La función para llamar si la operación set generó error. Debe tener el formato `Function(String jsonValue)` donde *jsonValue* tiene el formato: `{<feature>:true|false|undefined}`.

**Comentarios**  
Si el explorador desconoce la característica específica, esta función pasará un valor de `undefined` a la función de devolución de llamada.

**Requisitos** Windows 10, versión 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Comprueba si la sesión actual se registra de manera remota.

**Sintaxis**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Valor devuelto**  
**true** si la sesión actual es remota; de lo contrario, **false**.

**Requisitos**  
Windows 10, versión 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Comprueba si la sesión actual se ejecuta en una máquina virtual.

**Sintaxis**  
`Boolean SecureBrowser.security.isVMSession();`

**Valor devuelto**  
**true** si la sesión actual se ejecuta en una máquina virtual; de lo contrario, **false**.

**Comentarios**  
Esta comprobación de API solo puede detectar sesiones de VM que se ejecutan en determinados hipervisores que implementan las API adecuadas

**Requisitos**  
Windows 10, versión 1709

---