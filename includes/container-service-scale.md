# <a name="scale-agent-nodes-in-a-container-service-cluster"></a>Skalieren von Agentknoten in einem Container Service-Cluster
Nach dem [Bereitstellen eines Azure Container Service-Clusters](../articles/container-service/dcos-swarm/container-service-deployment.md) müssen Sie eventuell die Anzahl von Agent-Knoten ändern. Möglicherweise benötigen Sie mehr Agents, damit Sie mehr Containeranwendungen oder -instanzen ausführen können. 

Sie können die Anzahl von Agent-Knoten in einem DC/OS-, Docker Swarm- oder Kubernetes-Cluster über das Azure-Portal oder mithilfe von Azure CLI 2.0 ändern. 

## <a name="scale-with-the-azure-portal"></a>Skalieren über das Azure-Portal

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Containerdiensten**, und klicken Sie dann auf den Containerdienst, den Sie ändern möchten.
2. Klicken Sie auf dem Blatt **Containerdienst** auf **Agents**.
3. Geben Sie unter **VM-Anzahl** die gewünschte Anzahl von Agent-Knoten ein.

    ![Skalieren eines Pools im Portal](./media/container-service-scale/container-service-scale-portal.png)

4. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.

## <a name="scale-with-the-azure-cli-20"></a>Skalieren mit Azure CLI 2.0

Stellen Sie sicher, dass Sie die aktuelle Version von Azure CLI 2.0 [installiert](/cli/azure/install-az-cli2) haben und an einem Azure-Konto (`az login`) angemeldet sind.

### <a name="see-the-current-agent-count"></a>Anzeigen der aktuellen Agent-Anzahl
Um die Anzahl der Agents anzuzeigen, die sich derzeit im Cluster befinden, führen Sie den Befehl `az acs show` aus. Damit wird die Clusterkonfiguration angezeigt. Der folgende Befehl zeigt z.B. die Konfiguration des Containerdiensts namens `containerservice-myACSName` in der Ressourcengruppe `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

Der Befehl gibt die Anzahl von Agents im `Count`-Wert unter `AgentPoolProfiles` zurück.

### <a name="use-the-az-acs-scale-command"></a>Verwenden des Befehls „az acs scale“
Um die Anzahl von Agent-Knoten zu ändern, führen Sie den `az acs scale`-Befehl aus, und geben Sie die **Ressourcengruppe**, den **Namen des Containerdiensts** und die gewünschte **neue Agent-Anzahl** an. Durch Verwendung einer kleineren oder größeren Zahl können Sie jeweils zentral herunter- bzw. hochskalieren.

Um z.B. die Anzahl von Agents im vorherigen Cluster auf 10 zu ändern, geben Sie folgenden Befehl ein:

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

Azure CLI 2.0 gibt eine JSON-Zeichenfolge zurück, die die neue Konfiguration des Containerdiensts darstellt, einschließlich der neuen Anzahl von Agents.

Um weitere Befehlsoptionen zu erhalten, führen Sie `az acs scale --help` aus.

## <a name="scaling-considerations"></a>Überlegungen zur Skalierung

* Die Anzahl von Agent-Knoten muss zwischen 1 und 100 (einschließlich) liegen. 

* Das Kernnutzungskontingent Ihres Systems kann die Anzahl von Agent-Knoten in einem Cluster begrenzen.

* Skalierungsvorgänge für Azure-Knoten werden auf eine Azure-VM-Skalierungsgruppe angewendet, die den Agent-Pool enthält. In einem DC/OS-Cluster werden nur Agent-Knoten im privaten Pool von den in diesem Artikel gezeigten Vorgängen skaliert.

* Je nach der im Cluster bereitgestellten Orchestrierung können Sie die Anzahl von Instanzen eines im Cluster ausgeführten Containers separat skalieren. Verwenden Sie in einem DC/OS-Cluster z.B. die [Marathon-Schnittstelle](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md), um die Anzahl von Instanzen einer Containeranwendung zu ändern.

* Zurzeit wird die automatische Skalierung von Agent-Knoten in einem Containerdienstcluster nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich [weitere Beispiele](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) für die Verwendung von Befehlen von Azure CLI 2.0 mit Azure Container Service an.
* Hier erfahren Sie mehr über [DC/OS-Agent-Pools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.

