---
description: Cómo usar la información de contactos y calendario en la aplicación para UWP.
title: Contactos y calendario
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, contactos, calendario, citas, mensajes de correo
ms.localizationpriority: medium
ms.openlocfilehash: 239dbaa7799d9991a63223d1cd8706d34445a16b
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048242"
---
# <a name="contacts-my-people-and-calendar"></a>Contactos, Mis allegados y calendario


Puedes permitirle que los usuarios obtengan acceso a los contactos y las citas para que compartan contenido, correos electrónicos, información del calendario o mensajes, o cualquier funcionalidad que diseñes.

Para conocer los distintos modos en que tu aplicación puede obtener acceso a los contactos, consulta estos temas:

| Tema | Descripción |
|-------|-------------|
| [Seleccionar contactos](selecting-contacts.md) | Mediante el espacio de nombres [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002), tienes varias opciones para seleccionar contactos. Aquí te mostraremos cómo seleccionar un único contacto o varios contactos, y aprenderás a configurar el selector de contactos para recuperar solamente la información de contacto que necesita tu aplicación. |
| [Enviar correo electrónico](sending-email.md) | Se muestra cómo iniciar el cuadro de diálogo de redacción de correo electrónico para que el usuario pueda enviar un mensaje de correo electrónico. Puedes rellenar previamente los campos del correo electrónico con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario pulse el botón de enviar. |
| [Enviar un mensaje SMS](sending-an-sms-message.md) | En este tema se muestra cómo iniciar el cuadro de diálogo de redacción de mensajes SMS para que el usuario pueda enviar un mensaje SMS. Puedes rellenar previamente los campos del SMS con datos antes de mostrar el diálogo. El mensaje no se enviará hasta que el usuario presiona el botón de enviar. |
| [Administrar citas](managing-appointments.md) | Con el espacio de nombres [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359), puedes crear y administrar citas en la aplicación de calendario de un usuario. Aquí te mostraremos cómo crear una cita, agregarla a la aplicación de calendario, reemplazarla en dicha aplicación y quitarla de ella. También te enseñaremos cómo mostrar un intervalo de tiempo para una aplicación de calendario y crear un objeto de repetición de citas. |
| [Conectar la aplicación a acciones en una tarjeta de contacto](integrating-with-contacts.md) | Muestra cómo hacer que tu aplicación aparezca junto a acciones en una tarjeta de contacto o una minitarjeta de contacto. Los usuarios pueden elegir tu aplicación para realizar una acción, como abrir una página de perfil, realizar una llamada o enviar un mensaje. |
| [Agregar compatibilidad con Mis allegados a una aplicación](my-people-support.md) | Muestra cómo agregar compatibilidad con Mis allegados a una aplicación y cómo anclar y desanclar contactos en la barra de tareas. |
| [Uso compartido de Mis allegados](my-people-sharing.md) | Muestra cómo agregar compatibilidad para el uso compartido de Mis allegados, lo que permite a los usuarios compartir contenido con sus contactos anclados arrastrando archivos desde el Explorador de archivos a una ancla Mis allegados. |
| [Notificaciones de Mis allegados](my-people-notifications.md) | Muestra cómo crear y usar las notificaciones de Mis allegados, un nuevo tipo de notificación del sistema que se envía desde un contactos anclado. |

 

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de API de citas](https://go.microsoft.com/fwlink/p/?linkid=309836)
* [Ejemplo de API del administrador de contactos](https://go.microsoft.com/fwlink/p/?LinkID=310079)
* [Muestra de una aplicación de selector de contactos](https://go.microsoft.com/fwlink/p/?linkid=231575)
* [Ejemplo de control de las acciones de contacto](https://go.microsoft.com/fwlink/p/?LinkID=320151)
* [Ejemplo de integración de la tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
