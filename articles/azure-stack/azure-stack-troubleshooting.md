---
title: "Problembehandlung für Microsoft Azure Stack | Microsoft-Dokumentation"
description: "Problembehandlung für Azure Stack"
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: a7309693d8f7971e42768a13d12fa0a8bd81925a
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="microsoft-azure-stack-troubleshooting"></a>Problembehandlung für Microsoft Azure Stack
Dieses Dokument enthält allgemeine Informationen zur Problembehandlung für Azure Stack. 

Da das Technical Development Kit für Azure Stack als Auswertungsumgebung angeboten wird, ist kein offizieller Support vom Microsoft-Kundensupport verfügbar.  Suchen Sie im [MSDN-Forum für Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) nach weiterer Unterstützung und Informationen, falls Sie feststellen, dass ein Problem nicht dokumentiert ist.  

Die in diesem Abschnitt beschriebenen Empfehlungen zur Behandlung von Problemen basieren auf mehreren Quellen, und es kann vorkommen, dass sich Ihr spezifisches Problem nicht mit den Empfehlungen beheben lässt. Codebeispiele werden entsprechend dem aktuellen Entwicklungsstand bereitgestellt, und die erwarteten Ergebnisse können nicht garantiert werden. An diesem Abschnitt werden im Zuge der Implementierung von Produktverbesserungen regelmäßig Änderungen und Aktualisierungen vorgenommen.

## <a name="deployment"></a>Bereitstellung
### <a name="deployment-failure"></a>Fehler bei der Bereitstellung
Wenn während der Installation ein Fehler auftritt, können Sie die Option „rerun“ des Bereitstellungsskripts verwenden, um die Bereitstellung ab dem Schritt, in dem der Fehler aufgetreten ist, neu zu starten.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Am Ende der Bereitstellung ist die PowerShell-Sitzung noch geöffnet, und es wird keine Ausgabe angezeigt.
Dies ist wahrscheinlich nur auf das Standardverhalten des PowerShell-Befehlsfensters zurückzuführen, wenn dieses ausgewählt wurde. Die Bereitstellung des Development Kits war eigentlich erfolgreich, das Skript wurde jedoch bei der Auswahl des Fensters angehalten. Sie können überprüfen, ob dies der Fall ist, indem Sie in der Titelleiste des Befehlsfensters nach dem Wort „select“ suchen.  Drücken Sie die ESC-Taste, um die Auswahl aufzuheben. Danach sollte die Abschlussmeldung angezeigt werden.

## <a name="templates"></a>Vorlagen
### <a name="azure-template-wont-deploy-to-azure-stack"></a>Die Azure-Vorlage wird nicht in Azure Stack bereitgestellt.
Überprüfen Sie Folgendes:

* Die Vorlage verwendet einen Microsoft Azure-Dienst, der bereits verfügbar oder als Vorschau in Azure Stack verfügbar ist.
* Die für eine bestimmte Ressource verwendeten APIs werden von der lokalen Azure Stack-Instanz unterstützt, und der von Ihnen als Ziel angegebene Speicherort ist gültig („local“ im Azure Stack Development Kit gegenüber „USA, Osten“ oder „Indien, Süden“ in Azure).
* Sie haben [diesen Artikel](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) zu den Test-AzureRmResourceGroupDeployment-Cmdlets gelesen, die geringfügige Unterschiede in der Azure Resource Manager-Syntax erfassen.

