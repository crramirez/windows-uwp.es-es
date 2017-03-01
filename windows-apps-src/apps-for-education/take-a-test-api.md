---
author: TylerMSFT
Description: "La API de JavaScript de la aplicación &quot;Hacer un examen&quot; de Microsoft, te permite proteger los exámenes. Gracias a &quot;Hacer un examen&quot;, tendrás a mano un navegador seguro que evitará que los estudiantes usen otro equipo o recursos de Internet durante un examen."
title: API de JavaScript &quot;Hacer un examen&quot;.
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ac1a9b38a9857ae536025e682f98d01135850a19
ms.lasthandoff: 02/08/2017

---

# <a name="take-a-test-javascript-api"></a>API de JavaScript "Hacer un examen"

[Hacer un examen](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) es una aplicación basada en navegador que te permite realizar exámenes en línea bloqueados para pruebas de gran importancia. Asimismo, admite el estándar API de los navegadores SBAC en pruebas importantes en las que hay mucho en juego y te permite centrarte en el contenido del examen en vez de dedicar tu tiempo a bloquear Windows.

Hacer un examen usa la tecnología del navegador Microsoft Edge y proporciona una API de JavaScript que las aplicaciones web pueden usar para bloquear la administración de otras experiencias mientras se realiza un examen.

La API (basada en la [API fundamental de SBAC](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)) proporciona la funcionalidad texto a voz, así como la capacidad de realizar consultas para saber si el dispositivo está bloqueado, cuáles son los procesos del usuario y del sistema en ejecución y mucho más.

Si quieres obtener información acerca de la propia aplicación, consulta [Take a Test app technical reference (Referencia técnica de la aplicación Hacer un examen)](https://technet.microsoft.com/edu/windows/take-a-test-app-technical).

> [!Important]
> Estas API no funcionan en una sesión remota.  

Para solucionar cualquier problema, consulta [Solucionar problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos](troubleshooting.md).

## <a name="reference-documentation"></a>Documentación de referencia
La API de Hacer un examen consta de los siguientes espacios de nombres. 

| Espacio de nombres | Descripción |
|-----------|-------------|
|[espacio de nombres de seguridad](#security-namespace)|Te permite bloquear el dispositivo.|
|[espacio de nombres TTS](#tts-namespace)|Funcionalidad Texto a voz.|


 ### <a name="security-namespace"></a>Espacio de nombres de seguridad

El espacio de nombres de seguridad te permite bloquear el dispositivo, consultar la lista de procesos de usuario y de sistema, obtener direcciones IP y MAC y borrar los recursos web en caché.

| Método | Descripción   |
|--------|---------------|
|[clearCache](#clearCache) | Borra los recursos web guardados en caché |
|[cerrar](#close) | Cierra el explorador y se desbloquea el dispositivo |
|[enableLockDown](#enableLockDown) | Bloquea el dispositivo. También se usa para desbloquear el dispositivo |
|[getIPAddressList](#getIPAddressList) | Obtiene la lista de direcciones IP del dispositivo |
|[getMACAddress](#getMACAddress)|Obtiene la lista de direcciones MAC del dispositivo|
|[getProcessList](#getProcessList)|Obtiene la lista de procesos del usuario y del sistema en ejecución|
|[isEnvironmentSecure](#isEnvironmentSecure)|Determina si el contexto de bloqueo aún se aplica al dispositivo|  

---
<span id="clearCache"/>
### <a name="void-clearcache"></a>void clearCache()
Borra los recursos web guardados en caché.

**Sintaxis**  
`browser.security.clearCache();`

**Parámetros**  
`None`

**Valor devuelto**  
`None`

**Requisitos**  
Windows 10, versión 1607

---

<span id="close"/>
### <a name="closeboolean-restart"></a>close(boolean restart)
Cierra el explorador y se desbloquea el dispositivo.

**Sintaxis**  
`browser.security.close(false);`

**Parámetros**  
`restart` - Este parámetro se pasa por alto, pero debe proporcionarse.

**Valor devuelto**  
`None`

**Requisitos**  
Windows 10, versión 1607

---

<span id="enableLockDown"/>
### <a name="enablelockdownboolean-lockdown"></a>enableLockdown(boolean lockdown)
Bloquea el dispositivo. También se usa para desbloquear el dispositivo.

**Sintaxis**  
`browser.security.enableLockDown(true|false);`

**Parámetros**  
`lockdown` - `true` para ejecutar la aplicación Hacer un examen pasando por alto la pantalla de bloqueo y aplicar las directivas descritas en este [documento](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). `False` detiene la ejecución de Hacer un examen pasando por alto la pantalla de bloqueo y se cierra a menos que la aplicación no esté bloqueada, en cuyo caso el parámetro no tendrá ningún efecto.

**Valor devuelto**  
`None`

**Requisitos**  
Windows 10, versión 1607

---

<span id="getIPAddressList"/>
### <a name="string-getipaddresslist"></a>string[] getIPAddressList()
Obtiene la lista de direcciones IP del dispositivo.

**Sintaxis**  
`browser.security.getIPAddressList();`

**Parámetros**  
`None`

**Valor devuelto**  
`An array of IP addresses.`

---

<span id="getMACAddress" />
### <a name="string-getmacaddress"></a>string[] getMACAddress()
Obtiene la lista de direcciones MAC del dispositivo.

**Sintaxis**  
`browser.security.getMACAddress();`

**Parámetros**  
`None`

**Valor devuelto**  
`An array of MAC addresses.`

**Requisitos**  
Windows 10, versión 1607

---

<span id="getProcessList" />
### <a name="string-getprocesslist"></a>string[] getProcessList()
Obtiene la lista de los procesos en ejecución del usuario.

**Sintaxis**  
`browser.security.getProcessList();`

**Parámetros**  
`None`

**Valor devuelto**  
`An array of running process names.`

**Observaciones** La lista no incluye los procesos del sistema.

**Requisitos**  
Windows 10, versión 1607

---

<span id="isEnvironmentSecure" />
### <a name="boolean-isenvironmentsecure"></a>boolean isEnvironmentSecure()
Determina si el contexto de bloqueo aún se aplica al dispositivo.

**Sintaxis**  
`browser.security.isEnvironmentSecure();`

**Parámetros**  
`None`

**Valor devuelto**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**Requisitos**  
Windows 10, versión 1607

---

### <a name="tts-namespace"></a>Espacio de nombres TTS

El espacio de nombres TTS controla la funcionalidad de texto a voz de la aplicación.

| Método | Descripción |
|--------|-------------|
|[getStatus](#getStatus) | Obtiene el estado de la reproducción de voz.|
|[getVoices](#getVoices) | Obtiene una lista de paquetes de voz disponibles|
|[pause](#pause)|Pausa la síntesis de voz|
|[resume](#resume)|Reanuda la síntesis de voz en pausa.|
|[speak](#speak)|Síntesis del lado cliente de la opción texto a voz.|
|[stop](#stop)|Detiene la síntesis de voz.|

> [!Tip]
> La [API de síntesis de voz de Microsoft Edge](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) es una implementación de la [API de voz de W3C](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) y recomendamos a los desarrolladores que la usen cuando les sea posible.

---

<span id="getStatus" />
### <a name="string-getstatus"></a>string getStatus()
Obtiene el estado de la reproducción de voz.

**Sintaxis**  
`browser.tts.getStatus();`

**Parámetros**  
`None`

**Valor devuelto**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**Requisitos**  
Windows 10, versión 1607

---

<span id="getVoices" />
### <a name="string-getvoices"></a>string[] getVoices()
Obtiene una lista de paquetes de voz disponibles.

**Sintaxis**  
`browser.tts.getVoices();`

**Parámetros**  
`None`

**Valor devuelto**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**Requisitos**  
Windows 10, versión 1607

---

<span id="pause" />
### <a name="void-pause"></a>void pause()

Pausa la síntesis de voz.

**Sintaxis**  
`browser.tts.pause();`

**Parámetros**

`None`

**Valor devuelto**

`None`

**Requisitos**  
Windows 10, versión 1607

---

<span id="resume" />
### <a name="void-resume"></a>void resume()
Reanuda la síntesis de voz en pausa.

**Sintaxis**  
`browser.tts.resume();`

**Parámetros**
`None`

**Valor devuelto**
`None`

**Requisitos**  
Windows 10, versión 1607

---

<span id="speak" />
### <a name="void-speakstring-text-object-options-function-callback"></a>void speak(texto de cadena, opciones de objeto, devolución de llamada de la función)
Inicia la síntesis del lado cliente de la opción texto a voz.

**Sintaxis**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parámetros**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**Valor devuelto**  
`None`

**Observaciones** Las variables de la opción deben escribirse en minúscula. El género, idioma y los parámetros de voz forman parte de las cadenas.
El volumen, el tono y la velocidad deben estar indicadas en el archivo de lenguaje de marcado de la síntesis de voz (SSML) y no en el objeto de opciones.
El objeto de opciones debe seguir el orden, la nomenclatura y el uso de mayúsculas y minúsculas que se muestra en el ejemplo anterior.

**Requisitos**  
Windows 10, versión 1607

---

<span id="stop" />
### <a name="void-stop"></a>void stop()
Detiene la síntesis de voz.

**Sintaxis**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parámetros**  
`None`

**Valor devuelto**  
`None`

**Requisitos**  
Windows 10, versión 1607

