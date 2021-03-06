---
title: Configurar el entorno de desarrollo de UWP en Xbox
description: Obtenga información sobre cómo configurar y probar una UWP en un entorno de desarrollo de Xbox compuesto por un equipo de desarrollo conectado a una consola Xbox One a través de una red local.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 408568e38bd9147564e7ebece17466e22364f3cc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164189"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Configurar el entorno de desarrollo de UWP en Xbox

El entorno de desarrollo de la Plataforma universal de Windows (UWP) en Xbox consta de un equipo de desarrollo conectado a una consola Xbox One a través de una red local.
El equipo de desarrollo requiere Visual Studio 2015 Update 3, Visual Studio 2017 o Visual Studio 2019.
El equipo de desarrollo también requiere Windows 10, el SDK de Windows 10 compilación 14393 o posterior, y una variedad de herramientas de soporte técnico.

En este artículo se describen los pasos para configurar y probar el entorno de desarrollo.

## <a name="visual-studio-setup"></a>Instalación de Visual Studio

1. Instale Visual Studio 2015 Update 3, Visual Studio 2017 o Visual Studio 2019. Para obtener más información, consulta [Descargas y herramientas para Windows 10](https://dev.windows.com/downloads). Se recomienda usar la versión más reciente de Visual Studio para poder recibir las actualizaciones más recientes de los desarrolladores y la seguridad.


2. Si va a instalar Visual Studio 2017 o Visual Studio 2019, asegúrese de elegir la carga de trabajo de **desarrollo de plataforma universal de Windows** . Si es desarrollador de C++, asegúrese de que también selecciona la casilla de **herramientas de c++ plataforma universal de Windows** en el panel de **Resumen** de la derecha, en **plataforma universal de Windows desarrollo**. No forma parte de la instalación predeterminada.

    ![Instalación de Visual Studio 2019](images/development-environment-setup-1.png)

    Si va a instalar Visual Studio 2015 Update 3, asegúrese de que esté activada la casilla **herramientas de desarrollo de aplicaciones universales de Windows** .

    ![Instalar Visual Studio 2015 Update 2](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Configuración del SDK de Windows 10

Instala el SDK más reciente de Windows 10. Esto incluye la instalación de Visual Studio, pero si desea descargarla por separado, consulte el [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).


## <a name="enabling-developer-mode"></a>Habilitar el modo de desarrollador

Para poder implementar aplicaciones desde el equipo de desarrollo, debe habilitar el modo de desarrollador. En la aplicación **configuración** , vaya a **Actualizar & seguridad**  /  **para desarrolladores**y, en **usar características**para desarrolladores, seleccione **modo de desarrollador**.

## <a name="setting-up-your-xbox-one"></a>Configurar la consola Xbox One

Para poder implementar una aplicación en tu Xbox One, un usuario debe haber iniciado sesión en la consola. Puedes usar tu cuenta de Xbox Live existente o crear una cuenta nueva para la consola en el modo de desarrollador. 

## <a name="create-your-first-app"></a>Creación de la primera aplicación

1. Asegúrate de que el equipo de desarrollo esté en la misma red local que la consola Xbox One de destino. Por lo general, esto significa que deben usar el mismo enrutador y estar en la misma subred. Se recomienda una conexión de red con cable.

2. Asegúrate de que la consola Xbox One esté en el modo de desarrollador.  Para obtener más información, consulte [activación del modo de desarrollador de Xbox One](devkit-activation.md).

3. Decide el lenguaje de programación que quieres usar para la aplicación para UWP.

4. En el equipo de desarrollo, en Visual Studio, seleccione **nuevo/proyecto**.

5. En la ventana **nuevo proyecto** , seleccione **aplicación universal o en blanco de Windows (Windows universal)**.

### <a name="starting-a-c-project"></a>Iniciar un proyecto C#

  ![Cuadro de diálogo Nuevo proyecto](images/development-environment-setup-2.png)

1. En el cuadro de diálogo **nuevo proyecto de Windows universal** , seleccione compilar 14393 o posterior en el menú desplegable **versión mínima** . Seleccione el SDK más reciente en la lista desplegable **versión de destino** . Si se muestra el cuadro de diálogo **Modo de desarrollador**, haz clic en **Aceptar**. Se crea una nueva aplicación vacía.

2. Configura el entorno de desarrollo para la depuración remota:

    a. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones**y, a continuación, seleccione **propiedades**.

    b. En la pestaña **depurar** , cambie **plataforma** a **x64**. (x86 ya no es una plataforma admitida en Xbox).

    c. En **Opciones de inicio**, cambie el **dispositivo de destino** a **equipo remoto**.

    d. En **Máquina remota**, escribe la dirección IP del sistema o el nombre de host de la consola Xbox One. Para obtener información acerca de cómo obtener la dirección IP o el nombre de host, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).

    e. En la lista desplegable **Modo de autenticación**, selecciona **Universal (protocolo sin cifrar)**.

    ![Páginas de la propiedad BlankApp de C#](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>Iniciar un proyecto C++

  ![Proyecto C++](images/development-environment-setup-3.png)

1. En el cuadro de diálogo **nuevo proyecto de Windows universal** , seleccione compilar 14393 o posterior en el menú desplegable **versión mínima** . Seleccione el SDK más reciente en la lista desplegable **versión de destino** . Si se muestra el cuadro de diálogo **Modo de desarrollador**, haz clic en **Aceptar**. Se crea una nueva aplicación vacía.

2. Configura el entorno de desarrollo para la depuración remota:

   a. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones**y, a continuación, seleccione **propiedades**.

   b. En la pestaña **Depuración** , cambia **Depurador para iniciar** por **Máquina remota**.

   c. En **Nombre de máquina**, escribe la dirección IP del sistema o el nombre de host de la consola Xbox One. Para obtener información acerca de cómo obtener la dirección IP o el nombre de host, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).

   d. En la lista desplegable **Tipo de autenticación**, selecciona **Universal (protocolo sin cifrar)**.

   e. En el menú desplegable **plataforma** , seleccione **x64**.

    ![Páginas de la propiedad BlankApp de C++](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>ANCLAJE: emparejar el dispositivo con Visual Studio

1. Guarda la configuración y asegúrate de que la consola Xbox One esté en modo de desarrollador.

2. Con el proyecto abierto en Visual Studio, presione F5.

3. Si es la primera implementación, obtendrás un cuadro de diálogo de Visual Studio que te solicitará que emparejes el dispositivo mediante PIN.

    a. Para obtener un PIN, abre **Dev Home** desde la pantalla de inicio en la consola Xbox One.

    b. En la pestaña **Inicio** , en **acciones rápidas**, seleccione **Mostrar PIN de Visual Studio**.
  
    ![Cuadro de diálogo Emparejar con Visual Studio](images/development-environment-setup-5.png)

    c. Introduce el PIN en el cuadro de diálogo **Pair with Visual Studio**. El PIN siguiente es solo un ejemplo; el tuyo será diferente.

    ![Cuadro de diálogo Emparejar con PIN de Visual Studio](images/devhome_pin.png)

    d. Los errores de implementación, si los hay, aparecerán en la ventana **Resultados**.

Enhorabuena, has creado e implementado tu primera aplicación para UWP en Xbox correctamente.

## <a name="see-also"></a>Vea también
- [Activación del modo de desarrollador de Xbox One](devkit-activation.md)  
- [Descargas y herramientas para Windows 10](https://developer.microsoft.com/windows/downloads)  
- [Programa de Windows Insider](https://insider.windows.com/)  
- [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md) 
- [UWP en Xbox One](index.md)