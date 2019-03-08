---
title: Espacios aislados avanzados de Xbox Live
description: Obtenga información sobre cómo aislar el contenido de forma eficaz mediante el uso de espacios aislados de Xbox Live como un socio administrado o un ID@Xbox miembro.
ms.assetid: bd8a2c51-2434-4cfe-8601-76b08321a658
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4c54a4bf3ab665ded7bfaa45d3b6be9346c384d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599010"
---
# <a name="advanced-xbox-live-sandboxes"></a>Espacios aislados avanzados de Xbox Live

> **Tenga en cuenta** en este artículo explica el uso avanzado de espacios aislados y es aplicable principalmente a los estudios de juegos grandes que tienen varios equipos y los requisitos de permisos complejos.  Si forma parte del programa creadores de Xbox Live o un ID@Xbox para desarrolladores, se recomienda mirar el [introducción de espacios aislados de Xbox Live](../xbox-live-sandboxes.md)

Xbox Live *sandbox* proporciona todo un entorno privado para el desarrollo. Este documento explica qué son los espacios aislados, ¿por qué existen, cómo se aplican a los publicadores y cómo afectan los equipos internos de Xbox. Los destinatarios de este documento son publicadores que crean contenido Xbox One y usar los espacios aislados.

## <a name="summary"></a>Resumen

En Xbox Live, hay solo un entorno de producción único donde reside todas las versiones preliminares (en desarrollo y la versión beta), certificación y contenido de venta directa.

Aislamiento de contenido es la manera de asegurarse de que no hay ningún pérdidas de contenido de publicador en producción. En esencia, el aislamiento del contenido garantiza que cualquier usuario de entidad, el dispositivo o el título que solicita permiso para acceder a un recurso (un título o un servicio) se ha autorizado para acceder al recurso. Con aislamiento de contenido, las particiones se dividen en espacios aislados de donde se almacenan los datos de título o el servicio. En otras palabras, las directivas de autorización se definen dentro del ámbito de un espacio aislado.

Los espacios aislados son una forma de crear particiones de datos que se encuentra en producción. Con los servicios de Xbox 360 era, PartnerNet y ProductionNet son dos entornos diferentes. Con los servicios de Xbox One era, contiene un entorno de producción solo *n* distintos entornos virtuales, donde cada entorno virtual se llama a un espacio aislado. Dado que hay un entorno de producción único para todo el contenido, espacios aislados son realmente únicos entornos virtuales donde no se cruzan datos generados en un entorno a otro.

La siguiente ilustración muestra un entorno de producción solo en el que los editores pueden crear sus espacios aislados de desarrollo privada. Kits de desarrollo o cuentas de desarrollo autorizados solo permiso de acceso a estos espacios aislados.

Figura 1. Espacios aislados en un entorno de producción.

![](../images/sandboxes/sandboxes_image1.png)

Al igual que PublisherA tiene espacios aislados de su desarrollo, otros publicadores tienen sus propios espacios aislados de desarrollo. El mismo identificador de título puede residir en diferentes entornos limitados, pero los datos generados para el identificador del título están distintos entre los espacios aislados.

Hay dos espacios aislados del sistema que solo se pueden rellenar con Microsoft: CERT y comercial. Como sugieren los nombres, el espacio aislado de certificado es para los títulos que se está llevando a cabo antes de la certificación de versión, mientras que el espacio aislado de venta directa es el espacio aislado que representa el dinero que sea accesible para todos los usuarios y dispositivos.

Mientras que un identificador de título era anteriormente único en Xbox Live, ahora un identificador de título más de un identificador de espacio aislado es único. Lo mismo se aplica a los identificadores de producto y otros espacios de identificador que una vez que se trataban como único. Ahora debe estar emparejados con un identificador de espacio aislado. Todos los datos de Xbox Live se particionará principalmente por el identificador de espacio aislado en todo el sistema.

## <a name="initial-setup-for-a-title"></a>Configuración inicial para un título

