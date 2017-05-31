---
title: Integrieren eines Azure-Speicherkontos in CDN | Microsoft-Dokumentation
description: "Erfahren Sie, wie Azure Content Delivery Network (CDN) für die Übermittlung breitbandiger Inhalte eingesetzt wird, indem Blobs aus Azure Storage zwischengespeichert werden."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 0ea23f4f59737123f4400dafc7506e121f24ae34
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Integrieren eines Azure-Speicherkontos in CDN
Zum Zwischenspeichern von Inhalten aus Ihrem Azure-Speicher kann CDN aktiviert werden. Das Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung für die Übermittlung von Inhalten mit hohen Breitbandenanforderungen. Dazu werden Blobs und statische Inhalte von Compute-Instanzen an physischen Knotenpunkten in den USA, Europa, Asien, Australien und Südamerika zwischengespeichert.

## <a name="step-1-create-a-storage-account"></a>Schritt 1: Erstellen eines Speicherkontos
Gehen Sie folgendermaßen vor, um für ein Azure-Abonnement ein neues Speicherkonto zu erstellen. Ein Speicherkonto ermöglicht den Zugriff auf Speicherdienste von Azure. Das Speicherkonto stellt die höchste Ebene des Namespace für den Zugriff auf die verschiedenen Komponenten der Speicherdienste von Azure dar: Blob-, Warteschlangen- und Tabellenspeicherdienste. Weitere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md).

Um ein Speicherkonto zu erstellen, müssen Sie entweder der Dienstadministrator oder ein Co-Administrator für das zugehörige Abonnement sein.

> [!NOTE]
> Es gibt mehrere Methoden zum Erstellen eines Speicherkontos, einschließlich Azure-Portal und PowerShell.  In diesem Tutorial wird das Azure-Portal verwendet.  
> 
> 

**So erstellen Sie ein Speicherkonto für ein Azure-Abonnement**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie unten links auf **Neu**. Wählen Sie im Dialogfeld **Neu** die Option **Daten und Speicher** aus, und klicken Sie dann auf **Speicherkonto**.
    
    Das Blatt **Speicherkonto erstellen** wird angezeigt.   

       ![Create Storage Account][create-new-storage-account]  

3. Geben Sie in das Feld **Name** den Namen einer Unterdomäne ein. Dieser Eintrag kann 3 bis 24 Kleinbuchstaben und Zahlen enthalten.
   
    Dieser Wert wird der Hostname im URI, der zum Adressieren von Blob-, Warteschlangen- oder Tabellenspeicherressourcen für das Abonnement verwendet wird. Zum Adressieren einer Containerressource im Blob-Dienst verwenden Sie einen URI im folgenden Format; dabei bezieht sich *&lt;Speicherkontobezeichnung&gt;* auf den Wert, den Sie in **URL eingeben** eingegeben haben:
   
    http://*&lt;Speicherkontobezeichnung&gt;*.blob.core.windows.net/*&lt;MeinContainer&gt;*
   
    **Wichtig:** Die URL-Bezeichnung bildet die Unterdomäne des Speicherkonto-URI und muss unter allen gehosteten Diensten in Azure eindeutig sein.
   
    Dieser Wert wird auch als Name für dieses Speicherkonto im Portal verwendet oder beim programmatischen Zugriff auf dieses Konto.
4. Behalten Sie die Standardwerte für **Bereitstellungsmodell**, **Kontoart**, **Leistung** und **Replikation** bei. 
5. Wählen Sie das **Abonnement** aus, das für das Speicherkonto verwendet werden soll.
6. Wählen Sie eine **Ressourcengruppe**aus, oder erstellen Sie eine.  Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Wählen Sie einen Standort für Ihr Speicherkonto aus.
8. Klicken Sie auf **Erstellen**. Das Erstellen des Speicherkontos kann einige Minuten dauern.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>Schritt 2: Aktivieren von CDN für das Speicherkonto

Mit der neuesten Integration können Sie jetzt CDN für Ihr Speicherkonto aktivieren, ohne die Speicherportalerweiterung verlassen zu müssen. 

