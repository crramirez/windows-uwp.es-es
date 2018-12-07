---
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: Asociar Azure Active Directory con tu cuenta del centro de partners
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, inquilino de azure, inquilino de aad, inquilino de azure ad, administración de inquilinos, inquilinos
ms.localizationpriority: medium
ms.openlocfilehash: 9f807799740d7e832da2f6a6fa3ea63e00deaee4
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796456"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Asociar Azure Active Directory con tu cuenta del centro de partners

En orden para [Agregar y administrar usuarios de la cuenta](add-users-groups-and-azure-ad-applications.md), primero tienes que asociar tu cuenta del centro de partners con Azure Active Directory de la organización. 

[El centro de partners](https://partner.microsoft.com/dashboard) aprovecha Azure AD para la administración y el acceso a la cuenta de varios usuarios. Si la organización ya usa Office365 u otros servicios empresariales de Microsoft, ya tienes AzureAD. De lo contrario, puedes crear un nuevo inquilino de Azure AD desde dentro del centro de partners sin ningún coste adicional.

> [!TIP]
> En este tema es específico del programa de desarrolladores de aplicaciones de Windows en [El centro de partners](https://partner.microsoft.com/dashboard), pero la asociación de un inquilino y la administración de usuarios funcionan de forma similar para las cuentas en el programa de aplicaciones de escritorio de Windows (consulta el [Programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) para obtener más información) y en el programa de desarrolladores de Hardware de Windows (donde las referencias al rol de **Administrador** también se aplica a las cuentas de Hardware con el rol de **Administrador** ; consulta [Administración del panel](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) para obtener más información).

Un solo inquilino de Azure AD puede asociarse con varias cuentas del centro de partners. Solo debes tener un inquilino de Azure AD asociado con tu cuenta del centro de partners para agregar varios usuarios de la cuenta, pero también tienes la opción para agregar a varios inquilinos de Azure AD para una sola cuenta de centro de partners. Cualquier usuario con el rol de **Administrador** en la cuenta del centro de partners tendrán la opción de agregar y quitar a inquilinos de Azure AD de la cuenta.

> [!IMPORTANT]
> Después de asociar tu cuenta del centro de partners con el inquilino de Azure AD, para agregar y administrar usuarios de la cuenta en dicho inquilino, tendrás que iniciar sesión en el centro de partners como un usuario del mismo inquilino que tenga el rol de **Administrador** .


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Asociar tu cuenta del centro de partners con el inquilino de Azure AD existente de la organización

Si la organización ya usa Azure AD, sigue estos pasos para vincular tu cuenta del centro de partners.

1.  Desde [El centro de partners](https://partner.microsoft.com/dashboard) , selecciona el icono de engranaje (cerca de la esquina superior derecha del panel) y, a continuación, selecciona la **Configuración del desarrollador**. En el menú de **configuración** , selecciona **los inquilinos**.
2.  Selecciona la **asociación de Azure AD con tu cuenta del centro de partners**.
3.  Escribe tus credenciales de Azure AD del inquilino que quieras asociar.
4.  Revisa el nombre de la organización y de dominio de tu inquilino de AzureAD. Para completar la asociación, selecciona **Confirmar**.
5.  Si la asociación es correcta, estarás listo para agregar y administrar usuarios de la cuenta en la sección de **los usuarios** en el centro de partners.

> [!IMPORTANT]
> Para crear nuevos usuarios, o realizar otros cambios en Azure AD, tendrás que iniciar sesión en ese inquilino de Azure AD con una cuenta que tiene [permiso de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para dicho inquilino. Sin embargo, no necesitas el permiso de administrador global para asociar al inquilino o para agregar usuarios que ya existen en dicho inquilino a tu cuenta del centro de partners.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Crear un nuevo Azure AD para asociar con tu cuenta del centro de partners

Si necesitas configurar un nuevo Azure AD para vincular a tu cuenta del centro de partners, sigue estos pasos.

1.  Desde [El centro de partners](https://partner.microsoft.com/dashboard), selecciona el icono de engranaje (cerca de la esquina superior derecha del panel) y, a continuación, selecciona la **Configuración del desarrollador**. En el menú de **configuración** , selecciona **los inquilinos**.
2.  Selecciona **Crear nuevo Azure AD**.
3.  Escribe la información de directorio para el nuevo Azure AD:
    - **Nombre de dominio**: nombre único que usaremos para tu dominio de Azure AD, junto con ".onmicrosoft.com". Por ejemplo, si escribes "ejemplo", el dominio de Azure AD sería "ejemplo.onmicrosoft.com".
    - **Correo electrónico de contacto**: una dirección de correo electrónico donde nos podamos poner en contacto contigo sobre tu cuenta si es necesario.
    - **Global administrator user account info**: nombre, apellido, nombre de usuario y contraseña que quieres usar para la nueva cuenta de administrador global.
4.  Haz clic en **Crear** para confirmar la nueva información del dominio y de la cuenta.
5.  Inicia sesión con tu nuevo nombre de usuario y contraseña de administrador global de AzureAD para empezar a [agregar y administrar usuarios de la cuenta adicionales](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Administrar las asociaciones de inquilino de Azure AD

Después de haber asociado a un inquilino de Azure AD con tu cuenta del centro de partners, puedes agregar a nuevos inquilinos o quitar los existentes en la página de **inquilinos** .


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Agregar a varios inquilinos de Azure AD a tu cuenta del centro de partners

Cualquier usuario que tenga el rol de **Administrador** para una cuenta del centro de partners puede asociar a inquilinos de Azure AD con la cuenta.

Para asociar un nuevo inquilino, selecciona **Asociar otro inquilino de Azure AD** y luego sigue los pasos indicados anteriormente. Ten en cuenta que se te pedirán tus credenciales en el inquilino de Azure AD que quieres asociar.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Quitar a un inquilino de Azure AD de tu cuenta del centro de partners

Cualquier usuario que tenga el rol de **Administrador** para una cuenta del centro de partners puede quitar a inquilinos de Azure AD de la cuenta.

> [!IMPORTANT]
> Cuando se quita un inquilino, todos los usuarios que se han agregado a la cuenta del centro de partners desde ese inquilino ya no podrán iniciar sesión en la cuenta. 

Para quitar a un inquilino, busca su nombre en la página de **inquilinos** (en la **configuración de la cuenta**) y luego selecciona **Quitar**. Se te pedirá que confirmes que quieres quitar el inquilino. Una vez que lo haces, no a los usuarios en dicho inquilino podrá iniciar sesión en la cuenta del centro de partners y se quitarán los permisos que hayas configurado para los usuarios.

> [!TIP]
> No puedes quitar a un inquilino si actualmente estás inscrito en el centro de partners con una cuenta del mismo inquilino. Para quitar a un inquilino, debes iniciar sesión en el centro de partners como un **Administrador** para otro inquilino que está asociado con la cuenta. Si hay solo un inquilino asociado a la cuenta, dicho inquilino solo puede quitarse después de iniciar sesión con la cuenta de Microsoft que abrió dicha cuenta.


