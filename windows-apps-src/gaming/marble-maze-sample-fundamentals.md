---
author: eliotcowley
title: Conceptos básicos sobre la muestra de Marble Maze
description: Este documento describe las características fundamentales del proyecto Marble Maze; Por ejemplo, cómo usa Visual C++ en el entorno de Windows Runtime, cómo se crea y estructura, y cómo se compila.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.author: elcowle
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, juegos, muestra, directx, conceptos básicos, games, sample, fundamentals
ms.localizationpriority: medium
ms.openlocfilehash: f595c8f429c93a13d6342c281a90f3b0f5741621
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6667823"
---
# <a name="marble-maze-sample-fundamentals"></a>Conceptos básicos sobre la muestra de Marble Maze




En este tema se describen las características fundamentales del proyecto Marble Maze, como la forma en que usa Visual C++ en el entorno Windows Runtime, cómo se crea y estructura, y cómo se compila. El tema también describe varias de las convenciones que se usan en el código.

> [!NOTE]
> El código de ejemplo correspondiente a este documento se encuentra en el [Ejemplo de juego de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

Estos son algunos de los puntos principales que se tratan en este documento para cuando se planea y se desarrolla un juego para la Plataforma universal de Windows (UWP).

-   Usa la plantilla de Visual C++ **DirectX 11 App (Windows universal)** en Visual Studio para crear tu juego para UWP con DirectX.
-   Windows Runtime proporciona clases e interfaces para que puedas desarrollar aplicaciones para UWP de una manera más moderna y orientada a los objetos.
-   Usa referencias a objetos con el símbolo circunflejo (^) para administrar la duración de las variables de Windows Runtime, [Microsoft::WRL::ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) para administrar la duración de los objetos COM y [std::shared\_ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) o [std::unique\_ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) para administrar la duración de todos los demás objetos de C++ asignados por montón.
-   En la mayoría de los casos, usa el controlador de excepciones en vez de los códigos de resultados para tratar los errores inesperados.
-   Usa [anotaciones de SAL](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) junto con herramientas de análisis de código para ayudar a detectar errores en la aplicación.

## <a name="creating-the-visual-studio-project"></a>Creación del proyecto de Visual Studio


Si has descargado y extraído la muestra, puedes abrir el archivo **MarbleMaze_VS2017.sln** (en la carpeta de **C++** ) en Visual Studio y tendrá el código.

Cuando creamos el proyecto de Visual Studio para Marble Maze, empezamos con un proyecto existente. Sin embargo, si aún no tienes un proyecto existente que proporcione la funcionalidad básica que requiere tu juego para UWP con DirectX, recomendamos que crees un proyecto basado en la plantilla **DirectX 11 (Windows universal)** de Visual Studio, porque proporciona una aplicación 3D de trabajo básica. Para ello, sigue estos pasos:

1. En Visual Studio 2017, selecciona **archivo > Nuevo > proyecto …**

2. En la ventana **Nuevo proyecto** , en la barra lateral izquierda, selecciona **instalado > Plantillas > Visual C++**.

3. En la lista central, selecciona **DirectX 11 App (Universal Windows)**. Si no ves esta opción, puede que no tenga los componentes necesarios instalados&mdash;consulta [Modificar Visual Studio 2017 agregando o quitando los componentes y las cargas de trabajo](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) para obtener información sobre cómo instalar componentes adicionales.

4. Dar al proyecto un **nombre**, una **ubicación** para los archivos se almacenen y un **nombre de la solución**y haz clic en **Aceptar**.

![Nuevo proyecto](images/marble-maze-sample-fundamentals-1.png)

Un valor importante del proyecto en la plantilla **DirectX 11 App (Windows universal)** es la opción **/ZW**, que permite que el programa pueda usar las extensiones del lenguaje de Windows Runtime. Esta opción está habilitada de manera predeterminada al usar la plantilla de Visual Studio. Consulta [Configuración de opciones del compilador](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options) para obtener más información sobre cómo establecer las opciones del compilador en Visual Studio.

> **Precaución**  la opción de **/ZW** no es compatible con opciones **como/CLR**. En el caso de **/clr**, esto significa que no puedes tener como objetivo .NET Framework ni Windows Runtime desde el mismo proyecto de Visual C++.

 

Cada aplicación para UWP que compres en la Microsoft Store viene en forma de un paquete de la aplicación. El paquete de la aplicación incluye un manifiesto del paquete, que contiene información sobre tu aplicación. Por ejemplo, puedes especificar las funcionalidades (es decir, el acceso requerido a recursos del sistema protegidos o datos del usuario) de tu aplicación. Si tu aplicación necesita algún tipo de funcionalidad, usa el manifiesto del paquete para declarar la funcionalidad necesaria. El manifiesto también te permite especificar propiedades del proyecto, como las rotaciones admitidas del dispositivo, las imágenes de los iconos y la pantalla de presentación. Puedes editar el manifiesto abriendo **Package.appxmanifest** en el proyecto. Para más información sobre los paquetes de aplicaciones, consulta [Empaquetado de aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt270969).

##  <a name="building-deploying-and-running-the-game"></a>Compilar, implementar y ejecutar el juego

En los menús de lista desplegable de la parte superior de Visual Studio, a la izquierda del botón verde de reproducción, selecciona tu configuración de implementación. Recomendamos establecerla como objetivo **Debug** de la arquitectura de tu dispositivo (**x86** para 32 bits, **x64** para 64 bits) y para tu **máquina local**. También puedes probar en un **equipo remoto**, o en un **dispositivo** que esté conectado mediante USB. A continuación, haz clic en el botón verde de reproducción e implementa en tu dispositivo.

![Depurar; x64; Equipo local](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>Control del juego

Puedes usar táctil, el acelerómetro, el controlador de Xbox One o el mouse para controlar Marble Maze.

-   Usa el control de dirección del mando para cambiar el elemento de menú activo.
-   Usar la función táctil, el A o inicio botón en el controlador o el mouse para seleccionar un elemento de menú.
-   Usa la entrada táctil, el acelerómetro, el stick analógico izquierdo o el mouse para inclinar el laberinto.
-   Usar la función táctil, el A o inicio botón en el controlador o el mouse para cerrar menús, como por ejemplo la tabla de puntuaciones máximas.
-   Usa el botón de inicio en el controlador o la tecla P el teclado para pausar o reanudar el juego.
-   Usa el botón Back del mando o la tecla Inicio del teclado para reiniciar el juego.
-   Cuando la tabla de puntuaciones máximas esté visible, usa el botón Atrás en el controlador o la tecla inicio del teclado para borrar las puntuaciones.

##  <a name="code-conventions"></a> Convenciones del código


Windows Runtime es una interfaz de programación que puedes usar para crear aplicaciones para UWP que se ejecutan solo en un entorno de aplicación especial. Estas aplicaciones usan funciones autorizadas, tipos de datos y dispositivos y se distribuyen desde Microsoft Store. En el nivel inferior, Windows Runtime consta de una interfaz binaria de aplicaciones (ABI). La ABI es un contrato binario de nivel inferior que hace que varios lenguajes de programación, como JavaScript, los lenguajes .NET y Visual C++, puedan acceder a las API de Windows Runtime.

Para llamar a las API de Windows Runtime desde JavaScript y .NET, estos lenguajes requieren proyecciones que son específicas de cada entorno de lenguaje. Cuando llamas a una API de Windows Runtime desde JavaScript o .NET, estás invocando la proyección que, a su vez, llama a la función de ABI subyacente. Aunque puedes llamar a las funciones de ABI directamente en C++, Microsoft proporciona también proyecciones para C++ porque facilitan mucho el consumo de las API de Windows en tiempo de ejecución al tiempo que mantienen un alto rendimiento. Microsoft también proporciona extensiones de lenguaje para Visual C++ que admiten específicamente las proyecciones de Windows Runtime. Muchas de estas extensiones de lenguaje recuerdan a la sintaxis del lenguaje C++/CLI. Sin embargo, en vez de tener como destino CLR (Common Language Runtime), las aplicaciones nativas usan esta sintaxis para tener Windows Runtime como destino. El modificador de la referencia a objetos, o símbolo circunflejo (^), es una parte importante de esta nueva sintaxis porque permite que se eliminen automáticamente los objetos en tiempo de ejecución mediante el recuento de referencias. En vez de llamar a métodos como [AddRef](https://msdn.microsoft.com/library/windows/desktop/ms691379) y [Release](https://msdn.microsoft.com/library/windows/desktop/ms682317) para administrar la duración de un objeto de Windows Runtime, el tiempo de ejecución elimina el objeto cuando no hay ningún otro componente que le haga referencia, por ejemplo, cuando deja el ámbito o cuando estableces todas las referencias como **nullptr**. Otro aspecto importante del uso de Visual C++ para crear aplicaciones para UWP es la palabra clave **ref new**. Usa **ref new** en lugar de **new** para crear objetos de Windows Runtime con recuento de referencias. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822).

> [!IMPORTANT]
> Solo tienes que usar **^** y **ref new** cuando crees objetos de Windows Runtime o componentes de Windows Runtime. Puedes usar la sintaxis estándar de C++ cuando escribas código principal de la aplicación que no use Windows Runtime.

Marble Maze usa **^** junto con **Microsoft::WRL::ComPtr** para administrar objetos asignados por montón y minimizar las pérdidas de memoria. Te recomendamos que uses ^ para administrar la duración de las variables de Windows Runtime, **ComPtr** para administrar la duración de las variables COM (por ejemplo, si usas DirectX) y **std:: shared\_ptr** o **std:: unique\_ptr** para administrar la duración de todos los demás objetos de C++ asignados por montón.

 

Para más información sobre las extensiones de lenguaje disponibles para una aplicación para UWP con C++, consulta [Referencia de lenguaje Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871).

###  <a name="error-handling"></a> Administración de errores

Marble Maze usa el controlador de excepciones como método principal para tratar los errores inesperados. Aunque el código de los juegos tradicionalmente usa códigos de error o de registro, como los valores **HRESULT**, para indicar los errores, el controlador de excepciones tiene dos ventajas principales. En primer lugar, hace que el código sea más fácil de leer y mantener. Desde el punto de vista del código, el controlador de excepciones es una manera más eficaz de propagar un error a una rutina que pueda administrar dicho error. Normalmente, el uso de códigos de error requiere que cada función propague los errores explícitamente. Una segunda ventaja es que puedes configurar el depurador de Visual Studio para que se interrumpa cuando ocurra una excepción de manera que puedas parar inmediatamente en el lugar y contexto del error. Windows Runtime también usa mucho el control de excepciones. Por lo tanto, si usas el control de excepciones en tu código puedes combinar todo el control de errores en un solo modelo.

Recomendamos que uses las siguientes convenciones en tu modelo de administración de errores:

-   Usa excepciones para comunicar errores inesperados.
-   No uses excepciones para controlar el flujo de código.
-   Captura solo las excepciones que puedas administrar y de las que te puedas recuperar de forma segura. De lo contrario, no captures la excepción y deja que la aplicación termine.
-   Cuando llames a una rutina de DirectX que devuelva **HRESULT**, usa la función **DX::ThrowIfFailed**. Esta función se define en [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h). **ThrowIfFailed** inicia una excepción si el proporcionado **HRESULT** es un código de error. Por ejemplo, **E\_POINTER** hace que **ThrowIfFailed** inicie [Platform::NullReferenceException](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx).

    Cuando uses **ThrowIfFailed**, pon la llamada de DirectX en una línea aparte para mejorar la lectura del código, tal como se muestra en el siguiente ejemplo.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Aunque se recomienda evitar el uso de **HRESULT** de errores inesperados, es más importante evitar el uso del control de excepciones para controlar el flujo de código. Por lo tanto, se prefiere usar un valor de retorno **HRESULT** cuando sea necesario para controlar el flujo de código.

###  <a name="sal-annotations"></a> Anotaciones de SAL

Usa anotaciones de SAL junto con herramientas de análisis de código para detectar errores en tu aplicación.

Al usar el lenguaje de anotaciones de código fuente (SAL) de Microsoft, puedes anotar, o describir, cómo las funciones usan sus parámetros. Las anotaciones de SAL también describen valores de retorno. Las anotaciones de SAL funcionan con la herramienta de análisis de código C/C++ para detectar los posibles defectos en el código fuente C y C++. Los errores de codificación comunes notificados por la herramienta incluyen las saturaciones del búfer, la memoria sin inicializar, las desreferencias de puntero nulas y la pérdida de recursos y de memoria.

Ten en cuenta el método **basicloader:: Loadmesh** , que se declara en [BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h). Este método usa `_In_` para especificar el *nombre de archivo* es un parámetro de entrada (y, por tanto, solo se pueden leer desde), `_Out_` para especificar que *vertexBuffer* y *indexBuffer* son parámetros de salida (y, por tanto, solo se escribirá en) y `_Out_opt_` para especificar que *vertexCount* y *indexCount* son opcionales parámetros de salida (y es posible que se escriben en). Como *vertexCount* y *indexCount* son parámetros de salida opcionales, pueden ser **nullptr**. La herramienta de análisis de código C/C++ examina las llamadas a este método para asegurarse de que los parámetros que pase cumplan estos criterios.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Para realizar análisis de código en la aplicación, en la barra de menús, elige **compilación > Ejecutar análisis de código en la solución**. Para más información sobre el análisis de código, consulta [Analizar la calidad del código C/C++ mediante el análisis de código](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

La lista completa de anotaciones disponibles está definida en sal.h. Para obtener más información, consulta [Anotaciones de SAL](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Pasos siguientes


Lee [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md) para obtener información sobre cómo está estructurado el código de la aplicación Marble Maze y para ver las diferencias de las aplicaciones para UWP con DirectX respecto a las aplicaciones de escritorio tradicionales.

## <a name="related-topics"></a>Temas relacionados


* [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




