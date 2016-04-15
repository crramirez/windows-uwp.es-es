---
description: En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.
title: Enviar un mensaje SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contactos, SMS, enviar
---

# Enviar un mensaje SMS

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.

## Iniciar el cuadro de diálogo de redacción de SMS

Crea un nuevo objeto [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/Dn642160) y define los datos que quieras que se rellenen previamente en el cuadro de diálogo de redacción de correo electrónico. Llama a [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/Dn642160manager-showcomposesmsmessageasync) para que se muestre el cuadro de diálogo.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile, 
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault&lt;Windows.ApplicationModel.Contacts.ContactPhone&gt;();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

## Resumen y pasos siguientes

Este tema te ha enseñado a iniciar el cuadro de diálogo de redacción de SMS. Para información sobre cómo seleccionar contactos y usarlos como destinatarios de un mensaje SMS, consulta [Seleccionar contactos](selecting-contacts.md). Descarga las [muestras de aplicaciones universales de Windows](http://go.microsoft.com/fwlink/p/?linkid=619979) de GitHub para ver más ejemplos de cómo enviar y recibir mensajes SMS mediante una tarea en segundo plano.

## Temas relacionados

* [Seleccionar contactos](selecting-contacts.md)




<!--HONumber=Mar16_HO1-->


