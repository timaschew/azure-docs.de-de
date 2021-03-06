---
title: Kopieren von Daten aus Google BigQuery mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten aus Google BigQuery mithilfe einer Kopieraktivität in eine Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: jingwang
ms.openlocfilehash: ca12c7a3fe8a5ade8cf0e4ce00977bdcc9a300a6
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007653"
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory"></a>Kopieren von Daten aus Google BigQuery mithilfe von Azure Data Factory

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Google BigQuery zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Google BigQuery in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Data Factory bietet zum Herstellen von Konnektivität einen integrierten Treiber. Aus diesem Grund müssen Sie Treiber für die Verwendung dieses Connectors nicht manuell installieren.

>[!NOTE]
>Dieses Google BigQuery-Connector wird basierend auf den BigQuery-APIs erstellt. Beachten Sie, dass BigQuery die maximale Rate eingehender Anforderung begrenzt und entsprechende Kontingente projektbezogen durchsetzt, siehe [Kontingente & Limits – API-Anfragen](https://cloud.google.com/bigquery/quotas#api_requests). Stellen Sie sicher, dass Sie nicht zu viele gleichzeitige Anforderungen für das Konto auslösen.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten verwendet werden, die für den Google BigQuery-Connector spezifisch sind.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Google BigQuery verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **GoogleBigQuery** festgelegt werden. | JA |
| project | Das Projekt-ID des BigQuery-Standardprojekts, das abgefragt werden soll.  | JA |
| additionalProjects | Eine durch Trennzeichen getrennte Liste mit Projekt-IDs öffentlicher BigQuery-Projekte, auf die zugegriffen werden soll.  | Nein  |
| requestGoogleDriveScope | Ob Zugriff auf Google Drive angefordert werden soll. Das Zulassen des Zugriffs auf Google Drive ermöglicht die Unterstützung von Verbundtabellen, die BigQuery-Daten mit Daten auf Google Drive kombinieren. Der Standardwert ist **false**.  | Nein  |
| authenticationType | Der OAuth 2.0-Authentifizierungsmechanismus, der für die Authentifizierung verwendet wird. „ServiceAuthentication“ kann nur für eine selbstgehostete Integration Runtime verwendet werden. <br/>Zulässige Werte sind **UserAuthentication** und **ServiceAuthentication**. Weitere Eigenschaften und JSON-Beispiele für diese Authentifizierungstypen finden Sie in den Abschnitten nach dieser Tabelle. | JA |

### <a name="using-user-authentication"></a>Verwenden der Benutzerauthentifizierung

Legen Sie die Eigenschaft „authenticationType“ auf **UserAuthentication** fest, und geben Sie zusammen mit den im vorherigen Abschnitt beschriebenen generischen Eigenschaften die folgenden Eigenschaften an:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| clientId | Die ID der Anwendung, die verwendet wird, um das Aktualisierungstoken zu generieren. | Nein  |
| clientSecret | Das Geheimnis der Anwendung, das verwendet wird, um das Aktualisierungstoken zu generieren. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Nein  |
| refreshToken | Das Aktualisierungstoken, das von Google für die Autorisierung des Zugriffs auf BigQuery abgerufen wird. Informationen dazu, wie Sie ein Token erhalten, finden Sie unter [Abrufen von OAuth 2.0-Zugriffstoken](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens) und in [diesem Communityblog](https://jpd.ms/getting-your-bigquery-refresh-token-for-azure-datafactory-f884ff815a59). Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Nein  |

**Beispiel:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "clientId": "<id of the application used to generate the refresh token>",
            "clientSecret": {
                "type": "SecureString",
                "value":"<secret of the application used to generate the refresh token>"
            },
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refresh token>"
            }
        }
    }
}
```

### <a name="using-service-authentication"></a>Verwenden der Dienstauthentifizierung

Legen Sie die Eigenschaft „authenticationType“ auf **ServiceAuthentication** fest, und geben Sie zusammen mit den im vorherigen Abschnitt beschriebenen generischen Eigenschaften die folgenden Eigenschaften an. Dieser Authentifizierungstyp kann nur für eine selbstgehostete Integration Runtime verwendet werden.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| E-Mail | Die E-Mail-ID des Dienstkontos, die für „ServiceAuthentication“ verwendet wird. Diese kann nur für die selbstgehostete Integration Runtime verwendet werden.  | Nein  |
| keyFilePath | Der vollständige Pfad zur P12-Schlüsseldatei, die zur Authentifizierung der E-Mail-Adresse des Dienstkontos verwendet wird. | Nein  |
| trustedCertPath | Der vollständige Pfad der PEM-Datei mit vertrauenswürdigen Zertifizierungsstellenzertifikaten zur Überprüfung des Servers beim Verbindungsaufbau über SSL. Diese Eigenschaft kann nur festgelegt werden, wenn Sie SSL für die selbstgehostete Integration Runtime verwenden. Der Standardwert ist die Datei „cacerts.pem“, die mit der Integration Runtime installiert wird.  | Nein  |
| useSystemTrustStore | Gibt an, ob ein Zertifizierungsstellenzertifikat aus dem Vertrauensspeicher des Systems oder aus einer angegebenen PEM-Datei verwendet werden soll. Der Standardwert ist **false**.  | Nein  |

**Beispiel:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project id>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "ServiceAuthentication",
            "email": "<email>",
            "keyFilePath": "<.p12 key path on the IR machine>"
        },
        "connectVia": {
            "referenceName": "<name of Self-hosted Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
} 
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Google BigQuery-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Google BigQuery die „type“-Eigenschaft des Datasets auf **GoogleBigQueryObject** fest. Bei diesem Dataset-Typ gibt es keine zusätzliche typspezifische Eigenschaft.

**Beispiel**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Google BigQuery-Quelltyp unterstützt werden.

### <a name="googlebigquerysource-as-a-source-type"></a>GoogleBigQuerySource als Quelltyp

Legen Sie zum Kopieren von Daten aus Google BigQuery den Quelltyp in der Kopieraktivität auf **GoogleBigQuerySource** fest. Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **GoogleBigQuerySource** festgelegt werden. | JA |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `"SELECT * FROM MyTable"`. | JA |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "GoogleBigQuerySource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
