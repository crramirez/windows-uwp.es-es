---
author: jnHs
Description: In order to add and manage account users, you must first associate your Dev Center account with your organization's Azure Active Directory.
title: Asociar AzureActiveDirectory a la cuenta del Centro de desarrollo
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, azure ad, inquilino de azure, inquilino de aad, inquilino de azure ad, administración de inquilinos, inquilinos
ms.localizationpriority: medium
ms.openlocfilehash: dd729d76705849c981516109da39bbd27c140286
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "3957287"
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>Asociar AzureActiveDirectory a la cuenta del Centro de desarrollo

Para [agregar y administrar usuarios de la cuenta](add-users-groups-and-azure-ad-applications.md), primero tienes que asociar tu cuenta del Centro de desarrollo a AzureActiveDirectory de la organización. 

El Centro de desarrollo de Windows saca partido de AzureAD para la administración de cuentas de varios usuarios y su acceso. Si la organización ya usa Office365 u otros servicios empresariales de Microsoft, ya tienes AzureAD. De lo contrario, puedes crear un nuevo inquilino de AzureAD desde el Centro de desarrollo sin ningún coste adicional.

> [!TIP]
> Este tema es específico del programa de desarrolladores de aplicaciones de Windows, pero la asociación de un inquilino y la administración de usuarios funcionan de forma similar para las cuentas del Programa de aplicaciones de escritorio de Windows (consulta [Programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) para obtener más información) y en el Programa de desarrolladores de hardware de Windows (donde las referencias al rol **Administrador** también se aplican a las cuentas de hardware con el rol **Administrador**; consulta [Administración del panel](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) para obtener más información).

Un solo un inquilino de AzureAD puede asociarse con varias cuentas del Centro de desarrollo. Solo debes tener un inquilino de Azure AD asociado a tu cuenta del Centro de desarrollo para agregar varios usuarios de la cuenta, pero también tienes la opción de agregar varios inquilinos de Azure AD a una sola cuenta del Centro de desarrollo. Cualquier usuario con el rol **Administrador** en la cuenta del Centro de desarrollo tendrá la opción de agregar y quitar inquilinos de Azure AD.

> [!IMPORTANT]
> Después de asociar tu cuenta del Centro de desarrollo con tu inquilino de Azure AD, con el fin de agregar y administrar usuarios de la cuenta en dicho inquilino, tendrás que iniciar sesión en el Centro de desarrollo como usuario del mismo inquilino que cuenta con el rol **Administrador**.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Asociar la cuenta del Centro de desarrollo al inquilino de AzureAD existente de tu organización

Si tu organización ya usa Azure AD, sigue estos pasos para vincular tu cuenta del Centro de desarrollo.

1.  En el [panel del centro de desarrollo de Windows](https://partner.microsoft.com/dashboard), selecciona el icono de engranaje (cerca de la esquina superior derecha del panel) y, a continuación, selecciona la **configuración de la cuenta**. En el menú de **configuración** , selecciona **los inquilinos**.
2.  Selecciona **Asociar Azure AD con tu cuenta del Centro de desarrollo**.
3.  Escribe tus credenciales de Azure AD del inquilino que quieras asociar.
4.  Revisa el nombre de la organización y de dominio de tu inquilino de AzureAD. Para completar la asociación, selecciona **Confirmar**.
5.  Si la asociación es correcta, estarás listo para agregar y administrar usuarios de la cuenta en la sección **Usuarios** del Centro de desarrollo.

> [!IMPORTANT]
> Para crear nuevos usuarios, o realizar otros cambios en Azure AD, tendrás que iniciar sesión en ese inquilino de Azure AD con una cuenta que tiene [permiso de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para dicho inquilino. Sin embargo, no necesitas el permiso de administrador global para asociar el inquilino o para agregar los usuarios que ya existen en dicho inquilino a tu cuenta del Centro de desarrollo.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Crear un nuevo AzureAD para asociar a tu cuenta del Centro de desarrollo

Si necesitas configurar un nuevo Azure AD para vincular a tu cuenta del Centro de desarrollo, sigue estos pasos.

1.  En el [panel del centro de desarrollo de Windows](https://partner.microsoft.com/dashboard), selecciona el icono de engranaje (cerca de la esquina superior derecha del panel) y, a continuación, selecciona la **configuración de la cuenta**. En el menú de **configuración** , selecciona **los inquilinos**.
2.  Selecciona **Crear nuevo Azure AD**.
3.  Escribe la información de directorio para el nuevo Azure AD:
    - **Nombre de dominio**: nombre único que usaremos para tu dominio de Azure AD, junto con ".onmicrosoft.com". Por ejemplo, si escribes "ejemplo", el dominio de Azure AD sería "ejemplo.onmicrosoft.com".
    - **Correo electrónico de contacto**: una dirección de correo electrónico donde nos podamos poner en contacto contigo sobre tu cuenta si es necesario.
    - **Global administrator user account info**: nombre, apellido, nombre de usuario y contraseña que quieres usar para la nueva cuenta de administrador global.
4.  Haz clic en **Crear** para confirmar la nueva información del dominio y de la cuenta.
5.  Inicia sesión con tu nuevo nombre de usuario y contraseña de administrador global de AzureAD para empezar a [agregar y administrar usuarios de la cuenta adicionales](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Administrar las asociaciones de inquilino de Azure AD

Después de haber asociado un inquilino de Azure AD con tu cuenta del Centro de desarrollo, puedes agregar nuevos inquilinos o quitar los existentes en la página **Inquilinos**.


### <a name="add-multiple-azure-ad-tenants-to-your-dev-center-account"></a>Agregar varios inquilinos de Azure AD a tu cuenta del Centro de desarrollo

Cualquier usuario que tenga el rol **Administrador** en una cuenta del Centro de desarrollo puede asociar inquilinos de Azure AD con la cuenta.

Para asociar un nuevo inquilino, selecciona **Asociar otro inquilino de Azure AD** y luego sigue los pasos indicados anteriormente. Ten en cuenta que se te pedirán tus credenciales en el inquilino de Azure AD que quieres asociar.


### <a name="remove-an-azure-ad-tenant-from-your-dev-center-account"></a>Quitar un inquilino de Azure AD de tu cuenta del Centro de desarrollo

Cualquier usuario que tenga el rol **Administrador** en una cuenta del Centro de desarrollo puede quitar inquilinos de Azure AD de la cuenta.

> [!IMPORTANT]
> Cuando se quita un inquilino, todos los usuarios que se han agregado a la cuenta del Centro de desarrollo desde ese inquilino ya no podrán iniciar sesión en la cuenta. 

Para quitar a un inquilino, busca su nombre en la página de **inquilinos** (en la **configuración de la cuenta**) y luego selecciona **Quitar**. Se te pedirá que confirmes que quieres quitar el inquilino. Una vez que hagas este paso, ningún usuario del Centro de desarrollo de dicho inquilino podrá iniciar sesión en la cuenta del Centro de desarrollo y se quitarán los permisos que hayas configurado para dichos usuarios.

> [!TIP]
> No puedes quitar un inquilino si actualmente has iniciado sesión en el Centro de desarrollo con una cuenta del mismo inquilino. Para quitar un inquilino, debes iniciar sesión en el Centro de desarrollo como **Administrador** para otro inquilino que esté asociado a la cuenta. Si hay solo un inquilino asociado a la cuenta, dicho inquilino solo puede quitarse después de iniciar sesión con la cuenta de Microsoft que abrió dicha cuenta.


