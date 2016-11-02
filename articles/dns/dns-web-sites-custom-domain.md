<properties 
   pageTitle="Erstellen von benutzerdefinierten DNS-Einträgen für eine Web-App | Microsoft Azure " 
   description="Informationen zum Erstellen von benutzerdefinierten Domänen-DNS-Einträgen für die Web-App mithilfe von Azure DNS." 
   services="dns" 
   documentationCenter="na" 
   authors="cherylmc" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/16/2016"
   ms.author="cherylmc"/>

# Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne

Sie können Azure DNS verwenden, um eine benutzerdefinierte Domäne für Ihre Web-Apps zu hosten. Angenommen, Sie erstellen eine Azure-Web-App und möchten, dass Ihre Benutzer über contoso.com oder www.contoso.com als vollqualifizierten Domänennamen Zugriff darauf haben sollen.

Zu diesem Zweck müssen Sie zwei Einträge erstellen:

- Einen Stammeintrag des Typs „A“, der auf „contoso.com“ zeigt
- Einen Eintrag des Typs „CNAME“ für den „www“-Namen, der auf den A-Eintrag zeigt

Beachten Sie, dass Sie beim Erstellen eines A-Eintrags für eine Web-App in Azure den A-Eintrag manuell aktualisieren müssen, wenn sich die zugrunde liegende IP-Adresse für die Web-App ändert.

## Voraussetzungen

Bevor Sie beginnen, müssen Sie zuerst in Azure DNS eine DNS-Zone erstellen und die Zone in Ihrer Registrierungsstelle an Azure DNS delegieren.
 
1. Befolgen Sie zum Erstellen einer DNS-Zone die Anweisungen unter [Erstellen einer DNS-Zone](dns-getstarted-create-dnszone.md).
2. Befolgen Sie zum Delegieren Ihres DNS an Azure DNS die Anweisungen unter [Delegieren von Domänen an Azure DNS](dns-domain-delegation.md).

Nach dem Erstellen einer Zone und ihrer Delegierung an Azure DNS können Sie Einträge für Ihre benutzerdefinierte Domäne erstellen.

 
## 1\. Hinzufügen eines A-Eintrags für Ihre benutzerdefinierte Domäne

Ein A-Eintrag wird verwendet, um der IP-Adresse einen Namen zuzuordnen. Im folgenden Beispiel wird "@" als A-Eintrag einer IPv4-Adresse zugewiesen:

### Schritt 1
 
Erstellen Sie einen A-Eintrag, und weisen Sie ihn einer Variablen "$rs" zu.
	
	$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600 

### Schritt 2

Fügen Sie den IPv4-Wert dem zuvor erstellten Ressourceneintragssatz „@“ mithilfe der zugewiesenen „$rs“-Variablen hinzu. Der zugewiesene IPv4-Wert ist die IP-Adresse für Ihre Web-App.

Befolgen Sie zum Auffinden der IP-Adresse für eine Web-App die Anleitungen unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

	Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### Schritt 3

Übergeben Sie die Änderungen an den Ressourceneintragssatz. Verwenden Sie `Set-AzureRMDnsRecordSet`, um Änderungen am Ressourceneintragssatz in Azure DNS hochzuladen:

	Set-AzureRMDnsRecordSet -RecordSet $rs

## 2\. Erstellen eines CNAME-Eintrags für Ihre benutzerdefinierten Domäne

Wenn Ihre Domäne bereits von Azure DNS verwaltet wird (siehe [DNS-Domänendelegierung](dns-domain-delegation.md)), können Sie das folgende Beispiel verwenden, um einen CNAME-Eintrag für „contoso.azurewebsites.net“ zu erstellen:

### Schritt 1

