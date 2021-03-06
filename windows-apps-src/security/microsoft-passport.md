---
title: Windows Hello
description: En este artículo se describe la nueva tecnología de Windows Hello que se envía como parte del sistema operativo Windows 10 y se describe cómo los desarrolladores pueden implementar esta tecnología para proteger sus aplicaciones Plataforma universal de Windows (UWP) y los servicios back-end. En él se resaltan las funcionalidades específicas de estas tecnologías para ayudar a mitigar las amenazas que surgen del uso de credenciales convencionales y se proporcionan instrucciones sobre el diseño y la implementación de estas tecnologías como parte de una implementación de Windows 10.
ms.assetid: 0B907160-B344-4237-AF82-F9D47BCEE646
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, security
ms.localizationpriority: medium
ms.openlocfilehash: bcb43748baeeb7f68ec246cb0277fbef0944fc47
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750361"
---
# <a name="windows-hello"></a>Windows Hello

En este artículo se describe la nueva tecnología de Windows Hello que se incluye como parte del sistema operativo Windows 10 y se describe cómo los desarrolladores pueden implementar esta tecnología para proteger sus aplicaciones Plataforma universal de Windows (UWP) y los servicios back-end. En él se resaltan las funcionalidades específicas de estas tecnologías para ayudar a mitigar las amenazas que surgen del uso de credenciales convencionales y se proporcionan instrucciones sobre el diseño y la implementación de estas tecnologías como parte de una implementación de Windows 10.

Ten en cuenta que este artículo se centra en el desarrollo de aplicaciones. Para obtener información sobre la arquitectura y los detalles de implementación de Windows Hello, consulte la [Guía de Windows Hello en TechNet](/windows/keep-secure/microsoft-passport-guide).

Para obtener un ejemplo de código completo, vea el [ejemplo de código de Windows Hello en github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MicrosoftPassport).

Para ver un tutorial paso a paso sobre cómo crear una aplicación para UWP con Windows Hello y el servicio de autenticación de respaldo, consulte los artículos [aplicación de inicio de sesión](microsoft-passport-login.md) de Windows Hello y servicio de inicio de sesión de [Windows Hello](microsoft-passport-login-auth-service.md) .

## <a name="1-introduction"></a>1 Introducción

Una suposición fundamental sobre la seguridad de la información es que un sistema puede identificar quién lo está usando. La identificación de un usuario permite al sistema decidir si el usuario se identificó correctamente (un proceso conocido como autenticación) y luego determinar lo que ese usuario autenticado correctamente debe poder hacer (autorización). La gran mayoría de los sistemas informáticos implementados en todo el mundo dependen de las credenciales de usuario para tomar decisiones sobre la autenticación y la autorización, lo que significa que dependen de contraseñas reutilizables creadas por los usuarios para su seguridad. El lema que se cita a menudo en el que la autenticación puede implicar "algo que sabes, algo que tienes o algo que eres" resalta perfectamente el tema: una contraseña reutilizable es un factor de autenticación por sí mismo, por lo que cualquier persona que conozca la contraseña de un usuario puede suplantarle.

### <a name="11-problems-with-traditional-credentials"></a>1.1 Problemas con las credenciales tradicionales

Desde mediados de la década los 60, cuando Fernando Corbató y su equipo en el Massachusetts Institute of Technology abogaron por la introducción de la contraseña, los usuarios y administradores han tenido que lidiar con el uso de contraseñas de autenticación y autorización de usuarios. Con el tiempo, la tecnología del almacenamiento y uso de contraseñas ha avanzado un poco (con hash seguro y la aplicación de sal, por ejemplo), pero todavía nos enfrentamos a dos problemas. Las contraseñas son fáciles de clonar y son fáciles de robar. Además, los errores de implementación pueden hacerlas inseguras y los usuarios tienen dificultades a la hora de encontrar un equilibrio entre comodidad y seguridad.

#### <a name="111-credential-theft"></a>1.1.1 Robo de credenciales

El mayor riesgo de las contraseñas es simple: un atacante las puede robar fácilmente. Cada lugar en que se escribe, procesa o almacena una contraseña es vulnerable. Por ejemplo, un atacante puede robar una colección de contraseñas o hash desde un servidor de autenticación al interceptar el tráfico de red a un servidor de aplicaciones, al implantar malware en una aplicación o un dispositivo, al registrar las pulsaciones de teclas del usuario en un dispositivo o al mirar qué caracteres escribe un usuario. Estos son tan solo los métodos de ataque más comunes.

