---
author: JnHs
Description: "Descubre cómo crear grupos de usuarios conocidos que se usarán para la distribución de paquetes piloto y mucho más."
title: Crear grupos de usuarios conocidos
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, segmento, segmentos, grupo dirigido, clientes
ms.openlocfilehash: fc3986520e55ae0c636eb2db731df065463002b5
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2017
---
# <a name="create-known-user-groups"></a>Crear grupos de usuarios conocidos

Los grupos de usuarios conocidos te permiten agregar personas concretas a un grupo, con la dirección de correo electrónico asociada a su cuenta de Microsoft. Estos grupos de usuarios conocidos suelen usarse con [paquetes piloto](package-flights.md) para distribuir paquetes específicos para un determinado grupo de personas. También se pueden utilizar para enviar [notificaciones dirigidas](send-push-notifications-to-your-apps-customers.md) u [ofertas dirigidas](use-targeted-offers-to-maximize-engagement-and-conversions.md) a un grupo de determinados clientes como parte de tus campañas de participación.

Para que se contabilice como miembro del grupo, cada persona debe autenticarse en la Tienda con la cuenta de Microsoft asociada la dirección de correo electrónico que indiques. En el caso de paquetes piloto, el usuario debe utilizar [un dispositivo Windows10 que admita la distribución de paquetes piloto](package-flights.md) para descargar la aplicación.


## <a name="to-create-a-known-user-group"></a>Para crear un grupo de usuarios conocido

1.  En el panel de información del Centro de desarrollo de Windows, expande **Interactuar** en el panel de navegación izquierdo y luego selecciona **Grupos de clientes**. 
2.  En la sección **Mis grupos de clientes**, selecciona **Crear nuevo grupo**.
3.  En la página siguiente, selecciona el botón de opción **Grupo de usuario conocido**.
4.  En la casilla **Nombre de grupo**, escribe un nombre para el grupo de usuario conocido.
5.  Escribe las direcciones de correo electrónico de las personas que te gustaría agregar al grupo. Debes incluir al menos un correo electrónico, con un máximo de 10000. Puedes escribir las direcciones de correo electrónico directamente en el campo (separadas por espacios, comas, signos punto y coma o saltos de línea), o puedes hacer clic en el vínculo **Importar .csv** para crear el grupo piloto a partir de una lista de direcciones de correo electrónico en un archivo .csv.
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

Ten en cuenta que los cambios de pertenencia pueden tardar hasta 30minutos en implementarse. Si agregas personas a un grupo de usuarios conocido después de haber publicado un paquete piloto para dicho grupo, los paquetes se entregarán a esas nuevas personas de forma automática; no será necesario que crees y publiques un nuevo envío para ese paquete piloto. 






