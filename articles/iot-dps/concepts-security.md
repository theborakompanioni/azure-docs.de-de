---
title: Sicherheitskonzepte beim Azure IoT Hub Device Provisioning-Dienst | Microsoft-Dokumentation
description: "Beschreibt Konzepte der Sicherheitsbereitstellung, die speziell für Geräte mit dem Device Provisioning-Dienst und IoT Hub gelten"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 266b9b7eb228744075627e1e80710e63c27880cc
ms.openlocfilehash: 3ccbaaf55d2bdfedffcdb5ca069798328e2d75fd
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---

# <a name="iot-hub-device-provisioning-service-security-concepts"></a>Sicherheitskonzepte beim IoT Hub Device Provisioning-Dienst 

Der IoT Hub Device Provisioning-Dienst ist ein Hilfsdienst für IoT Hub, mit dem Sie Geräte ohne manuelles Eingreifen auf einem angegebenen IoT Hub konfigurieren können. Mit dem Device Provisioning-Dienst können Sie Millionen von Geräten auf sichere und skalierbare Weise bereitstellen. Dieser Artikel bietet eine Übersicht über die *Sicherheitskonzepte* bei der Gerätebereitstellung. Dieser Artikel ist für alle Personen relevant, die an der Vorbereitung von Geräten für die Bereitstellung beteiligt sind.

## <a name="attestation-mechanism"></a>Nachweismechanismus

Der Nachweismechanismus ist die Methode für das Überprüfen der Identität eines Geräts. Der Nachweismechanismus ist auch für die Registrierungsliste wichtig, über die der Bereitstellungsdienst erfährt, welche Methode zum Nachweis für ein bestimmtes Gerät verwendet werden soll.

> [!NOTE]
> IoT Hub verwendet das „Authentifizierungsschema“ für ein ähnliches Konzept in diesem Dienst.

Der Device Provisioning-Dienst unterstützt zwei Arten von Nachweisen:
* **X.509-Zertifikate** – basierend auf dem Standardauthentifizierungsablauf für X.509-Zertifikate.
* **SAS-Token** – basierend auf einer Nonce Herausforderung mit dem TPM-Standard für Schlüssel. Dies erfordert keine physische TPM-Instanz auf dem Gerät. Der Dienst erwartet für den Nachweis jedoch den Endorsement Key gemäß [TPM-Spezifikation](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Hardwaresicherheitsmodul

Das Hardwaresicherheitsmodul (HSM) dient der sicheren, hardwarebasierten Speicherung von Gerätegeheimnissen und gilt als die sicherste Form der Geheimnisspeicherung. Sowohl X.509-Zertifikate als auch SAS-Token können im HSM gespeichert werden. HSMs können mit beiden Nachweismechanismen verwendet werden, die die Bereitstellung unterstützt.

> [!TIP]
> Es wird dringend empfohlen, ein HSM mit Geräten zu verwenden, um Geheimnisse sicher auf Ihren Geräten zu speichern.

Gerätegeheimnisse können auch in Software (Arbeitsspeicher) gespeichert werden. Dies gilt jedoch im Vergleich zu einem HSM als die weniger sichere Speicherform.

## <a name="trusted-platform-module-tpm"></a>Trusted Platform Module (TPM)

TPM kann sich auf einen Standard zum sicheren Speichern von Schlüsseln, mit denen die Plattform authentifiziert wird, oder auf die E/A-Schnittstelle beziehen, die für die Interaktion mit den Modulen zum Implementieren des Standards verwendet wird. TPMs können als diskrete Hardware, integrierte Hardware, firmwarebasiert oder softwarebasiert vorhanden sein. Erfahren Sie mehr über [TPMs und TPM-Nachweis](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Der Device Provisioning-Dienst unterstützt nur TPM 2.0.

## <a name="endorsement-key"></a>Endorsement Key

Der Endorsement Key ist ein asymmetrischer Schlüssel innerhalb des TPM, der zur Fertigungszeit eingefügt wurde und für jedes TPM eindeutig ist. Der Endorsement Key kann nicht geändert oder entfernt werden. Der private Teil des Endorsement Key wird niemals außerhalb des TPM freigegeben. Der öffentliche Teil des Endorsement Key wird dagegen für die Erkennung eines Original-TPM verwendet. Erfahren Sie mehr über den [Endorsement Key](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

## <a name="storage-root-key"></a>Speicherstammschlüssel

Der Speicherstammschlüssel wird im TPM gespeichert und zum Schützen von in Anwendungen erstellten TPM-Schlüsseln verwendet, sodass diese Schlüssel nicht ohne das TPM verwendet werden können. Der Speicherstammschlüssel wird generiert, wenn Sie den Besitz des TPM übernehmen. Wenn Sie das TPM löschen, damit ein neuer Benutzer den Besitz übernehmen kann, wird ein neuer Speicherstammschlüssel generiert. Erfahren Sie mehr über den [Speicherstammschlüssel](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="root-certificate"></a>Stammzertifikat

Ein Stammzertifikat ist ein X.509-Zertifikattyp, der eine Zertifizierungsstelle darstellt und selbstsigniert ist. Es handelt sich um das Ende der Zertifikatkette.

## <a name="intermediate-certificate"></a>Zwischenzertifikat

Ein Zwischenzertifikat ist ein X.509-Zertifikat, das vom Stammzertifikat (oder einem anderen Zertifikat mit dem Stammzertifikat in seiner Kette) signiert wurde und das zum Signieren des untergeordneten Zertifikats verwendet wird.

## <a name="leaf-certificate"></a>Untergeordnetes Zertifikat

Ein untergeordnetes Zertifikat (oder Endeinheitzertifikat) wird zur Identifizierung des Zertifikatinhabers verwendet und enthält in seiner Zertifikatkette das Stammzertifikat. Mit dem untergeordneten Zertifikat werden kein anderen Zertifikate signiert.

