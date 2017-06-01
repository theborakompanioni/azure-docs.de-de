---
title: Sichern virtueller Windows-Computer in Azure | Microsoft-Dokumentation
description: "Schützen Sie Ihre virtuellen Windows-Computer, indem Sie sie mithilfe von Azure Backup sichern."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/05/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: c97ac8f6df1e0fd669b3789d402e1e60f2154205
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Sichern virtueller Windows-Computern in Azure

Sie können Ihre Daten schützen, indem Sie in regelmäßigen Abständen Sicherungen erstellen. Azure Backup erstellt Wiederherstellungspunkte, die in geografisch redundanten Recovery-Tresoren gespeichert werden. Wenn Sie eine Wiederherstellung von einem Wiederherstellungspunkt durchführen, können Sie den gesamten virtuellen Computer oder nur bestimmte Dateien wiederherstellen. In diesem Artikel wird erläutert, wie eine einzelne Datei von einem virtuellen Computer mit Windows Server und IIS wiederhergestellt wird. Wenn Sie noch nicht über einen virtuellen Computer verfügen, können Sie diesen mit dem [Windows-Schnellstart](quick-create-portal.md) erstellen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Sicherung eines virtuellen Computers
> * Planen einer täglichen Sicherung
> * Wiederherstellen einer Datei aus einer Sicherung




## <a name="backup-overview"></a>Übersicht über Azure Backup

Wenn der Azure Backup-Dienst einen Sicherungsauftrag initiiert, löst er die Sicherungserweiterung zum Erstellen einer Momentaufnahme aus. Der Azure Backup-Dienst verwendet die Erweiterung _VMSnapshot_. Die Erweiterung wird während der ersten VM-Sicherung installiert, wenn die VM ausgeführt wird. Wenn die VM nicht ausgeführt wird, erstellt der Backup-Dienst eine Momentaufnahme des zugrunde liegenden Speichers (da keine Schreibvorgänge der Anwendung erfolgen, während die VM beendet ist).

Beim Erstellen einer Momentaufnahme von Windows-VMs koordiniert sich der Backup-Dienst mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um eine konsistente Momentaufnahme der Datenträger des virtuellen Computers zu erhalten. Nachdem der Azure Backup-Dienst die Momentaufnahme erstellt hat, werden die Daten in den Sicherungstresor übertragen. Um die Effizienz zu maximieren, werden vom Dienst nur diejenigen Datenblöcke bestimmt und übertragen, die seit der vorherigen Sicherung geändert wurden.

Wenn die Datenübertragung abgeschlossen ist, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.


## <a name="create-a-backup"></a>Erstellen einer Sicherung
Erstellen Sie eine einfache geplante tägliche Sicherung in einem Recovery Services-Tresor. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite **Virtuelle Computer** aus. 
3. Wählen Sie in der Liste eine zu sichernde VM aus.
4. Klicken Sie auf dem Blatt des virtuellen Computers im Abschnitt **Einstellungen** auf **Sicherung**. Das Blatt **Sicherung aktivieren** wird geöffnet.
5. Wenn kein **Recovery Services-Tresor** vorhanden ist, können Sie auf **Neu erstellen** klicken und den Namen für den neuen Tresor angeben. Ein neuer Tresor wird in derselben Ressourcengruppe und an demselben Standort wie der virtuelle Computer erstellt.
6. Klicken Sie auf **Sicherungsrichtlinie**. In diesem Beispiel behalten Sie die Standardwerte bei. Klicken Sie auf **OK**.
7. Klicken Sie auf dem Blatt **Sicherung aktivieren** auf **Sicherung aktivieren**. Dadurch wird eine tägliche Sicherung basierend auf dem Standardzeitplan erstellt.
10. Um einen erste Wiederherstellungspunkt zu erstellen, klicken Sie auf dem Blatt **Sicherung** auf **Jetzt sichern**.
11. Klicken Sie auf dem Blatt **Jetzt sichern** auf das Kalendersymbol, verwenden Sie das Kalendersteuerelement zum Auswählen des letzten Aufbewahrungstags des Wiederherstellungspunkts, und klicken Sie auf **Sichern**.
12. Auf dem Blatt **Sicherung** für Ihren virtuellen Computer wird die Anzahl der Wiederherstellungspunkte angezeigt, die abgeschlossen sind.

    ![Wiederherstellungspunkte](./media/tutorial-backup-vms/backup-complete.png)
    
Die erste Sicherung dauert ca. 20 Minuten. Fahren Sie nach dem Abschluss der Sicherung mit dem nächsten Teil dieses Tutorials fort.

## <a name="recover-a-file"></a>Wiederherstellen einer Datei

Wenn Sie eine Datei versehentlich löschen oder daran Änderungen vornehmen, können Sie diese Datei über die Dateiwiederherstellung aus dem Sicherungstresor wiederherstellen. Die Dateiwiederherstellung verwendet ein Skript, das auf dem virtuellen Computer ausgeführt wird, um den Wiederherstellungspunkt als lokales Laufwerk einzubinden. Diese Laufwerke bleiben 12 Stunden eingebunden, damit Sie Dateien aus dem Wiederherstellungspunkt kopieren und auf dem virtuellen Computer wiederherstellen können.  

