---
title: 'DevOps für Anwendungen der Künstlichen Intelligenz (KI): Erstellen einer Continuous Integration-Pipeline in Azure mit der Docker, Kubernetes und der Python Flask-Anwendung'
description: 'DevOps für Anwendungen der Künstlichen Intelligenz (KI): Erstellen einer Continuous Integration-Pipeline in Azure mit der Docker und Kubernetes'
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 05/22/2018
ms.author: tdsp
ms.custom: (previous author=jainr, ms.author=jainr)
ms.openlocfilehash: c232680d5d1bf0eb761ff974ebf6608b67922f33
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496748"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>DevOps für Anwendungen der Künstlichen Intelligenz (KI): Erstellen einer Continuous Integration-Pipeline in Azure mit der Docker und Kubernetes
Für eine KI-Anwendung gibt es häufig zwei Arbeitsbereiche: Data Scientists, die Modelle für maschinelles Lernen erstellen, und App-Entwickler, die die Anwendung programmieren und den Endbenutzern zur Verfügung stellen. In diesem Artikel wird veranschaulicht, wie Sie eine Pipeline für Continuous Integration (CI) bzw. Continuous Delivery (CD) für eine KI-Anwendung implementieren. Eine KI-Anwendung ist eine Kombination aus Anwendungscode, der mit einem vortrainierten Modell für maschinelles Lernen (MK) eingebettet ist. In diesem Artikel wird ein vortrainiertes Modell aus einem privaten Azure Blob Storage-Konto genutzt, wobei auch ein AWS S3-Konto verwendet werden könnte. Zudem wird eine einfache Python Flask-Webanwendung in diesem Artikel verwendet.

> [!NOTE]
> Dies ist eine von mehreren Möglichkeiten, wie CI/CD durchgeführt werden kann. Es gibt jedoch auch Alternativen zu den unten genannten Tools und anderen Voraussetzungen. Weitere entwickelte Inhalte werden veröffentlicht, sobald diese verfügbar sind.
>
>

## <a name="github-repository-with-document-and-code"></a>GitHub-Repository mit Dokument und Code
Sie können den Quellcode von [GitHub](https://github.com/Azure/DevOps-For-AI-Apps) herunterladen. Ein [detailliertes Tutorial](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) steht ebenfalls zur Verfügung.

## <a name="pre-requisites"></a>Voraussetzungen
Im Folgenden sind die Voraussetzungen für die nachfolgend beschriebene CI/CD-Pipeline aufgeführt:
* [Azure DevOps-Organisation](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure Container Service (AKS)-Cluster mit Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Azure Container Registry-Konto (ACR)](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)
* [Installation von Kubectl zum Ausführen von Befehlen für den Kubernetes-Cluster](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Dies ist erforderlich, um die Konfiguration aus dem ACS-Cluster abzurufen. 
* Fork des Repositorys zu Ihrem GitHub-Konto

## <a name="description-of-the-cicd-pipeline"></a>Beschreibung der CI/CD-Pipeline
Die Pipeline startet für jeden neuen Commit, führt die Testsammlung aus, nimmt bei bestandenem Test den neuesten Build und erstellt daraus ein Paket in einen Docker-Container. Der Container wird dann über Azure Container Service (ACS) bereitgestellt, und die Images werden sicher in Azure Container Registry (ACR) gespeichert. ACS führt Kubernetes für die Verwaltung von Containerclustern aus, Sie können hierfür aber auch Docker Swarm oder Mesos wählen.

Die Anwendung pullt sicher das neueste Modell aus einem Azure Storage-Konto und verpackt dieses als Teil der Anwendung. Die bereitgestellte Anwendung verfügt über den App-Code und das als Einzelcontainer verpackte ML-Modell. So werden App-Entwickler und Data Scientists entkoppelt, um sicherzustellen, dass ihre Produktions-App immer den neuesten Code mit dem neuesten ML-Modell ausführt.

Unten sehen Sie die Pipelinearchitektur. 

![Architecture](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>Schritte der CI/CD-Pipeline
1. Entwickler arbeiten an der IDE ihrer Wahl am Anwendungscode.
2. Sie committen den Code bei der Quellcodeverwaltung ihrer Wahl (Azure DevOps bietet gute Unterstützung für verschiedene Quellcodeverwaltungen).
3. Die Data Scientists arbeiten separat an der Entwicklung ihres Modells.
4. Sobald sie mit dem Ergebnis zufrieden sind, veröffentlichen sie das Modell in einem Modellrepository. In diesem Fall wird ein Blob Storage-Konto verwendet. 
5. Ein Build wird in Azure DevOps basierend auf dem Commit in GitHub gestartet.
6. Die Azure DevOps-Buildpipeline pullt das neueste Modell aus dem Blobcontainer und erstellt einen Container.
7. Azure DevOps pusht das Image in ein privates Imagerepository in Azure Container Registry.
8. Nach einem festgelegten Zeitplan (nachts) wird die Releasepipeline gestartet.
9. Das neueste Image aus ACR wird über den Kubernetes-Cluster in ACS gepullt und bereitgestellt.
10. Die Benutzeranforderung der App erfolgt über den DNS-Server.
11. Der DNS-Server übergibt die Anforderung an das Lastenausgleichsmodul und sendet die Antwort an den Benutzer zurück.

## <a name="next-steps"></a>Nächste Schritte
* Details und Anweisungen zum Implementieren Ihrer eigenen CI/CD-Pipeline für Ihre Anwendung finden Sie im [Tutorial](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md).

## <a name="references"></a>Referenzen
* [Team Data Science-Prozess (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/azure/machine-learning/service/)
* [Azure DevOps](https://www.visualstudio.com/vso/)
* [Azure Kubernetes Services (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)
