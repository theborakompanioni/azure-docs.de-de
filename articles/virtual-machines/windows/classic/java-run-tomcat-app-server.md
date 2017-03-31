---
title: "Ausführen eines Java-Anwendungsservers auf einem klassischen virtuellen Azure-Computer | Microsoft-Dokumentation"
description: "Dieses Tutorial verwendet Ressourcen verwendet, die mit dem klassischen Bereitstellungsmodell erstellt wurden, und zeigt, wie Sie einen virtuellen Windows-Computer erstellen und zum Ausführen eines Apache Tomcat-Anwendungsservers konfigurieren."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 6e02f42613808bcb13c0057e9f8fcc1c02273e77
ms.lasthandoff: 03/25/2017


---
# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Ausführen eines Java-Anwendungsservers auf einem virtuellen Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde
> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Eine Resource Manager-Vorlage zum Bereitstellen einer Web-App mit Java 8 und Tomcat finden Sie [hier](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).

Mit Azure können Sie einen virtuellen Computer nutzen, um Serverfunktionen bereitzustellen. Beispielsweise kann ein unter Azure ausgeführter virtueller Computer so konfiguriert werden, dass er einen Java-Anwendungsserver wie Apache Tomcat hostet.

Nachdem Sie diesen Leitfaden durchgearbeitet haben, werden Sie besser verstehen, wie ein virtueller Computer unter Azure erstellt wird und wie er für die Ausführung eines Java-Anwendungsservers konfiguriert wird. Sie führen die folgenden Aufgaben aus:

* Erstellen eines virtuellen Computers, auf dem bereits ein Java Development Kit (JDK) installiert ist.
* Ausführen einer Remoteanmeldung an Ihrem virtuellen Computer.
* Installieren eines Java-Anwendungsservers – Apache Tomcat – auf dem virtuellen Computer
* Erstellen eines Endpunkts für den virtuellen Computer
* Öffnen eines Ports in der Firewall für den Anwendungsserver

Die abgeschlossene Installation hat zum Ergebnis, dass Tomcat auf einem virtuellen Computer ausgeführt wird.

