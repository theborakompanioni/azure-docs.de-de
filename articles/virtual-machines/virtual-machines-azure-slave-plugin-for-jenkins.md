<properties
	pageTitle="Verwenden des Azure Slave-Plug-Ins mit Jenkins Continuous Integration | Microsoft Azure"
	description="Beschreibt das Verwenden des Azure Slave-Plug-Ins mit Jenkins Continuous Integration."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="rmcmurray"
	manager="wpickett"
	editor="" />

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="java"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="robmcm"/>

# Verwenden des Azure Slave-Plug-Ins mit Jenkins Continuous Integration

Das Azure Slave-Plug-In für Jenkins ermöglicht Ihnen das Bereitstellen untergeordneter Knoten in Azure beim Ausführen von verteilten Builds.

## Installieren des Azure Slave-Plug-Ins

1. Klicken Sie im Jenkins-Dashboard auf **Manage Jenkins**.

1. Klicken Sie auf der Seite **Manage Jenkins** auf **Manage Plugins**.

1. Klicken Sie auf die Registerkarte **Available**.

1. Geben Sie im Filterfeld über der Liste der verfügbaren Plug-Ins **Azure** ein, um die Liste auf relevante Plug-Ins zu beschränken.

    Wenn Sie einen Bildlauf durch die Liste der verfügbaren Plug-Ins durchführen möchten, finden Sie das Azure Slave-Plug-In im Abschnitt **Cluster Management and Distributed Build**.

1. Aktivieren Sie das Kontrollkästchen **Azure Slave Plugin**.

1. Klicken Sie auf **Install without restart** oder auf **Download now and install after restart**.

Da das Plug-In jetzt installiert ist, sind die nächsten Schritte das Konfigurieren des Plug-Ins mit Ihrem Azure-Abonnementprofil und das Erstellen einer Vorlage, die verwendet wird, um den virtuellen Computer für den untergeordneten Knoten zu erstellen.


## Konfigurieren des Azure Slave-Plug-Ins mit Ihrem Abonnementprofil

Ein Abonnementprofil, das als Veröffentlichungseinstellungen bezeichnet wird, ist eine XML-Datei, die sichere Anmeldeinformationen und einige zusätzliche Informationen enthält, die Sie für die Arbeit mit Azure in Ihrer Entwicklungsumgebung benötigen. Um das Azure Slave-Plug-In zu konfigurieren, benötigen Sie Folgendes:

* Ihre Abonnement-ID
* Ein Verwaltungszertifikat für Ihr Abonnement

Sie finden dies in Ihrem [Abonnementprofil]. Es folgt ein Beispiel für ein Abonnementprofil.

	<?xml version="1.0" encoding="utf-8"?>

		<PublishData>

  		<PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

    	<Subscription

      		ServiceManagementUrl="https://management.core.windows.net"

      		Id="<Subscription ID value>"

      		Name="Pay-As-You-Go"
			ManagementCertificate="<Management certificate value>" />

  		</PublishProfile>

	</PublishData>

Sobald Sie Ihr Abonnementprofil haben, gehen folgendermaßen Sie vor, um das Azure Slave-Plug-In zu konfigurieren:

1. Klicken Sie im Jenkins-Dashboard auf **Manage Jenkins**.

1. Klicken Sie auf **Configure System**.

1. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Cloud** durch.

1. Klicken Sie auf **Add new cloud > Microsoft Azure**.

    ![Cloudabschnitt][cloud section]

    Dadurch werden die Felder angezeigt, in denen Sie Ihre Abonnementdetails eingeben müssen.

    ![Abonnementkonfiguration][subscription configuration]

1. Kopieren Sie die Abonnement-ID und die Verwaltungszertifikatswerte aus Ihrem Abonnementprofil, und fügen Sie sie in die entsprechenden Felder ein.

    Schließen Sie beim Kopieren der Abonnement-ID und des Verwaltungszertifikats nicht die Anführungszeichen ein, die die Werte umschließen.

1. Klicken Sie auf **Verify Configuration**.

1. Wenn die Konfiguration als korrekt überprüft wurde, klicken Sie auf **Speichern**.

