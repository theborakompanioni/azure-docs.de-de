<properties
    pageTitle="Compute-Optionen in Azure – Cloud Services | Microsoft Azure"
    description="Erfahren Sie mehr über die Compute-Hostingoptionen in Azure und deren Funktionsweise: App Service, Cloud Services und Virtual Machines"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="adegeo"/>


# <a name="should-i-choose-cloud-services-or-something-else?"></a>Sollte ich mich für Clouddienste oder eine andere Lösung entscheiden?

Ist Azure Cloud Services das Richtige für Sie? Azure bietet verschiedene Hostingmodelle für die Ausführung von Anwendungen. Jedes dieser Modelle bietet unterschiedliche Dienste. Dadurch können Sie genau das Modell auswählen, das sich am besten für Ihr Vorhaben eignet.

[AZURE.INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>
## <a name="tell-me-about-cloud-services"></a>Informationen zu Cloud Services

Cloud Services ist ein Beispiel für [Platform-as-a-Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Diese Technologie unterstützt genau wie [App Service](../app-service-web/app-service-web-overview.md) skalierbare und zuverlässige Anwendungen mit geringen Betriebskosten. Cloud Services wird genau wie App Service auf virtuellen Computern gehostet. Allerdings haben Sie bei Cloud Services mehr Kontrolle über die virtuellen Computer. Sie können Ihre eigene Software auf virtuellen Clouddienstcomputern installieren und per Remotezugriff auf sie zugreifen.

![cs_diagramcs_diagram](./media/cloud-services-choose-me/diagram.png)

Mehr Kontrolle bedeutet leider oft auch weniger Benutzerfreundlichkeit. Wenn Sie die zusätzlichen Verwaltungsoptionen nicht benötigen, ist es im Vergleich zu Cloud Services meist schneller und einfacher, Webanwendungen in Web-Apps in App Service zu implementieren.

Diese Technologie bietet zwei geringfügig unterschiedliche VM-Optionen: Instanzen von *Webrollen* führen eine Variante von Windows Server mit IIS aus, während Instanzen von *Workerrollen* die gleiche Windows Server-Variante ohne IIS ausführen. Cloud Services-Anwendungen verwenden dieselbe Kombination dieser zwei Optionen.

Eine beliebige Kombination dieser zwei leicht unterschiedlichen VM Hostingoptionen sind in einem Clouddienst verfügbar:

* **Webrolle**  
   Führt Windows Server mit Ihrer Web-App aus, die automatisch auf IIS bereitgestellt wird.

* **Workerrolle**  
  : Führt Windows-Server ohne IIS aus.

Einfache Anwendungen verwenden beispielsweise lediglich eine Webrolle, während komplexere Anwendungen eine Webrolle für die Verarbeitung eingehender Benutzeranforderungen verwenden und die durch diese Anforderungen generierte Arbeit an Workerrollen delegieren. (Diese Kommunikation erfolgt über [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) oder über [Azure-Warteschlangen](../storage/storage-introduction.md).)

Die obige Abbildung zeigt, dass alle VMs in einer einzigen Anwendung im gleichen Clouddienst laufen. Aus diesem Grund greifen die Benutzer über eine einzige öffentliche IP-Adresse auf die Anwendung zu, und die Anforderungen werden automatisch auf die einzelnen VMs  verteilt. Die virtuellen Computer in einer Cloud Services-Anwendung werden von der Plattform so [skaliert und bereitgestellt](cloud-services-how-to-scale.md) , dass Single Points of Failure vermieden werden.

Wichtig: Anwendungen werden zwar auf virtuellen Computern ausgeführt, Cloud Services stellt jedoch PaaS (und nicht IaaS) bereit. Das können Sie sich beispielsweise wie folgt vorstellen: Mit IaaS (zum Beispiel Azure Virtual Machines) erstellen und konfigurieren Sie zunächst die Ausführungsumgebung für Ihre Anwendung und stellen die Anwendung anschließend in dieser Umgebung bereit. In diesem Fall sind Sie für die Verwaltung dieser Umgebung verantwortlich und müssen beispielsweise neue gepatchte Versionen des Betriebssystems auf den einzelnen virtuellen Computern installieren. Im Gegensatz dazu ist die Umgebung bei PaaS gewissermaßen bereits vorhanden. Somit müssen Sie in diesem Fall nur noch Ihre Anwendung bereitstellen. Sie müssen sich weder um die Verwaltung der Plattform noch um das Aufspielen neuer Betriebssystemversionen kümmern.

## <a name="scaling-and-management"></a>Skalierung und Verwaltung
Mit Cloud Services erstellen Sie keine virtuellen Computer. Stattdessen teilen Sie Azure mithilfe einer Konfigurationsdatei mit, wie viele Instanzen Sie benötigen (beispielsweise **drei Webrolleninstanzen** und **zwei Workerrolleninstanzen**), und die Plattform erstellt diese Instanzen dann für Sie.  Sie wählen zwar die [Größe](cloud-services-sizes-specs.md) der zugrunde liegenden virtuellen Computer aus, müssen diese aber nicht selbst erstellen. Wenn Ihre Anwendung eine höhere Last zu bewältigen hat, können Sie bei Azure weitere virtuelle Computer anfordern. Nimmt die Last wieder ab, können Sie die Instanzen herunterfahren, sodass sie keine weiteren Kosten verursachen.

Cloud Services-Anwendungen werden Benutzern üblicherweise in zwei Schritten bereitgestellt: Zunächst führt ein Entwickler den [Upload der Anwendung](cloud-services-how-to-create-deploy.md) in den Stagingbereich der Plattform durch. Wenn der Entwickler die Anwendung live schalten möchte, kann er sie über das Azure-Verwaltungsportal in den Produktionsmodus versetzen. Der [Wechsel zwischen Staging und Produktion](cloud-services-nodejs-stage-application.md) erfolgt ohne Ausfallzeit. Dadurch können laufende Anwendungen ohne Beeinträchtigung der Benutzer auf neue Versionen aktualisiert werden.

## <a name="monitoring"></a>Überwachung
Cloud Services bietet außerdem Überwachungsfunktionen. Genau wie bei Azure Virtual Machines werden Ausfälle von physischen Servern erkannt und die entsprechenden virtuellen Computer auf einem anderen Server neu gestartet. Neben Hardwarefehlern erkennt Cloud Services aber auch Ausfälle von virtuellen Computern und Anwendungen. Im Gegensatz zu Virtual Machines verfügt Cloud Services in jeder Web- und Workerrolle über einen Agent. Dadurch können bei einem Ausfall neue virtuelle Computer und Anwendungsinstanzen gestartet werden.

Das PaaS-Konzept von Cloud Services hat noch weitere Auswirkungen. So müssen etwa Anwendungen für diese Technologie so entwickelt werden, dass sie auch dann ordnungsgemäß funktionieren, wenn eine beliebige Web- oder Workerrolle ausfällt. Aus diesem Grund dürfen Cloud Services-Anwendungen ihren Zustand nicht im Dateisystem der eigenen virtuellen Computer speichern. Im Gegensatz zu virtuellen Computern, die mit Azure Virtual Machines erstellt wurden, sind Schreibvorgänge auf virtuellen Cloud Services-Computern nicht persistent, da sie über keinen Virtual Machines-Datenträger verfügen. Cloud Services-Anwendungen müssen daher sämtliche Zustandsinformationen in SQL-Datenbank, Blobs, Tabellen oder auf andere externe Speichermedien schreiben. Auf diese Weise erstellte Anwendungen zeichnen sich durch eine bessere Skalierbarkeit und Fehlerresistenz aus – zwei wichtige Aspekte von Cloud Services.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Clouddienst-App in .NET](cloud-services-dotnet-get-started.md)  
[Erstellen einer Clouddienst-App in Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Erstellen einer Clouddienst-App in PHP](../cloud-services-php-create-web-role.md)  
[Erstellen einer Clouddienst-App in Python](cloud-services-python-ptvs.md)



<!--HONumber=Oct16_HO2-->


