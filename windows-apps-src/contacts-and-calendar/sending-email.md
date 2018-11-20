---
author: normesta
description: Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.
title: Enviar correo electrónico
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contactos, correo electrónico, enviar
ms.author: normesta
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0a28809210f71bf523e3cc5f9c8da1db9fbcc90c
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284722"
---
# <a name="send-email"></a>Enviar correo electrónico

Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.

**En este artículo**

-   [Iniciar el cuadro de diálogo de redacción de correo electrónico](#launch-the-compose-email-dialog)
-   [Resumen y pasos siguientes](#summary-and-next-steps)
-   [Temas relacionados](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Iniciar el cuadro de diálogo de redacción de correo electrónico

Crea un nuevo objeto [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) y define los datos que quieras que se rellenen previamente en el cuadro de diálogo de redacción de correo electrónico. Llama a [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) para que se muestre el cuadro de diálogo.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> Los datos adjuntos que agregas a un correo electrónico mediante el uso de la clase [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) aparecerá únicamente en la aplicación de correo. Si los usuarios tienen que cualquier otro programa de correo configurado como su programa de correo predeterminada, aparecerá la ventana de la redacción sin los datos adjuntos. Este es un problema conocido.

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Este tema te ha enseñado a iniciar el cuadro de diálogo de redacción de correo electrónico. Para información sobre la selección de contactos para usarlos como destinatarios de un mensaje de correo electrónico, consulta [Seleccionar contactos](selecting-contacts.md). Consulta [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) para seleccionar un archivo y usarlo como datos adjuntos de correo electrónico.

## <a name="related-topics"></a>Temas relacionados

* [Selección de contactos](selecting-contacts.md)
* [Cómo continuar la aplicación de Windows Phone después de llamar a un selector de archivos](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 
