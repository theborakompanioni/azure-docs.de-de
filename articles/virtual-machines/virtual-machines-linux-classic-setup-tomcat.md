---
title: Einrichten von Apache Tomcat auf einem virtuellen Linux-Computer | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Apache Tomcat7 unter Verwendung eines virtuellen Azure-Computers (VM) mit Linux einrichten können."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
translationtype: Human Translation
ms.sourcegitcommit: 61fa1d554be259a439d1da169d041d85652ab0cf
ms.openlocfilehash: 6359c2ed8d7f77b9433a4d6ceb805015b6048d4c


---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Einrichten von Tomcat7 auf einem virtuellen Linux-Computer mit Microsoft Azure
Apache Tomcat (oder einfach Tomcat, früher auch Jakarta Tomcat) ist ein Open-Source-Webserver und Servlet-Container und wurde von der Apache Software Foundation (ASF) entwickelt. Tomcat implementiert das Java-Servlet und die Java Server Pages-Spezifikationen (JSP) von Sun Microsystems. Tomcat bietet eine reine Java-HTTP-Webserverumgebung, in der Java-Code ausgeführt wird. In der einfachsten Konfiguration wird Tomcat in einem einzelnen Betriebssystem-Prozess ausgeführt. Dieser Prozess führt eine Java Virtual Machine (JVM) aus. Jede HTTP-Anforderung von einem Browser an Tomcat wird als separater Thread im Tomcat-Prozess verarbeitet.  

> [!IMPORTANT]
> Azure bietet zwei Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) und klassisch. Dieser Artikel gilt für das klassische Bereitstellungsmodell. Wir empfehlen für die meisten Neubereitstellungen das Resource Manager-Modell. Informationen zum Bereitstellen einer Ubuntu-VM mit Open JDK und Tomcat mithilfe einer Resource Manager-Vorlage finden Sie [in diesem Artikel](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).

In diesem Handbuch installieren Sie Tomcat7 in einem Linux-Image, das Sie in Azure bereitstellen.  

Sie erhalten Informationen zu folgenden Themen:  

* Erstellen eines virtuellen Computers in Azure.
* Vorbereiten des virtuellen Computers für Tomcat7.
* Installieren von Tomcat7.

