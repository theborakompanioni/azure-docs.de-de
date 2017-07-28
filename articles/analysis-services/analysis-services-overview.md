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
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/03/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: bf56aa4bafd929010bd916d09f7025ad76823d1f
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="what-is-azure-analysis-services"></a>Was ist Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services bietet professionelle Datenmodellierung in der Cloud. Es ist eine vollständig verwaltete PaaS-Lösung (Platform-as-a-Service), die umfassend in Azure-Datenplattformdienste integriert ist. Dank eines hochgradig optimierten OLAP-Datenanalysemoduls stellt Analysis Services eine umfangreiche Semantikmodellschicht zwischen umfangreichen, komplexen und oftmals unterschiedlichen Datenquellen bereit. Durch Zusammenziehen der Daten können Sie das Semantikmodell definieren und hochgradig angepasste, äußerst leistungsfähige Analysen für umfangreiche interaktive Analyseumgebungen in modernen Clienttools erstellen.

![Datenquellen](./media/analysis-services-overview/aas-overview-data-sources.png)


In [diesem Video](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) erfahren Sie, wie sich Azure Analysis Services in die allgemeinen Business Intelligence-Funktionen (BI) von Microsoft einfügt und wie Sie von der Verlagerung Ihrer Datenmodelle in die Cloud profitieren.

<!--
>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>
-->

## <a name="built-on-sql-server-analysis-services"></a>Aufbauend auf SQL Server Analysis Services
Azure Analysis Services ist mit der SQL Server Analysis Services Enterprise Edition kompatibel, die Sie bereits kennen. Azure Analysis Services unterstützt tabellarische Modelle mit dem Kompatibilitätsgrad 1200 und 1400. Partitionen, Sicherheit auf Zeilenebene, bidirektionale Beziehungen und Übersetzungen werden unterstützt. Dank In-Memory- und DirectQuery-Modi können selbst Abfragen für umfangreiche und komplexe Datasets blitzschnell ausgeführt werden.

