---
title: "Erweitern von lokalen Always On-Verfügbarkeitsgruppen auf Azure | Microsoft Docs"
description: "In diesem Tutorial werden mit dem klassischen Bereitstellungsmodell erstellte Ressourcen verwendet. Es beschreibt die Verwendung des Assistenten zum Hinzufügen von Replikaten in SQL Server Management Studio (SSMS) zum Hinzufügen eines Replikats einer AlwaysOn-Verfügbarkeitsgruppe in Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2016
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 38728a20527be060e63d2dbdea923263b3d6c7fa
ms.lasthandoff: 03/25/2017


---
# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Erweitern von lokalen AlwaysOn-Verfügbarkeitsgruppen auf Azure
AlwaysOn-Verfügbarkeitsgruppen bieten hohe Verfügbarkeit für Datenbankgruppen durch Hinzufügen sekundärer Replikate. Diese Replikate ermöglichen den Failover von Datenbanken bei Ausfällen. Darüber hinaus können Sie für das Auslagern von Lesearbeitslasten oder Sicherungsaufgaben verwendet werden.

Sie können lokale Verfügbarkeitsgruppen nach Microsoft Azure erweitern, indem Sie eine oder mehrere Azure-VMs mit SQL Server bereitstellen und diese dann Ihren lokalen Verfügbarkeitsgruppen als Replikate hinzufügen.

Dieses Lernprogramm setzt voraus, dass Sie über die folgenden Punkte verfügen:

