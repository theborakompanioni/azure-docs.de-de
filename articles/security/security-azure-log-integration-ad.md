---
title: "Azure-Protokollintegration (AZLOG) mit Active Directory-Überwachungsprotokollen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Azure-Protokollintegrationsdienst installieren und Protokolle aus Azure-Überwachungsprotokollen integrieren."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 03/29/2017
ms.author: barclayn
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: f7c2f702a4ff2af8bb7487d49f5c6f9bf5199a49
ms.lasthandoff: 04/14/2017


---

#<a name="integrate-azure-active-directory-audit-logs"></a>Integrieren von Azure Active Directory-Überwachungsprotokollen

Azure Active Directory-Überwachungsereignisse helfen Ihnen dabei, privilegierte Aktionen zu bestimmen, die in Azure Active Directory aufgetreten sind. Sie können die Typen von Ereignissen anzeigen, die Sie nachverfolgen können, indem Sie [Azure Active Directory-Überwachungsberichtereignisse](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md) überprüfen.

>[!NOTE]
Sie müssen die Schritte im Artikel [Erste Schritte](security-azure-log-integration-get-started.md) abgeschlossen haben, bevor Sie die Schritte in diesem Artikel durchführen können.

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Schritte zum Integrieren von Azure Active Directory-Überwachungsprotokollen

1. Öffnen Sie die Eingabeaufforderung, und wechseln Sie mit „cd“ zu **c:\Programme\Microsoft Azure Log Integration**.
2. Führen Sie den folgenden Befehl aus:

 ``azlog createazureid``
 
 Dabei werden Sie zur Angabe Ihrer Azure-Anmeldeinformationen aufgefordert. Anschließend wird in den Azure AD-Mandanten, die die Azure-Abonnements hosten, in denen der angemeldete Benutzer als Administrator, Co-Administrator oder Besitzer fungiert, ein Azure Active Directory-Dienstprinzipal erstellt. Handelt es sich bei dem angemeldeten Benutzer nur um einen Gastbenutzer des Azure AD-Mandanten, tritt bei dem Befehl ein Fehler auf. Die Authentifizierung gegenüber Azure wird über Azure Active Directory (AD) durchgeführt. Beim Erstellen eines Dienstprinzipals für die Azure-Protokollintegration wird die Azure AD-Identität erstellt, die Lesezugriff auf Azure-Abonnements erhält.
 
3. Führen Sie mit Ihrer Mandanten-ID den Befehl aus. Sie müssen Mitglied der Mandanten-Administratorrolle zum Ausführen des Befehls sein.

``Azlog.exe authorizedirectoryreader tenantId``

Beispiel

``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass die JSON-Dateien des Azure Active Directory-Überwachungsprotokolls erstellt wurden:

* **C:\Users\azlog\AzureActiveDirectoryJson**
* **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Verweisen Sie für den Standardconnector für die SIEM-Dateiweiterleitung auf den richtigen Ordner, um die Daten an die SIEM-Instanz zu übermitteln. Ja nach verwendetem SIEM-Produkt benötigen Sie unter Umständen einige Feldzuordnungen.

Unterstützung durch die Community ist über das [MSDN-Forum für die Azure-Protokollintegration](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration) verfügbar. Dieses Forum bietet der AzLog-Community die Möglichkeit, sich gegenseitig mit Fragen, Antworten, Tipps und Tricks zur optimalen Verwendung der Azure-Protokollintegration zu unterstützen. Darüber hinaus überwacht das Team für die Azure-Protokollintegration dieses Forum und bietet nach Möglichkeit entsprechende Hilfe an. 

Sie können zudem eine [Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md) öffnen. Wählen Sie zu diesem Zweck **Protokollintegration** als Dienst aus, für den Sie Support anfordern.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-Protokollintegration finden Sie in folgenden Dokumenten:

* [Microsoft Azure Log Integration for Azure logs (Microsoft Azure-Protokoll-Integration für Azure-Protokolle)](https://www.microsoft.com/download/details.aspx?id=53324): Download Center für Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
* [Introduction to Azure log integration](security-azure-log-integration-overview.md) (Einführung in die Azure-Protokollintegration): Dieses Dokument stellt die Azure-Protokollintegration, die wichtigsten Funktionen sowie die Funktionsweise vor.
* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.
* [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
* [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)](../security-center/security-center-integrating-alerts-with-log-integration.md) : In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.
* [New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Neue Features für Azure-Diagnose und Azure-Überwachungsprotokolle): In diesem Blogbeitrag werden Azure-Überwachungsprotokolle und andere Features vorgestellt, mit denen Sie sich einen Einblick in die Vorgänge Ihrer Azure-Ressourcen verschaffen können.

