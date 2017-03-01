---
author: jnHs
Description: Establecimiento de permisos personalizados para usuarios de cuentas.
title: Establecimiento de permisos personalizados para usuarios de cuentas
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 23d8c14bfdbfc05a1397fa67cb831d38ec092233
ms.lasthandoff: 02/08/2017

---

# <a name="set-custom-permissions-for-account-users"></a>Establecimiento de permisos personalizados para usuarios de cuentas

Al agregar usuarios a tu cuenta, puedes proporcionarles un [rol estándar](manage-account-users.md#roles-and-permissions) o puedes personalizar sus permisos para proporcionarle al usuario el nivel de acceso adecuado. Algunos de estos permisos se aplican a toda la cuenta, y algunos pueden haberse concedido a todos los productos o haberse limitado a determinados productos. 

Para usar permisos personalizados en lugar de roles estándar, haz clic en **Personalizar permisos** en la sección **Roles** al agregar o modificar la cuenta del usuario. 

> **Nota** Pueden aplicarse los mismos permisos con independencia de que vayas a agregar un usuario, un grupo o una aplicación de Azure AD.

Si quieres habilitar un permiso para el usuario, activa o desactiva la casilla con el ajuste que corresponda. 

![Guía de la configuración de acceso](images/permission_key.png)

- **Sin acceso**: el usuario no tendrá el permiso indicado.
- **Solo lectura**: el usuario tendrá acceso para ver las características relacionadas con el área indicada, pero no para realizar cambios.
- **Lectura y escritura**: el usuario tendrá acceso tanto para realizar cambios relacionados con el área como para verla.
- **Mixto**: no puedes seleccionar esta opción directamente, pero el indicador **Mixto** te mostrará si has permitido a una combinación de acceso de dicho permiso. Por ejemplo, si concedes acceso de **Solo lectura** a **Precios y disponibilidad** para **Todos los productos**, pero luego concedes acceso de **Lectura y escritura** a **Precios y disponibilidad** para un producto específico, el indicador **Precios y disponibilidad** de **Todos los productos** se mostrará como mixto. Lo mismo sucede si algunos productos presentan **Sin acceso** para un permiso y otros, acceso de **Lectura y escritura** o de **Solo lectura**.

En el caso de algunos permisos, como los relacionados con la visualización de datos analíticos, solo se puede conceder acceso de **Solo lectura**. Ten en cuenta que en la implementación actual algunos permisos no distinguen en absoluto entre acceso de **Solo lectura** y de **Lectura y escritura**. Revisa los detalles de cada permiso para comprender las capacidades específicas que concede el acceso de **Solo lectura** y de **Lectura y escritura**.

En las tablas siguientes se describen los datos concretos de cada permiso.

## <a name="account-level-permissions"></a>Permisos de nivel de cuenta

Los permisos de esta sección no se pueden limitar a productos específicos. La concesión de acceso a estos permisos otorga al usuario permiso para toda la cuenta.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Nombre de permiso</th>
    <th align="left">Solo lectura</th>
    <th align="left">Lectura y escritura</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    **Configuración de la cuenta**                    </td><td align="left">  Permite ver todas las páginas de la sección **Configuración de la cuenta**, incluida la [información de contacto](managing-your-profile.md).       </td><td align="left">  Permite ver todas las páginas de la sección **Configuración de la cuenta**. Permite realizar cambios en la [información de contacto](managing-your-profile.md) y en otras páginas, pero no en la cuenta de pago ni en el perfil fiscal (a menos que este permiso se haya concedido por separado).            </td></tr>
<tr><td align="left">    **Usuarios de cuenta**                       </td><td align="left">  Permite ver los usuarios que se han agregado a la cuenta en la sección **Administrar usuarios**.          </td><td align="left">  Permite agregar usuarios a la cuenta y realizar cambios en los usuarios existentes en la sección **Administrar usuarios**.             </td></tr>
<tr><td align="left">    **Informe de rendimiento de anuncios de nivel de cuenta** </td><td align="left">  Permite ver el [Informe de rendimiento de la publicidad](advertising-performance-report.md#account-level-advertising-performance-report) de nivel de cuenta. (No permite ver los informes de rendimiento de la publicidad de productos concretos a menos que ese permiso se conceda por separado).       </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Campañas de anuncios**                        </td><td align="left">  Permite ver las [campañas de anuncios](create-an-ad-campaign-for-your-app.md) que se han creado en la cuenta.      </td><td align="left">  Permite crear, administrar y ver las [campañas de anuncios](create-an-ad-campaign-for-your-app.md) que se han creado en la cuenta.          </td></tr>
<tr><td align="left">    **Mediación de anuncios**                        </td><td align="left">  Permite ver las [configuraciones de mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) de todos los productos de la cuenta.    </td><td align="left">  Permite ver y cargar las [configuraciones de mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) de todos los productos de la cuenta.        </td></tr>
<tr><td align="left">    **Informes de mediación de anuncios**                </td><td align="left">  Permite ver el [informe de mediación de anuncios](ad-mediation-report.md) de todos los productos de la cuenta.    </td><td align="left">  N/D    </td></tr>
<tr><td align="left">    **Informes del rendimiento de anuncios**              </td><td align="left">  Permite ver los [informes de rendimiento de la publicidad](advertising-performance-report.md) de todos los productos de la cuenta. (No permite ver el [informe de rendimiento de la publicidad](advertising-performance-report.md#account-level-advertising-performance-report) de nivel de cuenta a menos que ese permiso se conceda por separado).       </td><td align="left">  Permite ver los [informes de rendimiento de la publicidad](advertising-performance-report.md) de todos los productos de la cuenta. (No permite ver el [informe de rendimiento de la publicidad](advertising-performance-report.md#account-level-advertising-performance-report) de nivel de cuenta a menos que ese permiso se conceda por separado).         </td></tr>
<tr><td align="left">    **Unidades de anuncios**                            </td><td align="left">  Permite ver las [unidades de anuncios](monetize-with-ads.md) que se han creado para la cuenta.    </td><td align="left">  Permite crear, administrar y ver las [unidades de anuncios](monetize-with-ads.md) de la cuenta.             </td></tr>
<tr><td align="left">    **Anuncios de filiales**                       </td><td align="left">  Permite ver la utilización de [anuncios de filiales](about-affiliate-ads.md) de todos los productos de la cuenta.    </td><td align="left">  Permite administrar y ver la utilización de los [anuncios de filiales](about-affiliate-ads.md) de todos los productos de la cuenta.                </td></tr>
<tr><td align="left">    **Informe de rendimiento de filiales**      </td><td align="left">  Permite ver el [informe de rendimiento de filiales](affiliates-performance-report.md) de todos los productos de la cuenta.   </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Informes de anuncios de instalación de aplicaciones**             </td><td align="left">  Permite ver el [informe de anuncios de instalación de aplicaciones](app-install-ads-reports.md) de todos los productos de la cuenta.           </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Anuncios de la comunidad**                       </td><td align="left">  Permite ver de forma gratuita la utilización del [anuncio de la comunidad](about-community-ads.md) de todos los productos de la cuenta.          </td><td align="left">  Permite crear, administrar y ver la utilización del [anuncio de la comunidad](about-community-ads.md) gratuito de todos los productos de la cuenta.               </td></tr>
<tr><td align="left">    **Información de contacto**                        </td><td align="left">  Permite ver la [información de contacto](managing-your-profile.md) en la sección de configuración de la cuenta.        </td><td align="left">  Permite ver y editar la [información de contacto](managing-your-profile.md) en la sección de configuración de la cuenta.            </td></tr>
<tr><td align="left">    **Cumplimiento de COPPA**                    </td><td align="left">  Permite ver selecciones del [cumplimiento de COPPA](monetize-with-ads.md#coppa-compliance) (que indica si los productos se destinan a niños menores de 13 años) de todos los productos de la cuenta.                                            </td><td align="left">  Permite ver y editar selecciones del [cumplimiento de COPPA](monetize-with-ads.md#coppa-compliance) (que indica si los productos se destinan a niños menores de 13 años) de todos los productos de la cuenta.         </td></tr>
<tr><td align="left">    **Grupos de clientes**                     </td><td align="left">  Permite ver [grupos de clientes](create-customer-groups.md) (segmentos y grupos piloto) en la sección **Clientes**.      </td><td align="left">  Permite crear, editar y ver [grupos de clientes](create-customer-groups.md) (segmentos y grupos piloto) en la sección **Clientes**.       </td></tr>
<tr><td align="left">    **Nuevas aplicaciones**                            </td><td align="left">  Permite ver la nueva página de creación de aplicaciones, pero en realidad no permite crear nuevas aplicaciones en la cuenta.    </td><td align="left">  Permite [crear nuevas aplicaciones](create-your-app-by-reserving-a-name.md) en la cuenta mediante la reserva de nuevos nombres de aplicación, y crear envíos y enviar aplicaciones a la Tienda.     </td></tr>
<tr><td align="left">    **Nuevos conjuntos**&nbsp;*                       </td><td align="left">  Permite ver la nueva página de creación de conjuntos, pero en realidad no permite crear nuevos conjuntos en la cuenta.     </td><td align="left">  Permite crear nuevos conjuntos de productos.          </td></tr>
<tr><td align="left">    **Servicios de partners**&nbsp;*                  </td><td align="left">  Permite ver certificados para la instalación en servicios con el fin de recuperar XTokens.     </td><td align="left">  Permite administrar y ver certificados para la instalación en servicios con el fin de recuperar XTokens.       </td></tr>
<tr><td align="left">    **Cuenta de pago**                      </td><td align="left">  Permite ver la [información de la cuenta de pago](setting-up-your-payout-account-and-tax-forms.md#payout-account) en **Configuración de la cuenta**.     </td><td align="left">  Permite editar y ver la [información de la cuenta de pago](setting-up-your-payout-account-and-tax-forms.md#payout-account) en **Configuración de la cuenta**.       </td></tr>
<tr><td align="left">    **Resumen de pago**                      </td><td align="left">  Permite ver el [resumen de pago](payout-summary.md) para acceder y descargar información de informes de pago.       </td><td align="left">  Permite ver el [resumen de pago](payout-summary.md) para acceder y descargar información de informes de pago.   </td></tr>
<tr><td align="left">    **Usuarios de confianza**&nbsp;*                   </td><td align="left">  Permite ver los usuarios de confianza para recuperar XTokens.    </td><td align="left">  Permite administrar y ver los usuarios de confianza para recuperar XTokens.     </td></tr>
<tr><td align="left">    **Espacios aislados**&nbsp;*                         </td><td align="left">  Permite acceder a la página de **espacios aislados**, y ver los espacios aislados de la cuenta y cualquier configuración que se aplique a dichos espacios. No permite ver los productos ni los envíos de cada espacio aislado a menos que se otorguen los permisos de nivel de producto oportunos. </td><td align="left">  Permite acceder a la página de **espacios aislados**, y ver y administrar los espacios aislados en la cuenta, incluida la creación y la eliminación de espacios aislados y la administración de sus configuraciones. No permite ver los productos ni los envíos de cada espacio aislado a menos que se otorguen los permisos de nivel de producto oportunos.    </td></tr>
<tr><td align="left">    **Perfil fiscal**                         </td><td align="left">  Permite ver la [información del perfil fiscal y los formularios](setting-up-your-payout-account-and-tax-forms.md#tax-forms) en **Configuración de la cuenta**.     </td><td align="left">  Permite rellenar formularios fiscales y actualizar la [información del perfil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) en **Configuración de la cuenta**.     </td></tr>
<tr><td align="left">    **Cuentas de prueba**&nbsp;*                     </td><td align="left">  Permite ver las cuentas de la configuración de Xbox Live de prueba.      </td><td align="left">  Permite crear, administrar y ver las cuentas de configuración de Xbox Live de prueba.      </td></tr>
<tr><td align="left">    **Dispositivos Xbox**                        </td><td align="left">  Permite ver las consolas de desarrollo de Xbox habilitadas para la cuenta en la sección **Configuración de la cuenta**.       </td><td align="left">  Permite agregar, eliminar y ver las consolas de desarrollo de Xbox habilitadas para la cuenta en la sección **Configuración de la cuenta**.     </td></tr>
    </tbody>
    </table>

\* Los permisos marcados con un asterisco (*) conceden acceso a características que no están disponibles para todas las cuentas. Si tu cuenta no se ha habilitado para acceder a estas características, las selecciones de estos permisos no tendrán efecto alguno.   

## <a name="product-level-permissions"></a>Permisos de nivel de producto

Los permisos de esta sección se pueden conceder a todos los productos de la cuenta o se pueden personalizar para que se conceda el permiso solo a uno o varios productos específicos. Estos permisos se clasifican en cuatro categorías: **Análisis**, **Monetización**, **Publicación** y **Xbox Live**. Permite expandir cada una de estas categorías para ver los permisos individuales de cada categoría. 

Para conceder un permiso para todos los productos de la cuenta, realiza las selecciones de ese permiso (activando o desactivando la casilla para indicar **Solo lectura**, **Lectura y escritura** o **Sin acceso**) en la fila marcada con **Todos los productos**. 
 
> **Consejo** Las selecciones que se realicen para **todos los productos** se aplicarán a todos los productos actuales de la cuenta, así como a cualquier producto que se cree en la cuenta en el futuro.

Debajo de la fila **Todos los productos**, verás todos los productos de la cuenta en una fila aparte. Para conceder un permiso solo a un producto en particular, realiza tus selecciones de dicho permiso en la fila del producto en cuestión.

Cada complemento aparece en una fila aparte debajo de su producto correspondiente, junto con una fila **Todos los complementos**. Las selecciones que se realicen para **Todos los complementos** se aplicarán a todos los complementos actuales de dicho producto, así como a cualquier complemento que se cree para dicho producto en el futuro.

Ten en cuenta que algunos permisos no se pueden establecer para los complementos. Esto se debe a que no se pueden aplicar a los complementos (por ejemplo, el permiso **Comentarios del usuario**) o porque los permisos que se conceden en el nivel de producto primario se aplican a todos los complementos de dicho producto (por ejemplo, los **códigos promocionales**). Sin embargo, ten en cuenta que ningún permiso que esté disponible para los complementos se debe establecer por separado; los complementos no heredan las selecciones que se realizan para el producto principal. Por ejemplo, si quieres permitir a un usuario realizar selecciones de precios y disponibilidad para un complemento, deberías habilitar el permiso **Precios y disponibilidad** para el complemento (o para **todos los complementos**), con independencia de que hayas concedido el permiso **Precios y disponibilidad** para el producto principal. 

### <a name="analytics"></a>Análisis

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Adquisiciones**     </td><td>    Permite ver los informes de [adquisiciones](acquisitions-report.md) y de [adquisiciones de complementos](add-on-acquisitions-report.md) del producto.        </td><td>    N/D    </td><td>    N/D (la configuración para el producto principal incluye informes de adquisición de complementos)        </td><td>    N/D                         </td></tr>
    <tr><td align="left">    **Utilización** </td><td>    Permite ver el [informe de utilización](usage-report.md) del producto.     </td><td>    N/D       </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Mantenimiento** </td><td>    Permite ver el [informe Mantenimiento](health-report.md) del producto.    </td><td>    N/D     </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Comentarios del cliente**    </td><td>    Permite ver los informes [Valoración](ratings-report.md), [Críticas](reviews-report.md) y [Comentarios](feedback-report.md) del producto.       </td><td>    N/D (para responder a los comentarios o a las críticas, se debe conceder el permiso correspondiente para **ponerse en contacto con los clientes**)   </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Análisis de Xbox** </td><td>    Permite ver el informe de análisis de Xbox del producto. (Nota: este informe aún no está disponible).    </td><td>    N/D   </td><td>    N/D       </td><td>    N/D          </td></tr>
    <tr><td align="left">    **Tiempo real**   </td><td>    Permite ver el informe Tiempo real del producto.       </td><td>    N/D   </td><td>    N/D     </td><td>    N/D                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monetización

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Contacto con el cliente**  </td><td>    Permite ver las [respuestas a los comentarios de los clientes](respond-to-customer-feedback.md) y [a las opiniones de los clientes](respond-to-customer-reviews.md), siempre y cuando también se haya concedido permiso para **Comentarios del cliente**. También permite ver las [notificaciones dirigidas](send-push-notifications-to-your-apps-customers.md) que se han creado para el producto.    </td><td>    Permite [responder a los comentarios de los clientes](respond-to-customer-feedback.md) y [a las opiniones de los clientes](respond-to-customer-reviews.md), siempre y cuando también se haya concedido permiso para **Comentarios del cliente**. También permite [crear y enviar notificaciones dirigidas](send-push-notifications-to-your-apps-customers.md) del producto.                   </td><td>    N/D         </td><td>    N/D                          </td></tr>
    <tr><td align="left">    **Experimentación**</td><td>    Permite ver [experimentos (pruebas A/B)](../monetize/run-app-experiments-with-a-b-testing.md) y los datos de los experimentos del producto.   </td><td>    Permite crear, administrar y ver [experimentos (pruebas A/B)](../monetize/run-app-experiments-with-a-b-testing.md) del producto y ver datos de los experimentos.     </td><td>    N/D  </td><td>    N/D                 </td></tr>
    <tr><td align="left">    **Códigos promocionales**     </td><td>    Permite ver pedidos de [código promocional](generate-promotional-codes.md) e información sobre la utilización del producto y sus complementos, y también información sobre su utilización.         </td><td>    Permite ver, administrar y crear pedidos de [código promocional](generate-promotional-codes.md) del producto y sus complementos, y también ver información sobre su utilización.          </td><td>    N/D (la configuración relativa al producto principal se aplicará a todos los complementos)     </td><td>    N/D (la configuración relativa al producto principal se aplicará a todos los complementos)     </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>Publicación 

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Precios y disponibilidad**  </td><td>    Permite ver la página [Precios y disponibilidad](set-app-pricing-and-availability.md) de los envíos de productos.     </td><td>    Permite ver y editar la página [Precios y disponibilidad](set-app-pricing-and-availability.md) de los envíos de productos. </td><td>    Permite ver la página [Precios y disponibilidad](set-add-on-pricing-and-availability.md) de los envíos de complementos.   </td><td>    Permite ver y editar la página [Precios y disponibilidad](set-add-on-pricing-and-availability.md) de los envíos de complementos.          </td></tr>
    <tr><td align="left">    **Propiedades**   </td><td>    Permite ver la página [Propiedades](enter-app-properties.md) de envíos de productos.      </td><td>    Permite ver y editar la página [Propiedades](enter-app-properties.md) de envíos de productos.       </td><td>    Permite ver la página [Propiedades](enter-add-on-properties.md) de envíos de complementos.     </td><td>    Permite ver y editar la página [Propiedades](enter-add-on-properties.md) de envíos de complementos.               </td></tr>
    <tr><td align="left">    **Clasificaciones por edades**    </td><td>    Permite ver la página [Clasificaciones por edades](age-ratings.md) de envíos de productos.       </td><td>    Permite ver y editar la página [Clasificaciones por edades](age-ratings.md) de envíos de productos.    </td><td>    * Permite ver la página Clasificaciones por edades de envíos de complementos.          </td><td>    * Permite ver y editar la página Clasificaciones por edades de envíos de complementos.       </td></tr>
    <tr><td align="left">    **Paquetes**        </td><td>    Permite ver la página [Paquetes](upload-app-packages.md) de envíos de productos.  </td><td>    Permite ver y editar la página [Paquetes](upload-app-packages.md) de envíos de productos, incluidos los paquetes que se estén cargando.     </td><td>    * Permite ver objetivos y paquetes de familias de dispositivos (si procede) de envíos de complementos.   </td><td>    * Permite ver y editar los objetivos de las familias de dispositivos de envíos de complementos, incluidos los paquetes que se estén cargando (si corresponde).             </td></tr>
    <tr><td align="left">    **Descripciones de la Tienda**  </td><td>    Permite ver las [páginas de descripción de la Tienda](create-app-store-listings.md) de envíos de productos.  </td><td>    Permite ver y editar las [páginas de descripción de la Tienda](create-app-store-listings.md) de envíos de productos y agregar nuevas descripciones de la Tienda en idiomas distintos.     </td><td>    Permite ver las [páginas de descripción de la Tienda](create-add-on-store-listings.md) de envíos de complementos.            </td><td>    Permite ver y editar las [páginas de descripción de la Tienda](create-add-on-store-listings.md) de envíos de complementos y agregar descripciones de la Tienda en idiomas distintos.                 </td></tr>
    <tr><td align="left">    **Envío a la Tienda**     </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.           </td><td>    Permite enviar el producto a la Tienda y ver informes de certificación. Incluye los envíos nuevos y los actualizados. </td><td>No se concede ningún acceso si este permiso se establece como de solo lectura.     </td><td>    Permite enviar el complemento a la Tienda y ver informes de certificación. Incluye los envíos nuevos y los actualizados.</td></tr>
    <tr><td align="left">    **Creación de un nuevo envío**       </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.        </td><td>    Permite crear nuevos [envíos](app-submissions.md) para el producto.  </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.   </td><td>    Permite crear nuevos [envíos](add-on-submissions.md) para el complemento.        </td></tr>
    <tr><td align="left">    **Nuevos complementos**    </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura. </td><td>    Permite [crear nuevos complementos](set-your-add-on-product-id.md) para el producto. </td><td>    N/D    </td><td>    N/D        </td></tr>
    <tr><td align="left">    **Reservas de nombre**   </td><td>    Permite ver la página [Administrar nombres de aplicación](manage-app-names.md) del producto.</td><td>    Permite ver y editar la página [Administrar nombres de aplicación](manage-app-names.md) del producto, incluso reservar nombres adicionales y eliminar nombres reservados. </td><td>   * Permite ver los nombres reservados del complemento.    </td><td>   * Permite ver y editar nombres reservados del complemento.          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Configuración del servicio Xbox**&nbsp;\*    </td><td>    Permite ver la configuración relacionada con los logros, la opción multijugador, los marcadores y otra configuración de Xbox Live del producto.  </td><td>    Permite ver y editar la configuración relacionada con los logros, la opción multijugador, los marcadores y otra configuración de Xbox Live del producto.  </td><td>    N/D     </td><td>    N/D                      </td></tr>
    <tr><td align="left">    **Canales de aplicación**&nbsp;\*</td><td>    N/D  </td><td>    Permite publicar promocionales canales de vídeo en la consola Xbox para su visualización a través de OneGuide.  </td><td>  N/D </td><td> N/D </td></tr>
</tbody>
</table>

\* Los permisos marcados con un asterisco (*) conceden acceso a características que no están disponibles para todas las cuentas. Si tu cuenta no se ha habilitado para acceder a estas características, las selecciones de estos permisos no tendrán efecto alguno.  

