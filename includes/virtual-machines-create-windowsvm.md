1. Melden Sie sich beim [klassischen Portal](http://manage.windowsazure.com)an. 
2. Klicken Sie in der Befehlsleiste am unteren Ende des Fensters auf **Neu**.
3. Unter **Compute** klicken Sie auf **Virtueller Computer** und anschließend auf **From Gallery** (Aus Galerie).
   
    ![Navigieren Sie auf der Befehlsleiste zu „Aus Katalog“.](./media/virtual-machines-create-WindowsVM/fromgallery.png)
4. Im ersten Bildschirm können Sie für Ihren virtuellen Computer ein **Image** aus einer Liste im Image-Katalog auswählen. Sie können ein Image aus dem Katalog oder aus von Ihnen hochgeladenen Images und Datenträgern auswählen. Die verfügbaren Images können sich je nach dem von Ihnen verwendeten Abonnement unterscheiden.
5. Im zweiten Bildschirm wählen Sie einen Computernamen, Größe sowie Benutzername und Passwort des Administrators aus. Andernfalls wählen Sie die benötigte Schicht und die Größe, um Ihre Anwendung oder Ihren Leistungsumfang auszuführen. Hier einige Tipps:
   
   * **Name des virtuellen Computers** darf nur Buchstaben, Zahlen und Bindestriche enthalten. Er muss zudem mit einem Buchstaben beginnen und mit einem Buchstaben oder einer Zahl enden.
   * **Neuer Benutzername** bezieht sich auf das Administratorkonto, dass Sie zur Verwaltung des Servers verwenden. Das Kennwort muss 8 bis 123 Zeichen lang sein und mindestens drei der folgenden Elemente enthalten: Kleinbuchstaben, Großbuchstaben, eine Zahl und ein Sonderzeichen. **Sie brauchen den Benutzernamen und das Kennwort, um sich am virtuellen Computer anzumelden**.
   * Die Größe eines virtuellen Computers wirkt sich auf seine Betriebskosten sowie auf die Konfigurationsoptionen aus (z. B. die Anzahl von Datenträgern, die Sie anschließen können). Weitere Informationen finden Sie unter [Größen für virtuelle Computer](../articles/virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
6. Auf dem dritten Bildschirm können Sie die Ressourcen für Netzwerk, Speicher und Verfügbarkeit konfigurieren. Hier einige Tipps:
   
   * Der **DNS-Name des Clouddienstes** ist der allgemeine DNS-Name, der Teil des URIs wird, das verwendet wird, um den virtuellen Computer zu kontaktieren. Sie müssen einen eigenen Namen für den Clouddienst eingeben, da dieser in Azure eindeutig sein muss. Clouddienste sind wichtig für Szenarios, die [mehrere virtuelle Computer](../articles/virtual-machines/virtual-machines-windows-classic-connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)verwenden.
   * Bei **Region/Affinitätsgruppe/Virtuelles Netzwerk**geben Sie eine Region an, die zu Ihrem Ort passt. Sie können stattdessen ein virtuelles Netzwerk angeben.
   * Wenn der virtuelle Computer ein virtuelles Netzwerk verwenden soll, **müssen** Sie das virtuelle Netzwerk beim Erstellen des virtuellen Computers angeben. Sie können den virtuellen Computer nicht mit einem virtuellen Netzwerk verknüpfen, nachdem Sie den virtuellen Computer erstellt haben. Weitere Informationen finden Sie unter [Überblick über virtuelle Azure-Netzwerke](../articles/virtual-network/virtual-networks-overview.md).
   * Detaillierte Informationen zur Konfiguration von Endpunkten finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
7. Auf dem vierten Konfigurationsbildschirm können Sie den VM-Agent installieren und einige der verfügbaren Erweiterungen konfigurieren.
   
   > [!NOTE]
   > Der VM-Agent stellt Ihnen die Umgebung für das Installieren von Erweiterungen bereit, die Ihnen bei der Interaktion mit dem virtuellen Computer oder bei dessen Verwaltung helfen können. Details finden Sie unter [Informationen zum VM-Agent und zu Erweiterungen](../articles/virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).  
   > 
   > 
8. Nach der Erstellung des virtuellen Computers wird der neue virtuelle Computer im klassischen Portal unter **Virtuelle Computer** aufgelistet. In diesen Abschnitten werden auch der entsprechende Clouddienst und das Speicherkonto erstellt und aufgelistet. Der virtuelle Computer und der Clouddienst werden automatisch gestartet. Das Verwaltungsportal zeigt ihren Status als **Wird ausgeführt** an.
   
    ![Konfigurieren Sie den VM-Agent und die Endpunkte des virtuellen Computers.](./media/virtual-machines-create-WindowsVM/vmcreated.png)



<!--HONumber=Feb17_HO2-->


