---
title: Autorización de EDS
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " Autorización de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607610"
---
# <a name="eds-authorization"></a>Autorización de EDS
 
  * [Introducción](#ID4EN)
  * [Proceso de autorización](#ID4EFB)
  * [3.0 los tokens de: Vs multiusuario. Usuario único](#ID4EEC)
  * [¿EDS admite varios usuarios?](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>Introducción
 
Servicios de detección de entretenimiento (EDS) 3.1 no será compatible con tráfico anónimo. Se requiere autenticación para todas las solicitudes de EDS. EDS requerirá un XToken desde los que llaman a los clientes se autentiquen correctamente. Estos tokens se generan mediante XSTS y pueden obtenerse a través de diversos servicios de autenticación de Xbox (XAS). Existen servicios de autenticación independiente para el dispositivo, los usuarios y los títulos que definirán todas la identidad del token.
 
XSTS es el equipo selector para Xbox LIVE. Es la primera línea de defensa para determinar si un dispositivo o usuario está autorizado para conectarse a cualquiera de los servicios de Xbox LIVE. Tras autenticar al usuario, el XSTS genera un XToken que pueden usar para identificarse de forma segura a cualquier componente en el servicio. Este XToken es su pasaporte vida.
 
Las personas y las cosas, que se va a utilizar nuestros servicios. Y queremos que la mayoría de los objetos y las personas para que pueda usar nuestros servicios. ¿Pero cómo nos aseguramos de que las cosas no son fingiendo ser personas, y que las personas son realmente quienes dicen ser? Proporcionamos con tokens que pueden usar para identificarse a otros usuarios.
 
Estos tokens se generan mediante el XSTS y generalmente se conocen como XTokens. XToken es un término amplio que se usa para abarcar los tokens que contienen una gran variedad de cosas diferentes y pueden proceder de muchas formas diferentes, pero son todos creados, firmados y, opcionalmente, cifrar el servidor XSTS. Internamente, XSTS usa MXAN para crear y dar formato a los tokens. MXAN es el único componente que nunca se extrae información de un XToken. Servicios de consumo de los tokens de pasan los encabezados de solicitud a MXAN descifrado. Los tokens se procesan y se valida y se devuelven las notificaciones que se expresa en el token al servicio. A continuación, el servicio puede utilizar que estos valores para identificar el usuario o dispositivo que realiza la llamada y realizar acciones basadas en esa información de notificación.
 
Los tokens de identidad básica - usuario, dispositivo y título: se proporcionan los servicios de autenticación de Xbox (XAS). Cada XAS es responsable de generar un token de identidad que especifica valores para las distintas notificaciones que son responsables.
 
   * XASD (XAS para dispositivos): crea un DToken que proporciona una identidad de dispositivo
   * XASU (XAS para los usuarios): crea un UToken que proporciona una identidad de usuario
   * XAST (XAS para títulos): crea una; TToken que proporciona una identidad de título
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>Proceso de autorización
 
   * Obtener uno o varios tokens de identidad. Puede solicitar un XToken con a lo sumo cada uno de los tokens de T, U y D. Debe proporcionar al menos uno de D o U. 
     * Solicitar un DToken de XASD proporcionando los detalles de autenticación del dispositivo
     * Solicitar un UToken de XASU con algún tipo de autenticación de usuario. Probablemente se proporciona la autenticación de usuario en forma de un token de MSA (RPS).
     * Solicitar un; TToken de XAST. Los títulos que están disponibles dependen de la plataforma que se está ejecutando actualmente por lo que debe proporcionar un DToken XAST también.
  
   * Creación de una solicitud XSTS.
 
     * Definir el usuario de confianza que está solicitando un token para.
     * Rellenar las propiedades de la solicitud con los tokens de T, U o D.
    * Ejecutar la solicitud XSTS y almacenar en caché el XToken resultante. El XToken devuelto (como máximo) contiene toda la información de identidad de título, el usuario y dispositivo de los tokens de identidad y las notificaciones adicionales (estado actual de la suscripción, grupos de usuarios, etcetera.).
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 los tokens de: Vs multiusuario. Usuario único
 
Este es el formulario del token 3.0: `XBL3.0 x=&lt;hash>;&lt;token>`
 
En función de la &lt;hash >, el token se trata de manera diferente:
 
   * Si &lt;hash > es igual a * (asterisco), a continuación, ningún usuario concreto realiza la solicitud y todos los usuarios en el token están presentes en la entidad de seguridad deserializado. Esta es la forma multiusuario es true.
   * Si &lt;hash > es igual a - (dash), a continuación, ningún usuario está realizando la solicitud. Se eliminan los usuarios en la entidad de seguridad deserializado.
   * Si &lt;hash > no es igual que * o -, a continuación, es un identificador que indica que el usuario en el token está realizando la solicitud. Solo el usuario indicado estará presente en la entidad de seguridad deserializado. Se quitan todos los demás usuarios. Este es el token de usuario único 3.0.
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>¿EDS admite varios usuarios?
 * La respuesta es no. En el caso de que se ha descrito, la consola siempre enviará los tokens de usuario único. Incluso si hay varios usuarios ha iniciado sesión, debe haber un indicado "llamador", donde se quitan todas las identidades de otras.
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[URI de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)

   