In diesem Beispiel ist dargestellt, wie die Image-Datei, die auf der Standardwebseite für IIS verwendet wird, wiederhergestellt wird. 

1. Öffnen Sie einen Browser, und stellen Sie eine Verbindung mit der IP-Adresse der VM her, um die Standardseite von IIS anzuzeigen.

    ![Standardwebseite von IIS](./media/tutorial-backup-vms/iis-working.png)

2. Stellen Sie eine Verbindung zur VM her.
3. Öffnen Sie auf dem virtuellen Computer den **Datei-Explorer**, navigieren Sie zu \inetpub\wwwroot, und löschen Sie die Datei **iisstart.png**.
4. Aktualisieren Sie den Browser auf dem lokalen Computer, um sich zu vergewissern, dass das Image auf der IIS-Standardseite nicht mehr angezeigt wird.

    ![Standardwebseite von IIS](./media/tutorial-backup-vms/iis-broken.png)

5. Öffnen Sie auf Ihrem lokalen Computer eine neue Registerkarte, und navigieren Sie zum [Azure-Portal](https://portal.azure.com).
6. Wählen Sie im Menü auf der linken Seite **Virtuelle Computer** aus, und wählen Sie den virtuellen Computer aus der Liste aus.
8. Klicken Sie auf dem Blatt des virtuellen Computers im Abschnitt **Einstellungen** auf **Sicherung**. Das Blatt **Sicherung** wird geöffnet. 
9. Wählen Sie im Menü oben auf dem Blatt die Option **Dateiwiederherstellung (Vorschau)** aus. Das Blatt **Dateiwiederherstellung (Vorschau)** wird geöffnet.
10. Wählen Sie in **Schritt 1: Auswählen eines Wiederherstellungspunkts** in der Dropdownliste einen Wiederherstellungspunkt aus.
11. Klicken Sie in **Schritt 2: Herunterladen des Skripts zum Suchen und Wiederherstellen von Dateien** auf die Schaltfläche **Ausführbare Datei herunterladen**. Speichern Sie die Datei in Ihrem Ordner **Downloads**.
12. Öffnen Sie auf Ihrem lokalen Computer den **Datei-Explorer**, navigieren Sie zum **Downloads**-Ordner, und kopieren Sie die heruntergeladene EXE-Datei. Der Dateiname enthält Ihren VM-Namen als Präfix. 
13. Fügen Sie auf Ihrem virtuellen Computer (über die RDP-Verbindung) die EXE-Datei auf dem Desktop Ihrer VM ein. 
14. Navigieren Sie zum Desktop Ihrer VM, und doppelklicken Sie die EXE-Datei. Dadurch wird eine Eingabeaufforderung gestartet, und der Wiederherstellungspunkt wird als Dateifreigabe bereitgestellt, auf die Sie zugreifen können. Wenn die Erstellung der Freigabe abgeschlossen ist, geben Sie **q** ein, um die Eingabeaufforderung zu schließen.
15. Öffnen Sie Ihren virtuellen Computer, öffnen Sie **Datei-Explorer**, und navigieren Sie zum Laufwerksbuchstaben, der für die Dateifreigabe verwendet wurde.
16. Navigieren Sie zu \inetpub\wwwroot, kopieren Sie **iisstart.png** aus der Dateifreigabe, und fügen Sie es in \inetpub\wwwroot ein. Kopieren Sie beispielsweise „F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot“, um die Datei wiederherzustellen.
17. Öffnen Sie auf dem lokalen Computer die Browserregisterkarte, auf der Sie mit der IP-Adresse des virtuellen Computers verbunden sind, der die IIS-Standardseite anzeigt. Drücken Sie STRG+F5, um die Browserseite zu aktualisieren. Sie sollten nun sehen, dass das Image wiederhergestellt wurde.
18. Wechseln Sie auf dem lokalen Computer zurück zur Browserregisterkarte für das Azure-Portal, und klicken Sie in **Schritt 3: Aufheben der Bereitstellung der Datenträger nach der Wiederherstellung** auf die Schaltfläche **Bereitstellung der Datenträger aufheben**. Wenn Sie vergessen, diesen Schritt durchzuführen, wird die Verbindung mit dem Bereitstellungspunkt nach 12 Stunden automatisch getrennt. Nach diesen 12 Stunden müssen Sie ein neues Skript zum Erstellen eines neuen Bereitstellungspunkts herunterladen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Sicherung eines virtuellen Computers
> * Planen einer täglichen Sicherung
> * Wiederherstellen einer Datei aus einer Sicherung

Im nächsten Tutorial erhalten Sie Informationen zur Überwachung virtueller Computer.

> [!div class="nextstepaction"]
> [Überwachen virtueller Computer](tutorial-monitoring.md)










