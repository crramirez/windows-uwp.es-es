---
description: Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.
title: Enviar correo electrónico
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contactos, correo electrónico, enviar
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e7839a26afca81913e50296ac5ed9bb9210edbf2
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997902"
---
# <a name="send-email"></a>Enviar correo electrónico

Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.

**En este artículo**

-   [Iniciar el cuadro de diálogo de redacción de correo electrónico](#launch-the-compose-email-dialog)
-   [Resumen y pasos siguientes](#summary-and-next-steps)
-   [Temas relacionados](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Iniciar el cuadro de diálogo de redacción de correo electrónico

Crea un nuevo objeto [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) y define los datos que quieras que se rellenen previamente en el cuadro de diálogo de redacción de correo electrónico. Llama a [**ShowComposeNewEmailAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) para que se muestre el cuadro de diálogo.

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
> Los datos adjuntos que agregue a un correo electrónico mediante la clase [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) solo se mostrarán en la aplicación mail. Si los usuarios tienen algún otro programa de correo configurado como programa de correo electrónico predeterminado, la ventana de redacción aparecerá sin los datos adjuntos. Este es un problema conocido.

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Este tema te ha enseñado a iniciar el cuadro de diálogo de redacción de correo electrónico. Para información sobre la selección de contactos para usarlos como destinatarios de un mensaje de correo electrónico, consulta [Seleccionar contactos](selecting-contacts.md). Consulta [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) para seleccionar un archivo y usarlo como datos adjuntos de correo electrónico.

## <a name="related-topics"></a>Temas relacionados

* [Selección de contactos](selecting-contacts.md)
 