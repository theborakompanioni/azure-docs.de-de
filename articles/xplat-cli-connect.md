<properties
	pageTitle="Anmelden bei Azure über die Befehlszeilenschnittstelle | Microsoft Azure"
	description="Herstellen einer Verbindung mit Ihrem Azure-Abonnement über die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows"
	editor="tysonn"
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="multiple"
	ms.workload="multiple"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/13/2016"
	ms.author="danlep"/>

# Herstellen einer Verbindung mit einem Azure-Abonnement von der Azure Befehlszeilenschnittstelle (Azure-CLI)

Die Azure-Befehlszeilenschnittstelle beinhaltet eine Reihe von plattformübergreifenden Open Source-Befehlen für die Arbeit mit der Azure Platform. Dieser Artikel beschreibt Möglichkeiten, die Anmeldeinformationen für Ihr Azure-Konto anzugeben, um eine Verbindung der Azure-Befehlszeilenschnittstelle mit Ihrem Azure-Abonnement herzustellen. Wenn Sie die Befehlszeilenschnittstelle noch nicht installiert haben, finden Sie alle erforderlichen Informationen unter [Installieren der Azure-Befehlszeilenschnittstelle](xplat-cli-install.md). Wenn Sie über kein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](http://azure.microsoft.com/free/) erstellen.

Es gibt zwei Möglichkeiten, über die Azure-Befehlszeilenschnittstelle eine Verbindung mit Ihrem Abonnement herzustellen:

* **Anmelden bei Azure mit einem Geschäfts-, Schul- oder Unikonto oder mit einer Microsoft-Kontoidentität**: Verwenden Sie den Befehl `azure login` mit einer diesen Kontoidentitäten, um sich über Azure Active Directory zu authentifizieren. Die meisten Kunden, die neue Azure-Bereitstellungen erstellen, sollten diese Methode verwenden. Bei bestimmten Konten erfordert der Befehl `azure login`, dass Sie sich interaktiv über ein Webportal anmelden.

    Verwenden Sie den Befehl `azure login` auch, um einen Dienstprinzipal für eine Azure Active Directory-Anwendung zu authentifizieren. Dies ist zum Ausführen automatisierter Dienste nützlich.
    
    Nachdem Sie sich mit einer unterstützten Kontoidentität angemeldet haben, können Sie den Azure Resource Manager-Modus oder die CLI-Befehle des Azure-Dienstverwaltungsmodus verwenden.

* **Herunterladen und Verwenden einer Datei mit Veröffentlichungseinstellungen**: Hiermit wird ein Zertifikat auf dem lokalen Computer installiert, mit dem Sie Verwaltungsaufgaben ausführen können, solange das Abonnement und das Zertifikat gültig sind.

    Bei dieser Methode können Sie nur CLI-Befehle des Azure-Dienstverwaltungsmodus verwenden.

>[AZURE.NOTE] Bei Verwendung einer älteren Version der Azure-CLI als 0.9.10 können Sie den Befehl `azure login` nur mit Geschäfts-, Schul- oder Unikonten verwenden. Microsoft-Kontoidentitäten werden nicht unterstützt. Nach Wunsch können Sie jedoch [eine Geschäfts-, Schul- oder Uni-ID anhand Ihrer Microsoft-Konto-ID erstellen](virtual-machines/virtual-machines-windows-create-aad-work-id.md).

Hintergrundinformationen zu anderen Kontoidentitäten und Azure-Abonnements finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

## Verwenden der Azure-Anmeldung für die interaktive Authentifizierung

Verwenden Sie den Befehl `azure login` – ohne Argumente – für die interaktive Authentifizierung mit:

- einer Geschäfts-, Schul- oder Unikontoidentität (wird auch als *Organisationskonto* bezeichnet), für die Multi-Factor Authentication erforderlich ist, oder
- einer Microsoft-Kontoidentität, wenn Sie auf Befehle des Resource Manager-Modus zugreifen möchten

> [AZURE.NOTE]  In beiden Fällen werden Authentifizierung und Autorisierung mit Azure Active Directory durchgeführt. Wenn Sie eine Microsoft-Kontoidentität verwenden, greift der Anmeldeprozess auf Ihre Azure Active Directory-Standarddomäne zu. (Wenn Sie sich für ein kostenloses Azure-Konto registriert haben, ist Ihnen möglicherweise nicht bewusst, dass Azure Active Directory eine Standarddomäne für Ihr Konto erstellt hat.)

Geben Sie `azure login` ein, und befolgen Sie die unten gezeigten Anweisungen:

	azure login                                                                                                                                                                                         
	info:    Executing command login
	info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate. 

Kopieren Sie den oben angegebenen Code, und öffnen Sie einen Browser für http://aka.ms/devicelogin (oder, falls angegeben, eine andere Seite). Geben Sie den Code ein. Dann werden Sie aufgefordert, den Benutzernamen und das Kennwort für die Identität einzugeben, die Sie verwenden möchten. Wenn dieser Vorgang abgeschlossen ist, schließt die Befehlsshell den Anmeldevorgang ab. Dies sieht möglicherweise wie folgt aus:

	info:    Added subscription Visual Studio Ultimate with MSDN
	info:    Added subscription Azure Free Trial
	info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
	+
	info:    login command OK

## Verwenden der Azure-Anmeldung mit einem Benutzernamen und einem Kennwort


Verwenden Sie den Befehl `azure login` mit einem Parameter für den Benutzernamen oder mit einem Benutzernamen und einem Kennwort für die Authentifizierung, wenn Sie ein Geschäfts-, Schul- oder Unikonto verwenden möchten, für das keine Multi-Factor Authentication erforderlich ist. Im folgenden Beispiel wird der Benutzername eines Organisationskontos übergeben:

	azure login -u ahmet@contoso.onmicrosoft.com
	info:    Executing command login
	Password: *********
	|info:    Added subscription Visual Studio Ultimate with MSDN
	+
	info:    login command OK

Geben Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.

Wenn Sie sich das erste Mal mit diesen Anmeldeinformationen anmelden, werden Sie dazu aufgefordert zu bestätigen, dass ein Authentifizierungstoken im Cache gespeichert werden soll. Diese Eingabeaufforderung wird auch angezeigt, wenn Sie zuvor den Befehl `azure logout` verwendet haben (der weiter unten in diesem Artikel beschrieben wird). Um diese Eingabeaufforderung in Automatisierungsszenarien zu umgehen, führen Sie `azure login` mit dem `-q`-Parameter aus.

   

## Verwenden von „azure login“ mit einem Dienstprinzipal

Wenn Sie einen Dienstprinzipal für eine Active Directory-Anwendung erstellt haben und der Dienstprinzipal über Berechtigungen für Ihr Abonnement verfügt, können Sie den Befehl `azure login` verwenden, um den Dienstprinzipal zu authentifizieren. Je nach Szenario könnten Sie die Anmeldeinformationen des Dienstprinzipals als explizite Parameter des Befehls `azure login` oder über ein CLI-Skript bzw. Anwendungscode angeben. Sie können auch ein Zertifikat verwenden, um den Dienstprinzipal in Automatisierungsszenarien nicht interaktiv zu authentifizieren. Details und Beispiele finden Sie unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](resource-group-authenticate-service-principal.md).

