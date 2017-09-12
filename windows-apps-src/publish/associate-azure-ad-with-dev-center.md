---
author: jnHs
Description: "Para agregar y administrar usuarios de la cuenta, primero tienes que asociar tu cuenta del Centro de desarrollo a AzureActiveDirectory de la organización."
title: Asociar AzureActiveDirectory a la cuenta del Centro de desarrollo
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: ace9470cc707206461baa8c3dd72828ea68a8eb4
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2017
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>Asociar AzureActiveDirectory a la cuenta del Centro de desarrollo

Para [agregar y administrar usuarios de la cuenta](add-users-groups-and-azure-ad-applications.md), primero tienes que asociar tu cuenta del Centro de desarrollo a AzureActiveDirectory de la organización. 

El Centro de desarrollo de Windows aprovecha AzureAD para la administración de varios usuarios y la asignación de roles. Si la organización ya usa Office365 u otros servicios empresariales de Microsoft, ya tienes AzureAD. De lo contrario, puedes crear un nuevo inquilino de AzureAD desde el Centro de desarrollo sin ningún coste adicional.

> [!IMPORTANT]
> Para asociar AzureAD a la cuenta del Centro de desarrollo, tendrás que iniciar sesión en tu inquilino de AzureAD con una cuenta de [administrador global](http://go.microsoft.com/fwlink/?LinkId=746654).
> 
> Después de asociar tu cuenta del Centro de desarrollo a AzureAD, siempre tendrás que iniciar sesión en el Centro de desarrollo con la cuenta de administrador global de AzureAD (y no con una cuenta de Microsoft personal) para agregar y administrar los usuarios de la cuenta.

Ten en cuenta que solo una cuenta del Centro de desarrollo puede asociarse a un inquilino de AzureAD. Del mismo modo, solo un inquilino de AzureAD puede asociarse con una cuenta del Centro de desarrollo. Una vez establecida la asociación, no podrás quitarla sin ponerte en contacto con soporte técnico.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Asociar la cuenta del Centro de desarrollo al inquilino de AzureAD existente de tu organización

Si tu organización ya usa Azure AD, sigue estos pasos para vincular tu cuenta del Centro de desarrollo.

1.  Ve a tu **Configuración de la cuenta** y haz clic en **Administrar usuarios**.
2.  Haz clic en el botón **Asociación de Azure AD con tu cuenta del Centro de desarrollo**.
3.  Inicia sesión en tu cuenta de AzureAD. Esta cuenta debe tener permisos de [administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para poder configurar la asociación.
4.  Revisa el nombre de la organización y de dominio de tu inquilino de AzureAD. Para completar la asociación, haz clic en **Confirmar**.
5.  Si la asociación es correcta, estarás listo para agregar y administrar usuarios de la cuenta en la sección **Administrar usuarios** del Centro de desarrollo.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Crea una nueva aplicación de AzureAD para asociar a tu cuenta del Centro de desarrollo

Si necesitas configurar una nueva aplicación de Azure AD para vincular a tu cuenta del Centro de desarrollo, sigue estos pasos.

1.  Ve a tu **Configuración de la cuenta** y haz clic en **Administrar usuarios**.
2.  Haz clic en el botón **Crear nuevo Azure AD**.
3.  Escribe la información de directorio para la nueva Azure AD:
 - **Nombre de dominio**: nombre único que usaremos para tu dominio de Azure AD, junto con ".onmicrosoft.com". Por ejemplo, si escribes "ejemplo", el dominio de Azure AD sería "ejemplo.onmicrosoft.com".
 - **Correo electrónico de contacto**: una dirección de correo electrónico donde nos podamos poner en contacto contigo sobre tu cuenta si es necesario.
 - **Global administrator user account info**: nombre, apellido, nombre de usuario y contraseña que quieres usar para la nueva cuenta de administrador global.
4.  Haz clic en **Crear** para confirmar la nueva información del dominio y de la cuenta.
5.  Inicia sesión con tu nuevo nombre de usuario y contraseña de administrador global de AzureAD para empezar a [agregar y administrar usuarios de la cuenta adicionales](add-users-groups-and-azure-ad-applications.md).



