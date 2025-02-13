---
title: Intruducir las GitHub Actions a tu empresa
shortTitle: Introduce Actions
intro: 'Puedes planear cómo implementar las {% data variables.product.prodname_actions %} en tu empresa.'
versions:
  ghec: '*'
  ghes: '*'
  ghae: '*'
type: how_to
topics:
  - Actions
  - Enterprise
ms.openlocfilehash: ddd394e589b3866e80ba024f99bd2394d1743299
ms.sourcegitcommit: 9af8891fea10039b3374c76818634e05410e349d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2022
ms.locfileid: '148191866'
---
## Acerca de {% data variables.product.prodname_actions %} para empresas

{% data reusables.actions.about-actions %} Con {% data variables.product.prodname_actions %}, tu empresa puede automatizar, personalizar y ejecutar tus flujos de trabajo de desarrollo de software como las pruebas y despliegues. Para obtener más información, consulte "[Acerca de los datos {% data variables.product.prodname_actions %} para empresas](/admin/github-actions/getting-started-with-github-actions-for-your-enterprise/about-github-actions-for-enterprises)".

![Diagrama de los jobs que se ejecutan en los ejecutores auto-hospedados](/assets/images/help/images/actions-enterprise-overview.png)

Antes de que incluyas las {% data variables.product.prodname_actions %} en una empresa grande, primero necesitas planear tu adopción y tomar las decisiones de cómo tu empresa utilizará {% data variables.product.prodname_actions %} para apoyar de la mejor forma a tus necesidades únicas.

## Gobernanza y cumplimiento

Deberías crear un plan que rija el uso de las {% data variables.product.prodname_actions %} en tu empersa y logre tus obligaciones de cumplimiento.

Determina qué acciones {% ifversion actions-workflow-policy %}y flujos de trabajo reutilizables{% endif %} podrán usar los desarrolladores. {% ifversion ghes %}En primer lugar, decide si habilitarás el acceso a las acciones {% ifversion actions-workflow-policy %}y los flujos de trabajo reutilizables{% endif %} desde fuera de la instancia. {% data reusables.actions.access-actions-on-dotcom %} Para obtener más información, consulte "[Acerca del uso de acciones en su empresa](/admin/github-actions/managing-access-to-actions-from-githubcom/about-using-actions-in-your-enterprise)."

A continuación,{% else %}En primer lugar,{% endif %} decide si permitirás acciones {% ifversion actions-workflow-policy %}y flujos de trabajo reutilizables{% endif %} de terceros que no se hayan creado en {% data variables.product.company_short %}. Puedes configurar las acciones {% ifversion actions-workflow-policy %}y los flujos de trabajo reutilizables{% endif %} que pueden ejecutarse en el nivel de repositorio, organización y empresa, y puedes optar por permitir solo las acciones creadas en {% data variables.product.company_short %}. Si permites las acciones de terceros{% ifversion actions-workflow-policy %} y los flujos de trabajo reutilizables{% endif %}, puedes limitar las acciones permitidas a las de creadores verificados o a una lista específica de acciones{% ifversion actions-workflow-policy %} y flujos de trabajo reutilizables{% endif %}. Para obtener más información, consulte "[Administrar la configuración de {% data variables.product.prodname_actions %} para un repositorio](/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#managing-github-actions-permissions-for-your-repository)", "[Deshabilitar o limitar {% data variables.product.prodname_actions %} para su organización](/organizations/managing-organization-settings/disabling-or-limiting-github-actions-for-your-organization#managing-github-actions-permissions-for-your-organization)" y "[Aplicar directivas para {% data variables.product.prodname_actions %} en su empresa](/admin/policies/enforcing-policies-for-your-enterprise/enforcing-policies-for-github-actions-in-your-enterprise#enforcing-a-policy-to-restrict-the-use-of-github-actions-in-your-enterprise)".

{% ifversion actions-workflow-policy %} ![Captura de pantalla de las directivas de {% data variables.product.prodname_actions %}](/assets/images/help/organizations/enterprise-actions-policy-with-workflows.png) {%- else %} ![Captura de pantalla de las directivas de {% data variables.product.prodname_actions %}](/assets/images/help/organizations/enterprise-actions-policy.png) {%- endif %}

{% ifversion ghec or ghes > 3.4 %} Plantéate combinar OpenID Connect (OIDC) con los flujos de trabajo reutilizables para aplicar implementaciones continuas en todo el repositorio, la organización o la empresa. Puedes hacerlo si defines las condiciones de confianza en los roles de la nube con base en los flujos reutilizables. Para obtener más información, consulte "["Uso de OpenID Connect con flujos de trabajo reutilizables"](/actions/deployment/security-hardening-your-deployments/using-openid-connect-with-reusable-workflows)".
{% endif %}

