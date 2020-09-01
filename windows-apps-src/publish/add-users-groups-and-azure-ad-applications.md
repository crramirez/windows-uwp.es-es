---
Description: Puede Agregar usuarios, grupos y aplicaciones Azure AD a su cuenta del centro de Partners.
title: Agregar usuarios, grupos y aplicaciones Azure AD a la cuenta del centro de Partners
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, aplicación de Azure ad, AAD, usuario, grupo, varios usuarios, multiusuario
ms.localizationpriority: medium
ms.openlocfilehash: 1b156b396a1f14b4890a6be6b4259b7b2c87c9f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162099"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Agregar usuarios, grupos y aplicaciones Azure AD a la cuenta del centro de Partners

La sección **usuarios** del [centro de Partners](https://partner.microsoft.com/dashboard) (en configuración de la **cuenta**) permite usar Azure Active Directory para agregar usuarios a la cuenta del centro de Partners. A cada usuario se le asigna un rol (o un conjunto de permisos personalizados) que define su acceso a la cuenta. También puede agregar [grupos de usuarios](#groups) y [Azure ad aplicaciones](#azure-ad-applications) para concederles acceso a su cuenta del centro de Partners.

Una vez agregados los usuarios a la cuenta, puede [editar los detalles](#edit)de la cuenta, cambiar los [roles y los permisos](set-custom-permissions-for-account-users.md)o [quitar usuarios](#remove).

> [!IMPORTANT]
> Para agregar usuarios a su cuenta, primero debe [asociar la cuenta del centro de Partners con el inquilino de Azure Active Directory de su organización](associate-azure-ad-with-partner-center.md). 

Al agregar usuarios, tendrá que especificar su acceso a su cuenta del centro de Partners asignándoles un [rol o un conjunto de permisos personalizados](set-custom-permissions-for-account-users.md). 

Tenga en cuenta que todos los usuarios del centro de Partners (incluidos los grupos y las aplicaciones Azure AD) deben tener una cuenta activa en [un inquilino Azure ad que esté asociado a la cuenta del centro de Partners](associate-azure-ad-with-partner-center.md). La administración de usuarios se realiza en un inquilino a la vez; debe iniciar sesión con una cuenta de administrador para el inquilino en el que desea agregar o editar usuarios. La creación de un nuevo usuario en el centro de Partners también creará una cuenta para ese usuario en el inquilino de Azure AD en el que ha iniciado sesión y, si realiza cambios en el nombre de un usuario en el centro de Partners, realizará los mismos cambios en el inquilino de Azure AD de su organización.

> [!NOTE]
> Si la organización usa [integración de directorios](/previous-versions/azure/azure-services/jj573653(v=azure.100)) para sincronizar el servicio de directorio local con Azure AD, no podrá crear nuevos usuarios, grupos o aplicaciones de Azure AD en el Centro de partners. Usted (u otro administrador del directorio local) deberán crearlos directamente en el directorio local antes de poder verlos y agregarlos en el Centro de partners.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Agregar usuarios a la cuenta del centro de Partners

Para agregar usuarios a la cuenta del centro de Partners, vaya a la página **usuarios** en **configuración** de la cuenta y seleccione **Agregar usuarios.** Debe haber iniciado sesión con una cuenta de administrador para el Azure AD inquilino en el que desea trabajar. 

### <a name="add-existing-users"></a>Incorporación de usuarios ya existentes 

Puede seleccionar los usuarios que ya existen en el inquilino de su organización y concederles acceso a su cuenta del centro de Partners. 

<span id="from-directory" />

1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de Partners) y, a continuación, seleccione **configuración del desarrollador**. En el menú **configuración** , seleccione **usuarios**.
2.  En la página **usuarios** , seleccione **Agregar usuarios**. 
3.  Seleccione uno o varios usuarios en la lista que aparece. Puede usar el cuadro de búsqueda para buscar usuarios concretos.
    > [!TIP]
    > Si selecciona más de un usuario para agregar a la cuenta del centro de Partners, debe asignarle el mismo rol o conjunto de permisos personalizados. Para agregar varios usuarios con roles o permisos diferentes, repita los pasos siguientes para cada rol o conjunto de permisos personalizados.
4.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
5.  En la sección **roles** , especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los usuarios seleccionados.
6.  Haga clic en **Guardar**.

### <a name="additional-methods-for-adding-users"></a>Métodos adicionales para agregar usuarios

Si inició sesión con una cuenta de administrador que también tiene permisos de [administrador global](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el inquilino de Azure ad en el que está trabajando, tendrá opciones adicionales para agregar usuarios a la cuenta del centro de Partners. Deberá seleccionar una de las siguientes opciones:

-   **Agregar usuarios existentes**: elija los usuarios que ya existen en el directorio de su organización y concédale acceso a su cuenta del centro de Partners con el método descrito anteriormente.
-   **Crear nuevos usuarios**: cree nuevas cuentas de usuario de marca para agregarlas al directorio de la organización y a la cuenta del centro de Partners.
-   **Invitar a usuarios externos**: enviar invitaciones por correo electrónico a los usuarios que no están actualmente en el directorio de su organización. Se les invitará a acceder a la cuenta del centro de Partners y se creará una nueva cuenta de [usuario invitado](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en el inquilino de Azure ad.

<span id="new-user" />

### <a name="create-new-users"></a>Creación de nuevos usuarios

> [!IMPORTANT]
> Debe haber iniciado sesión con una cuenta de administrador global en el inquilino de Azure AD para poder crear nuevos usuarios.

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione **Agregar usuarios**y, a continuación, elija **crear nuevos usuarios**.
2.  Escribe el nombre, el apellido y el nombre de usuario del nuevo usuario.
3.  Si desea que el nuevo usuario tenga una [cuenta de administrador global](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) en el directorio de su organización, active la casilla **convertir este usuario en administrador global en el Azure ad, con control total sobre todos los recursos del directorio**. Esto le dará al usuario acceso completo a todas las características administrativas de la instancia de Azure AD de la empresa. Podrán agregar y administrar usuarios en el directorio de su organización (aunque no en el centro de Partners, a menos que conceda a la cuenta el [rol o los permisos](set-custom-permissions-for-account-users.md)adecuados). Si activas esta casilla, deberás proporcionar un **correo electrónico de recuperación de contraseña** para el usuario.
4.  Si activó la casilla para que **este usuario sea un administrador global en el Azure ad**, escriba un correo electrónico que el usuario pueda usar si necesita recuperar su contraseña.
5.  En la sección **Pertenencia a grupos**, seleccione los grupos a los que desee que pertenezca el nuevo usuario.
6.  En la sección **roles** , especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
7.  Haga clic en **Guardar**.
8.  En la página de confirmación, verás la información de inicio de sesión del nuevo usuario, incluida una contraseña temporal. Asegúrate de anotar dicha información y proporciónasela al usuario nuevo, ya que no podrás obtener acceso a la contraseña temporal después de salir de esta página.


<span id="email" />

### <a name="invite-outside-users"></a>Invitar a usuarios externos

> [!IMPORTANT]
> Debe haber iniciado sesión con una cuenta de administrador global en el inquilino de Azure AD para poder invitar a usuarios externos.

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione **Agregar usuarios**y, a continuación, elija **invitar a los usuarios por correo electrónico**.
1.  Escriba una o varias direcciones de correo electrónico (hasta diez), separadas por comas o por puntos y comas.
2.  En la sección **roles** , especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el usuario.
3.  Haga clic en **Guardar**.

Los usuarios a los que ha invitado recibirán una invitación por correo electrónico para unirse a su cuenta y se creará una nueva cuenta de [usuario invitado](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) en su inquilino de Azure ad. Cada usuario deberá aceptar la invitación antes de poder acceder a la cuenta.

Si necesita volver a enviar una invitación, busque el usuario en la página de **usuarios** y seleccione su dirección de correo electrónico (o el texto que dice **invitación pendiente**). A continuación, en la parte inferior de la página, haz clic en **Volver a enviar invitación**.

> [!IMPORTANT]
> Los usuarios externos a los que invite a la cuenta del centro de partners pueden tener asignados los mismos roles y permisos que otros usuarios. Sin embargo, los usuarios externos no podrán realizar ciertas tareas en Visual Studio, como asociar una aplicación a la tienda o crear paquetes para cargarlos en el almacén. Si un usuario necesita realizar estas tareas, elija **crear nuevos usuarios** en lugar de **invitar a usuarios externos**. (Si no desea agregar estos usuarios al inquilino de Azure AD existente, puede [crear un nuevo inquilino](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)y, a continuación, crear nuevas cuentas de usuario para ellos en ese inquilino). 


### <a name="changing-a-users-directory-password"></a>Cambiar la contraseña del directorio de un usuario

Si uno de los usuarios necesita cambiar su contraseña, puede hacerlo si proporcionó un **correo electrónico de recuperación de contraseña** al crear la cuenta de usuario. También puede actualizar la contraseña de un usuario siguiendo los pasos que se indican a continuación (si inició sesión con una cuenta de administrador global en el inquilino de Azure AD para cambiar la contraseña de un usuario). Tenga en cuenta que esta operación cambiará la contraseña del usuario en el inquilino de Azure AD y la contraseña que usa para acceder al Centro de partners. 

1.  En la página **Usuarios** (en **Configuración de la cuenta**), seleccione el nombre de la cuenta de usuario que desea editar.
2.  Seleccione el botón **Restablecer contraseña** situado en la parte inferior de la página.
3.  Aparecerá una página de confirmación con la información de inicio de sesión del usuario, incluida una contraseña temporal.

    > [!IMPORTANT]
    >  No olvide imprimir o copiar esta información y proporcionársela al usuario, ya que no podrá acceder a la contraseña temporal después de salir de esta página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Agregar grupos a la cuenta del centro de Partners

Puede Agregar un grupo del directorio de su organización a su cuenta del centro de Partners. Al hacerlo, cada usuario que sea miembro de ese grupo podrá tener acceso a él, con los permisos asociados al rol asignado del grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Agregar grupos del directorio de la organización

1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de Partners) y, a continuación, seleccione **configuración del desarrollador**. En el menú **configuración** , seleccione **usuarios**.
2. En la página **usuarios** , seleccione **agregar grupos**.
2.  Seleccione uno o varios grupos en la lista que aparece. Puede usar el cuadro de búsqueda para buscar grupos concretos.
    > [!TIP]
    > Si selecciona más de un grupo para agregar a la cuenta del Centro de partners, debe asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varios grupos con diferentes roles o permisos, repita los pasos siguientes para cada rol o conjunto de permisos personalizados.

3.  Cuando hayas terminado de seleccionar grupos, haz clic en **Agregar seleccionado**.
4.  En la sección **roles** , especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para los grupos seleccionados. Todos los miembros del grupo podrán acceder a su cuenta del centro de Partners con los permisos que aplique al grupo, independientemente de los roles y permisos asociados a su cuenta individual.
5.  Haga clic en **Guardar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Cree una nueva cuenta de grupo en el directorio de su organización y agréguela a su cuenta del centro de Partners.

Si desea conceder acceso al centro de partners a un grupo nuevo, puede crear un nuevo grupo en la sección **usuarios** . Tenga en cuenta que el nuevo grupo se creará en el directorio de su organización, no solo en la cuenta del centro de Partners.

1.  En la página **usuarios** (en **configuración del desarrollador**), haga clic en **agregar grupos**.
2.  En la siguiente página, seleccione **Nuevo grupo**.
3.  Escriba el nombre para mostrar del nuevo grupo.
4.  Especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para el grupo. Todos los miembros del grupo podrán acceder a su cuenta del centro de Partners con los permisos que aplique al grupo, independientemente de los roles y permisos asociados a su cuenta individual.
5.  Seleccione los usuarios que desea asignar al nuevo grupo en la lista que aparece. Puede usar el cuadro de búsqueda para buscar usuarios concretos.
6.  Cuando haya terminado de seleccionar usuarios, haga clic en **Agregar selección** para agregarlos al grupo nuevo.
7.  Haga clic en **Guardar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Agregar Azure AD aplicaciones a la cuenta del centro de Partners

Puede permitir que las aplicaciones o los servicios que forman parte del Azure AD de su organización tengan acceso a la cuenta del centro de Partners. Estas Azure AD cuentas de usuario de aplicación se pueden usar para llamar a las API de REST proporcionadas por los [servicios de Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Agregar aplicaciones de Azure AD desde el directorio de la organización

1.  1.  Seleccione el icono de engranaje (cerca de la esquina superior derecha del centro de Partners) y, a continuación, seleccione **configuración del desarrollador**. En el menú **configuración** , seleccione **usuarios**.
2. En la página **usuarios** , seleccione **Agregar Azure ad aplicaciones**.
3.  Seleccione una o varias aplicaciones de Azure AD en la lista que aparece. Puede usar el cuadro de búsqueda para buscar aplicaciones de Azure AD concretas.
    > [!TIP]
    > Si selecciona más de una aplicación de Azure AD para agregar a la cuenta del Centro de partners, debe asignarles el mismo rol o conjunto de permisos personalizados. Para agregar varias aplicaciones Azure AD con diferentes roles o permisos, repita los pasos siguientes para cada rol o conjunto de permisos personalizados.

4.  Cuando termine de seleccionar las aplicaciones de Azure AD, haga clic en **Agregar selección**.
5.  En la sección **roles** , especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para las aplicaciones Azure ad seleccionadas.
6.  Haga clic en **Guardar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Cree una nueva cuenta de aplicación de Azure AD en el directorio de su organización y agréguela a su cuenta del centro de Partners.

Si desea conceder acceso al centro de partners a una nueva cuenta de aplicación de Azure AD, puede crear una en la sección **usuarios** . Tenga en cuenta que esto creará una cuenta nueva en el directorio de su organización, no solo en la cuenta del centro de Partners.

> [!TIP]
> Si se utiliza principalmente esta aplicación de Azure AD para la autenticación en el Centro de partners y no necesita usuarios para acceder a ella directamente, puede especificar cualquier dirección válida como **URL de respuesta** y como **URI de id. de aplicación**, siempre y cuando ninguna otra aplicación de Azure AD use esos valores en el directorio.

1.  En la página **Usuarios** (en **Configuración de la cuenta**), seleccione **Agregar aplicaciones de Azure AD**.
2.  En la siguiente página, seleccione **New Azure AD application** (Nueva aplicación de Azure AD).
3.  Especifique la **URL de respuesta** de la nueva aplicación de Azure AD. Esta es la dirección URL en la que los usuarios pueden iniciar sesión y usar la aplicación de Azure AD (también se denomina a veces como URL de la aplicación o URL de inicio de sesión). La **URL de respuesta** no puede tener más de 256 caracteres y debe ser única dentro del directorio.
4.  Especifique el **URI de id. de aplicación** de la nueva aplicación de Azure AD. Se trata de un identificador lógico para la aplicación de Azure AD que se muestra cuando envía una solicitud de inicio de sesión único a Azure AD. Ten en cuenta que el **URI de identificador de aplicación** debe ser único para cada aplicación de Azure AD del directorio y no puede superar los 256 caracteres. Para obtener más información sobre el **URI de ID**. de aplicación, consulte integración de [aplicaciones con Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  En la sección **roles** , especifique los [roles o permisos personalizados](set-custom-permissions-for-account-users.md) para la aplicación Azure ad.
6.  Haga clic en **Guardar**.

Después de agregar o crear una aplicación de Azure AD, puede volver a la sección **Usuarios** y seleccionar el nombre de la aplicación para revisar la configuración de la aplicación, incluido el identificador del inquilino y del cliente, la URL de respuesta y el URI de identificador de aplicación.

> [!NOTE]
> Si tiene previsto usar las API de REST proporcionadas por los [servicios de Microsoft Store](../monetize/using-windows-store-services.md), necesitará los valores ID. de inquilino y identificador de cliente que se muestran en esta página para obtener un token de acceso Azure ad que puede usar para autenticar las llamadas a los servicios.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Administración de claves de una aplicación de Azure AD

Si la aplicación de Azure AD lee y escribe datos en Microsoft Azure AD, necesitará una clave. Puede crear claves para una aplicación Azure AD editando su información en el centro de Partners. También puede eliminar las claves que ya no son necesarias.

1.  En la página **Usuarios** (en **Configuración de la cuenta**), seleccione el nombre de la aplicación de Azure AD.
    > [!TIP]
    > Al hacer clic en el nombre de la aplicación Azure AD, verá todas las claves activas para la aplicación Azure AD, incluida la fecha en la que se creó la clave y cuándo expirará. Para quitar una clave que ya no sea necesaria, haz clic en **Quitar**.

2.  Para agregar una nueva clave, seleccione **Agregar nueva clave**.
3.  Verá una pantalla que muestra los valores de ID. de **cliente** y **clave** .
    > [!IMPORTANT]
    > Asegúrese de imprimir o copiar esta información, ya que no podrá acceder a ella de nuevo después de salir de esta página.

4.  Si desea crear más claves, seleccione **Add another key** (Agregar otra clave).

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar un usuario, un grupo o una aplicación Azure AD

Después de agregar usuarios, grupos o aplicaciones Azure AD a la cuenta del centro de Partners, puede realizar cambios en la información de su cuenta. 

> [!IMPORTANT]
> Los cambios que se realicen en [roles o permisos](set-custom-permissions-for-account-users.md) solo afectarán al acceso al centro de Partners. Todos los demás cambios (como cambiar el nombre o la pertenencia a grupos de un usuario, o la dirección URL de respuesta y el URI del ID. de aplicación para una aplicación Azure AD) se reflejarán en el inquilino de Azure AD de su organización, así como en la cuenta del centro de Partners. 

1.  En la página **usuarios** (en **configuración**de la cuenta), seleccione el nombre del usuario, grupo o Azure ad cuenta de aplicación que desea editar.
2.  Realice los cambios deseados. Los elementos que puede editar son los siguientes:
    -   Para un **usuario**, puede editar el nombre del usuario, el apellido o el nombre de usuario. También puede seleccionar o anular la selección de grupos en la sección **pertenencia a grupos** para actualizar la pertenencia a grupos.
    -   Para un **Grupo**, puede editar el nombre del grupo. (Para actualizar la pertenencia a grupos, edite los usuarios que desee agregar o quitar del grupo y realice cambios en la sección **pertenencia a grupos** ).
    -   En el caso de una **aplicación Azure ad**, puede especificar nuevos valores para la **dirección URL de respuesta** o el **URI de ID**. de aplicación.
    Recuerde que estos cambios se realizarán en el directorio de su organización, así como en la cuenta del centro de Partners.
3.  En el caso de los cambios relacionados con el acceso al centro de Partners, seleccione o anule la selección de los roles que desea aplicar o seleccione **personalizar permisos** y realice los cambios deseados. Estos cambios solo afectan al acceso al centro de Partners y no cambiarán los permisos del inquilino de Azure AD de su organización.
3.  Haga clic en **Guardar**.


## <a name="view-history-for-account-users"></a>Ver el historial de los usuarios de cuentas

Como propietario de la cuenta, puede ver el historial de exploración detallado de los usuarios adicionales que haya agregado a la cuenta.

En la página **usuarios** (en **configuración**de la cuenta), seleccione el vínculo que se muestra en **última actividad** para el usuario cuyo historial de exploración le gustaría revisar. Podrá ver las direcciones URL de todas las páginas que el usuario visitó en los últimos 30 días.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Quitar usuarios, grupos y aplicaciones Azure AD

Para quitar un usuario, un grupo o una aplicación Azure AD de la cuenta del centro de Partners, seleccione el vínculo **quitar** que aparece por su nombre en la página **usuarios** . Después de confirmar que desea quitarlo, el usuario, el grupo o la aplicación Azure AD ya no podrá tener acceso a su cuenta del centro de Partners (a menos que lo vuelva a agregar más tarde).

> [!IMPORTANT]
> La eliminación de un usuario, grupo o aplicación Azure AD significa que ya no tendrá acceso a su cuenta del centro de Partners. **No** elimina el usuario, el grupo o la aplicación Azure ad del directorio de su organización.

 