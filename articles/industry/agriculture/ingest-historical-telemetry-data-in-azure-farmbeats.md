---
title: Erfassen historischer Telemetriedaten
description: In diesem Artikel wird beschrieben, wie historische Telemetriedaten erfasst werden.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 76d355413bc0dceb91f7cfa1a3988f48e2701d5e
ms.sourcegitcommit: 541e6139c535d38b9b4d4c5e3bfa7eef02446fdc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75667499"
---
# <a name="ingest-historical-telemetry-data"></a>Erfassen historischer Telemetriedaten

In diesem Artikel erfahren Sie, wie Sie historische Sensordaten in Azure FarmBeats erfassen.

Das Erfassen historischer Daten von IoT-Ressourcen (Internet der Dinge) wie Geräten und Sensoren ist ein gängiges FarmBeats-Szenario. Sie erstellen Metadaten für Geräte und Sensoren und erfassen dann die historischen Daten in einem kanonischen Format in FarmBeats.

## <a name="before-you-begin"></a>Voraussetzungen

Für diesen Artikel muss FarmBeats installiert sein, und Sie müssen bereits historische Daten aus dem IoT gesammelt haben.
Sie müssen auch den Partnerzugriff aktivieren, wie in den folgenden Schritten erwähnt wird.

## <a name="enable-partner-access"></a>Aktivieren von Partnerzugriff

Sie müssen die Partnerintegration für Ihre Azure FarmBeats-Instanz aktivieren. Durch diesen Schritt wird ein Client erstellt, der als Gerätepartner Zugriff auf Ihre Azure FarmBeats-Instanz hat und folgende Werte bereitstellt, die in den weiteren Schritten benötigt werden.

- API-Endpunkt: Hierbei handelt es sich um die Datenhub-URL, z. B. https://\<Datenhub>.azurewebsites.net.
- Mandanten-ID
- Client-ID
- Geheimer Clientschlüssel
- EventHub-Verbindungszeichenfolge

Führen Sie folgende Schritte durch:

>[!NOTE]
> Die folgenden Schritte müssen von einem Administrator ausgeführt werden.

