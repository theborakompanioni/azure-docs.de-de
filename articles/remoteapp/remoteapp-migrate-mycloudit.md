---
title: "Ändern der Abrechnung für Azure RemoteApp | Microsoft Docs"
description: "Erfahren Sie, wie Sie die Abrechnung für Azure RemoteApp beenden."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 8f94da9a-7848-4ddc-b7b7-d9c280ccf4d7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mbaldwin
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 32fc673eeef01e80c73375bf264206beea8cfbe5
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="migrate-from-azure-remoteapp-to-mycloudit"></a>Migrieren von Azure RemoteApp zu MyCloudIT 

**Verwenden Sie derzeit Microsoft Azure RemoteApp?** Von MyCloudIT wurde ein automatisiertes Tool erstellt, um Ihre Azure RemoteApp-Sammlungen (ARA) zur MyCloudIT-Verwaltungsplattform zu migrieren und gleichzeitig weiterhin Microsoft Azure zu verwenden.

**Nutzen Sie das Azure Resource Manager-Portal**: Die abgeschlossene Migration zur MyCloudIT-Plattform ermöglicht sofortigen Zugriff auf das neue Azure Resource Manager-Portal von Azure. Dieses Portal enthält alle neuen Funktionen und Innovationen, die von Microsoft Azure angeboten werden, einschließlich des Zugriffs auf VM-Größen, um sicherzustellen, dass Ihre Bereitstellung entsprechend den Anforderungen Ihres Unternehmens erstellt wird.

**Führen Sie parallele Tests aus, um die richtige Lösung für Ihre Anforderungen sicherzustellen**: Das MyCloudIT-Migrationstool wurde erstellt, um den Migrationsvorgang zu initiieren und gleichzeitig Tests auszuführen, während die aktuellen ARA-Benutzer ARA weiterhin verwenden.  Ihre Benutzer verbleiben in ARA, bis Ihre Migration und die Tests abgeschlossen sind.  Das Migrationstool wurde für die Verarbeitung einer typischen ARA-Sammlung erstellt.  Falls Sie der Meinung sind, Ihr Szenario wäre einzigartig und nicht standardmäßig, kontaktieren Sie uns über [sales@conexlink.com](mailto:sales@conexlink.com), damit wir einen angepasste Plan zur Unterstützung Ihrer Migration bereitstellen können.

**Desktop-as-a-Service-Funktionen**: Bitte beachten Sie, dass MyCloudIT nicht nur die Ihnen vertrauten RemoteApp-Funktionen bietet, sondern zu den gleichen Kosten pro Monat und ohne minimale Benutzeranforderungen auch vollständige Desktop-as-a-Service-Funktionen.

## <a name="what-we-will-do-for-you"></a>Unser Angebot für Sie

MyCloudIT automatisiert mit dem automatisierten Migrationstool die Migration Ihrer Azure RemoteApp-Vorlage aus dem klassischen Azure-Portal Ihres Abonnements zum Azure Resource Manager-Portal Ihres Abonnements.  

> [!NOTE]
> Die Azure RemoteApp-Vorlage muss in der gleichen Azure-Region wie die ursprüngliche Azure RemoteApp-Bereitstellung bleiben.  Wenn Sie während der Migration Azure-Regionen oder Azure-Abonnements ändern müssen, kontaktieren Sie uns über [sales@conexlink.com](mailto:sales@conexlink.com), um weitere Anleitungen zu erhalten.

Lesen Sie die folgenden ausführlichen Informationen zum automatisierten Migrationsprozess mit dem MyCloudIT-Migrationstool:

