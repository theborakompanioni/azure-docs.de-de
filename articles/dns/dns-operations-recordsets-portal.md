<properties
   pageTitle="Verwalten von DNS-Ressourceneintragssätzen und Einträgen mit dem Azure-Portal | Microsoft Azure"
   description="Verwalten von DNS-Ressourceneintragssätzen und Einträgen, wenn Ihre Domäne in Azure DNS gehostet wird."
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
   ms.date="08/16/2016"
   ms.author="cherylmc"/>

# Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure-Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


In diesem Artikel wird gezeigt, wie Sie Ressourceneintragssätze und Einträge für die DNS-Zone im Azure-Portal verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Ressourceneintragssätzen und einzelnen DNS-Ressourceneinträgen zu verstehen. Bei einem Ressourceneintragssatz handelt es sich um die Sammlung von Einträgen in einer Zone, die den gleichen Namen und den gleichen Typ aufweisen. Weitere Informationen finden Sie unter [Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal](dns-getstarted-create-recordset-portal.md).

## Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Weitere Informationen zum Erstellen eines Ressourceneintragssatzes im Azure-Portal finden Sie unter [Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal](dns-getstarted-create-recordset-portal.md).


## Anzeigen eines Ressourceneintragssatzes

1. Wechseln Sie im Azure-Portal zum Blatt **DNS-Zone**.

2. Suchen Sie den Ressourceneintragssatz, und wählen Sie ihn aus. Dies öffnet die Eigenschaften des Ressourceneintragssatzes.

	![Suchen nach einem Ressourceneintragssatz](./media/dns-operations-recordsets-portal/searchset500.png)


## Hinzufügen eines Eintrags zu einem Ressourceneintragssatz

Sie können einem Ressourceneintragssatz bis zu 20 Einträge hinzufügen. Ein Ressourceneintragssatz darf nicht zwei identische Einträge enthalten. Leere Ressourceneintragssätze (ohne Einträge) können erstellt werden, doch werden sie nicht im Azure DNS-Namenserver angezeigt. Ressourceneintragssätze vom Typ CNAME können höchstens einen Eintrag enthalten.


1. Klicken Sie auf dem Blatt **Eigenschaften des Ressourceneintragssatzes** Ihrer DNS-Zone auf den Ressourceneintragssatz, dem Sie einen Eintrag hinzufügen möchten.

	![Auswählen eines Ressourceneintragssatzes](./media/dns-operations-recordsets-portal/selectset500.png)

2. Geben Sie die Einstellungen des Ressourceneintragssatzes an, indem Sie die Felder ausfüllen.

	![Hinzufügen eines Eintrags](./media/dns-operations-recordsets-portal/addrecord500.png)

2. Klicken Sie oben auf dem Blatt auf **Speichern**, um Ihre Einstellungen zu speichern. Schließen Sie dann das Blatt.

3. In der Ecke sehen Sie, dass der Eintrag gespeichert wird.

	![Speichern eines Ressourceneintragssatzes](./media/dns-operations-recordsets-portal/saving150.png)

Nachdem der Eintrag gespeichert wurde, spiegeln die Werte auf dem Blatt **DNS-Zone** den neuen Eintrag wider.


## Aktualisieren eines Eintrags

Wenn Sie einen Eintrag innerhalb eines vorhandenen Ressourceneintragssatzes aktualisieren, hängt vom Eintragstyp ab, welche Felder Sie aktualisieren können.

1. Suchen Sie auf dem Blatt **Eigenschaften des Ressourceneintragssatzes** Ihres Ressourceneintragssatzes nach dem Eintrag.