Es necesario que exista un título en el Portal para desarrolladores de Xbox (XDP) o el centro de partners. Se asigna un identificador de título y un identificador de producto y una configuración de servicio Id. (¿SCID).

En este nuevo mundo, un título o un producto por sí solo no significa nada a Xbox Live. Dado que debemos admitimos simultáneo por menor y el uso de desarrollo de un título único, debe admitir *de creación de instancias* de títulos con el fin de realizar y mantener las distinciones necesarias. Una instancia de un título reside en un espacio aislado y esto es donde entran en juego los espacios aislados.

Para crear un título, un publicador crea un grupo de productos, especifica el género del grupo de producto y, a continuación, crea productos individuales dentro de él. (Para obtener más información, consulte la documentación de XDP). El siguiente diagrama ilustra las relaciones entre un grupo de productos, un producto, una instancia de producto y un espacio aislado.

Figura 2. Las relaciones entre un grupo de productos, un producto, una instancia de producto y un espacio aislado.

![](../images/sandboxes/sandboxes_image2.png)

<a name="product-instances"></a>Instancias de productos
-----------------
Un *instancia de un producto* es una proyección de datos de título, producto y la configuración en un recinto de seguridad específico. Estos datos se describen en las siguientes tres áreas: configuración, metadatos de catálogo y los archivos binarios del servicio.

### <a name="service-configuration"></a>Configuración del servicio

> Definiciones de configuración de servicio (eventos, estadísticas, logros, etcetera). La configuración del servicio se define en el nivel de instancia del producto.

### <a name="catalog-metadata"></a>Metadatos de catálogo

> Los metadatos que se encuentra en el catálogo, incluidos texto venta, activos de material gráfico, información de disponibilidad/oferta, licencias de datos y mucho más.

### <a name="binary"></a>Binary

> Un archivo binario se puede representar de dos maneras:

1.  Metadatos solo para habilitar la instalación de prueba. Esto incluye el Id. de contenido, la información de versión e información de licencia.

2.  Binario completo se propaga a una red CDN y descargable para un cliente.

<a name="getting-the-access-right"></a>Obtener el acceso adecuado
------------------------
Hay dos tipos distintos de acceso a su contenido en Xbox One:

*Acceso en tiempo de diseño*: acceso desde un equipo a través de la herramienta XDP: permite que las personas que trabajan en sus productos cargar, organizar y trabajar con contenido, configuración y metadatos, pero no les permite ejecutar o reproducir las instancias de sus productos.

*Acceso en tiempo de ejecución*: acceso desde una consola Xbox, permite a los desarrolladores, evaluadores, los revisores y, finalmente, los clientes para ejecutar y reproducir las instancias de product.

**Tenga en cuenta** con el fin de que estén disponibles para el acceso de tiempo de ejecución, se debe colocar una instancia de un producto en un espacio aislado. Una vez que una compilación se coloca en un espacio aislado, los usuarios XDP o dispositivos de devkit que han concedido acceso a ese espacio aislado pueden ejecutar la instancia. Para ello, inicien sesión en Xbox One a través de una consola Xbox, mediante uno de sus cuentas de desarrollo: especial cuentas a esa función virtuales como usuarios con acceso de tiempo de ejecución.

Cuando nos referimos a los espacios aislados, normalmente, hablamos sobre el tiempo de ejecución de acceso al contenido que se ejecuta en Xbox Live. Para tener acceso a un servicio en Xbox Live, se requiere un identificador de título. Una vez un **appxmanifest** contiene un identificador de título, la consola enviará el identificador del título a Xbox Live. Servicios de seguridad de Xbox Live no proporcionará atrás un token válido a menos que la entidad de seguridad (usuario o dispositivo) haya otorgado acceso al título.

Este proceso de validación es el quid de aislamiento del contenido. Cuando se ve en un nivel muy alto:

