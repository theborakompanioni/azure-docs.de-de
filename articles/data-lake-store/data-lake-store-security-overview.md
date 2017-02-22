---
title: "Übersicht über die Sicherheit in Data Lake Store | Microsoft Docs"
description: "Es wird beschrieben, warum Azure Data Lake Store ein Big-Data-Speicher mit erhöhter Sicherheit ist."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 59f072c7a8272fc04e1d662c0ab17e7ee4500fa6
ms.openlocfilehash: 59bff7e1f7bdfc1786c4ee47ab2a652e2df85289


---
# <a name="security-in-azure-data-lake-store"></a>Sicherheit im Azure Data Lake-Speicher
Viele Unternehmen nutzen Big-Data-Analysen zur Gewinnung von Unternehmenseinblicken, um fundierte Entscheidungen treffen zu können. Ein Unternehmen verfügt unter Umständen über eine komplexe und regulierte Umgebung mit einer wachsenden Zahl von unterschiedlichen Benutzern. Unternehmen müssen sicherstellen, dass wichtige Geschäftsdaten sicherer gespeichert werden und dass einzelnen Benutzern die richtige Zugriffsebene gewährt wird. Azure Data Lake Store ist so konzipiert, dass diese Sicherheitsanforderungen besser erfüllt werden können. In diesem Artikel werden die Sicherheitsfunktionen von Data Lake Store beschrieben, z.B.:

* Authentifizierung
* Autorisierung
* Netzwerkisolation
* Datenschutz
* Überwachung

## <a name="authentication-and-identity-management"></a>Authentifizierung und Identitätsverwaltung
Die Authentifizierung ist der Prozess, bei dem die Identität des Benutzers überprüft wird, wenn der Benutzer mit Data Lake Store oder einem anderen Dienst interagiert, für den eine Verbindung mit Data Lake Store hergestellt wird. Für die Identitätsverwaltung und Authentifizierung nutzt Data Lake Store [Azure Active Directory](../active-directory/active-directory-whatis.md). Hierbei handelt es sich um eine umfassende Cloud-Lösung für die Identitäts- und Zugriffsverwaltung, mit der die Verwaltung von Benutzern und Gruppen vereinfacht wird.

Jedes Azure-Abonnement kann mit einer Instanz von Azure Active Directory verknüpft werden. Nur Benutzer- und Dienstidentitäten, die in Ihrem Azure Active Directory-Dienst definiert sind, können auf Ihr Data Lake Store-Konto zugreifen. Hierfür werden das Azure-Portal, Befehlszeilentools oder Clientanwendungen verwendet, die von Ihrer Organisation mit dem Azure Data Lake Store SDK erstellt werden. Die Nutzung von Azure Active Directory als zentrales Verfahren für die Zugriffssteuerung hat die folgenden wichtigen Vorteile:

* Vereinfachte Verwaltung des Identitätslebenszyklus: Die Identität eines Benutzers oder Diensts (Dienstprinzipalidentität) kann schnell erstellt und widerrufen werden, indem das Konto im Verzeichnis einfach gelöscht oder deaktiviert wird.
* Multi-Factor Authentication: Per [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) wird eine zusätzliche Sicherheitsebene für die Benutzeranmeldung und für Transaktionen geschaffen.
* Authentifizierung von jedem Client über ein geöffnetes Standardprotokoll, z.B. OAuth oder OpenID.
* Verbund mit Unternehmensverzeichnisdiensten und Cloudidentitätsanbietern.

## <a name="authorization-and-access-control"></a>Autorisierung und Zugriffssteuerung
Nachdem ein Benutzer per Azure Active Directory authentifiziert wurde, damit der Benutzer auf Azure Data Lake Store zugreifen kann, werden von der Autorisierung die Zugriffsberechtigungen für Data Lake Store gesteuert. Data Lake Store trennt die Autorisierung von konto- und datenbezogenen Aktivitäten auf folgende Weise:

* [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md) (Role-Based Access Control, RBAC) von Azure zur Kontoverwaltung
* POSIX-ACL für den Zugriff auf die Daten im Store

### <a name="rbac-for-account-management"></a>RBAC für die Kontoverwaltung
Für Data Lake Store werden standardmäßig vier grundlegende Rollen definiert. Diese Rollen ermöglichen unterschiedliche Vorgänge für ein Data Lake Store-Konto über das Azure-Portal, PowerShell-Cmdlets und REST-APIs. Mit den Rollen „Besitzer“ und „Mitwirkender“ können verschiedene Verwaltungsfunktionen für das Konto durchgeführt werden. Sie können die Rolle „Leser“ Benutzern zuweisen, die nur mit Daten interagieren.

