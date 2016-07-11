<properties
	pageTitle="Azure AD Connect Sync: Erneutes Ausführen des Installations-Assistenten | Microsoft Azure"
	description="Erläutert die Funktionsweise des Installations-Assistenten, wenn Sie ihn ein zweites Mal ausführen."
	keywords="Beim erneuten Ausführen des Azure AD Connect-Installations-Assistenten haben Sie die Möglichkeit, die Wartungseinstellungen zu konfigurieren."
	services="active-directory"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/27/2016"
	ms.author="andkjell"/>


# Azure AD Connect Sync: Erneutes Ausführen des Installations-Assistenten
Beim erstmaligen Ausführen des Azure AD Connect-Installations-Assistenten werden Sie durch die Konfiguration Ihrer Installation geführt. Beim erneuten Ausführen des Installations-Assistenten werden Ihnen Wartungsoptionen angeboten.

Sie finden den Installations-Assistenten im Startmenü unter der Bezeichnung **Azure AD Connect**.

![Startmenü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Wenn Sie den Installations-Assistenten starten, wird Ihnen eine Seite mit folgenden Optionen angezeigt:

![Seite mit einer Liste zusätzlicher Aufgaben](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Wenn Sie AD FS mit Azure AD Connect installiert haben, stehen Ihnen noch weitere Optionen zur Verfügung. Die zusätzlichen Optionen für AD FS sind unter [AD FS-Verwaltung](active-directory-aadconnect-federation-management.md#ad-fs-management) dokumentiert.

Wählen Sie eine der Aufgaben aus, und klicken Sie auf **Weiter**, um fortzufahren.

> [AZURE.IMPORTANT] Solange der Installations-Assistent geöffnet ist, werden alle Vorgänge im Synchronisierungsmodul angehalten. Stellen Sie sicher, dass Sie den Installations-Assistenten schließen, sobald Sie Ihre Konfigurationsänderungen abgeschlossen haben.

## Aktuelle Konfiguration anzeigen
Über diese Option erhalten Sie eine Schnellansicht Ihrer aktuell konfigurierten Optionen.

![Seite mit einer Liste aller Optionen und deren Status](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Um zurückzugehen, klicken Sie auf **Zurück**. Mit einem Klick auf **Beenden** schließen Sie den Installations-Assistenten.

## Synchronisierungsoptionen anpassen
Mit dieser Option nehmen Sie Änderungen an der Synchronisierungskonfiguration vor. Ihnen wird eine Teilmenge von Optionen im Installationspfad der Konfiguration angezeigt. Diese wird Ihnen auch dann angezeigt, wenn Sie zunächst die Expressinstallation verwendet haben.

- [Hinzufügen weiterer Verzeichnisse](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Informationen zum Entfernen eines Verzeichnisses finden Sie unter [Löschen eines Connectors](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Ändern der Filterung von Domänen und Organisationseinheiten](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Entfernen gruppenspezifischer Filterregeln.
- [Ändern optionaler Features](active-directory-aadconnect-get-started-custom.md#optional-features).

Die weiteren Optionen aus der anfänglichen Installation können nicht geändert werden und sind nicht verfügbar. Dazu zählen:

- Ändern des Attributs zur Verwendung für userPrincipalName und sourceAnchor.
- Ändern der Verknüpfungsmethode für Objekte aus einer anderen Gesamtstruktur.
- Aktivieren der gruppenbasierten Filterung.

## Aktualisieren des Verzeichnisschemas
Diese Option wird verwendet, wenn Sie das Schema in einer Ihrer lokalen AD DS-Gesamtstrukturen verändert haben. Zum Beispiel haben Sie möglicherweise Exchange installiert oder ein Upgrade auf ein Windows Server 2012-Schema mit Geräteobjekten ausgeführt. In diesem Fall müssen Sie Azure AD Connect anweisen, das Schema erneut aus den AD DS zu lesen und den Cache zu aktualisieren. Dies führt auch dazu, dass die Synchronisierungsregeln neu generiert werden. Wenn Sie beispielsweise das Exchange-Schema hinzufügen, werden der Konfiguration die Synchronisierungsregeln für Exchange hinzugefügt.

Wenn Sie diese Option auswählen, werden alle Verzeichnisse in Ihrer Konfiguration aufgeführt. Sie können die Standardeinstellung beibehalten und alle Gesamtstrukturen aktualisieren oder die Auswahl einiger Gesamtstrukturen aufheben.

![Seite mit einer Liste aller Verzeichnisse in der Umgebung](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## Stagingmodus konfigurieren
Mit dieser Option können Sie den Stagingmodus auf dem Server aktivieren und deaktivieren. Weitere Informationen zum Stagingmodus und dessen Verwendung finden Sie unter [Vorgänge](active-directory-aadconnectsync-operations.md#staging-mode).

Die Option zeigt an, ob Staging derzeit aktiviert oder deaktiviert ist: ![Option, die auch den aktuellen Status des Stagingmodus anzeigt](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Um den Status zu ändern, wählen Sie diese Option aus, und aktivieren bzw. deaktivieren Sie das Kontrollkästchen. ![Option, die auch den aktuellen Status des Stagingmodus anzeigt](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## Benutzeranmeldung ändern
Mit dieser Option können Sie von der Kennwortsynchronisierung zum Verbund oder umgekehrt wechseln. Ein Wechsel zu **Nicht konfigurieren** ist nicht möglich.

Weitere Informationen zu dieser Option finden Sie unter [Benutzeranmeldung](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).


## Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md).

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0629_2016-->