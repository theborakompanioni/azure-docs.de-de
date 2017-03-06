---
title: Interagieren mit Service Fabric-Clustern mithilfe der Befehlszeilenschnittstelle | Microsoft Docs
description: 'Gewusst wie: Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle'
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: c3ec8ff3-3b78-420c-a7ea-0c5e443fb10e
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/02/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 615e7ea84aae45f384edb671a28e4ff98b4ade3a
ms.openlocfilehash: d61b7a9c8199b15c8bb24e7146ea93a2f67fb0a7
ms.lasthandoff: 02/17/2017


---
# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle
Sie können die Azure-Befehlszeilenschnittstelle unter Linux verwenden, um von einem Linux-Computer aus mit einem Service Fabric-Cluster zu interagieren.

Laden Sie zunächst die neueste Version der Befehlszeilenschnittstelle aus dem Git-Repository herunter, und richten Sie sie in Ihrem Pfad ein. Verwenden Sie hierzu die folgenden Befehle:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Bei jedem unterstützten Befehl können Sie den Namen des Befehls eingeben, um die Hilfe für den jeweiligen Befehl aufzurufen. Für die Befehle wird die automatische Vervollständigung unterstützt. Mit dem folgenden Befehl erhalten Sie beispielsweise Hilfe zu allen Anwendungsbefehlen: 

```sh
 azure servicefabric application 
```

Sie können die Hilfe auch nach einem bestimmten Befehl filtern, wie im folgenden Beispiel zu sehen:

```sh
 azure servicefabric application  create
```

Führen Sie die folgenden Befehle aus, um die automatische Vervollständigung in der Befehlszeilenschnittstelle zu aktivieren:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Die folgenden Befehle stellen eine Verbindung mit dem Cluster her und zeigen die im Cluster enthaltenen Knoten an:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Wenn Sie benannte Parameter verwenden und deren Funktion ermitteln möchten, können Sie „--help“ an einen Befehl anhängen. Beispiel:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Wenn Sie über einen Computer, **der nicht dem Cluster angehört**, eine Verbindung mit einem Cluster mit mehreren Computern herstellen, verwenden Sie den folgenden Befehl:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Ersetzen Sie das PublicIPorFQDN-Tag durch die tatsächliche IP-Adresse oder den FQDN. Wenn Sie über einen Computer, **der dem Cluster angehört**, eine Verbindung mit einem Cluster mit mehreren Computern herstellen, verwenden Sie den folgenden Befehl:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Für die Interaktion mit Ihrem Linux-basierten Service Fabric-Cluster, den Sie über das Azure-Portal erstellt haben, können Sie PowerShell oder die Befehlszeilenschnittstelle verwenden. 

> [!WARNING]
> Da diese Cluster nicht sicher sind, setzen Sie Ihre One-Box unter Umständen einem Risiko aus, wenn Sie die öffentliche IP-Adresse dem Clustermanifest hinzufügen.

## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Herstellen einer Verbindung mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle
Mit den folgenden Befehlen der Azure-Befehlszeilenschnittstelle können Sie eine Verbindung mit einem sicheren Cluster herstellen. Die Details des Zertifikats müssen mit einem Zertifikat auf den Clusterknoten übereinstimmen.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

Falls Ihr Zertifikat über Zertifizierungsstellen (Certificate Authorities, CAs) verfügt, müssen Sie den Parameter „--ca-cert-path“ hinzufügen, wie im folgenden Beispiel zu sehen: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Mehrere Zertifizierungsstellen können als kommagetrennte Liste angegeben werden.

Falls der allgemeine Name im Zertifikat nicht dem Verbindungsendpunkt entspricht, können Sie die Überprüfung mithilfe des Parameters `--strict-ssl-false` umgehen, wie im folgenden Befehl zu sehen: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

Wenn Sie die Überprüfung der Zertifizierungsstelle überspringen möchten, können Sie den Parameter „--reject-unauthorized-false“ hinzufügen, wie im folgenden Befehl zu sehen: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

