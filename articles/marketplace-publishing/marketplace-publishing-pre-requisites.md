---
title: Nicht technische Voraussetzungen für das Erstellen eines Angebots für den Azure Marketplace | Microsoft Docs
description: 'Grundlegendes zu den Anforderungen für das Erstellen und Bereitstellen eines Angebots im Azure Marketplace, damit andere Benutzer dieses erwerben können. '
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: ''
editor: ''

ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio

---
# Allgemeine Voraussetzungen für das Erstellen eines Angebots für den Azure Marketplace
Bevor Sie mit der Angebotserstellung fortfahren, müssen Sie sich zunächst mit den allgemeinen, geschäftsprozessbezogenen Voraussetzungen vertraut machen.

## Sicherstellen, dass Sie bei Microsoft als Verkäufer registriert sind
Ausführliche Anweisungen zum Registrieren eines Verkäuferkontos bei Microsoft finden Sie unter [Erstellen eines Microsoft-Verkäuferkontos](marketplace-publishing-accounts-creation-registration.md).

* **Wenn Ihr Unternehmen bereits als Verkäufer im Developer Center registriert ist, und Sie ein neues Angebot erstellen möchten**, melden Sie sich mit der gleichen E-Mail-ID beim Veröffentlichungsportal an, mit der Sie sich beim Developer Center registriert haben. Dieser Schritt ist erforderlich, damit Developer Center und Veröffentlichungsportal miteinander verknüpft werden.
* **Wenn Ihr Unternehmen bereits als Verkäufer im Developer Center registriert ist, und Sie ein vorhandenes Angebot bearbeiten möchten**, dann melden Sie sich beim Veröffentlichungsportal entweder mit dem Administratorkonto oder einem Konto an, das als Co-Admin-Konto im Veröffentlichungsportal hinzugefügt wurde. Schritte zum Hinzufügen eines Co-Admin-Kontos sind unten angegeben.

## Schritte zum Hinzufügen eines Co-Admins im Veröffentlichungsportal
Administratoren des Veröffentlichungsportals können die anderen Mitglieder des Unternehmens, die an der Anwendung arbeiten, dem Veröffentlichungsportal als Co-Admins hinzufügen. **Wenn Sie Administrator sind**, können Sie mit den folgenden Schritten einen Co-Admin hinzufügen.

> [!NOTE]
> Für neue Benutzer: Bevor Sie einen Co-Admin im Veröffentlichungsportal hinzufügen, stellen Sie sicher, dass Sie mindestens eine Anwendung im Veröffentlichungsportal erstellt haben. Dies ist erforderlich, da die Registerkarte **HERAUSGEBER** nur nach der Erstellung mindestens einer Anwendung im Veröffentlichungsportal angezeigt wird.
> 
> 

1. Stellen Sie sicher, dass die Co-Admin-E-Mail-ID ein Microsoft-Konto (MSA) ist. Falls nicht, registrieren Sie sie mit diesem [Link](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1) als MSA.
2. Stellen Sie sicher, dass mindestens eine Anwendung unter dem Administratorkonto vorhanden ist, bevor Sie versuchen, einen Co-Admin hinzuzufügen.
3. Nachdem Sie die obigen Schritte ausgeführt haben, melden Sie sich mit der Co-Admin-E-Mail-ID beim Veröffentlichungsportal an und dann wieder ab.
4. Melden Sie sich jetzt mit der Admin-E-Mail-ID beim Veröffentlichungsportal an.
5. Navigieren Sie zu „Herausgeber“ > wählen Sie Ihr Konto > „Administratoren“ > fügen Sie den Co-Admin hinzu (siehe Screenshot unten)
   
    ![Abbildung](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Stellen Sie sicher, dass die in den einzelnen Phasen des Veröffentlichungsvorgangs (z.B. Developer Center, Veröffentlichungsportal) angegebenen E-Mail-IDs für jegliche Kommunikation von Microsoft überwacht werden.
7. Verwenden Sie für die Developer Center-Registrierung kein Konto, das einer einzelnen Person zugeordnet ist. Dies ist empfehlenswert, damit keine Abhängigkeit von einer einzelnen Person besteht.
8. Falls bei der Developer Center-Registrierung Probleme auftreten, lösen Sie mit diesem [Link](https://developer.microsoft.com/de-DE/windows/support) ein Ticket.

## Schritte zum Löschen eines Co-Admins im Veröffentlichungsportal
**Administratoren** können mithilfe der folgenden Schritte einen Co-Admin löschen.

1. Melden Sie sich mit der Admin-E-Mail-ID beim Veröffentlichungsportal an.
2. Wählen Sie unter **Herausgeber** Ihr Konto aus, und navigieren Sie zu **Administratoren** > **Co-Admins** (Co-Administratoren).
3. Klicken Sie neben dem zu löschenden Co-Admin auf die Schaltfläche **X**, wie auf dem folgenden Screenshot zu sehen:
   
    ![Abbildung](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Die Eingabe von Steuer- und Bankdaten für Ihr Unternehmen ist nicht erforderlich, wenn Sie ausschließlich kostenlose Angebote (oder Bring Your Own License) veröffentlichen.
> 
> Um beginnen zu können, muss die Unternehmensregistrierung abgeschlossen werden. Während das Unternehmen die Steuer- und Bankdaten im Microsoft Developer-Konto bearbeitet, können die Entwickler bereits damit beginnen, das VM-Image im [Veröffentlichungsportal](https://publish.windowsazure.com) zu erstellen, es zu zertifizieren und in der Azure-Stagingumgebung zu testen. Die endgültige Genehmigung Ihres Verkäuferkontos wird erst beim letzten Schritt für die Veröffentlichung Ihres Angebot im Azure Marketplace benötigt.
> 
> 

## Erwerben eines Azure-Abonnements mit nutzungsbasierter Bezahlung
Dieses Abonnement basiert auf einer nutzungsbasierten Bezahlung und wird zum Erstellen Ihrer VM-Images und zum Bereitstellen der Images im [Azure Marketplace](https://azure.microsoft.com/marketplace/) verwendet. Wenn Sie noch kein Abonnement besitzen, können Sie sich hier registrieren: https://account.windowsazure.com/signup?offer=ms-azr-0003p

## Verkäuferländer
> [!WARNING]
> Um Ihre Dienste im Azure Marketplace verkaufen zu können, muss die registrierte Organisation in einem der genehmigten Verkäuferländer ansässig sein. Diese Einschränkung hat auszahlungstechnische und steuerliche Gründe. Die Länderliste wird in Kürze erweitert, prüfen Sie sie also regelmäßig. Die vollständige Liste finden Sie in Abschnitt 1b der [Teilnahmerichtlinien für den Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## Nächste Schritte
Nachdem die nicht technischen Voraussetzungen erfüllt sind, geht es als Nächstes um die angebotsspezifischen technischen Voraussetzungen. Klicken Sie auf den Link zum jeweiligen Angebotstyp, den Sie für den Azure Marketplace erstellen möchten.

* [Technische Voraussetzungen für das Erstellen eines VM-Images für den Azure Marketplace](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Technische Voraussetzungen für das Erstellen einer Lösungsvorlage für den Azure Marketplace](marketplace-publishing-solution-template-creation-prerequisites.md)

## Siehe auch
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)

<!---HONumber=AcomDC_0824_2016-->