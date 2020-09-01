---
Description: La API de JavaScript de la aplicación "Hacer un examen" de Microsoft, te permite proteger los exámenes. Gracias a "Hacer un examen", tendrás a mano un navegador seguro que evitará que los estudiantes usen otro equipo o Internet durante un examen.
title: API de JavaScript "Hacer un examen".
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: windows 10, uwp, education
ms.localizationpriority: medium
ms.openlocfilehash: f5894e80c11d69c91be8492b80c3200e15a3dc31
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161269"
---
# <a name="take-a-test-javascript-api"></a>API de JavaScript "Hacer un examen"

[Realizar una prueba](/education/windows/take-tests-in-windows-10) es una aplicación para UWP basada en explorador que representa evaluaciones en línea bloqueadas para pruebas de gran interés, lo que permite a los educadores centrarse en el contenido de la evaluación en lugar de en cómo proporcionar un entorno de prueba seguro. Para lograrlo, usa una API de JavaScript que cualquier aplicación web puede usar. La API Take-a-test es compatible con el [estándar de API del explorador de SBAC](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) para un alto juego de pruebas básicas.

Vea la [referencia técnica](/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) de la aplicación de prueba para obtener más información sobre la propia aplicación. Para solucionar cualquier problema, consulta [Solucionar problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos](troubleshooting.md).

## <a name="reference-documentation"></a>Documentación de referencia
Las API Take a test existen en los siguientes espacios de nombres. Tenga en cuenta que todas las API dependen de un `SecureBrowser` objeto global.