![RBAC-Rollen](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-Rollen")

Beachten Sie Folgendes: Obwohl Rollen für die Kontoverwaltung zugewiesen werden, wirken sich einige Rollen auf den Datenzugriff aus. Sie müssen ACLs verwenden, um den Zugriff auf Vorgänge zu steuern, die ein Benutzer im Dateisystem durchführen kann. Die folgende Tabelle enthält eine Zusammenfassung der Verwaltungsrechte und Datenzugriffsrechte für die Standardrollen.

| Roles | Verwaltungsrechte | Datenzugriffsrechte | Erklärung |
| --- | --- | --- | --- |
| Keine Rolle zugewiesen |Keine |Per ACL gesteuert |Der Benutzer kann nicht das Azure-Portal oder die Azure PowerShell-Cmdlets verwenden, um in Data Lake Store zu navigieren. Der Benutzer kann nur die Befehlszeilentools verwenden. |
| Besitzer |Alle |Alle |Die Rolle „Besitzer“ ist ein Superuser. Mit dieser Rolle können alle Bereiche verwaltet werden, und es besteht Vollzugriff auf die Daten. |
| Leser |Schreibgeschützt |Per ACL gesteuert |Mit der Rolle „Leser“ können alle Daten zur Kontoverwaltung angezeigt werden, z.B. welcher Benutzer welcher Rolle zugewiesen ist. Änderungen können mit der Rolle „Leser“ aber nicht vorgenommen werden. |
| Mitwirkender |Alle Rechte mit Ausnahme de Hinzufügens und Entfernens von Rollen |Per ACL gesteuert |Mit der Rolle „Mitwirkender“ können einige Aspekte eines Kontos verwaltet werden, z.B. Bereitstellungen und die Erstellung und Verwaltung von Warnungen. Das Hinzufügen oder Entfernen von Rollen ist mit der Rolle „Mitwirkender“ nicht möglich. |
| Benutzerzugriffsadministrator |Rollen hinzufügen und entfernen |Per ACL gesteuert |Mit der Rolle „Benutzerzugriffsadministrator“ können Sie den Benutzerzugriff auf Konten verwalten. |

Eine Anleitung finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen zu Data Lake Store-Konten](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Verwenden von ACLs für Vorgänge in Dateisystemen
Data Lake Store ist ein hierarchisches Dateisystem, wie beispielsweise Hadoop Distributed File System (HDFS), und unterstützt [POSIX-ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Es steuert die Berechtigungen Lesen, Schreiben und Ausführen (read (r), write (w) und execute (x)) für Ressourcen für die Rolle „Besitzer“, für die Gruppe „Besitzer“ und für andere Benutzer und Gruppen. In der öffentlichen Vorschauversion von Data Lake Store (aktuelle Version) können ACLs für Stammordner, Unterordner und einzelne Dateien aktiviert werden. Weitere Informationen zur Funktionsweise von ACLs im Kontext von Data Lake Store finden Sie unter [Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md).

Es wird empfohlen, mit [Sicherheitsgruppen](../active-directory/active-directory-accessmanagement-manage-groups.md)ACLs für mehrere Benutzer zu definieren. Fügen Sie Benutzer einer Sicherheitsgruppe hinzu, und weisen Sie dieser Sicherheitsgruppe dann die ACLs für eine Datei oder einen Ordner zu. Dies ist hilfreich, wenn Sie benutzerdefinierten Zugriff gewähren möchten, da Sie hierfür nur maximal neun Einträge hinzufügen können. Weitere Informationen dazu, wie Sie in Data Lake Store gespeicherte Daten mit Azure Active Directory-Sicherheitsgruppen besser schützen können, finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen als Zugriffssteuerungslisten zum Azure Data Lake-Speicher-Dateisystem](data-lake-store-secure-data.md#filepermissions).

![Auflisten von Standardzugriff und benutzerdefiniertem Zugriff](./media/data-lake-store-security-overview/adl.acl.2.png "Auflisten von Standardzugriff und benutzerdefiniertem Zugriff")

## <a name="network-isolation"></a>Netzwerkisolation
Verwenden Sie Data Lake Store, damit Sie den Zugriff auf Ihren Datenspeicher auf Netzwerkebene besser steuern können. Sie können Firewalls einrichten und einen IP-Adressbereich für Ihre vertrauenswürdigen Clients definieren. Mit einem IP-Adressbereich können nur Clients, die über eine IP-Adresse innerhalb des definierten Bereichs verfügen, eine Verbindung mit Data Lake Store herstellen.

![Firewall-Einstellungen und IP-Zugriff](./media/data-lake-store-security-overview/firewall-ip-access.png "Firewalleinstellungen und IP-Adresse")

## <a name="data-protection"></a>Datenschutz
Azure Data Lake Store schützt Ihre Daten während des gesamten Lebenszyklus. Für Daten im Übertragungszustand verwendet Data Lake Store das branchenübliche TLS-Protokoll (Transport Layer Security), um Daten über das Netzwerk zu schützen.

![Verschlüsselung in Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "Verschlüsselung in Data Lake Store")

Data Lake Store bietet auch eine Verschlüsselung für Daten, die im Konto gespeichert sind. Sie können auswählen, ob Ihre Daten verschlüsselt werden sollen oder nicht. Wenn Sie sich für die Verschlüsselung entscheiden, werden in Data Lake Store gespeicherte Daten vor dem Speichern auf persistenten Medien verschlüsselt. In einem solchen Fall verschlüsselt Data Lake Store automatisch Daten vor der persistenten Speicherung und entschlüsselt Daten vor dem Abruf, sodass der Vorgang für den Client, der auf die Daten zugreift, vollständig transparent ist. Auf der Clientseite ist zum Verschlüsseln/Entschlüsseln von Daten keine Änderung des Codes erforderlich.

Für die Schlüsselverwaltung bietet Data Lake Store zwei Modi zur Verwaltung Ihrer Master-Verschlüsselungsschlüssel (Master Encryption Keys, MEKs), die zum Entschlüsseln von Daten erforderlich sind, die im Data Lake Store gespeichert sind. Sie können die MEKs entweder von Data Lake Store für sich verwalten lassen, oder Sie verwenden das Azure Key Vault-Konto und bleiben im Besitz der MEKs. Beim Erstellen eines Data Lake Store-Kontos geben Sie die Art der Schlüsselverwaltung an. Weitere Informationen zum Bereitstellen einer verschlüsselungsbezogenen Konfiguration finden Sie unter [Erste Schritte mit Azure Data Lake Store im Azure-Portal](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Überwachungs- und Diagnoseprotokolle
Sie können Überwachungs- oder Diagnoseprotokolle verwenden. Dies richtet sich jeweils danach, ob Sie Protokolle für verwaltungsbezogene Aktivitäten oder datenbezogene Aktivitäten benötigen.

* Für verwaltungsbezogene Aktivitäten werden Azure Resource Manager-APIs verwendet. Sie tauchen im Azure-Portal in Form von Überwachungsprotokollen auf.
* Für datenbezogene Aktivitäten werden die WebHDFS-REST-APIs verwendet. Sie tauchen im Azure-Portal in Form von Diagnoseprotokollen auf.

### <a name="auditing-logs"></a>Überwachungsprotokolle
Zur Einhaltung von Vorschriften benötigt eine Organisation unter Umständen geeignete Überwachungspfade, falls bestimmte Vorfälle genauer untersucht werden müssen. Data Lake Store verfügt über integrierte Überwachungsfunktionen, und alle Aktivitäten der Kontoverwaltung werden protokolliert.

Zeigen Sie für Überwachungspfade der Kontoverwaltung die Spalten an, die Sie protokollieren möchten, und wählen Sie sie aus. Sie können die Überwachungsprotokolle auch in Azure Storage exportieren.

![Überwachungsprotokolle](./media/data-lake-store-security-overview/audit-logs.png "Überwachungsprotokolle")

### <a name="diagnostic-logs"></a>Diagnoseprotokolle
Sie können Datenzugriff-Überwachungspfade im Azure-Portal (in den Diagnoseeinstellungen) festlegen und ein Azure-Blobspeicherkonto erstellen, unter dem die Protokolle gespeichert werden.

![Diagnoseprotokolle](./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnoseprotokolle")

Nachdem Sie die Diagnoseeinstellungen konfiguriert haben, können Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** anzeigen.

Weitere Informationen zum Arbeiten mit Diagnoseprotokollen in Azure Data Lake Store finden Sie unter [Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Zusammenfassung
Unternehmenskunden wünschen sich eine Cloudplattform für die Datenanalyse, die sicher und einfach zu verwenden ist. Azure Data Lake Store wurde für die bessere Erfüllung dieser Anforderungen per Identitätsverwaltung und Authentifizierung mithilfe der Azure Active Directory-Integration, ACL-basierter Autorisierung, Netzwerkisolation, Verschlüsselung von Daten während der Übertragung und im ruhenden Zustand (in zukünftigen Versionen verfügbar) sowie Überwachung konzipiert.

Wenn Sie neue Features für Data Lake Store wünschen, können Sie uns Ihr Feedback im [UserVoice-Forum zu Data Lake Store](https://feedback.azure.com/forums/327234-data-lake)senden.

## <a name="see-also"></a>Siehe auch
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Erste Schritte mit Data Lake Store](data-lake-store-get-started-portal.md)
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)




<!--HONumber=Feb17_HO1-->


