---
title: Migración de aplicaciones de WebLogic a Azure Virtual Machines
description: En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de WebLogic existente para que se ejecute en Azure Virtual Machines.
author: edburns
ms.author: edburns
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 4abc2383d0ff2e14cf550d96b87c75bfa6fab72e
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830703"
---
# <a name="migrate-weblogic-applications-to-azure-virtual-machines"></a>Migración de aplicaciones de WebLogic a Azure Virtual Machines

En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de WebLogic existente para que se ejecute en Azure Virtual Machines.

## <a name="pre-migration"></a>Antes de la migración

### <a name="define-what-you-mean-by-migration-complete"></a>Definición del significado de "migración completa"

Esta guía, y las ofertas de Azure Marketplace correspondientes, son un punto de partida para acelerar la migración de las cargas de trabajo de WebLogic Server a Azure. Es importante definir el ámbito del trabajo de migración. Por ejemplo, ¿está realizando una migración lift-and-shift estricta de la infraestructura existente a Azure Virtual Machines? Si es así, es posible que se sienta la tentación de realizar algunas mejoras mientras realiza la migración.

En la medida de lo posible, es mejor centrarse en un traslado lift-and-shift, teniendo en cuenta los cambios necesarios que se indican en esta guía. Defina lo que quiere decir con "migración completa" para que sepa cuándo ha alcanzado el objetivo. Cuando haya realizado la "migración completa", puede hacer una instantánea de sus máquinas virtuales, tal y como se describe en [Crear una instantánea](/azure/virtual-machines/windows/snapshot-copy-managed-disk). Cuando haya comprobado que puede realizar la restauración correctamente desde la instantánea, puede realizar las mejoras sin miedo a perder el progreso de la migración que ha conseguido hasta ahora.

### <a name="determine-whether-the-pre-built-marketplace-offers-are-a-good-starting-point"></a>Determinación de si las ofertas de Marketplace preconfiguradas son un buen punto de partida

