---
title: Ressourcengesamtstrukturkonzepte für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, was eine Ressourcengesamtstruktur in Azure Active Directory Domain Services ist und wie Ihre Organisation von einer solchen Struktur in einer Hybridumgebung profitieren kann, wenn es eingeschränkte Optionen zur Benutzerauthentifizierung oder Sicherheitsbedenken gibt.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: iainfou
ms.openlocfilehash: a583e32cbc3d58d5dfc5616335b2f38ad20fac14
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74234395"
---
# <a name="resource-forest-concepts-and-features-for-azure-active-directory-domain-services"></a>Ressourcengesamtstrukturkonzepte und -features für Azure Active Directory Domain Services

Azure Active Directory Domain Services (AD DS) stellt eine Anmeldeumgebung für ältere, lokale und Branchenanwendungen bereit. Benutzer, Gruppen und Kennworthashes für lokale und Cloudbenutzer werden mit der verwalteten Azure AD DS-Domäne synchronisiert. Diese synchronisierten Kennworthashes ermöglichen Benutzern das Verwenden einen einzigen Satzes von Anmeldeinformationen, den sie für die lokalen Instanzen von AD DS, Office 365 und Azure Active Directory verwenden können.

Obwohl diese Benutzerkennworthashes sicher sind und zusätzliche Sicherheitsvorteile bieten, können einige Organisationen die Hashes nicht mit Azure AD oder Azure AD DS synchronisieren. Benutzer in einer Organisation wissen Ihr Kennwort möglicherweise nicht, da sie nur Smartcard-Authentifizierung verwenden. Diese Einschränkungen verhindern für einige Organisationen, dass sie Azure AD DS verwenden, um lokale klassische Anwendungen per Lift & Shift zu Azure zu migrieren.

Um diese Anforderungen und Einschränkungen zu erfüllen, können Sie eine verwaltete Azure AD DS-Domäne erstellen, in der eine Ressourcengesamtstruktur verwendet wird. In diesem konzeptionellen Artikel wird erläutert, was Gesamtstrukturen sind und wie sie anderen Ressourcen vertrauen, um eine sichere Authentifizierungsmethode bereitzustellen. Azure AD DS-Ressourcengesamtstrukturen befinden sich derzeit in der Vorschauphase.

> [!IMPORTANT]
> Azure AD DS-Ressourcengesamtstrukturen unterstützen derzeit weder Microsoft Azure HDInsight noch Azure Files. Die standardmäßigen Azure AD DS-Benutzergesamtstrukturen unterstützen diese beiden zusätzlichen Dienste.

## <a name="what-are-forests"></a>Was sind Gesamtstrukturen?

Eine *Gesamtstruktur* ist ein logisches Konstrukt, das von Active Directory Domain Services (AD DS) zum Gruppieren von *Domänen* verwendet wird. Die Domänen speichern dann Objekte für Benutzer oder Gruppen und stellen Authentifizierungsdienste bereit.

In Azure AD DS enthält die Gesamtstruktur nur eine einzelne Domäne. Lokale AD DS-Gesamtstrukturen enthalten häufig mehrere Domänen. In großen Unternehmen sind, insbesondere nach Fusionen und Übernahmen, unter Umständen mehrere lokale Gesamtstrukturen vorhanden, die jeweils mehrere Domänen enthalten.

Eine verwaltete Azure AD DS-Domäne wird standardmäßig als *Benutzergesamtstruktur* erstellt. Mit dieser Art von Gesamtstruktur werden alle Objekte aus Azure AD synchronisiert – einschließlich aller in einer lokalen AD DS-Umgebung erstellten Benutzerkonten. Benutzerkonten können sich direkt bei der verwalteten Azure AD DS-Domäne authentifizieren, um sich beispielsweise bei einem in eine Domäne eingebundenen virtuellen Computer anzumelden. Eine Benutzergesamtstruktur ist geeignet, wenn die Kennworthashes synchronisiert werden können und Benutzer keine exklusiven Anmeldemethoden wie etwa die Smartcard-Authentifizierung verwenden.

In einer *Ressourcengesamtstruktur* für Azure AD DS authentifizieren sich Benutzer über eine unidirektionale Gesamtstruktur-*Vertrauensstellung* von der lokalen AD DS-Instanz aus. Hierbei werden die Benutzerobjekte und Kennworthashes nicht mit Azure AD DS synchronisiert. Die Benutzerobjekte und Anmeldeinformationen sind nur in der lokalen AD DS-Instanz vorhanden. Mit diesem Ansatz können Unternehmen Ressourcen und Anwendungsplattformen in Azure hosten, die eine klassische Authentifizierung (beispielsweise LDAPS, Kerberos oder NTLM) benötigen, und gleichzeitig sämtliche Authentifizierungsprobleme oder -bedenken beseitigen. Azure AD DS-Ressourcengesamtstrukturen befinden sich derzeit in der Vorschauphase.

