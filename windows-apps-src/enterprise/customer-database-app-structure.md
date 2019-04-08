---
title: Estructura de aplicación de base de datos de cliente
description: Revise la estructura del tutorial de base de datos de cliente, y por qué ha construido cómo lo está.
keywords: Enterprise, tutorial, clientes, datos, crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656620"
---
# <a name="customer-database-app-structure"></a>Estructura de aplicación de base de datos de cliente

Complejas aplicaciones de línea de negocio suelen tengan muchas páginas y las características y una gran cantidad de líneas de código. Por este motivo, resulta de vital importancia que diseñe la aplicación en torno a una estructura de predicción. Hay varios patrones de diseño de aplicación que sean adecuadas para aplicaciones empresariales, pero que se generaron con el objetivo de hacer más fácil entender y trabajar con una aplicación a gran escala.

Mientras el [tutorial de base de datos de cliente](customer-database-tutorial.md) presenta una aplicación de una página por motivos de simplicidad, implementa el patrón de diseño de la aplicación de Model-View-ViewModel (MVVM) para presentar estas ideas en acción. Como el nombre implica, el patrón de diseño MVVM separa la lógica de aplicación principal en tres categorías:

* Los modelos son las clases que contienen datos de la aplicación.
* Las vistas son la interfaz de usuario de una página dada.
* ViewModels proporcionan la lógica de aplicación. Esto puede incluir acciones del usuario desde la vista de control o administrar interacciones con los modelos.

Aunque esta aplicación no es un ejemplo perfecto y architypical de MVVM, muestran los principios fundamentales de separación de preocupaciones en acción. [Consulte aquí la aplicación.](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>Estructura de la aplicación

Después de abrir la aplicación, comenzar examinando el **el Explorador de soluciones.** Algunos de lo que ve debería haber familiar si ha trabajado con una aplicación para UWP antes, pero también verá una colección de carpetas que contienen los componentes de la aplicación.

![Punto de partida en el Explorador de soluciones de aplicación](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>Vistas

Interfaz de usuario de la de la aplicación se define en la carpeta Views. Dado que nuestro tutorial es una aplicación de una página ahora mismo, esto significa que hay una única vista - **CustomerListPage**. Tiene marcado XAML UI y código subyacente xaml.cs: estos dos archivos constituyen una vista. Iremos agregando elementos de interfaz de usuario a **CustomerListPage.xaml**.

> [!NOTE]
> Es posible que tenga en cuenta que esta aplicación no tiene una MainPage. Eso es porque se especifica en **App.xaml.cs** que debe iniciar la aplicación **CustomerListPage** cuando inicie.

### <a name="viewmodels"></a>Modelos de vista

Aunque esta aplicación sólo tiene una vista, tiene dos modelos de vista. ¿Por qué es esto?

**CustomerListPageViewModel.cs** es un modelo de vista estándar en el patrón MVVM. Es donde se encuentra la lógica fundamental de la página de aplicación y la página, trabajará con la mayor parte del tutorial. Cada acción de la interfaz de usuario realizada por el usuario se pasa a través de la vista para este modelo de vista para su procesamiento.

**CustomerViewModel.cs**, sin embargo, no está asociado a ninguna vista específica. En su lugar, asocia un concepto mediante programación (las propiedades que se han editado) con los datos contenidos en el modelo de clientes individuales.

### <a name="models"></a>Modelos

Esta aplicación contiene tres modelos, que almacenan los datos de la aplicación y proporcionan interfaces para interactuar con el repositorio. Si bien estos son componentes esenciales de la aplicación, no son algo que va a editar directamente en el tutorial.

Lo más importante es **Customer.cs**, que describe la estructura de datos de cliente que va a utilizar en el tutorial.

> [!NOTE]
> El tutorial pasa por alto el *correo electrónico* y *teléfono* propiedades del objeto de cliente. Si desea ir más allá de lo que se presenta en este caso, agrega estas dos propiedades en la interfaz de usuario de la aplicación es un buen primer paso.

### <a name="repository"></a>Repositorio

La carpeta del repositorio contiene clases que construirán e interactúan con la base de datos SQLite local. Para este tutorial, la base de datos de SQLite se presenta como-es. Mientras que agregará en el código para **CustomerListPageViewModel.cs** para llamar a métodos definidos por estas clases, no necesita realizar ningún cambio para configurarlas.

Para obtener más información en SQLite en UWP, [consulte este artículo](../data-access/sqlite-databases.md).

Si trata de la sección "Continuar" de este tutorial, esto es donde podrá crear una clase para conectarse a la base de datos remota de REST. También implementará el **ICustomerRepository** interfaz definida en la sección de modelos, pero tendrá un aspecto muy diferente de su homólogo de SQLite.

### <a name="other-elements"></a>Otros elementos

Como es habitual para aplicaciones UWP, se define el comportamiento de inicio de la aplicación en el **App.xaml.cs** clase. La mayor parte de este código es el código predeterminado para cualquier aplicación para UWP. Pero ya hemos realizado algunos pequeños cambios:

* Especificamos que la aplicación debe mostrar **CustomerListPage** en el inicio.
* Hemos creado un objeto de repositorio, que contendrá el origen de datos que estamos usando.
* Hemos agregado un **SQLiteDatabase** método, que inicializa la base de datos local y lo establece como el repositorio especificado.

Si trata de la sección "Continuar", agregará un método similar para inicializar un objeto del repositorio de REST. Dado que se haya separados por lo que nos ocupa y está usando la misma interfaz definida para las operaciones de SQLite y REST, este será el único código existente, deberá cambiar para usar REST en lugar de SQLite en la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Si ya ha completado el tutorial, puede retirar el [aplicación de ejemplo completa](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver cómo estas características se implementan en una escala mayor.

En caso contrario, ahora que sabe por qué todo lo que es el lugar, debe [devuelto al tutorial](customer-database-tutorial.md) y trabajar con la estructura que hemos incluido solo.