---
title: "Anmelden bei Azure über die Befehlszeilenschnittstelle | Microsoft Docs"
description: "Herstellen einer Verbindung mit Ihrem Azure-Abonnement über die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 31efab60690b54faf7992251fcd01e307c4464f2
ms.lasthandoff: 03/21/2017


---
# <a name="log-in-to-azure-from-the-azure-cli"></a>Anmelden bei Azure über die Azure-Befehlszeilenschnittstelle (CLI)
Die Azure-Befehlszeilenschnittstelle umfasst eine Reihe von plattformübergreifenden Open Source-Befehlen für die Arbeit mit Azure-Ressourcen. Dieser Artikel beschreibt verschiedene Möglichkeiten zum Angeben der Anmeldeinformationen für Ihr Azure-Konto, um für die Azure-Befehlszeilenschnittstelle eine Verbindung mit Ihrem Azure-Abonnement herzustellen:

* Führen Sie den CLI-Befehl `azure login` aus, um sich über Azure Active Directory zu authentifizieren. Diese Methode ermöglicht den Zugriff auf CLI-Befehle in beiden [Befehlsmodi](#cli-command-modes). Wenn Sie den Befehl ohne zusätzliche Optionen ausführen, werden Sie von `azure login` aufgefordert, die Anmeldung interaktiv über ein Webportal fortzusetzen. Weitere Befehlsoptionen für `azure login` finden Sie in den Szenarien dieses Artikels oder durch Eingabe von `azure login --help`.
* Falls Sie nur die CLI-Befehle des Azure Service Management-Modus verwenden möchten (was bei den meisten neuen Bereitstellungen nicht empfehlenswert ist), können Sie eine Datei mit Veröffentlichungseinstellungen herunterladen und installieren.

Wenn Sie die Befehlszeilenschnittstelle noch nicht installiert haben, finden Sie alle erforderlichen Informationen unter [Installieren der Azure-Befehlszeilenschnittstelle](cli-install-nodejs.md). Wenn Sie über kein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](http://azure.microsoft.com/free/) erstellen.

Hintergrundinformationen zu anderen Kontoidentitäten und Azure-Abonnements finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Szenario 1: Interaktive Azure-Anmeldung
Für bestimmte Konten müssen Sie bei Verwendung der Befehlszeilenschnittstelle `azure login` ausführen und den Anmeldeprozess anschließend über ein Webportal in einem Webbrowser fortsetzen. Dies wird als *interaktive Anmeldung* bezeichnet. Dies ist häufig bei Geschäfts-, Schul- oder Unikonten (auch *Organisationskonten* genannt) der Fall, die für eine mehrstufige Authentifizierung eingerichtet sind. Verwenden Sie die interaktive Anmeldung außerdem mit Ihrem Microsoft-Konto, wenn Sie Befehle im Resource Manager-Modus verwenden möchten.

Für die interaktive Anmeldung müssen Sie lediglich `azure login` (ohne Optionen) eingeben, wie im folgenden Beispiel zu sehen:

```
azure login
```                                                                                             

Die Ausgabe sieht in etwa wie folgt aus:

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate.
```
Kopieren Sie den Code aus der Befehlsausgabe, und rufen Sie in einem Browser die URL „http://aka.ms/devicelogin“ oder eine andere Seite auf (sofern angegeben). (Den Browser können Sie entweder auf dem gleichen Computer oder auf einem anderen Computer oder Gerät öffnen.) Geben Sie den Code ein. Dann werden Sie aufgefordert, den Benutzernamen und das Kennwort für die Identität einzugeben, die Sie verwenden möchten. Wenn dieser Vorgang abgeschlossen ist, schließt die Befehlsshell den Anmeldevorgang ab. Dies sieht möglicherweise wie folgt aus:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> Bei der interaktiven Anmeldung werden Authentifizierung und Autorisierung mit Azure Active Directory durchgeführt. Wenn Sie eine Microsoft-Kontoidentität verwenden, greift der Anmeldeprozess auf Ihre Azure Active Directory-Standarddomäne zu. (Wenn Sie sich für ein kostenloses Azure-Konto registriert haben, hat Azure Active Directory automatisch eine Standarddomäne für Ihr Konto erstellt.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Szenario 2: Azure-Anmeldung mit Benutzername und Kennwort
Verwenden Sie für die Authentifizierung den Befehl `azure login` mit dem Benutzernamenparameter (`-u`), wenn Sie ein Geschäfts-, Schul- oder Unikonto verwenden möchten, bei dem keine mehrstufige Authentifizierung erforderlich ist. Sie werden in der Befehlszeile zur Eingabe des Kennworts aufgefordert. (Das Kennwort kann optional auch als zusätzlicher Parameter des Befehls `azure login` übergeben werden.) Im folgenden Beispiel wird der Benutzername eines Organisationskontos übergeben:

    azure login -u myUserName@contoso.onmicrosoft.com

Anschließend werden Sie zur Eingabe Ihres Kennworts aufgefordert:

    info:    Executing command login
    Password: *********

Danach wird der Anmeldeprozess abgeschlossen.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Wenn Sie sich das erste Mal mit diesen Anmeldeinformationen anmelden, werden Sie dazu aufgefordert zu bestätigen, dass ein Authentifizierungstoken im Cache gespeichert werden soll. Diese Eingabeaufforderung wird auch angezeigt, wenn Sie zuvor den weiter unten in diesem Artikel beschriebenen Befehl `azure logout` verwendet haben. Um diese Eingabeaufforderung in Automatisierungsszenarien zu umgehen, führen Sie `azure login` mit dem `-q`-Parameter aus.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Szenario 3: Azure-Anmeldung mit einem Dienstprinzipal
Wenn Sie für eine Active Directory-Anwendung einen Dienstprinzipal erstellen und dieser über Berechtigungen für Ihr Abonnement verfügt, können Sie den Dienstprinzipal mithilfe des Befehls `azure login` authentifizieren. Je nach Szenario können Sie die Anmeldeinformationen des Dienstprinzipals als explizite Parameter des Befehls `azure login` angeben. Der folgende Befehl übergibt beispielsweise den Dienstprinzipalnamen und die Active Directory-Mandanten-ID:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Anschließend werden Sie zur Angabe des Kennworts aufgefordert. In Automatisierungsszenarien können Sie die Anmeldeinformationen auch per Befehlszeilenskript oder Anwendungscode angeben oder den Dienstprinzipal mithilfe eines Zertifikats auf nicht interaktive Weise authentifizieren. Details und Beispiele finden Sie unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Szenario 4: Verwenden einer Datei mit Veröffentlichungseinstellungen
Wenn Sie nur die CLI-Befehle im Azure-Dienstverwaltungsmodus benötigen (z.B. zum Bereitstellen von Azure-VMs im klassischen Bereitstellungsmodell), können Sie die Verbindung mithilfe einer Datei mit Veröffentlichungseinstellungen herstellen. Bei dieser Methode wird auf Ihrem lokalen Computer ein Zertifikat installiert, das es Ihnen ermöglicht, Verwaltungsaufgaben auszuführen, solange das Abonnement und das Zertifikat gültig sind.

* Vergewissern Sie sich zum **Herunterladen der Datei mit Veröffentlichungseinstellungen**, dass sich die Befehlszeilenschnittstelle im Dienstverwaltungsmodus befindet. Geben Sie hierzu `azure config mode asm` ein. Führen Sie dann den folgenden Befehl aus:

        azure account download

Ihr Standardbrowser wird geöffnet, und Sie werden aufgefordert, sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)anzumelden. Nach der Anmeldung wird eine `.publishsettings` -Datei heruntergeladen. Notieren Sie sich den Speicherort der Datei.

> [!NOTE]
> Wenn Ihr Konto mit mehreren Azure Active Directory-Mandanten verknüpft ist, werden Sie möglicherweise aufgefordert, das Active Directory auszuwählen, für das eine Datei mit Veröffentlichungseinstellungen heruntergeladen werden soll.
>
>

Die Active Directory-Instanz, die Sie auf der Downloadseite oder im klassischen Azure-Portal ausgewählt haben, wird zur Standardinstanz, die vom klassischen Portal und der Downloadseite verwendet wird. Nachdem eine Standardeinstellung festgelegt wurde, wird am oberen Rand der Downloadseite der Text **click here to return to the selection page** (Klicken Sie hier, um zur Auswahlseite zurückzukehren) angezeigt. Klicken Sie auf den bereitgestellten Link, um zur Auswahlseite zurückzukehren.

* Führen Sie den folgenden Befehl aus, um die **Datei mit den Veröffentlichungseinstellungen zu importieren**:

        azure account import <path to your .publishsettings file>

> [!IMPORTANT]
> Nachdem Sie Ihre Veröffentlichungseinstellungen importiert haben, sollten Sie die Datei `.publishsettings` löschen. Sie wird nicht mehr für die Azure-Befehlszeilenschnittstelle benötigt und stellt ein Sicherheitsrisiko dar, da sie für den Zugriff auf Ihr Abonnement verwendet werden kann.
>
>

## <a name="cli-command-modes"></a>Befehlsmodi der Befehlszeilenschnittstelle
Die Azure-Befehlszeilenschnittstelle bietet zwei Befehlsmodi für die Arbeit mit Azure-Ressourcen, die jeweils unterschiedliche Befehle aufweisen:

* **Resource Manager-Modus** : Für die Arbeit mit Azure-Ressourcen im Resource Manager-Bereitstellungsmodell. Führen Sie zum Festlegen dieses Modus `azure config mode arm`aus.
* **Dienstverwaltungsmodus** : Für die Arbeit mit Azure-Ressourcen im klassischen Bereitstellungsmodell. Führen Sie zum Festlegen dieses Modus `azure config mode asm`aus.

Nach der Installation befindet sich die aktuelle Version der Befehlszeilenschnittstelle standardmäßig im Resource Manager-Modus.

> [!NOTE]
> Der Resource Manager-Modus und der Dienstverwaltungsmodus schließen sich gegenseitig aus. Das heißt, dass Ressourcen, die in einem Modus erstellt wurden, nicht im anderen Modus verwaltet werden können.
>
>

## <a name="multiple-subscriptions"></a>Mehrere Abonnements
Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch Herstellen einer Verbindung mit Azure Zugriff auf alle Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Ein Abonnement wird als Standardabonnement ausgewählt und von der Azure-Befehlszeilenschnittstelle zum Ausführen von Vorgängen verwendet. Die Abonnements können mithilfe des Befehls `azure account list` angezeigt werden (einschließlich des aktuellen Standardabonnements). Die Ausgabe dieses Befehls sieht etwa so aus:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

In der obigen Liste gibt die Spalte **Current** das aktuelle Standardabonnement als „Azure-sub-1“ an. Um das Standardabonnement zu ändern, verwenden Sie den Befehl `azure account set` , und geben Sie das Abonnement an, das als Standard verwendet werden soll. Beispiel:

    azure account set Azure-sub-2

Damit wird das Standardabonnement in "Azure-sub-2" geändert.

> [!NOTE]
> Die Änderung des Standardabonnements tritt sofort und global in Kraft. Bei weiteren Befehlen der Azure-Befehlszeilenschnittstelle wird dann das neue Standardabonnement verwendet – ganz gleich, ob die Befehle in der gleichen Befehlsschnittstelleninstanz oder in einer anderen Instanz ausgeführt werden.
>
>

Wenn Sie in der Azure-Befehlszeilenschnittstelle ein anderes Abonnement als das Standardabonnement verwenden, aber das aktuelle Standardabonnement nicht ändern möchten, können Sie die Option `--subscription` und den Namen des Abonnements angeben, das Sie für den Vorgang verwenden möchten.

Sobald Sie mit Ihrem Azure-Abonnement verbunden sind, können Sie beginnen, die Befehle der Azure-Befehlszeilenschnittstelle zu verwenden, um mit Azure-Ressourcen zu arbeiten.

## <a name="storage-of-cli-settings"></a>Speichern der Einstellungen für die Befehlszeilenschnittstelle
Ihr Profil und die Protokolle für die Befehlszeilenschnittstelle werden in einem `.azure`-Verzeichnis gespeichert, das sich in Ihrem `user`-Verzeichnis befindet. Dabei spielt es keine Rolle, ob Sie sich mit dem Befehl `azure login` anmelden oder Veröffentlichungseinstellungen importieren. Ihr `user`-Verzeichnis wird von Ihrem Betriebssystem geschützt. Es empfiehlt sich jedoch, das `user`-Verzeichnis zusätzlich zu verschlüsseln. Sie haben dazu folgende Möglichkeiten:

* Windows: Ändern Sie die Verzeichniseigenschaften, oder verwenden Sie BitLocker.
* Mac: Aktivieren Sie FileVault für das Verzeichnis.
* Ubuntu: Verwenden Sie die Homeverzeichnis-Verschlüsselung. Andere Linux-Distributionen bieten ähnliche Funktionen.

## <a name="logging-out"></a>Abmelden
Verwenden Sie den folgenden Befehl, um sich abzumelden:

    azure logout -u <username>

Wenn die mit dem Konto verknüpften Abonnements nur über Active Directory authentifiziert werden, werden die Abonnementinformationen bei der Abmeldung aus dem lokalen Profil gelöscht. Wenn jedoch auch eine Datei mit Veröffentlichungseinstellungen für die Abonnements importiert wurde, werden durch die Abmeldung nur die auf Active Directory bezogenen Informationen aus dem lokalen Profil gelöscht.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Verwenden von Befehlen der Azure-Befehlszeilenschnittstelle finden Sie unter [Azure-CLI-Befehle im Resource Manager-Modus](virtual-machines/azure-cli-arm-commands.md) und unter [Befehle der Azure-Befehlszeilenschnittstelle im Dienstverwaltungsmodus](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* Wenn Sie weitere Informationen zur Azure-Befehlszeilenschnittstelle erhalten, den Quellcode herunterladen, Probleme melden oder etwas zum Projekt beitragen möchten, besuchen Sie die Webseite [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).
* Wenn Sie Probleme bei der Verwendung der Azure-Befehlszeilenschnittstelle oder Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

