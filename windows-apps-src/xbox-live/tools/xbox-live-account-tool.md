---
title: Herramienta de la cuenta de Windows Live de Xbox
description: Obtenga información sobre cómo usar la herramienta de cuenta de Xbox Live para crear cuentas de prueba para probar el título de Xbox Live habilitado rápidamente.
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, las pruebas, las cuentas de prueba
ms.localizationpriority: medium
ms.openlocfilehash: dead4e62e41b7b597ba9a578ee8f174386529937
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632830"
---
# <a name="xbox-live-account-tool"></a>Herramienta de la cuenta de Windows Live de Xbox

## <a name="what-is-xbox-live-account-tool"></a>¿Qué es la herramienta de cuenta de Xbox Live?
La herramienta de cuenta de Xbox Live es una herramienta diseñada para ayudar a los desarrolladores de título a configurar las cuentas existentes de desarrollo para probar escenarios de juego. Por ejemplo, puede usar la herramienta de cuenta de Xbox Live para cambiar el nombre de jugador de la cuenta de desarrollo, o agregar rápidamente los seguidores de 1000 a la lista de amigos de la cuenta.

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>¿Qué puedo hacer con la herramienta de cuenta de Xbox Live?
Se puede hacer lo siguiente:
  1. Ver configuración del perfil del usuario, XUID y privilegios activos
  2. Agregar una lista de seguidores a gráfico social de un usuario, ya sea desde un archivo de texto o un archivo csv de plataforma para desarrolladores de Xbox
  3. Administrar la lista de amigos del usuario: favoritos, desaparece, bloquear y desbloquear a los usuarios seguir y vea si siguen puede volver
  4. Cambiar la reputación del usuario de su desarrollo (y ver inmediatamente los valores stat reputación sin procesar)
  5. Cambiar el nombre de jugador de un usuario

## <a name="where-can-i-find-xbox-live-account-tool"></a>¿Dónde puedo encontrar herramientas de cuenta de Xbox Live?
La herramienta de cuenta de Xbox Live se puede encontrar como parte del paquete de herramientas de Xbox Live [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

## <a name="how-do-i-log-in"></a>¿Cómo iniciar?
Necesitará las credenciales del usuario que desea administrar y especifique el recinto de seguridad correcta. Asegúrese de que la cuenta de desarrollador tiene acceso al espacio aislado, en caso contrario, es posible que producirá un error en el inicio de sesión. La herramienta se diseñó con cuentas de desarrollo mediante un espacio aislado en mente.

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>¿Puedo usar una cuenta de venta directa o tiene que ser una cuenta de espacio aislada?
Sin duda, puede usar herramientas de cuenta de Xbox Live para administrar una cuenta de venta directa, pero no todas las características se admiten. Por ejemplo, no se puede cambiar la reputación de un usuario de venta directa.

## <a name="how-do-i-change-a-dev-users-gamertag"></a>¿Cómo se puede cambiar el nombre de jugador de un usuario de desarrollo?
Vaya a la pestaña "Nombre de jugador" y escriba un nombre de jugador. Nombres de jugadores solo puede contener números, letras y espacios y pueden tener solo 15 caracteres. Nombres de cuenta de desarrollo jugadores deben comenzar con un 2. Actualmente se admite solo un cambio.

## <a name="how-do-i-see-my-block-list"></a>¿Cómo se puede ver mi lista de bloques?
Vaya a la pestaña "People" y seleccione el encabezado de columna "Bloqueado" para ordenar por los usuarios que están bloqueados actualmente.

## <a name="how-do-i-follow-a-large-group-of-users"></a>¿Cómo se puede seguir un gran grupo de usuarios?
Si tiene una lista de XUIDs desea seguir, cópielos en un archivo de texto. Vaya a la pestaña "Seguir", seleccione "Importar lista" y elija el archivo. Deben rellenar el XUIDs en la vista de lista. Haga clic en "Confirmar cambios" para seguir los usuarios.

## <a name="how-do-i-block-someone"></a>Cómo bloquear una persona
Vaya a la pestaña "People" y seleccione el usuario o usuarios que desea bloquear. Presione el botón "bloque" y obtener bloquearán en lotes. Si observa un error, simplemente vuelva a intentar más tarde.

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>¿Cómo se puede cambiar la reputación de mi cuenta de desarrollo?
Vaya a la pestaña "Reputación". Seleccione los valores predeterminados que desee y pulse el botón "Confirmar cambios" para enviar la solicitud. Verá la estadística de reputación valores cambian si la solicitud es correcta.

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>¿Qué significan los valores en la pestaña de reputación?
En general reputación se calcula a partir de tres reputación subcarpetas: FairPlay (llevar a cabo varios jugadores), contenido generado por el usuario (clips de vídeo etc.) y las comunicaciones (mensajes y voz). Los valores sin procesar para cada categoría pueden oscilar entre 0 y 75, que significa mayor que reputación del usuario es mejor. La OverallStatIsBad indica si el usuario tiene reputación "Evitar Me".

## <a name="whats-the-black-area-at-the-bottom"></a>¿Qué es el área de color negro en la parte inferior?
Para mantener informado, pensamos que sería útil si los resultados de la depuración se imprimen en la interfaz de usuario. Que área le indicará lo que es hasta la herramienta y los errores de impresión se ejecuta en.

## <a name="wheres-my-gamerpic"></a>¿Dónde está mi gamerpic?
Somos conscientes de que un error que algunas cuentas de desarrollo no reciben un gamerpic generado automáticamente en tiempo de creación de cuenta.

## <a name="why-are-things-happening-so-slowly"></a>¿Por qué son las cosas que ocurre tan lenta?
Para las operaciones por lotes (por ejemplo, agregar seguidores), la herramienta realiza automáticamente los lotes para evitar que los tamaños de solicitud enorme.

## <a name="how-do-i-run-xbox-live-account-tool"></a>¿Cómo se ejecuta la herramienta de cuenta de Xbox Live?
Extraiga el SDK de Xbox Live en una carpeta y haga doble clic en el archivo Tools\XboxLiveAccountTool.exe para ejecutarlo.

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>Tengo una solicitud de característica. ¿Cómo puedo obtener mi característica incorporado?
Hable con su representante de Microsoft con las solicitudes de características y veremos qué podemos hacer.
