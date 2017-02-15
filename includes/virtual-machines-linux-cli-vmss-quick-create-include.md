Falls noch nicht erfolgt, können Sie eine [kostenlose Testversion eines Azure-Abonnements](https://azure.microsoft.com/pricing/free-trial/) und die [Azure-CLI](../articles/xplat-cli-install.md) erhalten, [die mit Ihrem Azure-Konto verbunden ist](../articles/xplat-cli-connect.md). Vergewissern Sie sich wie folgt, dass sich die Azure-Befehlszeilenschnittstelle im Resource Manager-Modus befindet.

```azurecli
azure config mode arm
```

Erstellen Sie jetzt die Skalierungsgruppe mithilfe des Befehls `azure vmss quick-create`. Das folgende Beispiel erstellt eine Linux-Skalierungsgruppe mit dem Namen `myVMSS` mit fünf VM-Instanzen in der Ressourcengruppe mit dem Namen `myResourceGroup`:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q Canonical:UbuntuServer:16.04.0-LTS:latest
```

Das folgende Beispiel erstellt eine Windows-Skalierungsgruppe mit der gleichen Konfiguration:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest
```

Falls Sie den Standort oder den URN (Uniform Resource Name) des Images anpassen möchten, sehen Sie sich bitte die Befehle `azure location list` und `azure vm image {list-publishers|list-offers|list-skus|list|show}` an.

Wenn der Befehl zurückgegeben wurde, wurde die Skalierungsgruppe erstellt. Zu dieser Skalierungsgruppe gehört ein Lastenausgleich mit NAT-Regeln, das den Port 50.000+i des Lastenausgleichs dem Port 22 der VM i zuordnet. Sobald wir den FQDN des Load Balancers herausgefunden haben, können wir daher mithilfe des SSH-Protokolls eine sichere Verbindung mit unseren virtuellen Computern herstellen:

```bash
# (if you decide to run this as a script, please invoke using bash)

# list load balancers in the resource group we created
#
# generic syntax:
# azure network lb list -g RESOURCE-GROUP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb list -g negatvmssrg | grep negatvmssrg)
split_line=( $line )
lb_name=${split_line[1]}

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is 
# associated to it
#
# generic syntax:
# azure network lb show -g RESOURCE-GROUP-NAME -n LOAD-BALANCER-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb show -g negatvmssrg -n $lb_name | grep loadBalancerFrontEnd)
split_line=( $line )
pip_name=${split_line[4]}

# now that we have the name of the public IP address, we can show the details to find the FQDN
#
# generic syntax:
# azure network public-ip show -g RESOURCE-GROUP-NAME -n PIP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network public-ip show -g negatvmssrg -n $pip_name | grep FQDN)
split_line=( $line )
FQDN=${split_line[3]}

# now that we have the FQDN, we can use ssh on port 50,000+i to connect to VM i (where i is 0-indexed)
#
# example to connct via ssh into VM "0":
ssh -p 50000 negat@$FQDN
```

<!--HONumber=Feb17_HO2-->


