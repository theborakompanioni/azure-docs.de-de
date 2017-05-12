Sie können eine Verbindung mit einer VM herstellen, die in Ihrem VNet bereitgestellt wird, indem Sie eine Remotedesktopverbindung mit Ihrer VM herstellen. Die beste Möglichkeit zur anfänglichen Sicherstellung, dass eine Verbindung mit Ihrer VM hergestellt werden kann, ist die Verwendung der privaten IP-Adresse anstelle des Computernamens. Auf diese Weise können Sie testen, ob die Verbindungsherstellung möglich ist, anstatt zu überprüfen, ob die Namensauflösung richtig konfiguriert ist.

1. **Ermitteln Sie die private IP-Adresse.** Sie können die private IP-Adresse einer VM ermitteln, indem Sie sich entweder die Eigenschaften für die VM im Azure-Portal ansehen oder PowerShell verwenden.

  - Azure-Portal: Suchen Sie im Azure-Portal nach Ihrem virtuellen Computer. Zeigen Sie die Eigenschaften für die VM an. Die private IP-Adresse ist aufgeführt.

  - PowerShell: Verwenden Sie das Beispiel, um eine Liste mit VMs und privaten IP-Adressen aus Ihren Ressourcengruppen anzuzeigen. Es ist nicht erforderlich, dieses Beispiel vor der Verwendung zu ändern.

    ```powershell
    $vms = get-azurermvm
    $nics = get-azurermnetworkinterface | where VirtualMachine -NE $null

    foreach($nic in $nics)
    {
      $vm = $vms | where-object -Property Id -EQ $nic.VirtualMachine.id
      $prv = $nic.IpConfigurations | select-object -ExpandProperty PrivateIpAddress
      $alloc = $nic.IpConfigurations | select-object -ExpandProperty PrivateIpAllocationMethod
      Write-Output "$($vm.Name): $prv,$alloc"
    }
    ```

2. **Stellen Sie eine Verbindung mit der VM her.** Vergewissern Sie sich, dass eine VPN-Verbindung mit Ihrem VNet besteht. Öffnen Sie eine Remotedesktopverbindung, indem Sie in der Taskleiste im Suchfeld „RDP“ oder „Remotedesktopverbindung“ eingeben und dann „Remotedesktopverbindung“ wählen. Sie können auch den Befehl `mstsc` in PowerShell verwenden, um eine Remotedesktopverbindung zu öffnen. Geben Sie in der Remotedesktopverbindung die private IP-Adresse der VM ein. Sie können auf „Optionen anzeigen“ klicken, um weitere Einstellungen anzupassen, und dann die Verbindung herstellen.

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>So führen Sie die Problembehandlung für die RDP-Verbindung mit einer VM durch

Falls Sie beim Herstellen einer Verbindung mit einem virtuellen Computer per VPN-Verbindung Probleme haben sollten, können Sie einige Dinge überprüfen.

- Stellen Sie sicher, dass die Herstellung der VPN-Verbindung erfolgreich war.
- Stellen Sie sicher, dass Sie die Verbindung mit der privaten IP-Adresse für die VM herstellen.
- Falls Sie mit der privaten IP-Adresse eine Verbindung mit der VM herstellen können, aber nicht mit dem Computernamen, sollten Sie sich vergewissern, dass das DNS richtig konfiguriert ist.
- Weitere Informationen finden Sie unter [Behandeln von Problemen bei Remotedesktopverbindungen mit einem virtuellen Azure-Computer](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).