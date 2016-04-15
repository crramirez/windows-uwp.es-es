---
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: Planificación del rendimiento
description: Los usuarios esperan que sus aplicaciones sean dinámicas, que su uso sea natural y que no agoten fácilmente la batería.
---
# Planificación del rendimiento

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Los usuarios esperan que sus aplicaciones sean dinámicas, que su uso sea natural y que no agoten fácilmente la batería. Técnicamente, el rendimiento es un requisito no funcional, pero tratarlo como una característica te ayudará a cumplir las expectativas de los usuarios. Especificar objetivos y realizar mediciones son factores clave. Determina cuáles son los escenarios críticos para el rendimiento, define lo que significa un buen rendimiento. A continuación, puedes realizar mediciones al principio y con la suficiente frecuencia durante el ciclo de vida del proyecto, para estar seguro de que cumples los objetivos.

## Especificar objetivos

La experiencia del usuario es una forma básica de definir el buen rendimiento. La percepción del usuario del rendimiento de una aplicación puede verse afectada por su tiempo de inicio. Un usuario debería tener en cuenta que un tiempo de inicio de una aplicación de menos de un segundo se considera un rendimiento excelente; un rendimiento de menos de 5 segundos, bueno, y uno de más de 5 segundos, deficiente.

Otras métricas tienen un impacto menos obvio en la experiencia del usuario; por ejemplo, la memoria. Las posibilidades de que una aplicación finalizada se suspenda o quede inactiva aumentan según la cantidad de memoria que use la aplicación activa. Como regla general, un alto uso de la memoria degrada la experiencia de todas las aplicaciones en el sistema, por lo que es razonable tener un objetivo de consumo de memoria. Ten en cuenta el tamaño general de la aplicación según lo perciben los usuarios: pequeño, mediano o grande. Las expectativas del rendimiento se correlacionan con esta percepción. Por ejemplo, una aplicación pequeña que no usa una gran cantidad de medios debería consumir menos de 100 MB de memoria.

Es mejor establecer un objetivo inicial y revisarlo más tarde, que no tener ningún objetivo en absoluto. Los objetivos de rendimiento de la aplicación deben ser específicos y mensurables y deben dividirse en tres categorías: el tiempo que tardan los usuarios o la aplicación en completar tareas (tiempo); la velocidad y la continuidad con la que la aplicación se redibuja en respuesta a la interacción del usuario (fluidez); y cómo conserva la aplicación los recursos del sistema, incluida la batería (eficiencia).

## Tiempo

Piensa en los intervalos de tiempo aceptables (*clases de interacción*) necesarios para que los usuarios realicen sus tareas en tu aplicación. Para cada clase de interacción, asigna una etiqueta, una opinión de percepción del usuario y unas duraciones máximas e ideales. Aquí tienes algunas sugerencias.

| Etiqueta para clase de interacción | Percepción del usuario                 | Ideal            | Máxima          | Ejemplos                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| Rapidez                    | Retraso apenas perceptible      | 100 milisegundos | 200 milisegundos | Mostrar la barra de la aplicación, presionar un botón (primera respuesta)                        |
| Típica                 | Veloz, pero no rápida             | 300 milisegundos | 500 milisegundos | Cambiar de tamaño, zoom semántico                                                        |
| Dinámica              | No es veloz, pero se percibe dinámica | 500 milisegundos | 1 segundo         | Navegar a otra página, reanudar la aplicación desde un estado suspendido          |
| Ejecución                  | Experiencia competitiva          | 1 segundo         | 3 segundos        | Iniciar la aplicación la primera vez o después de haberla cerrado |
| Continua              | Ya no se percibe dinámica      | 500 milisegundos | 5 segundos        | Descargar un archivo de Internet                                            |
| Cautiva                 | Extensa, el usuario podría marcharse    | 500 milisegundos | 10 segundos       | Instalar varias aplicaciones de la Tienda                                         |

 