## Einrichten einer Vorlage für virtuelle Computer für das Azure Slave-Plug-In

Eine Vorlage für virtuelle Computer definiert die Parameter, die das Plug-In zum Erstellen eines untergeordneten Knotens auf Azure verwendet. In den folgenden Schritten erstellen Sie eine Vorlage für einen virtuellen Ubuntu-Computer.

1. Klicken Sie im Jenkins-Dashboard auf **Manage Jenkins**.

1. Klicken Sie auf **Configure System**.

1. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Cloud** durch.

1. Suchen Sie im Abschnitt **Cloud** nach **Add Azure Virtual Machine Template**, und klicken Sie dann auf **Add**.

    ![VM-Vorlage hinzufügen][add vm template]

    Dadurch werden die Felder angezeigt, in denen Sie Details zur Vorlage eingeben, die Sie erstellen.

    ![leere allgemeine Konfiguration][blank general configuration]

1. Geben Sie im Feld **Name** einen Azure-Clouddienstnamen ein. Bezieht sich der von Ihnen eingegebene Name auf einen vorhandenen Clouddienst, wird der virtuelle Computer in diesem Dienst bereitgestellt. Andernfalls erstellt Azure einen neuen.

1. Geben Sie in das Feld **Beschreibung** Text ein, der die Vorlage beschreibt, die Sie erstellen. Dies dient nur Ihren Aufzeichnungen und wird nicht bei der Bereitstellung eines virtuellen Computers verwendet.

1. Das Feld **Labels** wird verwendet, um die Vorlage zu identifizieren, die Sie erstellen, und wird anschließend als Verweis auf die Vorlage beim Erstellen eines Jenkins-Auftrags verwendet. Geben Sie zu diesem Zweck **linux** in dieses Feld ein.

1. Klicken Sie in der Liste **Region** auf die Region, in der die virtuelle Maschine erstellt wird.

1. Klicken Sie in der Liste **Virtual Machine Size** die entsprechende Größe des virtuellen Computers an.

1. Geben Sie im Feld **Storage Account Name** ein Speicherkonto an, in dem die virtuelle Maschine erstellt wird. Stellen Sie sicher, dass es sich in der gleichen Region wie der Clouddienst befindet, den Sie verwenden möchten. Wenn Sie neuen Speicher erstellen möchten, können Sie dieses Feld leer lassen.

1. Die Aufbewahrungszeit gibt die Anzahl der Minuten an, bevor Jenkins einen Slave im Leerlauf löscht. Behalten Sie hier den Standardwert 60 bei. Wahlweise können Sie den Slave auch herunterfahren anstatt löschen, wenn er sich im Leerlauf befindet. Wählen Sie hierzu das Kontrollkästchen **Shutdown Only (Do Not Delete) After Retention Time**.

1. Wählen Sie in der Liste **Usage** die entsprechende Bedingung für die Verwendung dieses untergeordneten Knotens aus. Klicken Sie vorerst auf **Utilize this node as much as possible**.

    Zu diesem Zeitpunkt sollte das Formular in etwa so aussehen:

    ![Allgemeine Prüfpunkt-Vorlagenkonfiguration][checkpoint general template config]

    Der nächste Schritt ist das Bereitstellen von Informationen zum Betriebssystemimage, in dem der Slave erstellt werden soll.

1. Im Feld **Image Family or Id** müssen Sie angeben, welches Systemabbild auf Ihrem virtuellen Computer installiert wird. Sie können aus einer Liste von Imagefamilien auswählen oder ein benutzerdefiniertes Image angeben.

    Wenn Sie aus einer Liste von Imagefamilien auswählen möchten, geben Sie das erste Zeichen (Groß-/Kleinschreibung beachten) des Namens der Imagefamilie ein. Wenn Sie z. B. **U** eingeben, erhalten Sie eine Liste der Ubuntu Server-Familien. Nachdem Sie aus der Liste eine Auswahl getroffen haben, verwendet Jenkins die neueste Version des Systemimages aus dieser Familie bei der Bereitstellung des virtuellen Computers.

    ![Beispiel für Betriebssystem-Imageliste][OS Image list sample]

    Wenn Sie stattdessen ein vorhandenes benutzerdefiniertes Image verwenden möchten, geben Sie den Namen dieses benutzerdefinierten Images ein. Benutzerdefinierte Imagenamen werden nicht in einer Liste angezeigt. Daher müssen Sie sicherstellen, dass der Name richtig eingegeben wurde.

    Geben Sie für dieses Lernprogramm **U** ein, um eine Liste mit Ubuntu-Images anzuzeigen, und klicken Sie dann auf **Ubuntu Server 14.04 LTS**.