Sie können auch die Azure Stack-Vorlagen verwenden, die bereits im [GitHub-Repository](http://aka.ms/AzureStackGitHub/) bereitgestellt wurden, um Ihnen den Einstieg zu erleichtern.

## <a name="virtual-machines"></a>Virtuelle Computer
### <a name="default-image-and-gallery-item"></a>Standardimage und Katalogelement
Sie müssen vor der Bereitstellung virtueller Computer in Azure Stack zuerst ein Windows Server-Image und ein Katalogelement hinzufügen.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Nach einem Neustart des Azure Stack-Hosts werden einige virtuelle Computer möglicherweise nicht automatisch gestartet.
Nach dem Neustart des Hosts ist Azure Stack möglicherweise nicht sofort verfügbar.  Dies ist darauf zurückzuführen, dass [Infrastruktur-VMs](azure-stack-architecture.md#virtual-machine-roles) von Azure Stack und die RPs etwas Zeit zum Überprüfen den Konsistenz benötigen. Sie werden jedoch letztlich automatisch gestartet.

Es kann auch vorkommen, dass Mandanten-VMs nach einem Neustart des Azure Stack Development Kit-Hosts nicht automatisch gestartet werden.  Dies ist ein bekanntes Problem, und es sind nur einige manuelle Schritte erforderlich, um diese VMs wieder online zu schalten:

1.  Starten Sie auf dem Azure Stack Development Kit-Host im Startmenü den **Failovercluster-Manager**.
2.  Wählen Sie den Cluster **S-Cluster.azurestack.local** aus.
3.  Wählen Sie **Rollen** aus.
4.  Mandanten-VMs werden mit dem Zustand *gespeichert* angezeigt.  Sobald alle Infrastruktur-VMs ausgeführt werden, klicken Sie mit der rechten Maustaste auf die Mandanten-VMs, und wählen Sie **Start**, um die Ausführung des virtuellen Computers fortzusetzen.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ich habe einige virtuelle Computer gelöscht, die VHD-Dateien werden mir auf dem Datenträger aber weiterhin angezeigt. Ist dieses Verhalten zu erwarten?
Ja, dieses Verhalten ist zu erwarten. Es wurde aus den folgenden Gründen so entwickelt:

* Wenn Sie einen virtuellen Computer löschen, werden die VHDs nicht gelöscht. Datenträger sind separate Ressourcen in der Ressourcengruppe.
* Wenn ein Speicherkonto gelöscht wird, wird der Löschvorgang über Azure Resource Manager (Portal, PowerShell) sofort angezeigt, die darin enthaltenen Datenträger bleiben jedoch möglicherweise im Speicher erhalten, bis die Garbage Collection ausgeführt wird.

Wenn „verwaiste“ VHDs angezeigt werden, müssen Sie wissen, ob diese zu einem Ordner für ein gelöschtes Speicherkonto gehören. Wenn das Speicherkonto nicht gelöscht wurde, ist es normal, dass die VHDs noch angezeigt werden.

Weitere Informationen zum Konfigurieren des Schwellenwerts für die Aufbewahrung und zur bedarfsgesteuerten Anforderung der Freigabe von Speicherplatz finden Sie unter [Manage Storage Accounts](azure-stack-manage-storage-accounts.md) (Verwalten von Speicherkonten).

## <a name="storage"></a>Speicher
### <a name="storage-reclamation"></a>Freigabe von Speicherplatz
Es dauert unter Umständen bis zu 14 Stunden, bis freigegebene Kapazität im Portal angezeigt wird. Die Freigabe von Speicherplatz hängt von verschiedenen Faktoren ab, einschließlich der prozentualen Auslastung von internen Containerdateien im Blockblobspeicher. Je nach gelöschter Datenmenge besteht daher keine Garantie für die Menge des Speicherplatzes, der freigegeben werden kann, wenn der Garbage Collector ausgeführt wird.

## <a name="powershell"></a>PowerShell
### <a name="resource-providers-not-registered"></a>Ressourcenanbieter nicht registriert
Beim Herstellen einer Verbindung mit Mandantenabonnements mit PowerShell bemerken Sie unter Umständen, dass Ressourcenanbieter nicht automatisch registriert werden. Verwenden Sie das [Connect-Modul](https://github.com/Azure/AzureStack-Tools/tree/master/Connect), oder führen Sie den folgenden Befehl in PowerShell aus (nach dem [Installieren und Herstellen einer Verbindung](azure-stack-connect-powershell.md) als Mandant): 
  
       Get-AzureRMResourceProvider | Register-AzureRmResourceProvider

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

* Der interaktive CLI-Modus, d.h. der Befehl `az interactive`, wird in Azure Stack noch nicht unterstützt.
* Verwenden Sie zum Abrufen der Liste der in Azure Stack verfügbaren VM-Images den Befehl `az vm images list --all` anstelle des Befehls `az vm image list`. Wenn Sie die Option `--all` angeben, stellt dies sicher, dass die Antwort nur die Images zurückgibt, die in Ihrer Azure Stack-Umgebung verfügbar sind. 
* VM-Imagealiase, die in Azure verfügbar sind, gelten möglicherweise nicht für Azure Stack. Wenn Sie VM-Images verwenden, müssen Sie den gesamten URN-Parameter (Canonical:UbuntuServer:14.04.3-LTS:1.0.0) anstelle des Imagealias verwenden. Dieser URN muss mit den Imagespezifikationen übereinstimmen, die vom Befehl `az vm images list` abgerufen wurden.
* Standardmäßig verwendet die CLI 2.0 „Standard_DS1_v2“ als Standardgröße für VM-Images. Diese Größe ist in Azure Stack allerdings noch nicht verfügbar, daher müssen Sie den Parameter `--size` beim Erstellen eines virtuellen Computers explizit angeben. Sie können die Liste der in Azure Stack verfügbaren VM-Größen mit dem Befehl `az vm list-sizes --location <locationName>` abrufen.


## <a name="windows-azure-pack-connector"></a>Windows Azure Pack-Connector
* Wenn Sie das Kennwort des azurestackadmin-Kontos nach der Bereitstellung des Azure Stack Development Kits ändern, können Sie den Modus mit mehreren Clouds nicht mehr konfigurieren. Daher können Sie keine Verbindung mit der Windows Azure Pack-Zielumgebung herstellen.
* Nach dem Einrichten des Modus mit mehreren Clouds:
    * Benutzer können das Dashboard nur anzeigen, nachdem sie die Portaleinstellungen zurückgesetzt haben. (Klicken Sie im Benutzerportal Sie auf das Symbol für die Portaleinstellungen (Zahnradsymbol rechts oben). Klicken Sie unter **Standardeinstellungen wiederherstellen** auf **Übernehmen**.)
    * Die Dashboardtitel werden unter Umständen nicht angezeigt. Wenn dieses Problem auftritt, müssen Sie sie manuell wieder hinzufügen.
    * Einige Kacheln werden unter Umständen nicht korrekt angezeigt, wenn Sie diese zuerst zum Dashboard hinzuzufügen. Aktualisieren Sie den Browser, um dieses Problem zu beheben.




