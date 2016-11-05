---
title: Erstellen und verwalten einer DNS-Zone im Azure-Portal | Microsoft Docs
description: Erfahren Sie mehr über das Erstellen von DNS-Zonen für Azure DNS. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihres ersten DNS und zum Hosten Ihrer DNS-Domäne mit dem Azure-Portal.
services: dns
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: cherylmc

---
# Erstellen einer DNS-Zone im Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md)
> 
> 

Dieser Artikel führt Sie Schritt für Schritt durch die Erstellung einer DNS-Zone mithilfe des Azure-Portals. Sie können die DNS-Zone auch mithilfe von PowerShell oder der Befehlszeilenschnittstelle (CLI) erstellen.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

### Informationen zu Tags für Azure DNS
Tags sind eine Liste von Name-Wert-Paaren, die von Azure Resource Manager zum Beschriften von Ressourcen zu Abrechnungs- oder Gruppierungszwecken verwendet werden. Weitere Informationen zu Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

Sie können im Azure-Portal Tags über das Blatt **Einstellungen** Ihrer DNS-Zone hinzufügen.

## Erstellen einer DNS-Zone
1. Melden Sie sich auf dem Azure-Portal an.
2. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und dann auf **DNS-Zone**, um das Blatt der DNS-Zone zu öffnen.
   
    ![DNS-Zone](./media/dns-getstarted-create-dnszone-portal/openzone650.png)
3. Klicken Sie unten auf dem Blatt **DNS-Zone** auf **Erstellen**. Das Blatt **DNS-Zone erstellen** wird geöffnet.
   
    ![Erstellen einer Zone](./media/dns-getstarted-create-dnszone-portal/newzone250.png)
4. Geben Sie Ihrer DNS-Zone auf dem Blatt **DNS-Zone erstellen** einen Namen. Beispiel: *contoso.com*. Weitere Informationen finden Sie im obigen Abschnitt [Informationen zu DNS-Zonennamen](#names).
5. Geben Sie als nächstes die gewünschte Ressourcengruppe an. Sie können entweder eine neue Ressourcengruppe erstellen oder eine bereits vorhandene auswählen.
6. Geben Sie im Dropdownmenü **Speicherort** den Speicherort der Ressourcengruppe an. Beachten Sie, dass sich diese Einstellung auf den Speicherort der Ressourcengruppe, nicht den Speicherort der DNS-Zone bezieht. Die eigentliche DNS-Zonenressource ist automatisch allgemeingültig und kann und muss nicht im Portal angegeben werden.
7. Sie können das Kontrollkästchen **An Dashboard anheften** aktiviert lassen, um Ihre neue Zone auf dem Dashboard leichter wiederzufinden. Klicken Sie dann auf **Erstellen**.
   
    ![An das Dashboard anheften](./media/dns-getstarted-create-dnszone-portal/pindashboard150.png)
8. Nachdem Sie auf „Erstellen“ klicken, sehen Sie, wie Ihre neue Zone auf dem Dashboard konfiguriert wird.
   
    ![Wird erstellt](./media/dns-getstarted-create-dnszone-portal/creating150.png)
9. Wenn Ihre neue Zone erstellt wurde, wird das Blatt Ihrer neuen Zone auf dem Dashboard geöffnet.

## Anzeigen von Datensätzen
Beim Erstellen einer DNS-Zone werden auch die folgenden Einträge erstellt:

* Der Eintrag „Autoritätsursprung“ (SOA). Der SOA ist im Stamm jeder DNS-Zone vorhanden.
* Die autoritativen Namenserver (NS)-Einträge. Diese zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

Sie können die Einträge über das Azure-Portal anzeigen.

1. Klicken Sie auf dem Blatt **DNS-Zone** auf **Alle Einstellungen**, um das Blatt **Einstellungen** der DNS-Zone zu öffnen.
   
    ![Zone](./media/dns-getstarted-create-dnszone-portal/viewzonens500.png)
2. Im unteren Teil des Bereichs „Essentials“ finden Sie die Ressourceneintragssätze für die DNS-Zone.

    ![Zone](./media/dns-getstarted-create-dnszone-portal/viewzone500.png)

## Test
Sie können Ihre DNS-Zone mit DNS-Tools wie nslookup, dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) testen.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für die Zone sind in den NS-Einträgen enthalten, die von `Get-AzureRmDnsRecordSet` (oben) aufgelistet werden. Ersetzen Sie im folgenden Befehl die Werte durch die für Ihre Zone ordnungsgemäßen Werte.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)



## Löschen einer DNS-Zone
Sie können die DNS-Zone direkt aus dem Portal löschen. Vor dem Löschen einer DNS-Zone in Azure DNS müssen Sie alle Datensatzgruppen löschen, mit Ausnahme der NS- und SOA-Einträge im Zonenstamm, die beim Erstellen der Zone automatisch erstellt wurden.

1. Suchen Sie das Blatt **DNS-Zone** der Zone, die Sie löschen möchten, und klicken Sie dann oben auf dem Blatt auf **Löschen**.
2. Eine Meldung informiert Sie darüber, dass Sie alle Ressourceneintragssätze löschen müssen, mit Ausnahme der automatisch erstellten NS- und SOA-Einträge. Wenn Sie die Ressourceneintragssätze gelöscht haben, klicken Sie auf **Ja**. Beachten Sie, dass beim Löschen einer DNS-Zone aus dem Portal die Ressourcengruppe, die der DNS-Zone zugeordnet ist, nicht gelöscht wird.

## Nächste Schritte
Nach dem Erstellen einer DNS-Zone müssen [Ressourceneintragssätze und Einträge](dns-getstarted-create-recordset-portal.md) zum Auflösen von Namen für Ihre Internetdomäne erstellt werden.

<!---HONumber=AcomDC_0817_2016-->