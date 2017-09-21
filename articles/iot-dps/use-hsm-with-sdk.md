---
title: "Azure-Anleitung: Verwenden verschiedener HSMs mit dem Gerätebereitstellungsdienst-Client-SDK | Microsoft-Dokumentation"
description: "Azure-Anleitung: Verwenden verschiedener HSMs mit physischen Geräten und Simulatoren mit dem Gerätebereitstellungsdienst-Client-SDK"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 08/28/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: 
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: a5649ab873993d20803cb01a4b0ecc278c3ce16c
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk"></a>Verwenden verschiedener Hardwaresicherheitsmodule mit dem Gerätebereitstellungsdienst-Client-SDK
Die folgenden Schritte zeigen, wie Sie verschiedene [Hardwaresicherheitsmodule (HSMs)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) mit dem Gerätebereitstellungsdienst-Client-SDK in C# mit physischen Geräten und Simulatoren verwenden.  Der Bereitstellungsdienst unterstützt zwei Authentifizierungsmodi: X**.**509 und Trusted Platform Module (TPM).

## <a name="prerequisites"></a>Voraussetzungen

Bereiten Sie Ihre Entwicklungsumgebung gemäß dem Abschnitt „Vorbereiten der Entwicklungsumgebung“ aus dem Leitfaden [Erstellen und Bereitstellen eines simulierten Geräts mithilfe des IoT Hub Device Provisioning-Diensts (Vorschauversion)] (./quick-create-simulated-device.md) vor.

## <a name="enable-authentication-with-different-hsms"></a>Aktivieren der Authentifizierung mit verschiedenen HSMs

Der Authentifizierungsmodus (X**.**509 oder TPM) muss für das physische Gerät oder den Simulator aktiviert werden, bevor es bzw. er im Azure-Portal registriert werden kann.  Navigieren Sie zum Stammordner für „azure-iot-sdk-c“.  Führen Sie den entsprechenden Befehl für den gewünschten Authentifizierungsmodus aus.

### <a name="use-x509-with-simulator"></a>Verwenden von X**.**509 mit Simulator

Der Bereitstellungsdienst verfügt standardmäßig über einen DICE-Emulator (Device Identity Composition Engine), der ein X**.**509-Zertifikat für die Geräteauthentifizierung generiert.  Führen Sie den folgenden Befehl aus, um die X**.**509-Authentifizierung zu aktivieren:

```
cmake -Ddps_auth_type=x509 ..
```

