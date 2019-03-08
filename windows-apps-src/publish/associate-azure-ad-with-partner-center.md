---
Description: Con el fin de agregar y administrar los usuarios de cuentas, primero debe asociar su cuenta de centro de partners con Azure Active Directory de su organización.
title: Asociar Azure Active Directory con su cuenta de centro de partners
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, inquilino de azure, inquilino de aad, inquilino de azure ad, administración de inquilinos, inquilinos
ms.localizationpriority: medium
ms.openlocfilehash: aacfdb0044fa9b9368ecbd032629ed5e572ece99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605320"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Asociar Azure Active Directory con su cuenta de centro de partners

Para [agregar y administrar los usuarios con cuentas](add-users-groups-and-azure-ad-applications.md), primero debe asociar su cuenta de centro de partners con Azure Active Directory de su organización. 

[Centro de partners](https://partner.microsoft.com/dashboard) aprovecha Azure AD para la administración y acceso a la cuenta de varios usuarios. Si la organización ya usa Office 365 u otros servicios empresariales de Microsoft, ya tienes Azure AD. En caso contrario, puede crear un nuevo inquilino de Azure AD desde dentro del centro de partners, sin cargo adicional.

> [!TIP]
> Este tema es específico para el programa para desarrolladores de aplicaciones de Windows en [centro de partners](https://partner.microsoft.com/dashboard), pero la asociación de un inquilino y administración de usuarios funcionan de forma similar para las cuentas en el programa de aplicación de escritorio de Windows (consulte [Windows Programa de aplicación de escritorio](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) para obtener más información) y en el programa para desarrolladores de Hardware de Windows (donde las referencias a la **Manager** rol también se aplica a las cuentas de Hardware con el **administrador**  función; vea [panel administración](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) para obtener más información).

Un único inquilino de Azure AD puede asociarse a varias cuentas del centro de partners. Solo debe tener un inquilino de Azure AD asociado a su cuenta de centro de partners para agregar varios usuarios de la cuenta, pero también tiene la opción para agregar a varios inquilinos de Azure AD a una sola cuenta de centro de partners. Cualquier usuario con el **Manager** rol en la cuenta del centro de partners tendrá la opción de agregar y quitar los inquilinos de Azure AD de la cuenta.

> [!IMPORTANT]
> Después de asociar su cuenta de centro de partners con su inquilino de Azure AD, con el fin de agregar y administrar los usuarios de la cuenta de ese inquilino, necesitará iniciar sesión en el centro de asociados como un usuario en el mismo inquilino que tiene el **Manager** rol.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Asociar inquilino de Azure AD existente de su organización en su cuenta del centro de partners

Si su organización ya usa Azure AD, siga estos pasos para vincular la cuenta del centro de partners.

1.  Desde [centro de partners](https://partner.microsoft.com/dashboard), seleccione el icono de engranaje (cerca de la esquina superior derecha del panel) y, a continuación, seleccione **opciones del desarrollador**. En el **configuración** menú, seleccione **inquilinos**.
2.  Seleccione **asociar Azure AD con su cuenta de centro de partners**.
3.  Escribe tus credenciales de Azure AD del inquilino que quieras asociar.
4.  Revisa el nombre de la organización y de dominio de tu inquilino de Azure AD. Para completar la asociación, selecciona **Confirmar**.
5.  Si la asociación se realiza correctamente, a continuación, estará listo para agregar y administrar los usuarios de cuentas en el **usuarios** sección en el centro de partners.

> [!IMPORTANT]
> Para crear nuevos usuarios, o realizar otros cambios en Azure AD, tendrás que iniciar sesión en ese inquilino de Azure AD con una cuenta que tiene [permiso de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para dicho inquilino. Sin embargo, no es necesario el permiso de administrador global con el fin de asociar al inquilino o para agregar los usuarios que ya existen en ese inquilino a su cuenta de centro de partners.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Crear un nuevo de Azure AD para asociar su cuenta de centro de partners

Si necesita configurar un nuevo anuncio de Azure para vincular con la cuenta del centro de partners, siga estos pasos.

1.  Desde [centro de partners](https://partner.microsoft.com/dashboard), seleccione el icono de engranaje (cerca de la esquina superior derecha del panel) y, a continuación, seleccione **opciones del desarrollador**. En el **configuración** menú, seleccione **inquilinos**.
2.  Selecciona **Crear nuevo Azure AD**.
3.  Escribe la información de directorio para la nueva Azure AD:
    - **Nombre de dominio**: El nombre único que vamos a usar para el dominio de Azure AD, junto con ". onmicrosoft.com". Por ejemplo, si escribes "ejemplo", el dominio de Azure AD sería "ejemplo.onmicrosoft.com".
    - **Póngase en contacto con el correo electrónico**: Una dirección de correo electrónico donde podemos enviarte sobre su cuenta si es necesario.
    - **Información de cuenta de usuario de administrador global**: El nombre, último nombre, nombre de usuario y contraseña que desea usar para la nueva cuenta de administrador global.
4.  Haz clic en **Crear** para confirmar la nueva información del dominio y de la cuenta.
5.  Inicia sesión con tu nuevo nombre de usuario y contraseña de administrador global de Azure AD para empezar a [agregar y administrar usuarios de la cuenta adicionales](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Administrar las asociaciones de inquilino de Azure AD

Después de un inquilino de Azure AD que haya asociado con su cuenta de centro de partners, puede agregar nuevos inquilinos o quite los inquilinos existentes desde el **inquilinos** página.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Agregar a varios inquilinos de Azure AD a su cuenta de centro de partners

Cualquier usuario que tenga el **Manager** rol para una cuenta de centro de partners puede asociar los inquilinos de Azure AD con la cuenta.

Para asociar un nuevo inquilino, selecciona **Asociar otro inquilino de Azure AD** y luego sigue los pasos indicados anteriormente. Ten en cuenta que se te pedirán tus credenciales en el inquilino de Azure AD que quieres asociar.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Eliminar a un inquilino de Azure AD de su cuenta de centro de partners

Cualquier usuario que tenga el **Manager** rol para una cuenta de centro de partners puede quitar los inquilinos de Azure AD de la cuenta.

> [!IMPORTANT]
> Cuando se quita un inquilino, todos los usuarios que se han agregado a la cuenta del centro de partners de ese inquilino ya no podrá iniciar sesión en la cuenta. 

Para quitar un inquilino, busque su nombre en el **inquilinos** página (en **configuración de la cuenta**), a continuación, seleccione **quitar**. Se te pedirá que confirmes que quieres quitar el inquilino. Una vez hecho esto, ningún usuario en ese inquilino podrán iniciar sesión en la cuenta del centro de partners, y se quitarán los permisos que ha configurado para esos usuarios.

> [!TIP]
> No se puede quitar a un inquilino si has iniciado sesión en el centro de partners mediante una cuenta en el mismo inquilino. Para quitar un inquilino, debe iniciar sesión en el centro de partners como un **Manager** para otro inquilino que está asociado con la cuenta. Si hay solo un inquilino asociado a la cuenta, dicho inquilino solo puede quitarse después de iniciar sesión con la cuenta de Microsoft que abrió dicha cuenta.