1. Wählen Sie das Speicherkonto aus, und suchen Sie „CDN“, oder scrollen Sie im linken Navigationsmenü nach unten, und klicken Sie dann auf „Azure CDN“.
    
    Das Blatt **Azure CDN** wird angezeigt.

    ![CDN – Aktivieren der Navigation][cdn-enable-navigation]
    
2. Erstellen Sie einen neuen Endpunkt, indem Sie die entsprechenden Informationen angeben.
    - **CDN-Profil:** Sie können ein neues Profil erstellen oder ein vorhandenes Profil verwenden.
    - **Tarif:** Einen Tarif müssen Sie nur auswählen, wenn Sie ein neues CDN-Profil erstellen.
    - **CDN-Endpunktname:** Geben Sie einen Endpunktnamen Ihrer Wahl ein.

    > [!TIP]
       > Für den erstellten CDN-Endpunkt wird standardmäßig der Hostname Ihres Speicherkontos als Ursprung verwendet.

    ![CDN – Erstellen eines neuen Endpunkts][cdn-new-endpoint-creation]

3. Nach der Erstellung wird der neue Endpunkt in der Liste der Endpunkte oben angezeigt.

    ![CDN – Neuer Endpunkt für Speicherkonto][cdn-storage-new-endpoint]

> [!NOTE]
> Sie können CDN auch über die Azure CDN-Erweiterung aktivieren.[Tutorial](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>Schritt 3: Aktivieren weiterer CDN-Features

Klicken Sie auf dem Blatt „Azure CDN“ des Speicherkontos in der Liste auf den CDN-Endpunkt, um das Blatt für die CDN-Konfiguration zu öffnen. Sie können weitere CDN-Features für die Übertragung aktivieren, z.B. Komprimierung, Abfragezeichenfolge und Geofilterung. Sie können dem CDN-Endpunkt außerdem eine benutzerdefinierte Domänenzuordnung hinzufügen und HTTPS für benutzerdefinierte Domänen aktivieren.
    
![CDN – CDN-Konfiguration für Speicherkonto][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>Schritt 4: Zugriff auf CDN-Inhalte
Verwenden Sie für den Zugriff auf die im CDN zwischengespeicherten Inhalte den im Portal bereitgestellten CDN-URL. Die Adresse eines zwischengespeicherten Blobs hat etwa folgendes Format:

http://<*Endpunktname*\>.azureedge.net/<*meinÖffentlicherContainer*\>/<*Blobname*\>

> [!NOTE]
> Nachdem Sie den CDN-Zugriff auf ein Speicherkonto aktiviert haben, kommen alle öffentlich verfügbaren Objekte für den CDN-Edgezwischenspeicher in Frage. Wenn Sie ein Objekt ändern, das derzeit im CDN zwischengespeichert ist, stehen die neuen Inhalte erst dann über das CDN zur Verfügung, wenn das CDN seine Inhalte aktualisiert, wenn die Lebensdauer der zwischengespeicherten Inhalte abläuft.
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a>Schritt 5: Entfernen von Inhalten aus dem CDN
Wenn ein Objekt nicht mehr im Azure Content Delivery Network (CDN) zwischengespeichert werden soll, können Sie einen der folgenden Schritte ausführen:

* Sie können den öffentlichen Container zu einem privaten machen. Weitere Informationen finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](../storage/storage-manage-access-to-resources.md) .
* Sie können den CDN-Endpunkt mit dem Verwaltungsportal deaktivieren oder löschen.
* Sie können den gehosteten Dienst so ändern, dass er nicht mehr auf Anforderungen des Objekts antwortet.

Ein bereits im CDN zwischengespeichertes Objekt bleibt zwischengespeichert, bis die Lebensdauer des Objekts abgelaufen ist oder der Endpunkt gelöscht wird. Wenn die Lebensdauer abläuft, prüft das CDN, ob der CDN-Endpunkt weiterhin gültig ist und ob weiterhin anonym auf das Objekt zugegriffen werden kann. Ist dies nicht der Fall, wird das Objekt nicht mehr zwischengespeichert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Zuordnen von CDN-Inhalt (Content Delivery Network) zu einer benutzerdefinierten Domäne](cdn-map-content-to-custom-domain.md)
* [Aktivieren von HTTPS für eine benutzerdefinierte Domäne](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
