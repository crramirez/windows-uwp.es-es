---
Description: Puede agregar usuarios, grupos y aplicaciones de Azure AD a su cuenta de centro de partners.
title: Agregar usuarios, grupos y aplicaciones de Azure AD a su cuenta de centro de partners
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, aplicación de azure ad, aad, usuario, grupo, varios usuarios, varios usuarios
ms.localizationpriority: medium
ms.openlocfilehash: 326bb547ac5b0d31f5112d7d5737ddad0d592dd5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610130"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Agregar usuarios, grupos y aplicaciones de Azure AD a su cuenta de centro de partners

El **usuarios** sección de [centro de partners](https://partner.microsoft.com/dashboard) (bajo **configuración de la cuenta**) le permite usar Azure Active Directory para agregar usuarios a la cuenta del centro de partners. A cada usuario se le asigna un rol (o conjunto de permisos personalizados) que define su acceso a la cuenta. También puede agregar [grupos de usuarios](#groups) y [aplicaciones de Azure AD](#azure-ad-applications) para concederles acceso a la cuenta del centro de partners.

Después de que los usuarios se hayan agregado a la cuenta, puedes [editar detalles de la cuenta](#edit), cambiar [roles y permisos](set-custom-permissions-for-account-users.md) o [eliminar usuarios](#remove).

> [!IMPORTANT]
> Para agregar usuarios a su cuenta, primero debe [asociar su cuenta de centro de partners de inquilino de Azure Active Directory de su organización](associate-azure-ad-with-partner-center.md). 

Al agregar los usuarios, deberá especificar su acceso a la cuenta del centro de partners mediante la asignación de un [rol o un conjunto de permisos personalizados](set-custom-permissions-for-account-users.md). 

Tenga en cuenta que todos los usuarios de centro de partners (incluidos los grupos y aplicaciones de Azure AD) deben tener una cuenta activa en [un inquilino de Azure AD que está asociado con la cuenta del centro de partners](associate-azure-ad-with-partner-center.md). La administración de usuarios se realiza en un inquilino cada vez; debes iniciar sesión con una cuenta de administrador para el inquilino en el que quieres agregar o editar usuarios. Crear un nuevo usuario en el centro de partners, también se creará una cuenta para ese usuario en el inquilino de Azure AD a la que ha iniciado sesión y realizar cambios en un nombre de usuario en el centro de partners hará que los mismos cambios en el inquilino de Azure AD de su organización.

> [!NOTE]
> Si su organización usa [integración de directorios](https://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar el servicio de directorio locales con Azure AD, no podrá crear nuevos usuarios, grupos o aplicaciones de Azure AD en el centro de partners. Usted (u otro administrador en el directorio local) deberá crearlas directamente en el directorio local antes de, podrá ver y agregarlos en el centro de partners.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Agregar usuarios a la cuenta del centro de partners

Para agregar usuarios a la cuenta del centro de partners, vaya a la **usuarios** página **configuración de la cuenta** y seleccione **agregar usuarios.** Debes haber iniciado sesión con una cuenta de administrador para el inquilino de Azure AD en el que quieres trabajar. 

### <a name="add-existing-users"></a>Agregar usuarios existentes 

Puede seleccionar los usuarios que ya existen en el inquilino de su organización y otorga acceso a la cuenta del centro de partners. 

<span id="from-directory" />

1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de partners) y, a continuación, seleccione **opciones del desarrollador**. En el **configuración** menú, seleccione **usuarios**.
2.  En la página **Usuarios**, selecciona **Agregar usuarios**. 
3.  Selecciona uno o varios usuarios de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
    > [!TIP]
    > Si selecciona más de un usuario para agregar a la cuenta del centro de partners, debe asignar el mismo rol o conjunto de permisos personalizados. Para agregar varios usuarios con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.
4.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los usuarios seleccionados.
6.  Haga clic en **Guardar**.

### <a name="additional-methods-for-adding-users"></a>Métodos adicionales para agregar usuarios

Si ha iniciado sesión con una cuenta de administrador que también tiene [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) permisos para el inquilino de Azure AD que está trabajando, tendrá opciones adicionales para agregar usuarios a la cuenta del centro de partners. Deberás seleccionar una de las opciones siguientes:

-   **Agregue usuarios existentes**: Elija los usuarios que ya existen en el directorio de su organización y concederles acceso a la cuenta del centro de partners, utilizando el método descrito anteriormente.
-   **Crear nuevos usuarios**: Cree cuentas de usuario completamente nuevo para agregar a los directorios de su organización y su cuenta de centro de partners
-   **Invitar a usuarios externos**: Enviar invitaciones por correo electrónico a los usuarios que no están actualmente en el directorio de su organización. Se les invitará a acceder a su cuenta de centro de partners y un nuevo [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) cuenta se crearán para ellos en su inquilino de Azure AD.

<span id="new-user" />

### <a name="create-new-users"></a>Crear nuevos usuarios

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de Azure AD para crear nuevos usuarios.

1.  Desde el **usuarios** página (en **configuración de la cuenta**), seleccione **agregar usuarios**, a continuación, elija **crear nuevos usuarios**.
2.  Escribe el nombre, el apellido y el nombre de usuario del nuevo usuario.
3.  Si quieres que el usuario nuevo tenga una [cuenta de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) en el directorio de la organización, activa la casilla con la etiqueta **Make this user a Global administrator in your Azure AD, with full control over all directory resources**. De esta forma, el usuario podrá obtener acceso a todas las características administrativas en el Azure AD de tu empresa. Podrá agregar y administrar usuarios en el directorio de su organización (aunque asociado no está en Center, a menos que se conceda a la cuenta adecuado [permisos derol/](set-custom-permissions-for-account-users.md)). Si activas esta casilla, deberás proporcionar un **correo electrónico de recuperación de contraseña** para el usuario.
4.  Si has activado la casilla **Make this user a Global administrator in your Azure AD**, escribe un correo electrónico que el usuario pueda usar si necesita recuperar su contraseña.
5.  En la sección **Pertenencia al grupo**, selecciona cualquier grupo al que quieres que pertenezca el nuevo usuario.
6.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
7.  Haga clic en **Guardar**.
8.  En la página de confirmación, verás la información de inicio de sesión del nuevo usuario, incluida una contraseña temporal. Asegúrate de anotar dicha información y proporciónasela al usuario nuevo, ya que no podrás obtener acceso a la contraseña temporal después de salir de esta página.


<span id="email" />

### <a name="invite-outside-users"></a>Invitar a usuarios externos

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de Azure AD para invitar usuarios externos.

1.  Desde el **usuarios** página (en **configuración de la cuenta**), seleccione **agregar usuarios**, a continuación, elija **invitar a usuarios por correo electrónico**.
1.  Escribe una o varias direcciones de correo electrónico (hasta diez), separadas por coma o punto y coma.
2.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
3.  Haga clic en **Guardar**.

Los usuarios que has invitado recibirán una invitación por correo electrónico para unirse a tu cuenta y se les creará una nueva cuenta de [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en el inquilino de Azure AD. Cada usuario tendrá que aceptar tu invitación para poder obtener acceso a tu cuenta.

Si tienes que reenviar una invitación, busca al usuario en tu página **Usuarios** y selecciona su dirección de correo electrónico (o el texto que dice **Invitación pendiente**). A continuación, en la parte inferior de la página, haz clic en **Volver a enviar invitación**.

> [!IMPORTANT]
> Fuera de los usuarios que invite a unirse a la cuenta del centro de partners se puede asignar los mismos roles y permisos como otros usuarios. Sin embargo, los usuarios externos no podrán realizar determinadas tareas en Visual Studio, como asociar una aplicación a Store o crear paquetes para cargar a Store. Si un usuario necesita realizar esas tareas, elige **Crear nuevos usuarios** en lugar de **Invitar a usuarios externos**. (Si no quieres agregar estos usuarios a tu inquilino de Azure AD existente, puedes [crear un nuevo inquilino](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) y, a continuación, crear nuevas cuentas de usuario para ellos en ese inquilino). 


### <a name="changing-a-users-directory-password"></a>Cambiar la contraseña del directorio de un usuario

Si uno de los usuarios necesita cambiar su contraseña, puede hacerlo él mismo si has proporcionado un **correo electrónico de recuperación de contraseña** al crear la cuenta de usuario. También puedes actualizar la contraseña de un usuario siguiendo los pasos siguientes (si has iniciado sesión con una cuenta de administrador global en tu inquilino de Azure AD para cambiar la contraseña de un usuario). Tenga en cuenta que esta operación cambiará la contraseña del usuario en su inquilino de Azure AD, junto con la contraseña usen para tener acceso a centro de partners. 

1.  Desde el **usuarios** página (en **configuración de la cuenta**), seleccione el nombre de la cuenta de usuario que desea editar.
2.  Seleccione el **restablecer contraseña** situado en la parte inferior de la página.
3.  Aparecerá una página de confirmación con la información de inicio de sesión del usuario, incluida una contraseña temporal.

    > [!IMPORTANT]
    >  No olvide imprimir o copiar esta información y proporcionársela al usuario, ya no podrá tener acceso a la contraseña temporal después de salir de esta página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Agregar grupos a la cuenta del centro de partners

Puede agregar un grupo del directorio de su organización a la cuenta del centro de partners. Al hacerlo, todos los usuarios que pertenezcan a dicho grupo podrán tener acceso a la cuenta, con los permisos asociados al rol asignado al grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Agregar grupos del directorio de la organización

1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de partners) y, a continuación, seleccione **opciones del desarrollador**. En el **configuración** menú, seleccione **usuarios**.
2. Desde el **usuarios** página, seleccione **agregar grupos**.
2.  Selecciona uno o varios grupos de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar grupos específicos.
    > [!TIP]
    > Si selecciona más de un grupo para agregar a la cuenta del centro de partners, debe asignar el mismo rol o conjunto de permisos personalizados. Para agregar varios grupos con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar grupos, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los grupos seleccionados. Todos los miembros del grupo podrá acceder a su cuenta de centro de partners con los permisos que se aplica al grupo, independientemente de los roles y permisos asociados con su cuenta individual.
5.  Haga clic en **Guardar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Crear una nueva cuenta de grupo en el directorio de su organización y agréguelo a su cuenta de centro de partners

Si desea conceder acceso de centro de partners a un grupo nuevo, puede crear un nuevo grupo en el **usuarios** sección. Tenga en cuenta que se creará el nuevo grupo en el directorio de su organización, no solo en la cuenta de centro de partners.

1.  Desde el **usuarios** página (en **opciones del desarrollador**), haga clic en **agregar grupos**.
2.  En la siguiente página, seleccione **nuevo grupo**.
3.  Escribe el nombre para mostrar del nuevo grupo.
4.  Especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el grupo. Todos los miembros del grupo podrá acceder a su cuenta de centro de partners con los permisos que se aplica al grupo, independientemente de los roles y permisos asociados con su cuenta individual.
5.  Selecciona los usuarios que deseas asignar al nuevo grupo en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
6.  Cuando hayas terminado de seleccionar los usuarios, haz clic en **Agregar seleccionado** para agregarlos al nuevo grupo.
7.  Haga clic en **Guardar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Agregar aplicaciones de Azure AD a su cuenta de centro de partners

Puede permitir que las aplicaciones o servicios que forman parte de Azure su organización AD para acceder a su cuenta de centro de partners. Estas cuentas de usuario de la aplicación de Azure AD pueden usarse para llamar a las API de REST proporcionadas por los [servicios de Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Agregar aplicaciones de Azure AD desde el directorio de la organización

1.  1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de partners) y, a continuación, seleccione **opciones del desarrollador**. En el **configuración** menú, seleccione **usuarios**.
2. En la página **Usuarios**, selecciona **Agregar aplicaciones de Azure AD**.
3.  Selecciona una o varias aplicaciones de Azure AD en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar aplicaciones de Azure AD específicas.
    > [!TIP]
    > Si selecciona más de una aplicación de Azure AD para agregar a la cuenta del centro de partners, debe asignar el mismo rol o conjunto de permisos personalizados. Para agregar varias aplicaciones de Azure AD con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

4.  Cuando hayas terminado de seleccionar aplicaciones de Azure AD, haz clic en **Agregar seleccionado**.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para las aplicaciones de Azure AD seleccionadas.
6.  Haga clic en **Guardar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Cree una nueva aplicación de Azure AD de la cuenta en el directorio de su organización y su incorporación a su cuenta de centro de partners

Si desea conceder acceso de centro de partners a una cuenta de aplicación de Azure AD nueva marca, puede crear uno en el **usuarios** sección. Tenga en cuenta que esto creará una nueva cuenta de directorio de su organización, no solo en la cuenta del centro de partners.

> [!TIP]
> Si se utiliza principalmente esta aplicación de Azure AD para la autenticación del centro de partners y no es necesario a los usuarios para acceder a él directamente, puede escribir cualquier dirección válida para el **dirección URL de respuesta** y **App ID URI**, siempre no se utilizan como los valores por cualquier otra aplicación de Azure AD en el directorio.

1.  Desde el **usuarios** página (en **configuración de la cuenta**), seleccione **agregar aplicaciones de Azure AD**.
2.  En la siguiente página, seleccione **New Azure AD application**.
3.  Escribe la **dirección URL de respuesta** para la nueva aplicación de Azure AD. Esta es la dirección URL donde los usuarios pueden iniciar sesión y usar la aplicación de Azure AD (a veces, también conocida como dirección URL de la aplicación o dirección URL de inicio de sesión). La **dirección URL de respuesta** no puede superar los 256 caracteres y debe ser única en tu directorio.
4.  Escribe el **URI de identificador de aplicación** para la nueva aplicación de Azure AD. Se trata de un identificador lógico para la aplicación de Azure AD que se muestra cuando envía una solicitud de inicio de sesión único a Azure AD. Ten en cuenta que el **URI de identificador de aplicación** debe ser único para cada aplicación de Azure AD del directorio y no puede superar los 256 caracteres. Para obtener más información sobre el **URI de id. de aplicación**, consulta [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para la aplicación de Azure AD.
6.  Haga clic en **Guardar**.

Después de agregar o crear una aplicación de Azure AD, puedes volver a la sección **Usuarios** y seleccionar el nombre de la aplicación para revisar la configuración de la aplicación, incluidos los valores de Id. de inquilino, Id. de cliente, URL de respuesta y URI de id. de aplicación.

> [!NOTE]
> Si tienes previsto usar las API de REST que se incluyen con los [servicios de la Microsoft Store](../monetize/using-windows-store-services.md), necesitarás los valores de Id. de inquilino e Id. de cliente que se muestran en esta página para obtener un token de acceso de Azure AD que te permita autenticar las llamadas a los servicios.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Administrar claves para una aplicación de Azure AD

Si la aplicación de Azure AD lee y escribe datos en Microsoft Azure AD, necesitará una clave. Puede crear claves para una aplicación de Azure AD mediante la edición de su información en el centro de partners. También puedes quitar las claves que ya no sean necesarias.

1.  Desde el **usuarios** página (en **configuración de la cuenta**), seleccione el nombre de la aplicación de Azure AD.
    > [!TIP]
    > Al hacer clic en el nombre de la aplicación de Azure AD, verá todas las claves activas de la aplicación de Azure AD, incluida la fecha en que se creó la clave y fecha de expiración. Para quitar una clave que ya no sea necesaria, haz clic en **Quitar**.

2.  Para agregar una nueva clave, seleccione **agregar nueva clave**.
3.  Verás una pantalla que muestra los valores **Id. de cliente** y **Clave**.
    > [!IMPORTANT]
    > Asegúrese de imprimir ni copiar esta información, ya no podrá acceder a ella nuevamente después de salir de esta página.

4.  Seleccione si desea crear más claves, **agregar otra clave**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar un usuario, grupo o aplicación de Azure AD

Después de agregar los usuarios, grupos o aplicaciones de Azure AD a su cuenta de centro de partners, puede realizar cambios en su información de cuenta. 

> [!IMPORTANT]
> Los cambios realizados en [roles o permisos](set-custom-permissions-for-account-users.md) afectará solo al acceso de centro de partners. Inquilino su organización de Azure AD, así como en la cuenta del centro de partners se reflejarán todos los demás cambios (como cambiar un nombre de usuario o pertenencia a grupos, o la dirección URL de respuesta y URI de Id. de aplicación para una aplicación de Azure AD). 

1.  Desde el **usuarios** página (en **configuración de la cuenta**), seleccione el nombre de usuario, grupo o cuenta de aplicación de Azure AD que desea editar.
2.  Realiza los cambios que desees. A continuación se indican los elementos que puedes editar:
    -   En un **usuario**, puedes editar el nombre, apellido o nombre de usuario. También puedes seleccionar grupos, o anular su selección, en la sección **Pertenencia a grupos** para actualizar su pertenencia a los grupos.
    -   En un **grupo**, puedes editar el nombre del grupo. (Para actualizar la pertenencia al grupo, edita los usuarios que quieras agregar o quitar del grupo y realiza cambios en la sección **Pertenencia a grupos**).
    -   En una **Aplicación de Azure AD**, puedes especificar nuevos valores para las opciones **Dirección URL de respuesta** o **URI de identificador de aplicación**.
    Recuerde que estos cambios se realizarán en el directorio de su organización, así como en la cuenta del centro de partners.
3.  Para los cambios relacionados con acceso de centro de partners, seleccione o anule la selección de los roles que desee aplicar, o seleccione **personalizar permisos** y realice los cambios deseados. Estos cambios solo afectan al centro de partners obtener acceso y no cambiará los permisos en el inquilino de Azure AD de su organización.
3.  Haga clic en **Guardar**.


## <a name="view-history-for-account-users"></a>Ver el historial de usuarios de la cuenta

Como propietario de la cuenta, puedes ver el historial de exploración detallado de los usuarios adicionales que hayas agregado a la cuenta.

En el **usuarios** página (en **configuración de la cuenta**), seleccione el vínculo que aparece bajo **última actividad** para el usuario cuyo historial de exploración desea revisar. Podrás ver las direcciones URL de todas las páginas que el usuario ha visitado en los últimos 30 días.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Quitar usuarios, grupos y aplicaciones de Azure AD

Para quitar un usuario, grupo o aplicación de Azure AD de su cuenta de centro de partners, seleccione el **quitar** vínculo que aparece por su nombre en el **usuarios** página. Después de confirmar que desea quitarlo, ese usuario, grupo o aplicación de Azure AD ya no podrá acceder a su cuenta de centro de partners (a menos que agrega de nuevo más tarde).

> [!IMPORTANT]
> Quitar un usuario, grupo o aplicación de Azure AD significa que ya no tendrá acceso a la cuenta del centro de partners. Esto **no** elimina el usuario, grupo o aplicación de Azure AD del directorio de la organización.

 

