---
title: Aktivieren von FTP in App Service in Azure Stack | Microsoft-Dokumentation
description: Schritte zum Aktivieren von FTP in App Service in Azure Stack
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: anwestg
ms.translationtype: HT
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: bab69a9ff7e101f22713ba7f1ac2cbd0add7fdbd
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="enable-ftp-in-app-service-on-azure-stack"></a>Aktivieren von FTP in App Service in Azure Stack

Wenn Sie nach dem erfolgreichen Bereitstellen von App Service in Azure Stack die FTP-Veröffentlichung aktivieren möchten, damit Ihre Mandanten ihre Anwendungsdateien und Inhalte hochladen können, müssen einige zusätzliche Schritte erfolgen.  In künftigen Versionen werden diese Schritte automatisiert sein.

> [!NOTE]
> Diese Schritte müssen von Dienst- oder Organisationsadministratoren zum Konfigurieren eines Ressourcenanbieters für App Service in Azure Stack ausgeführt werden.

## <a name="enable-ftp"></a>Aktivieren von FTP

1.  Melden Sie sich beim Azure Stack-Portal als Dienstadministrator an.
2.  Navigieren Sie zu **Network interfaces** (Netzwerkschnittstellen), und wählen Sie die **FTP-NIC** unter **Resource Group** - **AppService-LOCAL** (Ressourcengruppe>AppService-LOCAL) aus. ![Azure Stack-Netzwerkschnittstellen][1]
3.  Notieren Sie die öffentliche IP-Adresse in **Public IP Address** der **FTP-NIC**. 
![Details zu Azure Stack-Netzwerkschnittstellen][2]
4.  Wechseln Sie als Nächstes zu **Virtual Machines** (Virtuelle Computer), und wählen Sie die **FTP0-VM** aus. ![Azure Stack: VMs][3]
5.  Öffnen Sie eine Remotedesktopsitzung mit der VM über die Schaltfläche **Connect** (Verbinden), und melden Sie sich mit den Administratoranmeldeinformationen, die Sie während der App Service-Bereitstellung festgelegt haben, bei der Sitzung an.  
![Azure Stack: VM-Details][4]
6.  Öffnen Sie auf der FTP-VM (FTP0-VM) **Internetinformationsdienste (IIS)-Manager**.
7.  Wählen Sie unter **Websites** die Option **Hosting FTP Site** (Hostende FTP-Site).
8.  Öffnen Sie **FTP-Firewallunterstützung**. ![IIS-Manager in App Service FTP0-VM][5]
9.  Geben Sie die öffentliche IP-Adresse der FTP-NIC ein, und klicken Sie auf **Übernehmen**. ![IIS-Manager: FTP-Firewallunterstützung][6]

## <a name="validate-the-enabling-of-ftp"></a>Überprüfen des Aktivierens von FTP

1.  Melden Sie sich beim Azure Stack-Portal entweder als Dienstadministrator oder Mandant an.
2.  Navigieren Sie zu **App Services**, und wählen Sie die Web-, mobile oder API-App aus, die Sie erstellt haben. ![App Services][7]
3.  Notieren Sie in den Anwendungsdetails den **FTP-Hostnamen** und den **FTP-/Bereitstellungsbenutzernamen**. ![App Service: App-Details][8]
> [!NOTE]
> Wenn unter **FTP-/Bereitstellungsbenutzername** kein Eintrag angezeigt wird, müssen Sie zuerst auf dem Blatt **Anmeldeinformationen für Bereitstellung** die Anmeldeinformationen für die Bereitstellung festlegen.

4.  Öffnen Sie Windows Explorer, und geben Sie den FTP-Hostnamen in die Dateiadressleiste ein, z.B. ftp://ftp.appservice.azurestack.local
5.  Wenn Sie aufgefordert werden, geben Sie die in Schritt 3 notierten **Bereitstellungsanmeldeinformationen** ein. Wenn dieses Feature aktiviert wurde, wird eine Verzeichnisliste mit dem Inhalt der App Service-Anwendung angezeigt. ![FTP-Dateiliste][9]
<!--Image references-->
[1]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interfaces.png
[2]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interface-details.png
[3]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines.png
[4]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines-FTP0-VM.png
[5]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager.png
[6]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager-FTP-Firewall-Support.png
[7]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-services.png
[8]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-service-app-detail.png
[9]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-ftp-file-listing.png