Otro riesgo relacionado es la reproducción de credenciales, donde un atacante captura una credencial válida al interceptar el tráfico de una red no segura y luego la reproduce más adelante para suplantar a un usuario válido. La mayoría de los protocolos de autenticación (como Kerberos y OAuth) protege contra ataques de reproducción al incluir una marca de tiempo en el proceso de cambio de credenciales. Sin embargo, esta táctica solo protege el token que emite el sistema de autenticación, pero no la contraseña que el usuario proporciona para obtener el ticket en primer lugar.

#### <a name="112-credential-reuse"></a>1.1.2 Reutilización de credenciales

El método común de usar una dirección de correo como nombre de usuario empeora un problema significativo. Un atacante que recupere correctamente un par de nombre de usuario y contraseña de un sistema en riesgo luego puede intentar ese mismo par en otros sistemas. Con una frecuencia sorprendente, esta táctica funciona para permitir a atacantes pasar de un sistema en riesgo a otros sistemas. Además, el uso de direcciones de correo como nombres de usuario conduce a problemas adicionales, que analizaremos más adelante en esta guía.

### <a name="12-solving-credential-problems"></a>1.2 Solucionar los problemas relacionados con las credenciales

Es difícil solucionar los problemas que presentan las contraseñas. Tan solo aumentar las exigencias de las directivas de contraseña no es suficiente, ya que los usuarios pueden simplemente reciclar, compartir o anotar las contraseñas. Aunque la educación de los usuarios es fundamental para la seguridad de autenticación, tampoco elimina el problema por sí sola.

Windows Hello reemplaza las contraseñas con una autenticación sólida de dos factores (2FA) mediante la comprobación de las credenciales existentes y la creación de una credencial específica del dispositivo que protege un gesto de usuario biométrico o basado en PIN. 


## <a name="2-what-is-windows-hello"></a>2 ¿Qué es Windows Hello?

Windows Hello es el nombre que Microsoft dio al nuevo sistema de inicio de sesión biométrico integrado en Windows 10. Dado que está integrado directamente en el sistema operativo, Windows Hello permite la identificación por rostro o huella digital para desbloquear los dispositivos de los usuarios. La autenticación se produce cuando el usuario proporciona su identificador biométrico único para tener acceso a las credenciales específicas del dispositivo, lo que significa que un atacante que robe el dispositivo no podrá iniciar sesión en él a menos que dicho atacante tenga el PIN. El almacén de credenciales seguro de Windows protege los datos biométricos en el dispositivo. Si usas Windows Hello para desbloquear un dispositivo, el usuario autorizado obtiene acceso a todos sus servicios, sitios web, datos, aplicaciones y experiencia de Windows.

El autenticador de Windows Hello se conoce como un saludo. Un saludo es único para la combinación de un dispositivo individual y un usuario específico. No se mueve entre dispositivos, no se comparte con un servidor o la aplicación que llama y no se pueden extraer fácilmente de un dispositivo. Si varios usuarios comparten un dispositivo, cada uno de ellos necesita configurar su propia cuenta. Cada cuenta obtiene un único saludo para ese dispositivo. Se puede considerar un saludo como un token que puedes usar para desbloquear (o liberar) una credencial almacenada. El saludo en sí no autentica el usuario en una aplicación o servicio, sino que libera las credenciales que sí pueden hacerlo. En otras palabras, el saludo no es una credencial de usuario, pero es un segundo factor para el proceso de autenticación.

### <a name="21-windows-hello-authentication"></a>autenticación de Windows Hello 2,1

Windows Hello ofrece una forma eficaz para que un dispositivo reconozca a un usuario individual y eso aborda la primera parte de la ruta entre un usuario y un elemento de datos o servicio solicitado. Después de que el dispositivo haya reconocido al usuario, aún tiene que autenticarlo antes de determinar si va a conceder acceso a un recurso solicitado. Windows Hello proporciona un 2FA seguro que está totalmente integrado en Windows y reemplaza las contraseñas reutilizables con la combinación de un dispositivo específico y un gesto o un PIN biométricos.

No obstante, Windows Hello no es solo un sustituto de los sistemas 2FA tradicionales. Conceptualmente, es similar a una tarjeta inteligente: la autenticación se realiza mediante primitivas criptográficas en lugar de comparaciones de cadenas y el material de clave del usuario se protege en el interior de hardware resistente a manipulaciones. Windows Hello no requiere los componentes de infraestructura adicionales necesarios para la implementación de tarjetas inteligentes, ya sea. En particular, no se necesita una infraestructura de claves públicas (PKI) para administrar los certificados, si no tienes ninguna. Windows Hello combina las principales ventajas de las tarjetas inteligentes (flexibilidad de implementación para tarjetas inteligentes virtuales y seguridad sólida para tarjetas inteligentes físicas) sin ninguna de sus desventajas.

### <a name="22-how-windows-hello-works"></a>2,2 Cómo funciona Windows Hello

