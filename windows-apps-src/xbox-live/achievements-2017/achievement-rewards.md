---
title: Premios por logros
description: Describe cómo puede configurar una mención especial para entregar las recompensas.
ms.assetid: b6fc5bdb-ba7b-4687-985e-894182f066da
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, logros, recompensas
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbcb06a2aba927301cf982d09fdb55192ec84c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617700"
---
# <a name="achievement-rewards"></a>Premios por logros

El siguiente diagrama ilustra cómo un desarrollador puede administrar el ciclo de vida de un título. El nuevo sistema de logro está diseñado para elevar nuestro mecánico familiarizado con mucha más flexibilidad, en cómo logros se desbloqueen, cómo y cuándo se agregan y, en qué ventajas, ofrecen a los usuarios, que ofrece al programador para el título de la serie como un servicio Agregar valor y mantener la participación de los usuarios con el tiempo.

##### <a name="figure-1---how-a-title-might-drive-user-behavior"></a>Figura 1.   Cómo puede impulsar a un título de comportamiento del usuario. #####
![rewarding_achievements](../images/omega/achievements_overview_01_drive_behavior.png)

### <a name="flexible-options-for-rewarding-achievement"></a>Opciones flexibles para gratificante logro ###
Con la Xbox One, hemos ampliado el sistema de logro para admitir opciones más flexibles de recompensa. Puntuación permanece como un premio valioso que realiza un seguimiento de una puntuación de juegos única y común para el usuario en el ecosistema Xbox Live, pero ahora, el desarrollador o el publicador, puede usar los logros como un mecanismo de entrega para una gama mucho más amplia de recompensas, tanto en el título de la y fuera de su título.

Un logro puede configurarse con varios premios, hasta una recompensa de cada tipo de recompensa. También se puede configurar un logro con ningún premio explícita; En tal caso, el icono de la realización actúa como un distintivo visual para el jugador que adquirió la consecución.

Xbox Live admite los siguientes tipos de recompensas:

* Puntuación
* Arte
* Recompensas en la aplicación

#### <a name="gamerscore"></a>Puntuación ####
Nos comprometemos a mantener la integridad del valor de puntuación que se ha compilado con nuestros usuarios Xbox Live. Hay solo una puntuación por usuario. Cualquier puntuación que un usuario se gana en las plataformas existentes como Xbox 360, Xbox One, Xbox Live o Windows 10 contará para un único puntuación para dicho usuario.

Cuando un usuario desbloquea una mención especial de puntuación, Xbox Live aumenta automáticamente la puntuación del usuario la cantidad configurada.

Hay restricciones en el que los títulos pueden ofrecer puntuación como un premio en sus logros. Consulte los documentos de directiva en https://developer.xboxlive.com/ para obtener la información más reciente.

#### <a name="art"></a>Arte ####
¿Tiene algunas imágenes concepto interesantes que los diseñadores dibujados al principio de la fase de comienzo de su título? ¿Tiene imágenes bonitas y de alta resolución que pueden decorar su aplicación concentrador cuando visitan los jugadores? ¿Quizás la aplicación admite varias máscaras? Con la recompensa de material gráfico, puede potenciar tropicales, atractivas experiencias en sus títulos y versiones posteriores que deben obtener los jugadores.

Alta resolución concepto imágenes, dibujos principios de diseño, había creado especialmente activos gráficos y otros recursos de imágenes digitales le pueden ofrecer una recompensa a los usuarios desbloquear un logro. Estos activos pueden mostrarse en el panel Xbox experiencia y pueden mostrarse en las experiencias de complementaria consultando el servicio logros para recuperar los metadatos pertinentes.

#### <a name="in-app-rewards"></a>Recompensas en la aplicación ####
Estamos introduciendo las recompensas en la aplicación con el fin de brindar a los desarrolladores mucha más flexibilidad y control sobre las ventajas que ofrece un logro. Las recompensas de la aplicación le permiten usar logros entregar personalizadas recompensas en juego directamente a los usuarios sin necesariamente el título de la actualización. Solo tendrá que configurar la recompensa de logro con un código, Id. o frase que reconocerá el título y cuando el usuario desbloquea la consecución, Xbox LIVE enviará ese código en su título, que se informa, por tanto, el título de la recompensa a entregar al usuario.

La recompensa propio depende de usted, el desarrollador. Ideas de recompensa incluyen:

* Moneda extra en el juego o puntos
* Acceso a un carácter especial, arma o asignación
* Un multiplicador experiencia temporal.

### <a name="configuring-in-app-rewards"></a>Configuración de recompensas en la aplicación ###
Configuración de un premio de la aplicación para un logro es bastante sencillo. El propietario de logro debe proporcionar un nombre de recompensa, una descripción de recompensa y un icono de reward además de un valor de recompensa. Este valor de recompensa viene determinada por el desarrollador y debe ser algo que puede interpretar y tratar cualquier juego o que el usuario puede escribir como parte de una experiencia de canje de recompensa de título específico.

Un ejemplo de un valor de recompensa que puede interpretar el juego podría ser un número de 5 dígitos o un especial de cadena que el juego o servicio del juego conoce se asigna a un elemento determinado del juego. Los programadores pueden desear aprovechar el servicio de almacenamiento administrado por título (TMS) para que sea fácil agregar nuevos valores de recompensa con el tiempo que el juego comprenderá cómo se leen.

Podría ser un ejemplo de un valor de recompensa que el usuario debe enviar un código especial o una cadena que el usuario escribe en una experiencia de canje en el título, dentro de una aplicación complementaria, o en el sitio Web del desarrollador.

### <a name="redeeming-in-app-rewards"></a>Canjear premios de la aplicación ###
Una aplicación reward surte efecto cuando el usuario canjea la recompensa dentro del juego. Títulos deben tener en cuenta que un usuario ha desbloqueado una mención especial que se configura con un premio de la aplicación para que el título correctamente puedan proporcionar la recompensa al usuario. Para ello, títulos deben hacer lo siguiente:

1. Consultar el servicio logros tras el lanzamiento de título o reanudación de título de suspensión para ver qué logros desbloqueados tienen recompensas en la aplicación y para obtener el código de recompensa de cada. Siempre debe realizarse para asegurarse de que detecte los logros que pueden haber sido desbloqueados mientras no se está ejecutando el título o en otra consola.  

    Para consultar, puede usar los URI logros RESTful o las API en el Namespace Microsoft.Xbox.Services.Achievements.

2. Regístrese para recibir una notificación cuando se desbloquea uno de sus logros. Esto es opcional, aunque probablemente deseable la mayoría de los títulos. Tenga en cuenta que los títulos solo recibirán esta notificación si el título realmente se está ejecutando cuando se produce el desbloqueo. Este es otro motivo por qué es importante el paso anterior.

   Para registrarse para una notificación de mención especial, use el método de AchievementNotifier.GetTitleIdFilteredSource. En este tema se incluye código de ejemplo que ilustra cómo se registra.
