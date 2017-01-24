---
title: Tomcat auf einem virtuellen Computer | Microsoft Docs
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
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: 75388bba3084099fcea8d9ec1f3a2c1e909e914f


---
# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Ausführen eines Java-Anwendungsservers auf einem virtuellen Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Eine Resource Manager-Vorlage zum Bereitstellen einer Web-App mit Java 8 und Tomcat finden Sie [hier](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).

Mit Azure können Sie einen virtuellen Computer nutzen, um Serverfunktionen bereitzustellen. Beispielsweise kann ein unter Azure ausgeführter virtueller Computer so konfiguriert werden, dass er einen Java-Anwendungsserver wie Apache Tomcat hostet. Nachdem Sie diesen Leitfaden durchgearbeitet haben, werden Sie besser verstehen, wie ein virtueller Computer unter Azure erstellt wird und wie er für die Ausführung eines Java-Anwendungsservers konfiguriert wird.

Sie erhalten Informationen zu folgenden Themen:

* Erstellen eines virtuellen Computers, auf dem bereits ein Java Development Kit (JDK) installiert ist.
* Ausführen einer Remoteanmeldung an Ihrem virtuellen Computer.
* Installieren eines Java-Anwendungsservers auf dem virtuellen Computer
* Erstellen eines Endpunkts für den virtuellen Computer
* Öffnen eines Ports in der Firewall für den Anwendungsserver

Für dieses Lernprogramm wird ein Apache Tomcat-Anwendungsserver auf einem virtuellen Computer installiert. Das Ergebnis des Installationsvorgangs ist eine Tomcat-Installation, die der Folgenden ähnelt.