Cuando el usuario configura Windows Hello en su equipo, genera un nuevo par de claves pública y privada en el dispositivo. El [módulo de plataforma segura](/windows/keep-secure/trusted-platform-module-overview) (TPM) genera y protege esta clave privada. Si el dispositivo no tiene un chip de TPM, la clave privada se cifra y se protege mediante software. Además, los dispositivos con TPM generan un bloque de datos que se puede usar para certificar que una clave está enlazada al TPM. Esta información de atestación puede usarse en la solución para decidir si se concede al usuario un nivel diferente de autorización, por ejemplo.

Para habilitar Windows Hello en un dispositivo, el usuario debe tener una cuenta de Azure Active Directory o una cuenta de Microsoft conectada en la configuración de Windows.

#### <a name="221-how-keys-are-protected"></a>2.2.1 cómo se protegen las claves

Cada vez que se genere material de clave, este se debe proteger contra los ataques. La forma más sólida de hacer esto es mediante hardware especializado. El uso de módulos de seguridad de hardware (HSM) para generar, almacenar y procesar claves para aplicaciones críticas para la seguridad tiene una larga trayectoria. Las tarjetas inteligentes son un tipo especial de HSM, como lo son también los dispositivos compatibles con el estándar Trusted Computing Group TPM. Siempre que sea posible, la implementación de Windows Hello aprovecha el hardware de TPM incorporado para generar, almacenar y procesar claves. Sin embargo, Windows Hello y Windows Hello for work no requieren un TPM incorporado.

Siempre que sea factible, Microsoft recomienda el uso de hardware de TPM. El TPM protege contra una variedad de ataques conocidos y posibles, incluidos los ataques de fuerza bruta contra los PIN. Además, el TPM proporciona un nivel adicional de protección después de un bloqueo de la cuenta. Si el TPM bloquea el material de clave, el usuario tendrá que restablecer el PIN. Restablecer el PIN significa que se quitarán todas las claves y certificados cifrados con el material de clave anterior.

#### <a name="222-authentication"></a>2.2.2 autenticación

Cuando un usuario quiere acceder a material de clave protegido, el proceso de autenticación: el usuario escribe un PIN o realiza un gesto biométrico para desbloquear el dispositivo, un proceso denominado a veces "liberar la clave".

Una aplicación nunca puede usar las claves de otra aplicación y un usuario nunca puede usar las claves de otro usuario. Estas claves se usan para firmar las solicitudes que se envían al proveedor de identidades o IDP, solicitando acceso a los recursos especificados. Las aplicaciones pueden usar API específicas para solicitar operaciones que requieren material de clave para determinadas acciones. El acceso a través de estas API requiere validación explícita mediante un gesto de usuario y el material de clave no se expone a la aplicación solicitante. En su lugar, la aplicación solicita una acción específica, como firmar un fragmento de datos, y la capa de Windows Hello controla el trabajo real y devuelve los resultados.

### <a name="23-getting-ready-to-implement-windows-hello"></a>2,3 preparar la implementación de Windows Hello

Ahora que tenemos un conocimiento básico de cómo funciona Windows Hello, echemos un vistazo a cómo implementarlos en nuestras propias aplicaciones.

Hay diferentes escenarios que se pueden implementar con Windows Hello. Por ejemplo, iniciar sesión en tu aplicación en un dispositivo. Otro escenario habitual sería autenticarse en un servicio. En lugar de usar un nombre de inicio de sesión y una contraseña, utilizará Windows Hello. En los capítulos siguientes, trataremos la implementación de un par de escenarios diferentes, incluido cómo autenticarse en los servicios con Windows Hello y cómo convertir de un sistema de nombre de usuario o contraseña existente en un sistema de Windows Hello.

## <a name="3-implementing-windows-hello"></a>3 implementación de Windows Hello

En este capítulo, comenzaremos con un escenario de Virgen sin ningún sistema de autenticación existente y explicaremos cómo implementar Windows Hello.

En la sección siguiente se describe cómo migrar desde un sistema existente que usa nombre de usuario y contraseña. Sin embargo, aunque ese capítulo te interese más, es recomendable echar un vistazo a este capítulo para obtener un conocimiento básico sobre el proceso y el código necesario.

### <a name="31-enrolling-new-users"></a>3.1 Inscripción de nuevos usuarios

Comenzamos con un nuevo servicio de marca que usará Windows Hello y un nuevo usuario hipotético que esté listo para registrarse en un dispositivo nuevo.

El primer paso consiste en comprobar que el usuario puede usar Windows Hello. La aplicación comprueba la configuración de usuario y las capacidades de la máquina para asegurarse de que puede crear las claves del identificador de usuario. Si la aplicación determina que el usuario aún no ha habilitado Windows Hello, solicita al usuario que lo configure antes de usar la aplicación.

