<properties
	pageTitle="Linux-Knoten in Azure Batch-Pools | Microsoft Azure"
	description="Erfahren Sie, wie parallele Compute-Workloads auf Pools auf virtuellen Linux-Computern in Azure Batch verarbeitet werden."
	services="batch"
	documentationCenter="python"
	authors="mmacy"
	manager="timlt"
	editor="" />

<tags
	ms.service="batch"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="na"
	ms.date="04/19/2016"
	ms.author="marsma" />

# Bereitstellen von Linux-Computeknoten in Azure Batch-Pools

Azure Batch ermöglicht Ihnen das parallele Ausführen von Computeworkloads auf virtuellen Linux- und Windows-Computern. Dieser Artikel enthält Informationen zum Erstellen von Pools von Linux-Computeknoten im Batch-Dienst mit den Clientbibliotheken [Batch Python][py_batch_package] sowie [Batch .NET][api_net].

> [AZURE.NOTE] Die Unterstützung von Linux in Batch befindet sich derzeit in der Vorschauphase. Einige Aspekte des hier beschriebenen Features können sich vor der allgemeinen Verfügbarkeit ändern. [Anwendungspakete](batch-application-packages.md) und [Tasks mit mehreren Instanzen](batch-mpi.md) werden derzeit auf Linux-Computeknoten **nicht unterstützt**.

## Konfiguration des virtuellen Computers

Wenn Sie einen Pool von Computeknoten in Batch erstellen, stehen Ihnen zwei Optionen zur Auswahl der Knotengröße und des Betriebssystems zur Verfügung: die **Clouddienstkonfiguration** und die **Konfiguration des virtuellen Computers**.

Die **Clouddienstkonfiguration** stellt *nur* Windows-Computeknoten bereit. Die verfügbaren Größen für Computeknoten sind unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md) aufgelistet. Die verfügbaren Betriebssysteme sind unter [Azure-Gastbetriebssystemversionen und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md) aufgeführt. Beim Erstellen eines Pools, der Cloud Services-Knoten enthält, müssen Sie nur die Knotengröße und dessen „Betriebssystemfamilie“ angeben, die Sie in diesen Artikeln finden. Beim Erstellen von Pools von Windows-Computeknoten wird Cloud Services am häufigsten verwendet.

Die **Konfiguration des virtuellen Computers** stellt sowohl Linux- als auch Windows-Images für Computeknoten bereit. Die verfügbaren Größen für Computeknoten sind unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) und [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) aufgelistet. Beim Erstellen eines Pools mit Knoten mit der Konfiguration des virtuellen Computers müssen Sie nicht nur die Knotengröße angeben, sondern auch die **VM-Imagereferenz** und die Batch **Knoten-Agent-SKU**, die auf den Knoten installiert werden soll.

### VM-Imagereferenz

Der Batch-Dienst nutzt im Hintergrund [Skalierungsgruppen für virtuelle Computer](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) für die Bereitstellung von Linux-Computeknoten, und die Betriebssystem-Images für diese virtuellen Computer werden vom [Marketplace für virtuelle Computer][vm_marketplace] bereitgestellt. Wenn Sie eine VM-Imagereferenz konfigurieren, müssen Sie die Eigenschaften eines Marketplace-VM-Images festlegen. Die folgenden Eigenschaften sind erforderlich, wenn Sie eine VM-Imagereferenz erstellen:

| **Imagereferenzeigenschaften** | **Beispiel** |
| ----------------- | ------------------------ |
| Herausgeber | Canonical |
| Angebot | UbuntuServer |
| SKU | 14\.04.4-LTS |
| Version | neueste |

