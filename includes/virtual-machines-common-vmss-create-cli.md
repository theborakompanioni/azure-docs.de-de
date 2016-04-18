Skalierungsgruppen virtueller Computer ermöglichen es Ihnen, mehrere VMs als Satz zu verwalten. Auf hoher Ebene haben Skalierungsgruppen die folgenden Vor- und Nachteile:

Vorteile:

1. Hohe Verfügbarkeit. Jede Skalierungsgruppe platziert ihre VMs in eine Verfügbarkeitsgruppe mit fünf Fehlerdomänen (FD) und fünf Updatedomänen (UD), um die Verfügbarkeit sicherzustellen (weitere Informationen zu FDs und UDs finden Sie unter [VM-Verfügbarkeit](../articles/virtual-machines/virtual-machines-linux-manage-availability.md)).
2. Einfache Integration mit dem Azure Load Balancer und App Gateway
3. Leichtere Integration mit Azure Autoscale
4. Vereinfachte Bereitstellung, Verwaltung und Bereinigung von virtuellen Computern
5. Unterstützung häufiger Windows- und Linux-Varianten sowie benutzerdefinierter Images

Nachteile:

1. Kann in einer Skalierungsgruppe keine Datenträger an VM-Instanzen anfügen. Muss stattdessen Azure Blob Storage, Azure Files Storage, Azure Table Storage oder andere Speicherlösungen verwenden.

## Schnellerfassung mithilfe der Azure-Befehlszeilenschnittstelle

Falls Sie dies nicht bereits getan haben, können Sie ein [Azure-Abonnement als kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) und die [Befehlszeilenschnittstelle](../articles/xplat-cli-install.md) erhalten, die [mit Ihrem Azure-Konto verbunden ist](../articles/xplat-cli-connect.md). Sobald Sie dies getan haben, können Sie die folgenden Befehle ausführen, um eine Schnellerfassung für eine VM-Skalierungsgruppe durchzuführen:

```bash
# make sure we are in resource manager mode 
azure config mode arm

# quick-create a VM scale set
#
# generic syntax:
# azure vmss quick-create -n VMSS-NAME -g RESOURCE-GROUP-NAME -l LOCATION -u USERNAME -p PASSWORD -C INSTANCE-COUNT -Q IMAGE-URN
#
# example:
azure vmss quick-create -n negatvmss -g negatvmssrg -l westus -u negat -p P4$$w0rd -C 5 -Q Canonical:UbuntuServer:14.04.4-LTS:latest
```

Falls Sie den Standort oder den URN (uniform resource name) des Images anpassen möchten, sehen Sie sich bitte die Befehle `azure location list` und `azure vm image {list-publishers|list-offers|list-skus|list|show}` an.

Wenn der Befehl zurückgegeben wurde, wurde die Skalierungsgruppe erstellt. Zu dieser Skalierungsgruppe gehört ein Lastenausgleich mit NAT-Regeln, das den Port 50.000+i des Lastenausgleichs dem Port 22 der VM i zuordnet. Daher werden wir, sobald wir den FQDN des Lastenausgleichs herausgefunden haben, in der Lage sein, mithilfe des SSH-Protokolls eine sichere Verbindung zu unseren VMs herzustellen:

```bash
# list load balancers in the resource group we created
#
# generic syntax:
# azure network lb list -g RESOURCE-GROUP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb list -g nsgvmsrg | grep nsgvmssrg)
split_line=( $line )
lb_name=${split_line[1]}

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is associated with it
#
# generic syntax:
# azure network lb show -g RESOURCE-GROUP-NAME -n LB-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb show -g nsgvmssrg -n $lb_name | grep loadBalancerFrontEnd)
split_line=( $line )
pip_name=${split_line[4]}

# now that we have the name of the public IP address, we can show the details to find the FQDN
#
# generic syntax:
# azure network public-ip show -g RESOURCE-GROUP-NAME -n PIP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network public-ip show -g nsgvmssrg -n $pip_name | grep FQDN)
split_line=( $line )
FQDN=${split_line[3]}

# now that we have the FQDN, we can ssh on port 50,000+i to ssh into VM i (where i is 0-indexed)
#
# example to ssh into VM "0":
ssh -p 50000 $FQDN
```

## Nächste Schritte

Weitere allgemeine Informationen finden Sie unter der [Hauptseite für VM-Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Weitere Dokumenten finden Sie unter der [Hauptseite für VM-Skalierungsgruppen](https://azure.microsoft.com/documentation/services/virtual-machines-scale-sets/).

Um Beispiele für Azure Resource Manager-Vorlagen, die VM-Skalierungsgruppen verwenden, zu finden, suchen Sie nach „vmss“ im [Github-Repository für Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates).

<!---HONumber=AcomDC_0406_2016-->