---
description: Descubra formas de permitir que los usuarios accedan a sus contactos y citas para poder compartir contenido, correos electrónicos, información de calendario o mensajes.
title: Contactos y calendario
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, contacts, calendar, appointments, email messages
ms.localizationpriority: medium
ms.openlocfilehash: 49fe277aa18627e304d2d6f27c023a13c85dccd0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174059"
---
# <a name="contacts-my-people-and-calendar"></a>Contactos, Mis allegados y calendario


Puedes permitirle que los usuarios obtengan acceso a los contactos y las citas para que compartan contenido, correos electrónicos, información del calendario o mensajes, o cualquier funcionalidad que diseñes.

Para conocer los distintos modos en que tu aplicación puede obtener acceso a los contactos, consulta estos temas:

| Tema | Descripción |
|-------|-------------|
| [Seleccionar contactos](selecting-contacts.md) | Mediante el espacio de nombres [<strong>Windows.ApplicationModel.Contacts</strong>](/uwp/api/Windows.ApplicationModel.Contacts), tienes varias opciones para seleccionar contactos. Aquí te mostraremos cómo seleccionar un único contacto o varios contactos, y aprenderás a configurar el selector de contactos para recuperar solamente la información de contacto que necesita tu aplicación. |
| [Enviar correo electrónico](sending-email.md) | Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar. |
| [Enviar un mensaje SMS](sending-an-sms-message.md) | En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar. |
| [Administrar citas](managing-appointments.md) | Con el espacio de nombres [<strong>Windows.ApplicationModel.Appointments</strong>](/uwp/api/Windows.ApplicationModel.Appointments), puedes crear y administrar citas en la aplicación de calendario de un usuario. Aquí te mostraremos cómo crear una cita, agregarla a la aplicación de calendario, reemplazarla en dicha aplicación y quitarla de ella. También te enseñaremos cómo mostrar un intervalo de tiempo para una aplicación de calendario y crear un objeto de repetición de citas. |
| [Conectar la aplicación a acciones en una tarjeta de contacto](integrating-with-contacts.md) | Muestra cómo hacer que tu aplicación aparezca junto a acciones en una tarjeta de contacto o una minitarjeta de contacto. Los usuarios pueden elegir tu aplicación para realizar una acción, como abrir una página de perfil, realizar una llamada o enviar un mensaje. |
| [Agregar compatibilidad con Mis allegados a una aplicación](my-people-support.md) | Muestra cómo agregar compatibilidad con Mis allegados a una aplicación y cómo anclar y desanclar contactos en la barra de tareas. |
| [Uso compartido de Mis allegados](my-people-sharing.md) | Muestra cómo agregar compatibilidad con el uso compartido de Mis allegados, que permite que los usuarios compartan contenido con sus contactos anclados mediante la acción de arrastrar archivos desde el Explorador de archivos hasta una chincheta de Mis allegados. |
| [Notificaciones de Mis allegados](my-people-notifications.md) | Muestra cómo crear y usar las notificaciones de Mis allegados, un nuevo tipo de notificación del sistema que se envía desde un contacto anclado. |

 

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de API de citas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Appointments)
* [Ejemplo de API del administrador de contactos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Contact%20manager%20API%20sample)
* [Ejemplo de una aplicación de selector de contactos](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/ContactPicker)
* [Ejemplo de control de las acciones de contacto](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/Handling%20Contact%20Actions)
* [Ejemplo de integración de la tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)