---
title: Azure-Abrechnungs-APIs | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die Azure-Abrechnungs-APIs für Nutzung und Gebührenkarte, mit denen Sie Einblicke in den Azure-Ressourcenverbrauch und damit verbundene Trends erlangen können."
services: 
documentationcenter: 
author: BryanLa
manager: ruchic
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 03/25/2017
ms.author: mobandyo;bryanla
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 869734ad4eb2d7a408cc0d33d1492b50b1b8dc0f
ms.lasthandoff: 03/28/2017


---
# <a name="use-azure-billing-apis-to-programmatically-get-insight-into-your-azure-usage"></a>Verwenden der Azure-Abrechnungs-APIs, um programmgesteuerte Einblicke in die Nutzung Ihrer Azure-Ressourcen zu erlangen
Verwenden Sie Azure-Abrechnungs-APIs, um Nutzungs- und Ressourcendaten mittels Pull in Ihre bevorzugten Datenanalysetools abzurufen. Mithilfe der Azure-Ressourcennutzungs- und -Gebührenkarten-APIs können Sie Ihre Kosten genau vorhersagen und verwalten. Die APIs werden als Ressourcenanbieter implementiert und sind Teil der API-Familie, die von Azure Resource Manager verfügbar gemacht wird.  

## <a name="azure-resource-usage-api-preview"></a>Azure-API zur Ressourcennutzung (Preview)
Mit der Azure-Ressourcennutzungs-API können Sie Ihre geschätzten Azure-Verbrauchsdaten abrufen. Die API umfasst:

