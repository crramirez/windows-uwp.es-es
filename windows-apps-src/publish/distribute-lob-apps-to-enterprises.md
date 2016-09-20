---
author: jnHs
Description: "Puedes publicar aplicaciones de línea de negocio (LOB) directamente para que las empresas las compren por volumen a través de la Tienda Windows para empresas, sin necesidad de que las aplicaciones estén disponibles en la Tienda de forma general."
title: Distribuir aplicaciones de LOB a empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.sourcegitcommit: 9ad7589344d2af986e52ae43acc3e48de6374ae6
ms.openlocfilehash: d7551e0456ce0e59dbbfa92690ddd5ba2ebaf8b4

---

# Distribuir aplicaciones de LOB a empresas


Puedes publicar aplicaciones de línea de negocio (LOB) directamente para que las empresas las compren por volumen a través de la Tienda Windows para empresas, sin necesidad de que las aplicaciones estén disponibles en la Tienda de forma general.

> 
            **Importante**  Por ahora, solo las aplicaciones gratuitas pueden distribuirse de forma exclusiva a las empresas a través de la Tienda Windows para empresas. Si envías una aplicación de pago como LOB, esta no estará disponible para la empresa en este momento. 

## Configurar la asociación de empresa


El primer paso para publicar aplicaciones LOB exclusivamente para una empresa es establecer la asociación entre tu cuenta y la tienda privada de la empresa.

> 
            **Importante**  Este proceso de asociación debe iniciarlo la empresa y debe enviarse a la dirección de correo electrónico de la **Información de contacto** de tu cuenta. Para obtener más información, consulta el artículo [Working with line-of-business apps (Trabajar con aplicaciones de línea de negocio)](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Si una empresa decide invitarte para que publiques aplicaciones para su uso exclusivo, recibirás un correo electrónico con un vínculo para confirmar la asociación. También puedes confirmar estas asociaciones en la sección **Asociaciones de empresa** de tu **Configuración de la cuenta**.

Para confirmar la asociación, haz clic en **Aceptar**. De este modo, podrás publicar aplicaciones desde tu cuenta para uso exclusivo de la empresa.

## Enviar aplicaciones de LOB


Cuando estés listo para publicar una aplicación para uso exclusivo de la empresa, el proceso es similar al proceso de envío de aplicaciones. La aplicación pasa por el mismo proceso de certificación y debe cumplir con todas las [directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944). Solo hay dos etapas del proceso que son diferentes.

### Distribución y visibilidad

Una vez hayas configurado una asociación de empresa, cada vez que envíes una aplicación, verás un cuadro desplegable en la sección **Distribución y visibilidad** de la página **Precios y disponibilidad** del envío. Esta opción está establecida en **Retail distribution** de forma predeterminada. Para que la aplicación sea exclusiva de una empresa, tienes que elegir **Line-of-business (LOB) distribution**.

Cuando se selecciona la opción **Line-of-business (LOB) distribution**, las opciones habituales de **Distribución y visibilidad** se reemplazan con una lista de las empresas para las que puedes publicar aplicaciones exclusivas.

Selecciona las empresas que deben obtener la aplicación. Nadie más podrá acceder a ella.

> 
            **Nota**  Tienes que seleccionar una empresa, como mínimo, para publicar una aplicación como línea de negocio.

### Licencias organizativas

De forma predeterminada, la casilla **licencias por volumen (en línea) administradas por la Tienda** se activa al enviar una aplicación. Al publicar aplicaciones de LOB, esta casilla debe estar activada para que la empresa pueda comprar tu aplicación por volumen. Esto no significa que la aplicación estará a disposición del público, sino para las empresas específicas que seleccionaste en la sección **Distribución y visibilidad**.

Si quieres que la aplicación esté disponible para la empresa a través de licencias sin conexión, puedes activar la casilla **Licencias desconectadas (sin conexión)** también.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).

### Implementación de aplicaciones de LOB para empresas

Después de hacer clic **Enviar a la Tienda**, la aplicación pasará por el proceso de certificación. Cuando esté lista, un administrador de la empresa debe agregarla a su tienda privada en el portal de la Tienda Windows para empresas. Luego la empresa puede implementar la aplicación para sus usuarios.

> 
            **Nota** Para obtener la aplicación de línea de negocio, la organización debe encontrarse en un [mercado admitido](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets) y no debes haber excluido ese mercado al enviar la aplicación. 

Para obtener más información, consulta [Trabajar con aplicaciones de línea de negocio](http://go.microsoft.com/fwlink/p/?LinkId=698846) y [Distribuir aplicaciones a través de la tienda privada](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### Actualizar las aplicaciones de LOB

Para publicar actualizaciones de una aplicación que ya publicaste como LOB, simplemente crea un nuevo envío. Puedes cargar nuevos paquetes o hacer cambios y luego solo tienes que hacer clic en **Enviar a la Tienda** para que la versión actualizada esté disponible. Asegúrate de que las selecciones de empresas en **Distribución y visibilidad** sean las mismas (a menos que intencionadamente quietas cambiarlas; por ejemplo, seleccionar una empresa adicional para que compre la aplicación o eliminar de una de las empresas a la que habías distribuido la aplicación anteriormente).

Si quieres dejar de ofrecer una aplicación que ya publicaste como línea de negocio y evitar nuevas compras, tienes que crear un nuevo envío. En primer lugar, cambia la selección en **Distribución y visibilidad** de **Line-of-business (LOB) distribution** a **Retail distribution**. A continuación, en las opciones de **Distribución y visibilidad**, elige **Ocultar esta aplicación e impedir la compra**. Después de que el envío pase por el proceso de certificación, la aplicación ya no estará disponible para nuevas compras (aunque quienes ya la tienen podrán seguir usándola).

### Distribuir aplicaciones de LOB a través de la instalación de prueba

La creación de aplicaciones a través de la Tienda para empresas garantiza que la aplicación esté firmada por la Tienda y que cumpla con las directivas estándar de esta.

En algunos casos, puede que las empresas no quieran que sus aplicaciones de LOB se envíen a través del Centro de desarrollo de Windows por diferentes razones (por ejemplo, por cuestiones de cumplimiento o debido a aplicaciones que necesitan funciones adicionales). En este caso, las empresas pueden implementar aplicaciones directamente en las máquinas a través de la instalación de prueba, sin necesidad de usar la Tienda Windows para empresas.

Para obtener más información, consulta [Transferir localmente aplicaciones de LOB en Windows10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 







<!--HONumber=Jun16_HO5-->