## Verwenden einer Datei mit Veröffentlichungseinstellungen

Wenn Sie nur die CLI-Befehle im Azure-Dienstverwaltungsmodus benötigen (z.B. zum Bereitstellen von Azure-VMs im klassischen Bereitstellungsmodell), können Sie die Verbindung mithilfe einer Datei mit Veröffentlichungseinstellungen herstellen.

* Mit dem folgenden Befehl können Sie die **Datei mit Veröffentlichungseinstellungen für Ihr Konto herunterladen** (nur im Dienstverwaltungsmodus verfügbar):

		azure account download

    Ihr Standardbrowser wird geöffnet, und Sie werden aufgefordert, sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) anzumelden. Nach der Anmeldung wird eine `.publishsettings`-Datei heruntergeladen. Notieren Sie sich den Speicherort der Datei.

    > [AZURE.NOTE] Wenn Ihr Konto mit mehreren Azure Active Directory-Mandanten verknüpft ist, werden Sie möglicherweise aufgefordert, das Active Directory auszuwählen, für das eine Datei mit Veröffentlichungseinstellungen heruntergeladen werden soll.

    Die Active Directory-Instanz, die Sie auf der Downloadseite oder im klassischen Azure-Portal ausgewählt haben, wird zur Standardinstanz, die vom klassischen Portal und der Downloadseite verwendet wird. Nachdem eine Standardeinstellung festgelegt wurde, wird am oberen Rand der Downloadseite der Text '__click here to return to the selection page__' (Klicken Sie hier, um zur Auswahlseite zurückzukehren) angezeigt. Klicken Sie auf den bereitgestellten Link, um zur Auswahlseite zurückzukehren.

* Führen Sie den folgenden Befehl aus, um die **Datei mit den Veröffentlichungseinstellungen zu importieren**:

		azure account import <path to your .publishsettings file>

	>[AZURE.IMPORTANT]Nachdem Sie Ihre Veröffentlichungseinstellungen importiert haben, müssen Sie die Datei `.publishsettings` löschen. Sie wird nicht mehr für die Azure-Befehlszeilenschnittstelle benötigt und stellt ein Sicherheitsrisiko dar, da sie für den Zugriff auf Ihr Abonnement verwendet werden kann.

## Mehrere Abonnements

Wenn Sie mehrere Azure-Abonnements besitzen, erhalten Sie durch das Herstellen einer Verbindung mit Azure Zugriff auf alle Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Ein Abonnement wird als Standardabonnement ausgewählt und von der Azure-Befehlszeilenschnittstelle zum Ausführen von Vorgängen verwendet. Mit dem Befehl `azure account list` können Sie die Abonnements anzeigen und herausfinden, welches Abonnement das Standardabonnement ist. Die Ausgabe dieses Befehls sieht etwa so aus:

	info:    Executing command account list
	data:    Name              Id                                    Current
	data:    ----------------  ------------------------------------  -------
	data:    Azure-sub-1       ####################################  true
	data:    Azure-sub-2       ####################################  false