* **Rollenbasierte Zugriffssteuerung für Azure** – Sie können Ihre Zugriffsrichtlinien im [Azure-Portal](https://portal.azure.com) oder mit [Azure PowerShell-Cmdlets](/powershell/azureps-cmdlets-docs) konfigurieren, um festzulegen, welche Benutzer oder Anwendungen Zugriff auf die Nutzungsdaten eines Abonnements erhalten. Aufrufer müssen zur Authentifizierung standardmäßige Azure Active Directory-Token verwenden. Ordnen Sie dem Aufrufer die Rolle „Leser“, „Besitzer“ oder „Mitwirkender“ zu, um Zugriff auf die Nutzungsdaten für ein bestimmtes Azure-Abonnement zu erhalten.
* **Stündliche oder tägliche Aggregationen** – Aufrufer können angeben, ob sie ihre Azure-Nutzungsdaten in stündlichen Buckets oder täglichen Buckets erhalten möchten. Die Standardeinstellung ist „Täglich“.
* **Instanzmetadaten (inkl. Ressourcentags)** – Details der Instanzebene, z.B. der vollqualifizierte Ressourcen-URI „(/subscriptions/{subscription-id}/..)“, Ressourcengruppeninformationen und Ressourcentags. Mit diesen Metadaten können Sie die Nutzung auf deterministische und programmgesteuerte Weise anhand der Tags zuordnen, z.B. bei der internen Verrechnung.
* **Ressourcenmetadaten** – Ressourcendetails (z.B. Meter Name, Meter Category, Meter Sub-Category, Unit und Region) lassen den Aufrufer besser verstehen, was genutzt wurde. Außerdem arbeiten wir an einer Vereinheitlichung der Terminologie für Ressourcenmetadaten im Azure-Portal, in der CSV-Datei zur Azure-Nutzung, in der CSV-Datei zur EA-Abrechnung und in anderen öffentlich zugänglichen Benutzeroberflächen, um Ihnen die Korrelation von Daten über verschiedene Umgebungen hinweg zu ermöglichen.
* **Nutzung für alle Angebotstypen** – Nutzungsdaten sind für alle Angebotstypen verfügbar, z.B. „Nutzungsbasierte Bezahlung“, „MSDN“, „Verbindliche Zusage“, „Guthaben“ und „EA“.

## <a name="azure-resource-ratecard-api-preview"></a>Azure-API für Ressourcengebührenkarte (Preview)
Mit der Azure-Ressourcengebührenkarten-API (Resource RateCard) können Sie die Liste der verfügbaren Azure-Ressourcen sowie jeweils die geschätzten Preise abrufen. Die API umfasst:

* **Rollenbasierte Zugriffssteuerung für Azure** – Sie können Ihre Zugriffsrichtlinien im [Azure-Portal](https://portal.azure.com) oder mit [Azure PowerShell-Cmdlets](/powershell/azureps-cmdlets-docs) konfigurieren, um festzulegen, welche Benutzer oder Anwendungen Zugriff auf die Gebührenkartendaten erhalten. Aufrufer müssen zur Authentifizierung standardmäßige Azure Active Directory-Token verwenden. Ordnen Sie dem Aufrufer die Rolle „Leser“, „Besitzer“ oder „Mitwirkender“ zu, um Zugriff auf die Nutzungsdaten für ein bestimmtes Azure-Abonnement zu erhalten.
* **Unterstützung für die Angebote „Nutzungsbasierte Bezahlung“, „MSDN“, „Verbindliche Zusage“ und „Guthaben“ („EA“ wird nicht unterstützt)** – Mit dieser API werden Preisinformationen der Azure-Angebotsebene bereitgestellt.  Aufrufer dieser API müssen die Angebotsinformationen übergeben, um Ressourcendetails und -preise abzurufen. Wir können derzeit keine EA-Preise angeben, da EA-Angebote über angepasste Preise pro Registrierung verfügen. 

## <a name="scenarios"></a>Szenarios
Hier sind einige Szenarios angegeben, die aufgrund der Kombination aus Nutzungs- und Gebührenkarten-APIs möglich sind:

* **Azure-Ausgaben während des Monats** – Setzen Sie die Nutzungs- und Gebührenkarten-APIs zusammen ein, um einen besseren Einblick in ihre Cloudausgaben während eines Monats zu erhalten. Hierzu analysieren sie die stündlichen und täglichen Buckets für die Nutzung und die Gebührenschätzungen.
* **Einrichten von Warnungen** – Verwenden Sie die Nutzungs- und Gebührenkarten-APIs, um die Verbrauchs- und Gebührenschätzungen für die Cloud abzurufen und ressourcenbasierte oder kostenbasierte Warnungen einzurichten.
* **Vorhersagen des Rechnungsbetrags** – Rufen Sie Ihren geschätzten Verbrauch und die geschätzten Cloudausgaben ab, und wenden Sie Machine Learning-Algorithmen an, um vorherzusagen, welcher Betrag sich am Ende des Abrechnungszeitraums ergibt.
* **Kostenanalyse vor der Nutzung** – Mit der Gebührenkarten-API können Sie vorhersagen, wie hoch ihre Rechnung bei der erwarteten Nutzung wäre, wenn sie ihre Workloads auf Azure verlagern. Wenn Sie über vorhandene Workloads in anderen Clouds bzw. Private Clouds verfügen, können Sie die Nutzung auch den Azure-Preisen zuordnen, um eine bessere Schätzung der Azure-Ausgaben zu erhalten. Diese Schätzungen bieten die Pivot-Funktion für ein Angebot an und ermöglichen Ihnen, verschiedene Angebotstypen über die nutzungsbasierte Bezahlung hinaus zu vergleichen und gegenüberzustellen, z.B. verbindliche Zusage und Guthaben. Außerdem können Sie mit den APIs Kostenunterschiede nach Region anzeigen und eine Was-wäre-wenn-Analyse durchführen, um Bereitstellungsentscheidungen zu erleichtern.
* **Was-wäre-wenn-Analyse** -
  
  * Sie können ermitteln, ob es kostengünstiger ist, Ihre Workloads in einer anderen Region oder unter einer anderen Konfiguration der Azure-Ressource auszuführen. Azure-Ressourcenkosten können je nach der Azure-Region variieren, in der sie ausgeführt werden.
  * Außerdem können Sie ermitteln, ob ein anderer Azure-Angebotstyp zu einem besseren Preis für eine Azure-Ressource führt.

## <a name="partner-solutions"></a>Partnerlösungen
Unter [Nutzungs- und Gebührenkarten-APIs von Microsoft Azure ermöglichen Cloudyn die ITFM-Bereitstellung für Kunden](billing-usage-rate-card-partner-solution-cloudyn.md) wird die Integrationsumgebung beschrieben, die vom Azure-Abrechnungs-API-Partner [Cloudyn](https://www.cloudyn.com/microsoft-azure/) angeboten wird. Dieser Artikel enthält ausführliche Informationen zu den Umgebungen, darunter ein Video mit einer Darstellung, wie Sie Cloudyn und die Azure-Abrechnungs-APIs verwenden können, um Einblicke in die Azure-Nutzungsdaten zu erhalten.

Unter [Integration von Cloud Cruiser und Microsoft Azure-Abrechnungs-API](billing-usage-rate-card-partner-solution-cloudcruiser.md) wird beschrieben, wie das [Express for Azure Pack von Cloud Cruiser](http://www.cloudcruiser.com/partners/microsoft/) direkt über das WAP-Portal (Windows Azure Pack) eingesetzt werden kann. Sie können sowohl die betrieblichen als auch die finanziellen Aspekte der Microsoft Azure Private Cloud oder gehosteten Public Cloud über eine zentrale Benutzeroberfläche nahtlos verwalten.   

## <a name="next-steps"></a>Nächste Schritte
* Ausführlichere Informationen zu beiden APIs finden Sie unter [Azure-Abrechnungs-REST-API – Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).
* Wenn Sie sich weiter mit Beispielcode beschäftigen möchten, sehen Sie sich die Codebeispiele für die Microsoft Azure-Abrechnungs-API unter [Azure-Codebeispiele](https://azure.microsoft.com/documentation/samples/?term=billing)an.

## <a name="learn-more"></a>Weitere Informationen
* Weitere Informationen finden Sie unter [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Weitere Informationen zu den Tools, die zum Entwickeln eines Verständnisses der Cloudausgaben erforderlich sind, finden Sie im Gartner-Artikel [Market Guide for IT Financial Management (ITFM) Tools](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb) (Marktleitfaden für IT Financial Management (ITFM)-Tools).


