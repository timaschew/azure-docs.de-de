---
title: Übersicht über die Zugriffssteuerung in Data Lake Storage Gen1 | Microsoft-Dokumentation
description: Grundlegende Informationen zur Funktionsweise der Zugriffssteuerung in Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 08991829c9c3d628b5028e04dbd4836647d94826
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567484"
---
# <a name="access-control-in-azure-data-lake-storage-gen1"></a>Zugriffssteuerung in Azure Data Lake Storage Gen1

Das von Azure Data Lake Storage Gen1 implementierte Zugriffssteuerungsmodell leitet sich von HDFS ab, das wiederum vom POSIX-Zugriffssteuerungsmodell abgeleitet wird. In diesem Artikel werden die Grundlagen des Zugriffssteuerungsmodells für Data Lake Storage Gen1 zusammengefasst. 

## <a name="access-control-lists-on-files-and-folders"></a>Zugriffssteuerungslisten für Dateien und Ordner

Es gibt zwei Arten von Zugriffssteuerungslisten (Access Control Lists, ACLs): **Zugriffs-ACLs** und **Standard-ACLs**.

* **Zugriffs-ACLs**: Diese Listen steuern den Zugriff auf ein Objekt. Dateien und Ordner verfügen jeweils über Zugriffs-ACLs.

* **Standard-ACLs**: Hierbei handelt es sich um eine Art Vorlage für ACLs, die einem Ordner zugeordnet sind und die Zugriffs-ACLs für alle untergeordneten Elemente bestimmen, die in unter diesem Ordner erstellt werden. Dateien verfügen über keine Standard-ACLs.


Zugriffs- und Standard-ACLs besitzen die gleiche Struktur.



> [!NOTE]
> Änderungen an der Standard-ACL für ein übergeordnetes Element haben keine Auswirkungen auf die Zugriffs- oder Standard-ACL bereits vorhandener untergeordneter Elemente.
>
>

## <a name="permissions"></a>Berechtigungen

Die Berechtigungen für ein Dateisystemobjekt sind **Lesen**, **Schreiben** und **Ausführen**. Sie können wie in der folgenden Tabelle beschrieben auf Dateien und Ordner angewendet werden:

|            |    File     |   Ordner |
|------------|-------------|----------|
| **Lesen (Read, R)** | Berechtigt zum Lesen von Dateiinhalten | Erfordert **Lesen** und **Ausführen**, um den Inhalt des Ordners aufzulisten.|
| **Schreiben (Write, W)** | Berechtigt zum Schreiben in eine Datei sowie zum Anfügen an eine Datei | Erfordert **Schreiben** und **Ausführen**, um untergeordnete Elemente in einem Ordner zu erstellen. |
| **Ausführen (Execute, X)** | Hat im Kontext von Data Lake Storage Gen1 keine Bedeutung | Erfordert das Durchlaufen der untergeordneten Elemente eines Ordners. |

### <a name="short-forms-for-permissions"></a>Kurzformen für Berechtigungen

**RWX** steht für **Lesen (Read), Schreiben (Write) und Ausführen (Execute)**. Es gibt auch ein noch kürzeres numerisches Format. Hierbei steht **4 für Lesen**, **2 für Schreiben** und **1 für Ausführen**, und Berechtigungen werden als Summe dieser Werte angegeben. Hier einige Beispiele.

| Numerische Form | Kurzform |      Bedeutung     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Lesen, Schreiben und Ausführen |
| 5            | `R-X`        | Lesen und Ausführen         |
| 4            | `R--`        | Lesen                   |
| 0            | `---`        | Keine Berechtigungen         |


### <a name="permissions-do-not-inherit"></a>Keine Vererbung bei Berechtigungen

Im von Data Lake Storage Gen1 verwendeten POSIX-basierten Modell werden Berechtigungen für ein Element direkt im Element gespeichert. Berechtigungen für ein Element können also nicht von den übergeordneten Elementen geerbt werden.

## <a name="common-scenarios-related-to-permissions"></a>Allgemeine Szenarien im Zusammenhang mit Berechtigungen

Im Folgenden sind einige allgemeine Szenarien aufgeführt, die veranschaulichen, welche Berechtigungen zum Ausführen bestimmter Vorgänge für ein Data Lake Storage Gen1-Konto erforderlich sind.

