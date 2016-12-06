


Eine Verfügbarkeitsgruppe hilft, dass Ihre virtuellen Computer während eines Ausfalls wie während einer Wartung verfügbar bleiben. Das Platzieren von mindestens zwei ähnlich konfigurierten virtuellen Computern in einer Verfügbarkeitsgruppe erstellt die Redundanz, die zum Wahren der Verfügbarkeit der Anwendungen oder Dienste erforderlich ist, die auf Ihrem virtuellen Computer ausgeführt werden. Weitere Informationen dazu, wie dies funktioniert, finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer][Verwalten der Verfügbarkeit virtueller Computer].

Sie sollten eine Kombination aus Verfügbarkeitssätzen und Endpunkten mit Lastenausgleich verwenden, um sicherzustellen, dass Ihre Anwendung stets verfügbar ist und effizient ausgeführt wird. Weitere Informationen zu Endpunkten mit Lastenausgleich finden Sie unter [Lastenausgleich für Azure-Infrastrukturdienste][Lastenausgleich für Azure-Infrastrukturdienste].

Mithilfe einer der beiden folgenden Optionen können Sie klassische virtuelle Computer einer Verfügbarkeitsgruppe hinzufügen:

* [Option 1: Erstellen Sie gleichzeitig einen virtuellen Computer und eine Verfügbarkeitsgruppe][Option 1: Erstellen Sie gleichzeitig einen virtuellen Computer und eine Verfügbarkeitsgruppe]. Fügen Sie der Gruppe anschließend die neuen virtuellen Computer hinzu, wenn Sie diese virtuellen Computer erstellen.
* [Option 2: Fügen Sie einer Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzu][Option 2: Fügen Sie einer Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzu].

> [!NOTE]
> Beim klassischen Modell müssen virtuelle Maschinen, die Sie in dieselbe Verfügbarkeitsgruppe versetzen möchten, zum selben Clouddienst gehören.
> 
> 

## <a id="createset"> </a>Option 1: Erstellen Sie gleichzeitig einen virtuellen Computer und eine Verfügbarkeitsgruppe
Dafür können Sie entweder das Azure-Portal oder die Azure PowerShell-Befehle verwenden.

So verwenden Sie das Azure-Portal:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.
2. Klicken Sie im Hub-Menü auf **+ Neu** und anschließend auf **Virtueller Computer**.
   
    ![Alternativer Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Wählen Sie das zu verwendende Marketplace-VM-Image aus. Sie können auswählen, ob Sie einen virtuellen Linux- oder einen virtuellen Windows-Computer erstellen.
4. Stellen Sie für den ausgewählten virtuellen Computer sicher, dass als Bereitstellungsmodell **Klassisch** festgelegt ist, und klicken Sie dann auf **Erstellen**.
   
    ![Alternativer Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Geben Sie einen Namen, einen Benutzernamen und ein Kennwort für den virtuellen Computer (Windows-Computer) oder den öffentlichen SSH-Schlüssel (Linux-Computer) ein. 
6. Wählen Sie eine Größe für den virtuellen Computer aus, und klicken Sie anschließend auf **Auswählen** , um den Vorgang fortzusetzen.
7. Navigieren Sie zu **Optionale Konfiguration > Verfügbarkeitsgruppe**, und wählen Sie die Verfügbarkeitsgruppe aus, der Sie den virtuellen Computer hinzufügen möchten.
   
    ![Alternativer Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Überprüfen Sie die Konfigurationseinstellungen. Klicken Sie auf **Erstellen**, wenn Sie fertig sind.
9. Im Hub-Menü können Sie unter **Virtuelle Computer** den Erstellungsstatus des virtuellen Computers nachverfolgen.

Beachten Sie die Informationen unter [Erstellen von virtuellen Windows-Computern mit PowerShell und dem klassischen Bereitstellungsmodell](../articles/virtual-machines/virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), wenn Sie einen virtuellen Azure-Computer mithilfe von Azure PowerShell-Befehlen erstellen und ihn einer neuen oder vorhandenen Verfügbarkeitsgruppe hinzufügen.

## <a id="addmachine"> </a>Option 2: Fügen Sie einer Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzu
Im Azure-Portal können Sie einer vorhandenen Verfügbarkeitsgruppe vorhandene klassische virtuelle Computer hinzufügen oder eine neue Verfügbarkeitsgruppe für Computer erstellen. (Beachten Sie, dass die virtuellen Computer in derselben Verfügbarkeitsgruppe zum selben Clouddienst gehören müssen.) Die Schritte sind beinah identisch. Mit Azure PowerShell können Sie einer vorhandenen Verfügbarkeitsgruppe den virtuellen Computer hinzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.
2. Klicken Sie im Hub-Menü auf **Virtuelle Computer (klassisch)**.
   
    ![Alternativer Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Klicken Sie in der Liste der virtuellen Computer auf die Namen der virtuellen Computer, die Sie der Gruppe hinzufügen möchten.
4. Wählen Sie in den **Einstellungen** des virtuellen Computers die Option **Verfügbarkeitsgruppe**.
   
    ![Alternativer Bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Wählen Sie die Verfügbarkeitsgruppe aus, der Sie virtuelle Computer hinzufügen möchten. Die virtuellen Computer müssen zum selben Clouddienst gehören wie die Verfügbarkeitsgruppe.
   
    ![Alternativer Bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klicken Sie auf **Speichern**.

Um Azure PowerShell-Befehle zu verwenden, öffnen Sie eine Azure PowerShell-Sitzung mit Administratorrechten, und führen Sie den folgenden Befehl aus. Für die Platzhalter (z.B. &lt;VmCloudServiceName&gt;) ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen < und >, durch die richtigen Namen.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Der virtuelle Computer muss möglicherweise neu gestartet werden, um das Hinzufügen zur Verfügbarkeitsgruppe abzuschließen.
> 
> 

<!-- LINKS -->
[Option 1: Erstellen Sie gleichzeitig einen virtuellen Computer und eine Verfügbarkeitsgruppe]: #createset
[Option 2: Fügen Sie einer Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzu]: #addmachine

[Lastenausgleich für Azure-Infrastrukturdienste]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Verwalten der Verfügbarkeit virtueller Computer]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md

[Erstellen eines virtuellen Computers unter Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtuelle Netzwerke im Überblick]: ../articles/virtual-network/virtual-networks-overview.md



<!--HONumber=Nov16_HO3-->


