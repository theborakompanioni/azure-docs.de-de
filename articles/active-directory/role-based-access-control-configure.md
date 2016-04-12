<properties
	pageTitle="Rollenbasierte Zugriffssteuerung in Azure Active Directory | Microsoft Azure"
	description="Führen Sie die ersten Schritte der Zugriffsverwaltung mit der rollenbasierten Zugriffssteuerung von Azure im Azure-Portal aus. Verwenden Sie Rollenzuweisungen, um in Ihrem Verzeichnis Berechtigungen zuzuweisen."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="identity"
	ms.date="03/30/2016"
	ms.author="kgremban"/>

# Rollenbasierte Access Control in Azure

## Rollenbasierte Access Control
Die rollenbasierte Access Control in Azure (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mithilfe von RBAC können Sie Aufgaben in Ihrem DevOps-Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. In diesem Artikel werden die Grundlagen der Zugriffsverwaltung vorgestellt, und dann erhalten Sie Hilfe bei den ersten Schritten mit RBAC im Azure-Portal.

### Grundlagen zur Zugriffsverwaltung in Azure
Jedes Azure-Abonnement ist mit einem Azure Active Directory-Verzeichnis (AD) verknüpft. Benutzer, Gruppen und Anwendungen aus diesem Verzeichnis können Ressourcen im Azure-Abonnement verwalten. Diese Zugriffsrechte werden mit dem Azure-Portal, Azure-Befehlszeilentools und Azure-Verwaltungs-APIs gewährt.

Der Zugriff wird gewährt, indem Benutzern, Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zugewiesen wird. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Ein Benutzer mit Zugriff auf eine Ressourcengruppe kann beispielsweise alle Ressourcen verwalten, die darin enthalten sind, z.B. Websites, virtuelle Computer und Subnetze.

![Beziehung zwischen Azure Active Directory-Elementen – Diagramm](./media/role-based-access-control-configure/rbac_aad.png)

Die von Ihnen zugewiesene RBAC-Rolle legt fest, welche Ressourcen der Benutzer, die Gruppe oder die Anwendung auf dieser Ebene verwalten kann.

### Integrierte Rollen
Azure RBAC verfügt über drei grundlegende Rollen, die für alle Ressourcentypen gelten:

- **Besitzer** verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren.
- **Mitwirkende** können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.
- **Leser** können vorhandene Azure-Ressourcen anzeigen.

Die verbleibenden RBAC-Rollen in Azure ermöglichen die Verwaltung von bestimmten Azure-Ressourcen. Mit der Rolle „Mitwirkender von virtuellen Computern“ können Benutzer beispielsweise virtuelle Computer erstellen und verwalten. Sie haben damit keinen Zugriff auf das virtuelle Netzwerk oder das Subnetz, mit dem der virtuelle Computer verbunden ist.

Unter [Integrierte RBAC-Rollen](role-based-access-built-in-roles.md) sind die Rollen aufgeführt, die in Azure verfügbar sind. Hierbei werden die Vorgänge und der Bereich angegeben, die mit jeder integrierten Rolle für Benutzer gewährt werden. Wenn Sie Ihre eigenen Rollen definieren möchten, um eine noch bessere Kontrolle zu haben, helfen Ihnen die Informationen zum Erstellen von [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md) weiter.

### Ressourcenhierarchie und Zugriffsvererbung
- Jedes **Abonnement** in Azure gehört jeweils nur zu einem Verzeichnis.
- Jede **Ressourcengruppe** gehört jeweils nur zu einem Abonnement.
- Jede **Ressource** gehört jeweils nur zu einer Ressourcengruppe.

Der Zugriff, den Sie auf übergeordneter Ebene gewähren, wird auf untergeordneter Ebene geerbt. Beispiel:

- Sie weisen die Rolle „Leser“ einer Azure AD-Gruppe im Abonnementbereich zu. Die Mitglieder dieser Gruppe können alle Ressourcengruppen und Ressourcen im Abonnement anzeigen.  
- Sie weisen die Rolle „Mitwirkender“ einer Anwendung im Ressourcengruppenbereich zu. Damit können Benutzer Ressourcen aller Typen in dieser Ressourcengruppe verwalten, aber keine anderen Ressourcengruppen des Abonnements.

### Azure RBAC im Vergleich zu klassischen Administratoren für Abonnements
Klassische Administratoren und Co-Admins für Abonnements verfügen über vollständigen Zugriff auf das Azure-Abonnement. Sie können Ressourcen über das [Azure-Portal](https://portal.azure.com) mit Azure Resource Manager-APIs oder über das [klassische Azure-Portal](https://manage.windowsazure.com) und Azure-Dienstverwaltungs-APIs verwalten. Im RBAC-Modell wird klassischen Administratoren die Besitzerrolle auf Abonnementebene zugewiesen.

Nur das Azure-Portal und die neuen Azure Resource Manager-APIs unterstützen Azure RBAC. Benutzer und Anwendungen, denen eine RBAC-Rolle zugewiesen ist, können das klassische Verwaltungsportal und die Azure-Dienstverwaltungs-APIs nicht verwenden.

### Autorisierung für Verwaltungsvorgänge im Vergleich zu Datenvorgängen
Azure RBAC unterstützt nur Verwaltungsvorgänge von Azure-Ressourcen im Azure-Portal und in den Azure Resource Manager-APIs. Über RBAC können nicht alle Vorgänge auf Datenebene für Azure-Ressourcen autorisiert werden. Beispielsweise können Speicherkonten mit RBAC verwaltet werden, aber für die Blobs oder Tabellen in einem Speicherkonto ist dies nicht möglich. Ebenso kann eine SQL-Datenbank verwaltet werden, aber nicht die darin enthaltenen Tabellen.

## Verwalten des Zugriffs über das Azure-Portal
### Zugriff anzeigen
Im Hauptblatt im [Azure-Portal](https://portal.azure.com) können Sie anzeigen, wer Zugriff auf eine Ressource, Ressourcengruppe oder ein Abonnement hat. Wir möchten beispielsweise sehen, wer Zugriff auf eine unserer Ressourcengruppen hat:

1. Wählen Sie in der Navigationsleiste auf der linken Seite das Symbol **Ressourcengruppe** aus.
2. Wählen Sie den Namen der Ressourcengruppe, die Sie untersuchen möchten, im Blatt **Ressourcengruppen** aus.
3. Wählen Sie das Symbol **Benutzer** oben rechts im Blatt mit der Ressourcengruppe aus.
4. Auf dem Blatt **Benutzer** werden alle Benutzer, Gruppen und Anwendungen aufgeführt, denen Zugriff auf die Ressourcengruppe gewährt wurde.

![Blatt „Benutzer“ – geerbter und zugewiesener Zugriff – Screenshot](./media/role-based-access-control-configure/view-access.png)

Beachten Sie, dass einigen Benutzern der Zugriff **zugewiesen** wurde, während andere ihn **geerbt** haben. Der Zugriff wird entweder speziell der Ressourcengruppe zugewiesen oder aus einer Zuweisung des übergeordneten Abonnements geerbt.

> [AZURE.NOTE] Klassische Administratoren und Co-Admins für Abonnements werden im neuen RBAC-Modell als Besitzer des Abonnements betrachtet.


### Zugriff hinzufügen
Sie gewähren Zugriff aus der Ressource, der Ressourcengruppe oder dem Abonnement, die bzw. das als Bereich der Rollenzuweisung gilt.

1. Wählen Sie auf dem Blatt **Benutzer** das Symbol **Hinzufügen**. ![Blatt „Zugriff hinzufügen“ – Rolle auswählen – Screenshot](./media/role-based-access-control-configure/grant-access1.png)
2. Wählen Sie die Rolle aus, die Sie zuweisen möchten.
3. Wählen Sie den Benutzer, die Gruppe oder die Anwendung als das Element in Ihrem Verzeichnis aus, für das Sie Zugriff gewähren möchten. Sie können das Verzeichnis mit Anzeigenamen, E-Mail-Adressen und Objektbezeichnern durchsuchen. ![Blatt „Benutzer hinzufügen“ – Durchsuchen – Screenshot](./media/role-based-access-control-configure/grant-access2.png)

### Zugriff entfernen

1. Wählen Sie auf dem Blatt **Benutzer** die Rollenzuweisung aus, die Sie entfernen möchten.
2. Klicken Sie auf dem Blatt mit den Zuweisungsdetails auf das Symbol **Entfernen**.
3. Klicken Sie auf **Ja**, um die Entfernung zu bestätigen.

![Blatt „Benutzer“ – Aus Rolle entfernen – Screenshot](./media/role-based-access-control-configure/remove-access1.png)

Geerbte Zuweisungen können auf untergeordneter Ebene nicht entfernt werden. Wechseln Sie in den übergeordneten Bereich, um die Rollenzuweisung zu entfernen.

![Blatt „Benutzer“ – Schaltfläche „Entfernen“ bei geerbtem Zugriff deaktiviert – Screenshot](./media/role-based-access-control-configure/remove-access2.png)

## Andere Tools zum Verwalten des Zugriffs
Sie können auch mit Azure RBAC-Befehlen in anderen Tools als dem Azure-Portal Rollen zuweisen und den Zugriff verwalten. Folgen Sie den Links, um weitere Informationen zu den Voraussetzungen und Hilfe bei den ersten Schritten mit Azure RBAC-Befehlen zu erhalten.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

## Nächste Schritte
- [Erstellen eines Verlaufsberichts zu Zugriffsänderungen](role-based-access-control-access-change-history-report.md)
- Weitere Informationen finden Sie unter [Integrierte RBAC-Rollen in Azure](role-based-access-built-in-roles.md).
- Definieren Sie Ihre eigenen [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md).

<!---HONumber=AcomDC_0406_2016-->