|    Vorgang             |    /    | Seattle/ | Portland/ | Data.txt     |
|--------------------------|---------|----------|-----------|--------------|
| Lesen von „Data.txt“            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Anfügen an „Data.txt“       |   `--X`   |   `--X`    |  `--X`      | `RW-`          |
| Löschen von „Data.txt“          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Erstellen von „Data.txt“          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Auflisten von „/“                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Auflisten von „/Seattle/“           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Auflisten von „/Seattle/Portland/“  |   `--X`   |   `--X`    |  `R-X`      | `---`          |


> [!NOTE]
> Wenn die beiden obigen Bedingungen erfüllt sind, werden zum Löschen der Datei keine Schreibberechtigungen für die Datei benötigt.
>
>


## <a name="users-and-identities"></a>Benutzer und Identitäten

Alle Dateien und Ordner verfügen über eigene Berechtigungen für folgende Identitäten:

* Der zuständige Benutzer
* Die zuständige Gruppe
* Benannte Benutzer
* Benannte Gruppen
* Alle anderen Benutzer

Die Identitäten von Benutzern und Gruppen sind Azure AD-Identitäten (Azure Active Directory). Sofern nichts anderes angegeben ist, kann ein „Benutzer“ im Data Lake Storage Gen1-Kontext also entweder ein Azure AD-Benutzer oder eine Azure AD-Sicherheitsgruppe sein.

### <a name="the-super-user"></a>Administrator

Ein Administrator verfügt über die meisten Rechte aller Benutzer im Data Lake Storage Gen1-Konto. Für einen Administrator gilt Folgendes:

* Er besitzt RWX-Berechtigungen für **alle** Dateien und Ordner.
* Er kann die Berechtigungen für jede Datei und jeden Ordner ändern.
* Er kann den zuständigen Benutzer und die zuständige Gruppe einer beliebigen Datei und eines beliebigen Ordners ändern.

Alle Benutzer, die der Rolle **Besitzer** für ein Data Lake Storage Gen1-Konto angehören, sind automatisch auch Administratoren.

### <a name="the-owning-user"></a>Der zuständige Benutzer

Der Benutzer, der das Element erstellt hat, ist automatisch der zuständige Benutzer für das Element. Der zuständige Benutzer hat folgende Möglichkeiten:

* Er kann die Berechtigungen einer Datei ändern, für die er als Besitzer fungiert.
* Er kann die zuständige Gruppe einer Datei ändern, für die er als Besitzer fungiert, solange der zuständige Benutzer auch der Zielgruppe angehört.

> [!NOTE]
> Der Besitzer kann einen anderen Besitzer einer Datei oder eines Ordners *nicht* ändern. Nur Administratoren können den zuständigen Benutzer einer Datei oder eines Ordners ändern.
>
>

### <a name="the-owning-group"></a>Die zuständige Gruppe

**Hintergrund**

In den POSIX-Zugriffssteuerungslisten ist jeder Benutzer einer „primären Gruppe“ zugeordnet. So kann beispielsweise die Benutzerin „Alice“ der Gruppe „finance“ angehören. Alice kann außerdem mehreren Gruppen angehören, aber eine Gruppe wird immer als ihre primäre Gruppe festgelegt. In POSIX gilt: Wenn Alice eine Datei erstellt, wird die zuständige Gruppe der Datei auf ihre primäre Gruppe festgelegt (in diesem Fall „finance“). Andernfalls verhält sich die zuständige Gruppe ähnlich wie zugewiesene Berechtigungen für andere Benutzer oder Gruppen.

Da den Benutzern in Data Lake Storage Gen1 keine „primäre Gruppe“ zugeordnet ist, wird die zuständige Gruppe wie unten angegeben zugewiesen.

**Zuweisen der zuständigen Gruppe für eine neue Datei oder einen neuen Ordner**

* **1. Fall**: Der Stammordner „/“. Dieser Ordner wird erstellt, wenn ein Data Lake Storage Gen1-Konto erstellt wird. In diesem Fall wird die zuständige Gruppe auf eine GUID festgelegt, die nur aus Nullen besteht.  Bei diesem Wert ist kein Zugriff zulässig.  Es ist ein Platzhalter, der bis zur Zuweisung einer Gruppe gilt.
* **2. Fall** (jeder andere Fall): Beim Erstellen eines neuen Elements wird die zuständige Gruppe aus dem übergeordneten Ordner kopiert.

**Ändern der zuständigen Gruppe**

Die zuständige Gruppe kann von folgenden Benutzern geändert werden:
* Beliebiger Administrator
* Zuständiger Benutzer, sofern er auch der Zielgruppe angehört

