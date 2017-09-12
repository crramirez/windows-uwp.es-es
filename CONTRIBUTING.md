# <a name="contributing-to-uwp-conceptual-documentation"></a>Contribuir a la documentación conceptual para UWP

Gracias por tu interés en la documentación para la Plataforma universal de Windows (UWP). Agradecemos tus comentarios, modificaciones y adiciones a nuestros documentos.

En esta página se describen los pasos básicos para contribuir a nuestra documentación para desarrolladores.

## <a name="public-and-private-repos"></a>Repositorios públicos y privados

Los documentos conceptuales de UWP se alojan en dos repositorios diferentes que luego se combinan, se mezclan y actualizan en un único sitio: un repositorio es para las contribuciones de cualquier persona y el otro es solo para empleados de Microsoft.

Si ***no*** eres empleado de Microsoft, trabaja en el [repositorio de contenido público](https://github.com/MicrosoftDocs/windows-uwp).

Si ***eres*** empleado de Microsoft, puedes trabajar en el repositorio público o el [repositorio de contenido privado](https://cpubwin.visualstudio.com/_git/windows-uwp). Los empleados pueden insertar cambios de manera dinámica algo más rápidamente si contribuyen en el repositorio privado o usan una [rama concreta](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/setup-local-repo-for-large-changes#what-branch-should-i-use-for-my-authoring) para los cambios que deben permanecer confidenciales hasta una fecha futura.

## <a name="editing-topics-on-the-public-repo"></a>Editar temas en el repositorio público

Hemos intentado hacer que las ediciones en un archivo existente sean lo más simple posible. 
- Si ya estás en el repositorio, simplemente navega al archivo y haz clic en el botón **Editar**.  
- Como alternativa, si estás viendo una página Docs.microsoft.com en el explorador, haz clic en el botón **Editar** de la parte superior derecha de la página. Serás redirigido al archivo de origen de marcado correcto en el repositorio, donde puedes hacer clic en el botón **Editar**. 

GitHub bifurca automáticamente el repositorio oficial en tu cuenta personal de GitHub, donde puedes realizar los cambios. Cuando hayas terminado, envía una solicitud de extracción hacia la rama "docs". Después de crear la solicitud de extracción, un miembro del equipo de documentación para UWP revisará los cambios. Si tu solicitud se acepta, las actualizaciones se publican en https://docs.microsoft.com/windows/.

Puedes aprender los conceptos básicos de marcado en tan solo unos minutos.  Para empezar, consulta [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) (Aprender sobre marcado).

## <a name="making-more-substantial-changes"></a>Hacer cambios más sustanciales

Para hacer cambios sustanciales en un artículo existente, agregar o cambiar imágenes, o contribuir a un nuevo artículo, tendrás que crear un clon local de nuestro repositorio de contenido privado. Sigue las [instrucciones de nuestra guía de creación de Windows](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/). Si todavía no configuraste una cuenta de GitHub y el alias de Microsoft unido al dominio, [empieza aquí](https://review.docs.microsoft.com/en-us/windows-authoring-guide/github-account).

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Usar problemas para proporcionar comentarios en la documentación conceptual para UWP

Si quieres proporcionar comentarios, en lugar de modificar directamente las páginas de documentación real, puedes [crear una incidencia en el repositorio público](https://github.com/MicrosoftDocs/windows-uwp/issues). Haz clic en la pestaña "Problemas" y luego en el botón **Nuevo problema**. Asegúrate de incluir el título del tema y la dirección URL de la página.

Los miembros del equipo de documentación para UWP revisan periódicamente los problemas y los clasificarán, asignar y solucionarán según corresponda.

*Para los problemas internos, usa la herramienta de solicitud de contenido WDG en [http://aka.ms/pubrequest](http://aka.ms/pubrequest). 
