---
Description: Para agregar y administrar usuarios de cuenta, primero debe asociar la cuenta del centro de Partners con el Azure Active Directory de su organización.
title: Asociar Azure Active Directory con su cuenta del centro de Partners
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Azure ad, inquilino de Azure, inquilino de AAD, inquilino de Azure ad, administración de inquilinos, inquilinos
ms.localizationpriority: medium
ms.openlocfilehash: fa26f4ea3fa91f43dea117ff5ffd2fec87514544
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846765"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Asociar Azure Active Directory con su cuenta del centro de Partners

Para [Agregar y administrar usuarios de cuenta](add-users-groups-and-azure-ad-applications.md), primero debe asociar la cuenta del centro de Partners con el Azure Active Directory de su organización. 

El [centro de Partners](https://partner.microsoft.com/dashboard) aprovecha Azure ad para el acceso y la administración de cuentas de varios usuarios. Si su organización ya usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene Azure AD. De lo contrario, puede crear un nuevo inquilino de Azure AD desde el centro de Partners sin cargo adicional.

> [!TIP]
> Este tema es específico del programa para desarrolladores de aplicaciones de Windows en el [centro de Partners](https://partner.microsoft.com/dashboard), pero la Asociación de un inquilino y la administración de usuarios funciona de forma similar para las cuentas en el programa de aplicación de escritorio de Windows (consulte programa de aplicación de [escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) para obtener más información) y en el programa para desarrolladores de hardware de [Dashboard Administration](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) Windows (donde las referencias **al rol de** **Administrador** también se aplica

Un único inquilino de Azure AD puede asociarse a varias cuentas del centro de Partners. Solo necesita tener un inquilino de Azure AD asociado a la cuenta del centro de partners para agregar varios usuarios de la cuenta, pero también tiene la opción de agregar varios inquilinos de Azure AD a una sola cuenta de centro de Partners. Cualquier usuario con el rol **Administrador** en la cuenta del Centro de partners tendrá la opción de agregar y quitar inquilinos de Azure AD de la cuenta.

> [!IMPORTANT]
> Después de asociar la cuenta del centro de Partners con el inquilino de Azure AD, para agregar y administrar usuarios de cuenta en ese inquilino, deberá iniciar sesión en el centro de partners como usuario del mismo inquilino que tenga el rol de **Administrador** .


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Asociar la cuenta del centro de Partners con el inquilino de Azure AD existente de su organización

Si su organización ya usa Azure AD, siga estos pasos para vincular la cuenta del centro de Partners.

1.  En el [centro de Partners](https://partner.microsoft.com/dashboard), seleccione el icono de engranaje (cerca de la esquina superior derecha del panel) y, después, seleccione Configuración del **desarrollador**. En el menú **configuración** , seleccione **inquilinos**.
2.  Seleccione **asociar Azure ad con su cuenta del centro de Partners**.
3.  Escriba sus credenciales de Azure AD para el inquilino que desea asociar.
4.  Revise el nombre de dominio y de la organización para el inquilino de Azure AD. Para completar la asociación, seleccione **Confirmar**.
5.  Si la asociación se realiza correctamente, estará listo para agregar y administrar los usuarios de la cuenta en la sección **Usuarios** del Centro de partners.

> [!IMPORTANT]
> Con el fin de crear nuevos usuarios o realizar otros cambios en el Azure AD, deberá iniciar sesión en ese Azure AD inquilino con una cuenta que tenga [permiso de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para ese inquilino. Sin embargo, no necesita permisos de administrador global para asociar el inquilino o para agregar usuarios que ya existen en ese inquilino a su cuenta del centro de Partners.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Cree una nueva Azure AD de marca para asociarla a su cuenta del centro de Partners

Si necesita configurar una nueva Azure AD para vincularla a su cuenta del centro de Partners, siga estos pasos.

1.  En el [centro de Partners](https://partner.microsoft.com/dashboard), seleccione el icono de engranaje (cerca de la esquina superior derecha del panel) y, después, seleccione Configuración del **desarrollador**. En el menú **configuración** , seleccione **inquilinos**.
2.  Seleccione **crear nueva Azure ad**.
3.  Escriba la información del directorio para la nueva instancia de Azure AD:
    - **Nombre de dominio**: el nombre único que se va a usar para el dominio de Azure ad, junto con ". onmicrosoft.com". Por ejemplo, si escribió "ejemplo", el dominio de AD Azure sería "ejemplo.onmicrosoft.com".
    - **Dirección de correo electrónico de contacto**: una dirección de correo electrónico para ponernos en contacto con información sobre la cuenta si es necesario.
    - **Información de la cuenta de usuario del administrador global**: nombre, apellidos, nombre de usuario y contraseña que desea utilizar para la nueva cuenta de administrador global.
4.  Haz clic en **Crear** para confirmar la nueva información del dominio y de la cuenta.
5.  Inicie sesión con su nuevo Azure AD nombre de usuario y contraseña de administrador global para empezar a [Agregar y administrar usuarios de cuentas adicionales](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Administrar Azure AD asociaciones de inquilinos

Después de asociar un inquilino de Azure AD a la cuenta del centro de Partners, puede agregar nuevos inquilinos o quitar los inquilinos existentes de la página de **inquilinos** .


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Adición de varios inquilinos de Azure AD a la cuenta del centro de Partners

Cualquier usuario que tenga el rol de **Administrador** de una cuenta del centro de Partners puede asociar Azure ad inquilinos con la cuenta.

Para asociar un inquilino nuevo, seleccione **asociar otro inquilino de Azure ad**y, a continuación, siga los pasos indicados anteriormente. Tenga en cuenta que se le pedirán sus credenciales en el Azure AD inquilino que desea asociar.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Quitar un inquilino de Azure AD de la cuenta del centro de Partners

Cualquier usuario que tenga el rol de **Administrador** de una cuenta del centro de Partners puede quitar Azure ad inquilinos de la cuenta.

> [!IMPORTANT]
> Cuando se elimina un inquilino, todos los usuarios que se agregaron a la cuenta del Centro de partners de ese inquilino ya no podrán iniciar sesión en la cuenta. 

Para quitar un inquilino, busque su nombre en la página de **inquilinos** (en configuración de la **cuenta**) y, luego, seleccione **quitar**. Se le pedirá que confirme que desea eliminar el inquilino. Una vez hecho esto, ningún usuario de ese inquilino podrá iniciar sesión en la cuenta del Centro de partners y se eliminarán los permisos que haya configurado para esos usuarios.

> [!TIP]
> No se puede eliminar un inquilino si ha iniciado sesión en el Centro de partners con una cuenta de ese mismo inquilino. Para eliminar un inquilino, debe iniciar sesión en el Centro de partners como **Administrador** de otro inquilino que esté asociado con la cuenta. Si solo hay un inquilino asociado con la cuenta, ese inquilino solo se podrá eliminar después de iniciar sesión con la cuenta Microsoft que abrió la cuenta.


