---
title: "Azure-Protokollintegration mit Azure Active Directory-Überwachungsprotokollen | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie den Dienst Azure-Protokollintegration installieren und Azure-Überwachungsprotokolle integrieren."
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
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 4a5c1cf7ee6f9e7ad43bb47f798520384606969c
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="integrate-azure-active-directory-audit-logs"></a>Integrieren von Azure Active Directory-Überwachungsprotokollen

Azure AD-Überwachungsereignisse (Azure Active Directory) helfen Ihnen, privilegierte Aktionen zu bestimmen, die in Azure Active Directory aufgetreten sind. Sie können die Typen von Ereignissen anzeigen, die Sie nachverfolgen können, indem Sie [Azure Active Directory-Überwachungsberichtereignisse](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md) überprüfen.

> [!NOTE]
> Zur Vorbereitung auf die Schritte in diesem Artikel müssen zunächst die Schritte im Artikel [Erste Schritte](security-azure-log-integration-get-started.md) ausgeführt werden.

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Schritte zum Integrieren von Azure Active Directory-Überwachungsprotokollen

1. Öffnen Sie die Eingabeaufforderung, und führen Sie diesen Befehl aus:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Führen Sie den folgenden Befehl aus: 
 
   ``azlog createazureid``

   Dabei werden Sie zur Angabe Ihrer Azure-Anmeldeinformationen aufgefordert. Anschließend wird in den Azure AD-Mandanten, die die Azure-Abonnements hosten, in denen der angemeldete Benutzer als Administrator, Co-Administrator oder Besitzer fungiert, ein Azure Active Directory-Dienstprinzipal erstellt. Handelt es sich bei dem angemeldeten Benutzer nur um einen Gastbenutzer des Azure AD-Mandanten, tritt bei dem Befehl ein Fehler auf. Die Authentifizierung gegenüber Azure wird über Azure AD durchgeführt. Beim Erstellen eines Dienstprinzipals für die Azure-Protokollintegration wird die Azure AD-Identität erstellt, die Lesezugriff auf Azure-Abonnements erhält.

3. Führen Sie den folgenden Befehl aus, um Ihre Mandanten-ID bereitzustellen. Zum Ausführen des Befehls müssen Sie Mitglied der Mandanten-Administratorrolle sein.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Beispiel:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass die JSON-Dateien des Azure Active Directory-Überwachungsprotokolls in ihnen erstellt wurden:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Das folgende Video veranschaulicht die in diesem Artikel beschriebenen Schritte:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Sie müssen sich an Ihren SIEM-Anbieter wenden, um spezifische Anweisungen zum Übertragen der Informationen in den JSON-Dateien in Ihr SIEM-System (Security Information and Event Management) zu erhalten.

Unterstützung durch die Community ist über das [MSDN-Forum für die Azure-Protokollintegration](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration) verfügbar. In diesem Forum kann sich die Community der Azure-Protokollintegration gegenseitig mit Fragen, Antworten, Tipps und Tricks unterstützen. Darüber hinaus überwacht das Team für die Azure-Protokollintegration dieses Forum und bietet nach Möglichkeit entsprechende Hilfe an.

Sie können zudem eine [Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md) öffnen. Wählen Sie **Protokollintegration** als Dienst aus, für den Sie Support anfordern.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-Protokollintegration finden Sie auf den folgenden Webseiten:

* [Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324) (Microsoft Azure-Protokoll-Integration für Azure-Protokolle, in englischer Sprache): Diese Seite im Download Center bietet Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
* [Einführung in die Microsoft Azure-Protokollintegration](security-azure-log-integration-overview.md): Dieser Artikel stellt die Azure-Protokollintegration, die wichtigsten Funktionen sowie die Funktionsweise vor.
* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner, in englischer Sprache): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.
* [Azure-Protokollintegration – Häufig gestellte Fragen](security-azure-log-integration-faq.md): In diesem Artikel werden Fragen zur Azure-Protokollintegration beantwortet.
* [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration](../security-center/security-center-integrating-alerts-with-log-integration.md): In diesem Artikel erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.
* [New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Neue Features für Azure-Diagnose und Azure-Überwachungsprotokolle, in englischer Sprache): In diesem Blogbeitrag werden Azure-Überwachungsprotokolle und andere Features vorgestellt, mit denen Sie sich einen Einblick in die Vorgänge Ihrer Azure-Ressourcen verschaffen können.