| Espacio de nombres | Descripción |
|-----------|-------------|
|[espacio de nombres de seguridad](#security-namespace)|Contiene API que le permiten bloquear el dispositivo para probar y aplicar un entorno de pruebas. |

### <a name="security-namespace"></a>Espacio de nombres de seguridad

El espacio de nombres de seguridad permite bloquear el dispositivo, comprobar la lista de procesos de usuario y del sistema, obtener direcciones MAC y IP y borrar recursos web almacenados en caché.

| Método | Descripción   |
|--------|---------------|
|[Bloqueo](#lockDown) | Bloquea el dispositivo para las pruebas. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Determina si el contexto de bloqueo aún se aplica al dispositivo. |
|[getDeviceInfo](#getDeviceInfo) | Obtiene detalles sobre la plataforma en la que se ejecuta la aplicación de prueba. |
|[examineProcessList](#examineProcessList)|Obtiene la lista de procesos de usuario y del sistema en ejecución.|
|[cercanos](#close) | Cierra el explorador y se desbloquea el dispositivo. |
|[getPermissiveMode](#getPermissiveMode)|Comprueba si el modo permisivo está activado o desactivado.|
|[setPermissiveMode](#setPermissiveMode)|Activa o desactiva el modo permisivo.|
|[emptyClipBoard](#emptyClipBoard)|Borra el Portapapeles del sistema.|
|[getMACAddress](#getMACAddress)|Obtiene la lista de direcciones MAC del dispositivo.|
|[getStartTime](#getStartTime) | Obtiene la hora a la que se inició la aplicación de prueba. |
|[getCapability](#getCapability) | Consulta si una funcionalidad está habilitada o deshabilitada. |
|[setCapability](#setCapability)|Habilita o deshabilita la capacidad especificada.| 
|[isRemoteSession](#isRemoteSession) | Comprueba si la sesión actual ha iniciado sesión de forma remota. |
|[isVMSession](#isVMSession) | Comprueba si la sesión actual se está ejecutando en una máquina virtual. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>Bloqueo
Bloquea el dispositivo. También se usa para desbloquear el dispositivo. La aplicación Web de prueba invocará esta llamada antes de permitir que los alumnos inicien las pruebas. El implementador debe tomar las medidas necesarias para proteger el entorno de pruebas. Los pasos que se llevan a cabo para proteger el entorno son específicos del dispositivo y, por ejemplo, incluyen aspectos como deshabilitar las capturas de pantalla, deshabilitar el chat de voz en modo seguro, borrar el Portapapeles del sistema, entrar en un modo de quiosco, deshabilitar espacios en dispositivos OSX/r, etc. La aplicación de prueba habilitará el bloqueo antes de que se inicie una evaluación y deshabilitará el bloqueo cuando el estudiante haya completado la evaluación y esté fuera de la prueba segura.

**Sintaxis**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Parámetros**  
* `enable` - **true** para ejecutar la aplicación de Take-a-test encima de la pantalla de bloqueo y aplicar las directivas descritas en este [documento](https://docs.microsoft.com/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** detiene la ejecución de Take-a-test sobre la pantalla de bloqueo y la cierra a menos que la aplicación no esté bloqueada. en cuyo caso no hay ningún efecto.  
* `onSuccess` -[opcional] la función a la que se llama una vez que el bloqueo se ha habilitado o deshabilitado correctamente. Debe tener el formato `Function(Boolean currentlockdownstate)` .  
* `onError` -[opcional] la función a la que se llama si se produce un error en la operación de bloqueo. Debe tener el formato `Function(Boolean currentlockdownstate)` .  

**Requisitos**  
Windows 10, versión 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Determina si el contexto de bloqueo aún se aplica al dispositivo. La aplicación Web de prueba invocará esto antes de permitir que los estudiantes inicien las pruebas y periódicamente cuando se encuentren dentro de la prueba.

**Sintaxis**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Parámetros**  
* `callback` : La función a la que se llama cuando se completa esta función. Debe tener el formato, `Function(String state)` donde `state` es una cadena JSON que contiene dos campos. El primero es el `secure` campo, que solo se mostrará `true` si se han habilitado todos los bloqueos necesarios (o características deshabilitadas) para habilitar un entorno de pruebas seguro y ninguno de ellos se ha puesto en peligro, ya que la aplicación entró en modo de bloqueo. El otro campo, `messageKey` , incluye otros detalles o información que es específico del proveedor. El objetivo es permitir a los proveedores incluir información adicional que aumenta la `secure` marca booleana:

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
Obtiene detalles sobre la plataforma en la que se ejecuta la aplicación de prueba. Se utiliza para aumentar cualquier información que se pueda distinguir del agente de usuario.

**Sintaxis**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Parámetros**  
* `callback` : La función a la que se llama cuando se completa esta función. Debe tener el formato, `Function(String infoObj)` donde `infoObj` es una cadena JSON que contiene varios campos. Se deben admitir los campos siguientes:
    * `os` representa el tipo de sistema operativo (por ejemplo: Windows, macOS, Linux, iOS, Android, etc.).
    * `name` representa el nombre de la versión del sistema operativo, si existe (por ejemplo: Sierra, Ubuntu).
    * `version` representa la versión del sistema operativo (por ejemplo: 10,1, 10 Pro, etc.)
    * `brand` representa la personalización de marca del explorador (por ejemplo: OAKS, CA, SmarterApp, etc.).
    * `model` representa el modelo de dispositivo solo para dispositivos móviles; NULL/no se utiliza para los exploradores de escritorio.

**Requisitos**  
Windows 10, versión 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Obtiene la lista de todos los procesos que se ejecutan en el equipo cliente propiedad del usuario. La aplicación de prueba lo invocará para examinar la lista y compararla con una lista de procesos que se han considerado como listas negras durante el ciclo de pruebas. Esta llamada se debe invocar al principio de una evaluación y periódicamente mientras el estudiante está llevando a cabo la evaluación. Si se detecta un proceso en la lista negra, la evaluación debe detenerse para conservar la integridad de las pruebas.

**Sintaxis**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Parámetros**  
* `blacklistedProcessList` -La lista de procesos que la aplicación de prueba tiene en la lista negra.  
`callback` : La función que se va a invocar una vez que se han encontrado los procesos activos. Debe tener el formato: `Function(String foundBlacklistedProcesses)` donde `foundBlacklistedProcesses` tiene el formato: `"['process1.exe','process2.exe','processEtc.exe']"` . Estará vacío si no se encontró ningún proceso en la lista negra. Si es null, indica que se ha producido un error en la llamada de función original.

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
* `restart` : Este parámetro se omite, pero se debe proporcionar.

**Comentarios** En Windows 10, versión 1607, el dispositivo debe estar bloqueado inicialmente. En versiones posteriores, este método cierra el explorador independientemente de si el dispositivo está bloqueado.

**Requisitos**  
Windows 10, versión 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
La aplicación Web de prueba debe invocar esto para determinar si el modo permisivo está activado o desactivado. En el modo permisivo, se espera que un explorador Relájese algunos de sus estrictos enlaces de seguridad para permitir que la tecnología de asistencia funcione con el explorador seguro. Por ejemplo, los exploradores que impidan agresivamente otras interfaces de la aplicación para que no se presenten en ellas podrían querer relajarse en el modo permisivo. 

**Sintaxis**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Parámetros**  
* `callback` : La función que se va a invocar cuando se complete esta llamada. Debe tener el formato: `Function(Boolean permissiveMode)` donde `permissiveMode` indica si el explorador se encuentra actualmente en modo permisivo. Si no está definida o es null, se produjo un error en la operación get.

**Requisitos**  
Windows 10, versión 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
La aplicación Web de prueba debe invocar esto para activar o desactivar el modo permisivo. En el modo permisivo, se espera que un explorador Relájese algunos de sus estrictos enlaces de seguridad para permitir que la tecnología de asistencia funcione con el explorador seguro. Por ejemplo, los exploradores que impidan agresivamente otras interfaces de la aplicación para que no se presenten en ellas podrían querer relajarse en el modo permisivo. 

**Sintaxis**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Parámetros**  
* `enable` : El valor booleano que indica el estado del modo permisivo deseado.  
* `callback` : La función que se va a invocar cuando se complete esta llamada. Debe tener el formato: `Function(Boolean permissiveMode)` donde `permissiveMode` indica si el explorador se encuentra actualmente en modo permisivo. Si no está definida o es null, se produjo un error en la operación SET.

**Requisitos**  
Windows 10, versión 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Borra el Portapapeles del sistema. La aplicación de prueba debe invocar esto para forzar la eliminación de los datos que se puedan almacenar en el Portapapeles del sistema. La función de **[bloqueo](#lockDown)** también realiza esta operación.

**Sintaxis**  
`void SecureBrowser.security.emptyClipBoard();`

**Requisitos**  
Windows 10, versión 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Obtiene la lista de direcciones MAC del dispositivo. La aplicación de prueba debe invocar esto para ayudar en los diagnósticos. 

**Sintaxis**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Parámetros**  
* `callback` : La función que se va a invocar cuando se complete esta llamada. Debe tener el formato: `Function(String addressArray)` donde `addressArray` tiene el formato: `"['00:11:22:33:44:55','etc']"` .

**Comentarios:**  
Es difícil confiar en las direcciones IP de origen para distinguir entre los equipos de los usuarios finales dentro de los servidores de prueba, ya que los firewalls/NAT/proxies se usan habitualmente en las escuelas. Las direcciones MAC permiten a la aplicación distinguir equipos cliente finales detrás de un firewall común con fines de diagnóstico.

**Requisitos**  
Windows 10, versión 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Obtiene la hora a la que se inició la aplicación de prueba.

**Sintaxis**  
`DateTime SecureBrowser.security.getStartTime();`

**Return**  
Objeto DateTime que indica la hora A la que se inició la aplicación de prueba.

**Requisitos**  
Windows 10, versión 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Consulta si una funcionalidad está habilitada o deshabilitada. 

**Sintaxis**  
`Object SecureBrowser.security.getCapability(String feature)`

**Parámetros**  
`feature` : La cadena para determinar la capacidad que se va a consultar. Las cadenas de capacidad válidas son "screenMonitoring", "Printing" y "textSuggestions" (no distingue mayúsculas de minúsculas).

**Valor devuelto**  
Esta función devuelve un objeto o un literal de JavaScript con la forma: `{<feature>:true|false}` . **true** si la capacidad consultada está habilitada, **false** si la funcionalidad no está habilitada o si la cadena de funcionalidad no es válida.

**Requisitos** de Windows 10, versión 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Habilita o deshabilita una funcionalidad específica en el explorador.

**Sintaxis**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Parámetros**  
* `feature` : La cadena para determinar la capacidad que se va a establecer. Las cadenas de capacidad válidas son `"screenMonitoring"` , `"printing"` y `"textSuggestions"` (no distingue mayúsculas de minúsculas).  
* `value` : La configuración prevista para la característica. Debe ser `"true"` o `"false"` .  
* `onSuccess` -[opcional] la función a la que se llamará una vez completada correctamente la operación de establecimiento. Debe tener el formato `Function(String jsonValue)` en el que *jsonValue* tiene el formato: `{<feature>:true|false|undefined}` .  
* `onError` -[opcional] la función a la que se llama si se produce un error en la operación de establecimiento. Debe tener el formato `Function(String jsonValue)` en el que *jsonValue* tiene el formato: `{<feature>:true|false|undefined}` .

**Comentarios:**  
Si la característica de destino es desconocida para el explorador, esta función pasará un valor de `undefined` a la función de devolución de llamada.

**Requisitos** de Windows 10, versión 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Comprueba si la sesión actual ha iniciado sesión de forma remota.

**Sintaxis**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Valor devuelto**  
**true** si la sesión actual es remota; de lo contrario, **false**.

**Requisitos**  
Windows 10, versión 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Comprueba si la sesión actual se está ejecutando en una máquina virtual.

**Sintaxis**  
`Boolean SecureBrowser.security.isVMSession();`

**Valor devuelto**  
**true** si la sesión actual se está ejecutando en una máquina virtual; en caso contrario, **false**.

**Comentarios:**  
Esta comprobación de API solo puede detectar sesiones de máquinas virtuales que se ejecutan en ciertos hipervisores que implementan las API adecuadas.

**Requisitos**  
Windows 10, versión 1709

---