-   Un grupo de entidad de seguridad puede contener los identificadores de usuario de Xbox (XUIDs), los identificadores de dispositivo, identificadores de título o Id. de servicio.

-   Un espacio aislado puede contener los identificadores de título, los identificadores de producto o identificadores (SCIDs) de la configuración de servicio.

-   Un grupo de entidad de seguridad se concede acceso a un espacio aislado.

Por lo tanto, para que un usuario o dispositivo tener acceso a un título de la versión preliminar en un espacio aislado, debe conceder acceso a través de XDP primero.

Figura 3. Un modelo para configurar el acceso a través de XDP.

![](../images/sandboxes/sandboxes_image3.png)

La eficacia de aislamiento de contenido está basada en el hecho de que su organización es propietaria de los siguientes procesos:

-   Creación de las cuentas de usuario XDP, las cuentas de desarrollo que cada usuario usará para iniciar sesión para el acceso de tiempo de ejecución y los grupos de usuarios en el que se concede pertenencia a cada usuario.

-   Creación de grupos de dispositivos de las consolas de confianza.

-   Para cada uno de los espacios aislados de desarrollo que especifica exactamente qué grupos de usuarios y grupos de dispositivos tiene acceso a las instancias de producto en ella.

En la ilustración siguiente se muestra un ejemplo de este programa de instalación.

Figura 4. Las credenciales de un usuario no autorizado producirá un error obtener acceso al espacio aislado, igual que las credenciales de una cuenta de usuario autorizada XDP normales. Solo las credenciales de la cuenta de desarrollo que pertenecen a la cuenta de usuario autorizada XDP correctamente obtengan acceso de tiempo de ejecución para el espacio aislado y todas las instancias de producto actualmente en ella.

![](../images/sandboxes/sandboxes_image4.png)

### <a name="dev-accounts-setup"></a>Configuración de cuentas de desarrollo

Las cuentas de desarrollo en Xbox One son cuentas estándar de Microsoft (MSA) con reglas especiales que se les aplica. Se utilizan en Xbox Live para el desarrollo. Una cuenta de desarrollador:

-   Se deben crear desde XDP o centro de partners.

-   Se le asigna el rol de desarrollador externo cuando crea los publicadores.

-   Está asociada a la cuenta XDP o centro de partners que creó la cuenta de desarrollo.

-   Solo se puede iniciar sesión a los kits de desarrollo. Inicio de sesión se deniega a una cuenta de desarrollador en los dispositivos de venta directa.

-   Puede comprar suscripción Gold de Xbox Live para desarrolladores u otras suscripciones de forma gratuita con el fin de probar.

### <a name="user-group-setup"></a>Configuración del grupo de usuario

Un grupo de usuarios, el primer tipo de grupo principal, es una colección de usuarios XDP. Cuando los usuarios XDP se agregan a grupos de usuarios, sus cuentas de desarrollo de flujo con estos usuarios XDP.

Por lo que cuando un grupo de usuarios se asigna a un espacio aislado, las cuentas de desarrollo asociados con los usuarios XDP en que el grupo de usuarios se agregan a grupos de entidad de seguridad adecuados y los principales grupos obtención una configuración de directiva con el recurso primario, establecido el espacio aislado de seguridad.

**Tenga en cuenta** los grupos de usuarios que se crean para tener acceso a los espacios aislados son los mismos grupos de usuario que se usan para impedir el acceso a datos de configuración en XDP para grupos de productos y los productos.

### <a name="device-setup"></a>Configuración de dispositivo

Un dispositivo también se agrega a un grupo de entidad de seguridad. Un dispositivo solo puede usarse como un kit de desarrollo si un derecho se compró a través de la Store para desarrolladores de juegos y se aprovisiona el dispositivo para que sea un kit de desarrollo. Una vez que un dispositivo se aprovisiona como un kit de desarrollo, el dispositivo aparece en la lista de dispositivos que pueden agregarse a grupos de dispositivos.

### <a name="device-group-setup"></a>Configuración del grupo de dispositivos

