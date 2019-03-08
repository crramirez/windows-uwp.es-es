---
title: Configuración de presencia enriquecida en el centro de partners
description: Obtenga información sobre cómo configurar cadenas de presencia enriquecida en el centro de partners
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, juegos, uwp, windows 10, Xbox uno, cadenas de presencia enriquecida, centro de partners
ms.openlocfilehash: 5c2add99c6fc6ad1c7f085eb35dc4fdba9e0688d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624130"
---
# <a name="configure-rich-presence-in-partner-center"></a>Configurar presencia enriquecida en el centro de partners

Las cadenas de presencia enriquecida Mostrar actividad de un usuario en el juego. Se muestran en el nombre de jugador de un jugador en el **amigos & clubes** lista, así como en su perfil de usuario de Xbox Live. Las cadenas de presencia enriquecida configuradas se anexan para el nombre del juego que se reproduce. Si crea un juego llamado BubblePop y configurar la cadena de presencia enriquecida "Popping burbujas con amigos", la cadena configurada generaría "BubblePop - Popping burbujas con amigos" como estado. A continuación puede ver cómo aparecerá una cadena de presencia enriquecida en contexto.

En los siguientes usuarios de captura de pantalla de Xbox Live **Roar última** y **Lucha Uno** están jugando a juegos mediante cadenas de presencia enriquecida.

![Ejemplo de la lista de amigos](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

En la siguiente captura de pantalla puede ver **de Lucha Uno** completa cadena presencia enriquecido en su perfil.

![Ejemplo de perfil](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> Las cadenas de presencia enriquecida no están disponibles para los títulos del programa de creadores de Xbox Live y, por tanto, no son configurables para estos títulos. El contenido de este artículo es para ID@Xbox y títulos de asociado administradas.

## <a name="requirements"></a>Requisitos

Antes de configurar las cadenas de presencia enriquecida y el título deben cumplir los siguientes criterios:

- Debe tener una cuenta de desarrollo de Windows.
- Su cuenta de desarrollo debe estar registrada en el ID@Xbox programa o como una cuenta de desarrollador de socio administrado.
- El título debe estar registrado en el centro de partners y ser Xbox Live habilitado.

Para poder usar las cadenas de presencia enriquecida debe configurar en el centro de partners.

## <a name="rich-presence-configuration-page"></a>Página de configuración de presencia enriquecida

Las cadenas de presencia enriquecida están configuradas como parte del servicio de Xbox Live para su título en [centro de partners](https://partner.microsoft.com/dashboard).

Vaya a la página de configuración de presencia enriquecida con las instrucciones siguientes:

1. Vaya a [centro de partners](https://partner.microsoft.com/dashboard) en developer.microsoft.com.
2. Inicie sesión con su cuenta de desarrollador de Windows registrada si se solicita el inicio de sesión.
3. Elija el título de Xbox Live habilitada o una aplicación de la **Introducción** página. No seleccione un título de programa de creadores ya no se habilitará para la configuración de la cadena de presencia enriquecida.
4. Haga clic en el **servicios** lista desplegable y seleccione Xbox Live.
5. Desplácese hacia abajo hasta la **presencia enriquecida** vincular y haga clic en él.

La página de presencia enriquecida muestra una breve descripción del servicio, un botón para crear una nueva cadena de presencia enriquecida y una lista de búsqueda de las cadenas configuradas previamente. Desde esta página puede configurar nuevas cadenas, así como editar y revisar las cadenas configuradas.

![Ejemplo de cadena de presencia enriquecida Config Page](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> Cadenas como "Playing neto ejecutor - deathmatch multijugador en la Base de la luna con 10 derribos." no estará disponible para los desarrolladores a partir de la actualización de 2017 de plataforma de datos. Plataforma de datos 2013 *Variables* no están disponibles en la plataforma de datos de 2017. En este caso, la variable es que el número de mata "10". La cadena equivalente después de la actualización de datos plataforma 2017 sería "Playing neto ejecutor - deathmatch multijugador en la Base de la luna." "Reproducción ejecutor Net - en los menús" sigue siendo una cadena válida de presencia enriquecida.

## <a name="create-a-new-rich-presence-string"></a>Crear una nueva cadena de presencia enriquecida

Para crear una cadena de presencia enriquecida, haga clic en el botón rotulado **cadena nueva presencia enriquecida**. Aparecerá la interfaz de usuario para rellenar el **presencia detalles** que incluyen el **identificador único de presencia enriquecida** , así como la **Mostrar cadena** para la nueva cadena de presencia enriquecida.

![nueva cadena de presencia completa interfaz de usuario](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**Identificador único de presencia enriquecida** -el identificador único de presencia enriquecida es una cadena que se usa para identificar la cadena de presencia enriquecida. Esta cadena se usará para establecer el estado de los jugadores del juego y está asociada con la cadena determinada que le gustaría mostrar. El identificador puede ser un máximo de 50 caracteres.

**Mostrar cadena** -mostrar la cadena es la que deseará mostrar anexado al estado de algunos jugador reproduciendo su juego. Esto es donde rellenará en la cadena de presencia enriquecida que desea mostrar al generar el interés en su juego. La pantalla puede tener un máximo de 100 caracteres, pero habrá instancias donde se mostrará solo los primeros 40 caracteres.

Para crear la nueva cadena de presencia enriquecida, rellene los campos y presione la **guardar** botón.
Al hacer clic en Guardar, que volverá a la página de configuración de presencia enriquecida donde podrá ver la nueva cadena de presencia enriquecida agregada a la lista de cadenas configuradas.

## <a name="review-edit-and-delete-strings"></a>Revisar, editar y eliminar cadenas

Aquí puede ver una página de configuración de presencia enriquecida con algunas cadenas configuradas.
![Página de presencia enriquecida configurado de ejemplo](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

Para revisar las cadenas creadas anteriormente, simplemente hay que examinar la lista en la página de configuración de presencia enriquecida. Allí puede ver tanto el identificador único de presencia enriquecida como cadena de presentación juntos. Esto será útil cuando necesite utilizar el identificador único de presencia enriquecida en el código de su título para especificar una cadena de presencia enriquecida.

Para editar una presencia enriquecida cadena basta con hacer clic en el **identificador único de presencia enriquecida** vínculo para la cadena que desea editar. Se le llevará a la misma interfaz de usuario utilizada para crear una nueva cadena de presencia enriquecida con la configuración actual de cadena rellenada para su edición. Después de realizar modificaciones haga clic en el **guardar** botón para actualizar la cadena configurada con los cambios.

Haga clic en una cadena configurado de presencia enriquecida de eliminar el **eliminar** vínculo en la página de configuración de presencia enriquecida en la misma fila que la cadena de presencia enriquecida que quiere eliminar. Se le pedirá que confirme la eliminación.

## <a name="further-reading"></a>Lecturas adicionales

Para obtener más información conceptual detallada sobre las características de la cadena de presencia enriquecida y cómo implementarlos, lea el [documentación presencia enriquecida](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview).