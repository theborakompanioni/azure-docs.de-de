---
title: "SSH-Unterstützung bei Azure App Service-Web-Apps unter Linux | Microsoft-Dokumentation"
description: "Erfahren Sie etwas über die Verwendung von SSH mit Azure-Web-Apps unter Linux."
keywords: Azure App Service, Web-App, Linux, OSS
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 6da663ea282e09b01ce380827fa7e31505712516
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="ssh-support-for-azure-web-app-on-linux"></a>SSH-Unterstützung für Azure-Web-Apps unter Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Übersicht

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) ist ein kryptografisches Netzwerkprotokoll für die sichere Verwendung von Netzwerkdiensten. Es wird am häufigsten für Remoteanmeldungen bei einem System mithilfe einer Befehlszeile und für die Remoteausführung von Verwaltungsbefehlen verwendet.

Web-Apps unter Linux unterstützen SSH in den App-Container für jedes der integrierten Docker-Images, die für den Laufzeitstapel neuer Web-Apps verwendet werden. 

![Laufzeitstapel](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Sie können SSH auch mit Ihren benutzerdefinierten Docker-Images verwenden, indem Sie den SSH-Server als Teil des Images einfügen und wie in diesem Thema beschrieben konfigurieren.



## <a name="making-a-client-connection"></a>Herstellen einer Clientverbindung

Um eine SSH-Clientverbindung herzustellen, muss die Hauptwebsite gestartet werden. 

Fügen Sie den SCM-Endpunkt (Source Control Management) für Ihre Web-App im folgenden Format in Ihren Browser ein:

        https://<your sitename>.scm.azurewebsites.net/webssh/host

Wenn Sie nicht bereits authentifiziert sind, müssen Sie sich mit Ihrem Azure-Abonnement für die Verbindung authentifizieren.

![SSH-Verbindung](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a>SSH-Unterstützung bei benutzerdefinierten Docker-Images

Damit ein benutzerdefiniertes Docker-Image die SSH-Kommunikation zwischen dem Container und dem Client unterstützt, führen Sie im Azure-Portal die folgenden Schritte für Ihr Docker-Image aus. 

Diese Schritte werden im Azure App Service-Repository [hier](https://github.com/Azure-App-Service/node/tree/master/4.4.7-1) als Beispiel gezeigt.

1. Fügen Sie die `openssh-server`-Installation in die [`RUN`-Anweisung](https://docs.docker.com/engine/reference/builder/#run) in der Dockerfile-Datei für Ihr Image ein, und legen Sie als Kennwort für das Root-Konto `"Docker!"` fest. 

    > [!NOTE] 
    > Diese Konfiguration erlaubt keine externen Verbindungen zum Container. Der SSH-Zugriff ist nur über die Kudu-/SCM-Website möglich, die über die Veröffentlichungsanmeldeinformationen authentifiziert wird.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. Fügen Sie der Dockerfile-Datei eine [`COPY`-Anweisung](https://docs.docker.com/engine/reference/builder/#copy) hinzu, mit der eine [sshd_config](http://man.openbsd.org/sshd_config)-Datei in das Verzeichnis */etc/ssh/* kopiert wird. Die Konfigurationsdatei sollte auf [dieser](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config) Datei „sshd_config“ im Azure-App-Service-GitHub-Repository basieren.

    > [!NOTE] 
    > Die Datei *sshd_config* muss Folgendes enthalten, da andernfalls ein Verbindungsfehler auftritt: 
    > * `Ciphers` muss mindestens einen der folgenden Werte enthalten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` muss mindestens einen der folgenden Werte enthalten: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. Fügen Sie Port 2222 in die [`EXPOSE`-Anweisung](https://docs.docker.com/engine/reference/builder/#expose) für die Dockerfile-Datei ein. Selbst wenn das Root-Kennwort bekannt ist, kann auf Port 2222 nicht aus dem Internet zugegriffen werden. Es handelt sich um einen internen Port, auf den nur von Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugegriffen werden kann.

    ```docker
    EXPOSE 2222 80
    ```

4. Stellen Sie sicher, dass der SSH-Dienst gestartet wird. Im [diesem](https://github.com/Azure-App-Service/node/blob/master/6.9.3-1/init_container.sh) Beispiel wird ein Shellskript im Verzeichnis */bin* verwendet.

    ```bash
    #!/bin/bash
    service ssh start
    ```

    Die Dockerfile-Datei verwendet die [`CMD`-Anweisung](https://docs.docker.com/engine/reference/builder/#cmd) zum Ausführen des Skripts.

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links finden Sie weitere Informationen zu Web-Apps unter Linux. In [unserem Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.

* [Erstellen von Apps in Azure-Web-Apps unter Linux](app-service-linux-how-to-create-web-app.md)
* [Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps unter Linux](app-service-linux-using-custom-docker-image.md)
* [Verwenden der PM2-Konfiguration für Node.js in Azure-Web-Apps unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwenden von .NET Core in Azure-Web-Apps unter Linux](app-service-linux-using-dotnetcore.md)
* [Verwenden von Ruby in Azure-Web-Apps unter Linux](app-service-linux-ruby-get-started.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)


