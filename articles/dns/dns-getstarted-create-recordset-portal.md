<properties
   pageTitle="Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mit dem Azure-Portal | Microsoft Azure"
   description="Erstellen von Hosteinträgen für Azure DNS und Erstellen von Ressourceneintragssätzen und Einträgen mit dem Azure-Portal"
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
   ms.date="05/06/2016"
   ms.author="cherylmc"/>



# Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)


Dieser Artikel beschreibt das Erstellen von Einträgen und Ressourceneintragssätzen mithilfe des Azure-Portals. Nach dem Erstellen der DNS-Zone müssen Sie die DNS-Einträge für Ihre Domäne hinzufügen. Zu diesem Zweck benötigen Sie zunächst grundlegende Informationen zu DNS-Datensätzen und Datensatzgruppen.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]


## Erstellen einer Datensatzgruppe und eines Datensatzes

Im folgende Beispiel wird die Erstellung eines Ressourceneintragssatzes und von Einträgen mit dem Azure-Portal schrittweise erläutert. Wir verwenden den DNS-Eintragstyp „A“.

1. Melden Sie sich beim Azure-Portal an.

2. Navigieren Sie zum Blatt der **DNS-Zone**, in der Sie einen Ressourceneintragssatz erstellen möchten.

3. Klicken Sie oben auf dem Blatt „DNS-Zone“ auf **Ressourceneintragssatz**, um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.
 
	![Neuer Ressourceneintragssatz](./media/dns-getstarted-create-recordset-portal/newrecordset500.png)

4. Benennen Sie auf dem Blatt **Ressourceneintragssatz hinzufügen** Ihren Ressourceneintragssatz. Sie können dem Ressourceneintragssatz z.B. den Namen **www** geben.
  
	![Hinzufügen eines Ressourceneintragssatzes](./media/dns-getstarted-create-recordset-portal/addrecordset500.png)

5. Wählen Sie bei „Typ“ den gewünschten Eintragstyp. Zum Beispiel **A**.

6. Legen Sie die **Gültigkeitsdauer** fest. Die Standardeinstellung für das Portal ist eine Stunde.

7. Fügen Sie die IP-Adressen hinzu, eine IP-Adresse pro Zeile. Durch Verwenden des vorgeschlagenen Namens für den Ressourceneintragssatz und den oben angegebenen Eintragstyp wird die IPv4-IP-Adresse dem A-Eintrag für den www-Ressourceneintragssatz hinzugefügt.

8. Wenn die IP-Adresse hinzugefügt wurde, klicken Sie unten auf dem Blatt auf **OK**. Der DNS-Ressourceneintragssatz wird erstellt.


## Nächste Schritte

Informationen zur Verwaltung Ihrer Ressourceneintragssätze und Einträge finden Sie unter [So erstellen und verwalten Sie DNS-Einträge und Ressourceneintragssätze mit dem Azure-Portal](dns-operations-recordsets-portal.md).

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

<!----HONumber=AcomDC_0518_2016-->