1. Laden Sie dieses [Skript](https://aka.ms/farmbeatspartnerscript) herunter, und extrahieren Sie es auf Ihrem lokalen Laufwerk. In der ZIP-Datei befinden sich zwei Dateien.
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und öffnen Sie Azure Cloud Shell. Diese Option ist auf der Symbolleiste in der rechten oberen Ecke des Portals verfügbar.

    ![Symbolleiste im Azure-Portal](./media/for-tutorials/navigation-bar-1.png)

3. Vergewissern Sie sich, dass die Umgebung auf **PowerShell** festgelegt ist.

    ![Einstellung „PowerShell“](./media/for-tutorials/power-shell-new-1.png)

4. Laden Sie die beiden Dateien, die Sie in Schritt 1 heruntergeladen haben, in Ihre Cloud Shell-Instanz hoch.

    ![Schaltfläche „Upload“ auf der Symbolleiste](./media/for-tutorials/power-shell-two-1.png)

5. Navigieren Sie zum Uploadverzeichnis der Dateien.

   >[!NOTE]
   > Die Datei wird standardmäßig in das Basisverzeichnis „/home/benutzername“ hochgeladen.
6. Führen Sie das Skript mithilfe dieses Befehls aus:

    ```azurepowershell-interactive
    ./generateCredentials.ps1
    ```

7. Folgen Sie den Anweisungen auf dem Bildschirm, um die Werte für **API-Endpunkt**, **Mandanten-ID**, **Client-ID**, **Geheimer Clientschlüssel** und **EventHub-Verbindungszeichenfolge** zu erfassen. Die EventHub-Verbindungszeichenfolge ist in der API-Antwort in Swagger enthalten.

## <a name="create-device-or-sensor-metadata"></a>Erstellen von Geräte- oder Sensormetadaten

 Nachdem Sie nun über die erforderlichen Anmeldeinformationen verfügen, können Sie das Gerät und die Sensoren definieren. Dazu erstellen Sie die Metadaten durch Aufrufen von FarmBeats-APIs. Beachten Sie, dass Sie die APIs als die Client-App, die Sie im obigen Abschnitt erstellt haben, aufrufen müssen.

 Der FarmBeats-Datenhub bietet folgende APIs für die Erstellung und Verwaltung von Geräte- oder Sensormetadaten:

- /**DeviceModel:** DeviceModel entspricht den Metadaten des Geräts, beispielsweise dem Hersteller und dem Gerätetyp, bei dem es sich um ein Gateway oder einen Knoten handeln kann.
- /**Device:** „Gerät“ (Device) entspricht einem physischen Gerät, das in dem landwirtschaftlichen Betrieb vorhanden ist.
- /**SensorModel:** „SensorModel“ entspricht den Metadaten des Sensors. Hierzu zählen beispielsweise der Hersteller, der Sensortyp – analog oder digital – und die Sensormessung, z. B. Umgebungstemperatur und Luftdruck.
- /**Sensor:** „Sensor“ entspricht einem physischen Sensor, der Werte aufzeichnet. Ein Sensor ist in der Regel mit einem Gerät mit einer Geräte-ID verbunden.  


|        DeviceModel   |  Vorschläge   |
| ------- | -------             |
|     Type (Knoten, Gateway)        |          Typ des Geräts – Knoten oder Gateway      |
|          Hersteller            |         Name des Herstellers    |
|  ProductCode                    |  Produktcode oder Modellname oder -nummer des Geräts. Beispiel: EnviroMonitor#6800.  |
|            Ports          |     Portname und -typ (digital oder analog).
|     Name                 |  Name zum Identifizieren der Ressource. Beispiel: Modell- oder Produktname.
      BESCHREIBUNG     | Aussagekräftige Beschreibung des Modells
|    Eigenschaften          |    Zusätzliche Eigenschaften des Herstellers   |
|    **Device**             |                      |
|   DeviceModelId     |     ID des zugeordneten Gerätemodells  |
|  HardwareId          | Eindeutige ID für das Gerät, z. B. die MAC-Adresse
|  ReportingInterval        |   Berichtsintervall in Sekunden
|  Location            |  Breitengrad (–90 bis +90), Längengrad (–180 bis +180) und Höhe (in Metern) des Geräts   
|ParentDeviceId       |    ID des übergeordneten Geräts, mit dem dieses Gerät verbunden ist. Beispielsweise ein Knoten, der mit einem Gateway verbunden ist. Ein Knoten weist die parentDeviceId als Gateway auf.  |
|    Name            | Ein Name zum Identifizieren der Ressource. Gerätepartner müssen einen Namen senden, der mit dem Gerätenamen auf der Partnerseite konsistent ist. Ist der Partnergerätename benutzerdefiniert, muss der gleiche benutzerdefinierte Name in FarmBeats angegeben werden.|
|     BESCHREIBUNG       |      Eine aussagekräftige Beschreibung |
|     Eigenschaften    |  Zusätzliche Eigenschaften des Herstellers
|     **SensorModel**        |          |
|       Type (analog, digital)          |      Art des Sensors (analog oder digital)       |
|          Hersteller            |       Sensorhersteller     |
|     ProductCode| Produktcode oder Modellname oder -nummer. Beispiel: RS-CO2-N01 |
|       SensorMeasures > Name       | Der Name der Sensormessung. Nur Kleinbuchstaben werden unterstützt. Geben Sie bei Messungen aus unterschiedlicher Tiefe die Tiefe an. Beispiel: soil_moisture_15cm. Dieser Name muss mit den Telemetriedaten übereinstimmen.  |
|          SensorMeasures > DataType       |Die Art der Telemetriedaten. Derzeit wird „double“ unterstützt.|
|    SensorMeasures > Type    |Die Art der Messung der Sensortelemetriedaten. Die systemdefinierten Typen sind AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration, PAR. Informationen zum Hinzufügen weiterer Typen finden Sie unter der API „/ExtendedType“.|
|        SensorMeasures > Unit              | Die Einheit der Sensortelemetriedaten. Die systemdefinierten Typen sind NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolPerMeterSquaredPerSecond, InchesPerHour. Informationen zum Hinzufügen weiterer Einheiten finden Sie unter der API „/ExtendedType“.|
|    SensorMeasures > AggregationType    |  Mögliche Werte: „none“, „average“, „maximum“, „minimum“ oder „StandardDeviation“  |
|          Name            | Name zum Identifizieren einer Ressource. Beispiel: Modell- oder Produktname.  |
|    BESCHREIBUNG        | Aussagekräftige Beschreibung des Modells  |
|   Eigenschaften       |  Zusätzliche Eigenschaften des Herstellers  |
|    **Sensor**      |          |
| HardwareId          |   Eindeutige, vom Hersteller festgelegte ID für den Sensor |
|  SensorModelId     |    ID des zugeordneten Sensormodells   |
| Location          |  Breitengrad (–90 bis +90), Längengrad (–180 bis +180) und Höhe (in Metern) des Sensors|
|   Port > Name        |  Name und Typ des Ports, über den der Sensor mit dem Gerät verbunden ist. Hierbei muss es sich um denselben Namen handeln, der auch im Gerätemodell definiert ist. |
|    DeviceID  |    ID des Geräts, mit dem der Sensor verbunden ist     |
| Name            |   Der Name zur Identifizierung der Ressource. Beispiel: Sensorname oder Produktname und Modellnummer oder Produktcode|
|    BESCHREIBUNG      | Eine aussagekräftige Beschreibung |
|    Eigenschaften        |Zusätzliche Eigenschaften des Herstellers |

Weitere Informationen zu Objekten finden Sie unter [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

### <a name="api-request-to-create-metadata"></a>API-Anforderung zum Erstellen von Metadaten

Für eine API-Anforderung kombinieren Sie die HTTP-Methode (POST), die URL des API-Diensts und den URI einer Ressource zum Abfragen, Übermitteln, Erstellen oder Löschen einer Anforderung. Dann fügen Sie mindestens einen HTTP-Anforderungsheader hinzu. Die URL des API-Diensts ist der API-Endpunkt, d. h. die Datenhub-URL (https://\<ihrdatenhub>.azurewebsites.net).  

### <a name="authentication"></a>Authentication

Der FarmBeats-Datenhub verwendet die Bearerauthentifizierung. Für diese sind folgende Anmeldeinformationen erforderlich, die Sie im vorherigen Abschnitt generiert haben:

- Client-ID
- Geheimer Clientschlüssel
- Mandanten-ID

Mit diesen Anmeldeinformationen kann der Aufrufer ein Zugriffstoken anfordern. Das Token muss in den nachfolgenden API-Anforderungen wie folgt im Headerabschnitt gesendet werden:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

Der folgende Python-Beispielscode stellt das Zugriffstoken bereit, das für nachfolgende API-Aufrufe an FarmBeats verwendet werden kann: 

```python
import azure 

from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net" [Azure website](https://<yourdatahub>.azurewebsites.net)
CLIENT_ID = "<Your Client ID>"   
CLIENT_SECRET = "<Your Client Secret>"   
TENANT_ID = "<Your Tenant ID>" 
AUTHORITY_HOST = 'https://login.microsoftonline.com' 
AUTHORITY = AUTHORITY_HOST + '/' + TENANT_ID 
#Authenticating with the credentials 
context = adal.AuthenticationContext(AUTHORITY) 
token_response = context.acquire_token_with_client_credentials(ENDPOINT, CLIENT_ID, CLIENT_SECRET) 
#Should get an access token here 
access_token = token_response.get('accessToken') 
```


**HTTP-Anforderungsheader**

Hier sind die gängigsten Anforderungsheader aufgeführt, die bei einem für den FarmBeats-Datenhub bestimmten API-Aufruf angegeben werden müssen:

- **Content-Type:** application/json
- **Autorisierung:** Bearer <Access-Token>
- **Accept:** application/json

### <a name="input-payload-to-create-metadata"></a>Eingabenutzlast für die Metadatenerstellung

DeviceModel


```json
{
  "type": "Node",
  "manufacturer": "string",
  "productCode": "string",
  "ports": [
    {
      "name": "string",
      "type": "Analog"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

Sicherungsmedium

```json
{
  "deviceModelId": "string",
  "hardwareId": "string",
  "farmId": "string",
  "reportingInterval": 0,
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "parentDeviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

SensorModel

```json
{
  "type": "Analog",
  "manufacturer": "string",
  "productCode": "string",
  "sensorMeasures": [
    {
      "name": "string",
      "dataType": "Double",
      "type": "string",
      "unit": "string",
      "aggregationType": "None",
      "depth": 0,
      "description": "string"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

```
Sensor

```json
{
  "hardwareId": "string",
  "sensorModelId": "string",
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "depth": 0,
  "port": {
    "name": "string",
    "type": "Analog"
  },
  "deviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```
Mit der folgenden Beispielanforderung wird ein Gerät erstellt. Diese Anforderung enthält eine JSON-Eingabe als Nutzlast mit dem Anforderungstext.

```bash
curl -X POST "https://<datahub>.azurewebsites.net/Device" -H  
"accept: application/json" -H  "Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  
\"reportingInterval\": 900,  \"name\": \"Device123\",  
\"description\": \"Test Device 123\"}" *
```

> [!NOTE]
> Die APIs geben eindeutige IDs für jede erstellte Instanz zurück. Diese IDs werden zum Senden der entsprechenden Telemetrienachrichten benötigt.

### <a name="send-telemetry"></a>Senden von Telemetriedaten

Nachdem Sie die Geräte und Sensoren in FarmBeats erstellt haben, können Sie nun die zugehörigen Telemetrienachrichten senden.

### <a name="create-a-telemetry-client"></a>Erstellen eines Telemetrieclients

Die Telemetriedaten müssen zur Verarbeitung an Azure Event Hubs gesendet werden. Der Azure Event Hubs-Dienst ermöglicht die Erfassung von Echtzeitdaten (Telemetrie) von verbundenen Geräten und Anwendungen. Wenn Sie Telemetriedaten an FarmBeats senden möchten, erstellen Sie einen Client, der Nachrichten an einen Event Hub in FarmBeats sendet. Weitere Informationen zum Senden von Telemetriedaten finden Sie unter [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

### <a name="send-a-telemetry-message-as-the-client"></a>Senden von Telemetrienachrichten als Client

Nachdem Sie eine Verbindung als Event Hubs-Client eingerichtet haben, können Sie Nachrichten im JSON-Format an den Event Hub senden.

Hier finden Sie Python-Beispielcode, mit dem Telemetriedaten als Client an einen angegebenen Event Hub gesendet werden:

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub Connection String provided by customer>"
EVENTHUBNAME = "<EventHub Name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical Telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

Konvertieren Sie das Format der historischen Sensordaten in ein kanonisches, für Azure FarmBeats geeignetes Format. Das kanonische Nachrichtenformat lautet wie folgt:

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        }
      ]
    }
 ]
}
```

Nachdem Sie die entsprechenden Geräte und Sensoren hinzugefügt haben, rufen Sie die Geräte-ID und die Sensor-ID aus der Telemetrienachricht ab, wie im vorherigen Abschnitt beschrieben.

Hier finden Sie ein Beispiel für eine Telemetrienachricht:


 ```json
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version": "1",
  "sensors": [
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 15
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 42
        }
      ]
    },
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 20
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 89
        }
      ]
    }
  ]
}
```


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu REST-API-basierten Integrationsdetails finden Sie unter [REST-API](references-for-azure-farmbeats.md#rest-api).
