<properties
   pageTitle="Erstellen und verwalten einer DNS-Zone im Azure-Portal | Microsoft Azure"
   description="Erfahren Sie mehr über das Erstellen von DNS-Zonen für Azure DNS. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihres ersten DNS und zum Hosten Ihrer DNS-Domäne mit dem Azure-Portal."
   services="dns"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="cherylmc"/>

# Erstellen und Verwalten einer DNS-Zone im Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md)



Dieser Artikel führt Sie Schritt für Schritt durch die Erstellung einer DNS-Zone mithilfe des Azure-Portals. Sie können die DNS-Zone auch mithilfe von PowerShell oder der Befehlszeilenschnittstelle (CLI) erstellen.

Die Domäne „contoso.com“ kann mehrere DNS-Einträge, wie z.B. „mail.contoso.com“ (für einen E-Mail-Server) und „www.contoso.com“ (für eine Website) enthalten. Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Um Ihre Domäne zu hosten, müssen Sie zunächst eine DNS-Zone erstellen. Alle DNS-Einträge, die für eine bestimmte Domäne erstellt wurden, befinden sich in einer DNS-Zone für die Domäne.

### <a name="names"></a>Informationen DNS-Zonennamen
 
- Der Name der Zone muss innerhalb der Ressourcengruppe eindeutig sein. Außerdem darf die Zone noch nicht vorhanden sein. Andernfalls schlägt der Vorgang fehl.

- Der gleiche Zonennamen kann in einer anderen Ressourcengruppe oder einem anderen Azure-Abonnement erneut verwendet werden. Wenn mehrere Zonen denselben Namen haben, werden jeder Instanz verschiedene Namensserveradressen zugewiesen, und nur eine Instanz kann von der übergeordneten Domäne delegiert werden. Weitere Informationen finden Sie unter [Delegieren von Domänen an Azure DNS](#delegate).

### Informationen zu Tags für Azure DNS


Tags sind eine Liste von Name-Wert-Paaren, die von Azure Resource Manager zum Beschriften von Ressourcen zu Abrechnungs- oder Gruppierungszwecken verwendet werden. Weitere Informationen zu Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

Sie können über das Azure-Portal Tags hinzufügen, und zwar im Blatt **Einstellungen** Ihrer DNS-Zone.


## Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.

2. Klicken Sie im Hubmenü auf **Neu > Netzwerk >** und dann auf **DNS-Zone**, um das Blatt der DNS-Zone-zu öffnen.
 
	![DNS-Zone](./media/dns-getstarted-create-dnszone-portal/openzone650.png)

3. Klicken Sie unten auf dem Blatt **DNS-Zone** auf **Erstellen**. Das Blatt **DNS-Zone erstellen** wird geöffnet.

	![Erstellen einer Zone](./media/dns-getstarted-create-dnszone-portal/newzone250.png)

4. Geben Sie Ihrer DNS-Zone im Blatt **DNS-Zone erstellen** einen Namen. Beispiel: *contoso.com*. Weitere Informationen finden Sie im obigen Abschnitt [Informationen zu DNS-Zonennamen](#names).

5. Geben Sie als nächstes die gewünschte Ressourcengruppe an. Sie können entweder eine neue Ressourcengruppe erstellen oder eine bereits vorhandene auswählen.

6. Geben Sie den Speicherort der Ressourcengruppe im Dropdownmenü **Speicherort** an. Beachten Sie, dass sich diese Einstellung auf den Speicherort der Ressourcengruppe, nicht den Speicherort der DNS-Zone bezieht. Die eigentliche DNS-Zonenressource ist automatisch allgemeingültig und kann und muss nicht im Portal angegeben werden.

7. Sie können das Kontrollkästchen **an Dashboard anheften** aktiviert lassen, um Ihre neue Zone auf dem Dashboard leichter wiederzufinden. Klicken Sie dann auf **Erstellen**.

	![An das Dashboard anheften](./media/dns-getstarted-create-dnszone-portal/pindashboard150.png)

8. Nachdem Sie auf „Erstellen“ klicken, sehen Sie, wie Ihre neue Zone auf dem Dashboard konfiguriert wird.

	![Wird erstellt](./media/dns-getstarted-create-dnszone-portal/creating150.png)

9. Wenn Ihre neue Zone erstellt wurde, wird das Blatt Ihrer neuen Zone auf dem Dashboard geöffnet.


## Anzeigen von DNS-Zoneneinträgen

Beim Erstellen einer DNS-Zone werden auch die folgenden Einträge erstellt:

- Der Eintrag „Autoritätsursprung“ (SOA). Der SOA ist im Stamm jeder DNS-Zone vorhanden.
- Die autoritativen Namenserver (NS)-Einträge. Diese zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren von Domänen an Azure DNS](dns-domain-delegation.md).

Sie können die Einträge über das Azure-Portal anzeigen.

1. Klicken Sie im Blatt **DNS-Zone** auf **Alle Einstellungen**, um das Blatt **Einstellungen** der DNS-Zone zu öffnen. 

	![Zone](./media/dns-getstarted-create-dnszone-portal/viewzonens500.png)


2. Im unteren Teil des Bereichs „Essentials“ finden Sie die Ressourceneintragssätze für die DNS-Zone.


	![Zone](./media/dns-getstarted-create-dnszone-portal/viewzone500.png)



## Löschen einer DNS-Zone

Sie können die DNS-Zone direkt aus dem Portal löschen. Vor dem Löschen einer DNS-Zone in Azure DNS müssen Sie alle Datensatzgruppen löschen, mit Ausnahme der NS- und SOA-Einträge im Zonenstamm, die beim Erstellen der Zone automatisch erstellt wurden.

1. Suchen Sie das Blatt **DNS-Zone** der Zone, die Sie löschen möchten, und klicken Sie dann oben auf dem Blatt auf **Löschen**.
 
2. Eine Meldung informiert Sie darüber, dass Sie alle Ressourceneintragssätze löschen müssen, mit Ausnahme der automatisch erstellten NS- und SOA-Einträge. Wenn Sie die Ressourceneintragssätze gelöscht haben, klicken Sie auf **Ja**. Beachten Sie, dass beim Löschen einer DNS-Zone aus dem Portal die Ressourcengruppe, die der DNS-Zone zugeordnet ist, nicht gelöscht wird.


## Nächste Schritte

Lesen Sie nach dem Erstellen der DNS-Zone die Artikel [Erste Schritte beim Erstellen von Ressourceneintragssätzen und Einträgen](dns-getstarted-create-recordset-portal.md) (nur in englischer Sprache verfügbar), [Verwalten von DNS-Zonen](dns-operations-dnszones.md), und [Verwalten von DNS-Einträgen](dns-operations-recordsets-portal.md) (nur in englischer Sprache verfügbar).

<!---HONumber=AcomDC_0406_2016-->