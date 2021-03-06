---
description: En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.
title: Enviar un mensaje SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contactos, SMS, enviar
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ceaeffdb0d8b3207a95e981f16590fe8e9e12e46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154659"
---
# <a name="send-an-sms-message"></a>Enviar un mensaje SMS

En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.

Para llamar a este código, declare las capacidades **chat**, **smsSend**y **chatSystem** en el manifiesto del paquete. Se trata de [funcionalidades restringidas](../packaging/app-capability-declarations.md#special-and-restricted-capabilities) , pero se pueden usar en la aplicación. Solo necesita aprobación si tiene previsto publicar la aplicación en la tienda. Consulte [tipos de cuenta, ubicaciones y tarifas](../publish/account-types-locations-and-fees.md).

## <a name="launch-the-compose-sms-dialog"></a>Iniciar el cuadro de diálogo de redacción de SMS

Crea un nuevo objeto [**ChatMessage**](/uwp/api/windows.applicationmodel.chat.chatmessage) y define los datos que quieras que se rellenen previamente en el cuadro de diálogo de redacción de correo electrónico. Llama a [**ShowComposeSmsMessageAsync**](/uwp/api/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) para que se muestre el cuadro de diálogo.

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

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

Puede usar el siguiente código para determinar si el dispositivo que ejecuta la aplicación puede enviar mensajes SMS.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Este tema te ha enseñado a iniciar el cuadro de diálogo de redacción de SMS. Para información sobre cómo seleccionar contactos y usarlos como destinatarios de un mensaje SMS, consulta [Seleccionar contactos](selecting-contacts.md). Descarga las [muestras de aplicaciones universales de Windows](https://github.com/Microsoft/Windows-universal-samples) de GitHub para ver más ejemplos de cómo enviar y recibir mensajes SMS mediante una tarea en segundo plano.

## <a name="related-topics"></a>Temas relacionados

* [Seleccionar contactos](selecting-contacts.md)