![Virtueller Computer mit Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>So erstellen Sie einen virtuellen Computer
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie nacheinander auf **Neu**, **Compute**, **Virtueller Computer** und **Aus Katalog**.
3. Wählen Sie im Dialogfeld **Image ds virtuellen Computers auswählen** die Option **JDK 7 Windows Server 2012** aus.
   Beachten Sie, dass **JDK 6 Windows Server 2012** verfügbar ist, wenn Ihre Legacyanwendungen nicht in JDK 7 ausgeführt werden können.
4. Klicken Sie auf **Weiter**.
5. Gehen Sie im Dialogfeld **Konfiguration des virtuellen Computers** wie folgt vor:
   1. Geben Sie einen Namen für den virtuellen Computer an.
   2. Geben Sie die Größe für den virtuellen Computer an.
   3. Geben Sie im Feld **Benutzername** einen Namen für den Administrator ein. Merken Sie sich diesen Namen und das als Nächstes eingegebene Kennwort. Sie benötigen diese Daten, wenn Sie sich von einem Remotestandort aus an dem virtuellen Computer anmelden.
   4. Geben Sie im Feld **Neues Kennwort** ein Kennwort ein, und geben Sie dieses erneut im Feld **Kennwort bestätigen** ein. Dies ist das Kennwort für das Administratorkonto.
   5. Klicken Sie auf **Weiter**.
6. Gehen Sie im nächsten Dialogfeld **Konfiguration des virtuellen Computers** wie folgt vor:
   1. Verwenden Sie für den **Clouddienst** die Standardeinstellung **Neuen Clouddienst erstellen**.
   2. Der Wert für **DNS-Name des Clouddiensts** muss auf cloudapp.net eindeutig sein. Ändern Sie wenn nötig diesen Wert, sodass Azure angibt, dass er eindeutig ist.
   3. Geben Sie eine Region, eine Affinitätsgruppe oder ein virtuelles Netzwerk an. Geben Sie für dieses Lernprogramm als Region **West-USA**an.
   4. Wählen Sie unter **Speicherkonto** die Option **Automatisch generiertes Speicherkonto verwenden** aus.
   5. Wählen Sie unter **Verfügbarkeitsgruppe** die Option **(Keine)** aus.
   6. Klicken Sie auf **Weiter**.
7. Gehen Sie im letzten Dialogfeld **Konfiguration des virtuellen Computers** wie folgt vor:
   1. Akzeptieren Sie die Standardeinträge für Endpunkte.
   2. Klicken Sie auf **Fertig stellen**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>So melden Sie sich von einem Remotestandort aus bei Ihrem virtuellen Computer an
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie auf **Virtuelle Computer**.
3. Klicken Sie auf den Namen des virtuellen Computers, an dem Sie sich anmelden möchten.
4. Nachdem der virtuelle Computer gestartet wurde werden unten auf der Seite über ein Popupmenü Verbindungen zugelassen.
5. Klicken Sie auf **Verbinden**.
6. Befolgen Sie die Anweisungen, um eine Verbindung mit dem virtuellen Computer herzustellen. Diese sollte das Speichern oder Öffnen der RDP-Datei beinhalten, die die Verbindungsdetails enthält. Sie müssen möglicherweise die url:port-Kombination als letzten Teil der ersten Zeile der RDP-Datei kopieren und ihn in eine Anwendung für die Remoteanmeldung einfügen.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>So installieren Sie einen Java-Anwendungsserver auf dem virtuellen Computer
Sie können einen Java-Anwendungsserver auf den virtuellen Computer kopieren oder ihn über ein Installationsprogramm installieren.

Für dieses Lernprogramm wird Tomcat installiert.

1. Sobald Sie an dem virtuellen Computer angemeldet sind, öffnen Sie eine Browsersitzung auf [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Doppelklicken Sie auf den Link für den **Windows Installer-Dienst (32-Bit/64-Bit)**. Mit diesem Verfahren wird Tomcat als Windows-Dienst installiert.
3. Starten Sie das Installationsprogramm, wenn Sie dazu aufgefordert werden.
4. Befolgen Sie im Setup-Assistenten für **Apache Tomcat** die Anweisungen für die Installation von Tomcat. Für dieses Lernprogramm können die Standardeinstellungen übernommen werden. Wenn das Dialogfeld **Completing the Apache Tomcat Setup Wizard** angezeigt wird, können Sie optional das Kontrollkästchen **Run Apache Tomcat** aktivieren, um Tomcat gleich zu starten. Klicken Sie auf **Fertig stellen** , um die Installation von Tomcat abzuschließen.

## <a name="to-start-tomcat"></a>So starten Sie Tomcat
Wenn Sie im Dialogfeld **Completing the Apache Tomcat Setup Wizard** nicht angegeben haben, dass Tomcat gestartet werden soll, öffnen Sie eine Eingabeaufforderung auf dem virtuellen Computer, und führen Sie folgenden Befehl aus: **net start Tomcat7**.

Öffnen Sie im Browser des virtuellen Computers die Adresse <http://localhost:8080>. Sie sollten nun sehen, dass Tomcat ausgeführt wird.

Um von externen Computern aus festzustellen, ob Tomcat ausgeführt wird, müssen Sie einen Endpunkt erstellen und einen Port öffnen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>So erstellen Sie einen Endpunkt für den virtuellen Computer
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie auf **Virtuelle Computer**.
3. Klicken Sie auf den Namen des virtuellen Computers, auf dem der Java-Anwendungsserver ausgeführt wird.
4. Klicken Sie auf **Endpunkte**.
5. Klicken Sie auf **Hinzufügen**.
6. Stellen Sie im Dialogfeld **Endpunkt hinzufügen** sicher, dass das Kontrollkästchen **Eigenständigen Endpunkt hinzufügen** aktiviert ist, und klicken Sie dann auf **Weiter**.
7. Gehen Sie im Dialogfeld **New endpoint details** wie folgt vor:
   1. Geben Sie einen Namen für den Endpunkt an. Beispiel: **HttpIn**.
   2. Geben Sie als Protokoll **TCP** an.
   3. Geben Sie als öffentlichen Port **80** an.
   4. Geben Sie als privaten Port **8080** an.
   5. Klicken Sie auf die Schaltfläche **Fertig stellen** , um das Dialogfeld zu schließen. Der Endpunkt wird jetzt erstellt.

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

An dieser Stelle sollte es nun möglich sein, die Tomcat-Website über einen externen Browser anzuzeigen. Dabei wird eine URL in der Form **http://*your\_DNS\_name*.cloudapp.net** verwendet, wobei ***your\_DNS\_name*** der DNS-Name ist, den Sie beim Erstellen des virtuellen Computers angegeben haben.

## <a name="application-lifecycle-considerations"></a>Überlegungen zum Lebenszyklus von Anwendungen
* Sie können Ihr eigenes Archiv für Web-Anwendungen (WAR) erstellen und im Ordner **webapps** hinzufügen. Erstellen Sie beispielsweise ein dynamisches JSP-Basiswebprojekt (Java Service Page), und exportieren Sie dieses als WAR-Datei. Kopieren Sie dann die WAR-Datei in den Apache Tomcat-Ordner **webapps** auf dem virtuellen Computer, und führen Sie die Datei in einem Browser aus.
* Beim Installieren des Tomcat-Diensts wird standardmäßig der manuelle Start festgelegt. Über das Snap-In "Dienste" können Sie zum automatischen Start wechseln. Starten Sie das Snap-In „Dienste“, indem Sie unter Windows auf **Start**, **Verwaltung** und dann auf **Dienste** klicken. Doppelklicken Sie auf den **Apache Tomcat**-Dienst, und legen Sie den **Starttyp** auf **Automatisch** fest.
  
    ![Festlegen, dass ein Dienst automatisch gestartet wird][service_automatic_startup]
  
    Der automatische Start von Tomcat bietet den Vorteil, dass der Dienst gestartet wird, wenn ein Neustart des virtuellen Computer erfolgt (zum Beispiel nach der Installation von Softwareupdates, die einen Neustart erfordern).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu anderen Diensten wie beispielsweise Azure Storage, Service Bus und SQL-Datenbank, die Sie möglicherweise zusammen mit Ihren Java-Anwendungen verwenden möchten, finden Sie im [Java Developer Center](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png



<!--HONumber=Dec16_HO1-->


