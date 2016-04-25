<properties 
	pageTitle="FAQ zu Azure API Management | Microsoft Azure" 
	description="In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und bewährten Methoden für Azure API Management." 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/12/2016" 
	ms.author="sdanie"/>

# FAQ zu Azure API Management

In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und bewährten Methoden für Azure API Management.

## Häufig gestellte Fragen

-	[Wie kann ich dem API Management-Team eine Frage stellen?](#how-can-i-ask-a-question-to-the-api-management-team)
-	[Was bedeutet es, wenn ein Feature sich in der Vorschau befindet?](#what-does-it-mean-if-a-feature-is-in-preview)
-	[Welche Optionen werden unterstützt, um die Verbindung zwischen dem API Management-Gateway und meinen Back-End-Diensten abzusichern?](#what-are-the-supported-options-to-secure-the-connection-between-the-api-management-gateway-and-my-backend-services)
-	[Wie kann ich eine API Management-Instanz in eine neue Instanz kopieren?](#how-can-i-copy-an-api-management-instance-to-a-new-instance)
-	[Kann ich meine API Management-Instanz programmgesteuert verwalten?](#can-i-manage-my-api-management-instance-programmatically)
-	[Wie kann ich Benutzer zur Administratorengruppe hinzufügen?](#how-can-i-add-a-user-to-the-administrators-group)
-	[Weshalb ist die Richtlinie, die ich hinzufügen möchte, im Richtlinien-Editor nicht aktiviert?](#why-is-the-policy-that-i-want-to-add-not-enabled-in-the-policy-editor)

### Wie kann ich dem API Management-Team eine Frage stellen?

-	Sie können Ihre Fragen im [MSDN-Forum zu API Management](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt) stellen.
-	Alternativ können Sie uns eine E-Mail an folgende Adresse senden: `apimgmt@microsoft.com`.

### Was bedeutet es, wenn ein Feature sich in der Vorschau befindet?

Ein Feature, das sich in der Vorschauphase befindet, ist funktionell abgeschlossen, wir suchen jedoch aktiv nach Feedback zu diesem Feature. Es ist möglich, dass wir als Reaktion auf Kundenfeedback eine wichtige Änderung vornehmen, und raten daher von der Verwendung der Funktion in Produktionsumgebungen ab. Wenn Sie Feedback zu Vorschaufeatures haben, senden Sie uns Ihre Kommentare über eine der unter [Wie kann ich dem API Management-Team eine Frage stellen?](#how-can-i-ask-a-question-to-the-api-management-team) beschriebenen Methoden.

### Welche Optionen werden unterstützt, um die Verbindung zwischen dem API Management-Gateway und meinen Back-End-Diensten abzusichern?

Es werden verschiedene Optionen unterstützt.

1. Verwenden Sie die HTTP-Standardauthentifizierung. Weitere Informationen finden Sie unter [Konfigurieren der API-Einstellungen](api-management-howto-create-apis.md#configure-api-settings).
2. Verwenden Sie die gegenseitige SSL-Authentifizierung, die unter [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management](api-management-howto-mutual-certificates.md) beschrieben ist.
3. Verwenden Sie IP-Whitelists für Ihren Back-End-Dienst. Wenn Sie über eine API Management-Instanz des Standard- oder Premium-Tarifs verfügen, bleibt die IP-Adresse des Gateways unverändert, und Sie können Ihre Whitelist für das Zulassen dieser IP-Adresse konfigurieren. Sie können die IP-Adresse Ihrer API Management-Instanz auf dem **Dashboard** im klassischen Azure-Portal abrufen.
4. Sie können Ihre API Management-Instanz mit einer Azure Virtual Network-Instanz (klassisch) verbinden. Weitere Informationen finden Sie unter [Einrichten von VPN-Verbindungen in Azure API Management](api-management-howto-setup-vpn.md).

### Wie kann ich eine API Management-Instanz in eine neue Instanz kopieren?

Sie haben verschiedene Möglichkeiten, um eine API Management-Dienstinstanz in eine neue Instanz zu kopieren.

-	Verwenden Sie die Sicherungs- und Wiederherstellungsfunktion von API Management. Weitere Informationen finden Sie unter [So implementieren Sie die Notfallwiederherstellung mit Sichern und Wiederherstellen von Diensten in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
-	Erstellen Sie mithilfe der [API Management-REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx) ein eigenes Sicherungs- und Wiederherstellungsfeature, um die gewünschten Entitäten aus Ihrer Dienstinstanz zu speichern und wiederherzustellen.
-	Laden Sie die Dienstkonfiguration unter Verwendung von Git herunter, und laden Sie sie auf eine neue Instanz hoch. Weitere Informationen finden Sie unter [Speichern und Konfigurieren der API Management-Dienstkonfiguration mit Git](api-management-configuration-repository-git.md).

### Kann ich meine API Management-Instanz programmgesteuert verwalten?

Ja, Sie können die Instanz mithilfe der [API Management-REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx) sowie über die PowerShell-Cmdlets für [Dienstbereitstellung](https://msdn.microsoft.com/library/mt619282.aspx) und [Dienstverwaltung](https://msdn.microsoft.com/library/mt613507.aspx) verwalten.

### Wie kann ich Benutzer zur Administratorengruppe hinzufügen?

Aktuell sind Administratoren auf Benutzer beschränkt, die sich über das klassische Azure-Portal und im Rahmen von Azure-Abonnements mit der API Management-Instanz als Administratoren oder Co-Administratoren anmelden. Benutzer, die im Herausgeberportal erstellt wurden, können nicht als Administratoren benannt oder zur Administratorengruppe hinzugefügt werden.


### Weshalb ist die Richtlinie, die ich hinzufügen möchte, im Richtlinien-Editor nicht aktiviert?

Wenn die Richtlinie, die Sie hinzufügen möchten, nicht aktiviert ist, stellen Sie sicher, dass Sie sich im richtigen Bereich für diese Richtlinie befinden. Für jede Richtlinie sind bestimmte Bereiche und Richtlinienabschnitte vorgesehen. Informationen zu den Richtlinienabschnitten und Bereichen für eine Richtlinie finden Sie in der [Richtlinienreferenz](https://msdn.microsoft.com/library/azure/dn894080.aspx) im Abschnitt **Verwendung** für die jeweilige Richtlinie.

<!---HONumber=AcomDC_0413_2016-->