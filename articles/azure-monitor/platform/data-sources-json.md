---
title: Erfassen benutzerdefinierter JSON-Daten in Log Analytics | Microsoft-Dokumentation
description: Benutzerdefinierte JSON-Datenquellen können in Log Analytics unter Verwendung des Log Analytics-Agents für Linux erfasst werden.  Diese benutzerdefinierten Datenquellen können einfache Skripts sein, die JSON zurückgeben, z.B. Curl oder eines von mehr als 300 Plug-Ins von FluentD. Dieser Artikel beschreibt die Konfiguration, die für diese Datenerfassung erforderlich ist.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: c8972df6f42920af6a9bd5f04a27f14dc647da44
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336472"
---
# <a name="collecting-custom-json-data-sources-with-the-log-analytics-agent-for-linux-in-log-analytics"></a>Erfassen benutzerdefinierter JSON-Datenquellen mit dem Log Analytics-Agent für Linux in Log Analytics
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]
Benutzerdefinierte JSON-Datenquellen können in Log Analytics unter Verwendung des Log Analytics-Agents für Linux erfasst werden.  Diese benutzerdefinierten Datenquellen können einfache Skripts sein, die JSON zurückgeben, wie z.B. [Curl](https://curl.haxx.se/) oder eines von mehr als [300 FluentD-Plug-Ins](http://www.fluentd.org/plugins/all). Dieser Artikel beschreibt die Konfiguration, die für diese Datenerfassung erforderlich ist.

> [!NOTE]
> Der Log Analytics-Agent für Linux v1.1.0-217+ ist für benutzerdefinierte JSON-Daten erforderlich.

## <a name="configuration"></a>Konfiguration

### <a name="configure-input-plugin"></a>Konfigurieren des Eingabe-Plug-Ins

Fügen Sie zum Erfassen von JSON-Daten in Log Analytics `oms.api.` am Anfang eines FluentD-Tags in einem Eingabe-Plug-In hinzu.

Folgendes ist z.B. eine separate Konfigurationsdatei `exec-json.conf` in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Dabei wird das FluentD-Plug-In `exec` verwendet, um im Abstand von 30 Sekunden einen Curl-Befehl auszuführen.  Die Ausgabe dieses Befehls wird vom JSON-Ausgabe-Plug-In erfasst.

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
Der Besitz für die unter `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` hinzugefügte Konfigurationsdatei muss mithilfe des folgenden Befehls geändert werden.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Konfigurieren des Ausgabe-Plug-Ins 
Fügen Sie die folgende Konfiguration für das Ausgabe-Plug-In der Hauptkonfiguration in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` oder als separate Konfigurationsdatei `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` hinzu

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-log-analytics-agent-for-linux"></a>Neustarten des Log Analytics-Agents für Linux
Starten Sie den Log Analytics-Agent für den Linux-Dienst mit dem folgenden Befehl neu.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Output
Die Daten werden in die Log Analytics mit einem Datensatz vom Typ `<FLUENTD_TAG>_CL` erfasst.

Beispiel: Das benutzerdefinierte Tag `tag oms.api.tomcat` in Log Analytics mit einem Datensatz vom Typ `tomcat_CL`.  Sie können alle Datensätze dieses Typs mit der folgenden Protokollsuche abrufen.

    Type=tomcat_CL

Geschachtelte Datenquellen werden unterstützt, aber auf der Grundlage des übergeordneten Felds indiziert. Die folgenden Daten werden z.B. von einer Log Analytics-Suche als `tag_s : "[{ "a":"1", "b":"2" }]` zurückgegeben.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](../../log-analytics/log-analytics-queries.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 
 
