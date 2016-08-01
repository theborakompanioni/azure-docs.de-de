<properties 
   pageTitle="Übersicht über die Sicherheit in Data Lake-Speicher | Microsoft Azure" 
   description="Es wird beschrieben, dass Azure Data Lake-Speicher ein sicherer Big Data-Speicher ist." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/18/2016"
   ms.author="nitinme"/>

# Sicherheit im Azure Data Lake-Speicher

Viele Unternehmen nutzen Big Data-Analysen, um sich geschäftliche Einblicke zu verschaffen und intelligente Entscheidungen treffen zu können. Diese Unternehmen verfügen häufig über eine komplexe und regulierte Umgebung mit immer mehr unterschiedlichen Benutzern. Daher ist es für Unternehmen sehr wichtig sicherzustellen, dass kritische Geschäftsdaten sicher und mit dem richtigen Zugriffsgrad für relevante Benutzer gespeichert werden. Der Azure Data Lake-Speicher ist so konzipiert, dass diese Sicherheitsanforderungen erfüllt werden. Dieser Artikel enthält Informationen zu den Sicherheitsfunktionen des Azure Data Lake-Speichers (ADLS), z.B.:

* Authentifizierung
* Autorisierung
* Netzwerkisolation
* Datenschutz
* Überwachung

 
## Authentifizierung und Identitätsverwaltung

Die Authentifizierung ist der Prozess, mit dem Benutzer ihre Identität nachweisen, wenn sie mit dem Data Lake-Speicher oder anderen Diensten mit Verbindung zum Data Lake-Speicher interagieren. Für die Identitätsverwaltung und Authentifizierung nutzt Data Lake-Speicher [Azure Active Directory](../active-directory/active-directory-whatis.md) (AAD). Hierbei handelt es sich um eine umfassende Cloud-Lösung für die Identitäts- und Zugriffsverwaltung, mit der die Verwaltung von Benutzern und Gruppen vereinfacht wird.

Heutzutage kann jedes Azure-Abonnement mit einer Azure Active Directory-Instanz verknüpft werden. Nur in diesem Verzeichnis definierte Benutzer- und Dienstidentitäten können auf Ihren Data Lake-Speicher zugreifen, indem sie das Azure-Portal, Befehlszeilentools oder mit dem Data Lake-Speicher-SDK erstellte Clientanwendungen verwenden. Die Nutzung von Azure Active Directory als zentrales Verfahren für die Zugriffssteuerung hat die folgenden wichtigen Vorteile:

* Die Verwaltung des Identitätslebenszyklus wird vereinfacht. Die Identität eines Benutzers oder Diensts (Dienstprinzipalidentität) kann schnell erstellt und widerrufen werden, indem das Konto im Verzeichnis einfach gelöscht oder deaktiviert wird.

* [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) wird unterstützt, wodurch eine zusätzliche Sicherheitsebene für die Benutzeranmeldung und Transaktionen geschaffen wird.

* Benutzer können die Authentifizierung von jedem Client mit offen zugänglichen Standardprotokollen durchführen, z.B. OAuth und OpenID.

* Außerdem wird der Verbund mit Unternehmensverzeichnisdiensten und Cloudidentitätsanbietern ermöglicht.

## Autorisierung und Zugriffssteuerung

Nachdem ein Benutzer von AAD für den Zugriff auf den Azure Data Lake-Speicher authentifiziert wurde, werden die Zugriffsberechtigungen für den Data Lake-Speicher per Autorisierung gesteuert. Data Lake Store trennt die Autorisierung von konto- und datenbezogenen Aktivitäten auf folgende Weise.

* [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md) (Role-Based Access Control, RBAC) von Azure zur Kontoverwaltung
* POSIX ACL für den Zugriff auf die Daten im Store.

### Verwenden von RBAC für die Kontoverwaltung

Standardmäßig werden vier grundlegende Rollen definiert. Diese ermöglichen unterschiedliche Vorgänge für ein Data Lake-Speicher-Konto über das Portal, PowerShell-Cmdlets und REST-APIs. Die Rollen **Besitzer** und **Mitwirkender** lassen verschiedene Verwaltungsfunktionen für das Konto zu. Benutzer, die nur mit Daten interagieren, können Sie der Rolle **Leser** hinzufügen.

