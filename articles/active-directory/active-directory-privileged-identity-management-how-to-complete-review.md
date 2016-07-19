<properties
   pageTitle="Abschließen einer Zugriffsüberprüfung | Microsoft Azure"
   description="Nachdem Sie eine Überprüfung in Azure AD Privileged Identity Management gestartet haben, erfahren Sie, wie Sie sie abschließen und die Ergebnisse anzeigen"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# Abschließen einer Zugriffsüberprüfung in Azure AD Privileged Identity Management


Nachdem eine [Sicherheitsüberprüfung gestartet wurde](active-directory-privileged-identity-management-how-to-start-security-review.md), können Administratoren für privilegierte Rollen den privilegierten Zugriff überprüfen. Azure AD Privileged Identity Management (PIM) sendet automatisch eine E-Mail, in der Benutzer aufgefordert werden, ihren Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten, können Sie die Anweisungen unter [Ausführen einer Sicherheitsüberprüfung](active-directory-privileged-identity-management-how-to-perform-security-review.md) senden.

Wenn der Zeitraum für die Sicherheitsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## Verwalten von Sicherheitsüberprüfungen

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und wählen Sie auf dem Dashboard die Anwendung **Azure AD Privileged Identity Management** aus.
2. Wählen Sie auf dem Dashboard den Abschnitt **Zugriffsüberprüfungen** aus.
3. Wählen Sie die Zugriffsüberprüfung aus, die Sie verwalten möchten.

Auf dem Detailblatt der Zugriffsüberprüfung stehen eine Reihe von Optionen zum Verwalten dieser Überprüfung zur Verfügung.

![Schaltflächen der PIM-Zugriffsüberprüfung – Screenshot][1]

### Erinnerung

Wenn eine Zugriffsüberprüfung so eingerichtet ist, dass die Benutzer sich selbst überprüfen, wird über die Schaltfläche **Erinnerung** eine Benachrichtigung gesendet.

### Beenden

Alle Zugriffsüberprüfungen weisen ein Enddatum auf, Sie können jedoch die Schaltfläche **Beenden** verwenden, um sie zu einem frühen Zeitpunkt abzuschließen. Wenn nicht alle Benutzer zu diesem Zeitpunkt überprüft wurden, ist dies nach dem Beenden der Überprüfung nicht mehr möglich. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

### Anwenden

Nachdem eine Überprüfung abgeschlossen wurde, weil das Enddatum erreicht ist oder weil sie manuell beendet wurde, wird mit der Schaltfläche **Übernehmen** das Ergebnis der Überprüfung implementiert. Wenn bei der Überprüfung der Zugriff eine Benutzers verweigert wurde, ist dies der Schritt, mit dem die Rollenzuweisung entfernt wird.

### Export

Wenn Sie die Ergebnisse der Sicherheitsüberprüfung manuell anwenden möchten, können Sie die Überprüfung exportieren. Über die Schaltfläche **Export** können Sie den Download einer CSV-Datei starten. Sie können die Ergebnisse in Excel oder einem anderen Programm verwalten, in dem sich CSV-Dateien öffnen lassen.

### Löschen

Wenn Sie an einer Überprüfung nicht weiter interessiert sind, löschen Sie sie. Über die Schaltfläche **Löschen** entfernen Sie die Überprüfung aus der PIM-Anwendung.

> [AZURE.IMPORTANT] Vor dem Löschvorgang wird keine Warnung angezeigt. Vergewissern Sie sich daher, dass Sie die Überprüfung tatsächlich löschen möchten.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png

<!---HONumber=AcomDC_0706_2016-->