Puedes acceder a la información sobre la actividad relacionada con las {% data variables.product.prodname_actions %} en las bitácoras de auditoría de tu empresa. Si tus necesidades de negocio requieren que retengas esta información más tiempo que los datos de registro de auditoría, planea cómo exportarás y almacenarás estos datos fuera de {% data variables.product.prodname_dotcom %}. Para obtener más información, consulta {% ifversion ghec %}"[Exportación de la actividad del registro de auditoría de la empresa](/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/exporting-audit-log-activity-for-your-enterprise)" y "[Streaming del registro de auditoría para la empresa](/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/streaming-the-audit-log-for-your-enterprise)".{% else %}{% ifversion audit-log-streaming %}"[Streaming del registro de auditoría para la empresa](/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/streaming-the-audit-log-for-your-enterprise)" y {% endif %}"[Reenvío de registros](/admin/monitoring-activity-in-your-enterprise/exploring-user-activity/log-forwarding)".{% endif %}

![Entradas de la bitácora de auditoría](/assets/images/help/repository/audit-log-entries.png)

## Seguridad

Deberías planear tu enfoque sobre el fortalecimiento de seguridad para las {% data variables.product.prodname_actions %}.

### Fortalecer la seguridad de los flujos de trabajo y repositorios individuales

Haz un plan para requerir buenas prácticas de seguridad para las personas que utilizan las características de {% data variables.product.prodname_actions %} dentro de tu empresa. Para obtener más información sobre estas prácticas, consulte "[Protección de seguridad de {% data variables.product.prodname_actions %}](/actions/security-guides/security-hardening-for-github-actions)."

También puedes fomentar la reutilización de flujos de trabajo que ya se hayan evaluado en su seguridad. Para obtener más información, consulte "[Innersourcing](#innersourcing)".

### Asegurar el acceso a los secretos y recursos de despliegue

Deberías planear dónde almacenarás tus secretos. Te recomendamos almacenar secretos en {% data variables.product.prodname_dotcom %}, pero podrías elegir almacenarlos en un proveedor de servicios en la nube.

En {% data variables.product.prodname_dotcom %}, puedes almacenar secretos a nivel de repositorio u organización. Los secretos a nivel de repositorio pueden limitarse a los flujos de trabajo en ciertos ambientes, tales como los de producción o de pruebas. Para obtener más información, consulte "[Secretos cifrados](/actions/security-guides/encrypted-secrets)".

![Captura de pantalla de una lista de secretos](/assets/images/help/settings/actions-org-secrets-list.png) Debería considerar agregar protección manual para las aprobaciones en el caso de los entornos confidenciales para que los flujos de trabajo deban aprobarse antes de obtener acceso a los secretos de los entornos. Para obtener más información, consulte "[Uso de entornos para la implementación](/actions/deployment/targeting-different-environments/using-environments-for-deployment)".

### Consideraciones de seguridad para las acciones de terceros

Existe un riesgo significativo en suministrar acciones desde repositorios de terceros en {% data variables.product.prodname_dotcom %}. Si permites cualquier acción de terceros, deberías crear lineamientos internos que motiven a tu equipo a seguir las mejores prácticas, tales como fijar acciones al SHA de confirmación completo. Para obtener más información, consulte "[Uso de acciones de terceros](/actions/security-guides/security-hardening-for-github-actions#using-third-party-actions)".

## Innersourcing