![Virtueller Computer mit Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>So erstellen Sie einen virtuellen Computer
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.  
2. Klicken Sie auf **Neu**, klicken Sie auf **Compute**, klicken Sie dann auf **Alles anzeigen** in **Ausgewählte Apps**.
3. Klicken Sie auf **JDK**, klicken Sie auf **JDK 8** im Bereich **JDK**.  
   Für den Fall, dass Sie Legacyanwendungen haben, die nicht in JDK 8 ausgeführt werden können, sind VM-Images verfügbar, die **JDK 6** und **JDK 7** unterstützen.
4. Wählen Sie im Bereich „JDK 8“ die Option **Klassisch** aus, und klicken Sie dann auf **Erstellen**.
5. Auf dem Blatt **Grundeinstellungen**:
   1. Geben Sie einen Namen für den virtuellen Computer an.
   2. Geben Sie im Feld **Benutzername** einen Namen für den Administrator ein. Notieren Sie sich diesen Namen und das zugehörige Kennwort, das im nächsten Feld folgt. Sie benötigen diese, wenn Sie sich remote beim virtuellen Computer anmelden.
   3. Geben Sie ein Kennwort in das Feld **Neues Kennwort** ein, und geben Sie das Kennwort erneut in das Feld **Kennwort bestätigen** ein. Dieses Kennwort wird für das Administratorkonto verwendet.
   4. Wählen Sie das entsprechende **Abonnement** aus.
   5. Klicken Sie für die **Ressourcengruppe** auf **Neu erstellen**, und geben Sie den Namen einer neuen Ressourcengruppe ein. Oder klicken Sie auf **Vorhandene verwenden**, und wählen Sie eine der verfügbaren Ressourcengruppen aus.
   6. Wählen Sie einen Standort aus, an sich dem der virtuelle Computer befindet, etwa **USA, Süden-Mitte**.
6. Klicken Sie auf **Weiter**.
7. Wählen Sie auf dem Blatt **VM-Imagegröße** die Option **A1 Standard** oder ein anderes geeignetes Image aus.
8. Klicken Sie auf **Auswählen**.

9. Klicken Sie auf dem Blatt **Einstellungen** auf **OK**. Sie können die Standardwerte verwenden, die von Azure bereitgestellt werden.  
10. Klicken Sie auf dem Blatt **Zusammenfassung** auf **OK**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>So melden Sie sich von einem Remotestandort aus bei Ihrem virtuellen Computer an
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Virtuelle Computer (klassisch)**. Klicken Sie ggf. auf **Weitere Dienste** in der unteren linken Ecke unter den Dienstkategorien. Der Eintrag **Virtuelle Computer (klassisch)** wird in der Gruppe **Compute** aufgelistet.
3. Klicken Sie auf den Namen des virtuellen Computers, an dem Sie sich anmelden möchten.
4. Nachdem der virtuelle Computer gestartet wurde, können über ein oben im Bereich angezeigtes Menü Verbindungen hergestellt werden.
5. Klicken Sie auf **Verbinden**.
6. Befolgen Sie die Anweisungen, um eine Verbindung mit dem virtuellen Computer herzustellen. Üblicherweise speichern oder öffnen Sie die RDP-Datei, die die Verbindungsdetails enthält. Sie müssen möglicherweise die url:port-Kombination als letzten Teil der ersten Zeile der RDP-Datei kopieren und ihn in eine Anwendung für die Remoteanmeldung einfügen.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>So installieren Sie einen Java-Anwendungsserver auf dem virtuellen Computer
Sie können einen Java-Anwendungsserver auf den virtuellen Computer kopieren oder ihn über ein Installationsprogramm installieren.

In diesem Tutorial wird Tomcat als der zu installierende Java-Anwendungsserver verwendet.

1. Sobald Sie an dem virtuellen Computer angemeldet sind, öffnen Sie eine Browsersitzung auf [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).
2. Doppelklicken Sie auf den Link für den **Windows Installer-Dienst (32-Bit/64-Bit)**. Mit diesem Verfahren wird Tomcat als Windows-Dienst installiert.
3. Starten Sie das Installationsprogramm, wenn Sie dazu aufgefordert werden.
4. Befolgen Sie im Setup-Assistenten für **Apache Tomcat** die Anweisungen für die Installation von Tomcat. Für dieses Lernprogramm können die Standardeinstellungen übernommen werden. Wenn das Dialogfeld **Completing the Apache Tomcat Setup Wizard** angezeigt wird, können Sie optional das Kontrollkästchen **Run Apache Tomcat** aktivieren, um Tomcat gleich zu starten. Klicken Sie auf **Fertig stellen** , um die Installation von Tomcat abzuschließen.

## <a name="to-start-tomcat"></a>So starten Sie Tomcat

Sie können Tomcat manuell starten, indem Sie eine Eingabeaufforderung auf dem virtuellen Computer öffnen und den Befehl **net&nbsp;start&nbsp;Tomcat8** ausführen.

Sobald Tomcat ausgeführt wird, können Sie auf Tomcat zugreifen, indem Sie die URL <http://localhost:8080> im Browser des virtuellen Computers eingeben.

Um von externen Computern aus festzustellen, ob Tomcat ausgeführt wird, müssen Sie einen Endpunkt erstellen und einen Port öffnen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>So erstellen Sie einen Endpunkt für den virtuellen Computer
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Virtuelle Computer (klassisch)**.
3. Klicken Sie auf den Namen des virtuellen Computers, auf dem der Java-Anwendungsserver ausgeführt wird.
4. Klicken Sie auf **Endpunkte**.
5. Klicken Sie auf **Hinzufügen**.
6. Führen Sie im Dialogfeld **Endpunkt hinzufügen** folgende Schritte aus:
   1. Geben Sie einen Namen für den Endpunkt an. Beispiel: **HttpIn**.
   2. Wählen Sie **TCP** als Protokoll aus.
   3. Geben Sie als öffentlichen Port **80** an.
   4. Geben Sie als privaten Port **8080** an.
   5. Wählen Sie **Deaktiviert** für die Floating IP-Adresse aus.
   6. Nehmen Sie keine Änderungen an der Zugriffssteuerungsliste vor.
   7. Klicken Sie auf die Schaltfläche **OK**, um das Dialogfeld zu schließen und den Endpunkt zu erstellen.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>So öffnen Sie einen Port in der Firewall für den virtuellen Computer
1. Melden Sie sich am virtuellen Computer an.
2. Klicken Sie unter Windows auf **Start**.
3. Klicken Sie auf **Systemsteuerung**.
4. Klicken Sie nacheinander auf **System und Sicherheit**, **Windows-Firewall** und **Erweiterte Einstellungen**.
5. Klicken Sie auf **Eingehende Regeln** und dann auf **Neue Regel**.  
   ![Neue eingehende Regel][NewIBRule]
6. Wählen Sie für **Regeltyp** die Option **Port** aus, und klicken Sie dann auf **Weiter**.  
   ![Neue eingehende Regel – Port][NewRulePort]
7. Wählen Sie im Bildschirm **Protokoll und Ports** die Option **TCP** aus, geben Sie **8080** unter **Bestimmter lokaler Port** ein, und klicken Sie dann auf **Weiter**.  
  ![Neue eingehende Regel][NewRuleProtocol]
8. Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie anschließend auf **Weiter**.
   ![Neue eingehende Regel – Aktion][NewRuleAction]
9. Achten Sie im Bildschirm **Profil** darauf, dass **Domäne**, **Privat** und **Öffentlich** ausgewählt sind, und klicken Sie dann auf **Weiter**.
   ![Neue eingehende Regel – Profil][NewRuleProfile]
10. Geben Sie im Bildschirm **Name** einen Namen für die Regel an, beispielsweise **HttpIn** (der Name der Regel muss jedoch nicht mit dem Namen des Endpunkts übereinstimmen). Klicken Sie anschließend auf **Fertig stellen**.  
    ![Neue eingehende Regel – Name][NewRuleName]

An diesem Punkt sollte Ihre Tomcat-Website in einem externen Browser angezeigt werden. Geben Sie in die Adressleiste des Browsers eine URL in der Form **http://*Ihr\_DNS\_Name*.cloudapp.net**ein, wobei***Ihr\_DNS\_Name*** der DNS-Name ist, den Sie beim Erstellen des virtuellen Computers angegeben haben.

## <a name="application-lifecycle-considerations"></a>Überlegungen zum Lebenszyklus von Anwendungen
* Sie können Ihr eigenes Archiv für Web-Anwendungen (WAR) erstellen und im Ordner **webapps** hinzufügen. Erstellen Sie z. B. ein elementares dynamisches JSP-Webprojekt (Java Service Page), und exportieren Sie das Projekt als WAR-Datei. Kopieren Sie die WAR-Datei im nächsten Schritt in den Apache Tomcat-Ordner **ebapps** auf dem virtuellen Computer, und führen Sie die Datei dann in einem Browser aus.
* Beim Installieren des Tomcat-Diensts wird standardmäßig der manuelle Start festgelegt. Über das Snap-In "Dienste" können Sie zum automatischen Start wechseln. Starten Sie das Snap-In „Dienste“, indem Sie unter Windows auf **Start**, **Verwaltung** und dann auf **Dienste** klicken. Doppelklicken Sie auf den **Apache Tomcat**-Dienst, und legen Sie den **Starttyp** auf **Automatisch** fest.

    ![Festlegen, dass ein Dienst automatisch gestartet wird][service_automatic_startup]

    Der automatische Start von Tomcat bietet den Vorteil, dass der Dienst gestartet wird, wenn ein Neustart des virtuellen Computer erfolgt (zum Beispiel nach der Installation von Softwareupdates, die einen Neustart erfordern).

## <a name="next-steps"></a>Nächste Schritte
Sie können sich mit den anderen Diensten beschäftigen (beispielsweise Azure Storage, Service Bus und SQL-Datenbank), die Sie möglicherweise in Ihre Java-Anwendungen einbinden möchten. Sehen Sie sich die Informationen an, die Sie im [Java Developer Center](https://azure.microsoft.com/develop/java/) finden.

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from the "To create an ednpoint for your virtual mache" 3/17/2017,
     to use the new portal.
6. In the **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In the **New endpoint details** dialog box:
-->

