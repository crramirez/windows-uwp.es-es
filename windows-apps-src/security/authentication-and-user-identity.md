---
title: "Autenticación e identidad de usuario"
description: "Las aplicaciones para la Plataforma universal de Windows (UWP) presentan varias opciones de autenticación de usuario, que van desde el inicio de sesión único (SSO) simple mediante el agente de autenticación web hasta la autenticación en dos fases de alta seguridad."
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 3c890ac8d8363982d9f014c36b6cba59bee39f20
ms.openlocfilehash: 5ed154b3d31e8741972edc8c8a0fd343f41d4167

---

# Autenticación e identidad de usuario


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Las aplicaciones para la Plataforma universal de Windows (UWP) presentan varias opciones de autenticación de usuario, que van desde el inicio de sesión único (SSO) simple mediante el [agente de autenticación web](web-authentication-broker.md) hasta la autenticación en dos fases de alta seguridad.

Para conexiones de aplicaciones normales a servicios de proveedor de identidad de terceros, como Facebook, Twitter, Flickr, etc., usa el [agente de autenticación web](web-authentication-broker.md). Para mayor comodidad, usa la [caja de seguridad de credenciales](credential-locker.md) para guardar la información de inicio de sesión del usuario y usar un perfil móvil de dicha información.

Recomendamos encarecidamente que empresas que usan Windows 10 consideren el uso de [Microsoft Passport y Windows Hello](microsoft-passport.md), lo que permitirá una autenticación en dos fases de alta seguridad. Si no es posible usar Microsoft Passport, las [tarjetas inteligentes](smart-cards.md) y la [biometría de huellas digitales](fingerprint-biometrics.md) pueden agregar una capa de seguridad adicional.

<table>
<tr><th>Tema</th><th>Descripción</th></tr>
<tr><td>[Caja de seguridad de credenciales](credential-locker.md)</td><td>En este artículo se describe cómo las aplicaciones pueden usar la caja de seguridad de credenciales para almacenar y recuperar credenciales de usuario de forma segura, y cómo transferirlas entre dispositivos con la cuenta de Microsoft del usuario.</td></tr>

<tr><td>[Biometría de huellas digitales](fingerprint-biometrics.md) </td><td>En este artículo se explica cómo agregar biometría de huellas digitales a la aplicación. La inclusión de una solicitud de autenticación con huella digital cuando el usuario deba dar su consentimiento a una acción concreta aumenta la seguridad de la aplicación. Por ejemplo, puedes solicitar la autenticación con huella digital antes de autorizar una compra desde la aplicación o de permitir el acceso a recursos restringidos. La autenticación con huella digital se administra mediante la clase [UserConsentVerifier](https://msdn.microsoft.com/library/windows/apps/dn279134) en el espacio de nombres [Windows.Security.Credentials.UI](https://msdn.microsoft.com/library/windows/apps/hh701356).</td></tr>
<tr><td>[Microsoft Passport y Windows Hello](microsoft-passport.md)</td><td>En este artículo se describe la nueva tecnología Microsoft Passport de Windows 10 y se explica cómo pueden implementarla los desarrolladores para proteger sus servicios backend y aplicaciones. En él se resaltan las funcionalidades específicas de estas tecnologías para ayudar a mitigar las amenazas de credenciales convencionales y se proporcionan instrucciones sobre cómo diseñar e implementar estas tecnologías como parte de la implementación de Windows10. </td></tr>
<tr><td>[Crear una aplicación de inicio de sesión de Microsoft Passport](microsoft-passport-login.md)</td><td>Primera parte de un tutorial completo acerca de cómo crear una aplicación para UWP (Plataforma universal de Windows) de Windows 10 que use Microsoft Passport como una alternativa a los sistemas tradicionales de autenticación de nombre de usuario y contraseña.</td></tr>
<tr><td>[Crear un servicio de inicio de sesión de Microsoft Passport](microsoft-passport-login-auth-service.md)</td><td>Parte 2 de un tutorial completo acerca de cómo usar Microsoft Passport como una alternativa a los sistemas tradicionales de autenticación de nombre de usuario y contraseña en aplicaciones para UWP (Plataforma universal de Windows) de Windows 10.</td></tr>
<tr><td>[Tarjetas inteligentes](smart-cards.md)</td><td>En este tema se explica la manera en que las aplicaciones pueden usar tarjetas inteligentes para conectar a los usuarios a servicios de red seguros, incluido cómo obtener acceso a los lectores de tarjetas inteligentes físicas, crear tarjetas inteligentes virtuales, comunicarse con tarjetas inteligentes, autenticar usuarios, restablecer códigos PIN de usuario y quitar o desconectar tarjetas inteligentes.</td></tr>
<tr><td>[Compartir certificados entre aplicaciones](share-certificates.md)</td><td>Las aplicaciones para UWP que requieren una autenticación segura que vaya más allá de una combinación de identificador de usuario y contraseña pueden usar certificados para la autenticación. La autenticación de certificado ofrece un alto grado de confianza al autenticar a un usuario. En algunos casos, un grupo de servicios querrá autenticar un usuario en varias aplicaciones. En este artículo se muestra cómo puedes autenticar varias aplicaciones usando el mismo certificado, y cómo facilitar un práctico código para que un usuario importe un certificado ofrecido para obtener acceso a servicios web protegidos.</td></tr>
<tr><td>[Desbloqueo de Windows con dispositivos IoT complementarios](companion-device-unlock.md)</td><td>Un dispositivo complementario es un dispositivo que puede actuar junto con tu equipo de escritorio Windows 10 para mejorar la experiencia de autenticación de usuario. Con el marco de dispositivo complementario, un dispositivo complementario puede proporcionar una experiencia enriquecida para Microsoft Passport, incluso cuando Windows Hello no está disponible (por ejemplo, si el equipo con Windows 10 no tiene una cámara para la autenticación facial o un dispositivo lector de huellas digitales).</td></tr>
<tr><td>[Administrador de cuentas web](web-account-manager.md)</td><td>En este artículo se describe cómo mostrar AccountsSettingsPane y conectar la aplicación para la Plataforma universal de Windows (UWP) a proveedores de identidad externos, como Microsoft o Facebook, con las nuevas API de Administrador de cuentas web de Windows 10. Aprenderás a solicitar permiso al usuario para usar su cuenta de Microsoft, obtener un token de acceso y usarlo para realizar operaciones básicas (como obtener datos de perfil o cargar archivos en OneDrive). </td></tr>
<tr><td>[Agente de autenticación web](web-authentication-broker.md)</td><td>En este artículo se explica cómo conectar tu aplicación a un proveedor de identidad en línea que usa protocolos de autenticación como OpenID u OAuth, por ejemplo, Facebook, Twitter, Flickr, Instagram, etc. El método [AuthenticateAsync](https://msdn.microsoft.com/library/windows/apps/br212066) envía una solicitud al proveedor de identidad en línea y recibe un token de acceso que describe los recursos del proveedor a los que tiene acceso la aplicación.</td></tr>
</table>




<!--HONumber=Aug16_HO3-->


