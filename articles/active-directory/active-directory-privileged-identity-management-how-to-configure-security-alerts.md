<properties
   pageTitle="Konfigurieren von Sicherheitswarnungen | Microsoft Azure"
   description="Erfahren Sie, wie Sie Sicherheitswarnungen für die Erweiterung Azure Privileged Identity Management konfigurieren."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="04/15/2016"
   ms.author="kgremban"/>

# Konfigurieren von Sicherheitswarnungen in Azure AD Privileged Identity Management

## Sicherheitswarnungen
Azure Privileged Identity Management (PIM) generiert folgende Warnungen, die auf dem PIM-Dashboard im Abschnitt „Warnungen“ angezeigt werden können.

| Warnung | Trigger | Empfehlungen |
| ----- | ------- | -------------- |
| **Permanente Aktivierung** | Ein Administrator wurde außerhalb von PIM dauerhaft zu einer Rolle zugewiesen. | Überprüfen Sie die neue Rollenzuweisung, und ändern Sie sie ggf. in eine temporäre Zuweisung. |
| **Verdächtige Aktivierungserneuerung privilegierter Rollen** | Es sind zu viele erneute Aktivierungen derselben Rolle in der in den Einstellungen festgelegten Zeit erfolgt. | Setzen Sie sich mit den Benutzern in Verbindung, um sicherzustellen, dass sie ihre Rollen erfolgreich aktivieren können. |
| **Für Rollenaktivierung ist schwache Authentifizierung konfiguriert** | Die Einstellungen enthalten Rollen ohne MFA. | Erwägen Sie, MFA für die Aktivierung aller Rollen anzufordern. |
| **Redundante Administratoren** | Es gibt temporäre Administratoren, die ihre Rollen längere Zeit nicht aktiviert haben. | Entfernen Sie Rollenzuweisungen, die nicht mehr benötigt werden. |
| **Zu viele globale Administratoren** | In den Einstellungen sind mehr globale Administratoren festgelegt als empfohlen. | Entfernen Sie Rollenzuweisungen, die nicht mehr benötigt werden, oder ändern Sie einige Zuweisungen in temporäre Zuweisungen. |

## Konfigurieren der Einstellungen für Sicherheitswarnungen

### Warnung „Verdächtige Aktivierungsverlängerungen für privilegierte Rollen“

Konfigurieren Sie die Einstellungen für den **Zeitrahmen für Aktivierungsverlängerung** und die **Anzahl von Aktivierungsverlängerungen**, um zu steuern, wann diese Warnung ausgelöst wird.

1. Wählen Sie im Dashboardabschnitt **Aktivität** die Option **Sicherheitswarnungen** aus. Das Blatt **Aktive Sicherheitswarnungen** wird angezeigt.
2. Klicken Sie auf **Einstellungen**.
3. Legen Sie den **Zeitrahmen für Aktivierungserneuerung** mit dem Schieberegler oder durch Eingabe der Anzahl von Minuten im Textfeld fest. Der maximale Wert beträgt 100.
4. Legen Sie die **Anzahl der Aktivierungserneuerungen** im Zeitrahmen fest. Verwenden Sie hierzu den Schieberegler, oder geben Sie im Textfeld die Anzahl der Erneuerungen ein. Der maximale Wert beträgt 100.
5. Klicken Sie auf **Speichern**.

### Warnung „Redundante Administratoren“
1. Wählen Sie im Dashboardabschnitt **Aktivität** die Option **Sicherheitswarnungen** aus. Das Blatt **Aktive Sicherheitswarnungen** wird angezeigt.
2. Klicken Sie auf **Einstellungen**.
3. Wählen Sie die zulässige Anzahl von Tagen ohne Rollenaktivierung aus, indem Sie den Schieberegler einstellen oder im Textfeld die Anzahl der Tage eingeben.
4. Klicken Sie auf **Speichern**.

### Warnung „Zu viele globale Administratoren“

Es gibt zwei Einstellungen, die diese Warnung auslösen können:
- **Mindestanzahl von globalen Administratoren** löst die Warnung aus, wenn die zulässige Anzahl von Administratoren überschritten wird.
- **Prozentsatz von globalen Administratoren** löst die Warnung aus, wenn der Prozentsatz von Administratoren, bei denen es sich um globale Administratoren handelt, höher ist als in den Einstellungen festgelegt.

1. Wählen Sie im Dashboardabschnitt **Aktivität** die Option **Sicherheitswarnungen** aus. Das Blatt **Aktive Sicherheitswarnungen** wird angezeigt.
2. Klicken Sie auf **Einstellungen**.
3. Legen Sie die **Minimale Anzahl globaler Administratoren** fest, indem Sie den Schieberegler einstellen oder im Textfeld die Zahl eingeben.
4. Legen Sie den **Prozentsatz von globalen Administratoren** fest, indem Sie den Schieberegler einstellen oder im Textfeld den Prozentsatz eingeben.
5. Klicken Sie auf **Speichern**.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->