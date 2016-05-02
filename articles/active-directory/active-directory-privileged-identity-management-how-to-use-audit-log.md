<properties
   pageTitle="Verwenden des Überwachungsprotokolls | Microsoft Azure"
   description="Erfahren Sie, wie Sie in der Erweiterung Azure Privileged Identity Management das Überwachungsprotokoll verwenden."
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

# Verwenden des Überwachungsprotokolls in Azure AD Privileged Identity Management

Im Privileged Identity Management (PIM)-Überwachungsprotokoll werden alle Benutzerzuweisungen und Aktivierungen in einem angegebenen Zeitraum angezeigt.

## Navigieren zum Überwachungsprotokoll
Wählen Sie auf dem Dashboard im [Azure-Portal](https://portal.azure.com) die App **Azure AD Privileged Identity Management** aus. Dort können Sie das Überwachungsprotokoll aufrufen, indem Sie im PIM-Dashboard auf **Überwachungsverlauf** klicken.

## Der Graph des Überwachungsprotokolls
Sie können mithilfe des Überwachungsprotokolls die Gesamtzahl der Aktivierungen, die maximale Anzahl der Aktivierungen pro Tag und die durchschnittliche Anzahl der Aktivierungen pro Tag in einem Liniendiagramm anzeigen. Sie können auch die Daten nach Rolle filtern, wenn der Überwachungsverlauf mehrere Rollen enthält.

Sortieren Sie das Protokoll mithilfe der Schaltflächen **Zeit**, **Aktion** und **Rolle**.

## Die Überwachungsprotokollliste
Die Überwachungsprotokollliste enthält die folgenden Spalten:

- **Anforderer** – der Benutzer, der die Rollenaktivierung oder -änderung angefordert hat. Wenn der Wert „Azure System“ lautet, suchen Sie im Azure-Überwachungsprotokoll nach weiteren Informationen.
- **Benutzer** – der Benutzer, der eine Rolle aktiviert oder einer Rolle zugewiesen ist.
- **Rolle** – die Rolle, die dem Benutzer zugewiesen ist oder die durch den Benutzer aktiviert wurde.
- **Aktion** – die vom Anforderer ausgeführten Aktionen. Mögliche Aktionen: Zuweisung, Aufhebung einer Zuweisung, Aktivierung oder Deaktivierung.
- **Zeit** – der Zeitpunkt, zu dem die Aktion erfolgt ist.
- **Argumentation** – wenn während der Aktivierung Text in das Feld "Grund" eingegeben wurde, wird er hier angezeigt.
- **Ablauf** – nur für die Aktivierung von Rollen relevant.

## Filtern des Überwachungsprotokolls

Sie können die im Überwachungsprotokoll angezeigten Informationen filtern, indem Sie auf die Schaltfläche **Filter** klicken. Das Blatt **Diagrammparameter aktualisieren** wird angezeigt.

Klicken Sie nach dem Einrichten der Filter auf **Aktualisieren**, um die Daten im Protokoll zu filtern. Wenn die Daten nicht sofort angezeigt werden, aktualisieren Sie die Seite.


### Ändern des Datumsbereichs
Verwenden Sie die Schaltflächen **Heute**, **Letzte Woche**, **Letzter Monat** oder **Benutzerdefiniert**, um den Zeitbereich des Überwachungsprotokolls zu ändern.

Wenn Sie auf **Benutzerdefiniert** klicken, werden die Datumsfelder **Von** und **Bis** angezeigt, um den Datumsbereich für das Protokoll anzugeben. Sie können die Datumsangaben im Format MM/TT/JJJJ eingeben oder auf das Symbol **Kalender** klicken und das Datum im Kalender auswählen.

### Ändern der Rollen im Protokoll

Aktivieren oder deaktivieren Sie das Kontrollkästchen **Rolle** neben jeder Rolle, die im Protokoll enthalten bzw. nicht enthalten sein soll.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->