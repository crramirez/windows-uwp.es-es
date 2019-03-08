---
title: Servicios web
description: Obtenga información sobre cómo crear un servicio Web para la aplicación
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, servicios web
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600020"
---
# <a name="set-up-web-services-in-udc"></a>Configurar los servicios Web en UDC

> [!WARNING]
> Es el siguiente artículo para ID@Xbox y desarrolladores de asociado administradas solo debido a restricciones de configuración del servicio Web. Configuración de servicios Web solo está disponible para los desarrolladores con el permiso de nivel de cuenta de usuarios de confianza concedido. Si no tiene control de la cuenta permisos de nivel de ponerse en contacto con su administrador de cuenta de desarrollo (DAM) para obtener ayuda.

Los publicadores pueden crear servicios Web si desean personalizar la manera en que sus títulos de las aplicaciones interactúan con servicios de Xbox Live. Servicios Web son configuraciones de nivel de publicador y pueden llamarse mediante cualquier título dentro de un espacio aislado de propiedad por el publicador mediante la configuración de inicio de sesión único.

Motivos para definir los servicios Web:

1. Proporcionar un inicio de sesión único a los usuarios de Xbox Live, en orden para el servicio web proporcionar inicio de sesión único a los usuarios de Xbox Live, debe configurarse como un usuario de confianza de Xbox Live. Cuando se configura este modo, los usuarios autenticados a Xbox Live se autenticarán automáticamente a su servicio sin tener que volver a escribir un conjunto diferente de credenciales.
2. Realizar llamadas de servicios de su servicio a los servicios de Xbox Live - si su producto usará uno de los servicios web para realizar llamadas a un servicio de Xbox Live, ya sea directamente o en nombre de usuarios individuales, necesitará un certificado de socio de negocio.

1. ## <a name="create-a-web-service"></a>Crear un servicio Web

1. Vaya a la [panel del centro de partners](https://partner.microsoft.com/dashboard/windows/overview)  
2. Haga clic en el icono con forma de engranaje en la esquina superior derecha de la página para obtener acceso a la **configuración** lista desplegable.
3. En la lista desplegable, seleccione **opciones del desarrollador**.
4. En la barra de navegación izquierdo, expanda la opción **Xbox Live** y seleccione **servicios Web**.

![gif de servicios Web ](../../images/dev-center/web-services/web-services.gif)

5. En la página de servicios Web, haga clic en **nuevo servicio Web**.
6. Escriba el nombre del servicio Web y elija el tipo de acceso según sea necesario.  
    * Acceso de telemetría permite que su servicio recuperar datos de telemetría de juegos para cualquiera de sus juegos.
    * Acceso al canal de la aplicación proporciona al proveedor de medios que posee el servicio de la autoridad para publicar mediante programación los canales de aplicación para su uso en la consola mediante el giro OneGuide.
7. Haga clic en **Guardar**.

En este momento, ha definido el servicio y Xbox Live es consciente de la existencia del servicio. Dependiendo de las razones para crear el servicio web, se le pedirá configurar Parties(Single Sing-On) para usuario autenticado o empresarial asociado Certificates(Service-to-service calls).  

## <a name="configure-relying-party"></a>Configuración de confianza

Un servicio web debe configurarse como un usuario de confianza de Xbox live con el fin de proporcionar la experiencia de inicio de sesión único a los usuarios de Xbox Live: los usuarios autenticados a Xbox Live se autenticarán automáticamente al servicio web sin tener que volver a escribir un conjunto de credenciales diferente. Para facilitar esto, se debe establecer la confianza entre los servicios de Xbox y el servicio web. Un conjunto de notificaciones (por ejemplo, el nombre de jugador, tipo de dispositivo, Id. de título) se usan como parte del usuario de confianza de entidades de configuraciones para aplicar esta relación de confianza. Se trata de la información que se intercambia entre Xbox Live y el servicio web para ayudar a autenticar automáticamente a los usuarios.

### <a name="create-a-relying-party"></a>Crear un usuario de confianza

1. Vaya al panel de centro de partners  
2. Haga clic en el icono con forma de engranaje en la esquina superior derecha de la página para obtener acceso a la **configuración** lista desplegable.
3. En la lista desplegable, seleccione **opciones del desarrollador**.
4. En la barra de navegación izquierdo, expanda la opción **Xbox Live** y seleccione **confianza**.
5. En la página usuarios de confianza, haga clic en **nueva confianza**.
6. Escriba un URI para el usuario de confianza en este formato: *ejemplo.com*.
7. Seleccione el tipo de cifrado que se usará para garantizar la seguridad del servicio de entidad de usuario de confianza.
8. Si ha seleccionado el cifrado simétrico con claves compartidas en el paso anterior, haga clic en **generar nueva clave** para obtener una nueva clave compartida. Siga las instrucciones en pantalla para guardar la clave de forma segura.
9. Escriba el **duración Token** en horas.
10. En **notificaciones**, la lista desplegable ofrece una lista de notificaciones que el servicio de entidad de usuario de confianza puede usar para fines de autenticación. Seleccione todas las notificaciones que se va a usar. Las notificaciones seleccionadas aparecerá debajo de la lista desplegable. Algunas notificaciones estándares se rellenará en el espacio de forma predeterminada.
11. Haga clic en **guardar** cuando haya terminado.  

## <a name="configure-a-business-partner-certificate"></a>Configurar un certificado de socio de negocio

Si su producto usará uno de los servicios web para realizar llamadas a un servicio de Xbox Live, ya sea directamente o en nombre de usuarios individuales, necesitará un certificado de socio de negocio.

### <a name="generate-a-business-partner-certificate"></a>Generar un certificado de socio de negocio

Continúe con los pasos siguientes después de crear correctamente un servicio Web.  

1. En la página de servicios Web, busque el servicio web que desea asociar un certificado de socio de negocio.
2. Seleccione el **generar certificado** vínculo en el servicio web elegido.
3. Haga clic en **Mostrar opciones** junto al generar un nuevo certificado. Esto mostrará los comandos que se deben ejecutar desde PowerShell con privilegios de administrador.
4. Ejecuta los comandos uno después de otro correctamente debe proporcionar un Base64 codificado blob. Esta es la clave pública. Copie la clave pública de PowerShell y péguelo en el marcador de posición para el Blob de CSP.
5. Haga clic en **descargar** y siga las instrucciones en la página para enlazar los certificados.
    1. Utilice el mismo equipo que usó para generar la clave pública.
    2. Ejecute este comando en PowerShell: *mmc.exe*
    3. Seleccione archivo y seleccione Agregar o quitar complemento.
    4. Seleccione certificados y seleccione Agregar. Asegúrese de seleccionar la cuenta de equipo para el complemento de certificado y, a continuación, haga clic en Finalizar y haga clic en Aceptar.
    5. Abra el almacén Personal\Certificate.
    6. Haga clic con el botón derecho en los certificados y seleccione todas las tareas y seleccione Importar.
    7. Seleccione el certificado que descargó desde UDC.
    8. Haga clic en el certificado en la interfaz de usuario después de que se ha importado y seleccione todas las tareas y seleccione Exportar.
    9. Siga al Asistente para exportación y no olvide seleccionar para exportar la clave privada con el certificado.
    10. Finalizar al Asistente para exportación.