Ressourcengesamtstrukturen bieten außerdem die Möglichkeit, Ihre Anwendungen komponentenweise per Lift & Shift zu migrieren. Viele ältere lokale Anwendungen sind mehrstufig, wobei häufig ein Webserver oder ein Front-End und viele datenbankbezogene Komponenten verwendet werden. Wegen dieser Stufen ist es schwierig, die gesamte Anwendung in einem Schritt per Lift & Shift in die Cloud zu migrieren. Mit Ressourcengesamtstrukturen können Sie Ihre Anwendung in einem schrittweisen Ansatz in die Cloud übertragen, wodurch es Ihnen vereinfacht wird, die Anwendung nach Azure zu verschieben.

## <a name="what-are-trusts"></a>Was sind Vertrauensstellungen?

In Organisationen, die mehrere Domänen haben, gibt es häufig Benutzer, die Zugriff auf freigegebene Ressourcen in einer anderen Domäne benötigen. Für einen Zugriff auf diese freigegebenen Ressourcen ist es erforderlich, dass sich Benutzer in einer Domäne bei einer anderen Domäne authentifizieren. Um diese Authentifizierungs- und Autorisierungsfunktionen zwischen Clients und Servern in verschiedenen Domänen bereitzustellen, muss es eine *Vertrauensstellung* zwischen den beiden Domänen geben.

Mit Domänenvertrauensstellungen wird erreicht, dass die Authentifizierungsmechanismen für jede Domäne den Authentifizierungen vertrauen, die aus einer anderen Domäne stammen. Vertrauensstellungen unterstützen den kontrollierten Zugriff auf freigegebene Ressourcen in einer Ressourcendomäne (die *vertrauende* Domäne), indem sie überprüfen, ob eingehende Authentifizierungsanforderungen von einer vertrauenswürdigen Zertifizierungsstelle (die *vertrauenswürdige* Domäne) stammen. Vertrauensstellungen fungieren als Brücken, die nur für überprüfte Authentifizierungsanforderungen zulassen, dass sie zwischen Domänen ausgetauscht werden.

Auf welche Weise eine Vertrauensstellung Authentifizierungsanforderungen übergibt, hängt von ihrer Konfiguration ab. Eine Vertrauensstellung kann in einer der folgenden Arten konfiguriert sein:

* **Unidirektional**: Ermöglicht Zugriff aus der vertrauenswürdigen Domäne auf Ressourcen in der vertrauenden Domäne.
* **Bidirektional**: Ermöglicht Zugriff aus jeder Domäne auf Ressourcen in der jeweils anderen Domäne.

Vertrauensstellungen können außerdem so konfiguriert sein, dass sie zusätzliche Vertrauensstellungen auf eine der folgenden Arten verarbeiten:

* **Nicht transitiv**: Die Vertrauensstellung besteht nur zwischen den beiden Partnerdomänen einer Vertrauensstellung.
* **Transitiv**: Die Vertrauensstellung wird automatisch auf alle anderen Domänen ausgedehnt, denen einer der Partner vertraut.

In einigen Fällen werden Vertrauensstellungen automatisch erstellt, wenn Domänen erstellt werden. In anderen Fällen müssen Sie den Typ der Vertrauensstellung auswählen und die entsprechenden Beziehungen explizit einrichten. Die verwendeten speziellen Typen der Vertrauensstellungen und die Strukturen dieser Vertrauensstellungen hängen davon ab, wie der Active Directory-Verzeichnisdienst strukturiert ist und ob verschiedene Versionen von Windows im Netzwerk vorhanden sind.

## <a name="trusts-between-two-forests"></a>Vertrauensstellungen zwischen zwei Gesamtstrukturen

Sie können Domänenvertrauensstellungen in einer einzelnen Gesamtstruktur auf eine andere Gesamtstruktur ausdehnen, indem Sie manuell eine unidirektionale oder bidirektionale Gesamtstruktur-Vertrauensstellung erstellen. Eine Gesamtstruktur-Vertrauensstellung ist eine transitive Vertrauensstellung, die nur zwischen der Stammdomäne einer Gesamtstruktur und der Stammdomäne einer zweiten Gesamtstruktur besteht.

