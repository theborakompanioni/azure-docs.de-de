---
title: "Optionen für die Migration aus Azure RemoteApp | Microsoft Docs"
description: "Informationen zu den Optionen für die Migration aus Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 7c75fd870efb93bcf6c69ccf8a8ea9b56288c749
ms.openlocfilehash: 0744b317f595786b91afce3c9e9d59f64ceef25d


---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Optionen für die Migration aus Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Wenn Sie die Nutzung von Azure RemoteApp aufgrund der [Ankündigung der Außerbetriebnahme](https://go.microsoft.com/fwlink/?linkid=821148) beendet oder Ihre Evaluierung abgeschlossen haben, müssen Sie eine Migration aus Azure RemoteApp zu einem anderen App-Dienst durchführen. Es gibt zwei verschiedene Ansätze zur Migration: eine selbstverwaltete Bereitstellung (häufig als IaaS-Bereitstellung [Infrastructure as a Service] bezeichnet) oder das Angebot einer vollständig verwalteten Bereitstellung (auch PaaS-Bereitstellung [Platform as a Service] genannt). 

Die IaaS-Lösung ist in Ihrem Besitz und wird von Ihnen selbst betrieben, verwaltet und direkt auf virtuellen Computern (VMs) oder physischen Systemen bereitgestellt. Die vollständig verwaltete PaaS-/SaaS-Lösung ist hingegen eher Azure RemoteApp vergleichbar. Das heißt, dass ein Partner eine Dienstschicht über einer Remotinglösung bereitstellt, die Betrieb und Wartung übernimmt, während Sie als Kunde sich um einen Teil der Image- und App-Verwaltung kümmern.

[Sehen Sie sich die Azure RemoteApp-Webinare zu Migrationsoptionen an](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), oder lesen Sie hier weiter, um weitere Informationen zu erhalten (einschließlich Beispielen zu den verschiedenen Hostingoptionen).

## <a name="self-managed-iaas-solutions"></a>Selbstverwaltete Lösungen (IaaS)
### <a name="rds-on-iaas"></a>**RDS in IaaS**
Sie können eine native sitzungsbasierte Bereitstellung von Remotedesktopdiensten (RDS) unter Windows Server entweder mithilfe von RemoteApp oder von Desktops durchführen, die sich entweder lokal oder in einer gehosteten Umgebung (z.B. auf Azure-VMs) befinden. RDS-in-IaaS-Bereitstellungen eignen sich am besten für Kunden, die bereits mit RDS-Bereitstellungen vertraut sind und über entsprechende technische Kenntnisse verfügen. 

> [!NOTE]
> Sie benötigen die Volumenlizenzierung mit Software Assurance für RDS-Clientzugriffslizenzen, um diese Bereitstellungsoption nutzen zu können.
> 
> 

Die Bereitstellung von RDS auf Azure-VMs ist einfacher als je zuvor, wenn Sie Bereitstellungs- und Patchvorlagen nutzen (lesen Sie die [Übersicht](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/), und [laden Sie sie dann herunter](https://aka.ms/rdautomation)). Sie erhalten in Azure RemoteApp dieselben elastischen Skalierungsmöglichkeiten mit zum klassischen Azure-Bereitstellungsmodell (nicht zum Azure Resource Manager-Modell) gehörenden Ressourcen, indem Sie das [Skript für die automatische Skalierung](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76) verwenden, wenngleich es weitere Anpassungen und Konfigurationen gibt. Wenn Sie RDS auf Azure-VMs bereitstellen, wird Support vom [Azure-Support](https://azure.microsoft.com/support/plans/) geboten, dessen Mitarbeiter Sie bei Azure RemoteApp unterstützt haben. Kontaktieren Sie den [Azure-Support](https://azure.microsoft.com/support/plans/), um Kostenschätzungen basierend auf Ihrer bisherigen Nutzung zu erhalten. Mit dem Kostenrechner, der in Kürze veröffentlicht wird, können Sie dann auch selbst Berechnungen vornehmen.  Darüber hinaus können Sie mit VMs der N-Serie (derzeit in der privaten Preview-Phase) einen virtuellen Grafikprozessor (vGPU) hinzufügen. Weitere Informationen dazu und zum [Nutzen von RDS-Verbesserungen unter Windows Server 2016](https://myignite.microsoft.com/videos/2794) erhalten Sie in unserer Ignite-Session.   

Zur Unterstützung Ihrer Bereitstellung bieten wir detaillierte Bereitstellungsanleitungen für [Windows Server 2012 R2](http://aka.ms/rdsonazure) und [Windows Server 2016](http://aka.ms/rdsonazure2016) zusammengestellt. Der [Blog zu Remotedesktop](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) bietet stets die neuesten Informationen.

### <a name="citrix-on-iaas"></a>**Citrix in IaaS**
Eine native Citrix-Bereitstellung von XenApp oder XenDesktop auf Sitzungsbasis kann lokal oder in einer gehosteten Umgebung (z.B. auf Azure-VMs) erfolgen. 

In der detaillierten Bereitstellungsanleitung [Citrix XA 7.6 on Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx) finden Sie weitere Informationen. Erfahren Sie mehr über [Citrix in Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), z.B. zum Kostenrechner. Sie finden auch Daten zur [Kontaktaufnahme mit Citrix](http://citrix.com/English/contact/index.asp), um Ihre Optionen zu besprechen.

## <a name="fully-managed-paassaas-offerings"></a>Vollständig verwaltete Bereitstellungen (PaaS/SaaS)
### <a name="citrix-cloud"></a>**Citrix Cloud**
[Vorhandenen Cloudlösung von Citrix](https://www.citrix.com/products/citrix-cloud/), deren Architektur mit der von Citrix XenApp Express identisch ist. Citrix bietet Bestandskunden von Azure RemoteApp einen [Aktionsrabatt von&50;%](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/). 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (technische Preview-Phase)**
[Registrieren Sie sich für die technische Preview-Version](http://now.citrix.com/remoteapp), und sehen Sie sich die zugehörige [Ignite-Session](https://myignite.microsoft.com/videos/2792) (Start bei Minute 20:30) an. Die Architektur von XenApp Express ist mit der von Citrix Cloud identisch, bietet aber eine vereinfachte Benutzeroberfläche für die Verwaltung sowie andere Features und Funktionen, die mit Azure RemoteApp vergleichbar sind. 

Erfahren Sie mehr über [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix Service Provider-Programm**
Das Citrix Service Provider-Programm erleichtert Dienstanbietern das Bereitstellen der Einfachheit des virtuellen Cloud Computings für kleine- und mittelständische Unternehmen, denen die gewünschten Dienste in einem einfachen nutzungsbasierten Zahlungsmodell angeboten werden. Citrix-Dienstanbieter bauen ihr Microsoft SPLA-Geschäft aus und erhöhen ihre Investitionen in die RDS-Plattform für alle Geräte. Sie bieten ortsunabhängigen Zugriff, eine sehr umfassende Anwendungsunterstützung, eine funktionsreiche Umgebung sowie mehr Sicherheit und Skalierbarkeit. Citrix-Dienstanbieter ziehen wiederum weitere Abonnenten an, steigern die Zufriedenheit der Kunden und senken ihre Betriebskosten. [Erfahren Sie mehr](http://www.citrix.com/products/service-providers.html), oder [finden Sie einen Partner](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Von Microsoft gehostete Dienstanbieter**
Hostingpartner bieten meist einen vollständig verwalteten gehosteten Windows-Desktop- und Anwendungsdienst. Dies kann Folgendes umfassen: Das Verwalten der Azure-Ressourcen, Betriebssysteme, Anwendungen und des Helpdesks unter Verwendung der Lizenzverträge des Partners mit Microsoft und anderer Softwareanbieter sowie einen Lizenzvertrag für Dienstanbieter, der den Weiterverkauf von SALs (Subscriber Access Licenses, Abonnentenzugriffslizenzen) erlaubt. Es folgen Details und Kontaktinformationen zu einigen Hostern, die sich auf das Unterstützen von Kunden bei ihrer Azure RemoteApp-Migration spezialisiert haben. Sehen Sie sich die [aktuelle Liste gehosteter Dienstanbieter an](http://aka.ms/rdsonazurecertified), die den Lernpfad und die Bewertung „RDS in IaaS“ durchlaufen haben.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) ist auf ISVs spezialisiert, die sich im Übergang zur Cloud befinden oder die ihre aktuellen Cloudinstallationen optimieren möchten. ASPEX bietet eine Vielzahl von verwalteten, DevOps- und Beratungsdiensten.  

Hauptstandort: Antwerpen, Belgien

Wirkungsbereich: Westeuropa

Partnerstatus: [Silber](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft-Clouddienstanbieter: Ja

Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides

Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](https://www.aspex.be/en/azure-remote-apps)

**Kontakt:**

* Telefonnummer: +3232202198
* E-Mail: [info@aspex.be](mailto:info@aspex.be)
* Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (Plattformname: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) ist eine Automatisierungsplattform für IT-Unternehmen zum Vereinfachen, Optimieren und Skalieren der Migration und Bereitstellung von Remotedesktops, Remoteanwendungen und Infrastruktur in der Microsoft Azure-Cloud. 

Die MyCloudIT-Plattform reduziert die Bereitstellungszeit um 95%, die Azure-Kosten um 30% und ermöglicht über einige wenige Tastenanschläge das Verlagern der gesamten IT-Infrastruktur des Kunden in die Cloud. Partner können jetzt Kunden in einem globalen Dashboard verwalten, Endbenutzer weltweit wie nie zuvor betreuen und den Umsatz steigern, und zwar ohne zusätzlichen Verwaltungsaufwand oder umfangreiche Azure-Schulungen.  

Hauptstandort: Dallas, Texas, USA

Wirkungsbereich: weltweit

Partnerstatus: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft-Clouddienstanbieter: Ja

Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides

Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](https://mycloudit.com/remote-app-microsoft/)

**Kontakt:**

* Brian Garoutte, VP of Business Development
  
   Telefonnummer: 972-218-0741
  
   E-Mail: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) ist auf das Bereitstellen gehosteter Desktoplösungen spezialisiert und bietet Kunden auf der ganzen Welt mithilfe von Azure und seinen eigenen Rechenzentren auf Microsoft-Technologie basierende Desktop- und ISV-Anwendungsumgebungen.

Hauptstandorte: London (Großbritannien), Singapur, Houston (Texas, USA)

Wirkungsbereich: weltweit

Partnerstatus: Gold

Microsoft-Clouddienstanbieter: Ja

Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides

Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](http://www.acuutech.com/ara-migration/)

**Kontakt:**

* Großbritannien:
  
  5/6 York House, Langston Road,
  
  Loughton, Essex IG10 3TQ
  
  Telefonnummer: +44 (0) 20 8502 2155
* Singapur:
  
  100 Cecil Street, #09-02, 
  
  The Globe, Singapore 069532
  
  Telefonnummer: +65 6709 4933
* Nordamerika: 
  
  3601 S. Sandman St.
  
  Suite 200, Houston, TX 77098
  
  Telefonnummer: +1 713 691 0800

#### <a name="saasplaza"></a>**SaaSplaza**
[SaaSplaza](http://www.saasplaza.com/) bietet ein vollständiges Microsoft Dynamics-Portfolio (NAV, AX, GP, SL, CRM) sowie eine private und öffentliche Cloud (Azure).

Primärer Standort: Niederlande

Wirkungsbereich: weltweit

Partnerstatus: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)

Microsoft-Clouddienstanbieter: Ja

Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides

**Kontakt:**

- EMEA:

   Prins Mauritslaan 29-35

   71 LP Badhoevedorp

   Niederlande

   Telefon: +31 20 547 8060 

- Amerika:

   171 Saxony Road, Suite 105

   Encinitas, CA 92024

   San Diego

   USA

   Telefon: +1 858 385 8900 

- APAC:

   105 Cecil Street
   
   \#11-08, The Octagon

   Singapur 069534

   Singapur
   
   Telefon – Singapur: +65 6222 6591

   Telefon – Australien: +61 2 8310 5568 
   
   Telefon – Neuseeland: +64 4 488 0321

## <a name="need-more-help"></a>Benötigen Sie weitere Hilfe?
Brauchen Sie für Ihre Entscheidung noch Hilfe, oder haben Sie weitere Fragen? Verwenden Sie eine der folgenden Methoden zum Anfordern von Hilfe. 

1. Senden Sie eine E-Mail an [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2. Kontaktieren Sie den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Öffnen Sie einen [Vorgang beim Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3. Rufen Sie uns an. [Suchen Sie die Telefonnummer eines Vertriebsmitarbeiters in Ihrer Nähe](https://azure.microsoft.com/overview/sales-number/).




<!--HONumber=Feb17_HO2-->


