---
author: Xansky
description: "Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar."
title: "Enviar correo electrónico"
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, email, send
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: b4b5b029c321256028993e283395a91bd0ed3d7c

---

# Enviar correo electrónico

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.

**En este artículo**

-   [Iniciar el cuadro de diálogo de redacción de correo electrónico](#launch-the-compose-email-dialog)
-   [Resumen y pasos siguientes](#summary-and-next-steps)
-   [Temas relacionados](#related-topics)

## Iniciar el cuadro de diálogo de redacción de correo electrónico

Crea un nuevo objeto [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) y define los datos que quieras que se rellenen previamente en el cuadro de diálogo de redacción de correo electrónico. Llama a [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) para que se muestre el cuadro de diálogo.

``` cs
private async void ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Email.EmailAttachment(
            attachmentFile.Name,
            stream);

        emailMessage.Attachments.Add(attachment);
    }

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
        
}
```

## Resumen y pasos siguientes

Este tema te ha enseñado a iniciar el cuadro de diálogo de redacción de correo electrónico. Para información sobre la selección de contactos para usarlos como destinatarios de un mensaje de correo electrónico, consulta [Seleccionar contactos](selecting-contacts.md). Consulta [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) para seleccionar un archivo y usarlo como datos adjuntos de correo electrónico.

## Temas relacionados

* [Selección de contactos](selecting-contacts.md)
* [Cómo continuar la aplicación de Windows Phone después de llamar a un selector de archivos](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 







<!--HONumber=Jun16_HO3-->


