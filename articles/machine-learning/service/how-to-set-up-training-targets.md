---
title: Einrichten von Computezielen zum Trainieren von Modellen mit dem Azure Machine Learning-Dienst | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Trainingsumgebungen (Computeziele) auswählen und konfigurieren, die zum Trainieren Ihrer Machine Learning-Modelle verwendet werden. Mit dem Azure Machine Learning-Dienst können Sie problemlos die Trainingsumgebungen wechseln. Beginnen Sie lokal mit dem Trainieren, und wenn ein horizontales Hochskalieren erforderlich ist, wechseln Sie zu einem cloudbasierten Computeziel.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 7eacc475145dac61db1717f1860e22cedd022262
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231446"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Auswählen und Verwenden eines Computeziels zum Trainieren Ihres Modells

Mit dem Azure Machine Learning-Dienst können Sie Ihr Modell in unterschiedlichen Umgebungen trainieren. Diese Umgebungen, die sogenannten __Computeziele__, können lokal oder in der Cloud gespeichert sein. In diesem Dokument erfahren Sie mehr über die unterstützten Computeziele und deren Verwendung.

Ein Computeziel ist die Ressource, die Ihr Trainingsskript ausführt oder Ihr Modell hostet, wenn es als Webdienst bereitgestellt wird. Sie können mithilfe des Azure Machine Learning-SDK oder der CLI erstellt und verwaltet werden. Wenn Sie über Computeziele verfügen, die von einem anderen Prozess (z.B. dem Azure-Portal oder der Azure-CLI) erstellt wurden, können Sie sie verwenden, indem Sie sie an den Arbeitsbereich Ihres Azure Machine Learning-Diensts anfügen.

Sie können mit lokalen Ausführungen auf Ihrem Computer beginnen und dann für andere Umgebungen hoch- und herunterskalieren, wie z.B. Data Science-Remote-VMs mit GPU oder Azure Batch AI. 

>[!NOTE]
> Der Code in diesem Artikel wurde mit der Azure Machine Learning SDK-Version 0.168 getestet. 

## <a name="supported-compute-targets"></a>Unterstützte Computeziele

Der Azure Machine Learning Service unterstützt die folgenden Computeziele:

