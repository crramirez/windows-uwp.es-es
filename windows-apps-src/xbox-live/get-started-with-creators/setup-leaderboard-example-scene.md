---
title: Uso de la escena de ejemplo de marcador en Unity
description: Muestra los pasos para configurar correctamente la escena de marcador de Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity, marcadores
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589750"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>La escena de ejemplo de marcador en Unity

[El complemento de Unity](https://github.com/Microsoft/xbox-live-unity-plugin) hospede un número de segundo plano de ejemplo para mostrar los servicios de Xbox Live disponibles para los desarrolladores de Unity. Una escena de este tipo es la escena de ejemplo de marcador. En la siguiente captura de pantalla, verá que la escena de ejemplo de marcador simplemente muestra un panel de inicio de sesión del usuario de Xbox Live y un panel que contiene el marcador. Si se alcanza en este momento play sin agregar a esta escena que encontrará en el panel de inicio de sesión se rellena con datos de usuario ficticio, pero la tabla de clasificación no carga ninguna información. Para obtener esta escena de ejemplo para cargar una tabla de clasificación real tendrá que agregar algunas cosas.

![Captura de pantalla de escena de marcador](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>Requisitos previos

Marcadores en Xbox Live se basan en las estadísticas en el servicio de estadísticas de Xbox Live. Antes de que puede rellenar un marcador con datos que deberá tener algunas estadísticas asociadas con sus cuentas de prueba. Si aún no ha agregado las estadísticas para el título de la puede aprender a hacerlo leyendo [agregar estadísticas de Reproductor y marcadores en el proyecto de Unity](add-stats-and-leaderboards-in-unity.md). Después de realizar las acciones en la sección de estadísticas de ese artículo, vuelva a este punto para mostrar un estado de la escena de marcador de ejemplo.

## <a name="the-leaderboard-inspector"></a>El inspector de marcador

El recurso prefabricado de marcador tiene una serie de valores que se pueden cambiar en la sección de inspector de componente de script de la tabla de líderes, como la interfaz de usuario *tema*, el Reproductor asociado con el marcador, la configuración del controlador de xbox, y otras opciones de marcador. Puede ver que la configuración de la tabla de clasificación se divide en secciones diferentes del inspector de más abajo.

![Captura de pantalla de inspector de marcador](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>Configuración de tema y muestre

En esta sección tiene una opción llamada *tema*. Esta es una simple lista desplegable que le permite utilizar un tema oscuro o claro para el marcador prefabricado. Esto cambiará el en segundo plano, fuentes y colores de la imagen de la prefabricado. El efecto se puede identificar fácilmente al reproducir la escena en el Reproductor de unity.

![Tema claro](../images/unity/leaderboard_light_theme.JPG) ![Tema oscuro](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>Configuración de estadísticas

En esta sección permite determinar qué tipo de datos se recuperarán cuando se rellena la tabla de líderes con las filas.

- **Número de Reproductor** -determina qué Reproductor está asociado con el marcador.
- **Stat** -la estadística usada para rellenar los datos de marcador. Esto es necesario para el recurso prefabricado de marcador cargar datos.
- **Tipo de marcador** : este menú desplegable aplica un filtro a los datos de marcador cargados. Si *Global* se selecciona el marcador estará sin filtrar y se mostrará cada jugador con un valor para la estadística seleccionada. Si *amigos* se selecciona el marcador se filtrarán a sólo mostrar jugadores en la tabla de líderes que también se encuentran en la lista de amigos. Si *favorito* se selecciona el marcador se filtrarán a sólo mostrar jugadores en la tabla de líderes que también se encuentran en la lista de favoritos.
- **Número de entradas** -un control deslizante con un intervalo de 1 a 100 que dicta cuántas filas de la tabla de líderes se devolverá a la vez. El número establecido determinará el número de filas de tabla de clasificación que se muestran por página.

### <a name="controller-configuration"></a>Configuración del controlador

El recurso prefabricado de marcador permite a los desarrolladores configurar fácilmente el uso del controlador de Xbox. La sección de configuración del controlador de la prefabricado permite habilitar y elija los botones que controlan el recurso prefabricado de marcador.

- **Habilitar el controlador de entrada** -un botón de alternancia del botón de radio simple. Si está activada, a continuación, puede usar un mando de Xbox para interactuar con el recurso prefabricado. Se requiere para la compatibilidad del controlador.
- **Número de joystick** -designa el controlador que puede interactuar con este marcador prefabricado.
- **Botón de controlador de página siguiente** -menú desplegable que controla qué botón carga la página siguiente de filas de tabla de clasificación.
- **Botón de controlador de página anterior** -menú desplegable que controla qué botón carga la página anterior de las filas de tabla de clasificación.
- **Botón de controlador de vista siguiente** -alterna entre el tipo de vista **todas** y **más CERCANO ME**.
- **Botón anterior de controlador de vista** -alterna entre el tipo de vista **todas** y **más CERCANO ME**.
- **Eje de entrada de desplazamiento vertical** : cadena que designa el eje de qué controlador está asociado con el desplazamiento.
- **Desplácese hacia el multiplicador de velocidad** -determina la velocidad de desplazamiento de controlador.

> [!NOTE]
> Cambiar los botones que se utiliza para cambiar la página o vista de la tabla de líderes también cambiará la imagen asociada con el botón utilizado para cada función del marcador. No es necesario modificar la sección de referencias de la interfaz de usuario manualmente para que coincida con la imagen para el botón.

### <a name="ui-references"></a>Referencias de la interfaz de usuario

Esta sección controla las imágenes y la composición general de la prefabricado de marcador. En esta sección no deben cambiarse para utilizar correctamente el recurso prefabricado de marcador. Sin embargo es posible que deba realizar ajustes en esta sección para personalizar la apariencia de la prefabricado para sus propios fines.

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>Rellenar la tabla de líderes de Reproductor de Unity (con datos falsos)

Para rellenar la tabla de líderes de Reproductor de Unity con datos deberá agregar una estadística para el recurso prefabricado de marcador. Visualización de la tabla de líderes prefabricado del inspector de revelará que puede tardar un objeto de tipo `Stat Base` como un parámetro público en su secuencia de comandos. Puede arrastrar cualquiera de los `State Base` escriba prefabricados `IntegerStat`, `DoubleStat`, o `StringStat` desde la carpeta prefabricados del complemento de Unity de Xbox Live y lo coloca en esta ubicación en la tabla de líderes prefabricado.

![Arrastre y coloque prefabricado Stat](../images/unity/stat-to-leaderbaord-drag.gif)

Ahora se reproducirá la escena de Unity y encontrará que el marcador se rellena con datos falsos como a continuación.

![Captura de pantalla de marcador de datos falsos rellena](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>Rellenar un Visual Studio compila el proyecto con datos reales

Con el fin de rellenar un marcador con datos reales para el título, deberá crear su juego para ejecutar localmente en el equipo. Necesitará una compilación local porque el editor de Unity no tiene acceso a Xbox Live. Además de crear el proyecto se ejecute localmente, tendrá que configurar el estado en el marcador para una estadística que se ha inicializado y tiene los valores para el título. Con el fin de asociar un stat a su marcador deberá modificar el identificador y el nombre para mostrar del objeto stat en el prefabricado de marcador. Necesitará el identificador que coincida con el de un estado configurado en [centro de partners](https://partner.microsoft.com/dashboard). Después de hacer esto, compile el proyecto como se describe en el [crear sección de la configuración de Xbox Live en el artículo de Unity](configure-xbox-live-in-unity.md#build-and-test-the-project). Ejecutar este proyecto como un x64 compilación destinadas a la máquina Local debería poder iniciar sesión con un nombre de jugador real y rellenar el marcador con datos reales.