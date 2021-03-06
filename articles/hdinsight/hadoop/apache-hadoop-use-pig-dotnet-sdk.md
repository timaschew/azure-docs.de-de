---
title: Ausführen von Apache Pig-Aufträgen mit dem .NET SDK für Hadoop – Azure HDInsight
description: Erfahren Sie, wie Sie mithilfe des .NET SDK für Hadoop Pig-Aufträge an Hadoop in HDInsight übermitteln.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: hrasheed
ms.openlocfilehash: 0aa45ae807f153e6f1a3fde1d648571b29802dc2
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632474"
---
# <a name="run-apache-pig-jobs-using-the-net-sdk-for-apache-hadoop-in-hdinsight"></a>Ausführen von Apache Pig-Aufträgen mithilfe des .NET SDK für Apache Hadoop in HDInsight

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Hier erfahren Sie, wie Sie mithilfe des .NET SDK für Apache Hadoop Apache Pig-Aufträge an Hadoop in Azure HDInsight übermitteln.

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. Mithilfe von Pig können Sie MapReduce-Vorgänge erstellen, indem Sie eine Reihe von Datentransformationen modellieren. In diesem Dokument erfahren Sie, wie Sie mit einer einfachen C#-Anwendung einen Pig-Auftrag an einen HDInsight-Cluster übermitteln.

## <a name="prerequisites"></a>Voraussetzungen

Zur Ausführung der in diesem Artikel aufgeführten Schritte benötigen Sie Folgendes.

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Windows oder Linux basiert

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012, 2013, 2015 oder 2017.

## <a name="create-the-application"></a>Erstellen der Anwendung

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen.

1. Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.

2. Geben Sie für das neue Projekt die folgenden Werte ein, oder wählen Sie sie aus:

   | Eigenschaft | Wert |
   | ------ | ------ |
   | Category (Kategorie) | Vorlagen/Visual C#/Windows |
   | Vorlage | Konsolenanwendung |
   | NAME | SubmitPigJob |

3. Klicken Sie auf **OK**, um das Projekt zu erstellen.

4. Klicken Sie im Menü **Extras** auf **Bibliothekspaket-Manager** oder **NuGet-Paket-Manager** und anschließend auf **Paket-Manager-Konsole**.

5. Verwenden Sie den folgenden Befehl, um die .NET SDK-Pakete zu installieren:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Doppelklicken Sie im Projektmappen-Explorer auf **Program.cs** , um die Datei zu öffnen. Ersetzen Sie den vorhandenen Code durch den folgenden Code:

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitPigJob
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. Drücken Sie **F5**, um die Anwendung zu starten.

8. Drücken Sie die **EINGABETASTE** , um die Anwendung zu beenden.

## <a name="next-steps"></a>Nächste Schritte

Informationen über Pig in HDInsight finden Sie unter [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md).

Weitere Informationen zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Dokumenten:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
