<properties
   pageTitle="Abschließen einer Sicherheitsüberprüfung | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Anwendung Azure AD Privileged Identity Management eine Überprüfung abschließen."
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
   ms.date="05/19/2016"
   ms.author="kgremban"/>

# Abschließen einer Sicherheitsüberprüfung in Azure AD Privileged Identity Management


Nachdem eine [Sicherheitsüberprüfung gestartet wurde](active-directory-privileged-identity-management-how-to-start-security-review.md), können Administratoren für privilegierte Rollen den privilegierten Zugriff überprüfen. Azure AD Privileged Identity Management (PIM) sendet automatisch eine E-Mail, in der Benutzer aufgefordert werden, ihren Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten, können Sie die Anweisungen unter [Ausführen einer Sicherheitsüberprüfung](active-directory-privileged-identity-management-how-to-perform-security-review.md) senden.

Wenn der Zeitraum für die Sicherheitsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## Verwalten von Sicherheitsüberprüfungen

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und wählen Sie auf dem Dashboard die Anwendung **Azure AD Privileged Identity Management** aus.
2. Wählen Sie auf dem Dashboard den Abschnitt **Sicherheitsüberprüfungen**. Das Blatt **Sicherheitsüberprüfungen** wird angezeigt.
3. Wählen Sie die Sicherheitsüberprüfung aus, die Sie verwalten möchten. Das Detailblatt der Sicherheitsüberprüfung wird angezeigt.

Auf dem Detailblatt der Sicherheitsüberprüfung stehen drei Optionen zum Verwalten dieser Überprüfung zur Verfügung. Sie können die Überprüfung abschließen, die Details exportieren oder die Überprüfung löschen.

### Abschließen oder Beenden einer Überprüfung

Wenn Benutzern der Zugriff verweigert wurde, können Sie die Überprüfung abschließen, um diese Benutzerrollenzuweisung aus dem Verzeichnis zu entfernen. Über die Schaltfläche **Überprüfung beenden** wird die Überprüfung archiviert.

### Exportieren einer Überprüfung

Wenn Sie die Ergebnisse der Sicherheitsüberprüfung manuell anwenden möchten, können Sie die Überprüfung exportieren. Über die Schaltfläche **Export** können Sie den Download als CSV-Datei starten. Sie können die Ergebnisse in Excel oder einem anderen Programm verwalten, in dem sich CSV-Dateien öffnen lassen.

### Löschen einer Überprüfung

> [AZURE.IMPORTANT] Vor dem Löschvorgang wird keine Warnung angezeigt. Vergewissern Sie sich daher, dass Sie die Überprüfung tatsächlich löschen möchten.

Wenn Sie an einer Überprüfung nicht weiter interessiert sind, löschen Sie sie. Über die Schaltfläche **Löschen** entfernen Sie die Überprüfung aus der PIM-Anwendung.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0525_2016-->