> [AZURE.TIP] Weitere Informationen zu diesen Eigenschaften und zur Auflistung von Marketplace-Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Beachten Sie, dass nicht alle Marketplace-Images derzeit mit Batch kompatibel sind. Weitere Informationen dazu finden Sie unter [Knoten-Agent-SKU](#node-agent-sku) (s.u.).

### Knoten-Agent-SKU

Der Batch-Knoten-Agent ist ein Programm, das auf jedem Knoten im Pool ausgeführt wird, und stellt die Befehls- und Steuerungsschnittstelle zwischen dem Knoten und dem Batch-Dienst dar. Es gibt verschiedene Implementierungen des Knoten-Agents (SKUs) für verschiedene Betriebssysteme. Insbesondere beim Erstellen einer Konfiguration für einen virtuellen Computer legen Sie zuerst die VM-Imagereferenz fest und anschließend den auf Knoten-Agent, der auf dem Image installiert werden soll. In der Regel ist jede Knoten-Agent-SKU mit mehreren VM-Images kompatibel. Hier sind einige Beispiele für Knoten-Agent-SKUs:

* batch.node.ubuntu 14.04
* batch.node.centos 7
* batch.node.windows amd64

> [AZURE.IMPORTANT] Nicht alle im Marketplace verfügbaren VM-Images sind mit den derzeit verfügbaren Batch-Knoten-Agents kompatibel. Sie müssen die Batch-SDKs verwenden, um die verfügbaren Knoten-Agent-SKUs und die VM-Images, mit denen sie kompatibel sind, aufzulisten. Weitere Informationen finden Sie unten in der [Liste von VM-Images](#list-of-virtual-machine-images).

## Erstellen eines Linux-Pools: Batch Python

Der folgende Codeausschnitt zeigt die Erstellung eines Pools von Ubuntu Server-Computeknoten mithilfe der [Microsoft Azure Batch-Clientbibliothek für Python][py_batch_package]. Referenzdokumentation für das Batch-Python-Modul finden Sie unter[azure.batch package ][py_batch_docs] unter „Read the Docs“ (Dokumente lesen).

In diesem Ausschnitt erstellen wir [ImageReference][py_imagereference] explizit und legen jede der Eigenschaften (Herausgeber, Angebot, SKU, Version) fest. Im Produktionscode empfehlen wir Ihnen jedoch die Verwendung der Methode [list\_node\_agent\_skus][py_list_skus], um die verfügbaren Kombinationen aus Image und Knoten-Agent-SKU zur Laufzeit zu bestimmen und eine Auswahl zu treffen.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Wie bereits erwähnt, wird empfohlen, [ImageReference][py_imagereference] nicht explizit zu erstellen, sondern die Methode [list\_node\_agent\_skus][py_list_skus] zu verwenden, um aus den derzeit unterstützten Kombinationen aus Knoten-Agent und Marketplace-Image eine Auswahl zu treffen. Der folgende Python-Codeausschnitt veranschaulicht die Verwendung dieser Methode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## Erstellen eines Linux-Pools: Batch .NET

Der folgende Codeausschnitt zeigt die Erstellung eines Pools von Ubuntu Server-Computeknoten mithilfe der [Batch .NET][nuget_batch_net]-Clientbibliothek. Auf MSDN finden Sie die [Batch .NET-Referenzdokumentation][api_net].

Im folgenden Codeausschnitt wird die Methode [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] eingesetzt, um aus der Liste der derzeit unterstützten Kombinationen von Marketplace-Image und Knoten-Agent-SKU eine Auswahl zu treffen. Diese Technik wird empfohlen, da sich die Liste der unterstützten Kombinationen von Zeit zu Zeit ändert (in der Regel werden weitere unterstützte Kombinationen hinzugefügt).

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Im oben aufgeführten Codeausschnitt wird zwar die Methode [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] eingesetzt, um unterstützte Kombinationen aus Image und Knoten-Agent-SKU aufzulisten und eine Auswahl zu treffen (empfohlene Vorgehensweise), Sie können jedoch [ImageReferenz][net_imagereference] auch explizit konfigurieren:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## Liste von VM-Images

Die folgende Tabelle gibt an, welche Marketplace-VM-Images **zum Zeitpunkt der Erstellung dieses Dokuments** mit den verfügbaren Batch-Knoten-Agents kompatibel sind. Dabei ist zu beachten, dass diese Liste nicht final ist, da Images und Knoten-Agents jederzeit hinzugefügt oder entfernt werden können. Es wird empfohlen, dass für die Batch-Anwendungen und -Dienste immer [list\_node\_agent\_skus][py_list_skus] (Python) und [ListNodeAgentSkus][net_list_skus] (Batch .NET) verwendet wird, um die aktuell verfügbaren SKUs zu bestimmen und eine Auswahl zu treffen.

> [AZURE.WARNING] Die unten aufgeführte Liste kann jederzeit geändert werden. Verwenden Sie immer die in den Batch-APIs verfügbaren Methoden **zum Auflisten von Knoten-Agent-SKUs**, um die kompatiblen virtuellen Computer und Knoten-Agent-SKUs aufzulisten und eine Auswahl zu treffen, wenn Sie Batch-Aufträge ausführen.

| **Herausgeber** | **Angebot** | **Image-SKU** | **Version** | **Knoten-Agent-SKU-ID** |
| ------- | ------- | ------- | ------- | ------- |
| Canonical | UbuntuServer | 14\.04.0-LTS | neueste | batch.node.ubuntu 14.04 |
| Canonical | UbuntuServer | 14\.04.1-LTS | neueste | batch.node.ubuntu 14.04 |
| Canonical | UbuntuServer | 14\.04.2-LTS | neueste | batch.node.ubuntu 14.04 |
| Canonical | UbuntuServer | 14\.04.3-LTS | neueste | batch.node.ubuntu 14.04 |
| Canonical | UbuntuServer | 14\.04.4-LTS | neueste | batch.node.ubuntu 14.04 |
| Canonical | UbuntuServer | 15\.10 | neueste | batch.node.debian 8 |
| Credativ | Debian | 8 | neueste | batch.node.debian 8 |
| OpenLogic | CentOS | 7,0 | neueste | batch.node.centos 7 |
| OpenLogic | CentOS | 7,1 | neueste | batch.node.centos 7 |
| OpenLogic | CentOS | 7,2 | neueste | batch.node.centos 7 |
| Oracle | Oracle-Linux-7 | OL70 | neueste | batch.node.centos 7 |
| SUSE | SLES | 12 | neueste | batch.node.opensuse 42.1 |
| SUSE | SLES | 12-SP1 | neueste | batch.node.opensuse 42.1 |
| SUSE | SLES-HPC | 12 | neueste | batch.node.opensuse 42.1 |
| SUSE | openSUSE | 13\.2 | neueste | batch.node.opensuse 13.2 |
| SUSE | openSUSE-Leap | 42\.1 | neueste | batch.node.opensuse 42.1 |
| MicrosoftWindowsServer | Windows Server | 2008-R2-SP1 | neueste | batch.node.windows amd64 |
| MicrosoftWindowsServer | Windows Server | 2012-Datacenter | neueste | batch.node.windows amd64 |
| MicrosoftWindowsServer | Windows Server | 2012-R2-Datacenter | neueste | batch.node.windows amd64 |
| MicrosoftWindowsServer | Windows Server | Windows-Server-Technical-Preview | neueste | batch.node.windows amd64 |

## Herstellen einer Verbindung mit Linux-Knoten

Während der Entwicklung oder bei der Fehlerbehebung ist es unter Umständen erforderlich, sich bei den Knoten im Pool anzumelden. Im Gegensatz zu Windows-Computeknoten können Sie das Remotedesktopprotokoll (RDP) für die Herstellung einer Verbindung mit Linux-Knoten nicht verwenden. Stattdessen ermöglicht der Batch-Dienst den SSH-Zugriff auf jeden Knoten zum Herstellen einer Remoteverbindung.

Der folgende Python-Codeausschnitt erstellt auf jedem Knoten in einem Pool einen Benutzer (was für die Remoteverbindung erforderlich ist) und druckt daraufhin die SSH-Verbindunginformationen für jeden Knoten.

```python
import getpass

# Specify the username and prompt for a password
username = "linuxuser"
password = getpass.getpass()

# Create the user that will be added to each node
# in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = client.compute_node.get_remote_login_settings(pool_id,
                                                          node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Im Folgenden sehen Sie eine Beispielausgabe für den o.g. Code für einen Pool mit vier Linux-Knoten:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Beachten Sie, dass Sie anstelle eines Kennworts einen öffentlichen SSH-Schlüssel festlegen können, wenn Sie einen Benutzer auf einem Knoten erstellen. In der Python-SDK erfolgt dies mithilfe des Parameters **ssh\_public\_key** auf [ComputeNodeUser][py_computenodeuser]. In .NET wird dafür die Eigenschaft [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key] verwendet.

## Preise

Azure Batch basiert auf der Technologie von Azure Cloud Services und von Azure Virtual Machines. Der Batch-Dienst wird kostenfrei angeboten, d.h. nur die Computeressourcen, die von Ihren Batch-Lösungen verbraucht werden, werden abgerechnet. Bei der Auswahl der **Clouddienstkonfiguration** erfolgt die Abrechnung auf Grundlage der Preisstruktur, die Sie unter [Cloud Services – Preise][cloud_services_pricing] finden. Die Abrechnung für die **Konfiguration des virtuellen Computers** erfolgt ausgehend von der Preisstruktur unter [Virtual Machines – Preise][vm_pricing].

## Nächste Schritte

### Batch Python-Codebeispiele

In den [Python-Codebeispielen][github_samples_py] im Repository [azure-batch-samples][github_samples] auf GitHub finden Sie verschiedene Skripts, aus denen ersichtlich ist, wie häufige Batchvorgänge wie die Erstellung von Pools, Jobs und Tasks usw. ausgeführt werden. Die zu den Python-Beispielen gehörige [README][github_py_readme]-Datei beinhaltet Details zur Installation der erforderlichen Pakete.

### Batch-Forum

Das [Azure Batch-Forum][forum] in MSDN eignet sich hervorragend, um Informationen zu Batch zu erhalten und Fragen zu diesem Dienst zu stellen. Nutzen Sie das Forum, um hilfreiche Beiträge zu lesen, und posten Sie selber Fragen, die während der Erstellung Ihrer Batch-Lösungen auftreten.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/de-DE/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/

[1]: ./media/batch-application-packages/app_pkg_01.png "Übersichtsdiagramm für Anwendungspakete"

<!---HONumber=AcomDC_0427_2016-->