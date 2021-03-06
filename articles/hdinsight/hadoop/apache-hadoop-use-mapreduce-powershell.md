---
title: 'Verwenden von MapReduce und PowerShell mit Apache Hadoop: Azure HDInsight'
description: Hier erfahren Sie, wie MapReduce-Aufträge mithilfe von PowerShell mit Apache Hadoop in HDInsight remote ausgeführt werden.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: hrasheed
ms.openlocfilehash: 753a0ad72e1d4b60a93daa570ceecc25d21bb228
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634140"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-powershell"></a>Ausführen von MapReduce-Aufträgen mit Apache Hadoop in HDInsight mithilfe von PowerShell

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Dieses Dokument enthält ein Beispiel zur Verwendung von Azure PowerShell zum Ausführen eines MapReduce-Auftrags in einem Hadoop-Cluster in HDInsight.

## <a id="prereq"></a>Voraussetzungen

* **Einen Azure HDInsight-Cluster (Hadoop in HDInsight)**

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Eine Arbeitsstation mit Azure PowerShell**.

## <a id="powershell"></a>Ausführen eines MapReduce-Auftrags

Azure PowerShell stellt *Cmdlets* bereit, mit denen Sie MapReduce-Aufträge in HDInsight remote ausführen können. Intern sendet PowerShell REST-Aufrufe an die auf dem HDInsight-Cluster ausgeführte Anwendung [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (früher Templeton genannt).

Die folgenden Cmdlets werden zum Ausführen der MapReduce-Aufträge in einem HDInsight-Remotecluster verwendet.

* **Connect-AzureRmAccount**: Authentifiziert Azure PowerShell für Ihr Azure-Abonnement.

* **New-AzureRmHDInsightMapReduceJobDefinition**: Erstellt aus den angegebenen MapReduce-Informationen eine neue *Auftragsdefinition*.

* **Start-AzureRmHDInsightJob**: Sendet die Auftragsdefinition an HDInsight und startet den Auftrag. Ein *Auftragsobjekt* wird zurückgegeben.

* **Wait-AzureRmHDInsightJob**: verwendet das Auftragsobjekt, um den Status des Auftrags zu prüfen. Es wird gewartet, bis der Auftrag abgeschlossen oder die Wartezeit überschritten ist.

* **Get-AzureRmHDInsightJobOutput**: Wird zum Abrufen der Ausgabe des Auftrags verwendet.

Die folgenden Schritte veranschaulichen, wie diese Cmdlets zum Ausführen eines Auftrags in einem HDInsight-Cluster verwendet werden.

1. Speichern Sie den folgenden Code in einem Editor in einer Datei namens **mapreducejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Öffnen Sie eine neue **Azure PowerShell** -Befehlsaufforderung. Navigieren Sie zum Speicherort der Datei **mapreducejob.ps1** , und verwenden Sie folgenden Befehl zum Ausführen des Skripts.

        .\mapreducejob.ps1

    Wenn Sie das Skript ausführen, werden Sie dazu aufgefordert, den Namen des HDInsight-Clusters und die Anmeldedaten für den Cluster einzugeben. Sie werden außerdem möglicherweise dazu aufgefordert, sich bei Ihrem Azure-Abonnement zu authentifizieren.

3. Nach Abschluss des Auftrags erhalten Sie eine Ausgabe ähnlich der folgenden:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Diese Ausgabe gibt an, dass der Auftrag erfolgreich abgeschlossen wurde.

    > [!NOTE]
    > Wenn sich für **ExitCode** ein anderer Wert als 0 ergibt, finden Sie weitere Informationen unter [Troubleshooting](#troubleshooting).

    In diesem Beispiel werden außerdem die heruntergeladenen Dateien in der Datei **output.txt** in dem Verzeichnis gespeichert, von dem aus Sie das Skript ausführen.

### <a name="view-output"></a>Anzeigen der Ausgabe

Öffnen Sie die Datei **output.txt** in einem Texteditor, um die von dem Auftrag erzeugten Wörter und Zählerstände anzuzeigen.

> [!NOTE]
> Die Ausgabedateien eines MapReduce-Auftrags sind unveränderlich. Wenn Sie dieses Beispiel erneut ausführen, müssen Sie daher den Namen der Ausgabedatei ändern.

## <a id="troubleshooting"></a>Problembehandlung

Wenn keine Informationen zurückgegeben werden, wenn der Auftrag abgeschlossen ist, zeigen Sie die Fehlermeldungen für den Auftrag an. Um Fehlerinformationen für diesen Auftrag anzuzeigen, fügen Sie folgenden Befehl am Ende der Datei **mapreducejob.ps1** hinzu. Anschließend speichern Sie die Datei und führen sie erneut aus.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Mit diesem Cmdlet werden die Informationen zurückgegeben, die bei der Auftragsverarbeitung in STDERR geschrieben wurden.

## <a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, um MapReduce-Aufträge auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce-Aufträgen in HDInsight:

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
