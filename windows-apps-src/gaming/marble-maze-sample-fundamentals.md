---
title: Conceptos básicos sobre la muestra de Marble Maze
description: En este documento se describen las características fundamentales del proyecto Marble Maze; por ejemplo, la forma en que usa Visual C++ en el entorno Windows Runtime, cómo se crea y se estructura, y cómo se compila.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, juegos, muestra, directx, conceptos básicos, games, sample, fundamentals
ms.localizationpriority: medium
ms.openlocfilehash: f04c17609976e8bd8f6c1c6143ed7b992b0bb3c5
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820620"
---
# <a name="marble-maze-sample-fundamentals"></a>Conceptos básicos sobre la muestra de Marble Maze




En este tema se describen las características fundamentales del proyecto Marble Maze, como la forma en que usa Visual C++ en el entorno Windows Runtime, cómo se crea y estructura, y cómo se compila. El tema también describe varias de las convenciones que se usan en el código.

> [!NOTE]
> El código de ejemplo correspondiente a este documento se encuentra en el [Ejemplo de juego de Marble Maze con DirectX](https://go.microsoft.com/fwlink/?LinkId=624011).

Estos son algunos de los puntos principales que se tratan en este documento para cuando se planea y se desarrolla un juego para la Plataforma universal de Windows (UWP).

-   Use la **aplicación de DirectX 11 (Windows Universal - C++/CX)** plantilla en Visual Studio para crear tu juego DirectX UWP.
-   Windows Runtime proporciona clases e interfaces para que puedas desarrollar aplicaciones para UWP de una manera más moderna y orientada a los objetos.
-   Usar referencias a objetos con el símbolo de acento circunflejo (^) para administrar la duración de las variables en tiempo de ejecución de Windows, [Microsoft::WRL::ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) para administrar la vigencia de objetos COM, y [std::shared\_ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) o [std::unique\_ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) para administrar la duración de todos los demás asignados por montón C++ objetos.
-   En la mayoría de los casos, usa el controlador de excepciones en vez de los códigos de resultados para tratar los errores inesperados.
-   Usa [anotaciones SAL](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) junto con herramientas de análisis de código para detectar errores en tu aplicación.

## <a name="creating-the-visual-studio-project"></a>Creación del proyecto de Visual Studio


Si has descargado y extraído el ejemplo, puedes abrir el archivo **MarbleMaze_VS2017.sln** (en la carpeta **C++** ) en Visual Studio, con lo que te aparecerá el código.

Cuando creamos el proyecto de Visual Studio para Marble Maze, empezamos con un proyecto existente. Sin embargo, si no dispone de un proyecto existente que proporciona la funcionalidad básica que necesita su juego DirectX UWP, recomendamos que cree un proyecto basado en Visual Studio **aplicación de DirectX 11 (Windows Universal - C++/CX)** plantilla porque proporciona una aplicación 3D básica. Para ello, realice los pasos siguientes:

1. En Visual Studio 2019, seleccione **archivo > Nuevo > proyecto...**

2. En el **crear un nuevo proyecto** ventana, seleccione **aplicación de DirectX 11 (Windows Universal - C++/CX)** . Si no ve esta opción, puede que no tenga instalados los componentes necesarios&mdash;vea [modificar Visual Studio 2019 agregando o quitando las cargas de trabajo y componentes](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) para obtener información acerca de cómo instalar componentes adicionales .

![Nuevo proyecto](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. Seleccione **siguiente**y, a continuación, escriba un **nombre del proyecto**, un **ubicación** para los archivos que se almacenan y un **nombre de la solución**y, a continuación, seleccione  **Crear**.



Una configuración importante del proyecto en el **aplicación de DirectX 11 (Windows Universal - C++/CX)** plantilla es la **/ZW** opción, que permite al programa usar las extensiones de lenguaje de Windows en tiempo de ejecución. Esta opción está habilitada de manera predeterminada al usar la plantilla de Visual Studio. Consulta [Configuración de opciones del compilador](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options) para obtener más información sobre cómo establecer las opciones del compilador en Visual Studio.

> **Precaución**    el **/ZW** opción no es compatible con opciones como **/CLR**. En el caso de **/clr**, esto significa que no puedes tener como objetivo .NET Framework ni Windows Runtime desde el mismo proyecto de Visual C++.

 

Todas las aplicaciones para UWP que se obtienen de la Microsoft Store viene en forma de un paquete de aplicación. El paquete de la aplicación incluye un manifiesto del paquete, que contiene información sobre tu aplicación. Por ejemplo, puedes especificar las funcionalidades (es decir, el acceso requerido a recursos del sistema protegidos o datos del usuario) de tu aplicación. Si tu aplicación necesita algún tipo de funcionalidad, usa el manifiesto del paquete para declarar la funcionalidad necesaria. El manifiesto también te permite especificar propiedades del proyecto, como las rotaciones admitidas del dispositivo, las imágenes de los iconos y la pantalla de presentación. Puedes editar el manifiesto abriendo **Package.appxmanifest** en el proyecto. Para más información sobre los paquetes de la aplicación, consulta [Empaquetado de aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/index).

##  <a name="building-deploying-and-running-the-game"></a>Compilar, implementar y ejecutar el juego

En los menús de lista desplegable de la parte superior de Visual Studio, a la izquierda del botón verde de reproducción, selecciona tu configuración de implementación. Recomendamos establecerla como objetivo **Debug** de la arquitectura de tu dispositivo (**x86** para 32 bits, **x64** para 64 bits) y para tu **máquina local**. También puedes probar en un **equipo remoto**, o en un **dispositivo** que esté conectado mediante USB. A continuación, haz clic en el botón verde de reproducción e implementa en tu dispositivo.

![Depurar; x64; equipo local](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>Control del juego

Puedes usar la entrada táctil, el acelerómetro, el mando de la Xbox One o el ratón para controlar Marble Maze.

-   Usa el control de dirección del mando para cambiar el elemento de menú activo.
-   Usa la entrada táctil, el botón A o Inicio del mando, o bien el ratón para seleccionar un elemento del menú.
-   Usa la entrada táctil, el acelerómetro, el stick analógico izquierdo o el mouse para inclinar el laberinto.
-   Usa la entrada táctil, el botón A o Inicio del mando, o bien el ratón para cerrar menús, como la tabla de puntuaciones máximas.
-   Usa el botón Inicio del mando o la tecla P del teclado para pausar o reanudar el juego.
-   Usa el botón Back del mando o la tecla Inicio del teclado para reiniciar el juego.
-   Cuando la tabla de puntuaciones máximas esté visible, usa el botón Atrás del mando o la tecla Inicio del teclado para borrar todas las puntuaciones.

##  <a name="code-conventions"></a>Convenciones del código


Windows Runtime es una interfaz de programación que puedes usar para crear aplicaciones para UWP que se ejecutan solo en un entorno de aplicación especial. Dichas aplicaciones usan funciones, tipos de datos y dispositivos autorizados y se distribuyen desde la Microsoft Store. En el nivel inferior, Windows Runtime consta de una interfaz binaria de aplicaciones (ABI). La ABI es un contrato binario de nivel inferior que hace que varios lenguajes de programación, como JavaScript, los lenguajes .NET y Visual C++, puedan acceder a las API de Windows Runtime.

Para llamar a las API de Windows Runtime desde JavaScript y .NET, estos lenguajes requieren proyecciones que son específicas de cada entorno de lenguaje. Cuando llamas a una API de Windows Runtime desde JavaScript o .NET, estás invocando la proyección que, a su vez, llama a la función de ABI subyacente. Aunque puedes llamar a las funciones de ABI directamente en C++, Microsoft proporciona también proyecciones para C++ porque facilitan mucho el consumo de las API de Windows en tiempo de ejecución al tiempo que mantienen un alto rendimiento. Microsoft también proporciona extensiones de lenguaje para Visual C++ que admiten específicamente las proyecciones de Windows Runtime. Muchas de estas extensiones de lenguaje recuerdan a la sintaxis del lenguaje C++/CLI. Sin embargo, en vez de tener como destino CLR (Common Language Runtime), las aplicaciones nativas usan esta sintaxis para tener Windows Runtime como destino. El modificador de la referencia a objetos, o símbolo circunflejo (^), es una parte importante de esta nueva sintaxis porque permite que se eliminen automáticamente los objetos en tiempo de ejecución mediante el recuento de referencias. En vez de llamar a métodos como [AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) y [Release](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) para administrar la duración de un objeto de Windows Runtime, el tiempo de ejecución elimina el objeto cuando no hay ningún otro componente que le haga referencia, por ejemplo, cuando deja el ámbito o cuando estableces todas las referencias como **nullptr**. Otro aspecto importante del uso de Visual C++ para crear aplicaciones para UWP es la palabra clave **ref new**. Usa **ref new** en lugar de **new** para crear objetos de Windows Runtime con recuento de referencias. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

> [!IMPORTANT]
> Solo tienes que usar **^** y **ref new** cuando crees objetos de Windows Runtime o componentes de Windows Runtime. Puedes usar la sintaxis estándar de C++ cuando escribas código principal de la aplicación que no use Windows Runtime.

Marble Maze usa **^** junto con **Microsoft::WRL::ComPtr** para administrar objetos asignados por montón y minimizar las pérdidas de memoria. Se recomienda que use ^ para administrar la duración de las variables en tiempo de ejecución de Windows, **ComPtr** para administrar la duración de las variables de COM (como cuando usas DirectX), y **std::shared\_ptr** o **std::unique\_ptr** para administrar la duración de todos los demás asignados por montón C++ objetos.

 

Para más información sobre las extensiones de lenguaje disponibles para una aplicación para UWP con C++, consulta [Referencia de lenguaje Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

###  <a name="error-handling"></a>Control de errores

Marble Maze usa el controlador de excepciones como método principal para tratar los errores inesperados. Aunque el código de los juegos tradicionalmente usa códigos de error o de registro, como los valores **HRESULT**, para indicar los errores, el controlador de excepciones tiene dos ventajas principales. En primer lugar, hace que el código sea más fácil de leer y mantener. Desde el punto de vista del código, el controlador de excepciones es una manera más eficaz de propagar un error a una rutina que pueda administrar dicho error. Normalmente, el uso de códigos de error requiere que cada función propague los errores explícitamente. Una segunda ventaja es que puedes configurar el depurador de Visual Studio para que se interrumpa cuando ocurra una excepción de manera que puedas parar inmediatamente en el lugar y contexto del error. Windows Runtime también usa mucho el control de excepciones. Por lo tanto, si usas el control de excepciones en tu código puedes combinar todo el control de errores en un solo modelo.

Recomendamos que uses las siguientes convenciones en tu modelo de administración de errores:

-   Usa excepciones para comunicar errores inesperados.
-   No uses excepciones para controlar el flujo de código.
-   Captura solo las excepciones que puedas administrar y de las que te puedas recuperar de forma segura. De lo contrario, no captures la excepción y deja que la aplicación termine.
-   Cuando llames a una rutina de DirectX que devuelva **HRESULT**, usa la función **DX::ThrowIfFailed**. Esta función se define en [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h). **ThrowIfFailed** inicia una excepción si el **HRESULT** proporcionado es un código de error. Por ejemplo, **E\_puntero** hace **ThrowIfFailed** para producir [NullReferenceException](https://docs.microsoft.com/cpp/cppcx/platform-nullreferenceexception-class).

    Cuando uses **ThrowIfFailed**, pon la llamada de DirectX en una línea aparte para mejorar la lectura del código, tal como se muestra en el siguiente ejemplo.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Aunque recomendamos que evites el uso de **HRESULT** para los errores inesperados, es más importante evitar el uso del controlador de excepciones para controlar el flujo de código. Por lo tanto, se prefiere usar un valor de retorno **HRESULT** cuando sea necesario para controlar el flujo de código.

###  <a name="sal-annotations"></a>Anotaciones de SAL

Usa anotaciones de SAL junto con herramientas de análisis de código para detectar errores en tu aplicación.

Al usar el lenguaje de anotaciones de código fuente (SAL) de Microsoft, puedes anotar, o describir, cómo las funciones usan sus parámetros. Las anotaciones de SAL también describen valores de retorno. Las anotaciones de SAL funcionan con la herramienta de análisis de código C/C++ para detectar los posibles defectos en el código fuente C y C++. Los errores de codificación comunes notificados por la herramienta incluyen las saturaciones del búfer, la memoria sin inicializar, las desreferencias de puntero nulas y la pérdida de recursos y de memoria.

Considera el método **BasicLoader::LoadMesh**, que se declara en [BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h). Este método usa `_In_` para especificar que *filename* es un parámetro de entrada (y por lo tanto solo se leerá), `_Out_` para especificar que *vertexBuffer* e *indexBuffer* son parámetros de salida (y por lo tanto solo se escribirá en ellos) y `_Out_opt_` para especificar que *vertexCount* e *indexCount* son parámetros de salida opcionales (y se podría escribir en ellos). Como *vertexCount* y *indexCount* son parámetros de salida opcionales, pueden ser **nullptr**. La herramienta de análisis de código C/C++ examina las llamadas a este método para asegurarse de que los parámetros que pase cumplan estos criterios.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Para realizar un análisis de código en tu aplicación, en la barra de menús elige **Compilar > Ejecutar análisis de código en la solución**. Para más información sobre el análisis de código, consulta [Analizar la calidad del código C/C++ mediante el análisis de código](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

La lista completa de anotaciones disponibles está definida en sal.h. Para obtener más información, consulta [Anotaciones de SAL](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Pasos siguientes


Lee [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md) para obtener información sobre cómo está estructurado el código de la aplicación Marble Maze y para ver las diferencias de las aplicaciones para UWP con DirectX respecto a las aplicaciones de escritorio tradicionales.

## <a name="related-topics"></a>Temas relacionados


* [Estructura de aplicación de Marble Maze](marble-maze-application-structure.md)
* [Desarrollar Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




