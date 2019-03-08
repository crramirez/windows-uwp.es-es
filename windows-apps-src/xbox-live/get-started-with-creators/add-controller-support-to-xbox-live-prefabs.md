---
title: Agregar compatibilidad con controladores para Xbox Live prefabricados
description: Agregar compatibilidad de controlador para usar el complemento Xbox Live Unity los prefabricados de Xbox Live
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity, la compatibilidad del controlador
ms.localizationpriority: medium
ms.openlocfilehash: 4d32ec62b8beec10256ed9a695866c2fd9bdd03e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622880"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>Agregar compatibilidad con controladores para Xbox Live prefabricados

> [!IMPORTANT]
> El complemento de Xbox Live Unity no es compatible con logros o juego multijugador en línea y solo se recomienda para [programa de creadores de Xbox Live](../developer-program-overview.md) miembros.

Todos los prefabricados de complemento de Xbox Live Unity admiten la entrada de controlador especificando en el inspector.

Por ejemplo, supongamos que tiene un objeto de juego llamado `UserProfile1` que se basa en el `UserProfile` prefabricado. Si desea asociar este objeto de juego al jugador 1 y pídale que inicie sesión con la `A` situado en su controlador de Xbox, basta con escribir `joystick 1 button 0` en el `Input Controller Button` campo en el inspector.

  ![Compatibilidad del controlador en el prefabricado UserProfile](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>Controlador prefabricado todos los campos de entrada
### <a name="userprofile-prefab"></a>UserProfile prefabricado
- **Botón del dispositivo de entrada:** Agrega e inicia sesión en un usuario de Xbox Live.

### <a name="social-prefab"></a>Prefabricado social
- **Botón de Alternar filtro controlador:** Alterna el filtro para mostrar los amigos "En línea" o amigos 'All'.

### <a name="leaderboard-prefab"></a>Recurso prefabricado de marcador
- **Primer botón de controlador:** Toma el Reproductor a la primera página de entradas de marcador.
- **Último botón de controlador:** Toma el Reproductor a la última página de entradas de marcador.
- **Botón de controlador siguiente:** Toma el Reproductor a la página siguiente de entradas de marcador.
- **Botón de controlador anterior:** Toma el Reproductor a la página anterior de las entradas de marcador.
- **Botón de controlador de actualización:** Actualiza la vista de tabla de clasificación.


### <a name="game-save-ui-prefab"></a>Recurso prefabricado de interfaz de usuario de juego guardar
- **Generar nuevo botón de controlador:** Genera un nuevo entero guardar datos.
- **Guardar datos controlador botón:** Guarda los datos actuales en el almacenamiento conectado.
- **Botón de controlador de datos de carga:** Carga los datos que actualmente se guarda en el almacenamiento conectado.
- **Obtenga información controlador botón:** Recupera información sobre los contenedores guardadas en el almacenamiento conectado.
- **Elimine el botón controlador de contenedor:** Elimina el contenedor guardado desde el almacenamiento conectado

## <a name="xbox-controller-button-mappings"></a>Asignaciones de botón del mando de Xbox

Para las asignaciones de botón de controlador Xbox en Unity, pruebe esta [página Wiki de controlador de Unity](https://wiki.unity3d.com/index.php?title=Xbox360Controller).
