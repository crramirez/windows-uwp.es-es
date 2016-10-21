---
author: TylerMSFT
Description: "La API de JavaScript de la aplicación &quot;Hacer un examen&quot; de Microsoft, te permite proteger los exámenes. Gracias a &quot;Hacer un examen&quot;, tendrás a mano un navegador seguro que evitará que los estudiantes usen otro equipo o Internet durante un examen."
title: API de JavaScript &quot;Hacer un examen&quot; de Microsoft.
translationtype: Human Translation
ms.sourcegitcommit: f2838d95da66eda32d9cea725a33fc4084d32359
ms.openlocfilehash: d7f185e83e81583fd6d7920e5412f76f3a97edd0

---

# API de JavaScript "Hacer un examen" de Microsoft

**Hacer un examen** es una aplicación basada en el navegador, que te permite realizar exámenes en línea bloqueados para pruebas de alto riesgo. Asimismo, admite el estándar API de los navegadores SBAC en pruebas fundamentales de alto riesgo y te permite centrarte en el contenido del examen en vez de dedicar tu tiempo a bloquear Windows.

**Hacer un examen** usa la tecnología del navegador Microsoft Edge y proporciona una API de JavaScript que las aplicaciones web pueden usar para bloquear la administración de otras experiencias mientras se realiza un examen.

La API (basada en la [API fundamental de SBAC](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)) proporciona la función texto a voz, así como la capacidad de realizar consultas para saber si el dispositivo está bloqueado, cuáles son los procesos del usuario y del sistema en ejecución y mucho más.

Si quieres obtener información acerca de la propia aplicación, consulta [Take a Test app technical reference (Referencia técnica de la aplicación Hacer un examen)](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396).

**Importante**

Las API no funcionan en una sesión remota.  
La aplicación Hacer un examen no admite solicitudes HTTP en nuevas ventanas.

Para solucionar cualquier problema, consulta [Solucionar problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos](troubleshooting.md).

**La API de Hacer un examen consta de los siguientes espacios de nombres:**  

| Espacio de nombres | Descripción |
|-----------|-------------|
|[espacio de nombres de seguridad](#security-namespace)| Funcionalidad Texto a voz|
|[espacio de nombres TTS](#tts-namespace)|Te permite bloquear el dispositivo|


 ## espacio de nombres de seguridad

Te permite bloquear el dispositivo, consultar la lista de procesos de usuario y de sistema, obtener direcciones IP y MAC y borrar los recursos web en caché.

| Método | Descripción   |
|--------|---------------|
|[clearCache](#clearCache) | Borra los recursos web guardados en caché |
|[cerrar](#close) | Cierra el explorador y se desbloquea el dispositivo |
|[enableLockDown](#enableLockDown) | Bloquea el dispositivo. También se usa para desbloquear el dispositivo |
|[getIPAddressList](#getIPAddressList) | Obtiene la lista de direcciones IP del dispositivo |
|[getMACAddress](#getMACAddress)|Obtiene la lista de direcciones MAC del dispositivo|
|[getProcessList](#getProcessList)|Obtiene la lista de procesos del usuario y del sistema en ejecución|
|[isEnvironmentSecure](#isEnvironmentSecure)|Determina si el contexto de bloqueo aún se aplica al dispositivo|

<span id="clearCache" />
### void clearCache()
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
### close(boolean restart)
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
### enableLockdown(boolean lockdown)
Bloquea el dispositivo. También se usa para desbloquear el dispositivo.

**Sintaxis**  
`browser.security.enableLockDown(true|false);`

**Parámetros**  
`lockdown` - `true` para ejecutar la aplicación Hacer un examen pasando por alto la pantalla de bloqueo y aplicar las directivas descritas en este [documento](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). `False` detiene la ejecución de Hacer un examen pasando por alto la pantalla de bloqueo y se cierra a menos que la aplicación no esté bloqueada, en cuyo caso el parámetro no tendrá ningún efecto.

**Valor devuelto**  
`None`

**Requisitos**  
Windows 10, versión 1607

---

<span id="getIPAddressList"/>
### string[] getIPAddressList()
Obtiene la lista de direcciones IP del dispositivo.

**Sintaxis**  
`browser.security.getIPAddressList();`

**Parámetros**  
`None`

**Valor devuelto**  
`An array of IP addresses.`

<span id="getMACAddress" />
### string[] getMACAddress()
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
### string[] getProcessList()
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
### boolean isEnvironmentSecure()
Determina si el contexto de bloqueo aún se aplica al dispositivo.

**Sintaxis**  
`browser.security.isEnvironmentSecure();`

**Parámetros**  
`None`

**Valor devuelto**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**Requisitos**  
Windows 10 versión 1607

---

## espacio de nombres TTS
| Método | Descripción |
|--------|-------------|
|[getStatus](#getStatus) | Obtiene el estado de la reproducción de voz|
|[getVoices](#getVoices) | Obtiene una lista de paquetes de voz disponibles|
|[pause](#pause)|Pausa la síntesis de voz|
|[resume](#resume)|Reanuda la síntesis de voz en pausa|
|[speak](#speak)|Síntesis del lado cliente de la opción texto a voz|
|[stop](#stop)|Detiene la síntesis de voz|

> [!Tip]
> La [API de síntesis de voz de Microsoft Edge](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) es una implementación de la [API de voz de W3C](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) y recomendamos que los desarrolladores usen esa API cuando les sea posible.

<span id="getStatus" />
### string getStatus()
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
### string[] getVoices()
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
### void pause()

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
### void resume()
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
### void speak(texto de cadena, opciones de objeto, devolución de llamada de la función)
Síntesis del lado cliente de la opción texto a voz.

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
### void stop()
Detiene la síntesis de voz.

**Sintaxis**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parámetros**  
`None`

**Valor devuelto**  
`None`

**Requisitos**  
Windows 10, versión 1607



<!--HONumber=Aug16_HO3-->


