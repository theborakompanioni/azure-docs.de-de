---
title: Verbinden eines Geräts mit C unter Windows | Microsoft Docs
description: Beschreibt, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung unter Windows ausführen.
services: ''
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2016
ms.author: dobett

---
# Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## Erstellen einer C-Beispielprojektmappe unter Windows
Die folgenden Schritte zeigen, wie Sie mit Visual Studio eine einfache, in C geschriebene Clientanwendung erstellen, die mit der vorkonfigurierten Lösung für die Remoteüberwachung kommuniziert.

Erstellen Sie ein Startprojekt in Visual Studio 2015 und fügen Sie die NuGet-Pakete für den IoT Hub-Geräteclienthinzu.

1. Erstellen Sie eine neue C-Konsolenanwendung mit Visual C++ in der **Win32-Konsolenanwendungsvorlage** in Visual Studio 2015. Geben Sie dem Projekt den Namen **RMDevice**.
2. Stellen Sie auf der Seite **Anwendungseinstellungen** im **Win32-Anwendungsassistenten** sicher, dass **Konsolenanwendung** ausgewählt ist, und deaktivieren Sie die Kontrollkästchen **Vorkompilierter Header** und **Prüfungen für den Security Development Lifecycle (SDL)**.
3. Löschen Sie im **Projektmappen-Explorer** die Dateien „stdafx.h“, „targetver.h“ und „stdafx.cpp“.
4. Benennen Sie im **Projektmappen-Explorer** die Datei „RMDevice.cpp“ in „RMDevice.c“ um.
5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Klicken Sie auf **Durchsuchen**, suchen Sie dann die folgenden NuGet-Pakete, und installieren Sie diese in das Projekt:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.HttpTransport
6. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, und klicken Sie dann auf **Eigenschaften**, um das Dialogfeld **Eigenschaftenseiten** des Projekts zu öffnen. Weitere Informationen hierzu finden Sie unter [Festlegen von Visual C++-Projekteigenschaften][lnk-c-project-properties].
7. Klicken Sie auf den Ordner **Linker** und dann auf die Eigenschaftenseite **Eingabe**.
8. Fügen Sie **crypt32.lib** zur Eigenschaft **Zusätzliche Abhängigkeiten** hinzu. Klicken Sie auf **OK** und dann erneut auf **OK**, um die Eigenschaftenwerte für das Projekt zu speichern.

## Angeben des Verhaltens des IoT Hub-Geräts
Die IoT Hub-Clientbibliotheken verwenden ein Modell, das sowohl das Format der Nachrichten angibt, die das Gerät an IoT Hub sendet, als auch die Befehle von IoT Hub, auf die das Gerät reagiert.

1. Öffnen Sie in Visual Studio die Datei „RMDevice.c“. Ersetzen Sie die vorhandenen `#include`-Anweisungen durch Folgendes:
   
    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```
2. Fügen Sie die folgenden Variablendeklarationen nach den `#include`-Anweisungen hinzu. Ersetzen Sie die Platzhalterwerte [Device Id] und [Device Key] durch die Gerätewerte aus dem Dashboard der Remoteüberwachungslösung. Verwenden Sie den IoT Hub-Hostnamen aus dem Dashboard, um [IoTHub Name] zu ersetzen. Beispiel: Wenn der IoT Hub-Hostname **contoso.azure-devices.net** lautet, ersetzen Sie [IoTHub Name] durch **contoso**:
   
    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```
3. Fügen Sie den folgenden Code hinzu, um das Modell zu definieren, das dem Gerät die Kommunikation mit IoT Hub ermöglicht. Dieses Modell gibt an, dass das Gerät die Daten „Temperatur“, „externe Temperatur“, „Luftfeuchtigkeit“ und eine Geräte-ID als Telemetriedaten sendet. Das Gerät sendet auch Metadaten über sich selbst an IoT Hub, einschließlich einer Liste von unterstützten Befehlen. Das Gerät reagiert auf die Befehle **SetTemperature** und **SetHumidity**:
   
    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);
   
    DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
    );
   
    DECLARE_STRUCT(DeviceProperties,
    ascii_char_ptr, DeviceID,
    _Bool, HubEnabledState
    );
   
    DECLARE_MODEL(Thermostat,
   
    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),
   
    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),
   
    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
    );
   
    END_NAMESPACE(Contoso);
    ```

## Implementieren des Geräteverhaltens
Sie müssen nun den Code hinzufügen, der das im Modell definierte Verhalten implementiert.

1. Fügen Sie die folgenden Funktionen hinzu, die ausgeführt werden, wenn das Gerät von IoT Hub die Befehle **SetTemperature** und **SetHumidity** empfängt:
   
    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }
   
    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```
2. Fügen Sie die folgende Funktion hinzu, die eine Nachricht an IoT Hub sendet:
   
    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }
   
        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```
3. Fügen Sie die folgende Funktion hinzu, die die SDK-Serialisierungsbibliothek einbindet:
   
    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```
4. Fügen Sie die folgende Funktion hinzu, um eine Verbindung mit IoT Hub einzugehen, Nachrichten zu versenden und zu empfangen und sich vom Hub zu trennen. Beachten Sie, dass das Gerät direkt nach dem Herstellen der Verbindung Metadaten über sich selbst an IoT Hub sendet, z.B. die Befehle, die es unterstützt. Dadurch kann die Projektmappe den Status des Geräts auf dem Dashboard in **Wird ausgeführt** ändern:
   
    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;
   
        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;
   
            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {
   
              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;
   
              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);
   
                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }
   
                }
   
                STRING_delete(commandsMetadata);
              }
   
              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;
   
              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;
   
                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);
   
                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }
   
                ThreadAPI_Sleep(1000);
              }
            }
   
            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
   
    Zu Referenzzwecken finden Sie nachstehend ein Beispiel für die **DeviceInfo**-Nachricht, die beim Start an IoT Hub gesendet wird:
   
    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
   
    Zu Referenzzwecken finden Sie nachstehend ein Beispiel für die **Telemetrie**-Nachricht, die an IoT Hub gesendet wird:
   
    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
   
    Zu Referenzzwecken finden Sie nachstehend ein Beispiel für einen **Befehl**, der von IoT Hub empfangen wird:
   
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```
5. Ersetzen Sie die **main**-Funktion durch folgenden Code, um die **remote\_monitoring\_run**-Funktion aufzurufen:
   
    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```
6. Klicken Sie auf **Projektmappe erstellen** und dann auf **Projektmappe**, um die Geräteanwendung zu erstellen.
7. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, klicken Sie auf **Debuggen**, und klicken Sie dann auf **Neue Instanz starten**, um das Beispiel auszuführen. In der Konsole werden Nachrichten angezeigt, während die Anwendung Telemetriebeispieldaten an IoT Hub sendet und Befehle empfängt.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx

<!---HONumber=AcomDC_0720_2016-->