* Eine unidirektionale Gesamtstruktur-Vertrauensstellung ermöglicht allen Benutzern in einer Gesamtstruktur, allen Domänen in der anderen Gesamtstruktur zu vertrauen.
* Eine bidirektionale Gesamtstruktur-Vertrauensstellung führt zu einer transitiven Vertrauensstellung zwischen allen Domänen in beiden Gesamtstrukturen.

Die Transitivität von Gesamtstruktur-Vertrauensstellungen ist auf die beiden Gesamtstrukturpartner beschränkt. Die Gesamtstruktur-Vertrauensstellung wird nicht auf weitere Gesamtstrukturen erweitert, denen einer der Partner vertraut.

![Darstellung einer Gesamtstruktur-Vertrauensstellung von Azure AD DS zu lokalem AD DS](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

Abhängig von der Active Directory-Struktur der Organisation können Sie unterschiedliche Konfigurationen für Domänen- und Gesamtstruktur-Vertrauensstellungen erstellen. Azure AD DS unterstützt nur eine unidirektionale Gesamtstruktur-Vertrauensstellung. In dieser Konfiguration können Ressourcen in Azure AD DS allen Domänen in einer lokalen Gesamtstruktur vertrauen.

## <a name="supporting-technology-for-trusts"></a>Unterstützende Technologie für Vertrauensstellungen

Für Vertrauensstellungen werden verschiedene Dienste und Features, beispielsweise DNS, verwendet, um nach Domänencontrollern in Partnergesamtstrukturen zu suchen. Vertrauensstellungen sind auch von NTLM- und Kerberos-Authentifizierungsprotokollen sowie von Windows-basierten Autorisierungs- und Zugriffssteuerungsmechanismen abhängig, um eine geschützte Kommunikationsinfrastruktur zwischen Active Directory-Domänen und -Gesamtstrukturen bereitzustellen. Mit den folgenden Diensten und Features werden erfolgreiche Vertrauensstellungen unterstützt.

### <a name="dns"></a>DNS

AD DS benötigt DNS, um Domänencontroller (DC) zu finden und zu benennen. Um erfolgreich funktionieren zu können, wird AD DS mit folgenden DNS-Komponenten unterstützt:

* Ein Dienst zur Namensauflösung, über den Netzwerkhosts und -dienste nach DCs suchen können
* Eine Benennungsstruktur, die es einem Unternehmen ermöglicht, seine Organisationsstruktur in den Namen der Verzeichnisdienstdomänen abzubilden

Üblicherweise wird ein DNS-Domänennamespace bereitgestellt, in dem der AD DS-Domänennamespace gespiegelt ist. Wenn vor der AD DS-Bereitstellung ein DNS-Namespace vorhanden ist, wird der DNS-Namespace in der Regel für Active Directory partitioniert, und es werden eine DNS-Unterdomäne und eine Delegierung für den Stamm der Active Directory-Gesamtstruktur erstellt. Zusätzliche DNS-Domänennamen werden dann für jede untergeordnete Active Directory-Domäne hinzugefügt.

DNS wird auch verwendet, um die Suche nach Active Directory-DCs zu unterstützen. Die DNS-Zonen werden mit DNS-Ressourceneinträgen aufgefüllt, mit denen Netzwerkhosts und -dienste nach Active Directory-DCs suchen können.

### <a name="applications-and-net-logon"></a>Anwendungen und Netzwerkanmeldung

Sowohl Anwendungen als auch der Netzwerkanmeldungsdienst (Anmeldedienst) sind Komponenten des Windows-Modells für verteilte Sicherheitskanäle. Anwendungen, die in Windows Server und Active Directory integriert sind, verwenden Authentifizierungsprotokolle, um mit dem Anmeldedienst zu kommunizieren, sodass ein geschützter Pfad eingerichtet werden kann, über den die Authentifizierung erfolgen kann.

### <a name="authentication-protocols"></a>Authentifizierungsprotokolle

Active Directory-DCs authentifizieren Benutzer und Anwendungen über eines der folgenden Protokolle:

* **Authentifizierungsprotokoll von Kerberos Version 5**
    * Das Protokoll von Kerberos Version 5 ist das Standardauthentifizierungsprotokoll, das von lokalen Computern verwendet wird, auf denen Windows ausgeführt wird und die Betriebssysteme von Drittanbietern unterstützen. Dieses Protokoll ist in RFC 1510 definiert und ist vollständig in Active Directory, Server Message Block (SMB), HTTP und Remote Procedure Call (RPC) sowie die Client- und Serveranwendungen integriert, die diese Protokolle verwenden.
    * Wenn das Kerberos-Protokoll verwendet wird, muss der Server keinen Kontakt mit dem DC aufnehmen. Stattdessen erhält der Client ein Ticket für einen Server, indem dieses Ticket von einem Domänencontroller in der Domäne des Serverkontos angefordert wird. Der Server überprüft dann das Ticket, ohne eine andere Berechtigungsinstanz zu konsultieren.
    * Ist an einer Transaktion ein Computer beteiligt ist, der das Protokoll von Kerberos Version 5 nicht unterstützt, wird das NTLM-Protokoll verwendet.

* **NTLM-Authentifizierungsprotokoll**
    * Das NTLM-Protokoll ist ein klassisches Netzwerkauthentifizierungsprotokoll, das von älteren Betriebssystemen verwendet wird. Aus Kompatibilitätsgründen wird es von Active Directory-Domänen verwendet, um Netzwerkauthentifizierungsanforderungen zu verarbeiten, die von Anwendungen stammen, die für frühere Windows-basierte Clients und Server oder für Betriebssysteme von Drittanbietern entwickelt wurden.
    * Wird das NTLM-Protokoll zwischen einem Client und einem Server verwendet, muss sich der Server an einen Domänenauthentifizierungsdienst auf einem Domänencontroller wenden, um die Clientanmeldeinformationen zu überprüfen. Der Server authentifiziert den Client, indem er die Clientanmeldeinformationen an einen Domänencontroller in der Domäne des Clientkontos weiterleitet.
    * Wenn zwei Active Directory-Domänen oder -Gesamtstrukturen über eine Vertrauensstellung verbunden sind, können Authentifizierungsanforderungen, die über diese Protokolle erfolgen, weitergeleitet werden, um Zugriff auf Ressourcen in beiden Gesamtstrukturen zu ermöglichen.

## <a name="authorization-and-access-control"></a>Autorisierung und Zugriffssteuerung

Autorisierungs- und Vertrauensstellungskomponenten arbeiten zusammen, um eine geschützte Kommunikationsinfrastruktur zwischen Active Directory-Domänen oder -Gesamtstrukturen bereitzustellen. Die Autorisierung bestimmt, welche Zugriffsebene ein Benutzer für Ressourcen in einer Domäne hat. Vertrauensstellungen ermöglichen die domänenübergreifende Autorisierung von Benutzern, indem sie einen Pfad bereitstellen, über den Benutzer in anderen Domänen authentifiziert werden können, sodass deren Anforderungen an freigegebene Ressourcen in diesen Domänen autorisiert werden können.

Wenn eine Authentifizierungsanforderung, die in einer vertrauenden Domäne ausgelöst wurde, von der vertrauenswürdigen Domäne validiert wurde, wird sie an die Zielressource übermittelt. Die Zielressource bestimmt dann anhand ihrer Zugriffssteuerungskonfiguration, ob die spezielle Anforderung, die von dem Benutzer, Dienst oder Computer ausgelöst wurde, in der vertrauenswürdigen Domäne autorisiert wird.

Vertrauensstellungen stellen diesen Mechanismus bereit, um Authentifizierungsanforderungen zu validieren, die an eine vertrauende Domäne übermittelt werden. Die Zugriffssteuerungsmechanismen auf dem Ressourcencomputer bestimmen die endgültige Zugriffsebene, die dem Anforderer in der vertrauenswürdigen Domäne gewährt wird.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Vertrauensstellungen finden Sie unter [Funktionsweise von Vertrauensstellungen für Ressourcengesamtstrukturen in Azure Active Directory Domain Services][concepts-trust].

Informationen zu den ersten Schritten beim Erstellen einer verwalteten Azure AD DS-Domäne mit einer Ressourcengesamtstruktur finden Sie unter [Tutorial: Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz mit erweiterten Konfigurationsoptionen][tutorial-create-advanced]. Danach können Sie die Schritte für [Erstellen einer ausgehenden Gesamtstruktur-Vertrauensstellung zu einer lokalen Domäne in Azure Active Directory Domain Services (Vorschauversion)][create-forest-trust] ausführen.

<!-- LINKS - INTERNAL -->
[concepts-trust]: concepts-forest-trust.md
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-forest-trust]: tutorial-create-forest-trust.md