Piensa cómo tu empresa puede utilizar características de {% data variables.product.prodname_actions %} para la automatización de innersource. El innersourcing es una forma de incorporar los beneficios de las metodologías de código abierto en tu ciclo de desarrollo de software interno. Para obtener más información, consulte [Introducción a innersource](https://resources.github.com/whitepapers/introduction-to-innersource/) en Recursos de {% data variables.product.company_short %}.

{% data reusables.actions.internal-actions-summary %}

{% ifversion ghec or ghes > 3.3 or ghae > 3.3 %} {% data reusables.actions.reusable-workflows-enterprise-beta %} Con los flujos de trabajo reutilizables, tu equipo puede activar un flujo de trabajo desde otro sin que se produzca una duplicación exacta. Los flujos de trabajo reutilizables promueven las mejores prácticas, ayudando a tu equipo a utilizar flujos de trabajo que estén bien diseñados y ya se hayan probado. Para obtener más información, consulte "[Reutilización de flujos de trabajo](/actions/learn-github-actions/reusing-workflows)".
{% endif %}

Para proporcionar un lugar inicial para que los desarrolladores creen flujos de trabajo nuevos, puedes utilizar flujos de trabajo inicial. Esto no solo ahorra tiempo a tus desarrolladores, sino que también promueve la consistencia y las mejores prácticas a lo largo de tu empresa. Para obtener más información, consulte "[Creación de flujos de trabajo de inicio para la organización](/actions/learn-github-actions/creating-starter-workflows-for-your-organization)".

{% ifversion not internal-actions %} Cada vez que los desarrolladores de flujos de trabajo quieran usar una acción que esté almacenada en un repositorio privado, deberán configurar el flujo de trabajo para que primero clone el repositorio. Para reducir la cantidad de repositorios que deben clonarse, considera agrupar las acciones que se utilizan más comúnmente en un solo repositorio. Para obtener más información, consulte "[Acerca de las acciones personalizadas](/actions/creating-actions/about-custom-actions#choosing-a-location-for-your-action)".
{% endif %}

## Administración de recursos

Debes planear cómo administrarás los recursos requeridos para utilizar las {% data variables.product.prodname_actions %}.

{% ifversion ghes %}
### Requisitos de hardware

Podrías necesitar mejorar los recursos de CPU y de memoria para que {% data variables.location.product_location %} controle la carga desde {% data variables.product.prodname_actions %} sin provocar una pérdida de rendimiento. Para obtener más información, consulte "[Introducción a {% data variables.product.prodname_actions %} para {% data variables.product.prodname_ghe_server %}](/admin/github-actions/getting-started-with-github-actions-for-your-enterprise/getting-started-with-github-actions-for-github-enterprise-server#review-hardware-requirements)".
{% endif %}

### Ejecutores

Los flujos de trabajo de las {% data variables.product.prodname_actions %} requieren ejecutores.{% ifversion ghec %} Puedes eleigr utilizar ejecutores hospedados en {% data variables.product.prodname_dotcom %} o ejecutores auto-hospedados. Los ejecutores hospedados en {% data variables.product.prodname_dotcom %} son convenientes porque los administra {% data variables.product.company_short %}, quien maneja el mantenimiento y mejoras por ti. Sin embargo, podrías querer considerar los ejecutores auto-hospedados si necesitas ejecutar un flujo de trabajo que accederá a los recursos tras tu cortafuegos o si quieres tener más control sobre los recursos, configuración o ubicación geográfica de tus máquinas de ejecutores. Para obtener más información, consulte "[Acerca de los datos ejecutores hospedados en {% data variables.product.prodname_dotcom %}](/actions/using-github-hosted-runners/about-github-hosted-runners)" y "[Acerca de los ejecutores autohospedados](/actions/hosting-your-own-runners/about-self-hosted-runners)".{% else %} Tendrá que hospedar sus propios ejecutores mediante la instalación de la aplicación de ejecutor autohospedado de {% data variables.product.prodname_actions %} en sus máquinas. Para obtener más información, consulte "[Acerca de los ejecutores autohospedados](/actions/hosting-your-own-runners/about-self-hosted-runners)."{% endif %}

{% ifversion ghec %}Si estás utilizando ejecutores auto-hospedados, tienes que decidir si quieres utilizar máquinas físicas, virtuales o contenedores.{% else %}Decide si quieres utilizar máquinas físicas, virtuales o contenedores para tus ejecutores auto-hospedados.{% endif %} Las máquinas físicas conservarán los restos de los jobs anteriores, así como las máquinas virtuales, a menos de que utilices una imagen nueva para cada job o que limpies las máquinas después de cada ejecución de un job. Si eliges los contenedores, debes estar consciente de que la actualización automática del ejecutor apagará al contenedor, lo cual puede ocasionar que los flujos de trabajo fallen. Deberías idear una solución para esto al prevenir las actualizaciones automáticas u omitir el comando para eliminar al contenedor.

También tienes que decidir dónde agregar cada ejecutor. Puedes agregar un ejecutor auto-hospedado a un repositorio individual o puedes hacerlo disponible para toda una organización en toda tu empresa. El agregar ejecutores a nivel de empresa u organización permite compartir ejecutores, lo cual podría reducir el tamaño de tu infraestructura para ejecutores. Puedes utilizar políticas para limitar el acceso a los ejecutores auto-hospedados a nivel de empresa y organización si asignas grupos de ejecutores a los repositorios u organizaciones específicas. Para obtener más información, consulte "[Agregar ejecutores autohospedados](/actions/hosting-your-own-runners/adding-self-hosted-runners)" y "[Administración del acceso a ejecutores autohospedados mediante grupos](/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups)".

{% ifversion ghec or ghes %} Deberías plantearse usar el escalado automático para incrementar o reducir automáticamente la cantidad de ejecutores autohospedados disponibles. Para obtener más información, consulte "[Escalado automático con ejecutores autohospedados](/actions/hosting-your-own-runners/autoscaling-with-self-hosted-runners)".
{% endif %}

Finalmente, deberías considerar el fortalecimiento de seguridad para los ejecutores auto-hospedados. Para obtener más información, consulte "[Protección de seguridad de {% data variables.product.prodname_actions %}](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

### Storage

{% data reusables.actions.about-artifacts %} Para obtener más información, consulte "[Almacenamiento de datos de flujos de trabajo como artefactos](/actions/advanced-guides/storing-workflow-data-as-artifacts)". 

{% ifversion actions-caching %}{% data variables.product.prodname_actions %} también tiene un sistema de almacenamiento en caché que puedes usar para almacenar en caché las dependencias a fin de acelerar las ejecuciones de flujos de trabajo. Para obtener más información, consulta "[Almacenamiento en caché de dependencias para acelerar los flujos de trabajo](/actions/using-workflows/caching-dependencies-to-speed-up-workflows)".{% endif %}

{% ifversion ghes %} Debes configurar el almacenamiento de blobs externo para artefactos de flujo de trabajo{% ifversion actions-caching %}, cachés{% endif %} y otros registros de flujo de trabajo. Decide qué proveedor de almacenamiento compatible utilizará tu empresa. Para obtener más información, consulte "[Introducción a {% data variables.product.prodname_actions %} para {% data variables.product.product_name %}](/admin/github-actions/getting-started-with-github-actions-for-your-enterprise/getting-started-with-github-actions-for-github-enterprise-server#external-storage-requirements)".
{% endif %}

{% ifversion ghec or ghes %}

Puedes usar la configuración de directiva para {% data variables.product.prodname_actions %} con el fin de personalizar el almacenamiento de artefactos de flujo de trabajo{% ifversion actions-caching %}, cachés{% endif %} y la retención de registros. Para más información, vea "[Aplicación de directivas para {% data variables.product.prodname_actions %} en la empresa](/admin/policies/enforcing-policies-for-your-enterprise/enforcing-policies-for-github-actions-in-your-enterprise)".

{% endif %}

{% ifversion ghec %} Su suscripción incluye algo de almacenamiento, pero si agrega almacenamiento adicional, su factura se modificará. Deberías hacer planes para estos costos. Para obtener más información, consulte "[Acerca de la facturación de {% data variables.product.prodname_actions %}](/billing/managing-billing-for-github-actions/about-billing-for-github-actions)".
{% endif %}

## Seguimiento del uso

Debes considerar hacer un plan para rastrear el uso que tu empresa tiene para las {% data variables.product.prodname_actions %}, tal como qué tan a menudo se ejecutan los flujos de trabajo, cuántas de estas ejecuciones están pasando y fallando y qué repositorios están utilizando cuáles flujos de trabajo.

{% ifversion ghec %} Puede ver los detalles de almacenamiento básicos y el uso de la transferencia de datos de las {% data variables.product.prodname_actions %} para cada organización de su empresa desde la configuración de facturación. Para obtener más información, consulte "[Visualización del uso de {% data variables.product.prodname_actions %}](/billing/managing-billing-for-github-actions/viewing-your-github-actions-usage#viewing-github-actions-usage-for-your-enterprise-account)".

Para obtener datos de uso más detallados, puedes{% else %}Puedes{% endif %} utilizar webhooks para suscribirte a la información sobre los jobs y las ejecuciones de los flujos de trabajo. Para obtener más información, consulte "[Acerca de los webhooks](/developers/webhooks-and-events/webhooks/about-webhooks)".

Haz un plan de cómo tu empresa podrá pasar la información de estos webhooks a un sistema para archivar datos. Puedes considerar utilizar "CEDAR.GitHub.Collector", una herramienta de código abierto que recolecta y procesa datos de webhook desde {% data variables.product.prodname_dotcom %}. Para obtener más información, consulte el [repositorio`Microsoft/CEDAR.GitHub.Collector`](https://github.com/microsoft/CEDAR.GitHub.Collector/).

También deberías planear cómo habilitarás a tus equipos para obtener los datos que necesitan desde tu sistema de archivado.
