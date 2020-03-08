---
Description: Puede Agregar usuarios, grupos y aplicaciones Azure AD a su cuenta del centro de Partners.
title: Agregar usuarios, grupos y aplicaciones Azure AD a la cuenta del centro de Partners
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, aplicación de Azure ad, AAD, usuario, grupo, varios usuarios, multiusuario
ms.localizationpriority: medium
ms.openlocfilehash: 41467f51e02f3cc700e3759f33d6fd6eea3ac7a6
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852560"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Agregar usuarios, grupos y aplicaciones Azure AD a la cuenta del centro de Partners

La sección **usuarios** del [centro de Partners](https://partner.microsoft.com/dashboard) (en configuración de la **cuenta**) permite usar Azure Active Directory para agregar usuarios a la cuenta del centro de Partners. A cada usuario se le asigna un rol (o conjunto de permisos personalizados) que define su acceso a la cuenta. También puede agregar [grupos de usuarios](#groups) y [Azure ad aplicaciones](#azure-ad-applications) para concederles acceso a su cuenta del centro de Partners.

Después de que los usuarios se hayan agregado a la cuenta, puedes [editar detalles de la cuenta](#edit), cambiar [roles y permisos](set-custom-permissions-for-account-users.md) o [eliminar usuarios](#remove).

> [!IMPORTANT]
> para agregar usuarios a su cuenta, primero debe [asociar la cuenta del centro de Partners con el inquilino de Azure Active Directory de su organización](associate-azure-ad-with-partner-center.md). 

Al agregar usuarios, tendrá que especificar su acceso a su cuenta del centro de Partners asignándoles un [rol o un conjunto de permisos personalizados](set-custom-permissions-for-account-users.md). 

Tenga en cuenta que todos los usuarios del centro de Partners (incluidos los grupos y las aplicaciones Azure AD) deben tener una cuenta activa en [un inquilino Azure ad que esté asociado a la cuenta del centro de Partners](associate-azure-ad-with-partner-center.md). La administración de usuarios se realiza en un inquilino cada vez; debes iniciar sesión con una cuenta de administrador para el inquilino en el que quieres agregar o editar usuarios. La creación de un nuevo usuario en el centro de Partners también creará una cuenta para ese usuario en el inquilino de Azure AD en el que ha iniciado sesión y, si realiza cambios en el nombre de un usuario en el centro de Partners, realizará los mismos cambios en el inquilino de Azure AD de su organización.

> [!NOTE]
> Si su organización usa la [integración de directorios](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573653(v=azure.100)?redirectedfrom=MSDN) para sincronizar el servicio de directorio local con el Azure ad, no podrá crear nuevos usuarios, grupos o aplicaciones de Azure ad en el centro de Partners. Usted (u otro administrador del directorio local) tendrá que crearlos directamente en el directorio local para poder verlos y agregarlos en el centro de Partners.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Agregar usuarios a la cuenta del centro de Partners

Para agregar usuarios a la cuenta del centro de Partners, vaya a la página **usuarios** en **configuración** de la cuenta y seleccione **Agregar usuarios.** Debes haber iniciado sesión con una cuenta de administrador para el inquilino de Azure AD en el que quieres trabajar. 

### <a name="add-existing-users"></a>Agregar usuarios existentes 

Puede seleccionar los usuarios que ya existen en el inquilino de su organización y concederles acceso a su cuenta del centro de Partners. 

<span id="from-directory" />

1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de Partners) y, a continuación, seleccione **configuración del desarrollador**. En el menú **configuración** , seleccione **usuarios**.
2.  En la página **Usuarios**, selecciona **Agregar usuarios**. 
3.  Selecciona uno o varios usuarios de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
    > [!TIP]
    > Si selecciona más de un usuario para agregar a la cuenta del centro de Partners, debe asignarle el mismo rol o conjunto de permisos personalizados. Para agregar varios usuarios con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.
4.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los usuarios seleccionados.
6.  Haga clic en **Guardar**.

### <a name="additional-methods-for-adding-users"></a>Métodos adicionales para agregar usuarios

Si inició sesión con una cuenta de administrador que también tiene permisos de [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el inquilino de Azure ad en el que está trabajando, tendrá opciones adicionales para agregar usuarios a la cuenta del centro de Partners. Deberás seleccionar una de las opciones siguientes:

-   **Agregar usuarios existentes**: elija los usuarios que ya existen en el directorio de su organización y concédale acceso a su cuenta del centro de Partners con el método descrito anteriormente.
-   **Crear nuevos usuarios**: cree nuevas cuentas de usuario de marca para agregarlas al directorio de la organización y a la cuenta del centro de Partners.
-   **Invite outside users**: envía invitaciones por correo electrónico a los usuarios que no se encuentran actualmente en el directorio de tu organización. Se les invitará a acceder a la cuenta del centro de Partners y se creará una nueva cuenta de [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en el inquilino de Azure ad.

<span id="new-user" />

### <a name="create-new-users"></a>Crear nuevos usuarios

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de Azure AD para crear nuevos usuarios.

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione **Agregar usuarios**y, a continuación, elija **crear nuevos usuarios**.
2.  Escribe el nombre, el apellido y el nombre de usuario del nuevo usuario.
3.  Si quieres que el usuario nuevo tenga una [cuenta de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) en el directorio de la organización, activa la casilla con la etiqueta **Make this user a Global administrator in your Azure AD, with full control over all directory resources**. De esta forma, el usuario podrá obtener acceso a todas las características administrativas en el Azure AD de tu empresa. Podrán agregar y administrar usuarios en el directorio de su organización (aunque no en el centro de Partners, a menos que conceda a la cuenta el [rol o los permisos](set-custom-permissions-for-account-users.md)adecuados). Si activas esta casilla, deberás proporcionar un **correo electrónico de recuperación de contraseña** para el usuario.
4.  Si has activado la casilla **Make this user a Global administrator in your Azure AD**, escribe un correo electrónico que el usuario pueda usar si necesita recuperar su contraseña.
5.  En la sección **Pertenencia al grupo**, selecciona cualquier grupo al que quieres que pertenezca el nuevo usuario.
6.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
7.  Haga clic en **Guardar**.
8.  En la página de confirmación, verás la información de inicio de sesión del nuevo usuario, incluida una contraseña temporal. Asegúrate de anotar dicha información y proporciónasela al usuario nuevo, ya que no podrás obtener acceso a la contraseña temporal después de salir de esta página.


<span id="email" />

### <a name="invite-outside-users"></a>Invitar a usuarios externos

> [!IMPORTANT]
> Debes iniciar sesión con una cuenta de administrador global en el inquilino de Azure AD para invitar usuarios externos.

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione **Agregar usuarios**y, a continuación, elija **invitar a los usuarios por correo electrónico**.
1.  Escribe una o varias direcciones de correo electrónico (hasta diez), separadas por coma o punto y coma.
2.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
3.  Haga clic en **Guardar**.

Los usuarios que has invitado recibirán una invitación por correo electrónico para unirse a tu cuenta y se les creará una nueva cuenta de [usuario invitado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en el inquilino de Azure AD. Cada usuario tendrá que aceptar tu invitación para poder obtener acceso a tu cuenta.

Si tienes que reenviar una invitación, busca al usuario en tu página **Usuarios** y selecciona su dirección de correo electrónico (o el texto que dice **Invitación pendiente**). A continuación, en la parte inferior de la página, haz clic en **Volver a enviar invitación**.

> [!IMPORTANT]
> Los usuarios externos a los que invite a la cuenta del centro de partners pueden tener asignados los mismos roles y permisos que otros usuarios. Sin embargo, los usuarios externos no podrán realizar determinadas tareas en Visual Studio, como asociar una aplicación a Store o crear paquetes para cargar a Store. Si un usuario necesita realizar esas tareas, elige **Crear nuevos usuarios** en lugar de **Invitar a usuarios externos**. (Si no quieres agregar estos usuarios a tu inquilino de Azure AD existente, puedes [crear un nuevo inquilino](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) y, a continuación, crear nuevas cuentas de usuario para ellos en ese inquilino). 


### <a name="changing-a-users-directory-password"></a>Cambiar la contraseña del directorio de un usuario

Si uno de los usuarios necesita cambiar su contraseña, puede hacerlo él mismo si has proporcionado un **correo electrónico de recuperación de contraseña** al crear la cuenta de usuario. También puedes actualizar la contraseña de un usuario siguiendo los pasos siguientes (si has iniciado sesión con una cuenta de administrador global en tu inquilino de Azure AD para cambiar la contraseña de un usuario). Tenga en cuenta que esto cambiará la contraseña del usuario en el inquilino de Azure AD, junto con la contraseña que usan para acceder al centro de Partners. 

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione el nombre de la cuenta de usuario que desea editar.
2.  Seleccione el botón **Restablecer contraseña** situado en la parte inferior de la página.
3.  Aparecerá una página de confirmación con la información de inicio de sesión del usuario, incluida una contraseña temporal.

    > [!IMPORTANT]
    >  Asegúrese de imprimir o copiar esta información y proporcionarla al usuario, ya que no podrá tener acceso a la contraseña temporal después de salir de esta página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Agregar grupos a la cuenta del centro de Partners

Puede Agregar un grupo del directorio de su organización a su cuenta del centro de Partners. Al hacerlo, todos los usuarios que pertenezcan a dicho grupo podrán tener acceso a la cuenta, con los permisos asociados al rol asignado al grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Agregar grupos del directorio de la organización

1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de Partners) y, a continuación, seleccione **configuración del desarrollador**. En el menú **configuración** , seleccione **usuarios**.
2. En la página **usuarios** , seleccione **agregar grupos**.
2.  Selecciona uno o varios grupos de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar grupos específicos.
    > [!TIP]
    > Si selecciona más de un grupo para agregar a la cuenta del centro de Partners, deberá asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varios grupos con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar grupos, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los grupos seleccionados. Todos los miembros del grupo podrán acceder a su cuenta del centro de Partners con los permisos que aplique al grupo, independientemente de los roles y permisos asociados a su cuenta individual.
5.  Haga clic en **Guardar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Cree una nueva cuenta de grupo en el directorio de su organización y agréguela a su cuenta del centro de Partners.

Si desea conceder acceso al centro de partners a un grupo nuevo, puede crear un nuevo grupo en la sección **usuarios** . Tenga en cuenta que el nuevo grupo se creará en el directorio de su organización, no solo en la cuenta del centro de Partners.

1.  En la página **usuarios** (en **configuración del desarrollador**), haga clic en **agregar grupos**.
2.  En la página siguiente, seleccione **nuevo grupo**.
3.  Escribe el nombre para mostrar del nuevo grupo.
4.  Especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el grupo. Todos los miembros del grupo podrán acceder a su cuenta del centro de Partners con los permisos que aplique al grupo, independientemente de los roles y permisos asociados a su cuenta individual.
5.  Selecciona los usuarios que deseas asignar al nuevo grupo en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
6.  Cuando hayas terminado de seleccionar los usuarios, haz clic en **Agregar seleccionado** para agregarlos al nuevo grupo.
7.  Haga clic en **Guardar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Agregar Azure AD aplicaciones a la cuenta del centro de Partners

Puede permitir que las aplicaciones o los servicios que forman parte del Azure AD de su organización tengan acceso a la cuenta del centro de Partners. Estas cuentas de usuario de la aplicación de Azure AD pueden usarse para llamar a las API de REST proporcionadas por los [servicios de Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Agregar aplicaciones de Azure AD desde el directorio de la organización

1.  1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de Partners) y, a continuación, seleccione **configuración del desarrollador**. En el menú **configuración** , seleccione **usuarios**.
2. En la página **Usuarios**, selecciona **Agregar aplicaciones de Azure AD**.
3.  Selecciona una o varias aplicaciones de Azure AD en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar aplicaciones de Azure AD específicas.
    > [!TIP]
    > Si selecciona más de una aplicación Azure AD para agregarla a su cuenta del centro de Partners, debe asignarle el mismo rol o conjunto de permisos personalizados. Para agregar varias aplicaciones de Azure AD con distintos roles y permisos, repite los pasos siguientes para cada rol o conjunto de permisos personalizados.

4.  Cuando hayas terminado de seleccionar aplicaciones de Azure AD, haz clic en **Agregar seleccionado**.
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para las aplicaciones de Azure AD seleccionadas.
6.  Haga clic en **Guardar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Cree una nueva cuenta de aplicación de Azure AD en el directorio de su organización y agréguela a su cuenta del centro de Partners.

Si desea conceder acceso al centro de partners a una nueva cuenta de aplicación de Azure AD, puede crear una en la sección **usuarios** . Tenga en cuenta que esto creará una cuenta nueva en el directorio de su organización, no solo en la cuenta del centro de Partners.

> [!TIP]
> Si usa principalmente esta aplicación Azure AD para la autenticación del centro de Partners y no necesita que los usuarios accedan a ella directamente, puede escribir cualquier dirección válida para la **dirección URL de respuesta** y el **URI del ID**. de aplicación, siempre que no se usen los valores en ninguna otra aplicación Azure ad en el directorio.

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione **Agregar Azure ad aplicaciones**.
2.  En la página siguiente, seleccione **nueva Azure ad aplicación**.
3.  Escribe la **dirección URL de respuesta** para la nueva aplicación de Azure AD. Esta es la dirección URL donde los usuarios pueden iniciar sesión y usar la aplicación de Azure AD (a veces, también conocida como dirección URL de la aplicación o dirección URL de inicio de sesión). La **dirección URL de respuesta** no puede superar los 256 caracteres y debe ser única en tu directorio.
4.  Escribe el **URI de identificador de aplicación** para la nueva aplicación de Azure AD. Se trata de un identificador lógico para la aplicación de Azure AD que se muestra cuando envía una solicitud de inicio de sesión único a Azure AD. Ten en cuenta que el **URI de identificador de aplicación** debe ser único para cada aplicación de Azure AD del directorio y no puede superar los 256 caracteres. Para obtener más información sobre el **URI de id. de aplicación**, consulta [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  En la sección **Roles**, especifica los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para la aplicación de Azure AD.
6.  Haga clic en **Guardar**.

Después de agregar o crear una aplicación de Azure AD, puedes volver a la sección **Usuarios** y seleccionar el nombre de la aplicación para revisar la configuración de la aplicación, incluidos los valores de Id. de inquilino, Id. de cliente, URL de respuesta y URI de id. de aplicación.

> [!NOTE]
> Si tienes previsto usar las API de REST que se incluyen con los [servicios de la Microsoft Store](../monetize/using-windows-store-services.md), necesitarás los valores de Id. de inquilino e Id. de cliente que se muestran en esta página para obtener un token de acceso de Azure AD que te permita autenticar las llamadas a los servicios.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Administrar claves para una aplicación de Azure AD

Si la aplicación de Azure AD lee y escribe datos en Microsoft Azure AD, necesitará una clave. Puede crear claves para una aplicación Azure AD editando su información en el centro de Partners. También puedes quitar las claves que ya no sean necesarias.

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione el nombre de la aplicación Azure ad.
    > [!TIP]
    > al hacer clic en el nombre de la aplicación Azure AD, verá todas las claves activas para la aplicación Azure AD, incluida la fecha en la que se creó la clave y cuándo expirará. Para quitar una clave que ya no sea necesaria, haz clic en **Quitar**.

2.  Para agregar una nueva clave, seleccione **Agregar nueva clave**.
3.  Verás una pantalla que muestra los valores **Id. de cliente** y **Clave**.
    > [!IMPORTANT]
    > Asegúrese de imprimir o copiar esta información, ya que no podrá acceder a ella de nuevo después de salir de esta página.

4.  Si desea crear más claves, seleccione **agregar otra clave**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar un usuario, grupo o aplicación de Azure AD

Después de agregar usuarios, grupos o aplicaciones Azure AD a la cuenta del centro de Partners, puede realizar cambios en la información de su cuenta. 

> [!IMPORTANT]
> Los cambios que se realicen en [roles o permisos](set-custom-permissions-for-account-users.md) solo afectarán al acceso al centro de Partners. Todos los demás cambios (como cambiar el nombre o la pertenencia a grupos de un usuario, o la dirección URL de respuesta y el URI del ID. de aplicación para una aplicación Azure AD) se reflejarán en el inquilino de Azure AD de su organización, así como en la cuenta del centro de Partners. 

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione el nombre del usuario, grupo o Azure ad cuenta de aplicación que desea editar.
2.  Realiza los cambios que desees. A continuación se indican los elementos que puedes editar:
    -   En un **usuario**, puedes editar el nombre, apellido o nombre de usuario. También puedes seleccionar grupos, o anular su selección, en la sección **Pertenencia a grupos** para actualizar su pertenencia a los grupos.
    -   En un **grupo**, puedes editar el nombre del grupo. (Para actualizar la pertenencia al grupo, edita los usuarios que quieras agregar o quitar del grupo y realiza cambios en la sección **Pertenencia a grupos**).
    -   En una **Aplicación de Azure AD**, puedes especificar nuevos valores para las opciones **Dirección URL de respuesta** o **URI de identificador de aplicación**.
    Recuerde que estos cambios se realizarán en el directorio de su organización, así como en la cuenta del centro de Partners.
3.  En el caso de los cambios relacionados con el acceso al centro de Partners, seleccione o anule la selección de los roles que desea aplicar o seleccione **personalizar permisos** y realice los cambios deseados. Estos cambios solo afectan al acceso al centro de Partners y no cambiarán los permisos del inquilino de Azure AD de su organización.
3.  Haga clic en **Guardar**.


## <a name="view-history-for-account-users"></a>Ver el historial de usuarios de la cuenta

Como propietario de la cuenta, puedes ver el historial de exploración detallado de los usuarios adicionales que hayas agregado a la cuenta.

En la página **usuarios** (en **configuración**de la cuenta), seleccione el vínculo que se muestra en **última actividad** para el usuario cuyo historial de exploración le gustaría revisar. Podrás ver las direcciones URL de todas las páginas que el usuario ha visitado en los últimos 30 días.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Quitar usuarios, grupos y aplicaciones de Azure AD

Para quitar un usuario, un grupo o una aplicación Azure AD de la cuenta del centro de Partners, seleccione el vínculo **quitar** que aparece por su nombre en la página **usuarios** . Después de confirmar que desea quitarlo, el usuario, el grupo o la aplicación Azure AD ya no podrá tener acceso a su cuenta del centro de Partners (a menos que lo vuelva a agregar más tarde).

> [!IMPORTANT]
> La eliminación de un usuario, grupo o aplicación Azure AD significa que ya no tendrá acceso a su cuenta del centro de Partners. Esto **no** elimina el usuario, grupo o aplicación de Azure AD del directorio de la organización.

 

