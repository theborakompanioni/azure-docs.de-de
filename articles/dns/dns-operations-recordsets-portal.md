<properties 
   pageTitle="Verwalten von DNS-Ressourceneintragssätzen und Einträgen mit dem Azure-Portal | Microsoft Azure" 
   description="Verwalten von DNS-Ressourceneintragssätzen und Einträgen, wenn Ihre Domäne in Azure DNS gehostet wird." 
   services="dns" 
   documentationCenter="na" 
   authors="cherylmc" 
   manager="carmon" 
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="dns"
   ms.devlang="en"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="03/29/2016"
   ms.author="cherylmc"/>

# So erstellen und verwalten Sie DNS-Einträge und Ressourceneintragssätze mit dem Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure-Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


Dieser Leitfaden veranschaulicht, wie Sie für Ihre DNS-Zone Ressourceneintragssätze und Einträge mit dem Azure-Portal erstellen und verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Datensatzgruppen und einzelnen DNS-Einträge zu verstehen. Ein Ressourceneintragssatz ist die Auflistung von Einträgen in einer Zone, die den gleichen Namen tragen und den gleichen Typ aufweisen. Weitere Informationen finden Sie unter [Grundlegende Informationen zu Ressourceneinträgen und Ressourceneintragssätzen](../dns-getstarted-create-recordset#Understanding-record-sets-and-records).

## Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Weitere Informationen zum Erstellen eines Ressourceneintragssatzes im Azure-Portal finden Sie unter [Create DNS records and record sets using the Azure portal](dns-getstarted-create-recordset-portal.md) (Erstellen von DNS-Einträgen und Ressourceneintragssätzen mit dem Azure-Portal).


## Anzeigen eines Ressourceneintragssatzes

1. Navigieren Sie im Azure-Portal zum Blatt Ihrer DNS-Zone.

2. Sie können den Ressourceneintragssatz suchen und ihn aus den aufgelisteten Elementen auswählen. Klicken Sie auf den Ressourceneintragssatz, um ihn auszuwählen. Dies öffnet die Eigenschaften des Ressourceneintragssatzes.

	![Suchen eines Ressourceneintragssatzes](./media/dns-operations-recordsets-portal/searchset500.png)


## Hinzufügen eines Eintrags zu einem vorhandenen Ressourceneintragssatz

Sie können einer Datensatzgruppe bis zu 20 Einträge hinzufügen. Ein Ressourceneintragssatz darf nicht zwei identische Einträge enthalten. Leere Datensatzgruppen (ohne Einträge) können erstellt werden, sie werden jedoch nicht im Azure DNS-Namenserver angezeigt. Ressourceneintragssätze vom Typ CNAME können höchstens einen Eintrag enthalten.


1. Klicken Sie im Blatt **Eigenschaften des Ressourceneintragssatzes** Ihrer DNS-Zone auf den Ressourceneintragssatz, dem Sie einen Eintrag hinzufügen möchten.

	![Auswählen eines Ressourceneintragssatzes](./media/dns-operations-recordsets-portal/selectset500.png)

2. Geben Sie die Einstellungen des Eintrags an, indem Sie die Felder ausfüllen.

	![Hinzufügen eines Eintrags](./media/dns-operations-recordsets-portal/addrecord500.png)

2. Klicken Sie oben auf dem Blatt auf **Speichern**, um die Einstellungen vor dem Schließen des Blattes zu speichern.

3. In der Ecke sehen Sie, dass der Eintrag gespeichert wird.

	![Speichern eines Eintrags](./media/dns-operations-recordsets-portal/saving150.png)

4. Nachdem der Eintrag gespeichert wurde, spiegeln die Werte für den Ressourceneintragssatz auf dem DNS-Blatt den neuen Eintrag wieder.


## Aktualisieren eines Eintrags

Wenn Sie einen Eintrag innerhalb eines vorhandenen Ressourceneintragssatzes aktualisieren, sind die aktualisierbaren Felder vom Eintragstyp abhängig, mit dem Sie arbeiten.

1. Suchen Sie in dem Blatt **Eingenschaften des Ressourceneintragssatzes** Ihres Ressourceneintragssatzes nach dem Eintrag.

2. Ändern Sie den Eintrag. Wenn Sie einen Eintrag ändern, können Sie die verfügbaren Einstellungen für den Eintrag ändern. Im folgenden Beispiel wurde das IP-Adressfeld aktiviert und die Änderung der IP-Adresse wird gerade vorgenommen.

	![Ändern eines Eintrags](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Klicken Sie oben auf dem Blatt auf **Speichern**, um Ihre Einstellungen zu speichern. Die Benachrichtigung, dass der Eintrag gespeichert wurde, wird in der oberen rechten Ecke angezeigt.

	
	![Speichern eines Eintrags](./media/dns-operations-recordsets-portal/saved150.png)



3. Nachdem der Eintrag gespeichert wurde, spiegeln die Werte für den Ressourceneintragssatz auf dem DNS-Blatt den aktualisierten Eintrag wieder.



## Entfernen eines Eintrags aus einem Ressourceneintragssatz

Sie können Einträge mit dem Azure-Portal aus einem Ressourceneintragssatz entfernen. Beachten Sie, dass durch das Entfernen des letzten Eintrags aus einem Ressourceneintragssatz nicht der Ressourceneintragssatz an sich gelöscht wird.

1. Suchen Sie in dem Blatt **Eingenschaften des Ressourceneintragssatzes** Ihres Ressourceneintragssatzes nach dem Eintrag.

2. Klicken Sie auf den Eintrag, den Sie entfernen möchten. Klicken Sie dann auf **Entfernen**.

	![Entfernen eines Eintrags](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Klicken Sie oben auf dem Blatt auf **Speichern**, um Ihre Einstellungen zu speichern.

3. Nachdem der Eintrag gespeichert wurde, zeigen die Werte für den Ressourceneintragssatz auf dem DNS-Blatt an, dass der Eintrag entfernt wurde.


## <a name="delete"></a>Löschen eines Ressourceneintragssatzes

1. Klicken Sie in dem Blatt **Eigenschaften des Ressourceneintragssatzes** Ihres Ressourceneintragssatzes auf **Entfernen**. 

	![Löschen eines Ressourceneintragssatzes](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Ein Dialogfeld erscheint und fragt, ob Sie den Ressourceneintragssatz „Name\_ des \_ Ressorceneintragssatzes“ wirklich löschen möchten.

3. Überprüfen Sie, dass der Name des Ressourceneintragssatzes dem entspricht, den Sie entfernen möchten, und klicken Sie dann auf **Ja**.

4. Im Blatt der DNS-Zone können Sie sicherstellen, dass der Ressourceneintragssatz nicht weiter sichtbar ist.


## Mit NS- und SOA-Einträgen arbeiten

Automatisch erstellte NS- und SOA-Einträge werden anderes verwaltet als andere Ressourceneintragssätze.

### Ändern von SOA-Einträgen

Sie können der automatisch erstellten SOA-Datensatzgruppe an der Zonenspitze (Name = „@“) keine Einträge hinzufügen oder daraus entfernen, Sie können jedoch die Parameter im SOA-Eintrag (außer „Host“) und die Gültigkeitsdauer der Datensatzgruppe ändern.

### Ändern von NS-Einträgen auf oberster Ebene der Zone

Sie können der automatisch erstellten NS-Datensatzgruppe an der Zonenspitze (Name = "@") keine Einträge hinzufügen, daraus entfernen und keine Einträge ändern. Sie können nur die Gültigkeitsdauer (TTL) der Datensatzgruppe ändern.

### Entfernen von SOA- oder NS-Ressourceneintragssätzen

Sie können nicht die SOA- und NS-Datensatzgruppen an der Zonenspitze (Name = "@") löschen, die beim Erstellen der Zone automatisch erstellt werden. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

## Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md). Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Datensatzgruppen mithilfe des .NET SDK](dns-sdk.md).

<!---HONumber=AcomDC_0406_2016-->