Ahora puedes asignar clases de interacción a escenarios de rendimiento de la aplicación. Puedes asignar, por ejemplo, una referencia en un momento dado de la aplicación, una parte de la experiencia del usuario y una clase de interacción a cada escenario. Aquí tienes algunas sugerencias para un ejemplo de aplicación de comidas.


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>Situación</th><th>Momento dado</th><th>Experiencia del usuario</th><th>Clase de interacción</th></tr>
<tr><td rowspan="3">Navegar a una página de recetas </td><td>Primera respuesta</td><td>Comienzo de la animación de transición de página</td><td>Rápida (de 100 a 200 milisegundos)</td></tr>
<tr><td>Dinámica</td><td>Carga de lista de ingredientes, sin imágenes</td><td>Dinámica (de 500 milisegundos a 1 segundo)</td></tr>
<tr><td>Visibilidad completa</td><td>Carga de todo el contenido, incluidas las imágenes</td><td>Continua (de 500 milisegundos a 5 segundos)</td></tr>
<tr><td rowspan="2">Buscar una receta</td><td>Primera respuesta</td><td>Clic en el botón Buscar</td><td>Rápida (de 100 a 200 milisegundos)</td></tr>
<tr><td>Visibilidad completa</td><td>Visualización de la lista de iconos de recetas locales</td><td>Típica (de 300 a 500 milisegundos)</td></tr>
</table>

Si vas a mostrar contenido en directo, te recomendamos que tengas también en cuenta la posibilidad de marcar objetivos de actualización del contenido. ¿El objetivo es actualizar contenido cada pocos segundos? ¿O la actualización de contenido debe realizarse cada pocos minutos, pocas horas o, incluso, una vez por día para tener una experiencia de usuario aceptable?

Con tus objetivos especificados, ahora tienes una mayor capacidad para probar, analizar y optimizar tu aplicación.

## Fluidez

Los objetivos mensurables de fluidez de la aplicación pueden incluir:

-   Ningún dibujo de pantalla que se haya vuelto a hacer se detiene ni comienza (problemas).
-   Las animaciones se representan a 60 fotogramas por segundo (FPS).
-   Cuando un usuario realiza un movimiento panorámico o de desplazamiento, la aplicación presenta de 3 a 6 páginas de contenido por segundo.

## Eficiencia

Los objetivos mensurables de eficiencia de la aplicación pueden incluir:

-   Para el proceso de tu aplicación, el porcentaje de CPU es igual o inferior a *N*, y el uso de la memoria (en MB) es igual o inferior a *M* en todo momento.
-   Cuando la aplicación está inactiva, *N* y *M* son igual a cero en el proceso de la aplicación.
-   La aplicación puede usarse activamente durante *X* horas con batería; cuando la aplicación está inactiva, el dispositivo retiene su carga durante *Y* horas.

## Diseñar la aplicación para el rendimiento

