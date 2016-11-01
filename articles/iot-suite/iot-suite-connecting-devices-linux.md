<properties
   pageTitle="Anschließen eines Geräts mit C unter Linux | Microsoft Azure"
   description="Es wird beschrieben, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung unter Linux ausführen."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="dobett"/>


# Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## Erstellen und Ausführen eines in C geschriebenen Beispielclients für Linux

Die folgenden Schritte veranschaulichen die Erstellung einer einfachen Clientanwendung, die in C geschrieben und unter Ubuntu Linux erstellt und ausgeführt wird. Diese kommuniziert mit der vorkonfigurierten Lösung für die Remoteüberwachung. Für diese Schritte benötigen Sie ein Gerät mit der Ubuntu-Version 15.04 oder 15.10. Ehe Sie fortfahren, installieren Sie die erforderlichen Pakete mit dem folgenden Befehl auf Ihrem Ubuntu-Gerät:

```
sudo apt-get install cmake gcc g++
```

## Installieren der Clientbibliotheken auf dem Gerät

Die Clientbibliotheken für Azure IoT Hub stehen als ein Paket bereit, das Sie mit dem Befehl **apt-Get** auf Ihrem Ubuntu-Gerät installieren können. Führen Sie die folgenden Schritte aus, um das Paket zu installieren, das die IoT Hub-Clientbibliotheks- und Headerdateien auf Ihrem Ubuntu-Computer enthält:

1. Fügen Sie das Repository „AzureIoT“ dem Computer hinzu:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Installieren Sie das Paket „azure-iot-sdk-c-dev“:

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## Hinzufügen von Code, um das Verhalten des Geräts anzugeben

Erstellen Sie auf Ihrem Ubuntu-Computer den Ordner **Remote\_monitoring**. Erstellen Sie im Ordner **remote\_monitoring** diese vier Dateien: **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h** und **CMakeLists.txt**.

Die Clientbibliotheken des IoT Hub-Serialisierungsprogramms verwenden ein Modell, das sowohl das Format der Nachrichten angibt, die das Gerät an IoT Hub sendet, als auch die Befehle von IoT Hub, auf die das Gerät reagiert.

1. Öffnen Sie in einem Text-Editor die Datei **remote\_monitoring.c**. Fügen Sie die folgenden `#include`-Anweisungen ein:

    ```
    #include "iothubtransportamqp.h"
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
    static const char* hubName = "IoTHub Name]";
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

### Hinzufügen von Code, um das Verhalten des Geräts zu implementieren

Wenn das Gerät einen Befehl aus dem Hub empfängt, fügen Sie auszuführende Funktionen und den Code hinzu, um simulierte Telemetriedaten an den Hub zu senden.

1. Fügen Sie die folgenden Funktionen hinzu, die ausgeführt werden, wenn das Gerät die im Modell definierten Befehle **SetTemperature** und **SetHumidity** empfängt:

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
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
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
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
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
        platform_deinit();
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

### Hinzufügen von Code zum Aufrufen der Funktion „remote\_monitoring\_run“

Öffnen Sie in einem Text-Editor die Datei **remote\_monitoring.h**. Fügen Sie den folgenden Code hinzu:

```
void remote_monitoring_run(void);
```

Öffnen Sie in einem Text-Editor die Datei **main.c**. Fügen Sie den folgenden Code hinzu:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## Erstellen der Clientanwendung mit CMake

Die folgenden Schritte beschreiben, wie Sie *CMake* verwenden, um Ihre Clientanwendung zu erstellen.

1. Öffnen Sie in einem Text-Editor die Datei **CMakeLists.txt** im Ordner **remote\_monitoring**.

2. Fügen Sie die folgenden Anweisungen hinzu, um zu definieren, wie Sie Ihre Clientanwendung erstellen:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. Erstellen Sie im Ordner **remote\_monitoring** einen Ordner zum Speichern der *make*-Dateien, die CMake generiert, und führen Sie dann die Befehle **cmake** und **make** wie folgt aus:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Führen Sie die Clientanwendung aus, und senden Sie die Telemetriedaten an IoT Hub:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

<!---HONumber=AcomDC_0720_2016-->