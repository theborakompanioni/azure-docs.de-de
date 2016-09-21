<properties
   pageTitle="Ermöglichen des öffentlichen Zugriffs auf eine ACS-App | Microsoft Azure"
   description="Es wird beschrieben, wie Sie den öffentlichen Zugriff auf einen Azure Container Service ermöglichen."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Microservices, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="adegeo"/>

# Ermöglichen des öffentlichen Zugriffs auf eine Azure Container Service-Anwendung

Alle DC/OS-Container im [Pool mit den öffentlichen Agents](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) von ACS werden automatisch für das Internet verfügbar gemacht. Standardmäßig sind die Ports **80**, **443** und **8080** geöffnet, und alle (öffentlichen) Container, die über diese Ports lauschen, sind zugänglich. In diesem Artikel wird beschrieben, wie Sie mehr Ports für Ihre Anwendungen im Azure Container Service öffnen.

## Öffnen eines Ports (Portal) 

Zunächst müssen wir den gewünschten Port öffnen.

1. Melden Sie sich beim Portal an.
2. Suchen Sie nach der Ressourcengruppe, in der Sie den Azure Container Service bereitgestellt haben.
3. Wählen Sie das Agent-Lastenausgleichsmodul aus (der Name hat in der Regel das Format **XXXX-agent-lb-XXXX**).

    ![Azure Container Service-Lastenausgleich](media/container-service-dcos-agents/agent-load-balancer.png)

4. Klicken Sie auf **Tests** und dann auf **Hinzufügen**.

    ![Azure Container Service-Lastenausgleich – Tests](media/container-service-dcos-agents/add-probe.png)

5. Füllen Sie das Testformular aus, und klicken Sie auf **OK**.

    | Feld | Beschreibung |
    | ----- | ----------- |
    | Name | Ein beschreibender Name des Tests. |
    | Port | Der Port des zu testenden Containers. |
    | Pfad | (Im HTTP-Modus) Der relative Websitepfad zum Test. HTTPS wird nicht unterstützt. |
    | Intervall | Der Zeitraum zwischen Testversuchen in Sekunden. |
    | Fehlerhafter Schwellenwert | Anzahl von aufeinander folgenden Testversuchen, bevor der Container als fehlerhaft angesehen wird. | 
    

6. Wechseln Sie zurück zu den Eigenschaften des Agent-Lastenausgleichs, und klicken Sie auf **Lastenausgleichsregeln** und dann auf **Hinzufügen**.

    ![Azure Container Service-Lastenausgleich – Regeln](media/container-service-dcos-agents/add-balancer-rule.png)

7. Füllen Sie das Formular für den Lastenausgleich aus, und klicken Sie auf **OK**.

    | Feld | Beschreibung |
    | ----- | ----------- |
    | Name | Ein beschreibender Name für den Lastenausgleich. |
    | Port | Der öffentliche Port für eingehenden Datenverkehr. |
    | Back-End-Port | Der interne öffentliche Port des Containers, an den Datenverkehr geleitet wird. |
    | Back-End-Pool | Die Container in diesem Pool sind das Ziel für diesen Lastenausgleich. |
    | Test | Der Test für die Ermittlung, ob ein Ziel im **Back-End-Pool** fehlerfrei ist. |
    | Sitzungspersistenz | Bestimmt, wie Datenverkehr von einem Client für die Dauer der Sitzung behandelt werden soll.<br><br>**Keine**: Aufeinander folgende Anforderungen von demselben Client können von jedem Container verarbeitet werden.<br>**Client-IP**: Aufeinander folgende Anforderungen von derselben Client-IP werden von demselben Container verarbeitet.<br>**Client-IP und Protokoll**: Aufeinander folgende Anforderungen von derselben Kombination aus Client-IP und Protokoll werden von demselben Container verarbeitet. |
    | Leerlauftimeout | (Nur TCP) Der Zeitraum zur Beibehaltung des geöffneten Zustands eines TCP/HTTP-Clients in Minuten, ohne dass *Keep-Alive*-Nachrichten verwendet werden. |

## Hinzufügen einer Sicherheitsregel (Portal)

Als Nächstes müssen wir eine Sicherheitsregel hinzufügen, mit der Datenverkehr von unserem geöffneten Port über die Firewall geleitet wird.

1. Melden Sie sich beim Portal an.
2. Suchen Sie nach der Ressourcengruppe, in der Sie den Azure Container Service bereitgestellt haben.
3. Wählen Sie **öffentliche** Agent-Netzwerksicherheitsgruppe (der Name hat in der Regel das Format **XXXX-agent-public-nsg-XXXX**).

    ![Azure Container Service-Netzwerksicherheitsgruppe](media/container-service-dcos-agents/agent-nsg.png)

4. Wählen Sie **Eingangssicherheitsregeln** und dann **Hinzufügen**.

    ![Azure Container Service-Netzwerksicherheitsgruppe – Regeln](media/container-service-dcos-agents/add-firewall-rule.png)

5. Füllen Sie die Firewallregel aus, um den öffentlichen Port zuzulassen, und klicken Sie auf **OK**.

    | Feld | Beschreibung |
    | ----- | ----------- |
    | Name | Ein beschreibender Name der Firewallregel. |
    | Priority | Prioritätsrang für die Regel. Je niedriger die Nummer ist, desto höher ist die Priorität. |
    | Quelle | Schränkt den IP-Adressbereich für eingehenden Datenverkehr ein, der mit dieser Regel zugelassen oder abgelehnt wird. Verwenden Sie **Alle**, um keine Einschränkung anzugeben. |
    | Dienst | Wählen Sie einen Satz mit vordefinierten Diensten aus, für die diese Sicherheitsregel gilt. Verwenden Sie andernfalls **Benutzerdefiniert**, um einen eigenen Satz zu erstellen. |
    | Protokoll | Schränkt den Datenverkehr basierend auf **TCP** oder **UDP** ein. Verwenden Sie die Option **Alle**, um keine Einschränkung anzugeben. |
    | Portbereich | Wenn **Dienst** auf **Benutzerdefiniert** festgelegt ist, wird hiermit der Portbereich angegeben, auf den sich diese Regel auswirkt. Sie können einen einzelnen Port, z.B. **80**, oder einen Bereich wie **1024 - 1500** verwenden. |
    | Aktion | Dient zum Zulassen oder Verweigern von Datenverkehr, der die Kriterien erfüllt. |

## Nächste Schritte

Informieren Sie sich über den Unterschied zwischen [öffentlichen und privaten DC/OS-Agents](container-service-dcos-agents.md).

Erhalten Sie weitere Informationen zum [Verwalten Ihrer DC/OS-Container](container-service-mesos-marathon-ui.md).

<!---HONumber=AcomDC_0907_2016-->