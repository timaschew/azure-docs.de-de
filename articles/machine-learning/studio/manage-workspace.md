---
title: Verwalten eines Machine Learning-Arbeitsbereichs – Azure Machine Learning Studio | Microsoft-Dokumentation
description: Verwalten des Zugriffs auf Azure Machine Learning-Arbeitsbereiche sowie Bereitstellen und Verwalten von ML-API-Webdiensten
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: c470d10b933b31d4cfe151fb541c2182562a2805
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314103"
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Verwalten eines Azure Machine Learning-Arbeitsbereichs

> [!NOTE]
> Informationen zum Verwalten von Webdiensten im Machine Learning Web Services-Portal finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](manage-new-webservice.md).
> 
> 

Sie können Machine Learning-Arbeitsbereiche im Azure-Portal verwalten.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So verwalten Sie einen Arbeitsbereich im Azure-Portal

1. Melden Sie sich über das Administratorkonto eines Azure-Abonnements beim [Azure-Portal](https://portal.azure.com/) an.
2. Geben Sie im Suchfeld oben auf der Seite „Machine Learning-Arbeitsbereiche“ ein, und wählen Sie dann **Machine Learning-Arbeitsbereiche** aus.
3. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Neben den Standardinformationen und -optionen zur Ressourcenverwaltung stehen folgende Optionen zur Verfügung:

- **Eigenschaften anzeigen:** Auf dieser Seite werden Informationen zum Arbeitsbereich und der Ressource angezeigt. Zudem können Sie hier das Abonnement und die Ressourcengruppe ändern, mit denen der Arbeitsbereich verbunden ist.
- **Speicherschlüssel neu synchronisieren:** Im Arbeitsbereich werden die Schlüssel für das Speicherkonto verwaltet. Wenn sich die Schlüssel für das Speicherkonto ändern, können Sie auf **Schlüssel neu synchronisieren** klicken, um die Schlüssel mit dem Arbeitsbereich zu synchronisieren.

Verwenden Sie zum Verwalten der dem Arbeitsbereich zugeordneten Webdienste das Portal für Machine Learning-Webdienste. Ausführliche Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](manage-new-webservice.md).

> [!NOTE]
> Zum Bereitstellen oder Verwalten neuer Webdienste muss Ihnen in dem Abonnement, für das der Webdienst bereitgestellt wird, eine Teilnehmer- oder Administratorrolle zugewiesen sein. Wenn Sie andere Benutzer in einen Machine Learning-Arbeitsbereich einladen, müssen Sie ihnen eine Teilnehmer- oder Administratorrolle im Abonnement zuweisen, damit sie Webdienste bereitstellen oder verwalten können. 
> 
>Weitere Informationen zum Festlegen von Zugriffsberechtigungen finden Sie unter [Verwalten des Zugriffs mit RBAC und dem Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Bereitstellen von Machine Learning mithilfe von Azure Resource Manager-Vorlagen](deploy-with-resource-manager-template.md). 