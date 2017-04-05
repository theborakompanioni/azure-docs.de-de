
1. Melden Sie sich bei Ihrem Azure-Abonnement an, indem Sie die Schritte unter [Herstellen einer Verbindung mit Azure von der Azure Befehlszeilenschnittstelle (Azure-CLI 1.0)](../articles/xplat-cli-connect.md)ausführen.

2. Stellen Sie wie folgt sicher, dass der klassische Bereitstellungsmodus aktiviert ist:

    ```azurecli
    azure config mode asm
    ```

3. Wählen Sie aus den verfügbaren Images wie folgt das Linux-Image aus, das Sie laden möchten:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    In einem Windows-Eingabeaufforderungsfenster verwenden Sie **find** anstelle von „grep“.
   
4. Verwenden Sie `azure vm create`, um eine VM mit dem Linux-Image aus der obigen Liste zu erstellen. Hierdurch werden ein Clouddienst und ein neues Speicherkonto erstellt. Sie können auch mithilfe der Option `-c` eine Verbindung zwischen dieser VM und einem vorhandenen Clouddienst herstellen. Erstellen Sie einen SSH-Endpunkt zum Anmelden beim virtuellen Linux-Computer mit der Option `-e`. Im folgenden Beispiel wird eine VM mit dem Namen `myVM` erstellt, indem das `Ubuntu-14_04_4-LTS`-Image am Standort `West US` verwendet und der Benutzername `ops` hinzugefügt wird:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > Geben Sie für einen virtuellen Linux-Computer die `-e`-Option in `vm create` an. SSH kann nach der Erstellung des virtuellen Computers nicht mehr aktiviert werden. Weitere Informationen zu SSH finden Sie unter [Verwenden von SSH mit Linux in Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Sie können die Attribute der VM mithilfe des Befehls `azure vm show` überprüfen. Im folgenden Beispiel werden Informationen zur VM mit dem Namen `myVM` aufgeführt:

    ```azurecli   
    azure vm show myVM
    ```

6. Starten Sie Ihre VM wie folgt mit dem Befehl `azure vm start`:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Nächste Schritte
Ausführliche Informationen zu diesen Azure-CLI 1.0-Befehlen für virtuelle Computer finden Sie unter [Verwendung der Azure CLI 1.0 mit der API der klassischen Bereitstellung](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