Para habilitar Windows Hello, el usuario solo tiene que configurar un PIN en la configuración de Windows, a menos que el usuario lo configure durante la configuración rápida (OOBE).

Las siguientes líneas de código muestran una forma sencilla de comprobar si el usuario está configurado para Windows Hello.

```csharp
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
if (!keyCredentialAvailable)
{
    // User didn't set up PIN yet
    return;
}
```

El siguiente paso es pedir al usuario la información necesaria para registrarse en el servicio. Puedes optar por pedir al usuario el nombre, los apellidos y la dirección de correo, así como un nombre de usuario único. Puedes usar la dirección de correo como el identificador único, tú decides.

En este escenario, usamos la dirección de correo como el identificador único del usuario. Una vez que el usuario se registre, considera la posibilidad de enviar un correo electrónico de validación para asegurarte de que la dirección sea válida. Esto te proporciona un mecanismo para restablecer la cuenta, si fuera necesario.

Si el usuario configuró su PIN, la aplicación crea el objeto [**KeyCredential**](/uwp/api/Windows.Security.Credentials.KeyCredential) del usuario. La aplicación también puede adquirir información opcional de atestación de la clave para obtener la prueba criptográfica de que la clave se genera en el TPM. La clave pública generada y, opcionalmente, la atestación, se envían al servidor back-end para registrar el dispositivo que se está usando. Cada par de claves generado en cada uno de los dispositivos será único.

El código para crear la [**KeyCredential**](/uwp/api/Windows.Security.Credentials.KeyCredential) tiene el aspecto siguiente:

```csharp
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

El método [**RequestCreateAsync**](/previous-versions/windows/dn973048(v=win.10)) es la parte que crea las claves pública y privada. Si el dispositivo tiene el chip TPM correcto, las API solicitarán el chip TPM para crear las claves pública y privada, y almacenar el resultado; si no hay ningún chip TPM disponible, el sistema operativo creará el par de claves en el código. No hay ninguna manera de que la aplicación acceda directamente a las claves privadas creadas. Parte de la creación de los pares de claves es también la información de atestación resultante. (Consulta el capítulo siguiente para más información sobre la atestación).

Después de crear la información de par de claves y atestación en el dispositivo del usuario, la clave pública, la información de atestación opcional y el identificador único (como la dirección de correo) deben enviarse al servicio de registro de back-end y almacenarse en el back-end.

Para permitir al usuario acceder a la aplicación en varios dispositivos, el servicio back-end debe ser capaz de almacenar varias claves para el mismo usuario. Dado que cada clave es única para cada dispositivo, almacenaremos todas estas claves asociadas al mismo usuario. Un identificador de dispositivo se usa para ayudar a optimizar la parte del servidor al autenticar los usuarios. Hablaremos sobre esto con más detalle en el siguiente capítulo.

Un esquema de base de datos de muestra para almacenar esta información en el backend podría tener este aspecto:

![Esquema de la base de datos de ejemplo Windows Hello](images/passport-db.png)

La lógica del registro podría tener el siguiente aspecto:

![Lógica de registro de Windows Hello](images/passport-registration.png)

Evidentemente, la información de registro que recopiles puede incluir mucha más información de identificación que lo hace en este escenario simple. Por ejemplo, si tu aplicación accede a un servicio protegido, como por ejemplo, para la banca, tendrías que solicitar comprobación de la identidad y otras cosas como parte del proceso de suscripción. Cuando se cumplan todas las condiciones, la clave pública de este usuario se almacenará en el back-end y se usará para la validación la próxima vez que el usuario utilice el servicio.

```csharp
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Credentials;

