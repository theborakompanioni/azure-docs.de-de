<properties 
	pageTitle="FAQ zu Azure API Management | Microsoft Azure" 
	description="In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und bewährten Methoden für Azure API Management." 
	services="api-management" 
	documentationCenter="" 
	authors="miaojiang" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/09/2016" 
	ms.author="mijiang"/>

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
-	[Wie erziele ich API-Versionsverwaltung mit API Management?](#how-can-i-achieve-api-versioning-with-api-management)
-	[Wie kann ich mehrere Umgebungen für APIs konfigurieren, z.B. für Sandkasten und Produktion?](#how-can-i-configure-multiple-environments-of-apis-for-example-sandbox-and-production)
-	[Wird SOAP in API Management unterstützt?](#is-soap-supported-in-api-management)
-	[Ist die Gateway-IP-Adresse für API Management konstant? Kann ich sie in Firewallregeln verwenden?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-	[Kann ich einen OAuth 2.0-Autorisierungsserver mit AD FS-Sicherheit konfigurieren?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-	[Welche Routingmethode verwendet API Management in einer Bereitstellung in mehreren geografischen Standorten?](#what-routing-method-does-api-management-use-when-deployed-to-multiple-geographic-locations)
-	[Kann ich eine API Management-Dienstinstanz mithilfe einer ARM-Vorlage erstellen?](#can-i-create-an-api-management-service-instance-using-an-arm-template)
-	[Kann ich ein selbstsigniertes SSL-Zertifikat für ein Back-End verwenden?](#can-i-use-a-self-signed-ssl-certificate-for-a-backend)
-	[Warum erhalte ich einen Authentifizierungsfehler, wenn ich versuche, das GIT-Repository zu klonen?](#why-am-i-getting-authentication-failure-when-i-try-to-clone-the-git-repository)
-	[Kann API Management mit ExpressRoute verwendet werden?](#does-api-management-work-with-express-route)


### Wie kann ich dem API Management-Team eine Frage stellen?

-	Sie können Ihre Fragen im [MSDN-Forum zu API Management](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt) stellen.
-	Sie können uns eine E-Mail an folgende Adresse senden: `apimgmt@microsoft.com`.
-	Sie können uns einen [Funktionswunsch](https://feedback.azure.com/forums/248703-api-management) übermitteln.

### Was bedeutet es, wenn ein Feature sich in der Vorschau befindet?

Ein Feature, das sich in der Vorschauphase befindet, ist funktionell abgeschlossen, wir suchen jedoch aktiv nach Feedback zu diesem Feature. Es ist möglich, dass wir als Reaktion auf Kundenfeedback eine wichtige Änderung vornehmen, und raten daher von der Verwendung der Funktion in Produktionsumgebungen ab. Wenn Sie Feedback zu Vorschaufeatures haben, können Sie uns Ihre Kommentare über eine der unter [Wie kann ich dem API Management-Team eine Frage stellen?](#how-can-i-ask-a-question-to-the-api-management-team) beschriebenen Methoden senden.

### Welche Optionen werden unterstützt, um die Verbindung zwischen dem API Management-Gateway und meinen Back-End-Diensten abzusichern?

Es werden verschiedene Optionen unterstützt.

1. Verwenden Sie die HTTP-Standardauthentifizierung. Weitere Informationen finden Sie unter [Konfigurieren der API-Einstellungen](api-management-howto-create-apis.md#configure-api-settings).
2. Verwenden Sie die gegenseitige SSL-Authentifizierung wie unter [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management](api-management-howto-mutual-certificates.md) beschrieben.
3. Verwenden Sie IP-Whitelists für Ihren Back-End-Dienst. Wenn Sie über eine API Management-Instanz des Standard- oder Premium-Tarifs verfügen, bleibt die IP-Adresse des Gateways unverändert, und Sie können Ihre Whitelist für das Zulassen dieser IP-Adresse konfigurieren. Sie können die IP-Adresse Ihrer API Management-Instanz im klassischen Azure-Portal auf dem **Dashboard** abrufen.
4. Sie können Ihre API Management-Instanz mit einer Azure Virtual Network-Instanz (klassisch) verbinden. Weitere Informationen finden Sie unter [Einrichten von VPN-Verbindungen in Azure API Management](api-management-howto-setup-vpn.md).

### Wie kann ich eine API Management-Instanz in eine neue Instanz kopieren?

Sie haben verschiedene Möglichkeiten, um eine API Management-Dienstinstanz in eine neue Instanz zu kopieren.

-	Verwenden Sie die Sicherungs- und Wiederherstellungsfunktion von API Management. Weitere Informationen finden Sie unter [So implementieren Sie die Notfallwiederherstellung mit Sichern und Wiederherstellen von Diensten in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
-	Erstellen Sie mithilfe der [API Management-REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx) ein eigenes Sicherungs- und Wiederherstellungsfeature, um die gewünschten Entitäten aus Ihrer Dienstinstanz zu speichern und wiederherzustellen.
-	Laden Sie die Dienstkonfiguration unter Verwendung von Git herunter, und laden Sie sie auf eine neue Instanz hoch. Weitere Informationen finden Sie unter [Speichern und Konfigurieren der API Management-Dienstkonfiguration mit Git](api-management-configuration-repository-git.md).

### Kann ich meine API Management-Instanz programmgesteuert verwalten?

Ja. Sie können die Instanz mithilfe der [API Management-REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx), des [Microsoft Azure API Management Service Management Library SDK](http://aka.ms/apimsdk) sowie über die PowerShell-Cmdlets für [Dienstbereitstellung](https://msdn.microsoft.com/library/mt619282.aspx) und [Dienstverwaltung](https://msdn.microsoft.com/library/mt613507.aspx) verwalten.

### Wie kann ich Benutzer zur Administratorengruppe hinzufügen?

Sie können dies mithilfe der folgenden Schritte erreichen:

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zu der Ressourcengruppe, die die gewünschte API Management-Instanz enthält.
3. Fügen Sie den gewünschten Benutzer zur Rolle „Mitwirkender für API Management“ hinzu.

Nach diesem Schritt kann sich der neu hinzugefügte Administrator mithilfe von Azure PowerShell-[Cmdlets](https://msdn.microsoft.com/library/mt613507.aspx) als Administrator anmelden:

1. Melden Sie sich mithilfe des `Login-AzureRmAccount`-Cmdlets an.
2. Legen Sie den Kontext auf das Abonnement fest, das den Dienst enthält. Verwenden Sie hierfür `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Rufen Sie das SSO-Token ab. Verwenden Sie hierfür `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Kopieren Sie die URL, und fügen Sie sie in den Browser ein. Der Benutzer sollte nun Zugriff auf das Verwaltungsportal haben.


### Weshalb ist die Richtlinie, die ich hinzufügen möchte, im Richtlinien-Editor nicht aktiviert?

Wenn die Richtlinie, die Sie hinzufügen möchten, nicht aktiviert ist, stellen Sie sicher, dass Sie sich im richtigen Bereich für diese Richtlinie befinden. Für jede Richtlinienanweisung sind bestimmte Bereiche und Richtlinienabschnitte vorgesehen. Informationen zu den Richtlinienabschnitten und Bereichen für eine Richtlinie finden Sie in der [Richtlinienreferenz](https://msdn.microsoft.com/library/azure/dn894080.aspx) im Abschnitt **Verwendung** für die jeweilige Richtlinie.


### Wie erziele ich API-Versionsverwaltung mit API Management?

-	Sie können in API Management separate APIs für die verschiedenen Versionen konfigurieren. Sie können beispielsweise `MyAPI v1` und `MyAPI v2` als unterschiedliche APIs bereitstellen, und Entwickler können auswählen, welche Version sie verwenden möchten.
-	Sie können Ihre API auch mit einer Dienst-URL konfigurieren, die kein Versionssegment enthält. Beispiel: `https://my.api`. Anschließend können Sie ein Versionssegment für die [URL-Umschreibevorlage](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) für jeden Vorgang konfigurieren. So können Sie beispielsweise einen Vorgang mit der [URL-Vorlage](api-management-howto-add-operations.md#url-template) `/resource` und der [URL-Umschreibevorlage](api-management-howto-add-operations.md#rewrite-url-template) `/v1/Resource` einrichten. Auf diese Weise können Sie den Wert des Versionssegments für jeden Vorgang separat ändern.
-	Wenn Sie ein standardmäßiges Versionssegment in der Dienst-URL der API beibehalten möchten, können Sie für ausgewählte Vorgänge eine Richtlinie festlegen, die die Richtlinie [set-backend-service](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) nutzt, um den Anforderungspfad für das Back-End zu ändern.

### Wie kann ich mehrere Umgebungen für APIs konfigurieren, z.B. für Sandkasten und Produktion?

Zurzeit stehen folgenden Optionen zur Verfügung:

-	Sie können verschiedene APIs im gleichen Mandanten hosten.
-	Sie können die gleichen APIs in verschiedenen Mandanten hosten.

### Wird SOAP in API Management unterstützt?

Zurzeit bieten wir eingeschränkte Unterstützung für SOAP in Azure API Management an; wir untersuchen dieses Feature weiter. Wir sind sehr daran interessiert, Beispiel-WSDLs von Ihren Kunden sowie eine Beschreibung der Features zu erhalten, die sie benötigen. So können wir das Feature fokussiert weiterentwickeln. Verwenden Sie die Kontaktinformationen unter [Wie kann ich dem API Management-Team eine Frage stellen?](#how-can-i-ask-a-question-to-the-api-management-team), um sich mit uns in Verbindung zu setzen.

Falls Sie das Feature unbedingt benötigen, finden Sie unter [Azure API Management - APIM, consuming a SOAP WCF service over HTTP](http://mostlydotnetdev.blogspot.com/2015/03/azure-api-management-apim-consuming.html) (Azure API Management – APIM, Verwendung eines SOAP-WCF-Diensts über HTTP) einige von der Community vorgeschlagene Problemumgehungen.

Die Implementierung der Lösung auf diese Weise erfordert die manuelle Konfiguration von Richtlinien und unterstützt keine WSDL-Import-/Exportvorgänge. Zudem müssen Benutzer den Text von Anforderungen formen, die mithilfe der Testkonsole im Entwicklerportal erstellt wurden.

### Ist die Gateway-IP-Adresse für API Management konstant? Kann ich sie in Firewallregeln verwenden?

In den Tarifen „Standard“ und „Premium“ ist die öffentliche IP-Adresse (VIP) des API Management-Mandanten während der gesamten Lebensdauer des Mandanten statisch, bis auf einige hier aufgeführte Ausnahmen. Beachten Sie, dass Mandanten mit Premium-Tarif, die für die Bereitstellung in mehreren Regionen konfiguriert sind, eine öffentliche IP-Adresse pro Region zugewiesen wird.

Die IP-Adresse ändert sich in folgenden Situationen:

-	Der Dienst wird gelöscht und neu erstellt.
-	Das Dienstabonnement wird gesperrt (z.B. aufgrund von nicht geleisteten Zahlungen) und reaktiviert.
-	Ein VNET wird hinzugefügt oder entfernt (VNETs werden nur im Premium-Tarif unterstützt).
-	Änderungen von regionalen Adressen, wenn eine Region verlassen und danach erneut eingetragen wird (Bereitstellung in mehreren Regionen wird nur im Premium-Tarif unterstützt).

Sie finden die IP-Adresse (bzw. die Adressen bei Bereitstellungen in mehreren Regionen) auf der Mandantenseite im klassischen Azure-Portal.

### Kann ich einen OAuth 2.0-Autorisierungsserver mit AD FS-Sicherheit konfigurieren?

Informationen zum Konfigurieren dieses Szenarios finden Sie unter [Using AD FS in API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/) (Verwenden von AD FS in API Management).

### Welche Routingmethode verwendet API Management in einer Bereitstellung in mehreren geografischen Standorten? 

API Management verwendet die [Leistungsorientierte Methode für das Datenverkehrsrouting](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method). Eingehender Datenverkehr wird zum nächstgelegenen API-Gateway weitergeleitet. Wenn eine Region offline geht, wird der eingehende Datenverkehr automatisch an das nächstgelegene Gateway weitergeleitet. Weitere Informationen zu Routingmethoden für Datenverkehr finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](../traffic-manager/traffic-manager-routing-methods.md).

### Kann ich eine API Management-Dienstinstanz mithilfe einer ARM-Vorlage erstellen?

Ja. Weitere Informationen finden Sie in den Schnellstartvorlagen für den [Azure API Management-Dienst](http://aka.ms/apimtemplate).

### Kann ich ein selbstsigniertes SSL-Zertifikat für ein Back-End verwenden?

Ja. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine [Back-End](https://msdn.microsoft.com/library/azure/dn935030.aspx)-Entität mit der Verwaltungs-API.
2. Legen Sie die skipCertificateChainValidation-Eigenschaft auf „true“ fest.
3. Wenn Sie die Verwendung eines selbstsignierten Zertifikats nicht mehr zulassen möchten, können Sie die Back-End-Entität löschen oder die skipCertificateChainValidation-Eigenschaft auf „false“ festlegen.

### Warum erhalte ich einen Authentifizierungsfehler, wenn ich versuche, das GIT-Repository zu klonen? 

Wenn Sie die GIT-Anmeldeinformationsverwaltung verwenden oder versuchen, das Repository über Visual Studio zu klonen, tritt möglicherweise ein bekanntes Problem mit dem Windows-Dialogfeld für die Anmeldeinformationen auf: Da die Kennwortlänge auf 127 Zeichen begrenzt ist, wird das von uns generierte Kennwort abgeschnitten. Wir arbeiten daran, das Kennwort zu verkürzen. Verwenden Sie vorerst GIT Bash für Klonvorgänge.

### Kann API Management mit ExpressRoute verwendet werden?

Ja.

<!---HONumber=AcomDC_0831_2016-->