1. Das Migrationstool scannt Ihre aktuellen Abonnements auf alle vorhandenen ARA-Bereitstellungen.  
2. Wählen Sie immer nur eine ARA-Sammlung zum Migrieren aus.  Wenn Sie über mehrere Sammlungen verfügen, führen Sie unser Tool mehrmals aus.
3. Sie haben die Option, Benutzerprofil-Datenträger (UPD) in Ihre neue Bereitstellung zu kopieren. Sie können also ältere Daten abrufen oder die UPDs manuell der neuen Bereitstellung zuordnen. Wenn Sie Ihre UPDs kopieren, speichern wir die UPDs und nehmen eine Textdatei auf, die den UPD-Namen den einzelnen Benutzernamen zuordnet.  Die UPDs werden in eine Freigabe auf dem RDSMGMT-Server `F:\Shares\LegacyUPD` kopiert und über die Freigabe `\\RDSmgmt\LegacyUPD` verfügbar gemacht. 
4. Bei Ihrer Migration entstehen keine Ausfallzeiten für die aktuelle ARA-Bereitstellung.  Aber wenn nach dem Erstellen der Kopie Änderungen an den UPDs (von ARA) vorgenommen werden, sind diese Änderungen nicht in den UPDs enthalten, die im Azure Resource Manager-Portal gespeichert sind. 
5. Wenn Sie über zusätzliche virtuelle Computer wie Domänencontroller und Dateiserver im klassischen virtuellen Azure-Netzwerk verfügen, richten wir VNet-Peering zwischen Ihrem vorhandenen klassischen virtuellen Azure-Netzwerk und dem neuen virtuellen Netzwerk ein, das wir für Sie im neuen Azure Resource Manager-Portal erstellt haben.
6. Unsere automatisierte Lösung richtet VNet-Peering nur zwischen dem vorhandenen klassischen virtuellen Azure-Netzwerk und dem neuen virtuellen Netzwerk ein, wenn Ihre vorhandene ARA-Bereitstellung eine Hybridbereitstellung ist. Dies bedeutet, dass Sie die Authentifizierung mit einem Active Directory-Domänencontroller unter Windows Server im vorhandenen klassischen virtuellen Netzwerk vornehmen. Wenn wir kein VNet-Peering für Ihre Sammlung einrichten, Sie aber VNet-Peering benötigen, kontaktieren Sie uns über [sales@conexlink.com](mailto:sales@conexlink.com), und wir konfigurieren Ihnen VNet-Peering ohne zusätzliche Kosten.
7. Mit unserer automatisierten Lösung wird sichergestellt, dass Ihre Azure DNS-Konfiguration mit den Einstellungen des neuen virtuellen Netzwerks aktualisiert wird, um sicherzustellen, dass Ihre neue Bereitstellung im klassischen VNet eine Verbindung mit Ihrem vorhandenen Domänencontroller herstellen kann.
8. Mit unserer automatisierten Lösung wird sichergestellt, dass keine IP-Adressenkonflikte entstehen, da wir dieses neue virtuelle Netzwerk erstellen und das VNet-Peering für Bereitstellungen einrichten, die über einen vorhandenen Active Directory-Server unter Windows Server verfügen.
9. Wenn Sie Azure AD nur für die Authentifizierung verwenden, erstellt MyCloudIT eine neue Active Directory-Domäne unter Windows Server und verwendet Azure AD Connect zum Synchronisieren von Benutzern zwischen der vorhandenen Azure AD-Instanz und der neuen Active Directory-Domäne unter Windows Server, die von MyCloudIT erstellt wurde.
10. Wenn Sie eine Active Directory-Domäne unter Windows Server zum Authentifizieren von ARA-Benutzern verwenden, verbindet unsere automatisierte Lösung über VNet-Peering Ihre neue MyCloudIT-Bereitstellung mit Ihrem vorhandenen Active Directory-Domänencontroller unter Windows Server.
11. Wenn Sie Azure Active Directory Domain Services für die Authentifizierung verwenden, können wir die Migration durchführen. Aber kontaktieren Sie uns bitte, damit wir einen benutzerdefinierten Migrationsplan für Sie erstellen können.  Senden Sie eine E-Mail an [sales@conexlink.com](mailto:sales@conexlink.com). 
12. Sobald die zu migrierende Sammlung bestätigt wurde, migriert unsere automatisierte Lösung Ihre ARA-Sammlung und die Benutzerprofil-Datenträger (optional) in die neue von MyCloudIT unterstützte RemoteApp-Lösung.
13. Sobald die Bereitstellung abgeschlossen ist, veröffentlichen wir die gleichen Anwendungen erneut, die in ARA veröffentlicht waren, und nach der Bereitstellung können Sie zusätzliche Anwendungen veröffentlichen.