1. Klicken Sie in der Liste **Launch Method** auf **SSH**.

1. Kopieren Sie das folgende Skript, und fügen Sie es in das Feld **Init Script** ein.

		# Install Java

		sudo apt-get -y update

		sudo apt-get install -y openjdk-7-jdk

		sudo apt-get -y update --fix-missing

		sudo apt-get install -y openjdk-7-jdk

		# Install git

		sudo apt-get install -y git

		#Install ant

		sudo apt-get install -y ant

		sudo apt-get -y update --fix-missing

		sudo apt-get install -y ant

    Das Initialisierungsskript wird ausgeführt, nachdem der virtuelle Computer erstellt wurde. In diesem Beispiel installiert das Skript Java, Git und ant.

1. Geben Sie in die Felder **Username** und **Password** Ihre bevorzugten Werte für das Administratorkonto ein, das auf Ihrem virtuellen Computer erstellt werden soll.

1. Klicken Sie auf **Verify Template**, um zu überprüfen, ob die angegebenen Parameter gültig sind.

1. Klicken Sie auf **Speichern**.


## Erstellen eines Jenkins-Auftrags, der auf einem untergeordneten Knoten in Azure ausgeführt wird

In diesem Abschnitt erstellen Sie eine Jenkins-Aufgabe, die auf einen untergeordneten Knoten in Azure ausgeführt wird. Dazu benötigen Sie Ihr eigenes Projekt auf GitHub.

1. Klicken Sie im Jenkins-Dashboard auf **New Item**.

1. Geben Sie einen Namen für die Aufgabe ein, die Sie erstellen.

1. Klicken Sie für den Projekttyp auf **Freestyle project**.

1. Klicken Sie auf **OK**.

1. Wählen Sie auf der Konfigurationsseite der Aufgabe **Restrict where this project can be run** aus.

1. Geben Sie im Feld **Label Expression** die Bezeichnung **linux** ein. Im vorherigen Abschnitt haben Sie eine Slave-Vorlage erstellt, die Sie **linux** genannt haben. Diese wird hier angegeben.

1. Klicken Sie im Abschnitt **Build** auf **Add build step**, und wählen Sie **Execute shell** aus.

1. Bearbeiten Sie das folgende Skript, und ersetzen Sie **(Ihr GitHub-Kontoname)**, **(Name Ihres Projekts)** und **(Projektverzeichnis)** durch entsprechende Werte, und fügen Sie das bearbeitete Skript im angezeigten Textbereich ein.

		# Clone from git repo

		currentDir="$PWD"

		if [ -e (your project directory) ]; then

  			cd (your project directory)

  			git pull origin master

		else

  			git clone https://github.com/(your GitHub account name)/(your project name).git

		fi

		# change directory to project

		cd $currentDir/(your project directory)

		#Execute build task

		ant

1. Klicken Sie auf **Speichern**.

1. Zeigen Sie im Jenkins-Dashboard auf die Aufgabe, die Sie gerade erstellt haben, und klicken Sie auf den Dropdownpfeil, um Aufgabenoptionen anzuzeigen.

1. Klicken Sie auf **Build now**.

Jenkins erstellt dann einen untergeordneten Knoten mithilfe der im vorherigen Abschnitt erstellten Vorlage und führt das Skript aus, das Sie im Buildschritt für diese Aufgabe angegeben haben.

## Nächste Schritte

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Abonnementprofil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png

<!---HONumber=AcomDC_0921_2016-->