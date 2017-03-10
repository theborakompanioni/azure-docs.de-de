---
title: "Schützen einer Key Vault-Instanz | Microsoft-Dokumentation"
description: "Verwalten Sie Zugriffsberechtigungen für Key Vault zur Verwaltung von Tresoren, Schlüsseln und Geheimnissen. Authentifizierungs- und Autorisierungsmodell für Key Vault und Schützen einer Key Vault-Instanz"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
translationtype: Human Translation
ms.sourcegitcommit: 36e0a52013b8d12c7e66c5955756a61a2c72b7dc
ms.openlocfilehash: c3507aed3cc44d6360b8ba3ddf172e1437c1227a
ms.lasthandoff: 01/05/2017


---
# <a name="secure-your-key-vault"></a>Schützen einer Key Vault-Instanz
Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter) für Ihre Cloudanwendungen. Da es sich hierbei um vertrauliche und geschäftskritische Daten handelt, empfiehlt es sich, den Zugriff auf Key Vault-Instanzen so zu konfigurieren, dass nur autorisierte Anwendungen und Benutzer auf Key Vault zugreifen können. Dieser Artikel enthält eine Übersicht über das Key Vault-Zugriffsmodell sowie Informationen zur Authentifizierung und Autorisierung. Außerdem erfahren Sie anhand eines Beispiels, wie Sie den Zugriff auf Key Vault für Ihre Cloudanwendungen schützen.

## <a name="overview"></a>Übersicht
Der Zugriff auf eine Key Vault-Instanz wird über zwei separate Schnittstellen gesteuert: die Verwaltungsebene und die Datenebene. Auf beiden Ebenen kann ein Aufrufer (ein Benutzer oder eine Anwendung) erst nach ordnungsgemäßer Authentifizierung und Autorisierung auf Key Vault zugreifen. Bei der Authentifizierung wird die Identität des Aufrufers ermittelt. Bei der Autorisierung wird dagegen bestimmt, welche Vorgänge der Aufrufer ausführen darf.

Für die Authentifizierung wird sowohl auf der Verwaltungs- als auch auf der Datenebene Azure Active Directory verwendet. Für die Autorisierung wird auf der Verwaltungsebene die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet, während auf der Datenebene Key Vault-Zugriffsrichtlinien zum Einsatz kommen.

Im Anschluss finden Sie eine kurze Übersicht über die behandelten Themen:

[Authentifizierung über Azure Active Directory](#authentication-using-azure-active-direcrory): In diesem Abschnitt erfahren Sie, wie sich ein Aufrufer mit Azure Active Directory authentifiziert, um über die Verwaltungs- oder Datenebene auf eine Key Vault-Instanz zuzugreifen. 

[Verwaltungsebene und Datenebene](#management-plane-and-data-plane): Hierbei handelt es sich um zwei Zugriffsebenen für den Zugriff auf Ihre Key Vault-Instanz. Jede Zugriffsebene unterstützt bestimmte Vorgänge. In diesem Abschnitt werden die Zugriffsendpunkte, die unterstützten Vorgänge und die Zugriffssteuerungsmethoden der jeweiligen Ebene beschrieben. 

[Zugriffssteuerung auf der Verwaltungsebene](#management-plane-access-control): Dieser Abschnitt beschäftigt sich mit der Gewährung des Zugriffs auf Verwaltungsebenenvorgänge mithilfe der rollenbasierten Zugriffssteuerung.

[Zugriffssteuerung auf der Datenebene](#data-plane-access-control): In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Key Vault-Zugriffsrichtlinien den Zugriff auf die Datenebene steuern.

[Beispiel](#example): Dieses Beispiel veranschaulicht, wie Sie die Zugriffssteuerung für Ihre Key Vault-Instanz einrichten, um drei Teams (Sicherheitsteam, Entwickler/Betreiber und Prüfer) die Ausführung bestimmter Aufgaben für die Entwicklung, Verwaltung und Überwachung einer Anwendung in Azure zu ermöglichen.

## <a name="authentication-using-azure-active-directory"></a>Authentifizierung über Azure Active Directory
Wenn Sie unter einem Azure-Abonnement eine Key Vault-Instanz erstellen, wird sie automatisch mit dem Azure Active Directory-Mandanten des Abonnements verknüpft. Alle Aufrufer (Benutzer und Anwendungen) müssen bei diesem Mandanten registriert sein, um auf die Key Vault-Instanz zugreifen zu können. Eine Anwendung oder ein Benutzer muss sich vor dem Zugriff auf die Key Vault-Instanz über Azure Active Directory authentifizieren. Das gilt sowohl auf der Verwaltungs- als auch auf der Datenebene. In beiden Fällen kann eine Anwendung auf zwei Arten auf Key Vault zugreifen:

* **Benutzer- und App-Zugriff**: Diese Methode wird üblicherweise für Anwendungen verwendet, die im Auftrag eines angemeldeten Benutzers auf Key Vault zugreifen. Beispiele hierfür wären etwa Azure PowerShell und das Azure-Portal. Benutzern kann auf zwei Arten Zugriff gewährt werden: Bei der ersten Option wird Benutzern Zugriff gewährt, damit sie von einer beliebigen Anwendung aus auf Key Vault zugreifen können. Bei der zweiten Option wird einem Benutzer nur bei Verwendung einer bestimmten Anwendung Zugriff auf Key Vault gewährt. (Letzteres wird auch als Verbundidentität bezeichnet.) 
* **Nur App-Zugriff**: Diese Methode ist für Anwendungen vorgesehen, die Daemon-Dienste, Hintergrundaufträge und Ähnliches ausführen. Die Identität der Anwendung erhält Zugriff auf die Key Vault-Instanz.

Bei beiden Anwendungstypen authentifiziert sich die Anwendung über Azure Active Directory mit einer der [unterstützten Authentifizierungsmethoden](../active-directory/active-directory-authentication-scenarios.md) und bezieht ein Token. Welche Authentifizierungsmethode verwendet wird, hängt von der Art der Anwendung ab. Anschließend sendet die Anwendung unter Verwendung des Tokens eine REST-API-Anforderung an Key Vault. Im Falle eines Zugriffs auf die Verwaltungsebene werden die Anforderungen über einen Azure Resource Manager-Endpunkt weitergeleitet. Beim Zugriff auf die Datenebene kommunizieren die Anwendungen hingegen direkt mit einem Key Vault-Endpunkt. Ausführlichere Informationen finden Sie in der [Gesamtdarstellung des Authentifizierungsablaufs](../active-directory/active-directory-protocols-oauth-code.md). 

Der Ressourcenname, für den die Anwendung ein Token anfordert, hängt davon ab, ob die Anwendung auf die Verwaltungs- oder auf die Datenebene zugreift. Folglich ist der Ressourcenname je nach Azure-Umgebung entweder ein Verwaltungsebenen- oder ein Datenebenen-Endpunkt, wie in der Tabelle in einem späteren Abschnitt beschrieben.

Die Verwendung eines einzelnen Authentifizierungsmechanismus für die Verwaltungs- und die Datenebene hat einige Vorteile:

* Organisationen können den Zugriff auf alle ihre Key Vault-Instanzen steuern.
* Wenn ein Benutzer aus der Organisation ausscheidet, verliert er umgehend den Zugriff auf sämtliche Key Vault-Instanzen in der Organisation.
* Organisationen können die Authentifizierung über die Optionen in Azure Active Directory anpassen und so beispielsweise die mehrstufige Authentifizierung aktivieren, um die Sicherheit zu verbessern.

## <a name="management-plane-and-data-plane"></a>Verwaltungsebene und Datenebene
Azure Key Vault ist ein Azure-Dienst und im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar. Wenn Sie eine Key Vault-Instanz erstellen, erhalten Sie einen virtuellen Container, in dem Sie andere Objekte wie Schlüssel, Geheimnisse und Zertifikate erstellen können. Anschließend greifen Sie über die Verwaltungs- oder über die Datenebene auf Ihre Key Vault-Instanz zu, um bestimmte Vorgänge auszuführen. Die Schnittstelle der Verwaltungsebene dient zum Verwalten der eigentlichen Key Vault-Instanz (also etwa zum Erstellen, Löschen und Aktualisieren von Key Vault-Attributen sowie zum Festlegen von Zugriffsrichtlinien für die Datenebene). Die Schnittstelle der Datenebene dient zum Hinzufügen, Löschen, Ändern und Verwenden der in Ihrer Key Vault-Instanz gespeicherten Schlüssel, Geheimnisse und Zertifikate.

Der Zugriff auf die Schnittstellen der Verwaltungs- und der Datenebene erfolgt über unterschiedliche Endpunkte (wie in der Tabelle zu sehen). Die zweite Spalte in der Tabelle gibt Aufschluss über die DNS-Namen für diese Endpunkte in verschiedenen Azure-Umgebungen. In der dritten Spalte werden die Vorgänge beschrieben, die Sie über die jeweilige Zugriffsebene ausführen können. Jede Zugriffsebene besitzt zudem einen eigenen Zugriffssteuerungsmechanismus: Für die Verwaltungsebene wird die Zugriffssteuerung über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure Resource Manager festgelegt, für die Zugriffssteuerung der Datenebene werden hingegen Key Vault-Zugriffsrichtlinien verwendet.

| Zugriffsebene | Zugriffsendpunkte | Vorgänge | Zugriffssteuerungsmechanismus |
| --- | --- | --- | --- |
| Verwaltungsebene |**Global:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br>  management.microsoftazure.de:443 |Erstellen/Lesen/Aktualisieren/Löschen von Key Vault <br> Festlegen von Zugriffsrichtlinien für Key Vault<br>Festlegen von Tags für Key Vault |Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) im Rahmen von Azure Resource Manager |
| Datenebene |**Global:**<br> &lt;Tresorname&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;Tresorname&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;Tresorname&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Tresorname&gt;.vault.microsoftazure.de:443 |Für Schlüssel: decrypt, encrypt, unwrapKey, wrapKey, verify, sign, get, list, update, create, import, delete, backup, restore<br><br> Für Geheimnisse: get, list, set, delete |Key Vault-Zugriffsrichtlinie |

Die Zugriffsteuerungen für die Verwaltungs- und die Datenebene sind voneinander unabhängig. Wenn Sie also beispielsweise einer Anwendung die Verwendung von Schlüsseln in einer Key Vault-Instanz ermöglichen möchten, müssen Sie ihr lediglich mithilfe von Key Vault-Zugriffsrichtlinien Zugriffsberechtigungen für die Datenebene gewähren, da die Anwendung keinen Zugriff auf die Verwaltungsebene benötigt. Umgekehrt gilt: Wenn ein Benutzer Lesezugriff auf Tresoreigenschaften und Tags, aber keinen Zugriff auf Schlüssel, Geheimnisse oder Zertifikate haben soll, können Sie ihm mithilfe von RBAC Lesezugriff gewähren, da in diesem Fall kein Zugriff auf die Datenebene erforderlich ist.

## <a name="management-plane-access-control"></a>Zugriffssteuerung auf der Verwaltungsebene
Auf der Verwaltungsebene werden Vorgänge ausgeführt, die sich auf die eigentliche Key Vault-Instanz auswirken. Hier können Sie beispielsweise eine Key Vault-Instanz erstellen oder löschen. Sie können eine Liste aller Tresore in einem Abonnement abrufen. Sie können Key Vault-Eigenschaften (etwa SKU und Tags) abrufen und Key Vault-Zugriffsrichtlinien festlegen, um zu steuern, welche Benutzer und Anwendungen auf die Schlüssel und Geheimnisse in Key Vault zugreifen können. Die Zugriffssteuerung auf der Verwaltungsebene basiert auf RBAC. Eine umfassende Liste mit den Key Vault-Vorgängen, die über die Verwaltungsebene ausgeführt werden können, finden Sie in der Tabelle im vorherigen Abschnitt. 

### <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)
Jedes Azure-Abonnement hat ein Azure Active Directory. Benutzern, Gruppen und Anwendungen aus diesem Verzeichnis kann Verwaltungszugriff auf Ressourcen im Azure-Abonnement gewährt werden, für die das Azure Resource Manager-Bereitstellungsmodell verwendet wird. Diese Art von Zugriffssteuerung wird als rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) bezeichnet. Zur Verwaltung dieses Zugriffs können Sie das [Azure-Portal](https://portal.azure.com/), die [Tools der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), [PowerShell](/powershell/azureps-cmdlets-docs) oder die [Azure Resource Manager-REST-APIs](https://msdn.microsoft.com/library/azure/dn906885.aspx) verwenden.

Bei Verwendung des Azure Resource Manager-Modells erstellen Sie Ihre Key Vault-Instanz in einer Ressourcengruppe und steuern den Zugriff auf die Verwaltungsebene dieser Key Vault-Instanz mithilfe von Azure Active Directory. So können Sie beispielsweise Benutzern oder einer Gruppe die Verwaltung von Key Vault-Instanzen in einer bestimmten Ressourcengruppe ermöglichen.

Durch Zuweisen geeigneter RBAC-Rollen können Sie Benutzern, Gruppen und Anwendungen in einem bestimmten Bereich Zugriff gewähren. Wenn Sie also etwa einem Benutzer Zugriff für die Verwaltung von Key Vault-Instanzen gewähren möchten, weisen Sie ihm für einen bestimmten Bereich die vordefinierte Rolle „Key Vault-Mitwirkender“ zu. Der Bereich wäre in diesem Fall entweder ein Abonnement, eine Ressourcengruppe oder einfach eine bestimmte Key Vault-Instanz. Eine auf Abonnementebene zugewiesene Rolle gilt für alle Ressourcengruppen und Ressourcen innerhalb des Abonnements. Eine auf Ressourcengruppenebene zugewiesene Rolle gilt für alle Ressourcen in der Ressourcengruppe. Eine für eine bestimmte Ressource zugewiesene Rolle gilt nur für diese Ressource. Es stehen mehrere vordefinierte Rollen zur Verfügung (siehe [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md)). Sollte hier keine passende Rolle für Sie dabei sein, können Sie auch eigene Rollen erstellen.

> [!IMPORTANT]
> Hinweis: Falls ein Benutzer für eine Key Vault-Verwaltungsebene über Mitwirkungsberechtigungen (RBAC) verfügt, kann er sich durch Festlegen einer Key Vault-Zugriffsrichtlinie (steuert den Zugriff auf den Datenebene) selbst Zugriff auf die Datenebene gewähren. Es empfiehlt sich daher, sehr genau darauf zu achten, wer als Mitwirkender Zugriff auf Ihre Key Vault-Instanzen hat, um sicherzustellen, dass nur autorisierte Benutzer auf Ihre Key Vault-Instanzen, Schlüssel, Geheimnisse und Zertifikate zugreifen und diese verwalten können.
> 
> 

## <a name="data-plane-access-control"></a>Zugriffssteuerung auf der Datenebene
Auf der Key Vault-Datenebene werden Vorgänge ausgeführt, die sich auf die Objekte in einer Key Vault-Instanz auswirken (also etwa auf Schlüssel, Geheimnisse und Zertifikate).  Hierzu zählen Schlüsselvorgänge wie das Erstellen, Importieren, Aktualisieren, Auflisten, Sichern und Wiederstellen von Schlüsseln sowie kryptografische Vorgänge wie das Signieren, Überprüfen, Verschlüsseln, Entschlüsseln, Verpacken, Entpacken und Festlegen von Tags und anderen Attributen für Schlüssel. Beispiele für geheimnisspezifische Vorgänge wären etwa Abrufen, Festlegen, Auflisten und Löschen.

Der Datenebenenzugriff wird durch Festlegen von Zugriffsrichtlinien für eine Key Vault-Instanz gewährt. Ein Benutzer, eine Gruppe oder eine Anwendung muss über Mitwirkungsberechtigungen (RBAC) für die Verwaltungsebene einer Key Vault-Instanz verfügen, um Zugriffsrichtlinien für diese Instanz festlegen zu können. Einem Benutzer, einer Gruppe oder einer Anwendung kann Zugriff zum Ausführen bestimmter Vorgänge für Schlüssel oder Geheimnisse in einer Key Vault-Instanz gewährt werden. Pro Key Vault-Instanz werden bis zu 16 Zugriffsrichtlinien unterstützt. Erstellen Sie eine Azure Active Directory-Sicherheitsgruppe, und fügen Sie ihr Benutzer hinzu, um mehreren Benutzern Datenebenenzugriff auf eine Key Vault-Instanz zu gewähren.

### <a name="key-vault-access-policies"></a>Key Vault-Zugriffsrichtlinien
Mit Key Vault-Zugriffsrichtlinien können separate Berechtigungen für Schlüssel, Geheimnisse und Zertifikate gewährt werden. Dadurch haben Sie beispielsweise die Möglichkeit, einem Benutzer nur Zugriff auf Schlüssel, aber keine Berechtigungen für Geheimnisse zu gewähren. Zugriffsberechtigungen für Schlüssel, Geheimnisse oder Zertifikate gelten jedoch auf Tresorebene. Mit anderen Worten: Key Vault-Zugriffsrichtlinien unterstützen keine Berechtigungen auf Objektebene. Zum Festlegen von Zugriffsrichtlinien für eine Key Vault-Instanz können Sie das [Azure-Portal](https://portal.azure.com/), die [Tools der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), [PowerShell](/powershell/azureps-cmdlets-docs) oder die [REST-APIs für die Key Vault-Verwaltung](https://msdn.microsoft.com/library/azure/mt620024.aspx) verwenden.

> [!IMPORTANT]
> Beachten Sie, dass Key Vault-Zugriffsrichtlinien auf Tresorebene gelten. Wenn einem Benutzer also beispielsweise die Berechtigung zum Erstellen und Löschen von Schlüsseln gewährt wird, kann er diese Vorgänge für alle Schlüssel in dieser Key Vault-Instanz ausführen.
> 
> 

## <a name="example"></a>Beispiel
Angenommen, Sie entwickeln eine Anwendung, die ein Zertifikat für SSL, Azure Storage zum Speichern von Daten und einen RSA-Schlüssel mit 2048 Bit für Signierungsvorgänge verwendet. Nehmen wir außerdem an, diese Anwendung wird auf einem virtuellen Computer (oder in einer VM-Skalierungsgruppe) ausgeführt. In diesem Fall können Sie alle Anwendungsgeheimnisse sowie das Bootstrap-Zertifikat, mit dem sich die Anwendung über Azure Active Directory authentifiziert, in einer Key Vault-Instanz speichern.

Im Anschluss finden Sie eine Zusammenfassung aller Schlüssel und Geheimnisse, die in einer Key Vault-Instanz gespeichert werden sollen:

* **SSL-Zertifikat**: Wird für SSL verwendet.
* **Speicherschlüssel**: Wird verwendet, um Zugriff auf das Speicherkonto erhalten.
* **RSA-Schlüssel (2048 Bit)**: Wird für Signierungsvorgänge verwendet.
* **Bootstrap-Zertifikat**: Wird für die Authentifizierung bei Azure Active Directory verwendet, um Zugriff auf die Key Vault-Instanz zu erhalten, damit der Speicherschlüssel abgerufen und der RSA-Schlüssel zum Signieren verwendet werden kann.

Kommen wir nun zu den Benutzern, die diese Anwendung verwalten, bereitstellen und überprüfen. In diesem Beispiel werden drei Rollen verwendet:

* **Sicherheitsteam:** Hierbei handelt es sich üblicherweise um IT-Mitarbeiter aus der Abteilung des Sicherheitsverantwortlichen für die gesamte Organisation (Chief Security Officer, CSO) oder um Mitarbeiter mit vergleichbaren Aufgaben, die für die sichere Verwahrung von Geheimnissen wie SSL-Zertifikaten, RSA-Schlüsseln für Signaturen, Verbindungszeichenfolgen für Datenbanken, Speicherkontoschlüsseln und Ähnlichem zuständig sind.
* **Entwickler/Betreiber:** Hierbei handelt es sich um die Mitarbeiter, die die Anwendung entwickeln und anschließend in Azure bereitstellen. Sie sind in der Regel nicht Teil des Sicherheitsteams und sollten daher keinen Zugriff auf vertrauliche Daten wie SSL-Zertifikate und RSA-Schlüssel haben – im Gegensatz zu der Anwendung, die sie entwickeln.
* **Prüfer:** Hierbei handelt es sich in der Regel um eine weitere Personengruppe, die sich nicht mit den Entwicklern oder den IT-Mitarbeitern im Allgemeinen überschneidet. Diese Gruppe prüft die ordnungsgemäße Verwendung und Verwaltung von Zertifikaten, Schlüsseln und Ähnlichem und achtet auf die Einhaltung der Datensicherheitsstandards. 

Darüber hinaus gibt es auch noch die Rolle des Abonnement- oder Ressourcengruppenadministrators. Dieser ist für unser Beispiel zwar nicht relevant, soll aber dennoch nicht unerwähnt bleiben. Der Abonnementadministrator richtet die anfänglichen Zugriffsberechtigungen für das Sicherheitsteam ein. In unserem Beispiel gehen wir davon aus, dass der Abonnementadministrator dem Sicherheitsteam Zugriff auf eine Ressourcengruppe gewährt hat, die alle für diese Anwendung benötigten Ressourcen enthält.

Als Nächstes sehen wir uns an, welche Aktionen die einzelnen Rollen im Kontext dieser Anwendung ausführen können.

* **Sicherheitsteam**
  * Erstellt Key Vault-Instanzen
  * Aktiviert die Key Vault-Protokollierung
  * Fügt Schlüssel/Geheimnisse hinzu
  * Erstellt Schlüsselsicherungen für die Notfallwiederherstellung
  * Legt Key Vault-Zugriffsrichtlinien fest, um Benutzern und Anwendungen Zugriff zu gewähren, damit sie bestimmte Vorgänge ausführen können
  * Ändert regelmäßig Schlüssel/Geheimnisse
* **Entwickler/Betreiber**
  * Rufen Verweise auf Bootstrap- und SSL-Zertifikate (Fingerabdrücke), Speicherschlüssel (geheimer URI) und den Signaturschlüssel (Schlüssel-URI) vom Sicherheitsteam ab
  * Entwickeln eine Anwendung, die programmgesteuert auf Schlüssel und Geheimnisse zugreift, und stellen diese Anwendung bereit
* **Prüfer**
  * Überprüfen anhand von Verwendungsprotokollen die ordnungsgemäße Verwendung von Schlüsseln/Geheimnissen sowie die Einhaltung von Datensicherheitsstandards

Als Nächstes sehen wir uns an, welche Key Vault-Zugriffsberechtigungen die einzelnen Rollen (und die Anwendung) für ihre jeweiligen Aufgaben benötigen. 

| Benutzerrolle | Berechtigungen auf Verwaltungsebene | Berechtigungen auf Datenebene |
| --- | --- | --- |
| Sicherheitsteam |Key Vault-Mitwirkender |Schlüssel: Sichern, Erstellen, Löschen, Abrufen, Importieren, Auflisten, Wiederherstellen <br> Geheimnisse: alle |
| Entwickler/Betreiber |Key Vault-Bereitstellungsberechtigung, damit die bereitgestellten virtuellen Computer Geheimnisse aus der Key Vault-Instanz abrufen können |Keine |
| Prüfer |Keine |Schlüssel: Auflisten<br>Geheimnisse: Auflisten |
| Anwendung |Keine |Schlüssel: Signieren<br>Geheimnisse: Abrufen |

> [!NOTE]
> Prüfer müssen über die Berechtigung zum Auflisten von Schlüsseln und Geheimnissen verfügen, damit sie Attribute für Schlüssel und Geheimnisse inspizieren können, die nicht in den Protokollen ausgegeben werden. Hierzu zählen etwa Tags sowie Aktivierungs- und Ablauftermine.
> 
> 

Neben der Berechtigung für Key Vault benötigen alle drei Rollen auch Zugriff auf andere Ressourcen. Beispielsweise müssen sie zum Bereitstellen von virtuellen Computern (oder Web-Apps und Ähnlichem) berechtigt sein. Entwickler/Betreiber benötigen zudem Mitwirkungszugriff auf diese Ressourcentypen. Prüfer benötigen Lesezugriff auf das Speicherkonto mit den gespeicherten Key Vault-Protokollen.

Da sich dieser Artikel in erster Linie mit dem Schutz des Zugriffs auf Ihre Key Vault-Instanz beschäftigt, gehen wir hier nur auf die Aspekte ein, die für dieses Thema relevant sind, und sparen etwa die Bereitstellung von Zertifikaten sowie den programmgesteuerten Zugriff auf Schlüssel und Geheimnisse aus. Diese Details werden an anderer Stelle behandelt. Informationen zum Bereitstellen von in Key Vault gespeicherten Zertifikaten für virtuelle Computer finden Sie in [diesem Blogbeitrag](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/). Beispielcode zur Authentifizierung bei Azure AD mithilfe eines Bootstrap-Zertifikats, um Zugriff auf Key Vault zu erlangen, finden Sie [hier](https://www.microsoft.com/download/details.aspx?id=45343).

Die meisten der Zugriffsberechtigungen können über das Azure-Portal gewährt werden. Wenn Sie allerdings präzisere Berechtigungen benötigen, müssen Sie unter Umständen Azure PowerShell (oder die Azure-Befehlszeilenschnittstelle) verwenden, um das gewünschte Ergebnis zu erzielen. 

Den im Anschluss bereitgestellten PowerShell-Codeausschnitten liegen folgende Annahmen zugrunde:

* Der Azure Active Directory-Administrator hat für die drei Rollen folgende Sicherheitsgruppen erstellt: „Contoso Security Team“, „Contoso App Devops“ und „Contoso App Auditors“. Der Administrator hat den Gruppen entsprechende Benutzer hinzugefügt.
* Die Ressourcen befinden sich alle in der Ressourcengruppe **ContosoAppRG**. Die Protokolle werden in **contosologstorage** gespeichert. 
* Die Key Vault-Instanz **ContosoKeyVault** und das für die Key Vault-Protokolle verwendete Speicherkonto **contosologstorage** müssen sich am gleichen Azure-Standort befinden.

Der Abonnementadministrator weist dem Sicherheitsteam zunächst die Rollen „key vault Contributor“ (Key Vault-Mitwirkender) und „User Access Administrator“ (Benutzerzugriffsadministrator) zu. Dadurch kann das Sicherheitsteam den Zugriff auf andere Ressourcen sowie Key Vault-Instanzen in der Ressourcengruppe „ContosoAppRG“ verwalten.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Das folgende Skript veranschaulicht, wie das Sicherheitsteam eine Key Vault-Instanz erstellen, die Protokollierung einrichten und Zugriffsberechtigungen für andere Rollen und die Anwendung festlegen kann. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

Die benutzerdefinierte Rolle kann nur dem Abonnement zugewiesen werden, unter dem die Ressourcengruppe „ContosoAppRG“ erstellt wird. Falls die gleichen benutzerdefinierten Rollen für andere Projekte unter anderen Abonnements verwendet werden, kann der Bereich um weitere Abonnements erweitert werden.

Die Zuweisung der benutzerdefinierten Rolle für die Entwickler/Betreiber im Zusammenhang mit der Bereitstellungs-/Aktionsberechtigung ist auf die Ressourcengruppe beschränkt. Dadurch können die Geheimnisse (SSL-Zertifikat und Bootstrap-Zertifikat) nur von virtuellen Computern abgerufen werden, die in der Ressourcengruppe „ContosoAppRG“ erstellt wurden. Virtuelle Computer, die vom Entwickler-/Betreiberteam in einer anderen Ressourcengruppe erstellt werden, können diese Geheimnisse nicht abrufen. Dies gilt auch, wenn ihnen die geheimen URIs bekannt sind.

Bei diesem Beispiel handelt es sich um ein einfaches Szenario. In der Praxis können die Szenarien komplexer ausfallen und eine entsprechende Anpassung der Berechtigungen für Ihre Key Vault-Instanz erfordern. In unserem Beispiel gehen wir etwa davon aus, dass das Sicherheitsteam die Verweise auf Schlüssel und Geheimnisse (URIs und Fingerabdrücke) bereitstellt, die das Entwickler-/Betreiberteam in seinen Anwendungen benötigt. Daher müssen sie den Entwicklern/Betreibern keinerlei Zugriff auf die Datenebene gewähren. Beachten Sie außerdem, dass es in diesem Beispiel in erster Linie um den Schutz Ihrer Key Vault-Instanz geht. Ähnliche Überlegungen sollten auch für den Schutz Ihrer [virtuellen Computer](https://azure.microsoft.com/services/virtual-machines/security/), [Speicherkonten](../storage/storage-security-guide.md) und anderen Azure-Ressourcen angestellt werden.

> [!NOTE]
> Hinweis: Dieses Beispiel zeigt, wie der Key Vault-Zugriff in der Produktion gesperrt wird. Die Entwickler müssen über ein eigenes Abonnement oder über eine eigene Ressourcengruppe mit uneingeschränkten Berechtigungen verfügen, um ihre Tresore, ihre virtuellen Computer und ihr Speicherkonto für die Anwendungsentwicklung verwalten zu können.
> 
> 

## <a name="resources"></a>Ressourcen
* [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../active-directory/role-based-access-control-configure.md)
  
  Dieser Artikel beschreibt die rollenbasierte Steuerung des Zugriffs auf Azure Active Directory, und wie sie funktioniert.
* [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md)
  
  In diesem Artikel werden alle integrierten, in RBAC verfügbaren Rollen ausführlich beschrieben.
* [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Dieser Artikel erläutert die Resource Manager-Bereitstellung sowie klassische Bereitstellungsmodelle und erläutert die Vorteile der Verwendung der Resource Manager- und Ressourcengruppen.
* [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  
  In diesem Artikel erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung mit Azure PowerShell verwalten.
* [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](../active-directory/role-based-access-control-manage-access-rest.md)
  
  Dieser Artikel beschreibt die Verwendung der REST-API zum Verwalten von RBAC.
* [Role-Based Access Control for Microsoft Azure from Ignite (Rollenbasierte Zugriffssteuerung für Microsoft Azure über Ignite)](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Dies ist ein Link zu einem Video auf Channel 9 von der MS Ignite-Konferenz 2015. Thema dieser Sitzung sind die Zugriffsverwaltungs- und Berichtsfunktionen in Azure und die Untersuchung bewährter Verfahren für das Sichern des Zugriffs auf Azure-Abonnements mit Azure Active Directory.
* [Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)
  
  Dieser Artikel beschreibt den vollständige OAuth 2.0-Ablauf für die Authentifizierung über Azure Active Directory.
* [REST-APIs für die Key Vault-Verwaltung](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Dieses Dokument ist die Referenz für die REST-APIs zur programmgesteuerten Key Vault-Verwaltung (einschließlich Festlegung von Key Vault-Zugriffsrichtlinien).
* [Key Vault-REST-APIs](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Link zur Referenzdokumentation für Key Vault-REST-APIs.
* [Schlüsselzugriffssteuerung](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Link zur Referenzdokumentation für die Geheimniszugriffssteuerung.
* [Geheimniszugriffssteuerung](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Link zur Referenzdokumentation für die Geheimniszugriffssteuerung.
* [Festlegen](https://msdn.microsoft.com/library/mt603625.aspx) und [Entfernen](https://msdn.microsoft.com/library/mt619427.aspx) von Key Vault-Zugriffsrichtlinien mithilfe von PowerShell
  
  Links zur Referenzdokumentation für PowerShell-Cmdlets zum Verwalten von Key Vault-Zugriffsrichtlinien.

## <a name="next-steps"></a>Nächste Schritte
Ein Tutorial mit ersten Schritten für Administratoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md).

Weitere Informationen zur Nutzungsprotokollierung für Key Vault finden Sie unter [Azure-Schlüsseltresor-Protokollierung](key-vault-logging.md).

Weitere Informationen zur Verwendung von Schlüsseln und Geheimnissen mit Azure Key Vault finden Sie unter [About keys, secrets, and certificates](https://msdn.microsoft.com/library/azure/dn903623.aspx) (Informationen zu Schlüsseln, Geheimnissen und Zertifikaten).

Sollten Sie Fragen zu Key Vault haben, besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).