> [!NOTE]
> Die zuständige Gruppe kann die ACLs einer Datei oder eines Ordners *nicht* ändern.
>
> Für Konten, die bis September 2018 erstellt wurden, wurde die zuständige Gruppe auf den Benutzer festgelegt, der das Konto erstellt hat (für Stammordner siehe **1. Fall** oben).  Ein einzelnes Benutzerkonto ist zum Bereitstellen von Berechtigungen über die zuständige Gruppe nicht gültig, sodass mit dieser Standardeinstellung keine Berechtigungen gewährt werden. Sie können diese Berechtigung einer gültigen Benutzergruppe zuweisen.


## <a name="access-check-algorithm"></a>Algorithmus für die Zugriffsüberprüfung

Im folgenden Pseudocode wird der Zugriffsüberprüfungsalgorithmus für Data Lake Storage Gen1-Konten veranschaulicht.

```
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or folder
  # Note: the "sticky bit" is not illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

  # Handle the owning user. Note that mask IS NOT used.
  entry = get_acl_entry( path, OWNER )
  if (user == entry.identity)
      return ( (desired_perms & e.permissions) == desired_perms )

  # Handle the named users. Note that mask IS used.
  entries = get_acl_entries( path, NAMED_USER )
  for entry in entries:
      if (user == entry.identity ) :
          mask = get_mask( path )
          return ( (desired_perms & entry.permmissions & mask) == desired_perms)

  # Handle named groups and owning group
  member_count = 0
  perms = 0
  entries = get_acl_entries( path, NAMED_GROUP | OWNING_GROUP )
  for entry in entries:
    if (user_is_member_of_group(user, entry.identity)) :
      member_count += 1
      perms | =  entry.permissions
  if (member_count>0) :
    return ((desired_perms & perms & mask ) == desired_perms)
 
  # Handle other
  perms = get_perms_for_other(path)
  mask = get_mask( path )
  return ( (desired_perms & perms & mask ) == desired_perms)
```

### <a name="the-mask"></a>Die Maske

Wie im Algorithmus für die Zugriffsüberprüfung gezeigt beschränkt die Maske den Zugriff auf **benannte Benutzer**, die **zuständige Gruppe** und **benannte Gruppen**.  

> [!NOTE]
> Bei einem neuen Data Lake Storage Gen1-Konto wird die Maske für die Zugriffs-ACL des Stammordners („/“) standardmäßig auf „RWX“ festgelegt.
>
>

### <a name="the-sticky-bit"></a>Das Sticky Bit

Das Sticky Bit ist ein erweitertes Feature eines POSIX-Dateisystems. Im Kontext von Data Lake Storage Gen1 wird das Sticky Bit höchstwahrscheinlich nicht benötigt. Kurz gefasst kann ein untergeordnetes Element nur vom jeweiligen zuständigen Benutzer gelöscht oder umbenannt werden, wenn das Sticky Bit für einen Ordner aktiviert ist.

Das Sticky Bit wird im Azure-Portal nicht angezeigt.

## <a name="default-permissions-on-new-files-and-folders"></a>Standardberechtigungen für neue Dateien und Ordner

Wenn unter einem bereits vorhandenen Ordner eine neue Datei oder ein erstellt wird, bestimmt die Standard-ACL des übergeordneten Ordners Folgendes:

- Eine Standard- und eine Zugriffs-ACL des untergeordneten Ordners
- Eine Zugriffs-ACL der untergeordneten Datei (Dateien besitzen keine Standard-ACL)

### <a name="umask"></a>umask

Beim Erstellen einer Datei oder eines Ordners wird „umask“ verwenden, um zu ändern, wie Standard-ACLs für das untergeordnete Element festgelegt werden. „umask“ ist ein 9-Bit-Wert für übergeordnete Ordner, der einen RWX-Wert für **zuständige Benutzer**, **zuständige Gruppen** und **Andere** enthält.

Für Azure Data Lake Storage Gen1 ist „umask“ ein konstanter Wert, der auf „007“ festgelegt ist. Dieser Wert wird wie folgt übersetzt

| umask-Komponente     | Numerische Form | Kurzform | Bedeutung |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | Für zuständige Benutzer wird die Standard-ACL des übergeordneten Elements für die Zugriffs-ACL des untergeordneten Elements kopiert | 
| umask.owning_group  |    0         |   `---`      | Für zuständige Gruppen wird die Standard-ACL des übergeordneten Elements für die Zugriffs-ACL des untergeordneten Elements kopiert | 
| umask.other         |    7         |   `RWX`      | Für „Andere“ werden alle Berechtigungen für die Zugriffs-ACL des untergeordneten Elements entfernt |

