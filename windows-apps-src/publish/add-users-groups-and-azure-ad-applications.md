---
Description: You can add users, groups, and Azure AD applications to your Partner Center account.
title: Agregar usuarios, grupos y aplicaciones de Azure AD a tu cuenta del centro de partners
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, aplicación de azure ad, aad, usuario, grupo, varios usuarios, multiusuario
ms.localizationpriority: medium
ms.openlocfilehash: 0ecdcf2b148f53fefb5edc7e1f2df0d6bab58475
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8792212"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Agregar usuarios, grupos y aplicaciones de Azure AD a tu cuenta del centro de partners

La sección de **los usuarios** del [Centro de partners](https://partner.microsoft.com/dashboard) (en la **configuración de la cuenta**) te permite usar Azure Active Directory para agregar usuarios a tu cuenta del centro de partners. A cada usuario se le asigna un rol (o conjunto de permisos personalizados) que define su acceso a la cuenta. También puedes agregar [grupos de usuarios](#groups) y [aplicaciones de Azure AD](#azure-ad-applications) para concederles acceso a tu cuenta del centro de partners.

Después de que los usuarios se hayan agregado a la cuenta, puedes [editar detalles de la cuenta](#edit), cambiar [roles y permisos](set-custom-permissions-for-account-users.md) o [eliminar usuarios](#remove).

> [!IMPORTANT]
> Para agregar usuarios a tu cuenta, debes primera [asociar tu cuenta del centro de partners con el inquilino de Azure Active Directory de la organización](associate-azure-ad-with-partner-center.md). 

Al agregar usuarios, tendrás que especificar su acceso a tu cuenta del centro de partners, asignándoles un [rol o conjunto de permisos personalizados](set-custom-permissions-for-account-users.md). 

Ten en cuenta que todos los usuarios del centro de partners (incluyendo grupos y aplicaciones de Azure AD) deben tener una cuenta activa en [un inquilino de Azure AD que está asociado con tu cuenta del centro de partners](associate-azure-ad-with-partner-center.md). La administración de usuarios se realiza en un inquilino cada vez; debes iniciar sesión con una cuenta de administrador para el inquilino en el que quieres agregar o editar usuarios. Crear un nuevo usuario en el centro de partners, también se creará una cuenta para dicho usuario en el inquilino de Azure AD en la que se inicia sesión y realizar cambios en un nombre de usuario en el centro de partners se reflejarán en el inquilino de Azure AD de tu organización.

> [!NOTE]
> Si tu organización usa la [integración de directorios](http://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar el servicio de directorio local con Azure AD, no podrás crear nuevos usuarios, grupos ni aplicaciones de Azure AD en el centro de partners. Tu (u otro administrador en el directorio local) deberá crearlos directamente en el directorio local podrás ver y agregarlos en el centro de partners.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Agregar usuarios a tu cuenta del centro de partners

Para agregar usuarios a tu cuenta del centro de partners, ve a la página de **usuarios** en la **configuración de la cuenta** y selecciona **Agregar usuarios.** Debes haber iniciado sesión con una cuenta de administrador para el inquilino de Azure AD en el que quieres trabajar. 

### <a name="add-existing-users"></a>Agregar usuarios existentes 

Puedes seleccionar los usuarios que ya existen en el inquilino de su organización y darles acceso a tu cuenta del centro de partners. 

<span id="from-directory" />

1.  Selecciona el icono de engranaje (cerca de la esquina superior derecha del centro de partners) y, a continuación, selecciona la **Configuración del desarrollador**. En el menú de **configuración** , seleccionar **usuarios**.
2.  En la página **Usuarios**, selecciona **Agregar usuarios**. 
3.  Selecciona uno o varios usuarios de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
    > [!TIP]
    > Si seleccionas más de un usuario para agregar a tu cuenta del centro de partners, debes asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varios usuarios con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.
4.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los usuarios seleccionados.
6.  Haz clic en **Guardar**.

### <a name="additional-methods-for-adding-users"></a>Métodos adicionales para agregar usuarios

Si has iniciado sesión con una cuenta de administrador que también tiene permisos de [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el inquilino de Azure AD que estás trabajando, tendrás opciones adicionales para agregar usuarios a tu cuenta del centro de partners. Deberás seleccionar una de las opciones siguientes:

-   **Agregar usuarios existentes**: elige los usuarios que ya existen en el directorio de la organización y darles acceso a tu cuenta del centro de partners, mediante el método que se ha descrito anteriormente.
-   **Crear nuevos usuarios**: crear cuentas de usuario nuevo para agregar al directorio de la organización tanto y tu cuenta del centro de partners
-   **Invite outside users**: envía invitaciones por correo electrónico a los usuarios que no se encuentran actualmente en el directorio de tu organización. Se les invitará se para acceder a tu cuenta del centro de partners y se creará una nueva cuenta de [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) para ellos en el inquilino de Azure AD.

<span id="new-user" />

### <a name="create-new-users"></a>Crear nuevos usuarios

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de AzureAD para crear nuevos usuarios.

1.  En la página de **los usuarios** (en la **configuración de la cuenta**), selecciona **Agregar usuarios**y luego elige **crear nuevos usuarios**.
2.  Escribe el nombre, el apellido y el nombre de usuario del nuevo usuario.
3.  Si quieres que el usuario nuevo tenga una [cuenta de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) en el directorio de la organización, activa la casilla con la etiqueta **Make this user a Global administrator in your Azure AD, with full control over all directory resources**. De esta forma, el usuario podrá obtener acceso a todas las características administrativas en el Azure AD de tu empresa. Podrán agregar y administrar usuarios en el directorio de la organización (aunque el centro de partners no, a menos que conceda a la cuenta los [permisos de rol](set-custom-permissions-for-account-users.md)de adecuados). Si activas esta casilla, deberás proporcionar un **correo electrónico de recuperación de contraseña** para el usuario.
4.  Si has activado la casilla **Make this user a Global administrator in your Azure AD**, escribe un correo electrónico que el usuario pueda usar si necesita recuperar su contraseña.
5.  En la sección **Pertenencia al grupo**, selecciona cualquier grupo al que quieres que pertenezca el nuevo usuario.
6.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
7.  Haz clic en **Guardar**.
8.  En la página de confirmación, verás la información de inicio de sesión del nuevo usuario, incluida una contraseña temporal. Asegúrate de anotar dicha información y proporciónasela al usuario nuevo, ya que no podrás obtener acceso a la contraseña temporal después de salir de esta página.


<span id="email" />

### <a name="invite-outside-users"></a>Invitar a usuarios externos

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de AzureAD para invitar usuarios externos.

1.  En la página de **los usuarios** (en la **configuración de la cuenta**), selecciona **Agregar usuarios**y luego elige **Invitar a usuarios por correo electrónico**.
1.  Escribe una o varias direcciones de correo electrónico (hasta diez), separadas por coma o punto y coma.
2.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
3.  Haz clic en **Guardar**.

Los usuarios que has invitado recibirán una invitación por correo electrónico para unirse a tu cuenta y se les creará una nueva cuenta de [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en el inquilino de AzureAD. Cada usuario tendrá que aceptar tu invitación para poder obtener acceso a tu cuenta.

Si tienes que reenviar una invitación, busca al usuario en tu página **Usuarios** y selecciona su dirección de correo electrónico (o el texto que dice **Invitación pendiente**). Luego, en la parte inferior de la página, haz clic en **Volver a enviar invitación**.

> [!IMPORTANT]
> Los usuarios externos que invites a unirse a tu cuenta del centro de partners se puede asignar los mismos roles y permisos que otros usuarios. Sin embargo, los usuarios externos no podrán realizar determinadas tareas en Visual Studio, como asociar una aplicación a Store o crear paquetes para cargar a Store. Si un usuario necesita realizar esas tareas, elige **Crear nuevos usuarios** en lugar de **Invitar a usuarios externos**. (Si no quieres agregar estos usuarios a tu inquilino de Azure AD existente, puedes [crear un nuevo inquilino](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) y, a continuación, crear nuevas cuentas de usuario para ellos en ese inquilino). 


### <a name="changing-a-users-directory-password"></a>Cambiar la contraseña del directorio de un usuario

Si uno de los usuarios necesita cambiar su contraseña, puede hacerlo él mismo si has proporcionado un **correo electrónico de recuperación de contraseña** al crear la cuenta de usuario. También puedes actualizar la contraseña de un usuario siguiendo los pasos siguientes (si has iniciado sesión con una cuenta de administrador global en tu inquilino de Azure AD para cambiar la contraseña de un usuario). Ten en cuenta que esto cambiará la contraseña del usuario en el inquilino de Azure AD, junto con la contraseña que utiliza para tener acceso al centro de partners. 

1.  En la página de **usuarios** (en la **configuración de la cuenta**), selecciona el nombre de la cuenta de usuario que quieres editar.
2.  Selecciona el botón de **Restablecer la contraseña** en la parte inferior de la página.
3.  Aparecerá una página de confirmación con la información de inicio de sesión del usuario, incluida una contraseña temporal.

    > [!IMPORTANT]
    >  Asegúrate de imprimir o copiar dicha información y proporcionársela al usuario, ya que no podrás acceder a la contraseña temporal después de salir de esta página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Agregar grupos a tu cuenta del centro de partners

Puedes agregar un grupo desde el directorio de la organización a tu cuenta del centro de partners. Al hacerlo, todos los usuarios que pertenezcan a dicho grupo podrán tener acceso a la cuenta, con los permisos asociados al rol asignado al grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Agregar grupos del directorio de la organización

1.  Selecciona el icono de engranaje (cerca de la esquina superior derecha del centro de partners) y, a continuación, selecciona la **Configuración del desarrollador**. En el menú de **configuración** , seleccionar **usuarios**.
2. En la página de **usuarios** , selecciona **Agregar grupos**.
2.  Selecciona uno o varios grupos de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar grupos específicos.
    > [!TIP]
    > Si seleccionas más de un grupo para agregar a tu cuenta del centro de partners, debes asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varios grupos con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar grupos, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los grupos seleccionados. Todos los miembros del grupo podrán tener acceso a tu cuenta de centro de partners con los permisos que apliques al grupo, independientemente de los roles y permisos asociados a sus cuentas individuales.
5.  Haz clic en **Guardar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Crear una nueva cuenta de grupo en el directorio de la organización y agregarla a tu cuenta del centro de partners

Si quieres conceder acceso al centro de partners a un nuevo grupo, puedes crear un nuevo grupo en la sección de **los usuarios** . Ten en cuenta que el nuevo grupo se creará en el directorio de la organización, no solo en tu cuenta del centro de partners.

1.  En la página de **usuarios** (en **Configuración del desarrollador**), haz clic en **Agregar grupos**.
2.  En la página siguiente, selecciona el **nuevo grupo**.
3.  Escribe el nombre para mostrar del nuevo grupo.
4.  Especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el grupo. Todos los miembros del grupo podrán tener acceso a tu cuenta de centro de partners con los permisos que apliques al grupo, independientemente de los roles y permisos asociados a sus cuentas individuales.
5.  Selecciona los usuarios que deseas asignar al nuevo grupo en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
6.  Cuando hayas terminado de seleccionar los usuarios, haz clic en **Agregar seleccionado** para agregarlos al nuevo grupo.
7.  Haz clic en **Guardar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Agregar aplicaciones de Azure AD a tu cuenta del centro de partners

Para permitir que las aplicaciones o servicios que forman parte de Azure la organización AD para acceder a tu cuenta del centro de partners. Estas cuentas de usuario de la aplicación de Azure AD pueden usarse para llamar a las API de REST proporcionadas por los [servicios de Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Agregar aplicaciones de AzureAD desde el directorio de la organización

1.  1.  Selecciona el icono de engranaje (cerca de la esquina superior derecha del centro de partners) y, a continuación, selecciona la **Configuración del desarrollador**. En el menú de **configuración** , seleccionar **usuarios**.
2. En la página **Usuarios**, selecciona **Agregar aplicaciones de Azure AD**.
3.  Selecciona una o varias aplicaciones de Azure AD en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar aplicaciones de AzureAD específicas.
    > [!TIP]
    > Si seleccionas más de una aplicación de Azure AD para agregar a tu cuenta del centro de partners, debes asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varias aplicaciones de AzureAD con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

4.  Cuando hayas terminado de seleccionar aplicaciones de AzureAD, haz clic en **Agregar seleccionada**.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para las aplicaciones de AzureAD seleccionadas.
6.  Haz clic en **Guardar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Crear una nueva aplicación de Azure AD de la cuenta en el directorio de la organización y agregarlo a tu cuenta del centro de partners

Si quieres conceder acceso al centro de partners a una nuevo Azure AD cuenta de aplicación, puedes crear una en la sección de **los usuarios** . Ten en cuenta que esto creará una nueva cuenta en el directorio de la organización, no solo en tu cuenta del centro de partners.

> [!TIP]
> Si usas principalmente esta aplicación de Azure AD para la autenticación del centro de partners y no necesitas que los usuarios accedan a ella directamente, puedes escribir cualquier dirección válida para la **Dirección URL de respuesta** y **URI de identificador de aplicación**, siempre y cuando esos valores no se usan en cualquier otro tipo de Azure Aplicación de AD en el directorio.

1.  En la página de **usuarios** (en la **configuración de la cuenta**), selecciona **Agregar aplicaciones de Azure AD**.
2.  En la página siguiente, selecciona la **aplicación de nuevo Azure AD**.
3.  Escribe la **dirección URL de respuesta** para la nueva aplicación de Azure AD. Esta es la dirección URL donde los usuarios pueden iniciar sesión y usar la aplicación de Azure AD (a veces, también conocida como dirección URL de la aplicación o dirección URL de inicio de sesión). La **dirección URL de respuesta** no puede superar los 256 caracteres y debe ser única en tu directorio.
4.  Introduce el **URI de id. de la aplicación** de la nueva aplicación de Azure AD. Se trata de un identificador lógico para la aplicación de Azure AD que se muestra cuando envía una solicitud de inicio de sesión único a Azure AD. Ten en cuenta que el **URI de identificador de aplicación** debe ser único para cada aplicación de Azure AD del directorio y no puede superar los 256 caracteres. Para obtener más información sobre el **URI de id. de aplicación**, consulta [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para la aplicación de AzureAD.
6.  Haz clic en **Guardar**.

Después de agregar o crear una aplicación de Azure AD, puedes volver a la sección **Usuarios** y seleccionar el nombre de la aplicación para revisar la configuración de la aplicación, incluidos los valores de Id. de inquilino, Id. de cliente, URL de respuesta y URI de id. de aplicación.

> [!NOTE]
> Si tienes previsto usar las API de REST que se incluyen con los [servicios de la Microsoft Store](../monetize/using-windows-store-services.md), necesitarás los valores de Id. de inquilino e Id. de cliente que se muestran en esta página para obtener un token de acceso de AzureAD que te permita autenticar las llamadas a los servicios.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Administrar claves para una aplicación de AzureAD

Si la aplicación de AzureAD lee y escribe datos en MicrosoftAzureAD, necesitará una clave. Puedes crear claves para una aplicación de Azure AD modificando su información en el centro de partners. También puedes quitar las claves que ya no sean necesarias.

1.  En la página de **usuarios** (en la **configuración de la cuenta**), selecciona el nombre de la aplicación de Azure AD.
    > [!TIP]
    > Al hacer clic en el nombre de la aplicación de Azure AD, verás todas las claves activas de la aplicación de Azure AD, incluida la fecha en la que se creó la clave y en la que expirará. Para quitar una clave que ya no sea necesaria, haz clic en **Quitar**.

2.  Para agregar una nueva clave, selecciona **Agregar nueva clave**.
3.  Verás una pantalla que muestra los valores **Id. de cliente** y **Clave**.
    > [!IMPORTANT]
    > Asegúrate de imprimir o copiar esta información, ya que no podrás volver a acceder a ella después de salir de esta página.

4.  Si quieres crear más claves, selecciona **Agregar otra clave**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar un usuario, grupo o aplicación de AzureAD

Después de agregar usuarios, grupos o aplicaciones de Azure AD a tu cuenta del centro de partners, puedes realizar cambios en la información de su cuenta. 

> [!IMPORTANT]
> Los cambios realizados en los [roles o permisos](set-custom-permissions-for-account-users.md) solo afectarán el acceso al centro de partners. Todos los demás cambios (por ejemplo, cambiar el nombre de un usuario o la pertenencia a grupos, o la dirección URL de respuesta y URI de identificador de aplicación para una aplicación de Azure AD) se reflejarán en tu inquilino de organización Azure AD, así como en tu cuenta del centro de partners. 

1.  En la página de **usuarios** (en la **configuración de la cuenta**), selecciona el nombre de usuario, grupo o cuenta de aplicación de Azure AD que quieres editar.
2.  Realiza los cambios que desees. A continuación se indican los elementos que puedes editar:
    -   En un **usuario**, puedes editar el nombre, apellido o nombre de usuario. También puedes seleccionar grupos, o anular su selección, en la sección **Pertenencia a grupos** para actualizar su pertenencia a los grupos.
    -   En un **grupo**, puedes editar el nombre del grupo. (Para actualizar la pertenencia al grupo, edita los usuarios que quieras agregar o quitar del grupo y realiza cambios en la sección **Pertenencia a grupos**).
    -   En una **Aplicación de Azure AD**, puedes especificar nuevos valores para las opciones **Dirección URL de respuesta** o **URI de identificador de aplicación**.
    Recuerda que estos cambios se realizarán en el directorio de la organización, así como en tu cuenta del centro de partners.
3.  Para realizar cambios relacionados con el acceso al centro de partners, selecciona o anula la selección de los roles que quieres aplicar, o selecciona **Personalizar los permisos** y realiza los cambios deseados. Estos cambios solo afectan el centro de partners acceder y no modificarán los permisos en el inquilino de Azure AD de tu organización.
3.  Haz clic en **Guardar**.


## <a name="view-history-for-account-users"></a>Ver el historial de usuarios de la cuenta

Como propietario de la cuenta, puedes ver el historial de exploración detallado de los usuarios adicionales que hayas agregado a la cuenta.

En la página de **usuarios** (en la **configuración de la cuenta**), selecciona el vínculo que se muestra en la **última actividad** del usuario cuyo historial de exploración quieres revisar. Podrás ver las direcciones URL de todas las páginas que el usuario ha visitado en los últimos 30 días.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Quitar usuarios, grupos y aplicaciones de AzureAD

Para quitar un usuario, grupo o aplicación de Azure AD de tu cuenta del centro de partners, selecciona el vínculo **Quitar** que aparece junto al nombre en la página de **los usuarios** . Después de confirmar que quieres quitarla, ese usuario, grupo o aplicación de Azure AD ya no podrá tener acceso a tu cuenta del centro de partners (a menos que agregar de nuevo más adelante).

> [!IMPORTANT]
> Quitar un usuario, grupo o aplicación de Azure AD significa que ya no tendrá acceso a tu cuenta del centro de partners. Esto **no** elimina el usuario, grupo o aplicación de AzureAD del directorio de la organización.

 