Tabellarische Modelle ermöglichen eine schnelle Entwicklung und sind hochgradig anpassbar. Für Entwickler enthalten tabellarische Modelle das Tabellenobjektmodell (TOM) zur Beschreibung von Modellobjekten. TOM wird in JSON über die [Skriptsprache für Tabellenmodelle (Tabular Model Scripting Language, TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) und die AMO-Datendefinitionssprache über den Namespace [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) verfügbar gemacht.

Neue Features in tabellarischen Modellen mit Kompatibilitätsgrad 1400 unterstützen Detailzeilen, Sicherheit auf Objektebene, unregelmäßige Hierarchien, eine moderne Datenabruffunktion in SSDT für Datenkonnektivität und viele weitere Verbesserungen. Und da die zugrunde liegenden Modellmetadaten gleich sind, lassen sich bereits vorhandene lokale Tabellenmodelllösungen problemlos in die Cloud migrieren.


## <a name="better-with-azure"></a>Besser mit Azure
Azure Analysis Services arbeitet mit vielen Azure-Datendiensten zusammen und ermöglicht so die Erstellung komplexer Analyselösungen.

Azure Analysis Services kann Daten aus Azure SQL-Datenbank, Azure SQL Data Warehouse und Azure Blob Storage nutzen. Sie können Data Warehouse-Lösungen für Unternehmen in Azure unter Verwendung eines Hub-and-Spoke-Modells erstellen, sodass sich das SQL Data Warehouse in der Mitte befindet und von mehreren BI-Modellen für unterschiedliche Geschäftsgruppen oder Themenbereiche umgeben ist.

Mit Azure Data Factory können Sie die Datenbewegungen und -transformationen orchestrieren – eine der Kernfunktionen jeder professionellen BI-/Analyselösung. Azure Analysis Services kann in jede beliebige Azure Data Factory-Pipeline integriert werden. Hierzu muss lediglich eine Aktivität hinzugefügt werden, die Daten in das Modell lädt. Azure Automation und Azure Functions können ebenfalls für einfache Modellorchestrierungsaufgaben mit benutzerdefiniertem Code verwendet werden.

Darüber hinaus ist Azure Analysis Services auch umfassend in Azure Active Directory integriert und bietet sicheren, rollenbasierten Zugriff auf Ihre wichtigen Daten.

## <a name="pricing"></a>Preise
Azure Analysis Services ist in den Tarifen „Developer“, „Basic“ und „Standard“ erhältlich. Die Plankosten in den einzelnen Tarifen sind jeweils abhängig von Verarbeitungsleistung, QPUs und Arbeitsspeichergröße. Bei der Servererstellung wählen Sie einen Plan innerhalb eines Tarifs aus. Sie können Pläne innerhalb eines Tarifs nach oben oder unten anpassen oder in einen höheren Tarif wechseln, ein Wechsel in einen niedrigeren Tarif ist jedoch nicht möglich.

![Tarifupgrade](./media/analysis-services-overview/aas-overview-tier-up.png)

Der Tarif kann auf die Schnelle über das Azure-Portal oder mithilfe des PowerShell-Cmdlets „AzureRmAnalysisServicesServer“ geändert werden. Weitere Informationen zu den verschiedenen Plänen und Tarifen sowie den Preisrechner zur Ermittlung des optimalen Plans für Ihre Anforderungen finden Sie unter [Azure Analysis Services – Preise](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="scale-resources"></a>Skalieren von Ressourcen
Skalieren Sie zentral hoch oder herunter, oder halten Sie Ihren Server an. Verwenden Sie das Azure-Portal, oder nutzen Sie PowerShell für umfassende Steuerungsmöglichkeiten. Sie bezahlen nur für die tatsächliche Nutzung.

Verwenden Sie beim Erstellen neuer Server das Cmdlet [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver), um Ihren Plan festzulegen. Bei bereits vorhandenen Servern können Sie den Plan mithilfe des Cmdlets [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver) festlegen. Sie verwenden Ihren Dienst nicht durchgehend? Dann halten Sie ihn einfach an – entweder über das Portal oder mithilfe des Cmdlets [Suspend-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver). Mit dem Cmdlet [Resume-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver) können Sie ihn wieder starten. Sie zahlen nur, wenn Ihr Server aktiv ist.


## <a name="regions"></a>Regionen
Azure Analysis Services-Server können in folgenden [Azure-Regionen](https://azure.microsoft.com/regions/) erstellt werden:

| Amerika | Europa | Asien-Pazifik |
|----------|--------|--------------|
|  Brasilien Süd<br> Kanada, Mitte<br> USA (Ost) 2<br> USA (Mitte/Norden)<br> USA (Mitte/Süden)<br> USA, Westen-Mitte<br> USA (West) | Nordeuropa<br> UK, Süden<br> Westeuropa |   Australien, Südwesten<br> Japan Ost<br> Südostasien<br> Indien, Westen  |

Diese Liste ist unter Umständen nicht vollständig, da immer wieder neue Regionen hinzukommen. Die Standortauswahl erfolgt, wenn Sie Ihren Server im Azure-Portal oder mithilfe von Azure Resource Manager-Vorlagen erstellen. Aus Leistungsgründen empfiehlt es sich, einen Standort in der Nähe Ihrer größten Benutzerbasis zu wählen. Stellen Sie Ihre Modelle auf redundanten Servern in mehreren Regionen bereit, um eine [hohe Verfügbarkeit](analysis-services-bcdr.md) zu gewährleisten.

## <a name="get-up-and-running-quickly"></a>Schnelle Betriebsbereitschaft
Über das Azure-Portal können Sie innerhalb weniger Minuten [einen Server erstellen](analysis-services-create-server.md). Und mit Azure Resource Manager-Vorlagen und PowerShell lassen sich Server mit einer deklarativen Vorlage bereitstellen. Mit einer einzelnen Vorlage können Sie mehrere Dienste sowie andere Azure-Komponenten (beispielsweise Speicherkonten) bereitstellen.  Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

Nach der Servererstellung können Sie direkt über das Azure-Portal ein tabellarisches Modell erstellen. Mit dem neuen Webdesigner-Feature (Vorschauversion) können Sie eine Verbindung mit einer Azure SQL-Datenbank- oder einer Azure SQL Data Warehouse-Datenquelle herstellen oder eine Power BI Desktop-PBIX-Datei importieren. Beziehungen zwischen Tabellen werden automatisch erstellt, und Sie können Measures erstellen oder die Datei „model.bim“ im JSON-Format direkt in Ihrem Browser bearbeiten.

## <a name="migrate-existing-tabular-models"></a>Migrieren vorhandener tabellarischer Modelle
Wenn Sie bereits über lokale SQL Server Analysis Services-Modelllösungen verfügen, können Sie ohne nennenswerte Änderungen zu Azure Analysis Services migrieren. Für die Migration können Sie Ihr Modell mithilfe von SSDT auf Ihrem Server bereitstellen. Alternativ können Sie in SSMS die Sicherungs- und Wiederherstellungsfunktion oder TMSL verwenden.

Falls Sie über lokale Datenquellen verfügen, müssen Sie ein [lokales Datengateway](analysis-services-gateway.md) installieren und konfigurieren. Falls Sie bereits Rollen und Rollenmitglieder konfiguriert haben, werden zwar die Rollen migriert, die Mitglieder müssen jedoch mithilfe von SSMS oder PowerShell erneut hinzugefügt werden.


## <a name="data-sources"></a>Datenquellen
Azure Analysis Services unterstützt das Herstellen von Verbindungen mit lokalen Datenquellen in Ihrer Organisation sowie mit Datenquellen in der Cloud. Kombinieren Sie Daten aus lokalen und cloudbasierten Datenquellen, um eine Hybridlösung zu erhalten. 

Neue tabellarische Modelle mit Kompatibilitätsgrad 1400 verwenden das moderne Datenabruffeature in SSDT (auf der Grundlage der M-Formelabfragesprache). Der Datenabruf bietet mehr Datentransformations- und Mashup-Features sowie die Möglichkeit, eigene erweiterte Abfragen mit der M-Formelsprache zu erstellen und zu bearbeiten. So können Sie beispielsweise mit tabellarischen Modellen mit Kompatibilitätsgrad 1400 Modelle auf der Grundlage von Datendateien in Azure Blob Storage erstellen.

Azure Analysis Services unterstützt die Verwendung von [DirectQuery](https://docs.microsoft.com/sql/analysis-services/tabular-models/directquery-mode-ssas-tabular) für eine Direktverbindung mit Azure SQL-Datenbank, Azure SQL Data Warehouse, SQL Server, SQL Server Data Warehouse, Oracle und relationalen Teradata-Datenbanken.

Weitere Informationen finden Sie unter [In Azure Analysis Services unterstützte Datenquellen](analysis-services-datasource.md).

## <a name="use-the-tools-you-already-know"></a>Verwenden Sie die Tools, die Sie bereits kennen

![BI-Entwicklertools](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) für Visual Studio
Modelle können mit dem kostenlosen [SQL Server Data Tools (SSDT) für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) entwickelt und bereitgestellt werden. SSDT beinhaltet Analysis Services-Projektvorlagen, die Sie bei der Einrichtung unterstützen. SSDT beinhaltet nun die modernen datenabrufbezogenen Datenquellenabfrage- und Mashup-Funktionen für tabellarische Modelle mit Kompatibilitätsgrad 1400. Wenn Sie mit dem Datenabruf in Power BI Desktop und Excel 2016 vertraut sind, wissen Sie bereits, wie einfach sich hochgradig angepasste Datenquellenabfragen erstellen lassen.

Mit dem neuen SSDT und tabellarischen Modellen mit Kompatibilitätsgrad 1400 müssen Sie keine lokale Analysis Services-Instanz mehr installieren, um eine Arbeitsbereichdatenbank zu hosten. SSDT verfügt nun über ein eigenes integriertes Analysis Services-Modul und eine eigene Datenbank. Wenn Sie soweit sind, können Sie Ihre Server in Azure direkt über SSDT bereitstellen. SSDT wird außerdem monatlich aktualisiert, sodass Sie schnell mit den neuesten Features arbeiten können.

#### <a name="sql-server-management-studio"></a>SQL Server Management Studio
Verwalten Sie Server und Modelldatenbanken mit [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Stellen Sie eine Verbindung mit Ihren Servern in der Cloud her. Führen Sie TMSL-Skripts direkt über das XMLA-Abfragefenster aus, und automatisieren Sie Aufgaben mithilfe von TMSL-Skripts. Die Features und Funktionen werden kontinuierlich erweitert. (SSMS wird monatlich aktualisiert.)

#### <a name="powershell"></a>PowerShell
Zur Verwaltung von Serverressourcen (beispielsweise Erstellen von Servern, Anhalten oder Fortsetzen von Servervorgängen oder Ändern des Servicelevels (Tarif)) werden AzureRM-Cmdlets (Azure Resource Manager) verwendet. Für andere Datenbankverwaltungsaufgaben (beispielsweise für das Hinzufügen oder Entfernen von Rollenmitgliedern, für die Verarbeitung oder für das Ausführen von TMSL-Skripts) werden Cmdlets im SQLServer-Modul verwendet. Sowohl das AzureRM- als auch das SQLServer-Modul steht im [PowerShell-Katalog](https://www.powershellgallery.com/) zur Verfügung.


## <a name="secure"></a>Schützen
![Datenvisualisierungen](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Authentifizierung
Die Benutzerauthentifizierung für Azure Analysis Services wird per [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md) durchgeführt. Wenn sich Benutzer bei einer Azure Analysis Services-Datenbank anmelden, verwenden sie eine Organisationskontoidentität mit Zugriff auf die betreffende Datenbank. Diese Benutzeridentitäten müssen Mitglied des standardmäßigen Azure Active Directory für das Abonnement sein, in dem sich der Azure Analysis Services-Server befindet. Weitere Informationen finden Sie unter [Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md).

#### <a name="data-security"></a>Datensicherheit
Azure Analysis Services nutzt Azure Blob Storage zum Beibehalten von Speicher und Metadaten für Analysis Services-Datenbanken. Datendateien im Blob werden mithilfe der serverseitigen Verschlüsselung (Server Side Encryption, SSE) von Azure Blob verschlüsselt. Bei Verwendung des Direktabfragemodus werden nur Metadaten gespeichert. Auf die tatsächlichen Daten wird von der Datenquelle zur Abfragezeit zugegriffen.

#### <a name="on-premises-data-sources"></a>Lokale Datenquellen
Das Installieren und Konfigurieren eines [lokalen Datengateways](analysis-services-gateway.md) ermöglicht den sicheren Zugriff auf die lokalen Daten in der Organisation. Gateways bieten Zugriff auf Daten im Direktabfragemodus und im speicherinternen Modus. Wenn ein Azure Analysis Services-Modell eine Verbindung mit einer lokalen Datenquelle herstellt, wird eine Abfrage zusammen mit den verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Der Gatewayclouddienst analysiert die Abfrage und überträgt die Anforderung per Push an einen Azure Service Bus. Das lokale Gateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab. Das Gateway ruft dann die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt für die Ausführung eine Verbindung mit der Datenquelle her. Anschließend werden die Ergebnisse aus der Datenquelle zurück an das Gateway und dann an die Azure Analysis Services-Datenbank gesendet.

Azure Analysis Services unterliegt den [Microsoft Online Services-Nutzungsbedingungen](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und der [Microsoft Online Services-Datenschutzerklärung](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Weitere Informationen zur Sicherheit in Azure finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="client-connections"></a>Clientverbindungen
![Datenvisualisierungen](./media/analysis-services-overview/aas-overview-clients.png)

Moderne Tools zur Untersuchung und Visualisierung von Daten wie Power BI, Excel und andere Drittanbietertools liefern Endbenutzern hochgradig interaktive und visuell aufbereitete Einblicke in Ihre Modelldaten.

Clients nutzen [Clientbibliotheken](analysis-services-data-providers.md) vom Typ MSOLAP, AMO oder ADOMD, um eine Verbindung mit Analysis Services-Servern herzustellen. Microsoft-Clientanwendungen wie Power BI Desktop und Excel installieren alle drei Clientbibliotheken. Abhängig von der Version und der Updatehäufigkeit handelt es sich dabei jedoch unter Umständen nicht um die neuesten, von Azure Analysis Services benötigten Versionen. Dies gilt auch für benutzerdefinierte Anwendungen oder andere Schnittstellen wie AsCmd, TOM, ADOMD.NET. Für diese Anwendungen müssen die Bibliotheken in der Regel manuell als Teil eines Pakets installiert werden.


## <a name="get-help"></a>Hier erhalten Sie Hilfe

#### <a name="documentation"></a>Dokumentation
Azure Analysis Services ist einfach einzurichten und zu verwalten. Hier finden Sie alle Informationen, die Sie zum Erstellen und Verwalten Ihrer Serverdienste benötigen. Wenn Sie ein Datenmodell für die Bereitstellung auf dem Server erstellen, erfolgt dies auf ähnliche Weise wie das Erstellen eines Datenmodells, das Sie auf einem lokalen Server bereitstellen. Unter [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) finden Sie eine umfassende Bibliothek mit Artikeln zu Konzepten, Vorgehensweisen, Referenzartikel und Tutorials.

#### <a name="videos"></a>Videos
Unter [Azure Analysis Services auf Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services) finden Sie hilfreiche Videos.

#### <a name="blogs"></a>Blogs
Dinge ändern sich schnell. Im [Azure Analysis Services-Teamblog](https://blogs.msdn.microsoft.com/analysisservices/) und im [Azure-Blog](https://azure.microsoft.com/blog/) (in englischer Sprache) erhalten Sie immer die neuesten Informationen.

#### <a name="community"></a>Community
Analysis Services verfügt über eine dynamische Community von Benutzern. Beteiligen Sie sich an der Diskussion im [Azure Analysis Services-Forum](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Feedback
Haben Sie Vorschläge oder Funktionsanfragen? Hinterlassen Sie Kommentare im [Azure Analysis Services-Feedbackforum](https://aka.ms/azureanalysisservicesfeedback) (in englischer Sprache).

Haben Sie Vorschläge zur Dokumentation? Sie können am Ende jedes Artikels mit Livefyre Kommentare hinzufügen.

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt mehr über Azure Analysis Services wissen, können Sie loslegen. Erfahren Sie, wie Sie in Azure [einen Server erstellen](analysis-services-create-server.md). Wenn Ihr Server bereit ist, absolvieren Sie das [Adventure Works-Tutorial](tutorials/aas-adventure-works-tutorial.md). Dort erfahren Sie, wie Sie ein voll funktionsfähiges tabellarisches Modell erstellen und auf Ihrem Server bereitstellen.

