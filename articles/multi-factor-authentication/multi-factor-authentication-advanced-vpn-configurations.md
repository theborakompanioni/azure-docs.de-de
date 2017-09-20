---
title: Erweiterte Szenarien mit Azure MFA und Drittanbieter-VPNs
description: "Enthält Schritt-für-Schritt-Anleitungen für die Integration von Azure MFA in Cisco, Citrix und Juniper."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1f94a214-d6f6-48a8-8a12-006b5896ae45
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: kgremban
ms.translationtype: HT
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: 61cf29acac22d546e1b75604f799993ad6ff9042
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Erweiterte Szenarien mit Azure Multi-Factor Authentication und Drittanbieter-VPN-Lösungen
Azure Multi-Factor Authentication kann verwendet werden, um eine nahtlose Verbindung mit verschiedenen VPN-Lösungen von Drittanbietern herzustellen. Dieser Artikel konzentriert sich auf die Cisco® ASA-VPN-Appliance, die Citrix NetScaler SSL-VPN-Appliance und die Juniper Networks Secure Access/Pulse Secure Connect Secure SSL-VPN-Appliance. Wir haben Konfigurationsanleitungen für diese drei häufig verwendeten virtuellen Geräte erstellt. Multi-Factor Authentication-Server kann jedoch in die meisten andere Systeme integriert werden, die RADIUS, LDAP, IIS oder anspruchsbasierte Authentifizierung bei AD FS nutzen. Weitere Details finden Sie unter [MFA-Serverkonfigurationen](multi-factor-authentication-get-started-server.md#next-steps).

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA-VPN-Appliance und Azure Multi-Factor Authentication
Azure Multi-Factor Authentication lässt sich in Ihre Cisco® ASA-VPN-Appliance integrieren, um für zusätzliche Sicherheit für Cisco AnyConnect® VPN-Anmeldungen und den Portalzugriff zu sorgen.  Sie können das LDAP- oder das RADIUS-Protokoll verwenden.  Wählen Sie eine der folgenden Optionen, um die ausführlichen Schritt-für-Schritt-Konfigurationsanleitungen herunterzuladen.

| Konfigurationshandbuch | Beschreibung |
| --- | --- |
| [Cisco ASA mit Anyconnect VPN und Azure MFA-Konfiguration für LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Integration Ihrer Cisco ASA-VPN-Appliance in Azure MFA per LDAP |
| [Cisco ASA mit Anyconnect-VPN und Azure MFA-Konfiguration für RADIUS](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Integration Ihrer Cisco ASA-VPN-Appliance in Azure MFA per RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL-VPN und Azure Multi-Factor Authentication
Azure Multi-Factor Authentication lässt sich in Ihre Citrix NetScaler SSL-VPN-Appliance integrieren, um für zusätzliche Sicherheit für Citrix NetScaler SSL-VPN-Anmeldungen und den Portalzugriff zu sorgen.  Sie können das LDAP- oder das RADIUS-Protokoll verwenden.  Wählen Sie eine der folgenden Optionen, um die ausführlichen Schritt-für-Schritt-Konfigurationsanleitungen herunterzuladen.

| Konfigurationshandbuch | Beschreibung |
| --- | --- |
| [Citrix NetScaler SSL-VPN und Azure MFA-Konfiguration für LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Integration Ihres Citrix NetScaler SSL-VPN in Azure MFA-Appliance per LDAP |
| [Citrix NetScaler SSL-VPN und Azure MFA-Konfiguration für RADIUS](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Integration Ihrer Citrix NetScaler SSL-VPN-Appliance in Azure MFA per RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL-VPN-Appliance und Azure Multi-Factor Authentication
Azure Multi-Factor Authentication lässt sich in Ihre Juniper/Pulse Secure SSL-VPN-Appliance integrieren, um für zusätzliche Sicherheit für Juniper/Pulse Secure SSL-VPN-Anmeldungen und den Portalzugriff zu sorgen.  Sie können das LDAP- oder das RADIUS-Protokoll verwenden.  Wählen Sie eine der folgenden Optionen, um die ausführlichen Schritt-für-Schritt-Konfigurationsanleitungen herunterzuladen.

| Konfigurationshandbuch | Beschreibung |
| --- | --- |
| [Juniper/Pulse Secure SSL-VPN und Azure MFA-Konfiguration für LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Integration Ihres Juniper/Pulse Secure SSL-VPN in Azure MFA-Appliance per LDAP |
| [Juniper/Pulse Secure SSL-VPN und Azure MFA-Konfiguration für RADIUS](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Integration Ihrer Juniper/Pulse Secure SSL-VPN-Appliance in Azure MFA per RADIUS |

## <a name="next-steps"></a>Nächste Schritte

- [Verbessern der vorhandenen Authentifizierungsinfrastruktur mit der NPS-Erweiterung für Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md)

- [Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](multi-factor-authentication-whats-next.md)
