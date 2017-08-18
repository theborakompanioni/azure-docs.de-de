---
title: "Erstellen eines klassischen virtuellen Azure-Computers, auf dem MySQL ausgeführt wird | Microsoft-Dokumentation"
description: Erstellen Sie einen virtuellen Azure-Computer mit Windows Server 2012 R2 und der MySQL-Datenbank mithilfe des klassischen Bereitstellungsmodells.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 98fa06d2-9b92-4d05-ac16-3f8e9fd4feaa
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 11850e5ce20efae88a7af9c1d2e4761ed2b70cd7
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2016"></a>Installieren von MySQL auf einem virtuellen Computer mit Windows Server 2016, der mit dem klassischen Bereitstellungsmodell erstellt wurde
[MySQL](https://www.mysql.com) ist eine beliebte Open-Source-SQL-Datenbank. In diesem Tutorial erfahren Sie, wie Sie die **Communityversion von MySQL 5.7.18** als MySQL-Server auf einem virtuellen Computer mit **Windows Server 2016** installieren und ausführen. Ihre Erfahrung ist möglicherweise für andere Versionen von MySQL oder Windows Server unterschiedlich.

Anleitungen zum Installieren von MySQL unter Linux finden Sie unter [Installieren von MySQL in Azure](../../linux/mysql-install.md).

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

## <a name="create-a-virtual-machine-running-windows-server-2016"></a>Erstellen eines virtuellen Computers unter Windows Server 2016
Wenn Sie noch nicht über einen virtuellen Computer mit Windows Server 2016 verfügen, können Sie diesen Computer anhand dieses [Tutorials](./tutorial.md) erstellen.

## <a name="attach-a-data-disk"></a>Datenträger anfügen
Nachdem der virtuelle Computer erstellt wurde, können Sie optional einen Datenträger anfügen. Das Hinzufügen eines Datenträgers empfiehlt sich für Produktionsworkloads und um zu vermeiden, dass zu wenig Speicherplatz auf dem Betriebssystemlaufwerk (C:) verfügbar ist.

Befolgen Sie die Anweisungen zum Anfügen eines leeren Datenträgers unter [Anfügen eines Datenträgers an einen virtuellen Windows-Computer](../attach-managed-disk-portal.md). Legen Sie die Hostcacheeinstellung auf **Keine** oder **Schreibgeschützt** fest.

## <a name="log-on-to-the-virtual-machine"></a>Melden Sie sich beim virtuellen Computer an.
Als Nächstes [melden Sie sich am virtuellen Computer an](./connect-logon.md) , sodass Sie MySQL installieren können.

## <a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Installieren und Ausführen von MySQL Community Server auf dem virtuellen Computer
Befolgen Sie diese Schritte, um die Community-Version von MySQL Server zu installieren, zu konfigurieren und auszuführen:

> [!NOTE]
> Beim Herunterladen von Elementen mithilfe von Internet Explorer können Sie die Option **Verstärkte Sicherheitskonfiguration für IE** deaktivieren, um das Herunterladen zu vereinfachen. Klicken Sie im Startmenü auf „Verwaltung > Server-Manager > Lokaler Server“ und anschließend auf **Verstärkte Sicherheitskonfiguration für IE** (um damit die Konfiguration auf „Aus“ festzulegen).
>
>

1. Wenn Sie über Remotedesktop eine Verbindung zum virtuellen Computer aufgebaut haben, klicken Sie im Startbildschirm auf **Internet Explorer** .
2. Wählen Sie oben rechts die Schaltfläche **Tools** (Zahnradsymbol) aus, und klicken Sie dann auf **Internetoptionen**. Klicken Sie auf die Registerkarte **Sicherheit**, auf das Symbol **Vertrauenswürdige Sites** und dann auf die Schaltfläche **Sites**. Fügen Sie http://*.mysql.com der Liste vertrauenswürdiger Websites hinzu. Klicken Sie auf **Schließen** und dann auf **OK**.
3. Geben Sie auf der Adressleiste von Internet Explorer https://dev.mysql.com/downloads/mysql/ ein.
4. Verwenden Sie die MySQL-Website, um die neueste Version von MySQL-Installer für Windows zu suchen und herunterzuladen. Laden Sie bei Auswahl des MySQL-Installers die Version mit dem kompletten Dateisatz herunter (beispielsweise „mysql-installer-community-5.7.18.0.msi“ mit einer Dateigröße von 352,8 MB), und speichern Sie die Installationsprogrammdatei.
5. Nachdem das Installationsprogramm heruntergeladen wurde, klicken Sie auf **Ausführen** , um die Installation zu starten.
6. Stimmen Sie auf der Seite **Lizenzvereinbarung** der Lizenzvereinbarung zu, und klicken Sie auf **Weiter**.
7. Klicken Sie auf der Seite **Einrichtungstyp wählen** auf den gewünschten Einrichtungstyp, und klicken Sie dann auf **Weiter**. Bei den folgenden Schritten wird davon ausgegangen, dass der Einrichtungstyp **Nur Server** ausgewählt ist.
8. Wenn die Seite **Anforderungen überprüfen** angezeigt wird, klicken Sie auf **Ausführen**, damit das Installationsprogramm fehlende Komponenten installieren kann. Befolgen Sie die angezeigten Anweisungen, z.B. für die C++ Redistributable Runtime.
9. Klicken Sie auf der Seite **Installation** auf **Ausführen**. Klicken Sie nach Abschluss der Installation auf **Weiter**.

10. Klicken Sie auf der Seite **Produktkonfiguration** auf **Weiter**.

11. Geben Sie auf der Seite **Typ und Netzwerke** Ihren gewünschten Konfigurationstyp und die Konnektivitätsoptionen an, einschließlich des TCP-Ports (falls erforderlich). Wählen Sie **Erweiterte Optionen anzeigen** aus, und klicken Sie dann auf **Weiter**.
    ![](./media/mysql-2008r2/MySQL_TypeNetworking.png)

12. Geben Sie auf der Seite **Konten und Rollen** ein sicheres MySQL-root-Kennwort ein. Fügen Sie bei Bedarf zusätzliche MySQL-Benutzerkonten hinzu, und klicken Sie dann auf **Weiter**.

    ![](./media/mysql-2008r2/MySQL_AccountsRoles_Filled.png)
13. Geben Sie auf der Seite **Windows-Dienst** nach Bedarf Änderungen an den Standardeinstellungen für die Ausführung von MySQL Server als Windows-Dienst an, und klicken Sie dann auf **Weiter**.

    ![](./media/mysql-2008r2/MySQL_WindowsService.png)
14. Die Optionen auf der Seite **Plug-Ins und Erweiterungen** sind optional. Klicken Sie auf zum Fortfahren auf **Weiter**.
15. Geben Sie auf der Seite **Erweiterte Optionen** nach Bedarf Änderungen an Protokollierungsoptionen an, und klicken Sie dann auf **Weiter**.

    ![](./media/mysql-2008r2/MySQL_AdvOptions.png)
16. Klicken Sie auf der Seite **Serverkonfiguration anwenden** auf **Ausführen**. Wenn die Konfigurationsschritte abgeschlossen sind, klicken Sie auf **Fertig stellen**.
17. Klicken Sie auf der Seite **Produktkonfiguration** auf **Weiter**.
18. Klicken Sie auf der Seite **Installation abgeschlossen** auf **Protokoll in Zwischenablage kopieren**, wenn Sie es später prüfen möchten, und klicken Sie dann auf **Fertig stellen**.
19. Geben Sie auf dem Startbildschirm **mysql** ein, und klicken Sie dann auf den **MySQL 5.7-Befehlszeilenclient**.
20. Geben Sie das root-Kennwort ein, das Sie in Schritt 12 angegeben haben. Es wird eine Eingabeaufforderung angezeigt, in der Sie Befehle zum Konfigurieren von MySQL angeben können. Details zu Befehlen und Syntax finden Sie unter [MySQL-Referenzhandbücher](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html).

    ![](./media/mysql-2008r2/MySQL_CommandPrompt.png)
21. Sie können auch die Standardeinstellungen für die Serverkonfiguration festlegen, z.B. die Basis- und Datenverzeichnisse sowie die Laufwerke. Weitere Informationen finden Sie unter [6.1.2 Serverkonfiguration: Standardeinstellungen](https://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Endpunkte konfigurieren

Damit der MySQL-Dienst auf Clientcomputern im Internet verfügbar ist, müssen Sie einen Endpunkt für den TCP-Port konfigurieren und eine Windows-Firewallregel erstellen. Der Standardport, an dem der MySQL-Serverdienst auf MySQL-Clients lauscht, ist 3306. Sie können einen anderen Port angeben, solange der Port mit dem Wert auf der Seite **Typ und Netzwerke** (Schritt 11 des vorherigen Verfahrens) übereinstimmt.

> [!NOTE]
> Sie sollten in einer Produktionsumgebung die Auswirkungen auf die Sicherheit sorgfältig abwägen, da dadurch der MySQL-Serverdienst auf allen Computern im Internet verfügbar ist. Sie können den Satz der Quell-IP-Adressen definieren, die den Endpunkt mit einer Zugriffssteuerungsliste (ACL) verwenden dürfen. Weitere Informationen finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](setup-endpoints.md).
>
>

So konfigurieren Sie einen Endpunkt für den MySQL Server-Dienst:

1. Klicken Sie im Azure-Portal auf **Virtuelle Computer (klassisch)**, auf den Namen des virtuellen MySQL-Computers und dann auf **Endpunkte**.
2. Klicken Sie in der Befehlsleiste auf **Hinzufügen**.
3. Geben Sie auf der Seite **Endpunkt hinzufügen** in **Name** einen eindeutigen Namen ein.
4. Wählen Sie als Protokoll **TCP** aus.
5. Geben Sie die Portnummer (z.B. **3306**) in die Felder **Öffentlicher Port** und **Privater Port** ein, und klicken Sie auf **OK**.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Hinzufügen einer Windows-Firewallregel zum Zulassen von MySQL-Datenverkehr
Um eine Windows-Firewallregel hinzuzufügen, die MySQL-Datenverkehr aus dem Internet zulässt, führen Sie den folgenden Befehl auf dem virtuellen MySQL Server-Computer an einer _Eingabeaufforderung mit erweiterten Berechtigungen von Windows PowerShell_ aus.

    New-NetFirewallRule -DisplayName "MySQL57" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public

## <a name="test-your-remote-connection"></a>Testen der Remoteverbindung
Zum Testen der Remoteverbindung mit dem virtuellen Azure-Computer, auf dem der MySQL-Serverdienst ausgeführt wird, müssen Sie den DNS-Namen des Clouddiensts mit dem virtuellen Computer angeben.

1. Klicken Sie im Azure-Portal auf **Virtuelle Computer (klassisch)**, auf den Namen des virtuellen MySQL-Servercomputers und dann auf **Übersicht**.
2. Notieren Sie den Wert für **DNS-Name** auf dem Dashboard. Beispiel:

   ![](media/mysql-2008r2/MySQL_DNSName.png)
3. Führen Sie den folgenden Befehl zur Anmeldung als MySQL-Benutzer aus, von einem lokalen Computer, der MySQL oder den MySQL-Client ausführt.

     mysql -u <yourMysqlUsername> -p -h <yourDNSname>

   Beispiel: Verwenden Sie den folgenden Befehl für den MySQL-Benutzernamen _dbadmin3_ und den DNS-Namen _testmysql.cloudapp.net_ für den virtuellen Computer, um mit der Verwendung von MySQL zu beginnen:

     mysql -u dbadmin3 -p -h testmysql.cloudapp.net

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Ausführen von MySQL finden Sie in der [MySQL-Dokumentation](http://dev.mysql.com/doc/).