Oracle y Microsoft se han asociado para incorporar a Azure Marketplace un conjunto de plantillas de soluciones de Azure que ofrecen un punto de partida sólido para la migración a Azure. Consulte la documentación de [Oracle Fusion Middleware](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/) para obtener la lista de las ofertas, y elija la que mejor se ajuste a su implementación existente. Puede ver la lista de ofertas [en la documentación de Oracle](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/select-required-oracle-weblogic-server-offer-azure-marketplace.html#GUID-187739C5-EE7A-47C6-B3BA-C0A0333DC398).

Si ninguna de las ofertas existentes es un buen punto de partida, tendrá que reproducir la implementación a mano con los recursos de Azure Virtual Machines. Para más información, consulte [¿Qué es IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)

### <a name="determine-whether-the-weblogic-version-is-compatible"></a>Determinación de si la versión de WebLogic es compatible

La versión existente de WebLogic debe ser compatible con la versión de las ofertas de IaaS. Esta consulta mostrará las ofertas de [WebLogic versión 12.2.1.3](https://azuremarketplace.microsoft.com/marketplace/apps?search=oracle%20weblogic%2012.2.1.3&page=1). Si la versión WebLogic existente no es compatible con esa versión, tendrá que reproducir la implementación a mano con los recursos de IaaS de Azure. Para más información, consulte [la documentación de Azure](https://azure.microsoft.com/overview/what-is-iaas/).

[!INCLUDE [inventory-server-capacity-virtual-machines](includes/migration/inventory-server-capacity-virtual-machines.md)]

[!INCLUDE [inventory-all-secrets](includes/migration/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/migration/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly](includes/migration/validate-that-the-supported-java-version-works-correctly.md)]

[!INCLUDE [inventory-jndi-resources](includes/migration/inventory-jndi-resources.md)]

[!INCLUDE [domain-configuration](includes/migration/domain-configuration.md)]

[!INCLUDE [determine-whether-session-replication-is-used](includes/migration/determine-whether-session-replication-is-used.md)]

[!INCLUDE [document-datasources](includes/migration/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/migration/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-management-over-rest-is-used](includes/migration/determine-whether-management-over-rest-is-used.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/migration/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use-virtual-machines](includes/migration/determine-whether-jms-queues-or-topics-are-in-use-virtual-machines.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/migration/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/migration/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/migration/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/migration/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/migration/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/migration/determine-whether-your-application-is-packaged-as-an-ear.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/migration/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [determine-whether-wlst-is-used](includes/migration/determine-whether-wlst-is-used.md)]

[!INCLUDE [validate-whether-and-how-the-file-system-is-used](includes/migration/validate-whether-and-how-the-file-system-is-used.md)]

[!INCLUDE [determine-the-network-topology](includes/migration/determine-the-network-topology.md)]

[!INCLUDE [account-for-the-use-of-jca-adapters-and-resource-adapters](includes/migration/account-for-the-use-of-jca-adapters-and-resource-adapters.md)]

[!INCLUDE [account-for-the-use-of-custom-security-providers-and-jaas](includes/migration/account-for-the-use-of-custom-security-providers-and-jaas.md)]

[!INCLUDE [determine-whether-weblogic-clustering-is-used](includes/migration/determine-whether-weblogic-clustering-is-used.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-used](includes/migration/determine-whether-the-java-ee-application-client-feature-is-used.md)]

## <a name="migration"></a>Migración

### <a name="select-a-weblogic-on-azure-virtual-machines-offer"></a>Selección de una oferta de WebLogic en Azure Virtual Machines

Las siguientes ofertas están disponibles para WebLogic en Azure Virtual Machines.

Durante la implementación de una oferta, se le pedirá que elija el tamaño de máquina virtual para los nodos de WebLogic Server. Es importante tener en cuenta todos los aspectos (memoria, procesador y disco) a la hora de elegir el tamaño de la máquina virtual. Para más información, consulte [la documentación de las ofertas](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/deploy-oracle-weblogic-server-administration-server-single-node.html) y la [documentación de Azure para elegir el tamaño de las máquinas virtuales](/azure/cloud-services/cloud-services-sizes-specs).

#### <a name="weblogic-server-single-node-with-no-admin-server"></a>Nodo único de WebLogic Server sin servidor de administración

Esta oferta crea una sola máquina virtual e instala WebLogic en ella, pero no configura ningún dominio, lo que resulta útil para escenarios en los que se tiene una configuración de dominio muy personalizada.

#### <a name="weblogic-server-single-node-with-admin-server"></a>Nodo único de WebLogic Server con servidor de administración

Esta oferta aprovisiona una única máquina virtual e instala WebLogic Server 12.1.2.3 en ella. Crea un dominio e inicia el servidor de administración.

#### <a name="weblogic-server-n-node-cluster"></a>Clúster de N nodos de WebLogic Server

Esta oferta crea un clúster de alta disponibilidad de máquinas virtuales de WebLogic Server.

#### <a name="weblogic-server-n-node-dynamic-cluster"></a>Clúster dinámico N nodos de WebLogic Server

Esta oferta crea un clúster dinámico escalable y de alta disponibilidad de máquinas virtuales de WebLogic Server.

### <a name="provision-the-offer"></a>Aprovisionamiento de la oferta

Una vez seleccionada la oferta con la que quiere empezar, siga las instrucciones de la [documentación de las ofertas](https://wls-eng.github.io/arm-oraclelinux-wls/) para aprovisionar dicha oferta. Asegúrese de elegir el nombre de dominio que coincida con el nombre de dominio existente. Incluso puede hacer coincidir la contraseña de dominio con la contraseña de dominio existente.

### <a name="migrate-the-domains"></a>Migración de los dominios

Una vez aprovisionada la oferta, puede examinar la configuración del dominio y seguir [esta guía](https://support.oracle.com/knowledge/Middleware/2336356_1.html) para obtener más información sobre cómo migrar los dominios.

### <a name="connect-the-databases"></a>Conexión de las bases de datos

Después de migrar los dominios, puede conectar las bases de datos siguiendo las instrucciones [de la documentación de la oferta](https://wls-eng.github.io/arm-oraclelinux-wls/#connecting-a-database-to-a-cluster). Estas instrucciones ayudan a tener en cuenta los secretos de las base de datos y las cadenas de acceso implicadas.

### <a name="account-for-keystores"></a>Cuenta de almacenes de claves

Debe tener en cuenta la migración de los almacenes de claves SSL que use la aplicación. Para más información, consulte [Configuración de almacenes de claves](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/secmg/identity_trust.html#GUID-7F03EB9C-9755-430B-8B86-17199E0C01DC).

### <a name="connect-the-jms-sources"></a>Conexión de los orígenes de JMS

Una vez conectadas las bases de datos, puede configurar JMS siguiendo las instrucciones que se indican en [Fusion Middleware: administración de recursos de JMS para Oracle WebLogic Server](https://docs.oracle.com/middleware/12213/wls/JMSAD/toc.htm), en la documentación de WebLogic.

### <a name="account-for-logging"></a>Cuenta para el registro

La configuración de registro existente se debe trasladar cuando se migre el dominio. Para más información, consulte [Configuración de los niveles de java.util.logging](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlach/taskhelp/logging/ConfigureJavaLoggingLevels.html) y [Configuración de los archivos de registro y filtrado de los mensajes de registro para Oracle WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wllog/index.html).

### <a name="migrating-your-applications"></a>Migración de las aplicaciones

Las técnicas que se usan para implementar las aplicaciones del equipo de desarrollo en servidores de prueba, de ensayo y de producción varían mucho en cada caso. En algunos casos, hay una plataforma de CI/CD muy evolucionada que permite implementar las aplicaciones en el servidor de WebLogic. En otros casos, el proceso puede ser más manual. Una ventaja de usar Azure Virtual Machines para migrar aplicaciones de WebLogic a la nube es que los procesos existentes seguirán funcionando.

Tendrá que configurar el grupo de seguridad de red que aprovisiona la oferta para poder acceder desde la canalización de CI/CD o el sistema de implementación manual. Para más información, consulte [Grupos de seguridad](/azure/virtual-network/security-overview) en la documentación de Azure.

### <a name="testing"></a>Prueba

Todas las pruebas de las aplicaciones en el contenedor deben configurarse para tener acceso a los nuevos servidores que se ejecutan en Azure. Al igual que en el caso de CI/CD, debe asegurarse de que las reglas de seguridad de red necesarias permiten que las pruebas tengan acceso a las aplicaciones implementadas en Azure. Para más información, consulte [Grupos de seguridad](/azure/virtual-network/security-overview) en la documentación de Azure.

## <a name="post-migration"></a>Después de la migración

Una vez alcanzados los objetivos de migración que se han definido [antes de la migración](#pre-migration), realice pruebas integrales de aceptación para comprobar que todo funciona según lo previsto. Entre los temas de mejoras posteriores a la migración se incluyen, entre otros:

* Usar Azure Storage para servir el contenido estático montado en las máquinas virtuales. Para más información, consulte [Conexión o desconexión de un disco de datos en una máquina virtual](/azure/lab-services/devtest-lab-attach-detach-data-disk).

* Implementar las aplicaciones en el clúster de WebLogic migrado con Azure DevOps. Para más información, consulte la [documentación de introducción a Azure DevOps](/azure/devops/get-started/?view=azure-devops).

* Mejorar la topología de red con servicios avanzados de equilibrio de carga. Para más información, consulte [Uso de servicios de equilibrio de carga en Azure](/azure/traffic-manager/traffic-manager-load-balancing-azure).

* Aproveche las identidades administradas de Azure para los secretos administrados y asigne un acceso basado en roles a los recursos de Azure. Para más información, consulte [¿Qué es Managed Identities for Azure Resources?](/azure/active-directory/managed-identities-azure-resources/overview)

* Integrar la autenticación y autorización de WebLogic Java EE con Azure Active Directory. Para más información, consulte [Guía de introducción a la integración de Azure Active Directory con las aplicaciones](/azure/active-directory/manage-apps/plan-an-application-integration).

* Usar Azure Key Vault para almacenar toda la información que funcione como un "secreto". Para más información, consulte [Conceptos básicos de Azure Key Vault](/azure/key-vault/basic-concepts).
