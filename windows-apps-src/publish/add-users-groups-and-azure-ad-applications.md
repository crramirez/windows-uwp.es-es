---
author: jnHs
Description: Puedes agregar usuarios, grupos y aplicaciones de AzureAD a tu cuenta del Centro de desarrollo.
title: Agregar usuarios, grupos y aplicaciones de AzureAD a tu cuenta del Centro de desarrollo
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 32f62d6022d075e71ce6bcc15e1603a2e11a68a5
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2017
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-dev-center-account"></a>Agregar usuarios, grupos y aplicaciones de AzureAD a tu cuenta del Centro de desarrollo

La sección [Administrar usuarios](manage-account-users.md) del Centro de desarrollo te permite usar AzureActiveDirectory para agregar usuarios a tu cuenta del Centro de desarrollo. A cada usuario se le asigna un rol (o conjunto de permisos personalizados) que define su acceso a la cuenta. También puedes agregar [grupos de usuarios](#groups) y [aplicaciones de AzureAD](#azure-ad-applications) para concederles acceso a tu cuenta del Centro de desarrollo.

Después de que los usuarios se hayan agregado a la cuenta, puedes [editar detalles de la cuenta](#edit), cambiar [roles y permisos](set-custom-permissions-for-account-users.md) o [eliminar usuarios](#remove).

> [!IMPORTANT]
> Para agregar usuarios a tu cuenta, primero tienes que [asociar tu cuenta del Centro de desarrollo al inquilino de Azure ActiveDirectory de la organización](associate-azure-ad-with-dev-center.md). 

Al agregar usuarios, ten en cuenta los siguientes aspectos. (Se aplican a grupos y aplicaciones de AzureAD, así como a usuarios individuales).

-   Todos los usuarios del Centro de desarrollo deben tener una cuenta activa en el directorio de la organización. La creación de un nuevo usuario en el Centro de desarrollo también creará una cuenta para dicho usuario en el inquilino de AzureAD de la organización.
-   [Los cambios](#edit) de nombre de usuario en el Centro de desarrollo se reflejarán en el inquilino de AzureAD de la organización.
-   Al agregar usuarios, tendrás que especificar su acceso a la cuenta del Centro de desarrollo asignándoles un [rol o conjunto de permisos personalizados](set-custom-permissions-for-account-users.md). 

> [!NOTE]
> Si tu organización usa la [integración de directorios](http://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar el servicio de directorio local con AzureAD, no podrás crear nuevos usuarios, grupos ni aplicaciones de AzureAD en el Centro de desarrollo. Tu (u otro administrador en el directorio local) deberá crearlos directamente en el directorio local para que puedas verlos en el Centro de desarrollo y agregarlos a este.


<span id="users" />
## <a name="add-users-to-your-dev-center-account"></a>Agregar usuarios a tu cuenta del Centro de desarrollo

Puedes agregar usuarios individuales a tu cuenta del Centro de desarrollo de una de las siguientes maneras:
-   Agregar usuarios que ya existan en el directorio de la organización
-   Crear un nuevo usuario para agregarlo al directorio de la organización y a tu cuenta del Centro de desarrollo
-   Agregar usuarios existentes que no se encuentren en el directorio de la organización


<span id="from-directory" />
### <a name="add-users-from-your-organizations-directory"></a>Agregar usuarios del directorio de la organización

1.  En la página **Administrar usuarios**, haz clic en **Agregar usuarios**.
2.  Selecciona uno o varios usuarios de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
    > [!TIP]
    > Si seleccionas más de un usuario para agregar a tu cuenta del Centro de desarrollo, debes asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varios usuarios con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los usuarios seleccionados.
5.  Haz clic en **Guardar**.


<span id="new-user" />
### <a name="create-a-new-user-account-in-your-organizations-directory-and-add-them-to-your-dev-center-account"></a>Crear una nueva cuenta de usuario en el directorio de la organización para agregarla a tu cuenta del Centro de desarrollo

Si quieres conceder acceso al Centro de desarrollo a una nueva cuenta del inquilino de Azure AD, puedes crear una en la sección **Administrar usuarios** haciendo clic en **Nuevo usuario**. 

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de AzureAD para agregar nuevos usuarios al directorio.

Si quieres que el usuario nuevo tenga una [cuenta de administrador global](http://go.microsoft.com/fwlink/p/?LinkId=746654) en el directorio de la organización, activa la casilla con la etiqueta **Make this user a Global administrator in your Azure AD, with full control over all directory resources**. De esta forma, el usuario podrá obtener acceso a todas las características administrativas en el Azure AD de tu empresa. Podrá agregar y administrar usuarios en el directorio de la organización (aunque no en el Centro de desarrollo, a menos que concedas a la cuenta los [roles/permisos](set-custom-permissions-for-account-users.md) adecuados). Si activas esta casilla, deberás proporcionar un **correo electrónico de recuperación de contraseña** para el usuario.

1.  En la página **Administrar usuarios**, haz clic en **Agregar usuarios**.
2.  En la página siguiente, haz clic en **Nuevo usuario**.
3.  Asegúrate de que el botón de radio **Add to Azure AD** está activado para crear una nueva cuenta en el directorio de la organización y agregar dicho usuario a la cuenta del Centro de desarrollo. 
4.  Escribe el nombre, el apellido y el nombre de usuario del nuevo usuario.
5.  Escribe el correo electrónico que el usuario puede usar si necesita recuperar su contraseña. Esto solo es necesario si has seleccionado la casilla **Make this user a Global administrator in your Azure AD**.
6.  En la sección **Pertenencia al grupo**, selecciona cualquier grupo al que quieres que pertenezca el nuevo usuario.
7.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
8.  Haz clic en **Guardar**.
9.  En la página de confirmación, verás la información de inicio de sesión del nuevo usuario, incluida una contraseña temporal. Asegúrate de anotar dicha información y proporciónasela al usuario nuevo, ya que no podrás obtener acceso a la contraseña temporal después de salir de esta página.


<span id="email" />
### <a name="add-a-user-from-outside-of-your-azure-ad-tenant-to-your-dev-center-account-and-your-directory"></a>Agregar un usuario desde fuera del inquilino de Azure AD a la cuenta del Centro de desarrollo y el directorio

Para invitar a los usuarios que actualmente no forman parte del inquilino de AzureAD a tu cuenta, sigue los pasos siguientes. Los usuarios recibirán una invitación por correo electrónico para unirse a su cuenta y se les creará una nueva cuenta de [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en el inquilino de AzureAD. 

1.  En la página **Administrar usuarios**, haz clic en **Agregar usuarios**.
2.  En la página siguiente, haz clic en **Nuevo usuario**.
3.  Selecciona el botón de selección **Invite users by email**.
3.  Escribe una o varias direcciones de correo electrónico (hasta diez), separadas por coma o punto y coma.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
6.  Haz clic en **Guardar**.

Los usuarios que has invitado recibirán un correo electrónico con una invitación para obtener acceso a tu cuenta del Centro de desarrollo. Cada usuario tendrá que aceptar tu invitación para poder obtener acceso a tu cuenta.

Si tienes que reenviar una invitación, busca al usuario en tu página **Administrar usuarios** y selecciona su dirección de correo electrónico (o el texto que dice **Invitación pendiente**) para editar la cuenta. Luego en la parte inferior de la página, haz clic en **Volver a enviar invitación**.


### <a name="changing-a-users-directory-password"></a>Cambiar la contraseña del directorio de un usuario

Si necesitas cambiar la contraseña de una cuenta de usuario que agregaste a tu cuenta del Centro de desarrollo, puedes hacerlo en la sección **Administrar usuarios**. Ten en cuenta que este paso cambiará la contraseña del usuario en el inquilino de AzureAD, junto con la contraseña que utiliza para tener acceso al Centro de desarrollo. 

Si has proporcionado un **correo electrónico de recuperación de contraseña** al crear la cuenta de usuario, el usuario podrá restablecer su propia contraseña. También puedes actualizar la contraseña de un usuario siguiendo estos pasos.

1.  En la página **Administrar usuarios**, haz clic en el nombre de la cuenta de usuario que quieres editar.
2.  Haz clic en el botón **Restablecer la contraseña** en la parte inferior de la página.
3.  Aparecerá una página de confirmación con la información de inicio de sesión del usuario, incluida una contraseña temporal.
    > [!IMPORTANT]
    >  Asegúrate de imprimir o copiar dicha información y proporcionársela al usuario, ya que no podrás acceder a la contraseña temporal después de salir de esta página.

<span id="groups" />
## <a name="add-groups-to-your-dev-center-account"></a>Agregar grupos a tu cuenta del Centro de desarrollo

Puedes agregar un grupo desde el directorio de la organización a tu cuenta del Centro de desarrollo. Al hacerlo, todos los usuarios que pertenezcan a dicho grupo podrán tener acceso a la cuenta, con los permisos asociados al rol asignado al grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Agregar grupos del directorio de la organización

1.  En la página **Administrar usuarios**, haz clic en **Agregar grupos**.
2.  Selecciona uno o varios grupos de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar grupos específicos.
    > [!TIP]
    > Si seleccionas más de un grupo para agregar a tu cuenta del Centro de desarrollo, debes asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varios grupos con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar grupos, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los grupos seleccionados. Todos los miembros del grupo podrán tener acceso a tu cuenta del Centro de desarrollo con los permisos que apliques al grupo, independientemente de los roles y permisos asociados a sus cuentas individuales.
5.  Haz clic en **Guardar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Crear una nueva cuenta de grupo en el directorio de la organización para agregarla a tu cuenta del Centro de desarrollo

Si quieres conceder acceso al Centro de desarrollo a un nuevo grupo, puedes crear uno en la sección **Administrar usuarios**. Ten en cuenta que el nuevo grupo se creará en el directorio de la organización, no solo en tu cuenta del Centro de desarrollo.

1.  En la página **Administrar usuarios**, haz clic en **Agregar grupos**.
2.  En la página siguiente, haz clic en **Nuevo grupo**.
3.  Escribe el nombre para mostrar del nuevo grupo.
4.  Especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el grupo. Todos los miembros del grupo podrán tener acceso a tu cuenta del Centro de desarrollo con los permisos que apliques al grupo, independientemente de los roles y permisos asociados a sus cuentas individuales.
5.  Selecciona los usuarios que deseas asignar al nuevo grupo en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
6.  Cuando hayas terminado de seleccionar los usuarios, haz clic en **Agregar seleccionado** para agregarlos al nuevo grupo.
7.  Haz clic en **Guardar**.


<span id="azure-ad-applications" />
## <a name="add-azure-ad-applications-to-your-dev-center-account"></a>Agregar aplicaciones de Azure AD a tu cuenta del Centro de desarrollo

Puedes permitir que las aplicaciones o servicios que forman parte de AzureAD de tu organización obtengan acceso a tu cuenta del Centro de desarrollo.

### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Agregar aplicaciones de AzureAD desde el directorio de la organización

1.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**.
2.  Selecciona una o varias aplicaciones de Azure AD en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar aplicaciones de AzureAD específicas.
    > [!TIP]
    > Si seleccionas más de una aplicación de AzureAD para agregar a tu cuenta del Centro de desarrollo, debes asignar a dichas aplicaciones el mismo rol o conjunto de permisos personalizados. Para agregar varias aplicaciones de AzureAD con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar aplicaciones de AzureAD, haz clic en **Agregar seleccionada**.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para las aplicaciones de AzureAD seleccionadas.
5.  Haz clic en **Guardar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Crear una nueva cuenta de aplicación de AzureAD en el directorio de la organización para agregarla a tu cuenta del Centro de desarrollo

Si quieres conceder acceso al Centro de desarrollo a una nueva cuenta de aplicación de Azure AD, puedes crear una en la sección **Administrar usuarios**. Ten en cuenta que esto creará una nueva cuenta en el directorio de la organización, no solo en tu cuenta del Centro de desarrollo.

> [!TIP]
> Si usas principalmente esta aplicación de AzureAD para la autenticación del Centro de desarrollo y no necesitas que los usuarios accedan a ella directamente, puedes escribir cualquier dirección válida en **Dirección URL de respuesta** y **URI de identificador de aplicación**, siempre que otra aplicación de AzureAD no use estos valores en el directorio.

1.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**.
2.  En la siguiente página, haz clic en **Nueva aplicación de Azure AD**.
3.  Escribe la **dirección URL de respuesta** para la nueva aplicación de Azure AD. Esta es la dirección URL donde los usuarios pueden iniciar sesión y usar la aplicación de Azure AD (a veces, también conocida como dirección URL de la aplicación o dirección URL de inicio de sesión). La **dirección URL de respuesta** no puede superar los 256 caracteres.
4.  Escribe el **URI de identificador de aplicación** para la nueva aplicación de Azure AD. Se trata de un identificador lógico para la aplicación de Azure AD que se muestra cuando envía una solicitud de inicio de sesión único a Azure AD. Ten en cuenta que el **URI de identificador de aplicación** debe ser único para cada aplicación de Azure AD del directorio y no puede superar los 256 caracteres.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para la aplicación de AzureAD.
6.  Haz clic en **Guardar**.

Después de agregar o crear una aplicación de Azure AD, puedes volver a la sección **Administrar usuarios** y hacer clic en el nombre de la aplicación para revisar la configuración de la aplicación, incluidos los valores de Id. de inquilino, Id. de cliente, URL de respuesta y URI de id. de aplicación.

> [!NOTE]
> Si tienes previsto usar las API de REST que se incluyen con los [servicios de la Tienda Windows](../monetize/using-windows-store-services.md), necesitarás los valores de Id. de inquilino e Id. de cliente que se muestran en esta página para obtener un token de acceso de AzureAD que te permita autenticar las llamadas a los servicios.   

<span id="manage-keys" />
### <a name="manage-keys-for-an-azure-ad-application"></a>Administrar claves para una aplicación de AzureAD

Si la aplicación de AzureAD lee y escribe datos en MicrosoftAzureAD, necesitará una clave. Puedes crear claves para una aplicación de Azure AD modificando su información en el Centro de desarrollo. También puedes quitar las claves que ya no sean necesarias.

1.  Desde la página **Administrar usuarios**, haz clic en el nombre de la aplicación de Azure AD.
    > [!TIP]
    > Al hacer clic en el nombre de la aplicación de Azure AD, verás todas las claves activas de la aplicación de Azure AD, incluida la fecha en la que se creó la clave y en la que expirará. Para quitar una clave que ya no sea necesaria, haz clic en **Quitar**.

2.  Para agregar una nueva clave, haz clic en **Agregar nueva clave**.
3.  Verás una pantalla que muestra los valores **Id. de cliente** y **Clave**.
    > [!IMPORTANT]
    > Asegúrate de imprimir o copiar esta información, ya que no podrás volver a acceder a ella después de salir de esta página.

4.  Si quieres crear más claves, haz clic en **Agregar otra clave**.

<span id="edit" />
## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar un usuario, grupo o aplicación de AzureAD

Después de agregar usuarios, grupos o aplicaciones de AzureAD a la cuenta del Centro de desarrollo, puedes realizar cambios en la información de su cuenta. 

> [!IMPORTANT]
> Los cambios realizados en los roles o permisos solo afectarán al acceso al Centro de desarrollo. Todos los demás cambios (como por ejemplo, cambiar el nombre de un usuario o la pertenencia a un grupo, o la Dirección URL de respuesta y el URI de identificador de aplicación para una aplicación de Azure AD) se reflejarán en el inquilino de AzureAD de la organización, así como en la cuenta del Centro de desarrollo. 

1.  En la página **Administrar usuarios**, haz clic en el nombre de la cuenta del usuario, grupo o aplicación de AzureAD que quieres editar.
2.  Realiza los cambios que desees. A continuación se indican los elementos que puedes editar:
    -   En un **usuario**, puedes editar el nombre, apellido o nombre de usuario. También puedes seleccionar grupos, o anular su selección, en la sección **Pertenencia a grupos** para actualizar su pertenencia a los grupos.
    -   En un **grupo**, puedes editar el nombre del grupo. (Para actualizar la pertenencia al grupo, edita los usuarios que quieras agregar o quitar del grupo y realiza cambios en la sección **Pertenencia a grupos**).
    -   En una **Aplicación de Azure AD**, puedes especificar nuevos valores para las opciones **Dirección URL de respuesta** o **URI de identificador de aplicación**.
    Recuerda que estos cambios se realizarán en el directorio de la organización, así como en la cuenta del Centro de desarrollo.
3.  Para realizar cambios relacionados con el acceso al Centro de desarrollo, selecciona o anula la selección de los roles que quieres aplicar o selecciona **Personalizar los permisos** para realizar los cambios que desees. Estos cambios solo afectan al acceso al Centro de desarrollo y no modificarán los permisos del inquilino de AzureAD de la organización.
3.  Haz clic en **Guardar**.


## <a name="view-history-for-account-users"></a>Ver el historial de usuarios de la cuenta

Como propietario de la cuenta, puedes ver el historial de exploración detallado de los usuarios adicionales que hayas agregado a la cuenta.

En la página **Administrar usuarios**, haz clic en el vínculo que se muestra debajo de **Última actividad** del usuario cuyo historial de exploración quieres revisar. Podrás ver las direcciones URL de todas las páginas que el usuario ha visitado en los últimos 30 días.

<span id="remove" />
## <a name="remove-users-groups-and-azure-ad-applications"></a>Quitar usuarios, grupos y aplicaciones de AzureAD

Para quitar un usuario, un grupo o una aplicación de Azure AD de tu cuenta del Centro de desarrollo, haz clic en el vínculo **Quitar** que aparece junto al nombre en la página **Administrar usuarios**. Después de confirmar que quieres quitarla, ese usuario, grupo o aplicación de Azure AD ya no tendrá acceso a tu cuenta del Centro de desarrollo (a menos que la agregues de nuevo más adelante).

> [!IMPORTANT] 
> La eliminación de un usuario, un grupo o una aplicación de Azure AD significa que ya no tendrá acceso a tu cuenta del Centro de desarrollo. Esto **no** elimina el usuario, grupo o aplicación de AzureAD del directorio de la organización.

 