Un grupo de dispositivos, el segundo tipo de grupo principal, también se puede asignar acceso a los espacios aislados. El programa de instalación es similar a la configuración del grupo de usuario detallada anteriormente.

## <a name="sandboxes"></a>Espacios aislados

<a name="what-is-a-sandbox"></a>¿Qué es un espacio aislado?
------------------
Simplemente, indicados *un espacio aislado es una manera de crear particiones de datos en producción*.

<a name="why-do-we-need-sandboxes"></a>¿Por qué necesitamos espacios aislados?
-------------------------
Igual que los usuarios y dispositivos acceso a los títulos, los títulos de acceso a servicios. Presentamos un concepto de "título del grupo de" donde los conjuntos de títulos se concedió acceso a los recursos del servicio.

Porque no hay un entorno de producción solo para Xbox One para todo el contenido (versión preliminar y comercial), deben evitarse (pre-preliminares o tienda) de varias instancias de un título del funcionamiento de las mismas instancias de recursos.

<a name="what-is-in-a-sandbox"></a>¿Qué es un espacio aislado?
---------------------
Un espacio aislado contiene una instancia de un producto para cada título que se agrega al espacio aislado.

<a name="what-is-a-sandbox-id"></a>¿Qué es un identificador de espacio aislado?
---------------------
Un identificador de espacio aislado es una unidad de creación de particiones de datos para un título, un producto o una configuración de servicio. Pueden haber varios títulos en el mismo espacio aislado, que es un requisito previo para poder compartir los datos de configuración de servicio.

Un identificador de espacio aislado (distingue mayúsculas de minúsculas) es una cadena en el formato siguiente: &lt;PublisherMoniker&gt;. *n*. Un identificador de espacio aislado de ejemplo, XLDP.5, se explica a continuación:

-   El *moniker publisher* es único en todos los publicadores. Por lo tanto, "XLPD" es el moniker de publicador para este publicador determinado. Cuando un publicador está "activado" en XDP por el Administrador de cuenta de desarrollador, se crea un moniker de publicador.

-   El dígito *"n"* identifique la cantidad de espacio aislado. En este caso, "5" es el sexto recinto de seguridad creado para este publicador.

Cuando los datos del título se mueven a través de servicios, servicios de Xbox usan el identificador de espacio aislado para identificar de forma única el "entorno" para los datos que se generan.

<a name="what-data-is-sandboxed"></a>¿Qué datos están en un espacio aislado?
-----------------------
El diagrama siguiente muestra los datos de usuario y el título están un espacio aislados.

![](../images/sandboxes/sandboxes_image5.png)

<a name="global-override-sandbox"></a>Espacio aislado de un reemplazo global
-----------------------
Establece el identificador de espacio aislado en su kit de desarrollo de un desarrollador y, por tanto, Establece el espacio aislado que se ejecuta el kit de desarrollo Esto también es conocido como el espacio aislado de un reemplazo global. Por lo tanto, todas las solicitudes realizadas a los servicios de Xbox Live (por ejemplo, logros, contactos, las licencias, EDS, etc.) de los títulos (aplicaciones de shell y aplicaciones regulares) en el desarrollo kit se realizan en ese espacio aislado.

El espacio aislado de un reemplazo global también implica que solo el contenido ingerido en el espacio aislado de un reemplazo global está visible cuando se examinan.

<a name="types-of-sandboxes"></a>Tipos de espacios aislados
-------------------------------------------------------------------------------------------------------------------------------------------------------
Hay dos categorías diferentes de los espacios aislados. Estas categorías se definen como sigue:

-   *Los espacios aislados de publicador*. Los publicadores tienen acceso a sus entornos de desarrollo limitados. Es posible que son similares a XLDP.0, XLDP.1, XLDP.2, XLDP.3, etcetera. Esto es donde los publicadores tendría que poner sus instancias de producto del título. Acceso a estos recintos viene determinado para los usuarios o dispositivos que tienen acceso las concesiones de los publicadores a