static async void RegisterUser(string AccountId)
{
    var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
    if (!keyCredentialAvailable)
    {
        // The user didn't set up a PIN yet
        return;
    }

    var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
    if (keyCreationResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = keyCreationResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttestationIncluded = false;
        bool keyAttestationCanBeRetrievedLater = false;

        keyAttestationResult = await userKey.GetAttestationAsync();
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLater = true;
        }
    }
    else if (keyCreationResult.Status == KeyCredentialStatus.UserCanceled ||
        keyCreationResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        // Show error message to the user to get confirmation that user
        // does not want to enroll.
    }
}
```

#### <a name="311-attestation"></a>3.1.1 Atestación

Al crear el par de claves, también es una opción solicitar la información de atestación que el chip de TPM genera. Esta información opcional puede enviarse al servidor como parte del proceso de suscripción. La atestación de clave de TPM es un protocolo que criptográficamente demuestra que una clave está enlazada a TPM. Este tipo de atestación se puede usar para garantizar que una operación criptográfica determinada se produjo en el TPM de un equipo concreto.

Cuando recibe la clave RSA generada, la declaración de atestación y el certificado AIK, el servidor debe comprobar las siguientes condiciones:

- La firma del certificado AIK es válida.
- El certificado AIK se encadena a una raíz de confianza.
- El certificado AIK y su cadena están habilitados para el OID del EKU "2.23.133.8.3" (el nombre descriptivo es Certificado de la clave de identidad de la atestación).
- El certificado AIK tiene validez temporal.
- Todos los certificados de la CA emisora de la cadena tienen validez temporal y no se han revocado.
- La declaración de atestación está formada correctamente.
- La firma del blob [**KeyAttestation**](/uwp/api/Windows.Security.Cryptography.Certificates.KeyAttestationHelper) usa una clave pública de AIK.
- La clave pública incluida en el blob [**KeyAttestation**](/uwp/api/Windows.Security.Cryptography.Certificates.KeyAttestationHelper) coincide con la clave RSA pública que el cliente envió junto con la declaración de atestación.

La aplicación podría asignar al usuario un nivel de autorización diferente, en función de estas condiciones. Por ejemplo, si se produce un error en una de estas comprobaciones, podría no inscribir al usuario o podría limitar lo que el usuario puede hacer.

### <a name="32-logging-on-with-windows-hello"></a>3,2 Inicio de sesión con Windows Hello

Una vez que el usuario se inscribe en el sistema, puede usar la aplicación. Según el escenario, puedes pedir a los usuarios que se autentiquen para poder empezar a usar la aplicación o simplemente pedirles que se autentiquen cuando empiecen a usar los servicios back-end.

### <a name="33-force-the-user-to-sign-in-again"></a>3.3 Exigir al usuario que vuelva a iniciar sesión

En algunos escenarios, quizás prefieras que el usuario demuestre que es la persona que ha iniciado sesión antes de acceder a la aplicación o a veces antes de hacer una acción determinada dentro de la aplicación. Por ejemplo, antes de que una aplicación de banca envíe el comando de transferencia de dinero al servidor, quieres asegurarte de que es el usuario, en lugar de alguien encontró un dispositivo conectado, que intenta realizar una transacción. Puedes exigir al usuario que inicie sesión nuevamente en la aplicación mediante la clase [**UserConsentVerifier**](/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier). La siguiente línea de código obligará al usuario a escribir sus credenciales.

La siguiente línea de código obligará al usuario a escribir sus credenciales.

```csharp
UserConsentVerificationResult consentResult = await UserConsentVerifier.RequestVerificationAsync("userMessage");
if (consentResult.Equals(UserConsentVerificationResult.Verified))
{
    // continue
}
```

Por supuesto, también puedes usar el mecanismo de respuesta de desafío del servidor, lo que exige al usuario especificar su código PIN o sus credenciales biométricas. Depende del escenario que, como desarrollador, necesites implementar. Este método se describe en la siguiente sección.

### <a name="34-authentication-at-the-backend"></a>3.4 Autenticación en el backend

Cuando la aplicación intente acceder a un servicio back-end protegido, el servicio envía un desafío a la aplicación. La aplicación usa la clave privada del usuario para firmar el desafío y lo vuelve a enviar al servidor. Dado que el servidor almacenó la clave pública de ese usuario, usa las API criptográficas estándar para asegurarse de que el mensaje se firmó con la clave privada correcta. En el cliente, la firma se realiza mediante las API de Windows Hello; el desarrollador nunca tendrá acceso a la clave privada de ningún usuario.

Además de comprobar las claves, el servicio también puede comprobar la atestación de clave y decidir si hay limitaciones invocadas sobre el almacenamiento de las claves en el dispositivo. Por ejemplo, si el dispositivo usa TPM para proteger las claves, la seguridad es mayor que cuando los dispositivos almacenan las claves sin TPM. La lógica de back-end podría decidir, por ejemplo, que el usuario solo pueda transferir cierta cantidad de dinero cuando no se usa ningún TPM para reducir los riesgos.

La atestación solo está disponible para los dispositivos que tienen un chip TPM versión 2.0 o posterior. Por tanto, debes tener en cuenta que esta información podría no estar disponible en todos los dispositivos.

El flujo de trabajo de cliente podría parecerse al del siguiente gráfico:

![Flujo de trabajo del cliente de Windows Hello](images/passport-client-workflow.png)

Cuando la aplicación llama al servicio en el backend, el servidor envía un desafío. El desafío se firma con el siguiente código:

```csharp
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

