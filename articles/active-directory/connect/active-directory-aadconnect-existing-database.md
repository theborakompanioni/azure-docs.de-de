---
title: Installieren von Azure AD Connect mithilfe einer vorhandenen ADSync-Datenbank | Microsoft-Dokumentation
description: Dieses Thema beschreibt die Verwendung einer vorhandenen ADSync-Datenbank.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.reviewer: cychua
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: a16daa1f320516a771f32cf30fca6f823076aa96
ms.openlocfilehash: d005042fffcf8f4ff99876961a55d254fd4fb2d5
ms.contentlocale: de-de
ms.lasthandoff: 09/02/2017

---

# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Installieren von Azure AD Connect mithilfe einer vorhandenen ADSync-Datenbank
Azure AD Connect erfordert eine SQL Server-Datenbank zum Speichern von Daten. Sie können entweder die mit Azure AD Connect installierte SQL Server 2012 Express LocalDB-Standardinstanz oder Ihre eigene vollständige Version von SQL Server verwenden. Bei der Installation von Azure AD Connect wurde bisher immer eine neue Datenbank mit dem Namen ADSync erstellt. Ab Azure AD Connect Version 1.1.613.0 können Sie Azure AD Connect optional mit einem Verweis auf eine vorhandene ADSync-Datenbank installieren.

## <a name="benefits-of-using-an-existing-adsync-database"></a>Vorteile der Verwendung einer vorhandenen ADSync-Datenbank
Mit Verweis auf eine vorhandene ADSync-Datenbank:

- Mit Ausnahme von Anmeldeinformationen wird die in der ADSync-Datenbank gespeicherte Konfiguration der Synchronisierung (einschließlich benutzerdefinierter Synchronisierungsregeln, Connectors, Filter und Konfiguration optionaler Funktionen) während der Installation automatisch wiederhergestellt und verwendet. Anmeldeinformationen, die Azure AD Connect verwendet, um Änderungen mit einem lokalen AD und Azure AD zu synchronisieren, werden verschlüsselt, und nur der vorherige Azure AD Connect-Server kann darauf zugreifen.
- Alle in der ADSync-Datenbank gespeicherten (mit Connectorbereichen und Metaverse verknüpften) Identitätsdaten und Synchronisierungscookies werden auch wiederhergestellt. Die neu installierte Azure AD Connect-Serverinstanz kann weiterhin von dort synchronisieren, wo die vorherige Azure AD Connect-Serverinstanz aufgehört hat – eine vollständige Synchronisierung ist nicht erforderlich.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Szenarien, in denen die Verwendung einer vorhandenen ADSync-Datenbank vorteilhaft ist
Diese Vorteile liegen in folgenden Szenarien vor:


- Sie haben eine vorhandene Bereitstellung von Azure AD Connect. Ihre vorhandene Azure AD Connect-Serverinstanz funktioniert nicht mehr, aber die SQL-Serverinstanz mit der ADSync-Datenbank ist noch funktionsfähig. Sie können eine neue Azure AD Connect-Serverinstanz installieren und sie auf die vorhandene ADSync-Datenbank verweisen. 
- Sie haben eine vorhandene Bereitstellung von Azure AD Connect. Ihre SQL-Serverinstanz mit der ADSync-Datenbank funktioniert nicht mehr. Sie besitzen jedoch eine aktuelle Sicherung der Datenbank. Sie können zunächst die ADSync-Datenbank in einer neuen SQL-Serverinstanz wiederherstellen. Anschließend können Sie eine neue Azure AD Connect-Serverinstanz installieren und sie auf die wiederhergestellte AdSync-Datenbank verweisen.
- Sie haben eine vorhandene Bereitstellung von Azure AD Connect, die LocalDB verwendet. Aufgrund der 10-GB-Grenze von LocalDB möchten Sie zu vollständigem SQL migrieren. Sie können die ADSync-Datenbank von LocalDB sichern und in einer SQL-Serverinstanz wiederherstellen. Anschließend können Sie eine neue Azure AD Connect-Serverinstanz installieren und sie auf die wiederhergestellte AdSync-Datenbank verweisen.
- Sie versuchen, einen Stagingserver einzurichten, und möchten sicherstellen, dass seine Konfiguration mit der des derzeit aktiven Servers übereinstimmt. Sie können die ADSync-Datenbank sichern und in einer anderen SQL-Serverinstanz wiederherstellen. Anschließend können Sie eine neue Azure AD Connect-Serverinstanz installieren und sie auf die wiederhergestellte AdSync-Datenbank verweisen.