-   *Los espacios aislados de Microsoft*. Estos son los espacios aislados integrados: RETAIL y CERT solo Microsoft puede publicar en estos recintos protegidos.

<a name="cert-sandbox"></a>Espacio aislado CERT
------------
Cuando un título está listo para la disponibilidad general, debe ir primero a través de la certificación. El recinto de seguridad del certificado es un espacio aislado controlados por Microsoft que solo los individuos de certificación tienen acceso. Los publicadores pueden ver el contenido que se va a través de certificación que son propietarios.

Las instancias de producto que se producirá un error en la certificación pueden devolverla a un espacio aislado de desarrollo para depurar y corregir los publicadores que utilizan XDP.

<a name="retail-sandbox"></a>Espacio aislado de venta directa
--------------
El espacio aislado de venta directa es el destino final de todo el contenido que se crea para Xbox One.

Después de un título pasa la certificación, se agrega al espacio aislado de venta directa. Sólo contenido firmado verde se puede ejecutar en el espacio aislado de venta directa. Esto tiene una implicación importante que las versiones beta controlado por el publicador también se realizan en el espacio aislado de venta directa. Datos generados en el espacio aislado de venta directa representan los datos de producción reales de clientes.

Tenga en cuenta que el acceso a contenido es que el espacio aislado de venta directa es todavía puede controlar a través de aislamiento de contenido.

Por ejemplo, versiones beta controlado por el publicador se ejecutan en el espacio aislado de venta directa, donde el publicador decide obtención los grupos de la entidad de seguridad tener acceso a los títulos de conjunto de recursos beta definido por el publicador. Los datos generados por los títulos de la versión beta del servicio es de producción real de datos y continuarán existiendo una vez que el título se dirige a la disponibilidad general.

<a name="cross-sandbox-data-interaction"></a>Interacción de los datos entre sandbox
------------------------------
Por definición, un espacio aislado es un contenedor que restringe el uso compartido de datos. Por lo tanto, no es posible interactuar con los datos entre sandbox.

## <a name="organizing-your-sandboxes"></a>Organizar los espacios aislados

En esta sección se proporciona un ejemplo de cómo un publicador puede organizar los espacios aislados. Un publicador debe aprender a usar los espacios aislados para organizar los datos.

**Tenga en cuenta** los ejemplos siguientes muestran solo la administración de acceso de tiempo de ejecución con aislamiento de contenido.

### <a name="scenario-1-two-titles-one-sandbox"></a>Escenario 1: Dos títulos, un recinto de seguridad

La estructura básica de un publicador podría ser:

-   Dos de los títulos que son accesibles para todos los usuarios y dispositivos que pertenecen el publicador para el tiempo de diseño y tiempo de ejecución.

-   Instancia de uno de los productos por título.

En este caso, el publicador solo necesita un recinto único para todo el contenido de versión preliminar.

El diagrama siguiente muestra un grupo de usuarios. El publicador puede usar un grupo de dispositivos en lugar de un grupo de usuarios, si se considera más fácil. Además, este grupo de usuarios tiene acceso de tiempo de diseño y tiempo de ejecución en espacio aislado XLDP.1 y los títulos en este espacio aislado.

![](../images/sandboxes/sandboxes_image6.png)

### <a name="scenario-2-one-title-different-teams"></a>Escenario 2: Un título, los equipos diferentes

En este modelo, los requisitos son:

-   Un título.

-   Equipo de desarrollo trabaja en compilaciones diarias.

-   Equipo de control de calidad trabaja en LKGs semanales.

-   Equipo de desarrollo debe depurar LKGs semanales en el caso de errores.

-   El equipo de finanzas necesita acceso a las tarjetas de precio y otros metadatos relacionados con la versión del catálogo de un título.