## <a name="post-migration-benefits"></a>Vorteile nach der Migration

1. Wir stellen die Verwaltungskonsole bereit, mit der Sie den gesamten Lebenszyklus Ihrer RemoteApp-Bereitstellung verwalten können.
2. Sie können Ihre virtuellen Computer über das Portal verwalten.  Sie können einzelne virtuelle Computer bei Bedarf starten/beenden und deren Größe ändern.
3. Die Verwaltungskonsole bietet die Möglichkeit zum Erstellen und Verwalten von Benutzern/Gruppen über unser Verwaltungsportal.
4. Die Verwaltungskonsole bietet die Möglichkeit zum Synchronisieren von Benutzern mit Office 365, um einmaliges Anmelden einzurichten.
5. Die Verwaltungskonsole bietet die Möglichkeit, zusätzliche RemoteApp- und Desktopsammlungen ohne doppelte Benutzerkosten oder minimale Benutzeranforderungen zu erstellen. 
6. Die Verwaltungskonsole bietet die Möglichkeit zum Veröffentlichen neuer RemoteApp-Anwendungen.
7. Die Verwaltungskonsole bietet die Möglichkeit, das Starten und Beenden Ihrer RemoteApp-Bereitstellung zu planen, wenn Sie die Lösung nur zu bestimmten Zeiten benötigen.
8. Die Verwaltungskonsole bietet die Möglichkeit zum Automatisieren der Installation und Konfiguration des Azure Backup-Agents, der für Ihre Kundendaten eine Versionsgeschichte der Dokumentenaufbewahrung ermöglicht.
9. Die Verwaltungskonsole bietet Zugriff auf Leistungsmetriken Ihrer Bereitstellung.  Dadurch haben Sie die Möglichkeit, potenzielle Leistungsengpässe zu erkennen, ohne zusätzliche Tools für die Leistungsverwaltung zu installieren.
10. Wenn Sie über mehrere Sitzungshosts verfügen, können Sie die automatische Skalierung aktivieren, sodass nur die Sitzungshosts ausgeführt werden, die ausgeführt werden müssen.
11. MyCloudIT ermöglicht den Zugriff auf den RDWeb-Gatewayserver über einen MyCloudIT-Domänennamen.  Wir bieten auch die Möglichkeit, für das Branding der Endbenutzer die URL einer benutzerdefinierten URL neu zuzuordnen.

## <a name="prerequisites-for-migration"></a>Voraussetzungen für die Migration

1. Sie müssen über Zugriff auf das Azure-Abonnement verfügen, das Ihre aktuelle Azure RemoteApp-Lösung hostet.
2. Sie müssen unserem Portal Berechtigungen in Ihrem Abonnement gewähren, um Ihre Vorlage zu migrieren und Ihre neue MyCloudIT-Bereitstellung zu erstellen/zu ändern.
3. Beachten Sie, dass aufgrund einer Einschränkung von Azure RemoteApp jede Sammlung nur drei Mal migriert werden kann.  Wenn Sie eine Sammlung mehr als drei Mal migrieren müssen, können Sie in Azure ein Ticket zum Erhöhen der Anzahl von Exportvorgängen erstellen oder uns kontaktieren, damit wir Ihnen bei der ARA-Anforderung zum Erhöhen der Anzahl von Exportvorgängen helfen können.

## <a name="mycloudit-billing"></a>MyCloudIT-Abrechnung

Informationen zum Vorhersagen und Verwalten Ihrer Azure-Gesamtkosten finden Sie unter [MyCloudIT Pricing for RemoteApp Solutions (MyCloudIT-Preise für RemoteApp-Lösungen)](https://mcitdocuments.blob.core.windows.net/terms/MyCloudIT_Pricing_Overview.pdf) (PDF).

Wenn Sie weitere Fragen haben, kontaktieren Sie uns über [sales@conexlink.com](mailto:sales@conexlink.com), oder sehen Sie sich das vollständige Demovideo zum [Migrationstool für Azure RemoteApp – MyCloudIT](https://www.youtube.com/watch?v=YQ_1F-JeeLM&t=482s) an. 