## <a name="prerequisite-information"></a>Erforderliche Informationen

Bevor Sie fortfahren, beachten Sie die folgenden wichtigen Hinweise:


- Informieren Sie sich unbedingt über die Voraussetzungen der Installation von Azure AD Connect – insbesondere die erforderliche Hardware, und welche Konten und Berechtigungen für die Installation von Azure AD Connect erforderlich sind. Die für die Installation von Azure AD Connect erforderlichen Berechtigungen sind im Modus „Vorhandene Datenbank verwenden“ mit denen der „benutzerdefinierten“ Installation identisch.
- Die zur Installation verwendete Version von Azure AD Connect muss die folgenden Kriterien erfüllen:
    - 1.1.613.0 oder höher UND
    - Mindestens die zuletzt mit der ADSync-Datenbank verwendete Version von Azure AD Connect. Wenn die zur Installation verwendete Azure AD Connect-Version höher ist als die zuletzt mit der ADSync-Datenbank verwendete Version, kann eine vollständige Synchronisierung erforderlich sein.  Dies ist erforderlich, wenn zwischen den beiden Versionen Schema- oder Synchronisierungsregeländerungen vorliegen. 
- Die verwendete ADSync-Datenbank sollte einen relativ aktuellen Synchronisierungsstatus aufweisen. Die letzte Synchronisierung mit der vorhandenen ADSync-Datenbank sollte innerhalb der letzten drei Wochen stattgefunden haben.
- Bei Installation von Azure AD Connect mit der Methode „Vorhandene Datenbank verwenden“ wird die auf der vorherigen Azure AD Connect-Serverinstanz konfigurierte Anmeldemethode nicht beibehalten. Darüber hinaus können Sie die Anmeldemethode während der Installation nicht konfigurieren. Sie können die Anmeldemethode erst konfigurieren, nachdem die Installation abgeschlossen ist.
- Mehrere Azure AD Connect-Serverinstanzen können nicht dieselbe ADSync-Datenbank nutzen. Die Methode „Vorhandene Datenbank verwenden“ ermöglicht Ihnen das Wiederverwenden einer vorhandenen ADSync-Datenbank mit einer neuen Azure AD Connect-Serverinstanz. Sie unterstützt nicht die gemeinsame Nutzung.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>Schritte zum Installieren von Azure AD Connect mit dem Modus „Vorhandene Datenbank verwenden“
1.  Laden Sie den Azure AD Connect-Installer (AzureADConnect.MSI) auf den Windows-Server herunter. Doppelklicken Sie auf den Azure AD Connect-Installer, um die Installation von Azure AD Connect zu starten.
2.  Sobald die MSI-Installation abgeschlossen ist, wird der Azure AD Connect-Assistent mit dem Setup im Express-Modus gestartet. Schließen Sie den Bildschirm durch Klicken auf das Symbol „Beenden“.
![Willkommen](media/active-directory-aadconnect-existing-database/db1.png)
3.  Rufen Sie eine neue Eingabeaufforderung oder PowerShell-Sitzung auf. Navigieren Sie zum Ordner <drive>\Programme\Microsoft Azure AD Connect. Führen Sie den Befehl „.\AzureADConnect.exe /useexistingdatabase“ aus, um den Azure AD Connect-Assistenten im Setupmodus „Vorhandene Datenbank verwenden“ auszuführen.
![PowerShell](media/active-directory-aadconnect-existing-database/db2.png)
4.  Sie werden mit dem Bildschirm „Willkommen bei Azure AD Connect“ begrüßt. Wenn Sie den Lizenzbedingungen und dem Datenschutzhinweis zugestimmt haben, klicken Sie auf **Fortfahren**.
![Willkommen](media/active-directory-aadconnect-existing-database/db3.png)
5.  Auf dem Bildschirm **Erforderliche Komponenten installieren** ist die Option **Vorhandenen SQL Server-Computer verwenden** aktiviert. Geben Sie den Namen der SQL-Server-Instanz an, auf der die ADSync-Datenbank gehostet wird. Wenn die zum Hosten der ADSync-Datenbank verwendete SQL Engine-Instanz nicht die Standardinstanz auf dem SQL Server-Computer ist, müssen Sie den Namen der SQL Engine-Instanz angeben. Darüber hinaus müssen Sie, wenn das SQL-Durchsuchen nicht aktiviert ist, auch die Portnummer der SQL Engine-Instanz angeben. Beispiel:         
![Willkommen](media/active-directory-aadconnect-existing-database/db4.png)           

