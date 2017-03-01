---
title: Was ist Azure Analysis Services? | Microsoft Docs
description: "Verschaffen Sie sich einen Überblick über Analysis Services in Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/27/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 653c9a04016c0157b2107710e170ba63b3a499f7
ms.openlocfilehash: 38f68d6df19d24492a234933e10d5d5cf34d1b23
ms.lasthandoff: 01/05/2017


---
# <a name="what-is-azure-analysis-services"></a>Was ist Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services bietet Unternehmen – basierend auf dem bewährten Analysemodul in Microsoft SQL Server Analysis Services – Datenmodellierungsfunktionen in der Cloud. 

Sehen Sie sich in diesem Video an, um zu erfahren, wie sich Azure Analysis Services in die allgemeinen Business Intelligence-Funktionen (BI) von Microsoft einfügt und wie Sie zudem vom Verlagern Ihrer semantischen Modelle in die Cloud profitieren können.

>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/AzureAnalysisServicesGettingStarted/player]
>
>

> [!IMPORTANT]
> Azure Analysis Services ist als **Vorschauversion** verfügbar. Einige Funktionen werden noch nicht unterstützt. Lesen Sie hierzu [Bemerkungen zur Vorschauversion](#preview-expectations) weiter unten in diesem Artikel. Und konsultieren Sie regelmäßig unseren [Azure Analysis Services-Blog](https://go.microsoft.com/fwlink/?linkid=830920) (in englischer Sprache), um die neuesten Informationen zu erhalten.
> 
> 

## <a name="built-on-sql-server-analysis-services"></a>Aufbauend auf SQL Server Analysis Services
Azure Analysis Services ist mit der SQL Server 2016 Analysis Services Enterprise Edition kompatibel, die Sie bereits kennen. Azure Analysis Services unterstützt tabellarische Modelle mit dem Kompatibilitätsgrad 1200. DirectQuery, Partitionen, Sicherheit auf Zeilenebene, bidirektionale Beziehungen und Übersetzungen werden unterstützt.

## <a name="use-the-tools-you-already-know"></a>Verwenden Sie die Tools, die Sie bereits kennen
![BI-Entwicklertools](./media/analysis-services-overview/aas-overview-dev-tools.png)

Beim Erstellen von Datenmodellen für Azure Analysis Services verwenden Sie dieselben Tools wie für SQL Server Analysis Services. Verwenden Sie zum Erstellen und Bereitstellen tabellarischer Modelle die neuesten Versionen von [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) oder [Azure Powershell](/powershell/azureps-cmdlets-docs)- und [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-Vorlagen in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connect-to-data-sources"></a>Verbindungen mit Datenquellen
Auf Servern in Azure bereitgestellte Datenmodelle unterstützen das Herstellen von Verbindungen mit lokalen Datenquellen in der Organisation oder mit Datenquellen in der Cloud. Kombinieren Sie Daten aus lokalen und Clouddatenquellen für eine hybride BI-Lösung.

![Datenquellen](./media/analysis-services-overview/aas-overview-data-sources.png)

Da sich der Server in der Cloud befindet, können Clouddatenquellen nahtlos verbunden werden. Beim Verbinden mit lokalen Datenquellen gewährleistet das [lokale Datengateway](analysis-services-gateway.md) schnelle, sichere Verbindungen mit dem Analysis Services-Server in der Cloud.  

 \* Einige Datenquellen werden in der Vorschauversion noch nicht unterstützt. Weitere Informationen hierzu finden Sie unter [Bemerkungen zur Vorschauversion](#preview-expectations) weiter unten in diesem Artikel.

## <a name="explore-your-data-from-anywhere"></a>Datenanalysen von überall aus
Sie können von jedem Standort eine Verbindung mit den Servern herstellen und [Daten abrufen](analysis-services-connect.md). Azure Analysis Services unterstützt das Herstellen einer Verbindung von Power BI Desktop, Excel, benutzerdefinierten Apps und browserbasierten Tools.

![Datenvisualisierungen](./media/analysis-services-overview/aas-overview-visualization.png)

 \* Power BI Embedded wird in der Vorschauversion noch nicht unterstützt.

## <a name="secure"></a>Sicher
#### <a name="user-authentication"></a>Benutzerauthentifizierung
Die Benutzerauthentifizierung für Azure Analysis Services wird per [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md) durchgeführt. Wenn sich Benutzer bei einer Azure Analysis Services-Datenbank anmelden, verwenden sie eine Organisationskontoidentität mit Zugriff auf die betreffende Datenbank. Diese Benutzeridentitäten müssen Mitglied des standardmäßigen Azure Active Directory für das Abonnement sein, in dem sich der Azure Analysis Services-Server befindet. Die [Verzeichnisintegration](https://technet.microsoft.com/library/jj573653.aspx) zwischen AAD und einer lokalen Instanz von Active Directory bietet eine hervorragende Möglichkeit, den lokalen Benutzern Zugriff auf eine Azure Analysis Services-Datenbank zu ermöglichen, sie ist jedoch nicht für alle Szenarien erforderlich.

Benutzer melden sich mit dem Benutzerprinzipalnamen (User Principal Name, UPN) ihres Kontos und ihrem Kennwort an. Bei Synchronisierung mit einem lokalen Active Directory ist der UPN des Benutzers häufig die E-Mail-Adresse des Benutzers bei der Organisation.

Berechtigungen zum Verwalten der Azure Analysis Services-Serverressource werden durch Zuweisen von Benutzern zu Rollen im Azure-Abonnement organisiert. Abonnementadministratoren verfügen standardmäßig über Besitzerberechtigungen für die Serverressource in Azure. Zusätzliche Benutzer können mithilfe von Azure Resource Manager hinzugefügt werden.

#### <a name="data-security"></a>Datensicherheit
Azure Analysis Services nutzt Azure Blob Storage zum Beibehalten von Speicher und Metadaten für Analysis Services-Datenbanken. Datendateien im Blob werden mithilfe der serverseitigen Verschlüsselung (Server Side Encryption, SSE) von Azure Blob verschlüsselt. Bei Verwendung des Direktabfragemodus werden nur Metadaten gespeichert. Der Zugriff auf die eigentlichen Daten erfolgt zum Zeitpunkt der Abfrage über die Datenquelle.

#### <a name="on-premises-data-sources"></a>Lokale Datenquellen
Das Installieren und Konfigurieren eines [lokalen Datengateways](analysis-services-gateway.md) ermöglicht den sicheren Zugriff auf die lokalen Daten in der Organisation. Gateways bieten Zugriff auf Daten im Direktabfragemodus und im speicherinternen Modus. Wenn ein Azure Analysis Services-Modell eine Verbindung mit einer lokalen Datenquelle herstellt, wird eine Abfrage zusammen mit den verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Der Gatewayclouddienst analysiert die Abfrage und überträgt die Anforderung per Push an einen Azure Service Bus. Das lokale Gateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab. Das Gateway ruft dann die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt für die Ausführung eine Verbindung mit der Datenquelle her. Anschließend werden die Ergebnisse aus der Datenquelle zurück an das Gateway und dann an die Azure Analysis Services-Datenbank gesendet.

Azure Analysis Services unterliegt den [Microsoft Online Services-Nutzungsbedingungen](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und der [Microsoft Online Services-Datenschutzerklärung](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Weitere Informationen zur Sicherheit in Azure finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Azure Analysis Services ist einfach einzurichten und zu verwalten. Hier finden Sie alle Informationen, die Sie zum Erstellen und Verwalten eines Servers benötigen. Wenn Sie ein Datenmodell für die Bereitstellung auf dem Server erstellen, erfolgt dies auf ähnliche Weise wie das Erstellen eines Datenmodells, das Sie auf einem lokalen Server bereitstellen. Unter [Analysis Services auf MSDN](https://msdn.microsoft.com/library/bb522607.aspx) finden Sie eine umfassende Bibliothek mit Artikeln zu Konzepten, Vorgehensweisen, Referenzartikel und Tutorials.

Unter [Azure Analysis Services auf Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services) bieten wir zudem eine Reihe hilfreicher Videos (in englischer Sprache).

Dinge ändern sich schnell. Im [Azure Analysis Services-Blog](https://go.microsoft.com/fwlink/?linkid=830920) (in englischer Sprache) erhalten Sie immer die neuesten Informationen.

## <a name="community"></a>Community
Analysis Services verfügt über eine dynamische Community von Benutzern. Beteiligen Sie sich an der Diskussion im [Azure Analysis Services-Forum](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Feedback
Haben Sie Vorschläge oder Funktionsanfragen? Hinterlassen Sie Kommentare im [Azure Analysis Services-Feedbackforum](https://aka.ms/azureanalysisservicesfeedback) (in englischer Sprache).

Haben Sie Vorschläge zur Dokumentation? Sie können am Ende jedes Artikels mit Disqus Kommentare hinzufügen.

## <a name="preview-expectations"></a>Bemerkungen zur Vorschauversion
Azure Analysis Services ist derzeit als Vorschauversion verfügbar. Es gibt einige Dinge, die Sie beachten sollten.

##### <a name="server-modes"></a>Servermodi
Azure Analysis Services unterstützt derzeit den tabellarischen Modus für tabellarische Modelle mit dem Kompatibilitätsgrad 1200. Der Modus „Mehrdimensional und Data Mining“ und der Modus „PowerPivot für SharePoint“ werden nicht unterstützt.

##### <a name="data-sources"></a>Datenquellen
Für die Vorschau werden in tabellarischen Modellen mit dem Kompatibilitätsgrad 1200, die auf einem Azure Analysis Services-Server bereitgestellt sind, die folgenden Datenquellen unterstützt.

| **Cloud** | **Lokal** |
| --- | --- |
| SQL database |SQL Server |
| SQL Data Warehouse |APS |
| Oracle | Oracle |
| Teradata | |

### <a name="data-source-providers"></a>Datenquellenanbieter
Datenmodelle in Azure Analysis Services erfordern zum Verbinden mit Datenquellen möglicherweise andere Datenanbieter als Datenmodelle in SQL Server Analysis Services. Die Datenanbieteranforderungen hängen davon ab, ob die Datenquelle lokal ist oder sich in der Cloud befindet, sowie vom Typ des Datenmodells (speicherintern oder Direktabfrage). Weitere Informationen finden Sie unter [Datenquellenverbindungen](analysis-services-datasource.md).

### <a name="client-connections"></a>Clientverbindungen
Power BI Embedded wird in der Vorschauversion noch nicht unterstützt.

Excel-Arbeitsmappen, die über eine Liveverbindung mit einem Azure Analysis Services-Server verfügen und auf OneDrive oder in SharePoint Online gespeichert sind, werden nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt mehr über Azure Analysis Services wissen, können Sie loslegen. Erfahren Sie, wie Sie in Azure [einen Server erstellen](analysis-services-create-server.md) und [auf ihm ein tabellarisches Modell bereitstellen](analysis-services-deploy.md).


