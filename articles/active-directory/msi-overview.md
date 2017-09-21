---
title: "Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory"
description: "Ein Überblick über die verwaltete Dienstidentität für Azure-Ressourcen."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 09/14/2017
ms.author: skwan
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 891a895419a4fe882e01495231f33a1d79eb42a9
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

#  <a name="managed-service-identity-msi-for-azure-resources"></a>Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine gängige Herausforderung beim Erstellen von Cloudanwendungen ist die Verwaltung der Anmeldeinformationen, die für die Authentifizierung bei Clouddiensten im Code enthalten sein müssen. Der Schutz dieser Anmeldeinformationen ist eine wichtige Aufgabe. Im Idealfall werden sie nie auf Entwicklerarbeitsstationen angezeigt oder in die Quellcodeverwaltung eingecheckt. Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mithilfe von MSI (Managed Service Identity, verwaltete Dienstidentität) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

## <a name="how-does-it-work"></a>Funktionsweise

Wenn Sie die verwaltete Dienstidentität für einen Azure-Dienst aktivieren, erstellt Azure in dem Azure AD-Mandanten, der von Ihrem Azure-Abonnement verwendet wird, automatisch eine Identität für die Dienstinstanz.  Im Hintergrund stellt Azure der Dienstinstanz die Anmeldeinformationen für die Identität bereit.  Der Code kann dann eine lokale Anforderung zum Abrufen von Zugriffstokens für Dienste senden, die die Azure AD-Authentifizierung unterstützen.  Azure übernimmt die Weitergabe der von der Dienstinstanz verwendeten Anmeldeinformationen.  Wenn die Dienstinstanz gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.

Es folgt ein Beispiel für die Funktionsweise der verwalteten Dienstidentität mit virtuellen Azure-Computern.

![Beispiel für MSI für virtuelle Computer](./media/msi-vm-example.png)

1. Azure Resource Manager empfängt eine Nachricht zur Aktivierung von MSI auf einem virtuellen Computer.
2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD, um die Identität des virtuellen Computers darzustellen. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.
3. Azure Resource Manager konfiguriert die Details des Dienstprinzipals in der MSI-VM-Erweiterung des virtuellen Computers.  Dieser Schritt umfasst das Konfigurieren der Client-ID und des Zertifikats, die von der Erweiterung zum Abrufen von Zugriffstokens von Azure AD verwendet werden.
4. Da die Dienstprinzipalidentität des virtuellen Computers nun bekannt ist, kann diesem der Zugriff auf Azure-Ressourcen erteilt werden.  Müsste Ihr Code zum Beispiel Azure Resource Manager aufrufen, würden Sie dem Dienstprinzipal des virtuellen Computers die entsprechende Rolle mithilfe der rollenbasierten Zugriffssteuerung (RBAC) in Azure AD zuweisen.  Wenn Ihr Code Key Vault aufrufen müsste, würden Sie dem Code den Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault gewähren.
5. Der auf dem virtuellen Computer ausgeführte Code fordert ein Token von einem lokalen Endpunkt an, der von der MSI-VM-Erweiterung gehostet wird: http://localhost:50342/oauth2/token.  Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Wenn der Code beispielsweise bei Azure Resource Manager authentifiziert werden soll, würden Sie Folgendes verwenden: „resource=https://management.azure.com/“.
6. Die MSI-VM-Erweiterung verwendet die konfigurierte Client-ID und das Zertifikat, um ein Zugriffstoken von Azure AD anzufordern.  Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

Jeder Azure-Dienst, der verwaltete Dienstidentitäten unterstützt, verfügt über eine eigene Methode dafür, wie der Code ein Zugriffstoken erhält. In den Tutorials zu den jeweiligen Diensten finden Sie die einzelnen Methoden zum Abrufen eines Tokens.

## <a name="which-azure-services-support-managed-service-identity"></a>Welche Azure-Dienste unterstützen verwaltete Dienstidentitäten?

Azure-Dienste, die verwaltete Dienstidentitäten unterstützen, können MSI zur Authentifizierung bei Diensten verwenden, die die Azure AD-Authentifizierung unterstützen.  MSI und die Azure AD-Authentifizierung werden derzeit in Azure integriert.  Sie sollten sich regelmäßig über Aktualisierungen informieren.

### <a name="azure-services-that-support-managed-service-identity"></a>Azure-Dienste, die verwaltete Dienstidentitäten unterstützen

Die folgenden Azure-Dienste unterstützen verwaltete Dienstidentitäten.

| Dienst | Status | Date |
| --- | --- | --- |
| Azure Virtual Machines | Vorschau | September 2017 |
| Skalierungsgruppen für virtuelle Azure-Computer | Vorschau | September 2017 |
| Azure App Service | Vorschau | September 2017 |
| Azure-Funktionen | Vorschau | September 2017 |

### <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-Dienste, die die Azure AD-Authentifizierung unterstützen

Die folgenden Dienste unterstützen die Azure AD-Authentifizierung und wurden mit Clientdiensten getestet, die verwaltete Dienstidentitäten verwenden.

| Dienst | Ressourcen-ID | Status | Date |
| --- | --- | --- | --- |
| Azure Resource Manager | https://management.azure.com/ | Verfügbar | September 2017 |
| Azure Key Vault | https://vault.azure.net/ | Verfügbar | September 2017 |
| Azure SQL | https://database.windows.net/ | Verfügbar | September 2017 |
| Azure Data Lake | https://datalake.azure.net/ | Verfügbar | September 2017 |

## <a name="how-much-does-managed-service-identity-cost"></a>Wie viel kosten verwaltete Dienstidentitäten?

Verwaltete Dienstidentitäten sind im Lieferumfang von Azure Active Directory Free enthalten. Dies ist die Standardbereitstellung für Azure-Abonnements.  Für verwaltete Dienstidentitäten fallen keine zusätzlichen Kosten an.

## <a name="support-and-feedback"></a>Support und Feedback

Wir freuen uns darauf, von Ihnen zu hören.

* Stellen Sie Fragen zu Stack Overflow mit dem Tag [azure-msi](http://stackoverflow.com/questions/tagged/azure-msi).
* Fordern Sie Funktionen an, oder geben Sie Feedback im [Azure AD-Feedbackforum für Entwickler](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences).

## <a name="try-managed-service-identity"></a>Verwaltete Dienstidentitäten testen

Nachdem Sie die Grundlagen nun kennen, können Sie sich in einem Tutorial zu verwalteten Dienstidentitäten darüber informieren, wie Sie auf andere Azure-Ressourcen zugreifen.

- [Zugreifen auf Azure Resource Manager mit einer Windows-VM-MSI](msi-tutorial-windows-vm-access-arm.md)
- [Zugreifen auf Azure Resource Manager mit einer Linux-VM-MSI](msi-tutorial-linux-vm-access-arm.md)
- [Zugreifen auf Azure Storage mit einer Windows-VM-MSI](msi-tutorial-windows-vm-access-storage.md)
- [Zugreifen auf Azure Storage mit einer Linux-VM-MSI](msi-tutorial-linux-vm-access-storage.md)
- [Zugreifen auf eine Nicht-Azure-Ressource mit einer Windows-VM-MSI](msi-tutorial-windows-vm-access-nonaad.md)
- [Zugreifen auf eine Nicht-Azure-Ressource mit einer Linux-VM-MSI](msi-tutorial-linux-vm-access-nonaad.md)
- [Verwaltete Dienstidentitäten mit Azure App Service oder Azure Functions](/azure/app-service/app-service-managed-service-identity)


