Öffnen Sie PowerShell, erstellen Sie einen neuen CNAME-Ressourceneintragssatz, und weisen Sie ihn einer „$rs“-Variablen zu: Hiermit wird ein Ressourceneintragssatz des Typs CNAME mit einer Gültigkeitsdauer von 600 Sekunden in der DNS-Zone „contoso.com“ erstellt.

	$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
 
	Name              : www
	ZoneName          : contoso.com
	ResourceGroupName : myresourcegroup
	Ttl               : 600
	Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
	RecordType        : CNAME
	Records           : {}
	Tags              : {}


### Schritt 2

Sobald die CNAME-Datensatzgruppe erstellt wurde, müssen Sie einen Aliaswert erstellen, der auf die Web-App verweist.

Mit der zuvor zugewiesenen Variable "$rs" können Sie den folgenden PowerShell-Befehl verwenden, um den Alias für die Web-App "contoso.azurewebsites.net" zu erstellen.

	Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
 
	Name              : www
	ZoneName          : contoso.com
	ResourceGroupName : myresourcegroup
	Ttl               : 600
	Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
	RecordType        : CNAME
	Records           : {contoso.azurewebsites.net}
	Tags              : {}

### Schritt 3

Committen Sie die Änderungen mit dem Cmdlet `Set-AzureRMDnsRecordSet`:

	Set-AzureRMDnsRecordSet -RecordSet $rs

Sie können überprüfen, ob der Eintrag korrekt erstellt wurde, indem Sie "www.contoso.com" mit nslookup abfragen, wie unten dargestellt:

	PS C:\> nslookup
	Default Server:  Default
	Address:  192.168.0.1
 
	> www.contoso.com
	Server:  default server
	Address:  192.168.0.1
	 
	Non-authoritative answer:
	Name:    <instance of web app service>.cloudapp.net
	Address:  <ip of web app service>
	Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## Erstellen eines „awverify“-Eintrags für Web-Apps


Wenn Sie einen A-Eintrag für Ihre Web-App verwenden möchten, müssen Sie einen Überprüfungsprozess durchlaufen, um sicherzustellen, dass Sie die benutzerdefinierte Domäne besitzen. Dieser Überprüfungsschritt erfolgt durch Erstellen eines speziellen CNAME-Eintrags mit dem Namen "awverify". Dieser Abschnitt gilt für nur A-Einträge.


### Schritt 1

Erstellen Sie den Eintrag „Awverify“. Im folgenden Beispiel wird der Eintrag "awverify" für "contoso.com" erstellt, um den Besitzer der benutzerdefinierten Domäne zu überprüfen:

	$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600
 
	Name              : awverify
	ZoneName          : contoso.com
	ResourceGroupName : myresourcegroup
	Ttl               : 600
	Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
	RecordType        : CNAME
	Records           : {}
	Tags              : {}


### Schritt 2

Nachdem der Ressourceneintragssatz „Awverify“ erstellt wurde, weisen Sie den Alias für den CNAME-Ressourceneintragssatz zu. Im folgenden Beispiel weisen wir den Alias für den CNAME-Ressourceneintragssatz „awverify.contoso.azurewebsites.net“ zu.

	Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
 
	Name              : awverify
	ZoneName          : contoso.com
	ResourceGroupName : myresourcegroup
	Ttl               : 600
	Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
	RecordType        : CNAME
	Records           : {awverify.contoso.azurewebsites.net}
	Tags              : {}

### Schritt 3

Führen Sie mithilfe des Cmdlets `Set-AzureRMDnsRecordSet cmdlet` ein Commit für die Änderungen aus, wie im folgenden Befehl gezeigt.

	Set-AzureRMDnsRecordSet -RecordSet $rs



## Nächste Schritte

Befolgen Sie die Anweisungen unter [Konfigurieren eines benutzerdefinierten Domänennamens für App Service](../app-service-web/web-sites-custom-domain-name.md), um die Web-App für das Verwenden einer benutzerdefinierten Domäne zu konfigurieren.








 

<!---HONumber=AcomDC_0817_2016-->