if (openKeyResult.Status == KeyCredentialStatus.Success)
{
    var userKey = openKeyResult.Credential;
    var publicKey = userKey.RetrievePublicKey();
    var signResult = await userKey.RequestSignAsync(message);

    if (signResult.Status == KeyCredentialStatus.Success)
    {
        return signResult.Result;
    }
    else if (signResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {

    }
}
```

La primera línea, [**KeyCredentialManager.OpenAsync**](/uwp/api/windows.security.credentials.keycredentialmanager.openasync), pedirá al sistema operativo que abra el identificador de clave. Si se lleva a cabo correctamente, puedes firmar el mensaje de desafío con el método [**KeyCredential.RequestSignAsync**](/uwp/api/windows.security.credentials.keycredential.requestsignasync), que hará que el sistema operativo solicite el PIN o las credenciales biométricas del usuario a través de Windows Hello. El desarrollador no tendrá en ningún momento acceso a la clave privada del usuario. Todo se protege a través de las API.

Las API solicitan al sistema operativo que firme el desafío con la clave privada. El sistema luego pide al usuario un código PIN o un inicio de sesión biométrico configurado. Cuando se especifique la información correcta, el sistema puede pedir al chip TPM que realice las funciones criptográficas y firme el desafío. (O bien, puede usar la solución de software de reserva, si no hay ningún TPM disponible). El cliente debe enviar el desafío firmado de vuelta al servidor.

Un flujo básico de desafío y respuesta se muestra en este diagrama de secuencia:

![Respuesta de desafío de Windows Hello](images/passport-challenge-response.png)

A continuación, el servidor debe validar la firma. Cuando se solicita la clave pública y se envía al servidor para usarla en futuras validaciones, se encuentra en un BLOB publicKeyInfo con codificación ASN. 1. Si observa el [ejemplo de código de Windows Hello en github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MicrosoftPassport), verá que hay clases auxiliares para ajustar las funciones de crypt32 para traducir el BLOB con codificación ASN. 1 a un BLOB de CNG, que se usa con más frecuencia. El blob contiene el algoritmo de clave pública, que es la RSA y la clave pública RSA.

En el ejemplo, el motivo por el que se convierte el BLOB con codificación ASN. 1 en un BLOB de CNG es para que se pueda usar con CNG (/windows/desktop/SecCNG/cng-portal) y la API BCrypt. Si busca el BLOB de CNG, le dirigirá a la [estructura de BCRYPT_KEY_BLOB](/windows/desktop/api/bcrypt/ns-bcrypt-_bcrypt_key_blob)relacionada. Esta superficie de API se puede usar para la autenticación y el cifrado en aplicaciones Windows. ASN. 1 es un estándar documentado para la comunicación de estructuras de datos que se pueden serializar y que se suele usar en criptografía de clave pública y con certificados. Este es el motivo por el que se devuelve la información de la clave pública de esta manera. La clave pública es una clave RSA; y ese es el algoritmo que usa Windows Hello cuando firma los datos.

Cuando tengas el blob CNG, debes validar el desafío firmado con la clave pública del usuario. Puesto que todo el mundo usa su propio sistema o tecnología back-end, no existe una forma genérica de implementar esta lógica. Usamos SHA256 como el algoritmo hash y Pkcs1 para el objeto SignaturePadding, por lo que debes asegurarte de que es lo que usas al validar la respuesta firmada del cliente. De nuevo, consulte la muestra para ver cómo hacerlo en su servidor en .NET 4.6, pero en general tendrá un aspecto similar al siguiente:

```csharp
using (RSACng pubKey = new RSACng(publicKey))
{
    retval = pubKey.VerifyData(originalChallenge, responseSignature, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
}
```

Leemos la clave pública almacenada, que es una clave RSA. Validamos el mensaje de desafío firmado con la clave pública y, si se constata, autorizamos al usuario. Si el usuario se autentica, la aplicación puede llamar a los servicios back-end del modo normal.

El código completo puede tener un aspecto similar al siguiente:

```csharp
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Security.Credentials;

static async Task<IBuffer> GetAuthenticationMessageAsync(IBuffer message, String AccountId)
{
    var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

    if (openKeyResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = openKeyResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var signResult = await userKey.RequestSignAsync(message);
        if (signResult.Status == KeyCredentialStatus.Success)
        {
            return signResult.Result;
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // Launch app-specific flow to handle the scenario 
            return null;
        }
    }
    else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
    {
        // PIN reset has occurred somewhere else and key is lost.
        // Repeat key registration
        return null;
    }
    else
    {
        // Show custom UI because unknown error has happened.
        return null;
    }
}
```

La implementación del mecanismo correcto de desafío y respuesta está fuera del ámbito de este documento, pero es algo que requiere mucha atención para crear correctamente un mecanismo seguro que impida acciones como, por ejemplo, ataques de reproducción o los ataques de tipo "Man-in-the-middle".

### <a name="35-enrolling-another-device"></a>3.5 Inscribir otro dispositivo

Hoy en día, es común que los usuarios tengan varios dispositivos con las mismas aplicaciones instaladas. ¿Cómo funciona esto cuando se usa Windows Hello con varios dispositivos?

Al usar Windows Hello, cada dispositivo creará un conjunto de claves privada y pública único. Esto significa que si quieres que un usuario pueda usar varios dispositivos, el back-end debe ser capaz de almacenar varias claves públicas de este usuario. Consulta el diagrama de base de datos de la sección 2.1 para ver un ejemplo de la estructura de la tabla.

El registro de otro dispositivo es prácticamente igual que el registro de un usuario por primera vez. Aún tienes que asegurarte de que el usuario que se registra en el nuevo dispositivo es realmente el usuario que dice ser. Puedes hacerlo con cualquier mecanismo de autenticación en dos fases que se use actualmente. Existen varias maneras de lograr esto de forma segura. Todo depende del escenario.

Por ejemplo, si aún se usan el nombre de inicio de sesión y la contraseña, puedes usarlos para autenticar al usuario y solicitar el uso en uno de sus métodos de comprobación, como SMS o correo electrónico. Si no tienes un nombre de inicio de sesión y una contraseña, también puedes usar uno de los dispositivos ya registrados y enviar una notificación a la aplicación en ese dispositivo. La aplicación autenticadora de MSA es un ejemplo de esto. En resumen, debes usar un mecanismo 2FA común para registrar los dispositivos adicionales para el usuario.

El código para registrar el nuevo dispositivo es exactamente el mismo que para registrar al usuario por primera vez (desde de la aplicación).

```csharp
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

Para que sea más fácil para el usuario reconocer los dispositivos que están registrados, puedes optar por enviar el nombre del dispositivo o cualquier otro identificador como parte del registro. Esto también es útil, por ejemplo, si deseas implementar un servicio en el backend donde los usuarios pueden anular el registro de dispositivos cuando se pierde un dispositivo.

### <a name="36-using-multiple-accounts-in-your-app"></a>3.6 Usar varias cuentas en tu aplicación

Además de admitir varios dispositivos en una sola cuenta, también es común admitir varias cuentas en una sola aplicación. Por ejemplo, quizás te conectas a varias cuentas de Twitter desde dentro de la aplicación. Con Windows Hello, puede crear varios pares de claves y admitir varias cuentas dentro de la aplicación.

Una forma de hacerlo guardando en un almacenamiento aislado el nombre de usuario o el identificador único que se describió en el capítulo anterior. Por lo tanto, cada vez que crees una nueva cuenta, almacenas el identificador de cuenta en el almacenamiento aislado.

En la interfaz de usuario de la aplicación, permites al usuario elegir una de las cuentas creadas anteriormente o registrarse para obtener una nueva. El flujo de creación de una nueva cuenta es el mismo que describimos antes. La elección de una cuenta es cuestión de enumerar las cuentas almacenadas en la pantalla. Una vez que el usuario selecciona una cuenta, usa el identificador de cuenta para iniciar sesión en tu aplicación:

```csharp
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);
```

El resto del flujo es el mismo que hemos descrito antes. Para ser más precisos, todas estas cuentas se protegen con el mismo código PIN o gesto biométrico, ya que en este escenario se usan en un único dispositivo con la misma cuenta de Windows.

## <a name="4-migrating-an-existing-system-to-windows-hello"></a>4 migración de un sistema existente a Windows Hello

En esta sección breve, trataremos una aplicación existente para la Plataforma universal de Windows y un sistema back-end que usan una base de datos que almacena el nombre de usuario y contraseña con hash. Estas aplicaciones recopilan las credenciales del usuario cuando se inician y usan estos datos cuando el sistema back-end devuelve el desafío de autenticación.

Aquí describiremos qué partes deben cambiarse o reemplazarse para que Windows Hello funcione.

Ya hemos descrito la mayoría de las técnicas en los capítulos anteriores. Agregar Windows Hello al sistema existente implica agregar un par de flujos diferentes en la parte de registro y autenticación del código.

Un método es permitir al usuario elegir cuándo realizar una actualización. Después de que el usuario inicie sesión en la aplicación y detecte que la aplicación y el sistema operativo son capaces de admitir Windows Hello, puede preguntar al usuario si desea actualizar las credenciales para usar este sistema moderno y más seguro. Puede usar el código siguiente para comprobar si el usuario es capaz de usar Windows Hello.

```csharp
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
```

La interfaz de usuario podría tener el siguiente aspecto:

![Interfaz de usuario de Windows Hello](images/passport-ui.png)

Si el usuario decide empezar a usar Windows Hello, cree el [**KeyCredential**](/uwp/api/Windows.Security.Credentials.KeyCredential) descrito antes. El servidor de registro en el backend agrega la clave pública y la declaración de atestación opcional a la base de datos. Dado que el usuario ya se autenticó con el nombre de usuario y la contraseña, el servidor puede vincular las nuevas credenciales a la información de usuario actual en la base de datos. El modelo de base de datos podría ser el mismo que en el que se usa en el ejemplo descrito anteriormente.

Si la aplicación pudo crear el objeto [**KeyCredential**](/uwp/api/Windows.Security.Credentials.KeyCredential) de los usuarios, almacena el identificador de usuario en el almacenamiento aislado para que el usuario pueda seleccionar esta cuenta en la lista cuando la aplicación se inicie de nuevo. A partir de este punto, el flujo sigue exactamente los ejemplos que se describen en los capítulos anteriores.

El último paso en la migración a un escenario completo de Windows Hello es deshabilitar la opción nombre de inicio de sesión y contraseña en la aplicación y quitar las contraseñas con hash almacenadas de la base de datos.

## <a name="5-summary"></a>5 Resumen

Windows 10 presenta un mayor nivel de seguridad que también es muy sencillo de poner en práctica. Windows Hello proporciona un nuevo sistema de inicio de sesión biométrico que reconoce al usuario e invalida activamente los esfuerzos para sortear la identificación correcta. Después, puede proporcionar varias capas de claves y certificados que nunca se pueden mostrar o usar fuera del módulo de plataforma segura. Además, existe una capa de seguridad adicional disponible a través del uso opcional de claves y certificados de identidad de atestación.

Como desarrollador, puedes usar esta guía de diseño e implementación de estas tecnologías con el fin de agregar fácilmente una autenticación segura a tus implementaciones de Windows 10 para proteger las aplicaciones y los servicios back-end. El código necesario es mínimo y fácil de entender. El trabajo pesado lo hace Windows 10.

Las opciones de implementación flexibles permiten a Windows Hello reemplazar o trabajar junto con el sistema de autenticación existente. La experiencia de implementación es fácil y económica. No se requiere ninguna infraestructura adicional para implementar la seguridad de Windows 10. Con Microsoft Hello integrado en el sistema operativo, Windows 10 ofrece la solución más segura a los problemas de autenticación dirigidos al desarrollador moderno.

¡Misión cumplida! Acabas de hacer que Internet sea un lugar seguro.

## <a name="6-resources"></a>6 Recursos

### <a name="61-articles-and-sample-code"></a>6.1 Artículos y código de ejemplo

- [Introducción a Windows Hello](https://support.microsoft.com/help/17215)
- [Detalles de implementación de Windows Hello](/windows/keep-secure/microsoft-passport-guide)
- [Ejemplo de código de Windows Hello en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MicrosoftPassport)

### <a name="62-terminology"></a>6.2 Terminología

| Término | Definición |
| ---- | ---------- |
| AIK | Una clave de Identidad de atestación se usa para proporcionar una prueba criptográfica (atestación de clave de TPM) como esta mediante la firma de las propiedades de la clave que no se puede migrar, y el suministro de las propiedades y la firma al usuario de confianza para su verificación. La firma resultante se denomina "declaración de atestación". Dado que la firma se crea mediante la clave privada AIK, que solo puede usarse en el TPM que la creó, el usuario de confianza puede confiar en que la clave atestada no se puede migrar realmente y no se puede usar fuera de ese TPM.AIK. |
| Certificado AIK | Un certificado AIK se usa para atestar la presencia de un AIK dentro de un TPM. También se usa para atestar que otras claves certificadas por el AIK provienen de ese TPM concreto. |
| IDP | Un IDP es un proveedor de identidades. Un ejemplo es la compilación de IDP de Microsoft para cuentas de Microsoft. Cada vez que una aplicación necesita autenticarse con una MSA, puede llamar al IDP de MSA. |
| PKI | La infraestructura de claves públicas se usa con frecuencia para señalar un entorno hospedado por una organización y responsable de crear claves, revocar claves, etc. |
| TPM | El Módulo de plataforma segura (TPM) se puede usar para crear pares de claves criptográficas públicas y privadas, de manera que la clave privada nunca se pueda revelar o usar fuera del TPM (es decir, las clave no se pueden migrar). |
| Atestación de clave de TPM | Un protocolo que criptográficamente demuestra que una clave está enlazada a TPM. Este tipo de atestación se puede usar para garantizar que una operación criptográfica determinada se produjo en el TPM de un equipo concreto. |

## <a name="related-topics"></a>Temas relacionados

* [Aplicación de inicio de sesión de Windows Hello](microsoft-passport-login.md)
* [Servicio de inicio de sesión de Windows Hello](microsoft-passport-login-auth-service.md)
