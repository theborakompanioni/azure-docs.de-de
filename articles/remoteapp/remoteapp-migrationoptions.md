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
ms.date: 04/26/2017
ms.author: mbaldwin
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 9ab63124e2521ee1922d15c1e388c54d50eb8301
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Optionen für die Migration aus Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .


Wenn Sie die Nutzung von Azure RemoteApp aufgrund der [Ankündigung der Außerbetriebnahme](https://go.microsoft.com/fwlink/?linkid=821148) beendet oder Ihre Evaluierung abgeschlossen haben, müssen Sie eine Migration aus Azure RemoteApp zu einem anderen App-Dienst durchführen. Es gibt zwei verschiedene Ansätze zur Migration: eine selbstverwaltete Bereitstellung (häufig als IaaS-Bereitstellung [Infrastructure as a Service] bezeichnet) oder das Angebot einer vollständig verwalteten Bereitstellung (auch PaaS-Bereitstellung [Platform as a Service] genannt). 

Die IaaS-Lösung ist in Ihrem Besitz und wird von Ihnen selbst betrieben, verwaltet und direkt auf virtuellen Computern (VMs) oder physischen Systemen bereitgestellt. Die vollständig verwaltete PaaS-/SaaS-Lösung ist hingegen eher Azure RemoteApp vergleichbar. Das heißt, dass ein Partner eine Dienstschicht über einer Remotinglösung bereitstellt, die Betrieb und Wartung übernimmt, während Sie als Kunde sich um einen Teil der Image- und App-Verwaltung kümmern.

[Sehen Sie sich die Azure RemoteApp-Webinare zu Migrationsoptionen an](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), oder lesen Sie hier weiter, um weitere Informationen zu erhalten (einschließlich Beispielen zu den verschiedenen Hostingoptionen).

## <a name="self-managed-iaas-solutions"></a>Selbstverwaltete Lösungen (IaaS)
### <a name="rds-on-iaas"></a>**RDS in IaaS**
Sie können eine native sitzungsbasierte Bereitstellung von Remotedesktopdiensten (RDS) unter Windows Server entweder mithilfe von RemoteApp oder von Desktops durchführen, die sich entweder lokal oder in einer gehosteten Umgebung (z.B. auf Azure-VMs) befinden. RDS-in-IaaS-Bereitstellungen eignen sich am besten für Kunden, die bereits mit RDS-Bereitstellungen vertraut sind und über entsprechende technische Kenntnisse verfügen. 

> [!NOTE]
> Sie benötigen die Volumenlizenzierung mit Software Assurance für RDS-Clientzugriffslizenzen, um diese Bereitstellungsoption nutzen zu können.

Die Bereitstellung von RDS auf Azure-VMs ist einfacher als je zuvor, wenn Sie Bereitstellungs- und Patchvorlagen nutzen (lesen Sie die [Übersicht](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/), und [laden Sie sie dann herunter](https://aka.ms/rdautomation)). Sie erhalten in Azure RemoteApp dieselben elastischen Skalierungsmöglichkeiten mit zum klassischen Azure-Bereitstellungsmodell (nicht zum Azure Resource Manager-Modell) gehörenden Ressourcen, indem Sie das [Skript für die automatische Skalierung](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76) verwenden, wenngleich es weitere Anpassungen und Konfigurationen gibt. Wenn Sie RDS auf Azure-VMs bereitstellen, wird Support vom [Azure-Support](https://azure.microsoft.com/support/plans/) geboten, dessen Mitarbeiter Sie bei Azure RemoteApp unterstützt haben. Kontaktieren Sie den [Azure-Support](https://azure.microsoft.com/support/plans/), um Kostenschätzungen basierend auf Ihrer bisherigen Nutzung zu erhalten. Mit dem Kostenrechner, der in Kürze veröffentlicht wird, können Sie dann auch selbst Berechnungen vornehmen.  Darüber hinaus können Sie mit VMs der N-Serie (derzeit in der privaten Preview-Phase) einen virtuellen Grafikprozessor (vGPU) hinzufügen. Weitere Informationen dazu und zum [Nutzen von RDS-Verbesserungen unter Windows Server 2016](https://myignite.microsoft.com/videos/2794) erhalten Sie in unserer Ignite-Session.   

Zur Unterstützung Ihrer Bereitstellung bieten wir detaillierte Bereitstellungsanleitungen für [Windows Server 2012 R2](http://aka.ms/rdsonazure) und [Windows Server 2016](http://aka.ms/rdsonazure2016) zusammengestellt. Der [Blog zu Remotedesktop](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) bietet stets die neuesten Informationen.

### <a name="citrix-on-iaas"></a>**Citrix in IaaS**
Eine native Citrix-Bereitstellung von XenApp oder XenDesktop auf Sitzungsbasis kann lokal oder in einer gehosteten Umgebung (z.B. auf Azure-VMs) erfolgen. 

In der detaillierten Bereitstellungsanleitung [Citrix XA 7.6 on Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx) finden Sie weitere Informationen. Erfahren Sie mehr über [Citrix in Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), z.B. zum Kostenrechner. Sie finden auch Daten zur [Kontaktaufnahme mit Citrix](http://citrix.com/English/contact/index.asp), um Ihre Optionen zu besprechen.

## <a name="fully-managed-paassaas-offerings"></a>Vollständig verwaltete Bereitstellungen (PaaS/SaaS)

### <a name="citrix-xenapp-essentials-released-april-2017"></a>Citrix XenApp Essentials (April 2017 veröffentlicht)
Citrix XenApp Essentials ist jetzt im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials) verfügbar. Es handelt sich um einen neuen Dienst zur Anwendungsvirtualisierung, der die Leistung und Flexibilität der Citrix-Cloudplattform mit der einfachen, bindenden und benutzerfreundlichen Vision von Microsoft Azure RemoteApp vereint. 

Vorhandene Azure RemoteApp-Kunden können sich [für eine kostenlose Testversion registrieren](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).  Hinweis: Nur der Citrix-Benutzerdienst ist kostenlos. Azure-Compute- und -Speicherinstanzen sind kostenpflichtig

Weitere Informationen:
- [Migrieren von Azure RemoteApp zu Citrix XenApp Essentials](remoteapp-migrate-citrix.md)
- [Citrix und Microsoft](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- [Citrix XenApp Essentials-Präsentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a>Citrix Cloud XenApp-Dienst und XenDesktop-Dienst 

[Citrix Cloud XenApp-Dienst und XenDesktop-Dienst](https://www.citrix.com/products/citrix-cloud/services.html) stellen die beste Lösung für die Bereitstellung von Apps und Desktops sowie von Funktionen zur erweiterten Verwaltung und Überwachung dar. 

#### <a name="conexlink-platform-name-mycloudit"></a>Conexlink (Plattformname: MyCloudIT)
[MyCloudIT](https://mycloudit.com) ist eine Automatisierungsplattform für IT-Unternehmen zum Vereinfachen, Optimieren und Skalieren der Migration und Bereitstellung von Remotedesktops, Remoteanwendungen und Infrastruktur in der Microsoft Azure-Cloud. 

Die MyCloudIT-Plattform reduziert die Bereitstellungszeit um 95 %, die Azure-Kosten um 30 % und ermöglicht über einige wenige Tastenanschläge das Verlagern der gesamten IT-Infrastruktur des Kunden in die Cloud. Partner können jetzt Kunden in einem globalen Dashboard verwalten, Endbenutzer weltweit wie nie zuvor betreuen und den Umsatz steigern, und zwar ohne zusätzlichen Verwaltungsaufwand oder umfangreiche Azure-Schulungen.  

> Hauptstandort: Dallas, Texas, USA
> 
> Wirkungsbereich: weltweit
> 
> Partnerstatus: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)
> 
> Microsoft-Clouddienstanbieter: Ja
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides
> 
> Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](https://mycloudit.com/remote-app-microsoft/)
> 
> Brian Garoutte, VP of Business Development
> 
> Telefonnummer: 972-218-0741
>   
> E-Mail: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

### <a name="frame"></a>Frame

IT-Organisationen in Wirtschaft und Regierung, verwaltete Dienstanbieter und führende Softwarehersteller nutzen Frame für das Erstellen und Verwalten ihrer sicheren softwarebezogenen Arbeitsbereiche in der Cloud. Für kleine wie große Organisationen vereinfacht Frame den Benutzerzugriff auf Windows-Anwendungen über beliebige Browser von jedem Gerät aus. Die Frame-Plattform enthält alles, was ein Administrator zum Bereitstellen von Anwendungen aus der Cloud benötigt, einschließlich der Azure-Infrastruktur und der RDS-Lizenzen (die Nutzung eigener Azure-Konten und Lizenzen ist optional). 

Weitere Informationen zu [Frame in Azure](https://www.fra.me/ara) 

> Hauptstandort: San Mateo, Kalifornien, USA
>
> Wirkungsbereich: weltweit
>
> Microsoft-Partner: Ja
> 
> Telefon: 1-480-269-4668

### <a name="awingu"></a>Awingu
Awingu bietet eine einfache Online-Arbeitsbereichslösung, die Legacy-Apps, SaaS und Dokumente eines HTML5-Browsers ausführt. Daher ist es möglich, beliebige Anwendungen auf beliebigen Gerätetypen sicher zur Verfügung zu stellen. Für SaaS-Dienste sind eine Vielzahl von Optionen für einmaliges Anmelden verfügbar. Zudem können verschiedene Dateisysteme (Cloud) tief in Ihren Arbeitsbereich integriert werden. Neben der vollständigen Mobilität bietet Ihnen der umfangreiche Online-Arbeitsbereich von Awingu die optimale Sicherheit durch genau abgestimmte Steuerelemente (z. B. herunterladen/hochladen), die vollständige Überwachung der Nutzung, Multi-Factor Authentication (z. B. Azure MFA), Sitzungsaufzeichnung und mehr. Awingu ermöglicht die Freigabe von Dokument- und Anwendungssitzungen für die optimierte und sichere Zusammenarbeit.
Die Lösung von Awingu ist für mehrere Mandanten, Multi-AD und offene APIs ausgelegt. Es wird von kleinen und großen Unternehmen, Clouddienstanbietern und [ISVs](http://www.isv2saas.com) verwendet. Diese Kunden schätzen insbesondere die Benutzerfreundlichkeit, die einfache Installation sowie die geringen Gesamtbetriebskosten.

Awingu All-in-One ist mit zwei integrierten gleichzeitigen Benutzern über den [Azure Marketplace verfügbar](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm). Weitere Lizenzen stehen über eine [breite Palette von Händlern und Vertriebspartnern](http://www.awingu.com/reseller) zur Verfügung.

Weitere Informationen finden Sie unter [Awingu on als Alternative zu Azure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).


> Hauptstandort: Belgien
> 
> Wirkungsbereiche: EMEA, Nordamerika und Brasilien
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides 
> 
> **Global**:
> 
> Arnaud Marlière, CMO
> 
> E-Mail: [arnaud@awingu.com](mailto:arnaud@awingu.com)
> 
> Telefon: +1 646 583 3025
> 
> **Zentrale in Belgien**:
> 
> Ottergemsesteenweg-Zuid 808 B44
> 
> 9000 Gent
> 
> E-Mail: [info@awingu.com](mailto:info@awingu.com) 
> 
> Telefon: +32 9 296 40 11
> 
> **USA**:
> 
> 7th floor, 1177 Ave of the Americas,
> 
> New York, NY 10036
> 
> E-Mail: [info.us@awingu.com](mailto:info.us@awingu.com)

### <a name="microsoft-hosted-service-provider"></a>Von Microsoft gehostete Dienstanbieter
Hostingpartner bieten meist einen vollständig verwalteten gehosteten Windows-Desktop- und Anwendungsdienst. Dies kann Folgendes umfassen: Das Verwalten der Azure-Ressourcen, Betriebssysteme, Anwendungen und des Helpdesks unter Verwendung der Lizenzverträge des Partners mit Microsoft und anderer Softwareanbieter sowie einen Lizenzvertrag für Dienstanbieter, der den Weiterverkauf von SALs (Subscriber Access Licenses, Abonnentenzugriffslizenzen) erlaubt. Es folgen Details und Kontaktinformationen zu einigen Hostern, die sich auf das Unterstützen von Kunden bei ihrer Azure RemoteApp-Migration spezialisiert haben. Sehen Sie sich die [aktuelle Liste gehosteter Dienstanbieter an](http://aka.ms/rdsonazurecertified), die den Lernpfad und die Bewertung „RDS in IaaS“ durchlaufen haben.  

### <a name="citrix-service-provider-program"></a>Citrix Service Provider-Programm
Das Citrix Service Provider-Programm erleichtert Dienstanbietern das Bereitstellen der Einfachheit des virtuellen Cloud Computings für kleine- und mittelständische Unternehmen, denen die gewünschten Dienste in einem einfachen nutzungsbasierten Zahlungsmodell angeboten werden. Citrix-Dienstanbieter bauen ihr Microsoft SPLA-Geschäft aus und erhöhen ihre Investitionen in die RDS-Plattform für alle Geräte. Sie bieten ortsunabhängigen Zugriff, eine sehr umfassende Anwendungsunterstützung, eine funktionsreiche Umgebung sowie mehr Sicherheit und Skalierbarkeit. Citrix-Dienstanbieter ziehen wiederum weitere Abonnenten an, steigern die Zufriedenheit der Kunden und senken ihre Betriebskosten. [Erfahren Sie mehr](http://www.citrix.com/products/service-providers.html), oder [finden Sie einen Partner](https://www.citrix.com/buy/partnerlocator.html).

#### <a name="acuutech"></a>Acuutech
[Acuutech](http://www.acuutech.com) ist auf das Bereitstellen gehosteter Desktoplösungen spezialisiert und bietet Kunden auf der ganzen Welt mithilfe von Azure und seinen eigenen Rechenzentren auf Microsoft-Technologie basierende Desktop- und ISV-Anwendungsumgebungen.

> Hauptstandorte: London (Großbritannien), Singapur, Houston (Texas, USA)
> 
> Wirkungsbereich: weltweit
> 
> Partnerstatus: Gold
> 
> Microsoft-Clouddienstanbieter: Ja
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides
> 
> Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](http://www.acuutech.com/ara-migration/)
> 
> **Vereinigtes Königreich**:
>   
> 5/6 York House, Langston Road,
>   
> Loughton, Essex IG10 3TQ
>   
> Telefonnummer: +44 (0) 20 8502 2155
> 
> **Singapur**:
>   
> 100 Cecil Street, #09-02, 
>   
> The Globe, Singapore 069532
> 
> Telefonnummer: +65 6709 4933
>   
> **Nordamerika**:
>   
> 3601 S. Sandman St.
>   
> Suite 200, Houston, TX 77098
>   
> Telefonnummer: +1 713 691 0800

#### <a name="aspex"></a>ASPEX
[ASPEX](http://www.aspex.be/en) ist auf ISVs spezialisiert, die sich im Übergang zur Cloud befinden oder die ihre aktuellen Cloudinstallationen optimieren möchten. ASPEX bietet eine Vielzahl von verwalteten, DevOps- und Beratungsdiensten.  

> Hauptstandort: Antwerpen, Belgien
> 
> Wirkungsbereich: Westeuropa
> 
> Partnerstatus: [Silber](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)
> 
> Microsoft-Clouddienstanbieter: Ja
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides
> 
> Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](https://www.aspex.be/en/azure-remote-apps)
> 
> Telefonnummer: +3232202198
> 
> E-Mail: [info@aspex.be](mailto:info@aspex.be)
> 
> Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="caasecom"></a>Caase.com
Mit [Caase.com](http://www.caase.com/) unterstützt Unternehmen, lokale Behörden, nichtstaatliche Einrichtungen und Institutionen aus dem Gesundheitswesen auf ihrem Weg in Richtung einer intelligenteren Nutzung der Microsoft Cloud. Seien Sie überall mit jedem Gerät zu geringen IT-Kosten produktiv und geschützt. Caase.com ist ein wahrer Spezialist für Microsoft Office365, Azure, Enterprise Mobility + Security und Windows. Caase.com erstellt mit Beratung, Migrationsdiensten, Übernahmeprogrammen, Schulungen, Verwaltung und Unterstützung von unserer Seite eine optimierte und sichere Plattform für die Zusammenarbeit für die Mitarbeiter, Partner und Lieferanten beider Kunden.
Caase.com ist der führende Kopf des Azure-Remotearbeitsbereichs (mobiler Arbeitsbereich) und des Digital Workplace (Social Intranet). Beide Lösungen – durch Übernahme realisiert – sind das Fundament, das dafür sorgt, dass Benutzer dieser Lösungen auf angenehme Weise, erfolgreich und effizient in die Microsoft Cloud gelangen.
Einen unterstützenden Film in niederländischer Sprache finden Sie hier: http://caase.com/over-ons/

> Wirkungsbereich: Sitz in den Niederlanden, globale Reichweite
> 
> Partnerstatus: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)
> 
> Microsoft-Clouddienstanbieter: Ja
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides
> 
> Azure RemoteApp-Migrationslösungen: Ja, [erfahren Sie mehr](http://caase.com/diensten/microsoft-azure/).
> 
> 
> Niederlande:
> 
> Rigtersbleek-Zandvoort 10 (De Spinnerij)
> 
> 7521 BE, Enschede
> 
> Telefon: +31 (0) 88 4320 000


#### <a name="nerdio"></a>Nerdio
[Nerdio for Azure](http://getnerdio.com/nfa/) ist eine IT-Automatisierungsplattform, die eine unglaublich einfache Bereitstellung, Verwaltung und Optimierung ganzer IT-Umgebungen in der Microsoft Cloud ermöglicht. Damit können virtuelle Desktops, Remoteanwendungen und Server in wenigen Stunden eingerichtet werden. Die Umgebung lässt sich mit dem Nerdio Admin Portal mit maximal drei Klicks verwalten. Durch die intelligente automatische Skalierung können 40 bis 60% Azure-IaaS-Ressourcen eingespart werden.

> Hauptstandort: Chicago, IL Wirkungsbereich: weltweit Partnerstatus: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft-Clouddienstanbieter: Ja
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides
> 
> Azure RemoteApp-Migrationslösungen: Ja
> 
> 
> 8001 Lincoln Ave
> 
> Suite 212
> 
> Skokie, IL 60077
> 
> USA
> 
> (844) 4NERDIO Durchwahl 6
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a>**SaaSplaza**
[SaaSplaza](http://www.saasplaza.com/) bietet ein vollständiges Microsoft Dynamics-Portfolio (NAV, AX, GP, SL, CRM) sowie eine private und öffentliche Cloud (Azure).

> Primärer Standort: Niederlande
> 
> Wirkungsbereich: weltweit
> 
> Partnerstatus: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)
> 
> Microsoft-Clouddienstanbieter: Ja
> 
> Anbieten sitzungsbasierter RemoteApp- und Remotedesktop-Lösungen: Ja, beides
> 
> **EMEA**:
> 
> Prins Mauritslaan 29-35
> 
> 71 LP Badhoevedorp
> 
> Niederlande
> 
> Telefon: +31 20 547 8060 
> 
>  **Amerika**:
> 
> 171 Saxony Road, Suite 105
> 
> Encinitas, CA 92024
> 
> San Diego
> 
> USA
> 
> Telefon: +1 858 385 8900 
> 
> **APAC**:
> 
> 105 Cecil Street
>    
> \#11-08, The Octagon
> 
> Singapur 069534
> 
> Singapur
>   
> Telefon – Singapur: +65 6222 6591
> 
> Telefon – Australien: +61 2 8310 5568 
>    
> Telefon – Neuseeland: +64 4 488 0321
> 
## <a name="need-more-help"></a>Benötigen Sie weitere Hilfe?
Brauchen Sie für Ihre Entscheidung noch Hilfe, oder haben Sie weitere Fragen? Verwenden Sie eine der folgenden Methoden zum Anfordern von Hilfe. 

1. Senden Sie eine E-Mail an [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2. Kontaktieren Sie den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Öffnen Sie einen [Vorgang beim Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3. Rufen Sie uns an. [Suchen Sie die Telefonnummer eines Vertriebsmitarbeiters in Ihrer Nähe](https://azure.microsoft.com/overview/sales-number/).


