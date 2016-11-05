---
title: 'Azure AD Connect-Synchronisierung: Bewährte Methoden zum Ändern der Standardkonfiguration | Microsoft Docs'
description: Stellt bewährte Methoden zum Ändern der Standardkonfiguration der Azure AD Connect-Synchronisierung vor.
services: active-directory
documentationcenter: ''
author: andkjell
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: markvi;andkjell

---
# Azure AD Connect-Synchronisierung: Bewährte Methoden zum Ändern der Standardkonfiguration
Dieses Thema dient zur Beschreibung der unterstützten und nicht unterstützten Änderungen der Azure AD Connect-Synchronisierung.

Die von Azure AD Connect erstellte Konfiguration funktioniert in der vorliegenden Form für die meisten Umgebungen, die ein lokales Active Directory-Verzeichnis mit Azure AD synchronisieren. In einigen Fällen müssen jedoch einige Änderungen an einer Konfiguration vorgenommen werden, um bestimmte Anforderungen zu erfüllen.

## Änderungen des Dienstkontos
Die Azure AD Connect-Synchronisierung läuft unter einem Dienstkonto, das vom Installations-Assistenten erstellt wurde. Dieses Dienstkonto enthält die Verschlüsselungsschlüssel für die von der Synchronisierung verwendete Datenbank. Es ist mit einem 127 Zeichen langen Kennwort erstellt, das nicht abläuft.

* Das Ändern oder Zurücksetzen des Kennworts des Dienstkontos wird **nicht unterstützt**. Dadurch würden die Verschlüsselungsschlüssel gelöscht, und der Dienst wäre nicht in der Lage, auf die Datenbank zuzugreifen und zu starten.

## Änderungen am Scheduler
Ab den Versionen von Build 1.1 (Februar 2016) können Sie den [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) so konfigurieren, dass ein anderer Synchronisierungszyklus als der Standardwert von 30 Minuten verwendet wird.

## Änderungen an Synchronisierungsregeln
Der Installations-Assistent verfügt über eine Konfiguration, die für die meisten gängigen Szenarien funktioniert. Falls Sie Änderungen an der Konfiguration vornehmen müssen, müssen Sie diese Regeln befolgen, um weiterhin über eine unterstützte Konfiguration zu verfügen.

* Sie können [Attributflüsse ändern](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes), wenn die standardmäßig festgelegten direkten Attributflüsse für Ihre Organisation nicht geeignet sind.
* Wenn Sie das [„Fließen“ eines Attributs verhindern](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) und alle vorhandenen Attributwerte in Azure AD entfernen möchten, müssen Sie für dieses Szenario eine Regel erstellen.
* [Deaktivieren Sie eine unerwünschte Synchronisierungsregel](#disable-an-unwanted-sync-rule), statt sie zu löschen. Eine gelöschte Regel wird bei einem Upgrade neu erstellt.
* Zum [Ändern einer standardmäßigen Regel](#change-an-out-of-box-rule) müssen Sie eine Kopie der Originalregel erstellen und die standardmäßige Regel deaktivieren. Der Synchronisierungsregel-Editor fordert Sie dazu auf und unterstützt Sie dabei.
* Exportieren Sie Ihre benutzerdefinierten Synchronisierungsregeln mit dem Synchronisierungsregel-Editor. Vom Editor erhalten Sie ein PowerShell-Skript, mit dem Sie die Regeln bei einem Notfallwiederherstellungsszenario problemlos neu erstellen können.

> [!WARNING]
> Die standardmäßigen Synchronisierungsregeln verfügen über einen Fingerabdruck. Wenn Sie diese Regeln ändern, stimmt der Fingerabdruck nicht mehr überein. In Zukunft treten unter Umständen Probleme auf, wenn Sie eine neue Version von Azure AD Connect anwenden möchten. Führen Sie Änderungen nur wie in diesem Artikel beschrieben durch.
> 
> 

### Deaktivieren einer unerwünschten Synchronisierungsregel
Löschen Sie keine standardmäßige Synchronisierungsregel. Sie wird beim nächsten Upgrade wiederhergestellt.

In einigen Fällen erstellt der Installations-Assistent eine Konfiguration, die für Ihre Topologie nicht funktioniert. Falls Sie beispielsweise über eine Topologie mit Kontoressourcengesamtstruktur verfügen, das Schema in der Kontogesamtstruktur aber um das Exchange-Schema erweitert haben, werden sowohl für die Kontogesamtstruktur als auch für die Ressourcengesamtstruktur Regeln für Exchange erstellt. In diesem Fall müssen Sie die Synchronisierungsregel für Exchange deaktivieren.

![Deaktivierte Synchronisierungsregel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

In der Abbildung oben wurde mit dem Installations-Assistenten ein altes Exchange 2003-Schema in der Kontogesamtstruktur gefunden. Diese Schemaerweiterung wurde hinzugefügt, bevor die Ressourcengesamtstruktur in die Umgebung von Fabrikam eingeführt wurde. Um sicherzustellen, dass keine Attribute aus der alten Exchange-Implementierung synchronisiert werden, sollte die Synchronisierungsregel wie gezeigt deaktiviert werden.

### Ändern einer standardmäßigen Regel
Wenn Sie Änderungen an einer standardmäßigen Regel vornehmen müssen, erstellen Sie eine Kopie der standardmäßigen Regel und deaktivieren Sie die ursprüngliche Regel. Nehmen Sie an der geklonten Regel dann die gewünschten Änderungen vor. Der Synchronisierungsregel-Editor unterstützt Sie bei diesen Schritten. Wenn Sie eine standardmäßige Regel öffnen, wird dieses Dialogfeld angezeigt: ![Warnung für standardmäßige Regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Wählen Sie **Ja**, um eine Kopie der Regel zu erstellen. Die geklonte Regel wird geöffnet. ![Geklonte Regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Nehmen Sie an dieser geklonten Regel die erforderlichen Änderungen für Bereich, Verknüpfung und Transformationen vor.

## Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0907_2016-->