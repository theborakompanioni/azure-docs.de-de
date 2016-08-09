<properties
   pageTitle="Erste Schritte mit Azure Automation DSC"
   description="Erläuterung und Beispiele für die gängigsten Aufgaben in Azure Automation DSC (Desired State Configuration, Konfiguration des gewünschten Zustands)"
   services="automation" 
   documentationCenter="na" 
   authors="eslesar" 
   manager="dongill" 
   editor="tysonn"/>

<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na" 
   ms.date="06/06/2016"
   ms.author="magoedte;eslesar"/>
   

# Erste Schritte mit Azure Automation DSC

In diesem Thema wird erläutert, wie die gängigsten Aufgaben mit Azure Automation DSC (Desired State Configuration, Konfiguration des gewünschten Zustands) ausgeführt werden, z. B. das Erstellen, Importieren und Kompilieren von Konfigurationen, das Integrieren (Onboarding) zu verwaltender Computer und Anzeigen von Berichten. Eine Übersicht über Azure Automation DSC finden Sie unter [Azure Automation DSC – Übersicht](automation-dsc-overview.md). Die DSC-Dokumentation finden Sie unter [Windows PowerShell DSC – Übersicht](https://msdn.microsoft.com/PowerShell/dsc/overview).

Dieses Thema bietet eine detaillierte Anleitung zur Verwendung von Azure Automation DSC. Wenn Sie eine Beispielumgebung wünschen, die bereits eingerichtet ist, ohne die in diesem Thema beschriebenen Schritte zu befolgen, können Sie [diese ARM-Vorlage](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup) verwenden. Mit dieser Vorlage wird eine vollständige Azure Automation DSC-Umgebung eingerichtet, einschließlich einer Azure-VM, die von Azure Automation DSC verwaltet wird.
 
## Voraussetzungen

Um die Beispiele in diesem Thema ausführen zu können, ist Folgendes erforderlich:

- Ein Azure Automation-Konto. Informationen zum Erstellen eines ausführenden Azure Automation-Kontos finden Sie unter [Azure Run As Account](automation-sec-configure-azure-runas-account.md) (Ausführendes Azure-Konto).
- Eine Azure Resource Manager-VM (nicht klassisch) unter Windows Server 2008 R2 oder höher. Eine Anleitung zum Erstellen einer VM finden Sie unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

## Erstellen einer DSC-Konfiguration

Wir erstellen eine einfache [DSC-Konfiguration](https://msdn.microsoft.com/powershell/dsc/configurations), die abhängig von der Knotenzuweisung entweder das Vorhandensein oder Nichtvorhandensein des Windows- bzw. IIS-Features **Web-Server** sicherstellt.

1. Öffnen Sie die Windows PowerShell ISE (oder einen beliebigen Texteditor).

2. Geben Sie den folgenden Text ein:

    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true

            }
        }

        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'

            }
        }
        }
    ```
3. Speichern Sie die Datei als `TestConfig.ps1`.

Diese Konfiguration ruft in jedem Knotenblock die Ressource [WindowsFeature](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource) auf, die das Vorhandensein oder Nichtvorhandensein des Features **Web-Server** sicherstellt.

## Importieren einer Konfiguration in Azure Automation

Als Nächstes importieren wir die Konfiguration in das Automation-Konto.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Konfigurationen**.

4. Klicken Sie auf dem Blatt **DSC-Konfigurationen** auf **Konfiguration hinzufügen**.

5. Wechseln Sie auf dem Blatt **Konfiguration importieren** zur Datei `TestConfig.ps1` auf Ihrem Computer.
    
    ![Screenshot des Blatts **Konfiguration importieren**](./media/automation-dsc-getting-started/AddConfig.png)
    

6. Klicken Sie auf **OK**.

## Anzeigen einer Konfiguration in Azure Automation

Nachdem Sie eine Konfiguration importiert haben, können Sie sie im Azure-Portal anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Konfigurationen**.

4. Klicken Sie auf dem Blatt **DSC-Konfigurationen** auf **TestConfig** (Name der Konfiguration, die Sie zuvor importiert haben).

5. Klicken Sie auf dem Blatt **TestConfig-Konfiguration** auf **Konfigurationsquelle anzeigen**.

    ![Screenshot des Blatts „TestConfig-Konfiguration“](./media/automation-dsc-getting-started/ViewConfigSource.png)
    
    Das Blatt **TestConfig-Konfigurationsquelle** wird geöffnet, und der PowerShell-Code der Konfiguration wird angezeigt.
    
## Kompilieren einer Konfiguration in Azure Automation

Bevor Sie einen gewünschten Status auf einen Knoten anwenden können, muss eine DSC-Konfiguration, die diesen Status definiert, in eine oder mehrere Knotenkonfigurationen (MOF-Dokumente) kompiliert und auf dem Pullserver von Automation DSC abgelegt werden. Eine ausführlichere Beschreibung der Kompilierung von Konfigurationen in Azure Automation DSC finden Sie unter [Kompilieren von Konfigurationen in Azure Automation DSC](automation-dsc-compile.md). Weitere Informationen zum Kompilieren von Konfigurationen finden Sie unter [DSC-Konfigurationen](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Konfigurationen**.

4. Klicken Sie auf dem Blatt **DSC-Konfigurationen** auf **TestConfig** (Name der Konfiguration, die Sie zuvor importiert haben).

5. Klicken Sie auf dem Blatt **TestConfig-Konfiguration** auf **Kompilieren** und dann auf **Ja**. Ein Kompilierungsauftrag wird gestartet.
    
    ![Screenshot des Blatts „TestConfig-Konfiguration“ mit hervorgehobener Schaltfläche „Kompilieren“](./media/automation-dsc-getting-started/CompileConfig.png)
    
> [AZURE.NOTE] Wenn Sie eine Konfiguration in Azure Automation kompilieren, werden alle erstellten MOF-Dateien mit der Knotenkonfiguration automatisch auf dem Pullserver bereitgestellt.

## Anzeigen eines Kompilierungsauftrags

Nachdem Sie eine Kompilierung gestartet haben, können Sie sie auf dem Blatt **Konfiguration** auf der Kachel **Kompilierungsaufträge** anzeigen. Die Kachel **Kompilierungsaufträge** zeigt derzeit ausgeführte, abgeschlossene und fehlerhafte Aufträge. Wenn Sie das Blatt eines Kompilierungsauftrags öffnen, werden Informationen zum Auftrag angezeigt, so z. B. aufgetretene Fehler und Warnungen, in der Konfiguration verwendete Eingabeparameter und Kompilierungsaufträge.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Konfigurationen**.

4. Klicken Sie auf dem Blatt **DSC-Konfigurationen** auf **TestConfig** (Name der Konfiguration, die Sie zuvor importiert haben).

5. Klicken Sie auf dem Blatt **TestConfig-Konfiguration** auf der Kachel **Kompilierungsaufträge** auf einen der aufgeführten Aufträge. Ein Blatt vom Typ **Kompilierungsauftrag** wird geöffnet, dessen Bezeichnung mit dem Startdatum des Kompilierungsauftrags versehen ist.

    ![Screenshot des Blatts „Kompilierungsauftrag“](./media/automation-dsc-getting-started/CompilationJob.png)
  
6. Klicken Sie auf dem Blatt **Kompilierungsauftrag** auf eine Kachel, um weitere Details zum Auftrag zu erhalten.

## Anzeigen von Knotenkonfigurationen

Bei erfolgreicher Erstellung eines Kompilierungsauftrags werden eine oder mehrere neue Knotenkonfigurationen erzeugt. Eine Knotenkonfiguration ist ein MOF-Dokument, das auf dem Pullserver bereitgestellt wird und per Pull abgerufen und auf Knoten angewendet werden kann. Die Knotenkonfigurationen finden Sie in Ihrem Automation-Konto auf dem Blatt **DSC-Knotenkonfigurationen**. Knotenkonfigurationen werden im Format *Konfigurationsname*.*Knotenname* benannt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Knotenkonfigurationen**.

    ![Screenshot des Blatts „DSC-Knotenkonfigurationen“](./media/automation-dsc-getting-started/NodeConfigs.png)
    
## Integrieren einer Azure-VM in die Verwaltung mit Azure Automation DSC

Mit Azure Automation DSC können Sie Azure-VMs (mit dem klassischen oder Resource Manager-Modell), lokale VMs, Linux-Computer, AWS-VMs und lokale physische Computer verwalten. In diesem Thema wird das Integrieren in die Verwaltung von ausschließlich Azure Resource Manager-VMs behandelt. Informationen zum Integrieren anderer Computertypen finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).

### So integrieren Sie eine Azure Resource Manager-VM in die Verwaltung mit Azure Automation DSC

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Knoten**.

4. Klicken Sie auf dem Blatt **DSC-Knoten** auf **Azure-VM hinzufügen**.

    ![Screenshot des Blatts „DSC-Knoten“ mit hervorgehobener Schaltfläche „Azure-VM hinzufügen“](./media/automation-dsc-getting-started/OnboardVM.png)

5. Klicken Sie auf dem Blatt **Azure-VMs hinzufügen** auf **Wählen Sie die zu integrierenden virtuellen Computer aus**.

6. Wählen Sie auf dem Blatt **VMs auswählen** den virtuellen Computer aus, der integriert werden soll, und klicken Sie auf **OK**.

    >[AZURE.IMPORTANT] Dies muss eine Azure Resource Manager-VM unter Windows Server 2008 R2 oder höher sein.
    
7. Klicken Sie auf dem Blatt **Azure-VMs hinzufügen** auf **Registrierungsdaten konfigurieren**.

8. Geben Sie auf dem Blatt **Registrierung** in das Feld **Name der Knotenkonfiguration** den Namen der Knotenkonfiguration ein, die Sie dem virtuellen Computer zuweisen möchten. Dieser muss dem Namen einer Knotenkonfiguration im Automation-Konto genau entsprechen. Das Angeben eines Namens an dieser Stelle ist optional. Sie können die zugewiesene Knotenkonfiguration nach dem Integrieren des Knotens ändern. Aktivieren Sie **Starten Sie den Knoten ggf. neu**, und klicken Sie dann auf **OK**.
    
    ![Screenshot des Blatts „Registrierung“](./media/automation-dsc-getting-started/RegisterVM.png)
    
    Die angegebene Knotenkonfiguration wird in unter **Konfigurationsmodushäufigkeit** angegebenen Intervallen auf den virtuellen Computer angewendet, und der virtuelle Computer sucht in unter **Aktualisierungshäufigkeit** angegebenen Intervallen nach Updates für die Knotenkonfiguration. Weitere Informationen zur Verwendung dieser Werte finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
    
9. Klicken Sie auf dem Blatt **Azure-VMs hinzufügen** auf **Erstellen**.

Azure startet den Prozess der Integration des virtuellen Computers. Sobald dieser abgeschlossen ist, wird der virtuelle Computer im Automation-Konto auf dem Blatt **DSC-Knoten** angezeigt.

## Anzeigen der Liste mit DSC-Knoten

Auf dem Blatt **DSC-Knoten** in Ihrem Automation-Konto können Sie die Liste aller Computer anzeigen, die in die Verwaltung integriert wurden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Knoten**.

## Anzeigen von Berichten für DSC-Knoten

Immer wenn Azure Automation DSC eine Konsistenzprüfung auf einem verwalteten Knoten ausführt, sendet der Knoten einen Statusbericht zurück zum Server. Sie können diese Berichte auf dem Blatt dieses Knotens anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Knoten**.

4. Klicken Sie auf der Kachel **Berichte** auf einen der Berichte in der Liste.

    ![Screenshot des Blatts „Bericht“](./media/automation-dsc-getting-started/NodeReport.png)

Auf dem Blatt einen einzelnen Berichts können Sie die folgenden Statusinformationen für die zugehörige Konsistenzprüfung finden:
- Berichtstatus: Gibt an, ob der Knoten kompatibel, die Konfiguration fehlerhaft oder der Knoten nicht kompatibel (Knoten befindet sich im Modus **applyandmonitor** und Computer nicht im gewünschten Zustand) ist.
- Startzeit der Konsistenzprüfung.
- Gesamtdauer der Konsistenzprüfung.
- Typ der Konsistenzprüfung.
- Fehler, einschließlich Fehlercode und -meldung.
- Alle in der Konfiguration verwendeten DSC-Ressourcen und der Status jeder Ressource (ob der Knoten den gewünschten Status für diese Ressource hat): Sie können auf die einzelnen Ressourcen klicken, um detailliertere Informationen zu erhalten.
- Name, IP-Adresse und Konfigurationsmodus des Knotens.

Sie können auch auf **Unformatierten Bericht anzeigen** klicken, um die tatsächlichen Daten anzuzeigen, die der Knoten an den Server sendet. Weitere Informationen zur Verwendung dieser Daten finden Sie unter [Verwenden eines DSC-Berichtsservers](https://msdn.microsoft.com/powershell/dsc/reportserver).

Nachdem ein Knoten in die Verwaltung integriert wurde, kann es einige Zeit dauern, bis der erste Bericht verfügbar ist. Sie müssen möglicherweise bis zu 30 Minuten auf den ersten Bericht warten, nachdem Sie einen Knoten integriert haben.

## Neuzuweisen eines Knotens zu einer anderen Knotenkonfiguration

Sie können einen Knoten einer anderen Knotenkonfiguration als der ursprünglich zugewiesenen zuweisen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Knoten**.

4. Klicken Sie auf dem Blatt **DSC-Knoten** auf den Namen des Knotens, den Sie neu zuweisen möchten.

5. Klicken Sie auf dem Blatt dieses Knotens auf **Knoten zuweisen**.

    ![Screenshot des Blatts „Knoten“ mit hervorgehobener Schaltfläche „Knoten zuweisen“](./media/automation-dsc-getting-started/AssignNode.png)

6. Wählen Sie auf dem Blatt **Knotenkonfiguration zuweisen** die Knotenkonfiguration aus, der Sie den Knoten zuweisen möchten, und klicken Sie dann auf **OK**.

    ![Screenshot des Blatts „Knotenkonfiguration zuweisen“](./media/automation-dsc-getting-started/AssignNodeConfig.png)
    
## Aufheben der Registrierung eines Knotens

Wenn ein Knoten nicht mehr von Azure Automation DSC verwaltet werden soll, können Sie seine Registrierung aufheben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.

3. Klicken Sie auf dem Blatt **Automation-Konto** auf **DSC-Knoten**.

4. Klicken Sie auf dem Blatt **DSC-Knoten** auf den Namen des Knotens, dessen Registrierung Sie aufheben möchten.

5. Klicken Sie auf dem Blatt dieses Knotens auf **Registrierung aufheben**.

    ![Screenshot des Blatts „Knoten“ mit hervorgehobener Schaltfläche „Registrierung aufheben“](./media/automation-dsc-getting-started/UnregisterNode.png)

## Verwandte Artikel
* [Azure Automation DSC – Übersicht](automation-dsc-overview.md)
* [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md)
* [Windows PowerShell DSC – Übersicht](https://msdn.microsoft.com/powershell/dsc/overview)
* [Azure Automation DSC-Cmdlets](https://msdn.microsoft.com/library/mt244122.aspx)
* [Azure Automation DSC – Preise](https://azure.microsoft.com/pricing/details/automation/)

<!---HONumber=AcomDC_0727_2016-->