Ahora puedes usar tus objetivos de rendimiento para diseñar la aplicación. Si usas la aplicación de comida que usamos como ejemplo, una vez que el usuario navegue a la página de recetas, puedes elegir actualizar los elementos de forma incremental ([update items incrementally](optimize-gridview-and-listview.md#update-items-incrementally)), para que el nombre de la receta se represente en primer lugar, y que así se aplace la visualización de los ingredientes y que la visualización de imágenes se aplace aún más. Esto mantiene la capacidad de respuesta y una interfaz de usuario fluida en el movimiento panorámico y el desplazamiento, con una representación de fidelidad total una vez que la interacción se reduce a un ritmo que pueda seguir el subproceso de interfaz de usuario. A continuación se enumeran algunos otros aspectos que se deben tener en cuenta.

**Interfaz de usuario**

-   Maximiza el tiempo de análisis y carga y la eficacia de la memoria de cada página de la interfaz de usuario de la aplicación (sobre todo en la página inicial), optimizando el marcado XAML ([optimizing your XAML markup](optimize-xaml-loading.md)). En pocas palabras, aplaza la carga de la interfaz de usuario y el código hasta que estos sean necesarios.
-   Para [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705), haz que todos los elementos tengan el mismo tamaño y usen tantas técnicas de optimización de ListView y GridView ([ListView and GridView optimization techniques](optimize-gridview-and-listview.md)) como te sea posible.
-   Declara la interfaz de usuario en forma de marcado, de manera que puedas cargar y reutilizar el marco en fragmentos, en lugar de crearlo de forma imperativa en el código.
-   Contrae los elementos de interfaz de usuario hasta que el usuario los necesite. Consulta la propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992).
-   Opta por las transiciones de tema y las animaciones en lugar de las animaciones de guion gráfico. Para obtener más información, consulta [Animations overview](https://msdn.microsoft.com/library/windows/apps/Mt187350) (Información general sobre animaciones). Recuerda que las animaciones de guion gráfico requieren actualizaciones constantes en la pantalla, y mantienen la CPU y la canalización de gráficos activas. Para conservar la batería, te recomendamos que no tengas animaciones en funcionamiento si el usuario no está interactuando con la aplicación.
-   Las imágenes deben cargarse en el tamaño apropiado para la vista en que la presentas, mediante el método [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210).

**CPU, memoria y energía**

-   Programa trabajos de menor prioridad para que se ejecuten en subprocesos o núcleos de menor prioridad. Consulta [Asynchronous programming](https://msdn.microsoft.com/library/windows/apps/Mt187335) (Programación asincrónica), la propiedad [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) y la clase [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).
-   Minimiza la superficie de memoria de la aplicación liberando los recursos que más usen la memoria (como los recursos multimedia), poniéndolos en suspensión.
-   Minimiza el espacio de trabajo del código.
-   Evita las pérdidas de memoria. Para ello, elimina el registro de los controladores de eventos y la referencia de elementos de la interfaz de usuario, siempre que te sea posible.
-   Para ahorrar batería, recuerda que debes ser controlar la frecuencia con la que se sondean los datos, se consultan los sensores o se programa el trabajo en la CPU cuando está inactiva.

**Acceso a los datos**

-   Si es posible, captura con antelación el contenido. Para realizar la captura previa automática, consulta la clase [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042). Para realizar la captura previa manual, consulta el espacio de nombres [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) y la clase [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517).
-   Si te es posible, almacena en la memoria caché el contenido cuyo acceso sea difícil. Consulta las propiedades [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) y [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622).
-   Para los errores de caché, muestra una interfaz de usuario de marcador de posición lo más rápido posible y que indique que la aplicación aún está cargando contenido. Haz que la transición de contenido de marcador de posición a dinámico se realice de una forma sencilla que no moleste al usuario. Por ejemplo, no cambie la posición del contenido debajo del dedo del usuario o el puntero del mouse mientras se carga el contenido dinámico.

**Iniciar y reanudar aplicaciones**

-   Aplaza la página de presentación de la aplicación y no la extiendas a menos que sea necesario. Para obtener más detalles, consulta [Creating a fast and fluid app launch experience](http://go.microsoft.com/fwlink/p/?LinkId=317595) (Crear una experiencia de inicio de la aplicación rápida y fluida) y [Display a splash screen for more time](https://msdn.microsoft.com/library/windows/apps/Mt187309) (Mostrar una pantalla de presentación durante más tiempo).
-   Deshabilita las animaciones que se produzcan inmediatamente después de descartar la página de presentación, ya que estas solo causan una sensación de demora en el inicio de la aplicación.

**Interfaz de usuario adaptable y orientación**

-   Usa la clase [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021).
-   Completa solo el trabajo requerido de manera inmediata y aplaza el trabajo intensivo para más adelante: la aplicación tiene entre 200 y 800 milisegundos para terminar el trabajo, antes de que el usuario vea la interfaz en estado recortado.

Con todo listo en materia de diseño de rendimiento, puedes empezar a codificar la aplicación.

## Instrumento de rendimiento

Durante la codificación, puedes agregar código que registre mensajes y eventos en determinados puntos mientras la aplicación se ejecuta. Una vez hayas probado la aplicación, puedes usar herramientas de generación de perfiles como Windows Performance Recorder y Windows Performance Analyzer (ambas incluidas en [Windows Performance Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh162945.aspx)) para crear y ver un informe sobre el rendimiento de la aplicación. En este informe, puedes buscar los mensajes y eventos para que te resulte más fácil analizar los resultados.

La Plataforma universal de Windows (UWP) proporciona varias API de registro, respaldadas por la característica [Event Tracing for Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/Bb968803) (Seguimiento de eventos para Windows), que en conjunto ofrecen una solución enriquecida de registro y seguimiento de eventos para aplicaciones de la Tienda Windows. Las API, que son parte del espacio de nombres [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677), incluyen las clases [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138), [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195), [**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) y [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217).

Para registrar un mensaje en un punto específico del informe mientras la aplicación se ejecuta, crea un objeto **LoggingChannel** y, a continuación, llama al método [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/Dn264202-logmessage) del objeto.

```csharp
// mediante Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel(&quot;MyLoggingChannel&quot;);

myLoggingChannel.LogMessage(LoggingLevel.Information, &quot;Aquí aparece el mensaje registrado&quot;);

// ...
```

Para registrar eventos de inicio y finalización en el informe durante un periodo de tiempo mientras la aplicación se ejecuta, crea un objeto **LoggingActivity** y, a continuación, llama al constructor [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195-loggingactivity) de esta forma.

```csharp
// mediante Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel ya se definió e inicializó en el ejemplo de código anterior.
mediante (myLoggingActivity = new LoggingActivity(&quot;MyLoggingActivity&quot;), myLoggingChannel))
{   // Una vez haya comenzado la actividad de registro, se registrará un evento de inicio.
    
    // Agrega código aquí para hacer algo interesante.
    
}   // Cuando finalice el registro de la actividad, se registrará un evento final.

// ...
```

Puedes consultar también la muestra de registro: [Logging sample](http://go.microsoft.com/fwlink/p/?LinkId=529576).

Con la aplicación instrumentada, ya puedes probar y medir el rendimiento.

## Probar y medir según los objetivos de rendimiento

Parte del plan de rendimiento es definir durante el desarrollo todos los puntos según los cuales se medirá el rendimiento. Esto sirve para diferentes fines en función de si mides durante la fase de creación del prototipo, la de desarrollo o la de implementación. Medir el rendimiento durante las primeras etapas de un prototipo puede resultar muy valioso, por lo que te recomendamos hacerlo tan pronto como tengas un código que realice algún trabajo significativo. Las mediciones en las primeras etapas te proporcionan una idea clara de los lugares donde se consumen más recursos en la aplicación y te proporcionan información para que puedas tomar las decisiones de diseño correctas. Esto da como resultado aplicaciones de alto rendimiento y escalado. Por lo general, resulta costeso cambiar los diseños más tarde. Si el rendimiento se mide tarde en el ciclo del producto, es posible que debas realizar modificaciones de última hora y que el rendimiento no sea bueno.

Usa estas técnicas y herramientas para probar la aplicación con respecto a los objetivos de rendimiento originales.

-   Realiza pruebas en distintas configuraciones de hardware, incluyendo equipos de escritorio y todo en uno, equipos portátiles, ultrabooks, tabletas y otros dispositivos móviles.
-   Realiza pruebas en distintos tamaños de pantalla. Si bien cuanto mayor sea la pantalla más se puede mostrar, presentar todo ese contenido adicional puede tener un impacto negativo en el rendimiento.
-   Elimina todas las variables de prueba que puedas.
    -   Cierra aplicaciones en segundo plano en el dispositivo de prueba. Para ello, en Windows, selecciona **Configuración** desde el menú Inicio &gt; **Personalización** &gt; **Pantalla de bloqueo**. Selecciona cada aplicación activa y selecciona **Ninguno**.
    -   Compila la aplicación en código nativo. Para ello, compílala en una configuración de lanzamiento antes de implementarla en el dispositivo de prueba.
    -   Para garantizar que el mantenimiento automático no afecta al rendimiento del dispositivo de prueba, desencadénalo manualmente y espera a que finalice. En Windows, en el menú Inicio, busca **Seguridad y mantenimiento**. En el área **Mantenimiento**, en **Mantenimiento automático**, selecciona **Iniciar mantenimiento** y espera a que el estado cambie de **Mantenimiento en curso**.
    -   Ejecuta la aplicación varias veces para eliminar las variables aleatorias de prueba y garantizar medidas coherentes.
-   Realiza pruebas para comprobar si se ha reducido la disponibilidad de consumo de energía. Ten en cuenta que los dispositivos de los usuarios podrían tener mucha menos energía que el equipo de desarrollo. Windows se ha diseñado teniendo en cuenta los dispositivos de baja energía, como los dispositivos móviles. Debes asegurarte de que las aplicaciones que se ejecutan en la plataforma funcionan correctamente en estos dispositivos. Como heurístico, cuenta con que un dispositivo de baja energía funciona a, aproximadamente, un cuarto de la velocidad de un equipo de escritorio, y define tus objetivos en consecuencia.
-   Combina herramientas, como Microsoft Visual Studio y Windows Performance Analyzer, para medir el rendimiento de la aplicación. Visual Studio está diseñado para proporcionar un análisis centrado en la aplicación, como la vinculación del código de origen. Windows Performance Analyzer está diseñado para proporcionar un análisis centrado en el sistema, como la información del sistema, información sobre los eventos de manipulación táctil e información sobre la entrada y salida en disco y el coste de la unidad de procesamiento gráfico (GPU). Ambas herramientas proporcionan capturas y exportación de seguimiento y pueden reabrir seguimientos compartidos y finales.
-   Antes de enviar la aplicación a la Tienda para su certificación, asegúrate de incorporar en los planes de prueba los casos relacionados con el rendimiento, tal como se describe en la sección "Pruebas de rendimiento" de [Windows App Certification Kit tests](windows-app-certification-kit-tests.md) (Pruebas del Kit para la certificación de aplicaciones en Windows) y en la sección "Rendimiento y estabilidad" de [Windows Store app test cases](https://msdn.microsoft.com/library/windows/apps/Dn275879) (Casos de pruebas de aplicaciones de la Tienda Windows).

Para obtener más información, consulta estos recursos y herramientas de generación de perfiles.

-   [Windows Performance Analyzer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh162945.aspx)
-   [Analyze performance using Visual Studio diagnostic tools](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh696636.aspx) (Analizar el rendimiento con herramientas de diagnóstico de Visual Studio)
						
-   Sesión //build/ [XAML Performance](https://channel9.msdn.com/Events/Build/2015/3-698) (Rendimiento de XAML)
-   Sesión //build/ [New XAML Tools in Visual Studio 2015](https://channel9.msdn.com/Events/Build/2015/2-697) (Nuevas herramientas XAML de Visual Studio 2015)

## Responder a los resultados de la prueba de rendimiento

Después de analizar los resultados de las pruebas de rendimiento, determina si es necesario hacer cambios. Por ejemplo:

-   ¿Debes cambiar alguna de tus decisiones de diseño de la aplicación u optimizar el código?
-   ¿Debes agregar, quitar o cambiar algunos de los instrumentos del código?
-   ¿Debes revisar alguno de los objetivos de rendimiento?

Si es necesario hacer cambios, hazlos y vuelve a instrumentar o probar y repite la operación.

## Optimizar

Optimizar las rutas de código crítico para el rendimiento de la aplicación es lo que más tiempo conlleva. La creación de perfiles te lo indicará. A menudo, existe un equilibrio entre la creación de software que cumpla con buenas prácticas de diseño y la escritura de código que funcione en la optimización más elevada. Suele ser mejor dar prioridad a la productividad del desarrollador y a un buen diseño del software en las áreas donde no importa tanto el rendimiento.



<!--HONumber=Mar16_HO1-->


