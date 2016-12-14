---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "Usa la API de envío de la tienda Windows para crear y administrar mediante programación los envíos para las aplicaciones que estén registradas en tu cuenta del Centro de desarrollo de Windows mediante programación."
title: "Creación y administración de envíos mediante el uso de servicios de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: f52059a37194b78db2f9bb29a5e8959b2df435b4
ms.openlocfilehash: 1172be1072f0c539828a08655236be467c6c9fba

---

# <a name="create-and-manage-submissions-using-windows-store-services"></a>Creación y administración de envíos mediante el uso de servicios de la Tienda Windows


Usa la *API de envío de la Tienda de Windows* para consultar mediante programación y crear envíos de aplicaciones, complementos (también conocidos como productos desde la aplicación o IAP) y los paquetes piloto para tu cuenta del Centro de desarrollo de Windows o de tu organización. Esta API es útil si tu cuenta administra muchas aplicaciones o complementos, y quieres automatizar y optimizar el proceso de envío para estos activos. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin del uso de la API de envío de la Tienda Windows:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
3.  Antes de llamar a un método en la API de envío de la tienda Windows, [consigue un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Tienda Windows antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.
4.  [Llamada a API de envío de la Tienda Windows](#call-the-windows-store-submission-api).


<span id="not_supported" />
>**Importante**

> * Esta API puede usarse solo para las cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API. Este permiso se habilita para cuentas de desarrollador en fases, y no todas las cuentas tienen este permiso habilitado en este momento. Para solicitar acceso anterior, inicia sesión en el panel del Centro de desarrollo, haz clic en **Comentarios** en la parte inferior del panel, selecciona **API de envío** para el área de comentarios y envía la solicitud. Recibirás un correo electrónico cuando se habilita este permiso para tu cuenta.
<br/><br/>
> * Esta API no puede usarse con aplicaciones o complementos que utilizan determinadas funciones que se introdujeron en el panel del Centro de desarrollo en agosto de 2016, incluidos (entre otros) actualizaciones obligatorias de aplicaciones y complementos consumibles administrados por la Tienda. Si usas la API de envío de la Tienda Windows con una aplicación o complemento que usa una de estas funciones, la API devolverá un código de error 409. En este caso, debes usar el panel para administrar los envíos para la aplicación o el complemento.


<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-submission-api"></a>Paso 1: Completar requisitos previos para usar la API de envío de la Tienda Windows

Antes de empezar a escribir código para llamar a la API de envío de Tienda Windows, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sin ningún coste adicional.

* Debes [asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo de Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account) y obtener tu identificador de inquilino, identificador de cliente y la clave. Necesitas estos valores para obtener un token de acceso de Azure AD que usarás en llamadas a la API de envío de Tienda Windows.

* Preparación de la aplicación para su uso con la API de envío de la Tienda Windows:

  * Si tu aplicación todavía no existe en el Centro de desarrollo, [crea tu aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). No puedes usar la API de envío de la Tienda Windows para crear una aplicación en el Centro de desarrollo; debes crear tu aplicación con el panel y, a continuación, puedes usar la API para acceder a la aplicación y crear envíos para este mediante programación. Sin embargo, puedes usar la API para crear complementos y paquetes piloto mediante programación antes de crear envíos para estos.

  * Antes de crear un envío para una aplicación determinada mediante esta API, primero debes [crear un envío de la aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), incluidas las respuestas a las preguntas de la [clasificación por edades](https://msdn.microsoft.com/windows/uwp/publish/age-ratings). Después de realizar este paso, podrás crear mediante programación nuevos envíos para esta aplicación con la API. No es necesario crear un envío de complementos o de paquetes piloto antes de usar la API para esos tipos de envíos.

  * Si creas o actualizas un envío de aplicaciones y necesitas incluir un paquete de aplicaciones, [prepara el paquete de aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vas a crear o actualizar el envío de una aplicación y necesitas incluir capturas de pantalla o imágenes para la descripción de Tienda, [prepara las imágenes y capturas de pantalla de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vas a crear o actualizar un envío de complementos y debes incluir un icono, [prepara el icono](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### <a name="how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account"></a>Asociación de una aplicación de Azure AD a tu cuenta del Centro de desarrollo de Windows

Antes de poder usar la API de envío de la Tienda Windows, debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y de cliente para la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de envío de la Tienda Windows. Necesitas el identificador de inquilino, de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

>**Nota**&nbsp;&nbsp;Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, de cliente y la clave, puedes volver a usarlos cuando necesites crear un nuevo token de acceso de Azure AD.

1.  En el Centro de desarrollo, ve a **Configuración de la cuenta**, haz clic en **Administrar usuarios** y asocia la cuenta del Centro de desarrollo de tu organización al directorio de Azure AD de tu organización. Para obtener instrucciones detalladas, consulta [Administración de usuarios de la cuenta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  En la página **Administración de usuarios**, haz clic en **Agregar aplicaciones de Azure AD**, agrega la aplicación de Azure AD que representa la aplicación o el servicio que usarás para acceder a los envíos de tu cuenta del Centro de desarrollo y asígnale el rol **Administrador**. Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en la página **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Adición de aplicaciones de Azure AD**. Para obtener más información, consulta [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Vuelve a la página **Administración de usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta la información sobre la administración de claves en [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de envío de la Tienda Windows, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Autorización** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para los parámetros *tenant\_id*, *client\_id* y *client\_secret*, especifica el identificador de inquilino, de cliente y la clave para la aplicación que recuperó del Centro de desarrollo en la sección anterior. Para el parámetro *Recurso*, debes especificar la URI ```https://manage.devcenter.microsoft.com```.

Después de que el token de acceso expire, puedes actualizarlo siguiendo las instrucciones [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-submission-api">
## <a name="step-3-use-the-windows-store-submission-api"></a>Paso 3: Uso de la API de envío de la Tienda Windows

Una vez que tengas un token de acceso de Azure AD, podrás llamar a métodos en la API de envío de la Tienda Windows. La API incluye muchos métodos que se agrupan en escenarios de aplicaciones, complementos y paquetes piloto. Para crear o actualizar los envíos, normalmente llamas a varios métodos en la API de envío de la Tienda Windows en un orden específico. Para obtener información sobre cada escenario y la sintaxis de cada método, consulta los artículos en la siguiente tabla.

>**Nota**&nbsp;&nbsp;Después de obtener un token de acceso, tienes 60 minutos para acceder a métodos en la API de envío de Tienda Windows antes de que el token expire.

| Situación       | Descripción                                                                 |
|---------------|----------------------------------------------------------------------|
| Aplicaciones |  Recupera los datos de todas las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows y crea envíos para aplicaciones. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Obtención de datos de la aplicación](get-app-data.md)</li><li>[Administración de envíos de aplicaciones](manage-app-submissions.md)</li></ul> |
| Complementos | Obtén, crea, elimina complementos para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los complementos. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administración de complementos](manage-add-ons.md)</li><li>[Administración de envíos de complementos](manage-add-on-submissions.md)</li></ul> |
| Paquetes piloto | Obtén, crea, elimina paquetes piloto para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los paquetes piloto. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administración de paquetes piloto](manage-flights.md)</li><li>[Administración de envíos de paquetes piloto](manage-flight-submissions.md)</li></ul> |

<span />

## <a name="code-examples"></a>Ejemplos de código

Los siguientes artículos proporcionan ejemplos de código detallados que muestran cómo usar la API de envío de la Tienda Windows en varios lenguajes diferentes:

* [Ejemplos de código de C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="troubleshooting"></a>Solución de problemas

| Problema      | Resolución                                          |
|---------------|---------------------------------------------|
| Después de llamar a la API de envío de la Tienda Windows de PowerShell, los datos de respuesta para la API está dañados si se convierte en formato JSON en un objeto de PowerShell mediante la cmdlet [ConvertFrom Json](https://technet.microsoft.com/en-us/library/hh849898.aspx) y, a continuación, volver al formato JSON mediante la cmdlet [ConvertTo Json](https://technet.microsoft.com/en-us/library/hh849922.aspx). |  De manera predeterminada, el parámetro de *-Profundidad* para la cmdlet [ConvertTo Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) se establece en 2 niveles de objetos, lo cual es demasiado superficial para la mayoría de los objetos JSON devueltos por la API de envío de la Tienda Windows. Cuando llames a la cmdlet [ConvertTo Json](https://technet.microsoft.com/en-us/library/hh849922.aspx), establece el parámetro de *-Profundidad* en un número mayor, como 20. |

## <a name="additional-help"></a>Ayuda adicional

Si tienes preguntas sobre la API de envío de la Tienda Windows o necesitas ayuda para administrar tus envíos con esta API, usa los siguientes recursos:

* Pregunta en nuestros [foros](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpsubmit).
* Visita nuestra [página de soporte técnico](https://developer.microsoft.com/windows/support) y solicita una de las opciones de soporte técnico asistido para el panel del Centro de desarrollo. Si se le pide que elija un tipo y categoría de problema, elige **Envío y certificación de aplicaciones** y **Enviar una aplicación**, respectivamente.  

## <a name="related-topics"></a>Temas relacionados

* [Obtención de datos de la aplicación](get-app-data.md)
* [Administración de envíos de aplicaciones](manage-app-submissions.md)
* [Administración de complementos](manage-add-ons.md)
* [Administración de envíos de complementos](manage-add-on-submissions.md)
* [Administración de paquetes piloto](manage-flights.md)
* [Administración de envíos de paquetes piloto](manage-flight-submissions.md)
 



<!--HONumber=Dec16_HO1-->


