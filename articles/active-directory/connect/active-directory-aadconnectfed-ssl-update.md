---
title: "Azure AD Connect: Aktualisieren des SSL-Zertifikats für eine Active Directory-Verbunddienste-Farm (AD FS) | Microsoft-Dokumentation"
description: "Dieses Dokument erläutert die Schritte zum Aktualisieren des SSL-Zertifikats einer AD FS-Farm mithilfe von Azure AD Connect."
services: active-directory
keywords: "Azure AD Connect, Aktualisieren von AD FS-SSL, Aktualisieren des AD FS-Zertifikats, Ändern des AD FS-Zertifikats, neues AD FS-Zertifikat, AD FS-Zertifikat, Aktualisieren des AD FS-SSL-Zertifikats, Aktualisieren des SSL-Zertifikats von AD FS, Konfigurieren des AD FS-SSL-Zertifikats, AD FS, SSL, Zertifikat, AD FS-Dienstkommunikationszertifikat, Aktualisieren des Verbunds, Konfigurieren des Verbunds, AAD Connect"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2016
ms.author: anandy
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: d8f4181d7b51109cc5a2e205a40225a7763d0be4
ms.lasthandoff: 03/07/2017


---    

# <a name="update-the-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Aktualisieren des SSL-Zertifikats für eine Active Directory-Verbunddienste-Farm (AD FS)

## <a name="overview"></a>Übersicht
In diesem Artikel wird beschrieben, wie Azure AD Connect verwendet werden kann, um das SSL-Zertifikat für die Active Directory-Verbunddienste-Farm (AD FS) zu aktualisieren.  Wenn die Anmeldemethode in Azure AD Connect auf AD FS festgelegt ist, kann das Azure AD Connect-Tool verwendet werden, um das SSL-Zertifikat für die AD FS-Farm über alle Verbünde und WAP-Server hinweg in 3 einfachen Schritten zu aktualisieren:

![Drei Schritte](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
>Weitere Informationen zu von AD FS verwendeten Zertifikaten finden Sie im Artikel [Understanding Certificates Used by AD FS (Grundlegendes zu den von AD FS verwendeten Zertifikaten)](https://technet.microsoft.com/library/cc730660.aspx)

##<a name="pre-requisites"></a>Voraussetzungen

* **AD FS-Farm**: Die AD FS-Farm sollte 2012 R2 oder neuer sein.
* **Azure AD Connect**: Stellen Sie sicher, dass die AD Connect-Version 1.1.443.0 oder neuer verwendet wird. Sie werden den Task „AD FS-SSL-Zertifikat aktualisieren“ entdecken.

![Task „SSL aktualisieren“](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

##<a name="step-1-provide-ad-fs-farm-information"></a>Schritt 1: Bereitstellen von Informationen zur AD FS-Farm

AAD Connect versucht, die Informationen über die AD FS-Farm automatisch über folgende Schritte abzurufen:
1. Abfragen der Farminformationen von AD FS (2016 oder neuer)
2. Verweisen auf die Informationen von vorherigen Ausführungen (lokal mit Azure AD Connect gespeichert) 

Die angezeigte Liste von Servern kann bearbeitet werden, indem Server hinzugefügt oder entfernt werden, um die derzeitige Konfiguration der AD FS-Farm darzustellen. Sobald die Serverinformationen bereitgestellt wurden, zeigt Azure AD Connect die Konnektivität und den aktuellen Status des SSL-Zertifikats an.

![AD FS-Serverinformationen](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

Wenn die Liste einen Server enthält, der nicht mehr Teil der AD FS-Farm ist, klicken Sie auf „Entfernen“, um den Server aus der Liste der Server in der AD FS-Farm zu löschen. 

![Offline-Server in der Liste](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)
 
>[!NOTE] 
> Das Entfernen aus der Liste von Servern für die AD FS-Farm in Azure AD Connect ist ein lokaler Vorgang, durch den die Informationen für die AD FS-Farm aktualisiert werden, die Azure AD Connect lokal verwaltet. Azure AD Connect wird keine Änderung der Konfiguration von AD FS vornehmen, um die Änderung wiederzugeben.    

##<a name="step-2-provide-new-ssl-certificate"></a>Schritt 2: Bereitstellen des neuen SSL-Zertifikats

Sobald die Informationen zu den AD FS-Farm-Servern bestätigt wurden, fragt Azure AD Connect nach dem neuen SSL-Zertifikat. Stellen Sie ein kennwortgeschütztes PFX-Zertifikat bereit, um die Installation fortzuführen. 

![SSL-Zertifikat](./media/active-directory-aadconnectfed-ssl-update/certificate.png)
 
Sobald das Zertifikat bereitgestellt wurde, führt Azure AD Connect für das Zertifikat eine Reihe von Überprüfungen der erforderlichen Komponenten durch, um sicherzustellen, dass das Zertifikat für die AD FS-Farm richtig ist:
1.    Der Antragstellername (SN) / Alternative Antragstellername (SAN) für das Zertifikat ist entweder der Name des Verbunddiensts oder ein Platzhalterzertifikat.
2.    Das Zertifikat ist mehr als 30 Tage lang gültig.
3.    Die Vertrauenskette des Zertifikats ist gültig. 
4.    Das Zertifikat ist kennwortgeschützt.

##<a name="step-3-select-servers-for-update"></a>Schritt 3: Auswählen von Servern für das Update

Wählen Sie im nächsten Schritt die Server aus, auf denen das SSL-Zertifikat aktualisiert werden muss. Server, die offline sind, können nicht für das Update ausgewählt werden. 

![Auswählen von zu aktualisierenden Servern](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

Nachdem die Konfiguration abgeschlossen ist, zeigt Azure AD Connect die Meldung mit dem Status des Updates an und stellt eine Option zum Überprüfen der AD FS-Anmeldung bereit.

![Konfiguration abgeschlossen](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

##<a name="faqs"></a>Häufig gestellte Fragen

* **Was sollte der Antragstellername des Zertifikats für das neue AD FS-SSL-Zertifikats sein?**

    Azure AD Connect überprüft, ob der Antragstellername des Zertifikats bzw. der alternative Antragstellername den Namen des Verbunddiensts enthält. Wenn der Name Ihres Verbunddiensts beispielsweise „fs.contoso.com“ lautet, muss der Antragstellername bzw. der alternative Antragstellername „fs.contoso.com“ lauten.  Platzhalterzertifikate werden ebenfalls akzeptiert. 

* **Warum werde ich auf der WAP-Server-Seite erneut nach Anmeldeinformationen gefragt?**

    Wenn die Anmeldeinformationen für die Verbindung mit AD FS-Servern nicht über die Berechtigung verfügen, auch die WAP-Server zu verwalten, fragt Azure AD Connect nach Anmeldeinformationen, die die Verwaltungsberechtigung für WAP-Server haben.

* **Der Server wird als offline angezeigt. Was soll ich tun?**

    Azure AD Connect kann keinen Vorgang durchführen, solange der Server offline ist. Wenn der Server Teil einer AD FS-Farm ist, überprüfen Sie die Konnektivität zum Server. Nachdem Sie das Problem gelöst haben, klicken Sie auf das Symbol zum Aktualisieren, um den Status im Assistenten zu aktualisieren. Wenn der Server zuvor Teil der Farm war, jedoch nicht mehr vorhanden ist, klicken Sie auf „Entfernen“, um ihn aus der Liste mit Servern zu löschen, die Azure AD Connect verwaltet.  Durch das Entfernen des Servers aus der Liste in Azure AD Connect wird nicht die AD FS-Konfiguration selbst geändert. Wenn Sie außerdem AD FS 2016 oder neuer verwenden, bleibt der Server in den Konfigurationseinstellungen und wird beim nächsten Ausführen des Tasks erneut angezeigt.

* **Kann ich einen Teil meiner Farm-Server mit dem neuen SSL-Zertifikat aktualisieren?**

    Ja. Sie können den Task „SSL-Zertifikat aktualisieren“ jederzeit erneut ausführen, um die verbleibenden Server zu aktualisieren. Auf der Seite „Server für SSL-Zertifikataktualisierung auswählen“ können Sie die Liste der Server nach „SSL-Ablaufdatum“ sortieren, um einfach auf die Server zuzugreifen, die noch nicht aktualisiert wurden. 

* **Ich habe den Server beim vorherigen Ausführen entfernt, er wird jedoch immer noch als offline angezeigt und auf der AD FS-Server-Seite aufgelistet. Warum wird der Offline-Server selbst nach dem Entfernen noch angezeigt?**

    Durch das Entfernen des Servers aus der Liste in Azure AD Connect wird dieser nicht aus der AD FS-Konfiguration entfernt. Azure AD Connect verweist auf AD FS (2016 oder neuer) auch für Informationen über die Farm und wenn der Server noch in der AD FS-Konfiguration vorhanden ist, wird er wieder in der Liste angezeigt.  

## <a name="next-steps"></a>Nächste Schritte

[Azure AD Connect und Verbund](active-directory-aadconnectfed-whatis.md)
[Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect](active-directory-aadconnect-federation-management.md)

