---
author: JnHs
Description: Learn how to create known user groups to use for package flighting and more.
title: Crear grupos de usuarios conocidos
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, grupo de destino, clientes, grupo piloto, grupos de usuarios, usuarios conocidos
ms.localizationpriority: high
ms.openlocfilehash: 06922ba9cde98f4bdf678dc281d261dda3bce2b0
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-known-user-groups"></a>Crear grupos de usuarios conocidos

Los grupos de usuarios conocidos te permiten agregar personas concretas a un grupo, usando la dirección de correo electrónico asociada a su cuenta de Microsoft. Estos grupos de usuarios conocidos se usan por lo general para distribuir paquetes específicos a un grupo seleccionado de personas con [distribuciones de paquetes piloto](package-flights.md), o para la distribución de un envío a una [audiencia privada](choose-visibility-options.md#audience). También se pueden utilizar para campañas de participación, como enviar [notificaciones dirigidas](send-push-notifications-to-your-apps-customers.md) u [ofertas dirigidas](use-targeted-offers-to-maximize-engagement-and-conversions.md) a un grupo de clientes específicos.

Para que se contabilice como miembro del grupo, cada persona debe autenticarse en la Tienda con la cuenta de Microsoft asociada la dirección de correo electrónico que indiques. Para descargar la aplicación con distribución de paquetes piloto, los miembros del grupo deben utilizar una versión de Windows 10 compatible con los paquetes piloto (Windows.Desktop, compilación 10586 o posterior, Windows.Mobile, compilación 10586.63 o Xbox One). Con envíos a audiencia privada, los miembros del grupo deben usar Windows 10, versión 1607 o posterior (incluyendo Xbox One).

## <a name="to-create-a-known-user-group"></a>Para crear un grupo de usuarios conocido

1. En el panel de información del Centro de desarrollo de Windows, expande **Interactuar** en el panel de navegación izquierdo y luego selecciona **Grupos de clientes**. 
2. En la sección **Mis grupos de clientes**, selecciona **Crear nuevo grupo**.
3. En la página siguiente, escribe un nombre para el grupo en el cuadro **Nombre de grupo**.
4. Asegúrate de que esté seleccionado el botón de radio **Grupo de usuarios conocido**.
5. Escribe las direcciones de correo electrónico de las personas que te gustaría agregar al grupo. Debes incluir al menos un correo electrónico, con un máximo de 10000. Puedes escribir las direcciones de correo electrónico directamente en el campo (separadas por espacios, comas, signos punto y coma o saltos de línea), o puedes hacer clic en el vínculo **Importar .csv** para crear el grupo piloto a partir de una lista de direcciones de correo electrónico en un archivo .csv.
6. Selecciona **Guardar**.

El grupo ahora estará disponible para su uso.

También puedes crear un grupo de usuarios conocido seleccionando **Crear un grupo piloto** en la página de creación del [paquete piloto](package-flights.md). Ten en cuenta que tendrás que volver a escribir toda la información que ya has proporcionado en la página de creación del paquete piloto si haces esto.

> [!IMPORTANT]
> Al utilizar grupos de usuarios conocidos con la distribución de paquetes piloto, asegúrate de obtener el permiso necesario de las personas que agregas a tu grupo y de que estas comprendan que obtendrán paquetes diferentes al envío de versión final. 

## <a name="to-edit-a-known-user-group"></a>Para editar un grupo de usuarios conocido

No puedes quitar un grupo de usuarios conocido del panel de información (ni cambiar su nombre) después de haberlo creado, pero puedes editar su pertenencia en cualquier momento.

Para revisar y editar los grupos de usuarios conocidos, expande el menú **Interactuar** en el menú de navegación izquierdo y selecciona **Grupos de clientes**. En **Mis grupos de clientes**, selecciona el nombre del grupo que quieras editar. También puedes editar un grupo de usuarios conocido en la página de creación del paquete piloto seleccionando **Ver y administrar los grupos existentes** al crear un nuevo paquete piloto o seleccionando el nombre del grupo en la página de información general de un paquete piloto. 

Después de seleccionar el grupo que deseas editar, puedes agregar o quitar las direcciones de correo electrónico directamente en el campo.

Para realizar cambios de mayor magnitud, selecciona **Exportar .csv** para guardar la información de pertenencia a un grupo en un archivo .csv. Realiza los cambios que quieras en este archivo y haz clic en **Importar .csv** para usar la nueva versión para actualizar la pertenencia al grupo.

Ten en cuenta que los cambios de pertenencia pueden tardar hasta 30minutos en implementarse. No necesitas publicar un nuevo envío para que nuevos miembros del grupo nueva puedan acceder a tu envío final a través de paquetes piloto o audiencia privada; tendrán acceso tan pronto como se implementen los cambios. 