2. Ändern Sie den Eintrag. Wenn Sie einen Eintrag ändern, können Sie die verfügbaren Einstellungen für den Eintrag ändern. Im folgenden Beispiel ist das Feld **IP-Adresse** ausgewählt, und die IP-Adresse wird gerade geändert.

	![Ändern Sie eines Eintrags](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Klicken Sie oben auf dem Blatt auf **Speichern**, um Ihre Einstellungen zu speichern. In der oberen rechten Ecke wird die Benachrichtigung angezeigt, dass der Eintrag gespeichert wurde.

	![Gespeicherter Ressourceneintragssatz](./media/dns-operations-recordsets-portal/saved150.png)


Nachdem der Eintrag gespeichert wurde, spiegeln die Werte für den Ressourceneintragssatz auf dem Blatt **DNS-Zone** den aktualisierten Eintrag wider.


## Entfernen eines Eintrags aus einem Ressourceneintragssatz

Sie können Einträge mit dem Azure-Portal aus einem Ressourceneintragssatz entfernen. Beachten Sie, dass durch das Entfernen des letzten Eintrags aus einem Ressourceneintragssatz nicht der Ressourceneintragssatz an sich gelöscht wird.

1. Suchen Sie auf dem Blatt **Eigenschaften des Ressourceneintragssatzes** Ihres Ressourceneintragssatzes nach dem Eintrag.

2. Klicken Sie auf den Eintrag, den Sie entfernen möchten. Wählen Sie dann **Entfernen**.

	![Entfernen eines Eintrags](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Klicken Sie oben auf dem Blatt auf **Speichern**, um Ihre Einstellungen zu speichern.

3. Nachdem der Eintrag entfernt wurde, zeigen die Werte für den Eintrag auf dem Blatt **DNS-Zone** an, dass der Eintrag entfernt wurde.


## <a name="delete"></a>Löschen eines Ressourceneintragssatzes

1. Klicken Sie auf dem Blatt **Eigenschaften des Ressourceneintragssatzes** Ihres Ressourceneintragssatzes auf **Entfernen**.

	![Löschen einer Datensatzgruppe](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Sie werden in einer Meldung gefragt, ob Sie den Ressourceneintragssatz wirklich löschen möchten.

3. Überprüfen Sie, ob der Name des Ressourceneintragssatzes dem entspricht, den Sie entfernen möchten, und klicken Sie dann auf **Ja**.

4. Überprüfen Sie auf dem Blatt **DNS-Zone**, ob der Ressourceneintragssatz nicht mehr sichtbar ist.


## Arbeiten mit NS- und SOA-Einträgen

Automatisch erstellte NS- und SOA-Einträge werden anderes verwaltet als andere Ressourceneintragssätze.

### Ändern von SOA-Einträgen

Sie können dem automatisch erstellten SOA-Ressourceneintragssatz an der Zonenspitze (Name = „@“) weder Einträge hinzufügen, noch Einträge daraus entfernen. Allerdings können Sie jeden Parameter im SOA-Eintrag (mit Ausnahme von „Host“) und die Gültigkeitsdauer des Ressourceneintragssatzes ändern.

### Ändern von NS-Einträgen an der Zonenspitze

Sie können dem automatisch erstellten NS-Ressourceneintragssatz an der Zonenspitze (Name = „@“) weder Einträge hinzufügen, noch Einträge daraus entfernen oder darin ändern. Sie können nur die Gültigkeitsdauer (TTL) des Ressourceneintragssatzes ändern.

### Löschen von SOA- oder NS-Ressourceneintragssätzen

Sie können den SOA- und NS-Ressourceneintragssatz an der Zonenspitze (Name = „@“), die beim Erstellen der Zone automatisch erstellt werden, nicht löschen. Sie werden automatisch gelöscht, wenn Sie die Zone löschen.

## Nächste Schritte

-   Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).
-   Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Datensatzgruppen mithilfe des .NET SDK](dns-sdk.md).
-   Weitere Informationen zu Reverse-DNS-Einträgen finden Sie unter [Mit PowerShell Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten](dns-reverse-dns-record-operations-ps.md).

<!---HONumber=AcomDC_0824_2016-->