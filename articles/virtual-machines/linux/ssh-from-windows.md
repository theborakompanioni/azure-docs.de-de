---
title: "Verwenden von SSH-Schlüsseln mit Windows für virtuelle Linux-Computer | Microsoft Docs"
description: "Erfahren Sie, wie Sie SSH-Schlüssel auf einem Windows-Computer erstellen und verwenden, um Verbindungen mit virtuellen Linux-Computern in Azure herzustellen."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: rasquill
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: 7f572adf499dd1fcd0db19500c9049af1e31cdea
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Gewusst wie: Verwenden von SSH-Schlüsseln mit Windows in Azure
> [!div class="op_single_selector"]
> * [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

Wenn Sie eine Verbindung mit virtuellen Linux-Computern in Azure herstellen, empfiehlt sich eine [Verschlüsselung mit öffentlichem Schlüssel](https://wikipedia.org/wiki/Public-key_cryptography), um die Sicherheit der Anmeldung bei Ihrem virtuellen Computer zu erhöhen. Bei diesem Verfahren erfolgt die Authentifizierung nicht per Benutzername und Kennwort. Stattdessen werden mithilfe des SSH-Befehls (Secure Shell) ein öffentlicher und ein privater Schlüssel ausgetauscht. Kennwörter sind anfällig für Brute-Force-Angriffe. Das gilt insbesondere bei virtuellen Computern mit Internetzugriff (also beispielsweise für Webserver). Dieser Artikel enthält eine Übersicht über SSH-Schlüssel und zeigt, wie Sie die entsprechenden Schlüssel auf einem Windows-Computer generieren.

## <a name="overview-of-ssh-and-keys"></a>Übersicht über SSH und Schlüssel
Mithilfe eines öffentlichen und eines privaten Schlüssels können Sie sich sicher bei Ihrem virtuellen Linux-Computer anmelden:

* Der **öffentliche Schlüssel** wird auf Ihrem virtuellen Linux-Computer oder in einem anderen Dienst platziert, den Sie für die Verschlüsselung mit öffentlichem Schlüssel verwenden möchten.
* Der **private Schlüssel** wird Ihrem virtuellen Linux-Computer bei der Anmeldung präsentiert, um Ihre Identität zu bestätigen. Bewahren Sie diesen privaten Schlüssel sicher auf. Geben Sie ihn nicht weiter.

Der öffentliche und der private Schlüssel können für mehrere virtuelle Computer und Dienste verwendet werden. Sie benötigen nicht für jeden virtuellen Computer oder Dienst, auf den Sie zugreifen möchten, ein eigenes Schlüsselpaar. Eine ausführliche Übersicht finden Sie unter [Public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) (Verschlüsselung mit öffentlichem Schlüssel).

SSH ist ein Protokoll für verschlüsselte Verbindungen, das die sichere Anmeldung über ungesicherte Verbindungen ermöglicht. Es ist das Standardverbindungsprotokoll für in Azure gehostete virtuelle Linux-Computer. SSH stellt zwar bereits eine verschlüsselte Verbindung bereit, bei Verwendung von Kennwörtern für SSH-Verbindungen ist der virtuelle Computer jedoch weiterhin anfällig für Brute-Force-Angriffe bzw. der Gefahr ausgesetzt, dass das Kennwort erraten wird. Die sicherere und bevorzugte Methode für die Verbindungsherstellung mit einem virtuellen Computer über SSH ist die Verwendung öffentlicher und privater Schlüssel, die auch als SSH-Schlüssel bezeichnet werden.

Falls Sie keine SSH-Schlüssel verwenden möchten, können Sie sich natürlich auch weiterhin mithilfe eines Kennworts bei Ihren virtuellen Linux-Computern anmelden. Bei virtuellen Computern ohne Internetverbindung sind Kennwörter unter Umständen ausreichend. Allerdings müssen Sie weiterhin Ihre Kennwörter für die einzelnen virtuellen Linux-Computer verwalten und sich um angemessene Kennwortrichtlinien und -verfahren kümmern, indem Sie beispielsweise eine Mindestlänge für Kennwörter festlegen und sicherstellen, dass sie regelmäßig aktualisiert werden. Durch die Verwendung von SSH-Schlüsseln wird die übergreifende Verwaltung einzelner Anmeldeinformationen für mehrere virtuelle Computer weniger komplex.

## <a name="windows-packages-and-ssh-clients"></a>Windows-Pakete und SSH-Clients
Die Verbindungsherstellung mit virtuellen Linux-Computern in Azure sowie deren Verwaltung wird mithilfe eines **SSH-Clients** durchgeführt. Auf Windows-Computern ist üblicherweise kein SSH-Client installiert. Im Windows 10 Anniversary Update wurde Bash für Windows hinzugefügt, und das neueste Windows 10 Creators Update stellt weitere Updates bereit. Mit dem Windows-Subsystem für Linux haben Sie die Möglichkeit, direkt aus einer Bash-Shell auf Hilfsprogramme, z.B. einen SSH-Client, zuzugreifen und diese auszuführen. Sie können dann entsprechend den Anleitungen eines beliebigen Linux-Dokuments vorgehen, etwa [Erstellen von SSH-Schlüsselpaaren für Linux](mac-create-ssh-keys.md). Bash für Windows befindet sich noch in der Entwicklung wird als Beta-Version bereitgestellt. Weitere Informationen zu Bash für Windows finden Sie unter [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about) (Bash unter Ubuntu unter Windows).

Wenn Sie nicht Bash für Windows, sondern eine andere Komponente verwenden möchten, finden Sie in den folgenden Paketen gängige Windows SSH-Clients, die Sie installieren können:

* [Git für Windows](https://git-for-windows.github.io/)
* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)


## <a name="which-key-files-do-you-need-to-create"></a>Erforderliche Schlüsseldateien
Für Azure sind öffentliche und private Schlüssel mit mindestens 2.048 Bit im Format **ssh-rsa** erforderlich. Wenn Sie Azure-Ressourcen mit dem klassischen Bereitstellungsmodell verwalten, müssen Sie auch eine PEM-Datei (`.pem`) generieren.

Im Folgenden sind die Dateitypen für die unterschiedlichen Bereitstellungsszenarien aufgeführt:

1. **ssh-rsa**-Schlüssel werden für Bereitstellungen über das [Azure-Portal](https://portal.azure.com) sowie für Resource Manager-Bereitstellungen über die [Azure-Befehlszeilenschnittstelle](../../cli-install-nodejs.md) benötigt.
   * Diese Schlüssel sind in der Regel alles, was die meisten Benutzer benötigen.
2. Eine `.pem`-Datei wird für die Erstellung virtueller Computer über die klassische Bereitstellung benötigt. Diese Schlüssel werden bei klassischen Bereitstellungen unterstützt, wenn das [Azure-Portal](https://portal.azure.com) oder die [Azure CLI](../../cli-install-nodejs.md) verwendet wird.
   * Die zusätzlichen Schlüssel und Zertifikate müssen nur erstellt werden, wenn Sie Ressourcen verwalten, die mit dem klassischen Bereitstellungsmodell erstellt wurden.

## <a name="install-git-for-windows"></a>Installieren von Git für Windows
Im vorherigen Abschnitt wurden mehrere Pakete aufgeführt, in denen das `openssl`-Tool für Windows enthalten ist. Dieses Tool wird benötigt, um öffentliche und private Schlüssel zu erstellen. In den folgenden Beispielen erfahren Sie, wie Sie **Git für Windows** installieren und verwenden. Sie können aber auch eines der anderen Pakete verwenden. Mit **Git für Windows** erhalten Sie Zugriff auf einige zusätzliche Open Source-Tools und -Hilfsprogramme (Open Source-Software, [OSS](https://en.wikipedia.org/wiki/Open-source_software)), die Sie bei der Arbeit mit virtuellen Linux-Computern unterstützen.

1. Laden Sie **Git für Windows** unter [https://git-for-windows.github.io/](https://git-for-windows.github.io/) herunter, und installieren Sie es.
2. Akzeptieren Sie bei der Installation die Standardoptionen, sofern Sie sie nicht ausdrücklich ändern müssen.
3. Führen Sie **Git Bash** aus (**Startmenü** > **Git** > **Git Bash**). Die Konsole sieht in etwa wie im folgenden Beispiel aus:

    ![Git für Windows Bash (Shell)](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a>Erstellen eines privaten Schlüssels
1. Erstellen Sie im Fenster **Git Bash** mithilfe von `openssl.exe` einen privaten Schlüssel. Im folgenden Beispiel werden ein Schlüssel namens `myPrivateKey` und ein Zertifikat namens `myCert.pem` erstellt:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Die Ausgabe sieht in etwa wie im folgenden Beispiel aus:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   Wenn Bash einen Fehler meldet, versuchen Sie, ein neues **Git Bash**-Fenster mit erhöhten Rechten zu öffnen. Führen Sie anschließend den Befehl `openssl` erneut aus.

2. Beantworten Sie die Fragen nach Ländername, Standort, Organisationsname usw.
3. Der neue private Schlüssel und das Zertifikat werden in Ihrem aktuellen Arbeitsverzeichnis erstellt. Als Sicherheitsmaßnahme sollten Sie die Berechtigungen für Ihren privaten Schlüssel so festlegen, dass nur Sie darauf zugreifen können:

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. Der [nächste Abschnitt](#create-a-private-key-for-putty) erläutert die Verwendung von PuTTYgen, um den öffentlichen Schlüssel anzuzeigen und zu verwenden, sowie das Erstellen eines privaten Schlüssels speziell zur Verwendung von PuTTY, um SSH-Verbindungen mit virtuellen Linux-Computern herzustellen. Der folgende Befehl generiert eine öffentliche Schlüsseldatei mit dem Namen `myPublicKey.key`, die Sie sofort verwenden können:

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. Falls Sie auch klassische Ressourcen verwalten müssen, konvertieren Sie `myCert.pem` in `myCert.cer` (DER-codiertes X509-Zertifikat). Führen Sie diesen optionalen Schritt nur aus, wenn Sie speziell ältere klassische Ressourcen verwalten müssen.

    Konvertieren Sie das Zertifikat mithilfe des folgenden Befehls:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Erstellen eines privaten Schlüssels für PuTTY
PuTTY ist ein verbreiteter SSH-Client für Windows. Sie können aber auch einen anderen SSH-Client verwenden. Wenn Sie PuTTY verwenden möchten, müssen Sie einen zusätzlichen Schlüsseltyp erstellen: einen privaten PuTTY-Schlüssel (PuTTY Private Key, PPK). Wenn Sie PuTTY nicht verwenden möchten, können Sie diesen Abschnitt überspringen.

Im folgenden Beispiel wird der zusätzliche private Schlüssel erstellt, der speziell für die Verwendung durch PuTTY vorgesehen ist:

1. Konvertieren Sie Ihren privaten Schlüssel mithilfe von **Git Bash** in einen privaten, für PuTTYgen verständlichen RSA-Schlüssel. Im folgenden Beispiel wird auf der Grundlage des bereits vorhandenen Schlüssels `myPrivateKey` ein Schlüssel namens `myPrivateKey_rsa` erstellt:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Als Sicherheitsmaßnahme sollten Sie die Berechtigungen für Ihren privaten Schlüssel so festlegen, dass nur Sie darauf zugreifen können:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. Laden Sie PuTTYgen unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) herunter, und führen Sie es aus.
3. Wählen Sie die Befehlsfolge **Datei** > **Privaten Schlüssel laden**.
4. Suchen Sie Ihren privaten Schlüssel (`myPrivateKey_rsa` im vorherigen Beispiel). Das Standardverzeichnis beim Start von **Git Bash** ist `C:\Users\%username%`. Ändern Sie den Dateifilter, sodass **Alle Dateien (\*.\*)** angezeigt werden:

    ![Laden des vorhandenen privaten Schlüssels in PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. Klicken Sie auf **Öffnen**. Eine Meldung informiert darüber, dass der Schlüssel erfolgreich importiert wurde:

    ![Schlüssel erfolgreich in PuTTYgen importiert](./media/ssh-from-windows/successfully-imported-key.png)
6. Klicken Sie auf **OK** , um die Meldung zu schließen.
7. Der öffentliche Schlüssel wird am oberen Rand des Fensters von **PuTTYgen** angezeigt. Dieser öffentliche Schlüssel muss beim Erstellen eines virtuellen Linux-Computers kopiert und in das Azure-Portal oder in die Azure Resource Manager-Vorlage eingefügt werden. Sie können auch auf die Option zum **Speichern des öffentlichen Schlüssels** klicken, um eine Kopie auf Ihrem Computer zu speichern:

    ![Speichern der Datei mit dem öffentlichen PuTTY-Schlüssel](./media/ssh-from-windows/save-public-key.png)

    Das folgende Beispiel zeigt, wie Sie diesen öffentlichen Schlüssel beim Erstellen eines virtuellen Linux-Computers kopieren und in das Azure-Portal einfügen. Der öffentliche Schlüssel wird dann in der Regel auf Ihrem neuen virtuellen Computer unter `~/.ssh/authorized_keys` gespeichert.

    ![Verwenden des öffentlichen Schlüssels beim Erstellen eines virtuellen Computers im Azure-Portal](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. Klicken Sie in **PuTTYgen** auf die Option zum **Speichern des privaten** Schlüssels:

    ![Speichern der Datei mit dem privaten PuTTY-Schlüssel](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > Sie werden gefragt, ob Sie den Vorgang ohne Eingabe einer Passphrase für Ihren Schlüssel fortsetzen möchten. Eine Passphrase ist vergleichbar mit einem Kennwort, das mit Ihrem privaten Schlüssel verknüpft wird. Sollte also jemand Ihren privaten Schlüssel an sich bringen, kann er sich trotzdem nicht authentifizieren. Er benötigt zusätzlich auch die Passphrase. Ohne Passphrase kann sich jeder, der über Ihren privaten Schlüssel verfügt, bei den virtuellen Computern oder Diensten anmelden, die diesen Schlüssel verwenden. Daher empfiehlt es sich, eine Passphrase zu erstellen. Wenn Sie die Passphrase vergessen, besteht jedoch keine Möglichkeit, diese wiederherzustellen.
   >
   >

    Falls Sie eine Passphrase eingeben möchten, klicken Sie auf **Nein**, geben Sie im Hauptfenster von PuTTYgen eine Passphrase ein, und klicken Sie anschließend erneut auf die Option zum **Speichern des privaten** Schlüssels. Klicken Sie andernfalls auf **Ja**, um den Vorgang ohne die optionale Passphrase fortzusetzen.
9. Geben Sie einen Namen und einen Speicherort für Ihre PPK-Datei ein.

## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Verwenden von PuTTY zum Herstellen einer SSH-Verbindung mit einem Linux-Computer
Bei PuTTY handelt es sich wie bereits erwähnt um einen verbreiteten SSH-Client für Windows. Sie können aber auch einen anderen SSH-Client verwenden. Die folgenden Schritte zeigen, wie Sie sich per SSH und unter Verwendung Ihres privaten Schlüssels bei Ihrem virtuellen Computer authentifizieren. Die Vorgehensweise ist in anderen SSH-Schlüsselclients ähnlich. Auch hier müssen Sie Ihren privaten Schlüssel laden, um die SSH-Verbindung zu authentifizieren.

1. Laden Sie PuTTY unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) herunter, und führen Sie es aus.
2. Geben Sie den Hostnamen oder die IP-Adresse des virtuellen Computers aus dem Azure-Portal ein:

    ![Herstellen einer neuen PuTTY-Verbindung](./media/ssh-from-windows/putty-new-connection.png)
3. Wählen Sie zunächst **Verbindung** > **SSH** > **Authentifizierung** und anschließend **Öffnen** aus. Navigieren Sie zu Ihrem privaten Schlüssel, und wählen Sie ihn aus:

    ![Auswählen Ihres privaten PuTTY-Schlüssels für die Authentifizierung](./media/ssh-from-windows/putty-auth-dialog.png)
4. Klicken Sie auf **Öffnen** , um die Verbindung mit Ihrem virtuellen Computer herzustellen.

## <a name="next-steps"></a>Nächste Schritte
Sie können den öffentlichen und privaten Schlüssel auch [mithilfe von OS X und Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) generieren.

Weitere Informationen zu Bash für Windows und zu den Vorteilen der Verfügbarkeit von OSS-Tools auf Ihrem Windows-Computer finden Sie unter [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about) (Bash unter Ubuntu unter Windows).

Sollten beim Herstellen der SSH-Verbindung mit Ihren virtuellen Linux-Computern Probleme auftreten, lesen Sie unter [Behandeln von Problemen, Fehlern oder Ablehnungen im Zusammenhang mit der SSH-Verbindung mit einem virtuellen Azure Linux-Computer](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) weiter.