La siguiente ilustración muestra que TitleX tiene dos instancias de producto: PI-1 y 2 de PI. Una instancia de un producto debe estar en un espacio aislado y dos instancias de producto del mismo título no pueden estar en el mismo espacio aislado. Por lo tanto, es TitleX-PI-1 en espacio aislado XLDP.1 y TitleX-PI-2 se encuentra en el espacio aislado XLDP.2.

El grupo de usuarios de desarrollo tiene acceso a ambos espacios aislados, mientras que el grupo de usuarios de preguntas y respuestas tiene acceso a solo sandbox XLDP.2.

Además, el usuario de finanzas (grupo C) tiene acceso en tiempo de diseño a TitleX. Dado que el grupo de usuarios de finanzas normalmente no realizará ninguna depuración en tiempo de ejecución de un título, se dividen.

**Tenga en cuenta** con independencia de la organización, un usuario XDP puede pertenecer a más de un grupo de usuarios.

![](../images/sandboxes/sandboxes_image7.png)

### <a name="scenario-3-two-titles-completely-separate"></a>Escenario 3: Dos títulos, completamente independientes

En este ejemplo, los requisitos cambian un poco:

-   Dos títulos.

-   Acceso a cada título debe limitarse a un determinado conjunto de personas.

-   Instancia de uno de los productos por título.

-   Un grupo de usuarios administrador debe tener acceso a datos de configuración de tiempo de diseño XDP para los títulos. Las personas en este grupo son todos los administradores para el publicador y pueden controlar todos los datos que se publican en el catálogo (metadatos de catálogo, finanzas, marketing, envío de certificación, etcetera.)

En este modelo, el publicador ha decidido mantener dos títulos completamente separada y asignado, por tanto, estas dos títulos en dos espacios aislados diferentes. El publicador también ha elegido para crear un grupo de usuarios de administración independiente y asignado acceso a los dos productos.

![](../images/sandboxes/sandboxes_image8.png)

### <a name="scenario-4-anyway-you-like-it"></a>Escenario 4: De todos modos como

Debido al número de conexiones y para mantener las palabras en breve, hemos elegido mostrar solo el espacio aislado de conexiones en tiempo de ejecución. Nada le impide agregar también otros permisos de acceso en tiempo de diseño.

En este ejemplo, los requisitos son:

-   Solo determinadas personas tengan acceso a determinados títulos dentro de su publicador.

-   El publicador se trabaja con proveedores de compañías diferentes y estos proveedores pueden ser a corto plazo.

-   El publicador debe ser capaz de retirada de un título y de esta forma, evitar el acceso a los datos que los proveedores o gozará como empleado ha tenido acceso a.

Para modelar este requisito, se puede adoptar una estructura como el siguiente.

Es el modelo sigue a continuación:

-   TitleX y TitleY tienen solo una instancia del producto en espacio aislado XLDP.1.

-   TitleZ tiene instancias de dos productos, de espacio aislado XLDP.2 y otro en espacio aislado XLDP.3.

-   FTE usuario grupo B se concede acceso a las instancias de product en todos los espacios aislados.

-   Grupo de usuario del proveedor A es un grupo de usuario solo de proveedor que se concede acceso al espacio aislado XLDP.1.

-   Grupo de dispositivos del proveedor C es un grupo de usuario solo de proveedor que se concede acceso al espacio aislado XLDP.3.

![](../images/sandboxes/sandboxes_image9.png)

## <a name="summary"></a>Resumen

Desarrollo de Xbox Live proporciona una gran oportunidad a los publicadores para probar en producción con servicios de calidad de producción y las cuentas de desarrollador MSA de producción. El aumento de funcionalidad y flexibilidad requiere pasos de configuración de nuevo en XDP para crear datos de título y administrar el acceso a los títulos mientras en desarrollo y en disponibilidad general.

Los espacios aislados son una forma de crear particiones de datos en producción. Dado que hay un entorno de producción único para todo el contenido, espacios aislados de actúan como "entornos virtuales" donde los datos generados en un entorno no pase a la otra.