![RBAC-Rollen](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-Rollen")

Beachten Sie Folgendes: Die Zuweisung dieser Rollen dient zwar der Kontoverwaltung, aber je nach Rolle kann sich dies auch auf Berechtigungen für den Datenzugriff auswirken. Für Vorgänge, die Benutzer im Dateisystem durchführen können, müssen Sie Zugriffssteuerungslisten (Access Control Lists, ACLs) verwenden. Unten ist eine Zusammenfassung der Verwaltungsrechte und Datenzugriffsrechte für diese Rollen angegeben.

| Roles | Verwaltungsrechte | Datenzugriffsrechte | Erklärung |
|--------------------------|---------------------------------|--------------------|-------------|
| Keine Rolle zugewiesen | Keine | Per ACL gesteuert | In diesen Fällen können Benutzer nicht das Azure-Portal oder die Azure PowerShell-Cmdlets verwenden, um im Data Lake-Speicher zu navigieren. Diese Benutzer müssen sich ausschließlich auf Befehlszeilentools verlassen. |
| Besitzer | Alle | Alle | Der Besitzer ist ein „Superuser“, sodass Sie mit der Rolle „Besitzer“ alle Bereiche verwalten können und Vollzugriff auf die Daten haben. | 
| Leser | Schreibgeschützt | Per ACL gesteuert | Ein „Leser“ kann alle Daten zur Kontoverwaltung anzeigen, z.B. welcher Benutzer welcher Rolle zugewiesen ist, aber er kann keine Änderungen vornehmen. |
| Mitwirkender | Alle Rechte mit Ausnahme de Hinzufügens und Entfernens von Rollen | Per ACL gesteuert | Ein „Mitwirkender“ kann andere Aspekte eines Kontos verwalten, z.B. das Erstellen/Verwalten von Warnungen, Bereitstellung usw. Ein „Mitwirkender“ kann keine Rollen hinzufügen oder entfernen. |
| Benutzerzugriffsadministrator | Rollen hinzufügen und entfernen | Per ACL gesteuert | Mit der Rolle „Benutzerzugriffsadministrator“ können Sie den Benutzerzugriff auf Konten verwalten. |

Eine Anleitung finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen zu Azure Data Lake Store-Konten](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### Verwenden von ACLs für Vorgänge in Dateisystemen

Azure Data Lake Store ist ein hierarchisches Dateisystem wie HDFS und unterstützt [POSIX-ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Es werden Besitzern, besitzenden Gruppen und anderen Benutzern/Gruppen die Zugriffsrechte „Lesen“ (r), „Schreiben“ (w) und „Ausführen“ (x) für Ressourcen gewährt. In der öffentlichen Vorschauversion des Data Lake-Speichers (aktuelle Version) werden ACLs nur im Stammordner aktiviert. Dies bedeutet, dass die ACLs, die Sie auf den Stammordner anwenden, auch für alle untergeordneten Ordner und Dateien gelten. In zukünftigen Versionen können Sie ACLs für jede Datei bzw. jeden Ordner festlegen.

Es wird empfohlen, ACLs für viele Benutzer auf einmal mithilfe von [Sicherheitsgruppen](../active-directory/active-directory-accessmanagement-manage-groups.md) zu definieren. Gruppieren Sie die Benutzer in einer Sicherheitsgruppe, und weisen Sie die ACLs für die Datei und den Ordner dann dieser Sicherheitsgruppe zu. Dies ist bei der Bereitstellung von benutzerdefiniertem Zugriff nützlich, da Sie im Rahmen eines benutzerdefinierten Zugriffs nur maximal neun Einträge hinzufügen können. Weitere Informationen zum Schützen von Daten, die im Azure Data Lake Store gespeichert sind, mithilfe von AAD-Sicherheitsgruppen finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen als Zugriffssteuerungslisten zum Azure Data Lake Store-Dateisystem](data-lake-store-secure-data.md#filepermissions).

![Standardzugriff und benutzerdefinierten Zugriff auflisten](./media/data-lake-store-security-overview/adl.acl.2.png "Standardzugriff und benutzerdefinierten Zugriff auflisten")

## Netzwerkisolation

Mit dem Azure Data Lake-Speicher können Sie den Zugriff auf Ihren Datenspeicher auf Netzwerkebene noch weiter einschränken. Sie können die Firewall aktivieren und einen IP-Adressbereich für Ihre vertrauenswürdigen Clients definieren. Nach der Aktivierung können nur Clients, deren IP-Adressen innerhalb des definierten Bereichs liegen, eine Verbindung mit dem Speicher herstellen.

![Firewalleinstellungen und IP-Zugriff](./media/data-lake-store-security-overview/firewall-ip-access.png "Firewalleinstellungen und IP-Adresse")

## Datenschutz

Organisationen möchten sicherstellen, dass ihre unternehmenskritischen Daten über den gesamten Lebenszyklus hinweg geschützt sind. Für Daten im Übertragungszustand verwendet der Data Lake-Speicher das branchenübliche TLS-Protokoll (Transport Layer Security-Protokoll), um Daten zwischen Client und Data Lake-Speicher zu schützen.

Zukünftige Versionen werden auch über den Schutz von ruhenden Daten verfügen.

## Überwachungs- und Diagnoseprotokolle

Sie können Überwachungs- oder Diagnoseprotokolle verwenden. Dies richtet sich jeweils danach, ob Sie Protokolle für verwaltungsbezogene Aktivitäten oder datenbezogene Aktivitäten benötigen.

*  Für verwaltungsbezogene Aktivitäten werden die Azure Resource Manager-APIs verwendet. Sie tauchen im Azure-Portal in Form von Überwachungsprotokollen auf.
*  Für datenbezogene Aktivitäten werden die WebHDFS-APIs verwendet. Sie tauchen im Azure-Portal in Form von Diagnoseprotokollen auf.

### Überwachungsprotokolle

Zur Einhaltung von Bestimmungen benötigen Organisationen ggf. geeignete Überwachungspfade, falls ein Vorfall genauer untersucht werden muss. Data Lake-Speicher verfügt über integrierte Überwachungsfunktionen, mit denen alle Aktivitäten der Kontoverwaltung protokolliert werden.

Für Überwachungspfade der Kontoverwaltung können Sie die Spalten auswählen, die protokolliert werden sollen, und Überwachungsprotokolle in den Azure-Speicher exportieren.

![Überwachungsprotokolle](./media/data-lake-store-security-overview/audit-logs.png "Überwachungsprotokolle")

### Diagnoseprotokolle

Sie können die Überwachungspfade für den Datenzugriff über das Azure-Portal (**Diagnoseeinstellungen**) aktivieren und ein Azure-Blobspeicherkonto angeben, in dem die Protokolle gespeichert werden.

![Diagnoseprotokolle](./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnoseprotokolle")

Nachdem Sie die Diagnoseeinstellungen aktiviert haben, können Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** verfolgen.

## Zusammenfassung

Unternehmenskunden wünschen sich eine Cloudplattform für die Datenanalyse, die sicher und einfach zu verwenden ist. Azure Data Lake-Speicher wurde für die Erfüllung dieser Anforderungen konzipiert und verfügt über Identitätsverwaltung und Authentifizierung per Azure Active Direction-Integration, ACL-basierte Autorisierung, Netzwerkisolation, Verschlüsselung von Daten während der Übertragung und im ruhenden Zustand (in zukünftigen Versionen verfügbar) sowie Überwachung.

Wenn Sie neue Features für den Data Lake Store wünschen, senden Sie uns Ihr Feedback im [UserVoice-Forum](https://feedback.azure.com/forums/327234-data-lake).

## Weitere Informationen

- [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
- [Erste Schritte mit dem Data Lake-Speicher](data-lake-store-get-started-portal.md)
- [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)

<!---HONumber=AcomDC_0720_2016-->