<properties
   pageTitle="Starten einer Sicherheitsüberprüfung | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Anwendung Azure AD Privileged Identity Management eine Sicherheitsüberprüfung für privilegierte Identitäten erstellen."
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

# Starten einer Sicherheitsüberprüfung in Azure AD Privileged Identity Management

Rollenzuweisungen „veralten“, wenn Benutzer über privilegierten Zugriff verfügen, den sie nicht mehr benötigen. Um die Risiken im Zusammenhang mit diesen „veralteten“ Rollenzuweisungen zu verringern, sollten Sicherheitsadministratoren die den Benutzern zugewiesenen Rollen regelmäßig überprüfen. Dieses Dokument beschreibt die Schritte zum Starten einer Sicherheitsüberprüfung in Azure AD Privileged Identity Management (PIM).

## Starten einer Sicherheitsüberprüfung
> [AZURE.NOTE] Wenn Sie die PIM-Anwendung noch nicht zu Ihrem Dashboard im Azure-Portal hinzugefügt haben, finden Sie die notwendigen Schritte unter [Erste Schritte mit Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

Auf der Hauptseite der PIM-Anwendung gibt es drei Möglichkeiten, eine Sicherheitsüberprüfung zu starten:

- **Sicherheitsüberprüfungen** > **Überprüfen** > Schaltfläche **Überprüfen**
- **Rollen** > Schaltfläche **Überprüfen**
- Wählen Sie in der Rollenliste die zu überprüfende Rolle aus > Schaltfläche **Überprüfen**

Wenn Sie auf die Schaltfläche **Überprüfen** klicken, werden die Blätter **Rollenüberprüfung starten** und **Eine Rolle für die Überprüfung auswählen** angezeigt. Das Element **Eine Rolle für die Überprüfung auswählen** ist ausgewählt.

### Auswählen der zu überprüfenden Rolle

1. Wählen Sie auf dem Blatt **Eine Rolle für die Überprüfung auswählen** die gewünschte Rolle aus der Rollenliste aus. Sie können jeweils nur eine Rolle auswählen. Das Blatt **Eine Rolle für die Überprüfung auswählen** wird durch das Blatt **Prüfer auswählen** ersetzt. Bei der Auswahl der Prüfer stehen Ihnen zwei Optionen zur Verfügung:
  - Ich: Verwenden Sie diese Option, wenn Sie eine Vorschau zur Funktionsweise von Sicherheitsüberprüfungen anzeigen möchten, ohne andere Administratoren einzubeziehen.
  - Selbstüberprüfung durch Rollenmitglieder: Mit dieser Option können Benutzer ihre Rollenzuweisungen selbst überprüfen.
2. Wählen Sie eine der beiden Optionen aus, um die Arbeit mit den Überprüfungsdetails zu beginnen. Das Blatt **Standardeinstellungen ändern** wird angezeigt.

### Überprüfung mit Option "Ich"

Wenn Sie die Option "Ich" als Prüfer ausgewählt haben, fahren Sie mit der Sicherheitsüberprüfung fort. Weitere Informationen zum Abschließen der Überprüfung finden Sie unter [Azure Privileged Identity Management: Durchführen einer Sicherheitsüberprüfung](active-directory-privileged-identity-management-how-to-perform-security-review.md).

### Selbstüberprüfung durch Rollenmitglieder

Wenn Sie festgelegt haben, dass die Benutzer ihre Rollenzuweisungen selbst überprüfen sollen, führen Sie die folgenden Schritte aus, um die Überprüfung einzurichten und Benachrichtigungen zu senden.

1. Benennen Sie die Überprüfung, indem Sie den Namen der Überprüfung im Feld **Name** eingeben. Geben Sie der Überprüfung einen eindeutigen Namen, der die Überprüfung beschreibt und deren Verfolgung vereinfacht.
2. Geben Sie ein Startdatum für die Überprüfung im Feld **Startdatum** ein.
3. Geben Sie ein Enddatum für die Überprüfung im Feld **Enddatum** ein. Im Folgenden sind einige Aspekte aufgeführt, die Sie beim Festlegen des Enddatums für die Überprüfung beachten sollten:
  - Wie viele Personen werden überprüft?
  - Wie schnell können die Benutzer die PIM-Anwendung im Azure-Portal hinzufügen und die Überprüfung abschließen?
4. Klicken Sie auf dem Blatt **Standardwerte ändern** auf die Schaltfläche **OK**. Das Blatt wird geschlossen.
5. Klicken Sie auf dem Blatt **Rollenüberprüfung starten** auf die Schaltfläche **OK**. Das Blatt wird geschlossen. Im Hauptmenü des Azure-Portals wird eine Benachrichtigung angezeigt. Aktualisieren Sie das Dashboard, indem Sie auf die Schaltfläche **Aktualisieren** klicken. Die Sicherheitsüberprüfung wird im Abschnitt **Sicherheitsüberprüfungen** angezeigt.
6. Benachrichtigen Sie die Personen in der Rolle, dass sie die PIM-Anwendung hinzufügen und anschließend [ihren eigenen Administratorzugriff überprüfen](active-directory-privileged-identity-management-how-to-perform-security-review.md) sollen.  

## Verwalten der Sicherheitsüberprüfung

Sie können den Fortschritt der Überprüfungen durch die Benutzer im Azure AD PIM-Dashboard im Abschnitt mit den Sicherheitsüberprüfungen nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn [die Überprüfung abgeschlossen](active-directory-privileged-identity-management-how-to-complete-review.md) ist.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Inhaltsverzeichnis zu PIM
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->