6.  Auf dem Bildschirm **Mit Azure AD verbinden** müssen Sie die Anmeldeinformationen als globaler Administrator Ihres Azure AD-Verzeichnisses angeben. Sie sollten ein Konto in der standardmäßigen Domäne „onmicrosoft.com“ verwenden. Dieses Konto dient ausschließlich der Erstellung eines Dienstkontos in Azure AD und wird nach Abschluss des Assistenten nicht mehr verwendet.
![Herstellen einer Verbindung](media/active-directory-aadconnect-existing-database/db5.png)
 
7.  Auf dem Bildschirm **Verzeichnisse verbinden** wird die für die Verzeichnissynchronisierung konfigurierte vorhandene AD-Gesamtstruktur mit einem roten Kreuzsymbol aufgelistet. Zum Synchronisieren der Änderungen von einer lokalen AD-Gesamtstruktur ist ein AD DS-Konto erforderlich. Der Azure AD Connect-Assistent kann nicht die Anmeldeinformationen des in der ADSync-Datenbank gespeicherten AD DS-Kontos abrufen, da die Anmeldeinformationen verschlüsselt sind und nur von der vorherigen Azure AD Connect-Serverinstanz entschlüsselt werden können. Klicken Sie auf **Anmeldeinformationen ändern**, um das AD DS-Konto für die AD-Gesamtstruktur anzugeben.
![Verzeichnisse](media/active-directory-aadconnect-existing-database/db6.png)
 
 
8.  Im Popupdialogfeld können Sie entweder (i) eine Enterprise-Administratoranmeldeinfo eingeben und Azure AD Connect das AD DS-Konto für Sie erstellen lassen, oder (ii) das AD DS-Konto selbst erstellen und Azure AD Connect die Anmeldeinformationen angeben. Nachdem Sie eine Option ausgewählt und die erforderlichen Anmeldeinformationen angeben haben, klicken Sie auf **OK**, um das Popupdialogfeld zu schließen.
![Willkommen](media/active-directory-aadconnect-existing-database/db7.png)
 
 
9.  Sobald die Anmeldeinformationen bereitgestellt sind, wird das rote Kreuzsymbol durch ein grünes Häkchensymbol ersetzt. Klicken Sie auf **Weiter**.

![Willkommen](media/active-directory-aadconnect-existing-database/db8.png)
 
 
10. Klicken Sie auf dem Bildschirm **Bereit zur Konfiguration** auf **Installieren**.
![Willkommen](media/active-directory-aadconnect-existing-database/db9.png)
 
 
11. Nachdem die Installation abgeschlossen ist, wird die Azure AD Connect-Serverinstanz automatisch für den Stagingmodus aktiviert. Sie sollten die Serverkonfiguration und ausstehenden Exporte vor dem Deaktivieren des Stagingmodus auf unerwartete Änderungen überprüfen. 

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Sie Azure AD Connect installiert haben, können Sie [die Installation überprüfen und Lizenzen zuweisen](active-directory-aadconnect-whats-next.md).
- Weitere Informationen zu diesen Features, die mit der Installation aktiviert wurden: [Verhindern von versehentlichen Löschungen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) und [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).
- Weitere Informationen zu folgenden allgemeinen Themen: [Scheduler und Auslösen der Synchronisierung](active-directory-aadconnectsync-feature-scheduler.md).
- Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