Es wird vorausgesetzt, dass Sie bereits über ein Azure-Abonnement verfügen.  Falls nicht, können Sie sich auf der [Azure-Website](https://azure.microsoft.com/) für eine kostenlose Testversion registrieren. Wenn Sie über ein MSDN-Abonnement verfügen, finden Sie weitere Informationen unter [Microsoft Azure-Sonderpreis: MSDN-, MPN- und Bizspark-Leistungen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Weitere Informationen zu Azure finden Sie unter [Was ist Azure?](https://azure.microsoft.com/overview/what-is-azure/)

In diesem Artikel wird davon ausgegangen, dass Sie über allgemeine Kenntnisse zu Tomcat und Linux verfügen.  

## <a name="phase-1-create-an-image"></a>Phase 1: Erstellen eines Image
In dieser Phase erstellen Sie einen virtuellen Computer mit einem Linux-Image in Azure.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>Schritt 1: Generieren eines SSH-Authentifizierungsschlüssels
SSH ist ein wichtiges Tool für Systemadministratoren. Die Konfiguration der Zugriffssicherheit auf Grundlage eines vom Menschen bestimmten Kennworts wird jedoch nicht empfohlen. Böswillige Benutzer können über einen Benutzernamen und ein unsicheres Kennwort in Ihr System eindringen.

Die gute Nachricht ist, dass es eine Möglichkeit gibt, wie Sie den Remotezugriff zulassen können, ohne sich über Kennwörter sorgen zu müssen. Die Methode sieht die Authentifizierung mithilfe asymmetrischer Kryptografie vor. Der private Schlüssel des Benutzers ist der Schlüssel, der die Authentifizierung vornimmt. Sie können sogar das Konto des Benutzers sperren, um die Kennwortauthentifizierung zu untersagen.

Ein weiterer Vorteil dieser Methode ist, dass Sie nicht verschiedene Kennwörter benötigen, um sich auf verschiedenen Servern anzumelden. Sie können sich auf allen Servern mit dem persönlichen privaten Schlüssel authentifizieren. Dies erspart Ihnen auch, sich mehrere Kennwörter merken zu müssen.



Führen Sie die folgenden Schritte aus, um den SSH-Authentifizierungsschlüssel zu generieren.

1. Laden und installieren Sie PuTTYgen von folgender Adresse: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Führen Sie „Puttygen.exe“ aus.
3. Klicken Sie auf **Generieren**, um die Schlüssel zu generieren. Während des Vorgangs können Sie den Zufallsfaktor durch Bewegen der Maus über den leeren Bereich im Fenster erhöhen.  
   ![Screenshots des PuTTY-Schlüsselgenerators mit Schaltfläche zum Generieren eines neuen Schlüssels][1]
4. Nach dem Generierungsvorgang zeigt „Puttygen.exe“ Ihren neuen öffentlichen Schlüssel.  
   ![Screenshots des PuTTY-Schlüsselgenerators mit neuem öffentlichen Schlüssel und Schaltfläche zum Speichern des privaten Schlüssels][2]
5. Wählen Sie den öffentlichen Schlüssel aus. Kopieren und speichern Sie ihn in einer Datei namens „publicKey.pem“. Klicken Sie nicht auf **Öffentlichen Schlüssel speichern**, weil sich das Dateiformat des gespeicherten öffentlichen Schlüssels vom gewünschten öffentlichen Schlüssel unterscheidet.
6. Klicken Sie auf **Save private key** (Privaten Schlüssel speichern), und speichern Sie den Schlüssel dann in einer Datei namens „privateKey.ppk“.

### <a name="step-2-create-the-image-in-the-azure-portal"></a>Schritt 2: Erstellen des Images im Azure-Portal
1. Klicken Sie im [Portal](https://portal.azure.com/) auf der Taskleiste auf **Neu**, um ein Image zu erstellen. Wählen Sie dann das Linux-Image aus, das auf Ihren Anforderungen basiert. Dieses Beispiel verwendet das Ubuntu 14.04-Image.
![Screenshot des Portals mit der Schaltfläche „Neu“][3]

2. Geben Sie unter **Hostname** den Namen der URL ein, die Sie und Internetclients für den Zugriff auf diesen virtuellen Computer verwenden. Legen Sie den letzten Teil des DNS-Namens fest, z.B. als „tomcatdemo“. Azure generiert dann diese URL: tomcatdemo.cloudapp.net.  

3. Kopieren Sie für **SSH Authentication Key** (SSH-Authentifizierungsschlüssel) den Schlüsselwert aus der Datei „publicKey.pem“, die den von PuttyGen generierten öffentlichen Schlüssel enthält.  
![Feld mit dem SSH-Authentifizierungsschlüssel im Portal][4]

4. Konfigurieren Sie weitere Einstellungen nach Bedarf, und klicken Sie dann auf **Erstellen**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Phase 2: Vorbereiten des virtuellen Computers für Tomcat7
In dieser Phase konfigurieren Sie einen Endpunkt für Tomcat-Datenverkehr und stellen dann eine Verbindung mit dem neuen virtuellen Computer her.

### <a name="step-1-open-the-http-port-to-allow-web-access"></a>Schritt 1: Öffnen des HTTP-Ports, um Webzugriff zu erlauben
Endpunkte in Azure bestehen aus einem Protokoll (TCP oder UDP) sowie aus einem öffentlichen und einem privaten Port. Der private Port ist der Port, an dem der Dienst auf dem virtuellen Computer lauscht. Der öffentliche Port ist der Port, an dem der Azure-Clouddienst extern auf internetbasierten Datenverkehr lauscht.  

TCP-Port 8080 ist die Standardportnummer, an der Tomcat lauscht. Wenn Sie diesen Port auf einem Azure-Endpunkt öffnen, können Sie und andere Internetclients auf Tomcat-Seiten zugreifen.  

1. Klicken Sie im Azure-Portal auf **Durchsuchen** > **Virtuelle Computer**, und klicken Sie dann auf den virtuellen Computer, den Sie erstellt haben.  
   ![Screenshot des Verzeichnisses „Virtuelle Computer“][5]
2. Klicken Sie zum Hinzufügen eines Endpunkts zu einem virtuellen Computer auf das Feld **Endpunkte** .
   ![Screenshot mit dem Feld „Endpunkte“][6]
3. Klicken Sie auf **Hinzufügen**.  

   1. Geben Sie in **Endpunkt** einen Endpunktnamen ein und dann unter **Öffentlicher Port** den Port „80“ ein.  

      Wenn Sie den Port auf 80 festlegen, müssen Sie die Portnummer nicht in die URL einschließen, die Ihnen den Zugriff auf Tomcat ermöglicht. Beispiel: http://tomcatdemo.cloudapp.net.    

      Wenn Sie den Port auf einen anderen Wert festlegen (z.B. 81), müssen Sie die Portnummer der URL für den Zugriff auf Tomcat hinzufügen. Beispiel: http://tomcatdemo.cloudapp.net:81/.
   2. Geben Sie unter **Privater Port** 8080 ein. Tomcat lauscht standardmäßig an TCP-Port 8080. Wenn Sie den Standardüberwachungsport von Tomcat geändert haben, sollten Sie den **privaten Port** so ändern, dass er mit dem Tomcat-Überwachungsport übereinstimmt.  
      ![Screenshot der Benutzeroberfläche mit den Befehlen „Hinzufügen“, „Öffentlicher Port“ und „Privater Port“][7]
4. Klicken Sie auf **OK**, um den Endpunkt Ihrem virtuellen Computer hinzuzufügen.

### <a name="step-2-connect-to-the-image-you-created"></a>Schritt 2: Herstellen einer Verbindung mit dem Image, das Sie erstellt haben
Sie können ein beliebiges SSH-Tool für das Herstellen einer Verbindung mit Ihrem virtuellen Computer auswählen. In diesem Beispiel wird PuTTY verwendet.  

1. Rufen Sie den DNS-Namen Ihres virtuellen Computers aus dem Portal ab.
    1. Klicken Sie auf **Durchsuchen** > **Virtuelle Computer**.
    2. Wählen Sie den Namen Ihres virtuellen Computers aus, und klicken Sie dann auf **Eigenschaften**.
    3. Rufen Sie den DNS-Namen auf der Kachel **Eigenschaften** im Feld **Domänenname** ab.  

2. Rufen Sie die Portnummer für SSH-Verbindungen aus dem Feld **SSH** ab.  
![Screenshot mit der Nummer der Portnummer der SSH-Verbindung][8]

3. Laden Sie [PuTTY](http://www.putty.org/) herunter.  

4. Klicken Sie nach dem Herunterladen auf die ausführbare Datei „Putty.exe“. Konfigurieren Sie in der PuTTY-Konfiguration die grundlegenden Optionen mit dem Hostnamen und der Portnummer, die Sie aus den Eigenschaften Ihres virtuellen Computers abgerufen haben.   
![Screenshot mit den PuTTY-Konfigurationsoptionen „Hostname“ und „Port“][9]

5. Klicken Sie im linken Bereich auf **Connection** > **SSH** > **Auth** (Verbindung>SSH>-Authentifizierung), und klicken Sie dann auf **Browse** (Durchsuchen), um den Speicherort der Datei „privateKey.ppk“ anzugeben. Die Datei „privateKey.ppk“ enthält den privaten Schlüssel, der von PuTTYgen generiert wird (siehe den Abschnitt „Phase 1: Erstellen eines Images“ dieses Artikels).  
![Screenshot mit der Verzeichnishierarchie von Verbindungen und der Schaltfläche zum Durchsuchen][10]

6. Klicken Sie auf **Öffnen**. Möglicherweise wird ein Meldungsfeld angezeigt. Wenn Sie den DNS-Namen und die Portnummer richtig konfiguriert haben, klicken Sie auf **Ja**.
![Screenshot mit der Benachrichtigung][11]

7. Sie werden aufgefordert, Ihren Benutzernamen einzugeben.  
![Screenshot, der zeigt, wo Sie den Benutzernamen eingeben][12]

8. Geben Sie den Benutzernamen an, den Sie zum Erstellen der virtuellen Computers in „Phase 1: Erstellen eines Images“ weiter oben in diesem Artikel verwendet haben. Folgendes sollte angezeigt werden:  
![Screenshot mit der Authentifizierungsbestätigung][13]

## <a name="phase-3-install-software"></a>Phase 3: Installieren von Software
In dieser Phase installieren Sie die Java-Laufzeitumgebung, Tomcat7 und andere Tomcat7-Komponenten.  

### <a name="java-runtime-environment"></a>Java-Laufzeitumgebung
Tomcat wird in Java geschrieben. Es gibt zwei Arten von Java Development Kits (JDKs): OpenJDK und Oracle JDK. Sie können das Gewünschte auswählen.  

> [!NOTE]
> Beide JDKs haben fast den gleichen Code für die Klassen in der Java-API, aber der Code für den virtuellen Computer ist unterschiedlich. OpenJDK verwendet eher offene Bibliotheken, während Oracle JDK eher geschlossene Bibliotheken verwendet. Oracle JDK bietet mehr Klassen sowie verschiedene Fehlerkorrekturen und ist stabiler als OpenJDK.

#### <a name="install-openjdk"></a>Installieren von OpenJDK  

Verwenden Sie den folgenden Befehl aus, um OpenJDK herunterzuladen.   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* So erstellen Sie ein Verzeichnis, dass die JDK-Dateien enthält:  

        sudo mkdir /usr/lib/jvm  
* So extrahieren Sie die JDK-Dateien in das Verzeichnis "/usr/lib/jvm/":  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a>Installieren von Oracle JDK


Verwenden Sie den folgenden Befehl, um Oracle JDK von der Oracle-Website herunterzuladen.  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* So erstellen Sie ein Verzeichnis, dass die JDK-Dateien enthält:  

        sudo mkdir /usr/lib/jvm  
* So extrahieren Sie die JDK-Dateien in das Verzeichnis "/usr/lib/jvm/":  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* So legen Sie Oracle JDK als Standard-JVM (Java Virtual Machine) fest  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a>Bestätigen, dass die Java-Installation erfolgreich war
Sie können einen Befehl wie den folgenden verwenden, um zu testen, ob die Java-Laufzeitumgebung ordnungsgemäß installiert ist:  

    java -version  

Wenn Sie OpenJDK installiert haben, sollte eine Meldung wie die folgende angezeigt werden: ![Meldung zur erfolgreichen Installation von OpenJDK][14]

Wenn Sie Oracle JDK installiert haben, sollte eine Meldung wie die folgende angezeigt werden: ![Meldung zur erfolgreichen Installation von Oracle JDK][15]

### <a name="install-tomcat7"></a>Installieren von Tomcat7
Geben Sie den folgenden Befehl für die Installation von Tomcat7 ein.  

    sudo apt-get install tomcat7  

Wenn Sie Tomcat7 nicht verwenden, verwenden Sie die entsprechende Variante dieses Befehls.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Bestätigen, dass die Tomcat7-Installation erfolgreich war
Um zu überprüfen, ob Tomcat7 erfolgreich installiert wurde, navigieren Sie zum DNS-Namen Ihres Tomcat-Servers. Für das Beispiel in diesem Artikel lautet die URL so: http://tomcatexample.cloudapp.net/. Wenn sinngemäß die folgende Meldung angezeigt wird, ist Tomcat7 ordnungsgemäß installiert.
![Meldung zur erfolgreichen Installation von Tomcat7][16]

### <a name="install-other-tomcat7-components"></a>Installieren anderer Tomcat7-Komponenten
Es gibt weitere optionale Tomcat-Komponenten, die Sie installieren können.  

Verwenden Sie den Befehl **sudo apt-cache search tomcat7**, um alle verfügbaren Komponenten anzuzeigen. Verwenden Sie die folgenden Befehle aus, um einige nützliche Komponenten zu installieren.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a>Phase 4: Konfigurieren von Tomcat7
In dieser Phase verwalten Sie Tomcat.

### <a name="start-and-stop-tomcat7"></a>Starten und Beenden von Tomcat7
Der Tomcat7-Server wird bei der Installation von Tomcat7 automatisch gestartet. Sie können den Server auch selbst mit dem folgenden Befehl starten:   

    sudo /etc/init.d/tomcat7 start

So beenden Sie Tomcat7

    sudo /etc/init.d/tomcat7 stop

So zeigen Sie Status von Tomcat7 an

    sudo /etc/init.d/tomcat7 status

So starten Sie Tomcat-Dienste neu 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7-Verwaltung
Sie können die Tomcat-Benutzerkonfigurationsdatei bearbeiten, um die Administratoranmeldeinformationen einzurichten. Verwenden Sie den folgenden Befehl:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Beispiel:  
![Screenshot der Ausgabe des Befehls „sudo vi“][17]  

> [!NOTE]
> Erstellen Sie ein sicheres Kennwort für den Administratorbenutzernamen.  

Nach der Bearbeitung dieser Datei sollten Sie Tomcat7-Dienste mit dem folgenden Befehl neu starten, um sicherzustellen, dass die Änderungen wirksam werden:  

    sudo /etc/init.d/tomcat7 restart  

Öffnen Sie den Browser, und geben Sie die URL **http://<your tomcat server DNS name>/manager/html** ein. Für das Beispiel in diesem Artikel ist die URL http://tomcatexample.cloudapp.net/manager/html.  

Nachdem die Verbindung hergestellt wurde, müsste in etwa Folgendes angezeigt werden:   
![Screenshot von Tomcat Web Application Manager][18]

## <a name="common-issues"></a>Häufige Probleme
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Auf den virtuellen Computer mit Tomcat und Moodle kann nicht über das Internet zugegriffen werden.
#### <a name="symptom"></a>Symptom  
  Tomcat wird ausgeführt, die Tomcat-Standardseite wird jedoch nicht in Ihrem Browser angezeigt.
#### <a name="possible-root-cause"></a>Mögliche Ursache   

  * Der Tomcat-Überwachungsport ist nicht identisch mit dem privaten Port des Endpunkts des virtuellen Computers für Tomcat-Datenverkehr.  

     Überprüfen Sie die Endpunkteinstellungen des öffentlichen und des privaten Ports, und stellen Sie sicher, dass der private Port mit dem Tomcat-Überwachungsport identisch ist. Anweisungen zum Konfigurieren von Endpunkten für den virtuellen Computer finden Sie unter „Phase1: Erstellen eines Images“ in diesem Artikel.  

     Öffnen Sie zur Bestimmung des Tomcat-Überwachungsports „/etc/httpd/conf/httpd.conf“ (Red Hat-Version) oder „/etc/tomcat7/server.xml“ (Debian-Version). Standardmäßig ist der Tomcat-Überwachungsport 8080. Beispiel:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Wenn Sie einen virtuellen Computer wie Debian oder Ubuntu verwenden und den Standard-Überwachungsport für Tomcat (z. B. 8081) ändern möchten, sollten Sie auch den Port für das Betriebssystem öffnen. Öffnen Sie zunächst das Profil:  

        sudo vi /etc/default/tomcat7  

     Löschen Sie dann den Kommentar der letzten Zeile und ändern Sie "Nein" zu "Ja".  

        AUTHBIND=yes
  2. Die Firewall hat den Überwachungsport von Tomcat deaktiviert.

     Es wird nur die Tomcat-Standardseite auf dem lokalen Host angezeigt. Das Problem ist sehr wahrscheinlich, dass der Port, an dem Tomcat lauscht, durch die Firewall blockiert wird. Sie können das Tool w3m verwenden, um die Webseite zu durchsuchen. Die folgenden Befehle installieren w3m und navigieren zur Apache-Standardseite:  


        sudo yum  install w3m w3m-img


        w3m http://localhost:8080  
#### <a name="solution"></a>Lösung

  * Wenn der Tomcat-Überwachungsport nicht mit dem privaten Port des Endpunkts für den Datenverkehr an den virtuellen Computer identisch ist, müssen Sie den privaten Port so ändern, dass dieser identisch mit dem Tomcat-Überwachungsport ist.   
  2. Wenn das Problem durch die Firewall/iptables verursacht wird, fügen Sie „/etc/sysconfig/iptables“ die folgenden Zeilen hinzu. Die zweite Zeile ist nur für HTTPS-Datenverkehr erforderlich:  

      -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT

      -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

     > [!IMPORTANT]
     > Vergewissern Sie sich, dass sich die vorherigen Zeilen über allen Zeilen befinden, die den globalen Zugriff einschränken, wie z.B.: -A INPUT -j REJECT --reject-with icmp-host-prohibited



Führen Sie den folgenden Befehl aus, um die iptables erneut zu laden:

    service iptables restart

Dies wurde unter CentOS 6.3 getestet.

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a>Beim Hochladen von Projektdateien in das Verzeichnis „/var/lib/tomcat7/webapps“ wird der Zugriff verweigert.
#### <a name="symptom"></a>Symptom
  Wenn Sie einen beliebigen SFTP-Client (z.B. FileZilla) zum Herstellen einer Verbindung mit Ihrem virtuellen Computer verwenden und zu „/var/lib/tomcat7/webapps/“ navigieren, um Ihre Website zu veröffentlichen, erhalten Sie sinngemäß die folgende Fehlermeldung:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Mögliche Ursache
  Sie haben keine Zugriffsberechtigungen für den Ordner "/var/lib/tomcat7/webapps".  
#### <a name="solution"></a>Lösung  
  Sie müssen die Berechtigungen aus dem Root-Konto abrufen. Sie können den Besitzer dieses Ordners von „root“ in den Benutzernamen ändern, den Sie bei der Bereitstellung des Computers verwendet haben. Das folgende Beispiel zeigt den Kontonamen "azureuser":  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Verwenden Sie die Option "-R", um die Berechtigungen auch auf alle Dateien in einem Verzeichnis anzuwenden.  

  Dieser Befehl funktioniert auch für Verzeichnisse. Die Option „-R“ ändert die Berechtigungen für alle Dateien und Verzeichnisse innerhalb des Verzeichnisses. Beispiel:  

     sudo chown -R username:group directory  

  Dieser Befehl ändert den Besitzer (Benutzer und Gruppe) für alle Dateien und Verzeichnisse im Verzeichnis.  

  Der folgende Befehl ändert nur die Berechtigung des Ordner „directory“. Die Dateien und Ordner im Verzeichnis werden nicht geändert.  

     sudo chown username:group directory

[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png



<!--HONumber=Feb17_HO3-->