In der oben angeführten Liste gibt die Spalte **Aktuell** das aktuelle Standardabonnement als Azure-sub-1 an. Um das Standardabonnement zu ändern, verwenden Sie den Befehl `azure account set`, und geben Sie das Abonnement an, das als Standard verwendet werden soll. Beispiel:

	azure account set Azure-sub-2

Damit wird das Standardabonnement in "Azure-sub-2" geändert.

> [AZURE.NOTE] Die Änderung des Standardabonnements tritt sofort in Kraft, und es ist eine globale Änderung. Für weitere Befehle der Azure-Befehlszeilenschnittstelle wird das neue Standardabonnement verwendet, unabhängig davon, ob die Befehle in derselben oder einer anderen Befehlsschnittstelleninstanz ausgeführt werden.

Wenn Sie in der Azure-Befehlszeilenschnittstelle ein anderes Abonnement als das Standardabonnement verwenden, aber das aktuelle Standardabonnement nicht ändern möchten, können Sie die Option `--subscription` und den Namen des Abonnements angeben, das Sie für den Vorgang verwenden möchten.

Sobald Sie mit Ihrem Azure-Abonnement verbunden sind, können Sie beginnen, die Befehle der Azure-Befehlszeilenschnittstelle zu verwenden, um mit Azure-Ressourcen zu arbeiten.

## Befehlsmodi der Befehlszeilenschnittstelle

Die Azure-Befehlszeilenschnittstelle bietet zwei Befehlsmodi für die Arbeit mit Azure-Ressourcen, die jeweils unterschiedliche Befehle aufweisen:

* **Resource Manager-Modus**: Für die Arbeit mit Azure-Ressourcen im Resource Manager-Bereitstellungsmodell. Führen Sie zum Festlegen dieses Modus `azure config mode arm` aus.

* **Dienstverwaltungsmodus**: Für die Arbeit mit Azure-Ressourcen im klassischen Bereitstellungsmodell. Führen Sie zum Festlegen dieses Modus `azure config mode asm` aus.

Nach der Installation wird die Befehlszeilenschnittstelle im Service Management-Modus ausgeführt.

>[AZURE.NOTE]Der Resource Manager-Modus und der Dienstverwaltungsmodus schließen sich gegenseitig aus. Das heißt, dass Ressourcen, die in einem Modus erstellt wurden, nicht im anderen Modus verwaltet werden können.

## Speichern der Einstellungen für die Befehlszeilenschnittstelle

Unabhängig davon, ob Sie sich mit dem Befehl `azure login` anmelden oder Veröffentlichungseinstellungen importieren, werden Ihr Profil und die Protokolle für die CLI in einem `.azure`-Verzeichnis gespeichert, das sich in Ihrem `user`-Verzeichnis befindet. Ihr `user`-Verzeichnis wird durch das Betriebssystem geschützt; es wird jedoch empfohlen, zusätzliche Schritte zum Verschlüsseln Ihres `user`-Verzeichnisses auszuführen. Sie haben dazu folgende Möglichkeiten:

* Windows: Ändern Sie die Verzeichniseigenschaften, oder verwenden Sie BitLocker.
* Mac: Aktivieren Sie FileVault für das Verzeichnis.
* Ubuntu: Verwenden Sie die Homeverzeichnis-Verschlüsselung. Andere Linux-Distributionen bieten ähnliche Funktionen.

## Abmelden

Verwenden Sie den folgenden Befehl, um sich abzumelden:

	azure logout -u <username>

Wenn die mit dem Konto verknüpften Abonnements nur über Active Directory authentifiziert wurden, werden bei der Abmeldung die Abonnementinformationen aus dem lokalen Profil gelöscht. Wenn jedoch auch eine Datei mit Veröffentlichungseinstellungen für die Abonnements importiert wurde, dann werden durch die Abmeldung nur die auf Active Directory bezogenen Informationen aus dem lokalen Profil gelöscht.
## Nächste Schritte

* Informationen zum Verwenden von Befehlen der Azure-Befehlszeilenschnittstelle finden Sie unter [Azure-CLI-Befehle im Resource Manager-Modus](./virtual-machines/azure-cli-arm-commands.md) und unter [Befehle der Azure-Befehlszeilenschnittstelle im Dienstverwaltungsmodus](virtual-machines-command-line-tools.md).

* Wenn Sie weitere Informationen zur Azure-Befehlszeilenschnittstelle erhalten, den Quellcode herunterladen, Probleme melden oder etwas zum Projekt beitragen möchten, besuchen Sie die Webseite [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).

* Wenn Sie Probleme bei der Verwendung der Azure-Befehlszeilenschnittstelle oder Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=azurescripting).

<!---HONumber=AcomDC_0713_2016-->