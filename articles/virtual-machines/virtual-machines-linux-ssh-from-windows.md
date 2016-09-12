<properties 
	pageTitle="Verwenden von SSH unter Windows zum Herstellen einer Verbindung mit virtuellen Linux-Computern | Microsoft Azure" 
description="Erfahren Sie, wie Sie SSH-Schlüssel auf einem Windows-Computer erstellen und verwenden, um Verbindungen mit virtuellen Linux-Computern in Azure herzustellen." 
	services="virtual-machines-linux" 
	documentationCenter="" 
	authors="squillace" 
	manager="timlt" 
	editor=""
	tags="azure-service-management,azure-resource-manager" />

<tags 
	ms.service="virtual-machines-linux" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-linux" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/29/2016" 
	ms.author="rasquill"/>

#Verwenden von SSH mit Windows in Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md)


In diesem Thema wird beschrieben, wie Sie unter Windows mithilfe von **ssh-rsa** öffentliche und private Schlüsseldateien im **PEM**-Format erstellen und verwenden, um mit diesen und dem Befehl **ssh** Verbindungen mit Ihren Linux-VMs in Azure herzustellen. Wenn Sie bereits **PEM**-Dateien erstellt haben, können diese verwenden, um virtuelle Linux-Computer zu erstellen, zu denen Sie mit **ssh** eine Verbindung herstellen können. Viele weitere Befehle verwenden das **SSH**-Protokoll und Schlüsseldateien für das sichere Ausführen von Aufgaben, insbesondere **scp** oder [Secure Copy](https://en.wikipedia.org/wiki/Secure_copy), das Dateien sicher zwischen Computern kopieren kann, die **SSH**-Verbindungen unterstützen.

> [AZURE.NOTE] Falls Sie einen Moment Zeit haben, würden wir uns freuen, wenn Sie an dieser [kurzen Umfrage](https://aka.ms/linuxdocsurvey) teilnehmen könnten, um zur Verbesserung der Dokumentation für virtuelle Azure-Computer unter Linux beizutragen. Jede Antwort hilft uns dabei, Sie noch besser bei Ihrer Arbeit zu unterstützen.

## Erforderliche SSH- und Schlüsselerstellungsprogramme

**SSH** &#8212; oder [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) &#8212; ist ein Protokoll für verschlüsselte Verbindungen, das die sichere Anmeldung über ungesicherte Verbindungen ermöglicht. Es ist das Standardprotokoll für die Verbindung mit virtuellen Linux-Computern, die in Azure gehostet werden, sofern Sie nicht andere Verbindungsmechanismen für Ihre virtuellen Linux-Computer konfigurieren haben. Windows-Benutzer können mit **ssh**-Clientimplementierung auch Verbindungen mit Linux-VMs in Azure herstellen und diese verwalten, Windows-Computer bieten i. d. R. aber standardmäßig keinen **ssh**-Client, sodass Sie einen auswählen müssen.

Folgende häufig verwendete Clients können Sie installieren:

- [puTTY und puTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)
- [Git für Windows](https://git-for-windows.github.io/) bietet schon im Lieferumfang die Umgebung und Tools

Wenn Sie über etwas mehr Erfahrungen verfügen, können Sie auch den [neuen Port des **OpenSSH**-Toolsets für Windows](http://blogs.msdn.com/b/powershell/archive/2015/10/19/openssh-for-windows-update.aspx) testen. Beachten Sie jedoch, dass sich dieser Code zurzeit noch in der Entwicklung befindet, deshalb sollten Sie die Codebasis überprüfen, bevor Sie sie für Produktionssysteme verwenden.

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Erforderliche Schlüsseldateien

Ein grundlegendes SSH-Setup für Azure umfasst ein öffentliches und privates **SSH-RSA**-Schlüsselpaar mit 2048 Bits (**ssh-keygen** speichert diese Dateien standardmäßig unter **~/.ssh/id\_rsa** und **~/.ssh/id-rsa.pub**, sofern Sie die Standardeinstellungen nicht ändern) sowie eine `.pem`-Datei, die zur Verwendung mit dem klassischen Bereitstellungsmodell des klassischen Portals aus der **id\_rsa**-Datei mit dem privaten Schlüssel generiert wird.

Im Folgenden sind die Dateitypen für die unterschiedlichen Bereitstellungsszenarien aufgeführt:

1. **SSH-RSA**-Schlüssel sind unabhängig vom Bereitstellungsmodell für alle Bereitstellungen mithilfe des [Azure-Portals](https://portal.azure.com) erforderlich.
2. PEM-Dateien sind erforderlich, um VMs mithilfe des [klassischen Portals](https://manage.windowsazure.com) zu erstellen. PEM-Dateien werden auch in klassischen Bereitstellungen mit der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) unterstützt.

> [AZURE.NOTE] Wenn Sie vorhaben, mit dem klassischen Bereitstellungsmodell bereitgestellte Dienste zu verwalten, können Sie auch eine **CER**-Datei erstellen. Dies erfordert jedoch weder die Verwendung von **ssh** noch das Herstellen einer Verbindung mit Linux-VMs und ist daher nicht Gegenstand dieses Artikels. Geben Sie zum Erstellen dieser Dateien unter Windows Folgendes ein: <br /> openssl.exe x509 -outform der -in myCert.pem -out myCert.cer

## Abrufen von ssh-keygen und openssl unter Windows ##

[Dieser Abschnitt](#What-SSH-and-key-creation-programs-do-you-need) oben listet verschiedene Dienstprogramme auf, die `ssh-keygen` und `openssl` für Windows enthalten. Nachstehend finden Sie einige Beispiele:

###Verwenden von Git für Windows###

1.	Laden Sie Git für Windows von folgendem Speicherort herunter, und installieren Sie es: [https://git-for-windows.github.io/](https://git-for-windows.github.io/)
2.	Führen Sie Git Bash im Startmenü über „Alle Apps > Git Shell“ aus.

> [AZURE.NOTE] Beim Ausführen der obigen `openssl`-Befehle tritt möglicherweise der folgende Fehler auf:

        Unable to load config info from /usr/local/ssl/openssl.cnf

Die einfachste Lösung ist, die Umgebungsvariable `OPENSSL_CONF` festzulegen. Der Prozess zum Festlegen dieser Variable variieren abhängig von der Shell, die Sie in Github konfiguriert haben:

**Powershell:**

        $Env:OPENSSL_CONF="$Env:GITHUB_GIT\ssl\openssl.cnf"

**CMD:**

        set OPENSSL_CONF=%GITHUB_GIT%\ssl\openssl.cnf

**Git Bash:**

        export OPENSSL_CONF=$GITHUB_GIT/ssl/openssl.cnf
	

###Verwenden von Cygwin###

1.	Laden und installieren Sie Cygwin von folgendem Speicherort: [http://cygwin.com/](http://cygwin.com/)
2.	Stellen Sie sicher, dass das OpenSSL-Paket und alle seine Abhängigkeiten installiert sind.
3.	Führen Sie `cygwin` aus.

## Erstellen eines privaten Schlüssels##

1.	Befolgen Sie eine der obigen Anweisungen, um `openssl.exe` auszuführen.
2.	Geben Sie den folgenden Befehl ein:

  ```
  openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 -keyout myPrivateKey.key -out myCert.pem
  ```
3.	Ihr Bildschirm sollte nun folgendermaßen aussehen:

  ```
  $ openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 -keyout myPrivateKey.key -out myCert.pem
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

4.	Beantworten Sie die Fragen, die Ihnen gestellt werden.
5.	Es wären zwei Dateien erstellt worden: `myPrivateKey.key` und `myCert.pem`.
6.	Wenn Sie die API direkt ohne das Verwaltungsportal verwenden möchten, konvertieren Sie die Datei `myCert.pem` in `myCert.cer` (DER-codiertes X509-Zertifikat). Verwenden Sie dafür den folgenden Befehl:

  ```
  openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
  ```

## Erstellen eines PPK für Putty ##

1. Download and install Puttygen from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Puttygen ist möglicherweise nicht in der Lage, den zuvor erstellten privaten Schlüssel (`myPrivateKey.key`) zu lesen. Führen Sie den folgenden Befehl aus, um ihn in einen privaten RSA-Schlüssel zu übersetzen, den Puttygen verstehen kann:

		# openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
		# chmod 600 ./myPrivateKey_rsa

	Der obige Befehl sollte den neuen privaten Schlüssel „myPrivateKey\_rsa“ generieren.

3. Führen Sie `puttygen.exe` aus.

4. Wählen Sie die Befehlsfolge "Datei > Privaten Schlüssel laden".

5. Suchen Sie den privaten Schlüssel, den wir oben als `myPrivateKey_rsa` benannt haben. Ändern Sie den Dateienfilter, so dass er **Alle Dateien (*.*)** anzeigt.

6. Klicken Sie auf **Öffnen**. Sie erhalten eine Aufforderung, die ungefähr folgendermaßen aussieht:

	![linuxgoodforeignkey](./media/virtual-machines-linux-ssh-from-windows/linuxgoodforeignkey.png)

7. Klicken Sie auf **OK**.

8. Klicken Sie auf **Privaten Schlüssel speichern**. Diese Option ist im Screenshot unten hervorgehoben:

	![linuxputtyprivatekey](./media/virtual-machines-linux-ssh-from-windows/linuxputtygenprivatekey.png)

9. Speichern Sie die Datei als PPK.


## Verwenden von Putty für eine Verbindung mit einem Linux-Computer ##

1.	Laden und installieren Sie Putty von folgendem Speicherort: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.	Führen Sie putty.exe aus.
3.	Geben Sie den Hostnamen ein. Verwenden Sie dafür die IP aus dem Verwaltungsportal:

	![linuxputtyconfig](./media/virtual-machines-linux-ssh-from-windows/linuxputtyconfig.png)

4.	Klicken Sie auf der Registerkarte „Verbindung“ auf „SSH > Auth“, um Ihren privaten Schlüssel zu wählen, und anschließend auf **Öffnen**. Das auszufüllende Feld finden Sie auf dem Screenshot unten:

	![linuxputtyprivatekey](./media/virtual-machines-linux-ssh-from-windows/linuxputtyprivatekey.png)

5.	Klicken Sie auf **Öffnen**, um die Verbindung mit Ihrem virtuellen Computer herzustellen.
 

<!---HONumber=AcomDC_0831_2016-->