Informationen zu Hardware mit DICE finden Sie [hier](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>Verwenden von X**.**509 mit Hardware

Der Bereitstellungsdienst kann mit X**.**509 für andere Hardware verwendet werden.  Für die Verbindungsherstellung wird eine Schnittstelle zwischen der Hardware und dem SDK benötigt.  Informationen zur Schnittstelle erhalten Sie vom HSM-Hersteller.

### <a name="use-tpm"></a>Verwenden von TPM

Der Bereitstellungsdienst kann eine Verbindung mit Windows- und Linux-basierten TPM-Hardwarechips mit SAS-Token herstellen.  Führen Sie den folgenden Befehl aus, um die TPM-Authentifizierung zu aktivieren:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Verwenden von TPM mit Simulator

Wenn Sie über kein Gerät mit TPM-Chips verfügen, können Sie bei der Entwicklung unter dem Windows-Betriebssystem einen Simulator verwenden.  Führen Sie den folgenden Befehl aus, um die TPM-Authentifizierung zu aktivieren und den TPM-Simulator auszuführen:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>Erstellen des SDKs
Das SDK muss vor dem Erstellen der Geräteregistrierung erstellt werden.

### <a name="linux"></a>Linux
- So erstellen Sie das SDK unter Linux:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Fügen Sie zum Erstellen von Debugbinärdateien dem Projektgenerierungsbefehl die entsprechende CMake-Option hinzu. Beispiel:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Für die SDK-Erstellung stehen zahlreiche [CMake-Konfigurationsoptionen](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html) zur Verfügung. So können Sie dem CMake-Projektgenerierungsbefehl beispielsweise ein Argument hinzufügen, um einen der verfügbaren Protokollstapel zu deaktivieren:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Sie können auch einen Komponententest erstellen und ausführen:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Wenn Sie das SDK unter Windows erstellen möchten, führen Sie die folgenden Schritte aus, um Projektdateien zu generieren:
    - Öffnen Sie eine Developer-Eingabeaufforderung für VS2015.
    - Führen Sie im Stammverzeichnis des Repositorys die folgenden CMake-Befehle aus:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    Dieser Befehl erstellt x86-Bibliotheken. Wenn Sie Bibliotheken für x64 erstellen möchten, passen Sie das cmake-Generatorargument an: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Nach erfolgreicher Projekterstellung enthält der Ordner `cmake` eine Visual Studio-Projektmappendatei (SLN-Datei). So erstellen Sie das SDK:
    - Öffnen Sie **cmake\azure_iot_sdks.sln** in Visual Studio, und erstellen Sie die Projektmappe. **ODER:**
    - Führen Sie an der Eingabeaufforderung, die Sie zum Generieren der Projektdateien verwendet haben, den folgenden Befehl aus:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Verwenden Sie zum Erstellen von Debugbinärdateien das entsprechende MSBuild-Argument: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Für die SDK-Erstellung stehen zahlreiche CMake-Konfigurationsoptionen zur Verfügung. So können Sie dem CMake-Projektgenerierungsbefehl beispielsweise ein Argument hinzufügen, um einen der verfügbaren Protokollstapel zu deaktivieren:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Sie können auch Komponententests erstellen und ausführen:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Einzubeziehende Bibliotheken
- Die folgenden Bibliotheken sollten in Ihrem SDK enthalten sein:
    - Der Bereitstellungsdienst: dps_http_transport, dps_client, dps_security_client
    - IoT Hub-Sicherheit: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-dps"></a>Erstellen eines Geräteregistrierungseintrags im Gerätebereitstellungsdienst

### <a name="tpm"></a>TPM
Gehen Sie bei Verwendung von TPM gemäß der Anleitung unter [Erstellen und Bereitstellen eines simulierten Geräts mithilfe des IoT Hub Device Provisioning-Diensts (Vorschauversion)](./quick-create-simulated-device.md) vor, um einen Geräteregistrierungseintrag im Gerätebereitstellungsdienst zu erstellen und den ersten Start zu simulieren.

### <a name="x509"></a>X**.**509
1. Wenn Sie ein Gerät beim Bereitstellungsdienst registrieren möchten, müssen Sie sich den Endorsement Key und die Registrierungs-ID des jeweiligen Geräts notieren. Die entsprechenden Angaben finden Sie im Bereitstellungstool des Client-SDKs. Führen Sie den folgenden Befehl aus, um das Zertifikat der Stammzertifizierungsstelle (für Registrierungsgruppen) und das Signaturgeberzertifikat (für die individuelle Registrierung) auszugeben:
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Gerätebereitstellungsdienst.
   - X**.**509, individuelle Registrierung: Wählen Sie auf dem Zusammenfassungsblatt des Bereitstellungsdiensts die Option **Registrierungen verwalten** aus. Klicken Sie auf der Registerkarte **Individual Enrollments** (Individuelle Registrierungen) im oberen Bereich auf die Schaltfläche **Hinzufügen**. Wählen Sie als *Mechanismus* für den Identitätsnachweis die Option **X**.**509** aus, und laden Sie das Signaturgeberzertifikat gemäß den Anweisungen auf dem Blatt hoch. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 
   - X**.**509, Gruppenregistrierung: Wählen Sie auf dem Zusammenfassungsblatt des Bereitstellungsdiensts die Option **Registrierungen verwalten** aus. Klicken Sie auf der Registerkarte **Group Enrollments** (Gruppenregistrierungen) im oberen Bereich auf die Schaltfläche **Hinzufügen**. Wählen Sie als *Mechanismus* für den Identitätsnachweis die Option **X**.**509** aus, geben Sie einen Gruppen- und einen Zertifizierungsnamen ein, und laden Sie das Zertifikat der Stammzertifizierungsstelle gemäß den Anweisungen auf dem Blatt hoch. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

## <a name="connecting-to-iot-hub-after-provisioning"></a>Herstellen einer Verbindung mit IoT Hub nach der Bereitstellung

Nachdem das Gerät mit dem Bereitstellungsdienst bereitgestellt wurde, verwendet die folgende API den HSM-Authentifizierungsmodus, um eine Verbindung mit IoT Hub herzustellen: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```

