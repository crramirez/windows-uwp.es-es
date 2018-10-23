---
title: Caja de seguridad de credenciales
description: En este artículo se describe cómo las aplicaciones de la Plataforma universal de Windows (UWP) pueden usar la caja de seguridad de credenciales para almacenar y recuperar credenciales de usuario de forma segura, y cómo moverlas entre dispositivos con la cuenta Microsoft del usuario.
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: c6412f28e60ed0401fb96098fd38128a37491c8d
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5410564"
---
# <a name="credential-locker"></a>Caja de seguridad de credenciales




En este artículo se describe cómo las aplicaciones de la Plataforma universal de Windows (UWP) pueden usar la caja de seguridad de credenciales para almacenar y recuperar credenciales de usuario de forma segura, y cómo moverlas entre dispositivos con la cuenta Microsoft del usuario.

Por ejemplo, tienes una aplicación que se conecta a un servicio para obtener acceso a recursos protegidos como archivos multimedia o redes sociales. Tu servicio necesita la información de inicio de sesión de cada usuario. Has incorporado una interfaz de usuario a tu aplicación que obtiene el nombre de usuario y la contraseña que después se usan para iniciar la sesión del usuario en el servicio. Con la API de la caja de seguridad de credenciales, puedes guardar el nombre de usuario y la contraseña de tu usuario y recuperarlos fácilmente para iniciar la sesión del usuario automáticamente la próxima vez que abra tu aplicación, independientemente del dispositivo en que se encuentre.

Las credenciales de usuario almacenadas en CredentialLocker *no* expiran, *no* les afecta [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) y *no* se borran debido a la inactividad como los datos móviles tradicionales. Sin embargo, solo puedes almacenar hasta 20 credenciales por aplicación en CredentialLocker.

La caja de seguridad de credenciales funciona de forma diferente para cuentas de dominio. Si se guardan credenciales con tu cuenta Microsoft y asocias esa cuenta con una cuenta de dominio (como la cuenta que usas en el trabajo), las credenciales se trasferirán a esa cuenta de dominio. Sin embargo, las nuevas credenciales que se agregan al iniciar sesión con la cuenta de dominio no se incluirán en el perfil móvil. De esta manera se garantiza que las credenciales privadas del dominio no queden expuestas fuera del mismo.

## <a name="storing-user-credentials"></a>Almacenar credenciales de usuario


1.  Obtén una referencia a la caja de seguridad de credenciales usando el objeto [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) del espacio de nombres [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089).
2.  Crea un objeto [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) que contenga un identificador para tu aplicación, el nombre de usuario y la contraseña, y pásalo al método [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) para agregar la credencial a la caja de seguridad.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>Recuperar credenciales de usuario


Tienes varias maneras para recuperar credenciales de usuario de la caja de seguridad de credenciales una vez que tengas una referencia al objeto [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081).

-   Puedes recuperar todas las credenciales que el usuario ha proporcionado para tu aplicación en la caja de seguridad con el método [**PasswordVault.RetrieveAll**](https://msdn.microsoft.com/library/windows/apps/br227088).

-   Si conoces el nombre de usuario de las credenciales almacenadas, puedes recuperar todas las credenciales de ese nombre de usuario con el método [**PasswordVault.FindAllByUserName**](https://msdn.microsoft.com/library/windows/apps/br227084).

-   Si conoces el nombre de recurso de las credenciales almacenadas, puedes recuperar todas las credenciales de ese nombre de recurso con el método [**PasswordVault.FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083).

-   Por último, si conoces el nombre de usuario y el nombre de recurso de una credencial, puedes recuperar solo esa credencial con el método [**PasswordVault.Retrieve**](https://msdn.microsoft.com/library/windows/apps/br227087).

Veamos un ejemplo en el que hemos almacenado el nombre de recurso globalmente en una aplicación e iniciamos la sesión del usuario automáticamente si encontramos unas credenciales para él. Si encontramos varias credenciales para el mismo usuario, le pedimos que seleccione la credencial predeterminada que se usará para iniciar sesión.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    var loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }

    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}


private Windows.Security.Credentials.PasswordCredential GetCredentialFromLocker()
{
    Windows.Security.Credentials.PasswordCredential credential = null;

    var vault = new Windows.Security.Credentials.PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);
    if (credentialList.Count > 0)
    {
        if (credentialList.Count == 1)
        {
            credential = credentialList[0];
        }
        else
        {
            // When there are multiple usernames,
            // retrieve the default username. If one doesn't
            // exist, then display UI to have the user select
            // a default username.

            defaultUserName = GetDefaultUserNameUI();

            credential = vault.Retrieve(resourceName, defaultUserName);
        }
    }

    return credential;
}
```

## <a name="deleting-user-credentials"></a>Eliminar credenciales de usuario


Eliminar las credenciales de usuario en la caja de seguridad de credenciales también es un proceso rápido en dos pasos.

1.  Obtén una referencia a la caja de seguridad de credenciales usando el objeto [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) del espacio de nombres [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089).

2.  Pasa las credenciales que quieres eliminar al método [**PasswordVault.Remove**](https://msdn.microsoft.com/library/windows/apps/hh701242).

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>Procedimientos recomendados


Usa la caja de seguridad de credenciales solo para contraseñas y no para blobs de datos más grandes.

Guarda las contraseñas en la caja de seguridad de credenciales solamente si se cumplen estos criterios:

-   El usuario inicia sesión correctamente.
-   El usuario decidió guardar las contraseñas.

Nunca almacenes credenciales en texto sin formato con los datos de la aplicación o la configuración de itinerancia.
