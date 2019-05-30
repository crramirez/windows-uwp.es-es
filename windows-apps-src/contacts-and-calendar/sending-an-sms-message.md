---
description: En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.
title: Enviar un mensaje SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contactos, SMS, enviar
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 01f6bd595de369afae8ac091a70c857bbd198519
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360340"
---
# <a name="send-an-sms-message"></a>Enviar un mensaje SMS

En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar.

Para llamar a este código, declare el **chat**, **smsSend**, y **chatSystem** funcionalidades en el manifiesto del paquete. Estos son [capacidades restringidas](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities) pero puede usar en la aplicación. Necesita aprobación solo si tiene la intención de publicar la aplicación en el Store. Consulte [cuenta tipos, ubicaciones y precios](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees).

## <a name="launch-the-compose-sms-dialog"></a>Iniciar el cuadro de diálogo de redacción de SMS

Crea un nuevo objeto [**ChatMessage**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat.chatmessage) y define los datos que quieras que se rellenen previamente en el cuadro de diálogo de redacción de correo electrónico. Llama a [**ShowComposeSmsMessageAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) para que se muestre el cuadro de diálogo.

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

Puede usar el código siguiente para determinar si el dispositivo que se está ejecutando la aplicación es capaz de enviar mensajes SMS.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Este tema te ha enseñado a iniciar el cuadro de diálogo de redacción de SMS. Para información sobre cómo seleccionar contactos y usarlos como destinatarios de un mensaje SMS, consulta [Seleccionar contactos](selecting-contacts.md). Descarga las [muestras de aplicaciones universales de Windows](https://go.microsoft.com/fwlink/p/?linkid=619979) de GitHub para ver más ejemplos de cómo enviar y recibir mensajes SMS mediante una tarea en segundo plano.

## <a name="related-topics"></a>Temas relacionados

* [Seleccionar contactos](selecting-contacts.md)