Nach dem Herstellen der Verbindung können Sie durch Ausführen anderer Befehle über die Befehlszeilenschnittstelle mit dem Cluster interagieren. 

## <a name="deploying-your-service-fabric-application"></a>Bereitstellen einer Service Fabric-Anwendung
Führen Sie die folgenden Befehle aus, um die Service Fabric-Anwendung zu kopieren, zu registrieren und zu starten:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Upgraden Ihrer Anwendung
Der Prozess ähnelt dem [Prozess unter Windows](service-fabric-application-upgrade-tutorial-powershell.md).

Erstellen, kopieren und registrieren Sie Ihre Anwendung über das Projektstammverzeichnis. Wenn der Name Ihrer Anwendungsinstanz „fabric:/MySFApp“ und der Typ „MySFApp“ lautet, sehen die entsprechenden Befehle wie folgt aus:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Ändern Sie Ihre Anwendung, und erstellen Sie den geänderten Dienst neu.  Aktualisieren Sie die Manifestdatei des geänderten Diensts („ServiceManifest.xml“) mit den aktualisierten Versionen für den Dienst (und ggf. mit aktualisiertem Code, aktualisierter Konfiguration oder aktualisierten Daten). Passen Sie auch das Manifest der Anwendung („ApplicationManifest.xml“) mit der aktualisierten Versionsnummer für die Anwendung und dem geänderten Dienst an.  Führen Sie anschließend die folgenden Befehle aus, um Ihre aktualisierte Anwendung zu kopieren und registrieren:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Nun können Sie das Anwendungsupgrade mithilfe des folgenden Befehls starten:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Das Anwendungsupgrade kann daraufhin mit SFX überwacht werden. Die Aktualisierung der Anwendung dauert nur wenige Minuten.  Sie können auch eine aktualisierte Anwendung mit einem Fehler ausprobieren und die automatische Rollbackfunktion in Service Fabric testen.

## <a name="converting-from-pfx-to-pem-and-vice-versa"></a>Konvertieren von PFX zu PEM und umgekehrt

Unter Umständen müssen Sie auf Ihrem lokalen Computer (unter Windows oder Linux) ein Zertifikat installieren, um auf sichere Cluster zuzugreifen, die sich möglicherweise in einer anderen Umgebung befinden. Wenn Sie also beispielsweise über einen Windows-Computer auf einen geschützten Linux-Cluster zugreifen, müssen Sie Ihr Zertifikat ggf. von PFX in PEM konvertieren (und umgekehrt). 

Mit dem folgenden Befehl konvertieren Sie eine PEM-Datei in eine PFX-Datei:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

Mit dem folgenden Befehl konvertieren Sie eine PFX-Datei in eine PEM-Datei:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Ausführliche Informationen finden Sie in der [OpenSSL-Dokumentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html).

<a id="troubleshooting"></a>
## <a name="troubleshooting"></a>Problembehandlung
### <a name="copying-of-the-application-package-does-not-succeed"></a>Das Anwendungspaket kann nicht kopiert werden.
Überprüfen Sie, ob `openssh` installiert ist. Diese Komponente ist unter Ubuntu Desktop standardmäßig nicht installiert. Installieren Sie sie mithilfe des folgenden Befehls:

```
 sudo apt-get install openssh-server openssh-client**
```

Sollte sich das Problem dadurch nicht beheben lassen, deaktivieren Sie PAM für ssh, indem Sie mithilfe der folgenden Befehle die Datei **sshd_config** ändern:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Tritt das Problem immer noch auf, führen Sie die folgenden Befehle aus, um die Anzahl von ssh-Sitzungen zu erhöhen:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Da bei der ssh-Authentifizierung noch keine Schlüssel unterstützt werden (weil die Plattform ssh verwendet, um Pakete zu kopieren), muss stattdessen die Kennwortauthentifizierung verwendet werden.



## <a name="next-steps"></a>Nächste Schritte
Richten Sie die Entwicklungsumgebung ein, und stellen Sie eine Service Fabric-Anwendung in einem Linux-Cluster bereit.