Der umask-Wert, der von Azure Data Lake Storage Gen1 verwendet wird, bedeutet, dass der Wert für „Andere“ nie standardmäßig an untergeordnete Elemente übertragen wird, unabhängig davon, was die Standard-ACL angibt. 

Im folgenden Pseudocode wird gezeigt, wie der umask-Wert angewendet wird, wenn die ACLs für ein untergeordnetes Element erstellt werden.

```
def set_default_acls_for_new_child(parent, child):
    child.acls = []
    for entry in parent.acls :
        new_entry = None
        if (entry.type == OWNING_USER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_user))
        elif (entry.type == OWNING_GROUP) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_group))
        elif (entry.type == OTHER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.other))
        else :
            new_entry = entry.clone(perms = entry.perms )
        child_acls.add( new_entry )
```

## <a name="common-questions-about-acls-in-data-lake-storage-gen1"></a>Allgemeine Fragen zu ACLs in Data Lake Storage Gen1

### <a name="do-i-have-to-enable-support-for-acls"></a>Muss ich die Unterstützung für ACLs aktivieren?

Nein. Die ACL-basierte Zugriffssteuerung ist für ein Data Lake Storage Gen1-Konto immer aktiviert.

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Welche Berechtigungen werden zum rekursiven Löschen eines Ordners und seines Inhalts benötigt?

* Der übergeordnete Ordner muss über **Schreib- und Ausführungsberechtigungen** verfügen.
* Der zu löschende Ordner und alle darin enthaltenen Ordner müssen über **Lese-, Schreib- und Ausführungsberechtigungen** verfügen.

> [!NOTE]
> Sie benötigen zum Löschen von Dateien in Ordnern keine Schreibberechtigungen. Außerdem gilt: Der Stammordner „/“ kann **nie** gelöscht werden.
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a>Wer ist der Besitzer einer Datei oder eines Ordners?

Der Ersteller einer Datei oder eines Ordners wird als Besitzer festgelegt.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Welche Gruppe wird bei der Erstellung als zuständige Gruppe einer Datei oder eines Ordners festgelegt?

Die zuständige Gruppe wird aus der zuständigen Gruppe des übergeordneten Ordners kopiert, unter dem die neue Datei oder der neue Ordner erstellt wird.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ich bin der zuständige Benutzer einer Datei, verfüge aber nicht über die erforderlichen RWX-Berechtigungen. Wie gehe ich vor?

Der zuständige Benutzer kann die Berechtigungen der Datei ändern und sich so selbst die erforderlichen RWX-Berechtigungen gewähren.

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Wenn ich mir die ACLs im Azure-Portal ansehe, sehe ich dort Benutzernamen, aber über APIs werden mir GUIDs angezeigt. Warum ist das so?

Einträge in den ACLs werden als GUIDs gespeichert, die Benutzern in Azure AD entsprechen. Die APIs geben die GUIDs unverändert zurück. Das Azure-Portal wandelt die GUIDs zur Vereinfachung der ACL-Verwendung nach Möglichkeit in benutzerfreundliche Namen um.

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a>Warum werden im Azure-Portal manchmal GUIDs in den ACLs angezeigt?

Eine GUID wird angezeigt, wenn der Benutzer in Azure AD nicht mehr vorhanden ist. Dies ist meist der Fall, wenn der Benutzer aus dem Unternehmen ausgeschieden ist oder sein Konto in Azure AD gelöscht wurde.

### <a name="does-data-lake-storage-gen1-support-inheritance-of-acls"></a>Unterstützt Data Lake Storage Gen1 die Vererbung von ACLs?

Nein, aber Standard-ACLs können zum Festlegen von ACLs für untergeordnete Dateien und Ordner verwendet werden, die unter dem übergeordneten Ordner neu erstellt wurden.  

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Wo finde ich weitere Informationen zum POSIX-Zugriffssteuerungsmodell?

* [POSIX-Zugriffssteuerungslisten (ACLs) unter Linux](https://www.linux.com/news/posix-acls-linux)
* [HDFS Permission Guide](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) (Handbuch zu HDFS-Berechtigungen)
* [POSIX FAQ (Häufig gestellte Fragen zu POSIX)](http://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)
* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)
* [POSIX-ACL unter Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs)
* [ACL using Access Control Lists on Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/) (ACL mit Zugriffssteuerungslisten unter Linux)

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über Azure Data Lake Storage Gen1](data-lake-store-overview.md)