|Computeziel| GPU-Beschleunigung | Automatisierte Hyperparameteroptimierung | Automatisierte Modellauswahl | Kann in Pipelines verwendet werden|
|----|:----:|:----:|:----:|:----:|
|[Lokaler Computer](#local)| Vielleicht | &nbsp; | ✓ | &nbsp; |
|[Data Science Virtual Machine (DSVM)](#dsvm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Batch AI](#batch)| ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>* Azure Databricks und Azure Data Lake Analytics können __nur__ in einer Pipeline verwendet werden. Weitere Informationen zu Pipelines finden Sie unter [Pipelines in Azure Machine Learning](concept-ml-pipelines.md).

__[Azure Container Instances (ACI)](#aci)__  kann ebenfalls zum Trainieren von Modellen verwendet werden. Dies ist ein serverloses Cloudangebot, das kostengünstig ist und eine einfache Erstellung und Verwendung ermöglicht. GPU-Beschleunigung, automatisierte Hyperparameteroptimierung und automatisierte Modellauswahl werden durch ACI nicht unterstützt. Außerdem kann es nicht in einer Pipeline verwendet werden.

Die wichtigsten Unterscheidungsmerkmale zwischen den Computezielen sind:
* __GPU-Beschleunigung__: GPUs sind mit Data Science Virtual Machine und Azure Batch AI verfügbar. Sie haben möglicherweise Zugriff auf eine GPU auf Ihrem lokalen Computer, abhängig von der Hardware, den Treibern und den installierten Frameworks.
* __Automatisierte Hyperparameteroptimierung__: Die automatisierte Hyperparameteroptimierung von Azure Machine Learning hilft Ihnen dabei, die besten Hyperparameter für Ihr Modell zu finden.
* __Automatisierte Modellauswahl__: Azure Machine Learning Service kann beim Erstellen eines Modells intelligente Empfehlungen für die Algorithmus- und Hyperparameterauswahl bereitstellen. Die automatisierte Modellauswahl hilft Ihnen beim schnelleren Konvergieren zu einem Modell mit hoher Qualität als beim manuellen Testen unterschiedlicher Kombinationen. Weitere Informationen finden Sie im Dokument [Tutorial: Train a classification model with automated machine learning in Azure Machine Learning](tutorial-auto-train-models.md) (Tutorial: Automatisches Trainieren eines Klassifizierungsmodells mit automatisiertem Machine Learning in Azure Machine Learning).
* __Pipelines__: Mit dem Azure Machine Learning Service haben Sie die Möglichkeit, verschiedene Aufgaben wie das Training und die Bereitstellung in einer Pipeline zu kombinieren. Pipelines können gleichzeitig oder nacheinander ausgeführt werden und stellen einen zuverlässigen Automatisierungsmechanismus bereit. Weitere Informationen finden Sie im Dokument [Pipelines and Azure Machine Learning](concept-ml-pipelines.md) (Pipelines und Azure Machine Learning).

Sie können das Azure Machine Learning-SDK, die Azure-CLI oder das Azure-Portal verwenden, um Computeziele zu erstellen. Sie können vorhandene Computeziele auch verwenden, indem Sie sie zu Ihrem Arbeitsbereich hinzufügen (anfügen).

> [!IMPORTANT]
> Eine vorhandene Azure Containers Instance kann nicht an Ihren Arbeitsbereich angefügt werden. Stattdessen müssen Sie eine neue Instanz erstellen.
>
> Sie können Azure HDInsight, Azure Databricks oder Azure Data Lake Storage nicht innerhalb eines Arbeitsbereichs erstellen. Stattdessen müssen Sie die Ressource erstellen und dann an Ihren Arbeitsbereich anfügen.

## <a name="workflow"></a>Workflow

Beim Workflow zum Entwickeln und Bereitstellen eines Modells mit Azure Machine Learning werden die folgenden Schritte ausgeführt:

1. Entwickeln von Machine Learning-Trainingsskripts in Python.
1. Erstellen und Konfigurieren oder Anfügen eines vorhandenen Computeziels.
1. Übermitteln der Trainingsskripts an das Computeziel.
1. Überprüfen der Ergebnisse, um das beste Modell zu ermitteln.
1. Registrieren des Modells in der Modellregistrierung.
1. Bereitstellen des Modells.

> [!IMPORTANT]
> Ihr Trainingsskript ist nicht an ein bestimmtes Computeziel gebunden. Sie können zunächst auf Ihrem lokalen Computer mit dem Trainieren beginnen und anschließend das Computeziel wechseln, ohne dass Sie das Trainingsskript umschreiben müssen.

Der Wechsel von einem Computeziel zu einem anderen umfasst das Erstellen einer [Laufzeitkonfiguration](concept-azure-machine-learning-architecture.md#run-configuration). Die Laufzeitkonfiguration definiert, wie das Skript auf dem Computeziel ausgeführt werden soll.

## <a name="training-scripts"></a>Trainingsskripts

Wenn Sie eine Trainingsausführung beginnen, wird das gesamte Verzeichnis übermittelt, das Ihre Trainingsskripts enthält. Eine Momentaufnahme wird erstellt und an das Computeziel gesendet. Weitere Informationen finden Sie auf unter [Snapshots](concept-azure-machine-learning-architecture.md#snapshot) (Momentaufnahmen).

## <a id="local"></a>Lokaler Computer

Beim lokalen Trainieren verwenden Sie das SDK zum Übermitteln des Trainingsvorgangs. Sie können eine vom Benutzer oder vom System verwaltete Umgebung zum Trainieren verwenden.

### <a name="user-managed-environment"></a>Vom Benutzer verwaltete Umgebung

In einer vom Benutzer verwalteten Umgebung müssen Sie sicherstellen, dass alle erforderlichen Pakete in der Python-Umgebung verfügbar sind, in der Sie das Skript ausführen möchten. Der folgende Codeausschnitt ist ein Beispiel für die Trainingskonfiguration für eine vom Benutzer verwaltete Umgebung:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

Ein Jupyter-Notebook, das das Training in einer vom Benutzer verwalteten Umgebung veranschaulicht, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).
  
### <a name="system-managed-environment"></a>Vom System verwaltete Umgebung

Vom System verwaltete Umgebungen hängen davon ab, dass Conda die Abhängigkeiten verwaltet. Conda erstellt eine Datei mit dem Namen `conda_dependencies.yml`, die eine Liste der Abhängigkeiten enthält. Sie können anschließend vom System die Erstellung einer neuen Conda-Umgebung anfordern und Ihre Skripts darin ausführen. Vom System verwaltete Umgebungen können später wiederverwendet werden, sofern die `conda_dependencies.yml`-Dateien unverändert bleiben. 

Bis eine neue Umgebung erstmals eingerichtet ist, können einige Minuten vergehen, je nach der Größe der erforderlichen Abhängigkeiten. Der folgende Codeausschnitt zeigt die Erstellung einer vom System verwalteten Umgebung, die von scikit-learn abhängig ist:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Ein Jupyter-Notebook, das das Training in einer vom System verwalteten Umgebung veranschaulicht, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).

## <a id="dsvm"></a>Data Science Virtual Machine

Der lokale Computer verfügt möglicherweise nicht über die Compute- oder GPU-Ressourcen, die zum Trainieren des Modells erforderlich sind. In diesem Fall können Sie den Trainingsprozess hoch- oder herunterskalieren, indem Sie zusätzliche Computeziele wie etwa Data Science Virtual Machines (DSVM) hinzufügen.

> [!WARNING]
> Azure Machine Learning unterstützt nur virtuelle Computer, die Ubuntu ausführen. Beim Erstellen oder Auswählen eines virtuellen Computers muss dieser Ubuntu verwenden.

Die folgenden Schritte verwenden das SDK zum Konfigurieren einer Data Science Virtual Machine (DSVM) als Trainingsziel:

1. Erstellen oder Anfügen eines virtuellen Computers
    
    * Zum Erstellen einer neuen DSVM überprüfen Sie zuerst, ob eine DSVM mit dem gleichen Namen vorhanden ist. Wenn nicht, erstellen einen neuen virtuellen Computer:
    
        ```python
        from azureml.core.compute import DsvmCompute
        from azureml.core.compute_target import ComputeTargetException

        compute_target_name = 'mydsvm'

        try:
            dsvm_compute = DsvmCompute(workspace = ws, name = compute_target_name)
            print('found existing:', dsvm_compute.name)
        except ComputeTargetException:
            print('creating new.')
            dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
            dsvm_compute = DsvmCompute.create(ws, name = compute_target_name, provisioning_configuration = dsvm_config)
            dsvm_compute.wait_for_completion(show_output = True)
        ```
    * Um einen vorhandenen virtuellen Computer als Computeziel anzufügen, müssen Sie den vollqualifizierten Domänennamen, den Anmeldenamen und das Kennwort für den virtuellen Computer bereitstellen.  Ersetzen Sie in diesem Beispiel ```<fqdn>``` durch den öffentlichen vollqualifizierten Domänennamen des virtuellen Computers oder die öffentliche IP-Adresse. Ersetzen Sie ```<username>``` und ```<password>``` durch den SSH-Benutzer und das Kennwort für den virtuellen Computer:

        ```python
        from azureml.core.compute import RemoteCompute

        dsvm_compute = RemoteCompute.attach(ws,
                                        name="attach-dsvm",
                                        username='<username>',
                                        address="<fqdn>",
                                        ssh_port=22,
                                        password="<password>")

        dsvm_compute.wait_for_completion(show_output=True)
    
   It takes around 5 minutes to create the DSVM instance.

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

1. Verwenden Sie den folgenden Code, um die Computeressourcen zu löschen, wenn Sie fertig sind:

    ```python
    dsvm_compute.delete()
    ```

Ein Jupyter-Notebook, das das Training auf einer Data Science-VM veranschaulicht, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb).

## <a id="batch"></a>Azure Batch AI

Wenn das Trainieren Ihres Modells einen langen Zeitraum in Anspruch nimmt, können Sie Azure Batch AI zum Verteilen des Trainings in einem Cluster von Computeressourcen in der Cloud verwenden. Batch AI kann auch zum Aktivieren einer GPU-Ressource konfiguriert werden.

Im folgenden Beispiel wird nach einem vorhandenen Batch AI-Cluster anhand des Namens gesucht. Wenn keiner gefunden wird, wird er erstellt:

```python
from azureml.core.compute import BatchAiCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
batchai_cluster_name = os.environ.get("BATCHAI_CLUSTER_NAME", ws.name + "gpu")
cluster_min_nodes = os.environ.get("BATCHAI_CLUSTER_MIN_NODES", 1)
cluster_max_nodes = os.environ.get("BATCHAI_CLUSTER_MAX_NODES", 3)
vm_size = os.environ.get("BATCHAI_CLUSTER_SKU", "STANDARD_NC6")
autoscale_enabled = os.environ.get("BATCHAI_CLUSTER_AUTOSCALE_ENABLED", True)


if batchai_cluster_name in ws.compute_targets():
    compute_target = ws.compute_targets()[batchai_cluster_name]
    if compute_target and type(compute_target) is BatchAiCompute:
        print('found compute target. just use it. ' + batchai_cluster_name)
else:
    print('creating a new compute target...')
    provisioning_config = BatchAiCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                vm_priority = 'lowpriority', # optional
                                                                autoscale_enabled = autoscale_enabled,
                                                                cluster_min_nodes = cluster_min_nodes, 
                                                                cluster_max_nodes = cluster_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

Zum Anfügen eines vorhandenen Batch AI-Clusters als Computeziel müssen Sie die Azure-Ressourcen-ID angeben. Führen Sie zum Abrufen der Ressourcen-ID beim Azure-Portal die folgenden Schritte aus:
1. Suchen Sie nach dem Dienst `Batch AI` unter **Alle Dienste**
1. Klicken Sie auf den Namen des Arbeitsbereichs, zu dem Ihr Cluster gehört.
1. Wählen Sie den Cluster aus.
1. Klicken Sie auf **Eigenschaften**.
1. Kopieren Sie die **ID**.

Im folgenden Beispiel wird das SDK zum Anfügen eines Clusters zu Ihrem Arbeitsbereich verwendet. Ersetzen Sie im Beispiel `<name>` durch einen Computenamen. Dieser muss nicht mit dem Namen des Clusters übereinstimmen. Ersetzen Sie `<resource-id>` durch die Azure-Ressourcen-ID (s.o.):

```python
from azureml.core.compute import BatchAiCompute
BatchAiCompute.attach(workspace=ws,
                      name=<name>,
                      resource_id=<resource-id>)
```

Sie können den Batch AI-Cluster und den Auftragsstatus mithilfe der folgenden Azure CLI-Befehle überprüfen:

- Überprüfen Sie den Clusterstatus. Mithilfe von `az batchai cluster list` können Sie anzeigen, wie viele Knoten ausgeführt werden.
- Überprüfen Sie den Auftragsstatus. Mithilfe von `az batchai job list` können Sie anzeigen, wie viele Aufträge ausgeführt werden.

Das Erstellen des Batch AI-Clusters dauert ca. fünf Minuten.

Ein Jupyter-Notebook, das das Training in einem Batch AI-Cluster veranschaulicht, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb).

## <a name='aci'></a>Azure Container Instance (ACI)

Azure Container Instances sind isolierte Container, die schnellere Startzeiten bieten und bei denen keine Verwaltung von virtuellen Computern durch die Benutzer erforderlich ist. Der Azure Machine Learning-Dienst verwendet Linux-Container, die in den Regionen „USA, Westen“, „USA, Osten“, „Europa, Westen“, „Europa, Norden“, „USA, Westen 2“ und „Asien, Südosten“ verfügbar sind. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](https://docs.microsoft.com/azure/container-instances/container-instances-quotas#region-availability). 

Das folgende Beispiel zeigt, wie Sie das SDK zum Erstellen eines ACI-Computeziels und zum Trainieren eines Modells verwenden: 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use ACI to run script.
run_config.target = "containerinstance"

# ACI container group is only supported in certain regions, which can be different than the region the Workspace is in.
run_config.container_instance.region = 'eastus'

# set the ACI CPU and Memory 
run_config.container_instance.cpu_cores = 1
run_config.container_instance.memory_gb = 2

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

# use conda_dependencies.yml to create a conda environment in the Docker image
run_config.environment.python.user_managed_dependencies = False

# auto-prepare the Docker image when used for the first time (if it is not already prepared)
run_config.auto_prepare_environment = True

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Das Erstellen eines ACI-Computeziels dauert zwischen einigen Sekunden bis zu einigen Minuten.

Ein Jupyter-Notebook, das das Training in einer Azure Container Instance veranschaulicht, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb).

## <a id="databricks"></a>Azure Databricks

Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Sie kann beim Trainieren von Modellen mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

> [!IMPORTANT]
> Ein Azure Databricks-Computeziel kann nur in einer Machine Learning-Pipeline verwendet werden.
>
> Sie müssen einen Azure Databricks-Arbeitsbereich erstellen, um ihn zum Trainieren Ihres Modells zu verwenden. Informationen zum Erstellen dieser Ressource finden Sie im Dokument [Ausführen eines Spark-Auftrags in Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Um Azure Databricks als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename:__ Der Name, der der Computeressource zugewiesen werden soll.
* __Ressourcen-ID:__ Die Ressourcen-ID des Azure Databricks-Arbeitsbereichs. Der folgende Text ist ein Beispiel für das Format dieses Werts:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.Databricks/workspaces/<databricks-workspace-name>
    ```

    > [!TIP]
    > Verwenden Sie zum Abrufen der Ressourcen-ID den folgenden Azure CLI-Befehl. Ersetzen Sie `<databricks-ws>` durch den Namen Ihres Databricks-Arbeitsbereichs:
    > ```azurecli-interactive
    > az resource list --name <databricks-ws> --query [].id
    > ```

* __Zugriffstoken:__ Das zur Authentifizierung bei Azure Databricks verwendete Zugriffstoken. Informationen zum Generieren eines Zugriffstokens finden Sie im Dokument [Authentifizierung](https://docs.azuredatabricks.net/api/latest/authentication.html).

Der folgende Code veranschaulicht, wie Azure Databricks als Computeziel angefügt wird:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_resource_id = os.environ.get("AML_DATABRICKS_RESOURCE_ID", "<databricks_resource_id>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_resource_id {}'.format(databricks_resource_id))
    print('databricks_access_token {}'.format(databricks_access_token))
    databricks_compute = DatabricksCompute.attach(
             workspace=ws,
             name=databricks_compute_name,
             resource_id=databricks_resource_id,
             access_token=databricks_access_token
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics ist eine umfangreiche Datenanalyseplattform in der Azure-Cloud. Sie kann beim Trainieren von Modellen mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

> [!IMPORTANT]
> Ein Azure Data Lake Analytics-Computeziel kann nur in einer Machine Learning-Pipeline verwendet werden.
>
> Sie müssen ein Azure Data Lake Analytics-Konto erstellen, um es zum Trainieren Ihres Modells zu verwenden. Informationen zum Erstellen dieser Ressource finden Sie im Dokument [Erste Schritte mit Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Um Data Lake Analytics als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename:__ Der Name, der der Computeressource zugewiesen werden soll.
* __Ressourcen-ID:__ Die Ressourcen-ID des Data Lake Analytics-Kontos. Der folgende Text ist ein Beispiel für das Format dieses Werts:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.DataLakeAnalytics/accounts/<datalakeanalytics-name>
    ```

    > [!TIP]
    > Verwenden Sie zum Abrufen der Ressourcen-ID den folgenden Azure CLI-Befehl. Ersetzen Sie `<datalakeanalytics>` durch den Namen Ihres Data Lake Analytics-Kontos:
    > ```azurecli-interactive
    > az resource list --name <datalakeanalytics> --query [].id
    > ```

Der folgende Code veranschaulicht, wie Data Lake Analytics als Computeziel angefügt wird:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_id = os.environ.get("AML_ADLA_RESOURCE_ID", "<adla_resource_id>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_id))
    adla_compute = AdlaCompute.attach(
             workspace=ws,
             name=adla_compute_name,
             resource_id=adla_resource_id
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning-Pipelines können nur ausgeführt werden, wenn die Daten im Standarddatenspeicher des Data Lake Analytics-Kontos gespeichert werden. Wenn die Daten, die Sie verwenden möchten, in einem nicht standardmäßigen Speicher gespeichert sind, können Sie die Daten mithilfe von [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) vor dem Trainieren kopieren.

## <a id="hdinsight"></a>Anfügen eines HDInsight-Clusters 

HDInsight ist eine beliebte Plattform für Big Data-Analysen. Sie stellt Apache Spark bereit, das zum Trainieren Ihres Modells verwendet werden kann.

> [!IMPORTANT]
> Erstellen Sie den HDInsight-Cluster, bevor Sie ihn zum Trainieren Ihres Modells verwenden. Informationen zum Erstellen von Spark auf einem HDInsight-Cluster finden Sie unter [Schnellstart: Erstellen eines Spark-Clusters in HDInsight mithilfe einer Vorlage](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Beim Erstellen des Clusters müssen Sie einen SSH-Benutzernamen und ein Kennwort angeben. Notieren Sie diese Werte, da Sie sie benötigen, wenn Sie HDInsight als Computeziel verwenden.
>
> Sobald der Cluster erstellt wurde, hat er den vollqualifizierten Domänennamen (FQDN) `<clustername>.azurehdinsight.net`, wobei `<clustername>` der Name ist, die Sie für den Cluster angegeben haben. Sie benötigen diese Adresse (oder die öffentliche IP-Adresse des Clusters), um sie als Computeziel zu verwenden.

Um HDInsight als Computeziel zu konfigurieren, müssen Sie den vollqualifizierten Domänennamen, den Clusteranmeldenamen und das Kennwort für den HDInsight-Cluster bereitstellen. Im folgenden Beispiel wird das SDK zum Anfügen eines Clusters zu Ihrem Arbeitsbereich verwendet. Ersetzen Sie in diesem Beispiel `<fqdn>` durch den öffentlichen vollqualifizierten Domänennamen des Clusters oder die öffentliche IP-Adresse. Ersetzen Sie `<username>` und `<password>` durch den SSH-Benutzer und das Kennwort für den Cluster:

> [!NOTE]
> Besuchen Sie das Azure-Portal, und wählen Sie Ihren HDInsight-Cluster aus, um den FQDN für Ihren Cluster zu finden. Im Abschnitt __Übersicht__ ist der FQDN Teil des Eintrags __URL__. Entfernen Sie einfach `https://` am Anfang des Werts.
>
> ![Screenshot der HDInsight-Clusterübersicht mit hervorgehobenem URL-Eintrag](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Übermitteln der Trainingsausführung

Eine Trainingsausführung kann auf zwei Arten übermittelt werden:

* Übermitteln eines `ScriptRunConfig`-Objekts
* Übermitteln eines `Pipeline`-Objekts

> [!IMPORTANT]
> Die Azure Databricks- und Azure Data Lake Analytics-Computeziele können nur in einer Pipeline verwendet werden.
> Das lokale Computeziel kann nicht in einer Pipeline verwendet werden.

### <a name="submit-using-scriptrunconfig"></a>Übermitteln mithilfe von `ScriptRunConfig`

Das Codemuster zum Übermitteln einer Trainingsausführung mithilfe von `ScriptRunConfig` ist unabhängig vom Computeziel immer gleich:

* Erstellen Sie ein `ScriptRunConfig`-Objekt mithilfe der Ausführungskonfiguration für das Computeziel.
* Übermitteln Sie die Ausführung.
* Warten Sie, bis die Ausführung abgeschlossen ist.

Im folgenden Beispiel wird die Konfiguration für das vom System verwaltete lokale Computeziel verwendet, die weiter oben in diesem Dokument erstellt wurde:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```

Ein Jupyter-Notebook, das das Training mit Spark auf HDInsight veranschaulicht, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb).

### <a name="submit-using-a-pipeline"></a>Übermitteln über eine Pipeline

Das Codemuster zum Übermitteln einer Trainingsausführung über eine Pipeline ist unabhängig vom Computeziel immer gleich:

* Fügen Sie der Pipeline einen Schritt für die Computeressource hinzu.
* Übermitteln Sie eine Ausführung über die Pipeline.
* Warten Sie, bis die Ausführung abgeschlossen ist.

Im folgenden Beispiel wird das Azure Databricks-Computeziel verwendet, das weiter oben in diesem Dokument erstellt wurde:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Weitere Informationen zu Machine Learning-Pipelines finden Sie im Dokument [Pipelines und Azure Machine Learning](concept-ml-pipelines.md).

Beispiele für Jupyter Notebooks, die das Training über eine Pipeline veranschaulichen, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Computeanzeige und -einrichtung mithilfe des Azure-Portals

Sie können anzeigen, welche Computeziele Ihrem Arbeitsbereich aus dem Azure-Portal zugeordnet sind. Führen Sie die folgenden Schritte aus, um die Liste abzurufen:

1. Besuchen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu Ihrem Arbeitsbereich.
2. Klicken Sie auf den Link __Compute__im Abschnitt __Anwendungen__.

    ![Registerkarte „Computer“ anzeigen](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Erstellen eines Computeziels

Führen Sie die oben genannten Schritte aus, um eine Liste von Computezielen anzuzeigen, und führen Sie dann die folgenden Schritte aus, um ein Computeziel zu erstellen:

1. Klicken Sie auf das Symbol __+__, um ein Computeziel hinzuzufügen.

    ![Compute hinzufügen ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Geben Sie einen Namen für das Computeziel ein.
1. Wählen Sie den Computetyp aus, der für das __Training__ angefügt werden soll. 

    > [!IMPORTANT]
    > Nicht alle Computetypen können im Azure-Portal erstellt werden. Derzeit können folgende Typen für das Training erstellt werden:
    > 
    > * Virtual Machine
    > * Batch AI

1. Wählen Sie __Neu erstellen__, und füllen Sie das erforderliche Formular aus. 
1. Klicken Sie auf __Erstellen__
1. Sie können den Status des Erstellvorgangs anzeigen, indem Sie das Computeziel aus der Liste auswählen.

    ![Anzeigen der Computeliste](./media/how-to-set-up-training-targets/View_list.png) Anschließend werden die Details für das Computeziel angezeigt.
    ![Details anzeigen](./media/how-to-set-up-training-targets/vm_view.PNG)
1. Jetzt können Sie eine Ausführung für Ziele wie die oben genannten übermitteln.

### <a name="reuse-existing-compute-in-your-workspace"></a>Wiederverwenden von vorhandenem Compute in Ihrem Arbeitsbereich

Führen Sie die oben genannten Schritte aus, um eine Liste von Computezielen anzuzeigen, und führen Sie dann die folgenden Schritte aus, um ein Computeziel wiederzuverwenden:

1. Klicken Sie auf das Symbol **+**, um ein Computeziel hinzuzufügen.
2. Geben Sie einen Namen für das Computeziel ein.
3. Wählen Sie den Computetyp aus, der für das Training angefügt werden soll.

    > [!IMPORTANT]
    > Nicht alle Computetypen können im Portal angefügt werden.
    > Derzeit können folgende Typen für das Training angefügt werden:
    > 
    > * Virtual Machine
    > * Batch AI

1. Wählen Sie „Vorhandene verwenden“.
    - Wählen Sie beim Anfügen von Batch AI-Clustern das Computeziel aus der Dropdownliste aus, wählen Sie den Batch AI-Arbeitsbereich und der Batch AI-Cluster, und klicken Sie dann auf **Erstellen**.
    - Geben Sie beim Anfügen eines virtuellen Computers die IP-Adresse, die Kombination aus Benutzername und Kennwort, private/öffentliche Schlüssel und den Port an, und klicken Sie auf „Erstellen“.

    > [!NOTE]
    > Microsoft empfiehlt die Verwendung von SSH-Schlüsseln, da sie sicherer als Kennwörter sind. Kennwörter sind anfällig für Brute-Force-Angriffen, während SSH-Schlüssel auf kryptografischen Signaturen basieren. Informationen zum Erstellen von SSH-Schlüsseln für die Verwendung mit Azure Virtual Machines finden Sie in den folgenden Dokumenten:
    >
    > * [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Verwenden von SSH-Schlüsseln mit Windows in Azure]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

5. Sie können den Bereitstellungsstatus anzeigen, indem Sie das Computeziel in der Liste auswählen.
6. Jetzt können Sie eine Ausführung für diese Ziele übermitteln.

## <a name="examples"></a>Beispiele
Die folgenden Notebooks verdeutlichen die Konzepte in diesem Artikel:
* [01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm)
* [01.getting-started/03.train-on-aci/03.train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci)
* [01.getting-started/05.train-in-spark/05.train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark)
* [tutorials/01.train-models.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb)

Rufen Sie die Notebooks ab: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [What is the Azure Machine Learning SDK for Python?](https://aka.ms/aml-sdk) (Was ist das Azure Machine Learning-SDK für Python?)
* [Train an image classification model with Azure Machine Learning](tutorial-train-models-with-aml.md) (Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning)
* [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst)
* [Pipelines and Azure Machine Learning](concept-ml-pipelines.md) (Pipelines und Azure Machine Learning)