* Ein aktives Azure-Abonnement. Sie können sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
* Eine vorhandene, lokale AlwaysOn-Verfügbarkeitsgruppe. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen](https://msdn.microsoft.com/library/hh510230.aspx).
* Konnektivität zwischen dem lokalen Netzwerk und Ihrem virtuellen Azure-Netzwerk. Weitere Informationen zum Erstellen dieses virtuellen Netzwerks finden Sie unter [Konfigurieren eines Standort-zu-Standort-VPN im klassischen Azure-Portal](../../../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

## <a name="add-azure-replica-wizard"></a>Assistent zum Hinzufügen von Azure-Replikaten
In diesem Abschnitt erfahren Sie, wie Sie mit dem **Assistenten zum Hinzufügen von Azure-Replikaten** Ihre AlwaysOn-Verfügbarkeitsgruppenlösung um Azure-Replikate erweitern.

1. Erweitern Sie in SQL Server Management Studio die Option **Hohe Verfügbarkeit mit Always On** > **Verfügbarkeitsgruppen** > **[Name Ihrer Verfügbarkeitsgruppe]**.
2. Klicken Sie mit der rechten Maustaste auf **Verfügbarkeitsreplikate**, und klicken Sie dann auf **Replikat hinzufügen**.
3. Standardmäßig wird der **Assistent zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen** angezeigt. Klicken Sie auf **Weiter**.  Wenn Sie bei einer früheren Ausführung des Assistenten unten auf der Seite die Option **Diese Seite nicht mehr anzeigen** aktiviert haben, wird dieser Bildschirm nicht angezeigt.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. In diesem Fall müssen Sie Verbindungen mit allen vorhandenen sekundären Replikaten herstellen. Klicken Sie auf **Verbinden…** neben jedem Replikat, oder klicken Sie auf **Alle verbinden…** unten auf dem Bildschirm. Klicken Sie nach der Authentifizierung auf **Weiter** , um zum nächsten Bildschirm zu gelangen.
5. Auf der Seite **Replikate angeben** befinden sich mehrere Registerkarten: **Replikate**, **Endpunkte**, **Sicherungseinstellungen** und **Listener**. Klicken Sie auf der Registerkarte **Replikate** auf **Azure-Replikat hinzufügen…** , um den Assistenten zum Hinzufügen von Azure-Replikaten zu starten.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Wählen Sie ein vorhandenes Azure-Verwaltungszertifikat aus dem lokalen Windows-Zertifikatspeicher aus, wenn Sie zuvor eins installiert hatten. Wählen Sie die ID eines Azure-Abonnements aus, wenn Sie zuvor eins verwendet hatten, oder geben Sie sie ein. Sie können auf „Download“ klicken, um ein Azure-Verwaltungszertifikat herunterzuladen und zu installieren und mithilfe eines Azure-Kontos die Abonnementliste herunterzuladen.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Sie müssen jedes Feld auf der Seite mit Werten auffüllen, die zum Erstellen es virtuellen Azure-Computers (VM) verwendet werden, der für das Hosten des Replikats bestimmt ist.
   
   | Einstellung | Beschreibung |
   | --- | --- |
   | **Image** |Wählen Sie die gewünschte Kombination aus Betriebssystem und SQL Server aus |
   | **Größe des virtuellen Computers** |Wählen Sie die Größe für die VM, die Ihren Geschäftsanforderungen optimal entspricht |
   | **VM-Name** |Geben Sie einen eindeutigen Namen für den neuen virtuellen Computer an. Der Name muss zwischen 3 und 15 Zeichen enthalten, er darf nur aus Buchstaben, Zahlen und Bindestrichen bestehen und muss mit einem Buchstaben beginnen und mit einem Buchstaben oder einer Zahl enden. |
   | **VM-Benutzername** |Geben Sie einen Benutzernamen für das Administratorkonto auf dem virtuellen Computer ein |
   | **VM-Administratorkennwort** |Geben Sie ein Kennwort für das neue Konto an |
   | **Kennwort bestätigen** |Bestätigen Sie das Kennwort für das neue Konto |
   | **Virtuelles Netzwerk** |Geben Sie das virtuelle Azure-Netzwerk an, das die neue VM verwenden soll. Weitere Informationen über virtuelle Netzwerke erhalten Sie unter [Übersicht über virtuelle Netzwerke](../../../virtual-network/virtual-networks-overview.md). |
   | **Subnetz des virtuellen Netzwerks** |Geben Sie das Subnetz des virtuellen Netzwerks an, das die neue VM verwenden soll |
   | **Domäne** |Bestätigen Sie, dass der voreingestellte Wert für die Domäne zutreffend ist |
   | **Domänenbenutzername** |Geben Sie ein Konto an, das sich in der lokalen Administratorgruppe auf dem lokalen Clusterknoten befindet |
   | **Kennwort** |Geben Sie das Kennwort für den Domänenbenutzernamen an |
8. Klicken Sie auf **OK** , um die Bereitstellungseinstellungen zu bestätigen.
9. Als Nächstes werden die rechtlichen Bestimmungen angezeigt. Lesen Sie die Bedingungen durch, und klicken Sie auf **OK** , wenn Sie ihnen zustimmen.
10. Die Seite **Replikate angeben** wird erneut angezeigt. Überprüfen Sie die Einstellungen für das neue Azure-Replikat auf den Registerkarten **Replikate**, **Endpunkte** und **Sicherungseinstellungen**. Ändern Sie die Einstellungen entsprechend Ihren Geschäftsanforderungen.  Weitere Informationen zu den auf diesen Registerkarten aufgeführten Parametern finden Sie unter [Seite „Replikate angeben“ (Assistent für neue Verfügbarkeitsgruppen/Assistent zum Hinzufügen von Replikaten)](https://msdn.microsoft.com/library/hh213088.aspx). Beachten Sie, dass für Verfügbarkeitsgruppen, die Azure-Replikate enthalten, auf der Registerkarte „Listener“ keine Listener erstellt werden können. Ferner wird, wenn vor dem Ausführen des Assistenten bereits ein Listener erstellt wurde, eine Meldung angezeigt, die Sie darauf hinweist, dass der Listener von Azure nicht unterstützt wird. Das Erstellen von Listenern wird im Abschnitt **Erstellen eines Listeners für eine Verfügbarkeitsgruppe** behandelt.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. Klicken Sie auf **Weiter**.
12. Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** die gewünschte Methode der Datensynchronisierung aus, und klicken Sie auf **Weiter**. Für die meisten Szenarien sollten Sie **Vollständige Datensynchronisation**auswählen. Weitere Informationen zu Methoden der Datensynchronisierung finden Sie unter [Seite „Anfängliche Datensynchronisierung auswählen“ (Assistenten für AlwaysOn-Verfügbarkeitsgruppen)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Überprüfen Sie die Ergebnisse auf der Seite **Überprüfung**. Beheben Sie noch bestehende Probleme, und führen Sie die Überprüfung ggf. erneut aus. Klicken Sie auf **Weiter**.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Überprüfen Sie die Einstellungen auf der Seite **Zusammenfassung**, und klicken Sie dann auf **Fertig stellen**.
15. Der Bereitstellungsvorgang beginnt. Wenn der Assistent erfolgreich abgeschlossen wird, klicken Sie auf **Schließen** , um den Assistenten zu verlassen.

> [!NOTE]
> Der Assistent zum Hinzufügen von Azure-Replikaten erstellt eine Protokolldatei unter „Benutzer\Benutzername\AppData\Local\SQL Server\AddReplicaWizard“. Diese Protokolldatei kann zum Beheben von Fehlern in fehlerhaften Azure-Replikatbereitstellungen verwendet werden. Wenn der Assistent keinerlei Aktionen ausführt, wird ein Rollback aller zuvor vorgenommenen Vorgänge ausgeführt, einschließlich der Löschung der bereitgestellten VM.
> 
> 

## <a name="create-an-availability-group-listener"></a>Erstellen eines Listeners für eine Verfügbarkeitsgruppe
Nach dem Erstellen der Verfügbarkeitsgruppe sollten Sie einen Listener für Clientverbindungen mit den Replikaten erstellen. Listener leiten eingehende Verbindungen entweder an das primäre oder an ein schreibgeschütztes sekundäres Replikat weiter. Weitere Informationen zu Listenern finden Sie unter [Konfigurieren eines ILB-Listeners für Always On-Verfügbarkeitsgruppen in Azure](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Nächste Schritte
Über die Verwendung des **Assistenten zum Hinzufügen von Azure-Replikaten** zum Erweitern Ihrer AlwaysOn-Verfügbarkeitsgruppe in Azure hinaus können Sie auch einen Teil Ihrer SQL Server-Workloads vollständig nach Azure auslagern. Informationen zum Einstieg finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server in Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie unter [SQL Server auf virtuellen Azure-Computern](../sql/virtual-machines-windows-sql-server-iaas-overview.md).


