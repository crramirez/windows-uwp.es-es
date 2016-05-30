---
author: jnHs
Description: Agrega usuarios a tu cuenta del Centro de desarrollo y asígnales roles con permisos específicos.
title: Administrar usuarios de la cuenta
ms.assetid: 9245F0D0-7D8F-4741-AFB4-FBA5601D0A9B
---

# Administrar usuarios de la cuenta


Puedes usar Azure Active Directory para agregar usuarios a tu cuenta del Centro de desarrollo. A cada usuario se le asigna un rol que le proporciona un conjunto específico de permisos para la cuenta. También se puede asignar un rol a un grupo de usuarios o a una aplicación de Azure AD.

> **Importante**  Para agregar y administrar usuarios de la cuenta, primero tienes que asociar tu cuenta del Centro de desarrollo a Azure Active Directory de la organización. Para ello, es necesario iniciar sesión en Azure AD con una cuenta de [administrador local](http://go.microsoft.com/fwlink/?LinkId=746654). Una vez establecida esta asociación, no podrás quitarla sin ponerte en contacto con soporte técnico.

 

## Asociar la cuenta del Centro de desarrollo a Azure Active Directory de la organización


El Centro de desarrollo de Windows aprovecha Azure Active Directory para la administración de varios usuarios y la asignación de roles. Si la organización ya usa Office 365 u otros servicios de Microsoft, ya tienes Azure AD. De lo contrario, puedes crear un nuevo Azure AD desde el Centro de desarrollo sin ningún coste adicional. 

Ten en cuenta que solo una cuenta del Centro de desarrollo puede asociarse con Azure AD. Del mismo modo, solo Azure AD puede asociarse con una cuenta del Centro de desarrollo.

> **Nota**  Solo puedes agregar los usuarios a tu cuenta del Centro de desarrollo si forman parte de Azure AD de tu organización (o si creas nuevas cuentas de Azure AD para ellos). No podrás agregar usuarios a tu cuenta del Centro de desarrollo con sus cuentas de Microsoft personales.

Para asociar tu cuenta del Centro de desarrollo a Azure AD existente de tu organización:

1.  Ve a tu **Configuración de la cuenta** y haz clic en **Administrar usuarios**.
2.  Haz clic en el botón **Associate Azure AD with your Dev Center account**.
3.  Inicia sesión en tu cuenta de Azure AD. Esta cuenta debe tener permisos de [administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para poder configurar la asociación.
4.  Revisa el nombre de la organización y de dominio de tu cuenta de Azure AD. Para completar la asociación, haz clic en **Confirmar**.
5.  Si la asociación es correcta, estarás listo para agregar y administrar usuarios de la cuenta en la página **Administrar usuarios** de tu cuenta, según se describe en las secciones siguientes.
  
Para crear una nueva Azure AD para asociar con la cuenta del Centro de desarrollo:
1.  Ve a tu **Configuración de la cuenta** y haz clic en **Administrar usuarios**.
2.  Haz clic en el botón **Create new Azure AD**.
3.  Escribe la información de directorio para la nueva Azure AD:
- **Nombre de dominio**: el nombre exclusivo que usaremos para el dominio de Azure AD, junto con ".onmicrosoft.com". Por ejemplo, si escribes "ejemplo", el dominio de Azure AD sería "ejemplo.onmicrosoft.com". 
- **Correo electrónico de contacto**: una dirección de correo electrónico donde nos podamos poner en contacto contigo sobre tu cuenta si es necesario.
- **Global administrator user account info**: el nombre, el apellido, el nombre de usuario y la contraseña que quieres usar para la nueva cuenta de administrador. 
4.  Haz clic en **Crear** para confirmar la nueva información del dominio y de la cuenta.
5.  Inicia sesión con tu nuevo nombre de usuario y contraseña de administrador global de Azure AD para empezar a agregar y administrar los usuarios adicionales de la cuenta en la página **Administrar usuarios** de tu cuenta, como se describe en las secciones siguientes.


> **Importante**  Después de asociar tu cuenta del Centro de desarrollo con Azure AD, siempre tendrás que iniciar sesión en el Centro de desarrollo con la cuenta de administrador global de Azure AD (y no con una cuenta de Microsoft personal) para agregar y administrar los usuarios de la cuenta.

## Agregar y administrar usuarios, grupos y aplicaciones de Azure AD de la cuenta


Una vez establecida la asociación, puedes agregar usuarios, grupos y aplicaciones de Azure AD a tu cuenta. También puedes cambiar los roles, editar los detalles de la cuenta o quitar usuarios.

> **Nota**  Si tu organización usa la [integración de directorios](http://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar el servicio de directorio local con Azure AD, no podrás crear nuevos usuarios, grupos ni aplicaciones de Azure AD en el Centro de desarrollo. Tu (u otro administrador en el directorio local) deberá crearlos directamente en el directorio local para que puedas verlos en el Centro de desarrollo y agregarlos a este.

Al administrar usuarios, ten en cuenta lo siguiente:

-   Todos los usuarios del Centro de desarrollo deben tener una cuenta activa en Azure AD de la organización.
-   Si creas un **nuevo** usuario o grupo en el Centro de desarrollo, también se agregará a Azure AD de la organización.
-   Los cambios de nombre de usuario o nombre efectuados en el Centro de desarrollo, se reflejarán en Azure AD de la organización.
-   Los usuarios (incluidos los grupos y las aplicaciones de Azure AD) pueden acceder a la totalidad de la cuenta del Centro de desarrollo con los permisos asociados a su rol asignado. No puedes limitar el acceso de un usuario para que solamente pueda trabajar con determinadas aplicaciones o productos desde la aplicación (IAP).
-   Puedes permitir que un usuario, un grupo o una aplicación de Azure AD acceda a varias funciones de un rol si seleccionas varios roles.
-   Un usuario con un determinado rol también puede formar parte de un grupo que tenga un rol diferente. En ese caso, el usuario tendrá acceso a la función asociada a ambos roles.

### Roles y permisos

A cada usuario, grupo o aplicación de Azure AD que agregas a una cuenta debes asignarle al menos uno de los siguientes roles. Cada rol tiene un conjunto específico de permisos para realizar determinadas funciones en la cuenta.

> **Nota**  El propietario de la cuenta es la persona que la creó inicialmente con una cuenta de Microsoft (en lugar de un usuario agregado a través de Azure AD). Dicho propietario de la cuenta es la única persona con acceso completo a la cuenta, lo que incluye la capacidad de eliminar aplicaciones, crear y editar todos los usuarios de la cuenta y modificar todas las opciones financieras y de la cuenta. Al crear paquetes de la aplicación en Microsoft Visual Studio, se debe usar la cuenta de Microsoft que se usó para crear la cuenta.

| Rol                 | Descripción              |
|----------------------|--------------------------|
| Administrador              | Tiene acceso completo a la cuenta, excepto para cambiar la configuración fiscal y de pago. Esto incluye la administración de usuarios en el Centro de desarrollo, pero recuerda que la capacidad de crear y eliminar usuarios depende de los permisos que tenga la cuenta en Azure AD. Es decir, si a un usuario se le asigna el rol Administrador, pero no tiene permisos de administrador en Azure AD de la organización, no podrá crear nuevos usuarios ni eliminar los usuarios del directorio (pero puede cambiar el rol que un usuario tiene en el Centro de desarrollo). |
| Desarrollador            | Puedes cargar paquetes y enviar aplicaciones y productos desde la aplicación, así como ver el [Informe de uso](usage-report.md) para obtener información detallada de telemetría. No puedes ver la configuración de la cuenta ni la información financiera.                                                                                                                                                                                                                                                                                                                     |
| Colaborador empresarial | Puede acceder a información financiera y establecer detalles de precios. No puede crear o enviar nuevas aplicaciones y productos desde la aplicación ni modificar la configuración de la cuenta.                                                                                                                                                                                                                                                                                                                                                              |
| Colaborador financiero  | Puede ver [informes de pago](payout-summary.md). No puede modificar las aplicaciones, los productos desde la aplicación ni la configuración de la cuenta.                                                                                                                                                                                                                                                                                                                                                                                 |
| Vendedor             | Puede [responder a las opiniones de los clientes](respond-to-customer-reviews.md) y ver [informes analíticos](analytics.md) que no sean financieros. No puede modificar las aplicaciones, los productos desde la aplicación ni la configuración de la cuenta.                                                                                                                                                                                                                                                                                                            |

> **Nota**  Los usuarios con el rol de administrador o desarrollador pueden enviar aplicaciones a través del panel. Sin embargo, al crear paquetes de la aplicación en Visual Studio, se debe usar la cuenta de Microsoft que se usa para abrir la cuenta de desarrollador, en lugar de una cuenta de Azure AD.

### Agregar y administrar usuarios de la cuenta

Para identificar los usuarios que quieres agregar a tu cuenta del Centro de desarrollo y asignarles roles, haz clic en **Agregar usuarios**.

Puedes agregar uno o varios usuarios del directorio de la organización a tu cuenta del Centro de desarrollo. Ten en cuenta que si agregas más de un usuario al mismo tiempo, debes asignarles el mismo rol. Si quieres agregar usuarios pero asignarles diferentes roles, repite los siguientes pasos para cada rol.

**Agregar usuarios del directorio de la organización**

1.  En la página **Administrar usuarios**, haz clic en **Agregar usuarios**.
2.  Selecciona uno o varios usuarios de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
3.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, selecciona uno o varios roles para asignar a este conjunto de usuarios.
5.  Haz clic en **Guardar**.

Si quieres conceder acceso al Centro de desarrollo a una nueva cuenta de usuario, puedes crear una en la sección **Administrar usuarios**. Ten en cuenta que esto creará una nueva cuenta en el directorio de la organización, no solo en tu cuenta del Centro de desarrollo.

**Crear una cuenta de usuario**

1.  En la página **Administrar usuarios**, haz clic en **Agregar usuarios**.
2.  En la página siguiente, haz clic en **Nuevo usuario**.
3.  Escribe el nombre, el apellido y el nombre de usuario del nuevo usuario.
4.  En la sección **Roles**, selecciona uno o varios roles para asignar al nuevo usuario.
5.  En la sección **Pertenencia al grupo**, selecciona cualquier grupo al que quieres que pertenezca el nuevo usuario.
6.  Haz clic en **Guardar**.
7.  En la página de confirmación, verás la información de inicio de sesión del nuevo usuario, incluida una contraseña temporal. Asegúrate de anotar dicha información y proporciónasela al usuario nuevo, ya que no podrás acceder a la contraseña temporal después de salir de esta página.

Puedes hacer cambios en las cuentas de usuario que agregaste a tu cuenta del Centro de desarrollo en la sección **Administrar usuarios**. Ten en cuenta que los cambios en el nombre del usuario o la pertenencia a grupos se reflejarán en el directorio de la organización, no solo en la cuenta del Centro de desarrollo. Los cambios realizados en un rol del usuario solo afectan el acceso de este al Centro de desarrollo.

**Editar cuenta de usuario**

1.  En la página **Administrar usuarios**, haz clic en el nombre de la cuenta de usuario que quieres editar.
2.  Realiza cualquiera de los siguientes cambios:
    -   Edita el nombre, el apellido o el nombre de usuario del usuario. Recuerda que estos cambios se realizarán en el directorio de la organización.
    -   En la sección **Roles**, selecciona o anula la selección de los roles que quieres agregar o quitar a este usuario.
    -   En la sección **Pertenencia al grupo**, selecciona o anula la selección de los grupos a los que quieres que se una el usuario o de los que quieres quitarlo. Recuerda que estos cambios se realizarán en el directorio de la organización.

3.  Haz clic en **Guardar**.

Si necesitas cambiar la contraseña de una cuenta de usuario que agregaste a tu cuenta del Centro de desarrollo, puedes hacerlo en la sección **Administrar usuarios**. Ten en cuenta que esto cambiará la contraseña de directorio del usuario, no solo la contraseña de su acceso al Centro de desarrollo.

**Cambiar la contraseña del directorio de un usuario**

1.  En la página **Administrar usuarios**, haz clic en el nombre de la cuenta de usuario que quieres editar.
2.  Haz clic en el botón **Restablecer la contraseña** en la parte inferior de la página.
3.  Aparecerá una página de confirmación con la información de inicio de sesión del usuario, incluida una contraseña temporal.
  > **Importante**  Asegúrate de imprimir o copiar dicha información y proporciónasela al usuario, ya que no podrás acceder a la contraseña temporal después de salir de esta página.

### Agregar y administrar grupos

Cuando agregas un grupo del directorio de la organización a tu cuenta del Centro de desarrollo, todos los usuarios que forman parte de ese grupo pueden acceder a ella, con los permisos asociados al rol asignado al grupo. Ten en cuenta que los cambios realizados en los grupos (incluidos su nombre o la pertenencia a) se reflejarán en el directorio de la organización.

Ten en cuenta que si agregas más de un grupo al mismo tiempo, debes asignarles el mismo rol. Si quieres agregar grupos pero asignarles diferentes roles, repite los siguientes pasos para cada rol.

**Agregar grupos del directorio de la organización**

1.  En la página **Administrar usuarios**, haz clic en **Agregar grupos**.
2.  Selecciona uno o varios grupos de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar grupos específicos.
3.  Cuando hayas terminado de seleccionar grupos, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, selecciona uno o varios roles para asignar a este conjunto de grupos.
5.  Haz clic en **Guardar**.

Si quieres conceder acceso al Centro de desarrollo a un nuevo grupo, puedes crear uno en la sección **Administrar usuarios**. Ten en cuenta que el nuevo grupo se creará en el directorio de la organización, no solo en tu cuenta del Centro de desarrollo.

**Crear una nueva cuenta de grupo**

1.  En la página **Administrar usuarios**, haz clic en **Agregar grupos**.
2.  En la página siguiente, haz clic en **Nuevo grupo**.
3.  Escribe el nombre para mostrar del nuevo grupo.
4.  Selecciona uno o varios roles para asignar al nuevo grupo. Todos los miembros del grupo podrán acceder a tu cuenta del Centro de desarrollo con los permisos asociados a ese rol.
5.  Selecciona uno o varios usuarios de la lista que se muestra. Puedes usar el cuadro de búsqueda para buscar usuarios específicos.
6.  Cuando hayas terminado de seleccionar usuarios, haz clic en **Agregar seleccionado**.
7.  Haz clic en **Guardar**.

Puedes realizar cambios en las cuentas de grupo que agregaste a tu cuenta del Centro de desarrollo en la sección **Administrar usuarios**. Ten en cuenta que los cambios en el nombre o la pertenencia del grupo se reflejarán en el directorio de la organización, no solo en la cuenta del Centro de desarrollo. Los cambios realizados en un rol del grupo solo afectan al acceso al Centro de desarrollo de ese grupo.

**Editar una cuenta de grupo**

1.  En la página **Administrar usuarios**, haz clic en el nombre de la cuenta de grupo que quieres editar.
2.  Para cambiar la información del grupo, realiza los cambios necesarios en el nombre del grupo. Recuerda que estos cambios se realizarán en el directorio de la organización.
3.  Para cambiar el rol del grupo, selecciona o anula la selección de los roles que quieres aplicar al grupo.
4.  Haz clic en **Guardar**.

### Agregar y administrar aplicaciones de Azure AD

Puedes permitir que las aplicaciones o servicios que forman parte de Azure AD de tu organización accedan a tu cuenta del Centro de desarrollo.

Ten en cuenta que si agregas más de una aplicación de Azure AD al mismo tiempo, debes asignarles el mismo rol. Si quieres agregar grupos pero asignarles diferentes roles, repite los siguientes pasos para cada rol.

**Agregar aplicaciones de Azure AD desde el directorio de la organización**

1.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**.
2.  Selecciona una o varias aplicaciones de Azure AD en la lista que aparece. Puedes usar el cuadro de búsqueda para buscar aplicaciones de Azure AD específicas.
3.  Cuando hayas terminado de seleccionar aplicaciones de Azure AD, haz clic en **Agregar seleccionado**.
4.  En la sección **Roles**, selecciona uno o varios roles para asignar a este conjunto de aplicaciones de Azure AD.
5.  Haz clic en **Guardar**.

Si quieres conceder acceso al Centro de desarrollo a una nueva cuenta de aplicación de Azure AD, puedes crear una en la sección **Administrar usuarios**. Ten en cuenta que esto creará una nueva cuenta en el directorio de la organización, no solo en tu cuenta del Centro de desarrollo.

**Crear una nueva aplicación de Azure AD**

1.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**.
2.  En la siguiente página, haz clic en **Nueva aplicación de Azure AD**.
3.  Escribe la **dirección URL de respuesta** para la nueva aplicación de Azure AD. Esta es la dirección URL donde los usuarios pueden iniciar sesión y usar la aplicación de Azure AD (a veces, también conocida como dirección URL de la aplicación o dirección URL de inicio de sesión). La **dirección URL de respuesta** no puede superar los 256 caracteres.
4.  Escribe el **URI de identificador de aplicación** para la nueva aplicación de Azure AD. Se trata de un identificador lógico para la aplicación de Azure AD que se muestra cuando envía una solicitud de inicio de sesión único a Azure AD. Ten en cuenta que el **URI de identificador de aplicación** debe ser único para cada aplicación de Azure AD del directorio y no puede superar los 256 caracteres.
5.  En la sección **Roles**, selecciona uno o varios roles para asignar a la nueva aplicación de Azure AD.
6.  Haz clic en **Guardar**.

Puedes realizar cambios en las aplicaciones de Azure AD que agregaste a tu cuenta del Centro de desarrollo en la sección **Administrar usuarios**. Ten en cuenta que los cambios en la dirección URL de respuesta y el URI de identificador de aplicación se reflejarán en el directorio de la organización, no solo en la cuenta del Centro de desarrollo. Los cambios en los roles solo afectarán a los permisos de la aplicación de Azure AD en el Centro de desarrollo.

**Editar una aplicación de Azure AD**

1.  En la página **Administrar usuarios**, haz clic en el nombre de la cuenta de aplicación de Azure AD que quieres editar.
2.  Para cambiar la **dirección URL de respuesta** o el **URI de identificador de aplicación**, escribe los nuevos valores aquí. Recuerda que estos cambios se realizarán en el directorio de la organización.
3.  Para cambiar el rol de la aplicación de Azure AD, selecciona o anula la selección de los roles que quieres aplicar.
4.  Haz clic en **Guardar**.

Si la aplicación de Azure AD lee y escribe datos en Microsoft Azure AD, necesitará una clave. Puedes crear claves para una aplicación de Azure AD modificando su información en el Centro de desarrollo. También puedes quitar las claves que ya no sean necesarias.

**Administrar claves para una aplicación de Azure AD**

1.  Desde la página **Administrar usuarios**, haz clic en el nombre de la aplicación de Azure AD.

    > **Sugerencia**  Al hacer clic en el nombre de la aplicación de Azure AD, verás todas las claves activas de la aplicación de Azure AD, incluida la fecha en la que se creó la clave y en la que expirará. Para quitar una clave que ya no sea necesaria, haz clic en **Quitar**.

2.  Para agregar una nueva clave, haz clic en **Agregar nueva clave**.

3.  Verás una pantalla que muestra los valores **Id. de cliente** y **Clave**.

    > **Importante**  Asegúrate de imprimir o copiar esta información, ya que no podrás volver a acceder a ella después de salir de esta página.

4.  Si quieres crear más claves, haz clic en **Agregar otra clave**.

### Quitar usuarios, grupos y aplicaciones de Azure AD

Para quitar un usuario, un grupo o una aplicación de Azure AD de tu cuenta del Centro de desarrollo, haz clic en el vínculo **Quitar** que aparece junto al nombre en la página **Administrar usuarios**. Después de confirmar que quieres quitarla, ese usuario, grupo o aplicación de Azure AD ya no tendrá acceso a tu cuenta del Centro de desarrollo (a menos que la agregues de nuevo más adelante).

> **Nota**  La eliminación de un usuario, un grupo o una aplicación de Azure AD significa que ya no tendrá acceso a tu cuenta del Centro de desarrollo. Esto no elimina el usuario, el grupo o la aplicación de Azure AD del directorio de la organización.

 

 

 






<!--HONumber=May16_HO2-->


