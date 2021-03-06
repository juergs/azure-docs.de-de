---
title: 'Playbook: Bewährte Sicherheitsmethoden für Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Dieser Artikel enthält allgemeine Hinweise zu bewährten Sicherheitsmethoden in Azure SQL-Datenbank.
ms.service: sql-database
ms.subservice: security
author: VanMSFT
ms.author: vanto
ms.topic: article
ms.date: 12/23/2019
ms.reviewer: ''
ms.openlocfilehash: f93ab61fcba53ebf39adf8ad56137f4a1df7d5fd
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75615023"
---
# <a name="azure-sql-database-security-best-practices-playbook"></a>Playbook: Bewährte Sicherheitsmethoden für SQL-Datenbank

## <a name="overview"></a>Übersicht

Dieses Dokument enthält Anleitungen zum Beheben allgemeiner Sicherheitsanforderungen für neue oder vorhandene Anwendungen mit Azure SQL-Datenbank. Es ist nach Sicherheitsbereichen auf hoher Ebene gegliedert. Informationen zur Behandlung bestimmter Bedrohungen finden Sie im Abschnitt [Allgemeine Sicherheitsbedrohungen und mögliche Risikominderungen](#common-security-threats-and-potential-mitigations). Obwohl einige der vorgestellten Empfehlungen auch dann gelten, wenn Sie Anwendungen aus einer lokalen Umgebung zu Azure migrieren, bilden Migrationsszenarien nicht den Schwerpunkt dieses Dokuments.

### <a name="azure-sql-database-deployment-offers-covered-in-this-guide"></a>Bereitstellungsangebote für Azure SQL-Datenbank, die in dieser Anleitung behandelt werden

- [SQL-Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-single-index): [Einzelne Datenbanken](sql-database-single-database.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) auf [Azure SQL-Datenbank-Servern](sql-database-servers.md)
- [Verwaltete Instanzen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index)

### <a name="sql-deployment-offers-not-covered-in-this-guide"></a>SQL- Bereitstellungsangebote, die in diesem Leitfaden nicht behandelt werden

- Azure SQL Data Warehouse
- Azure SQL-VMs (IaaS)
- SQL Server (lokal)

### <a name="audience"></a>Zielgruppe

Die Zielgruppen für dieses Handbuch sind Kunden, die mit Fragen zum Sichern von Azure SQL-Datenbank konfrontiert sind. Die Rollen, für die dieser Artikel zu bewährten Methoden interessant ist, gehören beispielsweise:

- Sicherheitsarchitekten
- Sicherheits-Manager
- Compliance Officers
- Datenschutzbeauftragte
- Sicherheitstechniker

### <a id="using"></a> Verwenden dieses Leitfadens

Dieses Dokument ist als Begleitmaterial zu unserer vorhandenen Dokumentation zur [Sicherheit von Azure SQL-Datenbank](sql-database-security-overview.md) gedacht.

Sofern nicht anders angegeben, empfehlen wir Ihnen, alle in jedem Abschnitt aufgeführten bewährten Methoden zu befolgen, um das jeweilige Ziel oder die jeweilige Anforderung zu erreichen. Um bestimmte Sicherheitscompliancestandards oder bewährte Methoden zu erfüllen, werden wichtige Kontrollmechanismen zur Einhaltung von Vorschriften im Abschnitt „Anforderungen“ oder „Ziele“ aufgeführt, sofern anwendbar. Dies sind die Sicherheitsstandards und -bestimmungen, auf die in diesem Dokument verwiesen wird:

- [FedRAMP](https://www.fedramp.gov/documents/): AC-04, AC-06
- [SOC](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/sorhome.html): CM-3, SDL-3
- [ISO/IEC 27001](https://www.iso27001security.com/html/27001.html): Zugriffssteuerung, Kryptografie
- [Microsoft OSA-Methoden (Operational Security Assurance)](https://www.microsoft.com/en-us/securityengineering/osa/practices): Methode Nr. 1-6 und Nr. 9
- [NIST-Sonderpublikation 800-53 zu Sicherheitskontrollen](https://nvd.nist.gov/800-53): AC-5, AC-6
- [PCI-DSS](https://www.pcisecuritystandards.org/document_library): 6.3.2, 6.4.2

### <a name="feedback"></a>Feedback

Wir planen die laufende Aktualisierung der hier aufgeführten Empfehlungen und bewährten Methoden. Stellen Sie über den Link **Feedback** am Ende dieses Artikels Anregungen oder Korrekturen für dieses Dokument bereit.

## <a name="authentication"></a>Authentication

Die Authentifizierung ist der Prozess, bei dem bestätigt wird, dass der Benutzer derjenige ist, der er zu sein vorgibt. Azure SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

- SQL-Authentifizierung
- Authentifizierung über Azure Active Directory

### <a name="central-management-for-identities"></a>Zentrale Verwaltung für Identitäten

Die zentrale Identitätsverwaltung bietet die folgenden Vorteile:

- Verwalten von Gruppenkonten und Steuern von Benutzerberechtigungen ohne Duplizieren von Anmeldungen bei Azure SQL-Datenbank-Servern und -Datenbanken.
- Vereinfachte und flexible Berechtigungsverwaltung.
- Verwaltung von Anwendungen im richtigen Maßstab.

**Implementierung**:

- Verwenden Sie die Azure Active Directory-Authentifizierung (Azure AD) für die zentralisierte Identitätsverwaltung.

**Bewährte Methoden:**

- Erstellen Sie einen Azure AD-Mandanten, [erstellen Sie Benutzer](../active-directory/fundamentals/add-users-azure-active-directory.md), um menschliche Benutzer darzustellen, und erstellen Sie [Dienstprinzipale](../active-directory/develop/app-objects-and-service-principals.md), die Apps, Dienste und Automatisierungstools darstellen. Dienstprinzipale entsprechen Dienstkonten in Windows und Linux. 

- Weisen Sie Azure AD-Prinzipalen über die Gruppenzuweisung Zugriffsrechte für Ressourcen zu: Erstellen Sie Azure AD-Gruppen, gewähren Sie Zugriff auf Gruppen, und fügen Sie den Gruppen einzelne Mitglieder hinzu. Erstellen in Ihrer Datenbank eigenständige Datenbankbenutzer, die Ihren Azure AD-Gruppen zugeordnet sind. 
  - Weitere Informationen finden Sie in den Artikeln [Konfigurieren und Verwalten von Azure Active Directory-Authentifizierung mit SQL](sql-database-aad-authentication-configure.md) und [Verwenden von Azure AD für die Authentifizierung mit SQL](sql-database-aad-authentication.md).
  > [!NOTE]
  > In einer verwalteten Instanz können Sie auch Anmeldungen erstellen, die Azure AD-Prinzipalen in der Masterdatenbank zugeordnet sind. Siehe [CREATE LOGIN (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).

- Die Verwendung von Azure AD-Gruppen vereinfacht die Berechtigungsverwaltung des Gruppenbesitzers, und der Besitzer der Ressource kann der Gruppe Mitglieder hinzufügen bzw. aus ihr entfernen. 

- Erstellen Sie eine separate Gruppe für den Azure AD-Administrator für SQL-Datenbank-Server.

  - Lesen Sie dazu den Artikel [Bereitstellen eines Azure Active Directory-Administrators für Ihren Azure SQL-Datenbank-Server](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server).

- Überwachen Sie Änderungen der Azure AD-Gruppenmitgliedschaft mithilfe von Azure AD-Überwachungsaktivitätsberichten. 

- Für eine verwaltete Instanz ist ein separater Schritt erforderlich, um einen Azure AD-Administrator zu erstellen. 
  - Lesen Sie dazu den Artikel [Bereitstellen eines Azure Active Directory-Administrators für Ihre verwaltete Instanz](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance). 

> [!NOTE]
> - Azure AD Authentifizierung wird in Azure SQL-Überwachungsprotokollen aufgezeichnet, aber nicht in Azure AD-Anmeldeprotokollen.
> - In Azure gewährte RBAC-Berechtigungen gelten nicht für Azure SQL-Datenbank-Berechtigungen. Diese Berechtigungen müssen in SQL-Datenbank mithilfe vorhandener SQL-Berechtigungen manuell erstellt/zugeordnet werden.
> - Auf der Clientseite muss die Azure AD-Authentifizierung auf das Internet oder über eine benutzerdefinierte Route (User Defined Route, UDR) auf ein VNET zugreifen können.
> - Das Azure AD-Zugriffstoken wird auf der Clientseite zwischengespeichert, und seine Lebensdauer hängt von der Tokenkonfiguration ab. Lesen Sie dazu den Artikel [Konfigurierbare Tokenlebensdauern in Azure Active Directory](../active-directory/develop/active-directory-configurable-token-lifetimes.md).

### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA)

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 2, ISO-Zugriffssteuerung (Access Control, AC)

Indem Azure Multi-Factor Authentication (MFA) eine weitere Form der Authentifizierung erfordert, bietet das Verfahren zusätzliche Sicherheit .

**Implementierung**:

- [Aktivieren Sie MFA](../active-directory/authentication/concept-mfa-howitworks.md) in Azure AD mit bedingtem Zugriff, und verwenden Sie interaktive Authentifizierung. 

- Die Alternative besteht darin, MFA für das gesamte Azure AD oder die gesamte AD-Domäne zu aktivieren.

**Bewährte Methoden:**

- Aktivieren Sie bedingten Zugriff in Azure AD (erfordert ein Premium-Abonnement). 
  - Lesen Sie dazu den Artikel [Bedingter Zugriff in Azure AD](../active-directory/conditional-access/overview.md).  

- Erstellen Sie Azure AD-Gruppen, und aktivieren Sie die MFA-Richtlinie für ausgewählte Gruppen mithilfe des bedingten Azure AD-Zugriffs. 
  - Weitere Informationen finden Sie im Artikel [Planen der Bereitstellung bedingten Zugriffs](../active-directory/conditional-access/plan-conditional-access.md). 

- MFA kann für das gesamte Azure AD oder für das gesamte Active Directory im Verbund mit Azure AD aktiviert werden. 

- Verwenden Sie den interaktiven Authentifizierungsmodus von Azure AD für SQL-Datenbank, bei dem ein Kennwort interaktiv angefordert wird, gefolgt von MFA-Authentifizierung:      
  - Verwenden Sie universelle Authentifizierung in SSMS. Lesen Sie dazu den Artikel [Verwenden der mehrstufigen Authentifizierung von AAD mit Azure SQL-Datenbank und Azure SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).
  - Verwenden Sie interaktive Authentifizierung, die in SQL Server Data Tools (SSDT) unterstützt wird. Lesen Sie dazu den Artikel [Azure Active Directory-Unterstützung in SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory?view=azuresqldb-current).
  - Verwenden Sie andere SQL-Tools, die MFA unterstützen. 
    - Unterstützung des SSMS-Assistenten zum Exportieren/Extrahieren/Bereitstellen der Datenbank  
    - [sqlpackage.exe](https://docs.microsoft.com/sql/tools/sqlpackage): Option „/ua“ 
    - [sqlcmd-Hilfsprogramm](https://docs.microsoft.com/sql/tools/sqlcmd-utility): Option -G (interaktiv)
    - [bcp-Hilfsprogramm](https://docs.microsoft.com/sql/tools/bcp-utility): Option -G (interaktiv) 

- Implementieren Sie Ihre Anwendungen zum Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe interaktiver Authentifizierung mit MFA-Unterstützung. 
  - Lesen Sie dazu den Artikel [Herstellen einer Verbindung mit Azure SQL-Datenbank mit Azure Multi-Factor Authentication](active-directory-interactive-connect-azure-sql-db.md). 
  > [!NOTE]
  > Dieser Authentifizierungsmodus erfordert benutzerbasierte Identitäten. In Fällen, in denen ein vertrauenswürdiges Identitätsmodell verwendet wird, das die individuelle Azure AD-Benutzerauthentifizierung umgeht (z.B. durch Verwendung von verwalteten Identitäten für Azure-Ressourcen), gilt MFA nicht.

### <a name="minimize-the-use-of-password-based-authentication-for-users"></a>Minimieren der Verwendung der kennwortbasierten Authentifizierung für Benutzer

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 4, ISO-Zugriffssteuerung (Access Control, AC)

Kennwortbasierte Authentifizierungsmethoden sind eine schwächere Form der Authentifizierung. Anmeldeinformationen können kompromittiert oder versehentlich offengelegt werden.

**Implementierung**:

- Verwenden Sie in Azure AD integrierte Authentifizierung, bei der die Verwendung von Kennwörtern entfällt.

**Bewährte Methoden:**

- Verwenden Sie SSO-Authentifizierung (Single Sign-On, einmaliges Anmelden) mithilfe von Windows-Anmeldeinformationen. Bilden Sie einen Verbund der lokalen AD-Domäne mit Azure AD, und verwenden Sie integrierte Windows-Authentifizierung (für in die Domäne eingebundene Computer mit Azure AD).
  - Lesen Sie dazu den Artikel [SSMS-Unterstützung für integrierte Azure AD-Authentifizierung](sql-database-aad-authentication-configure.md#active-directory-integrated-authentication).

### <a name="minimize-the-use-of-password-based-authentication-for-applications"></a>Minimieren der Verwendung der kennwortbasierten Authentifizierung für Anwendungen 

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 4, ISO-Zugriffssteuerung (Access Control, AC)

**Implementierung**:

- Aktivieren Sie eine verwaltete Azure-Identität. Sie können auch integrierte oder zertifikatbasierte Authentifizierung verwenden. 

**Bewährte Methoden:**

- Verwenden Sie [verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md).
  - [Systemseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql.md) 
  - [Benutzerseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
  - [Verwenden von Azure SQL-Datenbank aus App Service mit verwalteter Identität (ohne Codeänderungen)](https://github.com/Azure-Samples/app-service-msi-entityframework-dotnet)

- Verwenden Sie zertifikatbasierte Authentifizierung für eine Anwendung. 
  - Weitere Informationen finden Sie in diesem [Codebeispiel](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/token). 

- Verwenden Sie Azure AD-Authentifizierung für die integrierte Verbunddomäne und den in die Domäne eingebundenen Computer (siehe Abschnitt weiter oben).
  - Weitere Informationen finden Sie in der [Beispielanwendung für integrierte Authentifizierung](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/integrated).

### <a name="protect-passwords-and-secrets"></a>Schützen von Kennwörtern und Geheimnissen

Wenn Kennwörter nicht vermeidbar sind, stellen Sie sicher, dass diese geschützt sind.

**Implementierung**:

- Verwenden Sie Azure Key Vault zum Speichern von Kennwörtern und Geheimnissen. Wann immer möglich, verwenden Sie MFA für Azure SQL-Datenbank mit Azure AD-Benutzern.

**Bewährte Methoden:**

- Wenn das Vermeiden von Kennwörtern oder Geheimnissen nicht möglich ist, speichern Sie Benutzerkennwörter und Anwendungsgeheimnisse in Azure Key Vault, und verwalten Sie den Zugriff über Key Vault-Zugriffsrichtlinien. 

- Verschiedene App-Entwicklungsframeworks können auch frameworkspezifische Mechanismen zum Schutz von Geheimnissen in der App zur Verfügung stellen. Beispiel:  [ASP.NET Core-App](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows).

### <a name="use-sql-authentication-for-legacy-applications"></a>Verwenden von SQL-Authentifizierung für ältere Anwendungen 

SQL-Authentifizierung bezieht sich auf die Authentifizierung eines Benutzers beim Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe von Benutzername und Kennwort. Eine Anmeldung muss in jedem SQL-Datenbank-Server oder verwalteten Instanz erstellt werden, und ein Benutzer muss in jeder Datenbank erstellt werden.

**Implementierung**:

- Verwenden Sie SQL-Authentifizierung.

**Bewährte Methoden:**

- Erstellen Sie als Serveradministrator Anmeldungen und Benutzer. Sofern nicht eigenständige Datenbankbenutzer mit Kennwörtern verwendet werden, werden alle Kennwörter in der Masterdatenbank gespeichert.
  - Lesen Sie dazu den Artikel [Steuern und Gewähren des Datenbankzugriffs für SQL-Datenbank und SQL Data Warehouse](sql-database-manage-logins.md).

- Bewährte Methoden für die Kennwortverwaltung:
  - Stellen Sie ein komplexes Kennwort bereit, das aus lateinischen Groß- und Kleinbuchstaben, Ziffern (0 bis 9) und nicht alphanumerischen Zeichen (etwa $, !, # oder %) besteht.
  - Verwenden Sie längere Passphrases anstelle von kürzeren, zufällig ausgewählten Zeichen.
  - Erzwingen Sie mindestens alle 90 Tage eine manuelle Kennwortänderung.

## <a name="access-management"></a>Zugriffsverwaltung

Zugriffsverwaltung ist der Vorgang der Steuerung und Verwaltung des Zugriffs und der Berechtigungen autorisierter Benutzer auf die Azure SQL-Datenbank.

### <a name="implement-principle-of-least-privilege"></a>Implementieren des Prinzips der geringsten Rechte

> [!NOTE]
> Erwähnt in: FedRamp-Kontrollmechanismen AC-06, NIST: AC-6, OSA-Methode Nr. 3

Das Prinzip der geringsten Rechte besagt, dass Benutzer nicht umfangreichere Berechtigungen besitzen sollten, als für die Ausführung ihrer Aufgaben erforderlich sind. Weitere Informationen finden Sie im Artikel [Minimale Administration](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview).

**Implementierung**:

Weisen Sie nur die notwendigen [Berechtigungen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) zu, um die erforderlichen Aufgaben auszuführen:

- Auf der SQL-Datenebene: 
    - Verwenden Sie granulare Berechtigungen und benutzerdefinierte Datenbankrollen (oder Serverrollen in MI): 
        1. Erstellen der erforderlichen Rollen
            - [CREATE ROLE](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql)
            - [CREATE SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/create-server-role-transact-sql)
        1. Erstellen der erforderlichen Benutzer
            - [CREATE USER](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql)
        1. Hinzufügen von Benutzern als Mitglieder zu Rollen 
            - [ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql)
            - [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql)
        1. Weisen Sie dann Rollen Berechtigungen zu. 
            - [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) 
    - Stellen Sie sicher, dass Benutzer nicht unnötigen Rollen zugewiesen werden.

- In Azure Resource Manager:
  - Verwenden Sie integrierte Rollen (sofern verfügbar) oder benutzerdefinierte RBAC-Rollen, und weisen Sie die erforderlichen Berechtigungen zu.
    - [Integrierte Rollen für Azure](../role-based-access-control/built-in-roles.md) 
    - [Benutzerdefinierte Rollen für Azure-Ressourcen](../role-based-access-control/custom-roles.md)

**Bewährte Methoden:**

Die folgenden bewährten Methoden sind optional, führen jedoch zu einer besseren Verwaltbarkeit und Unterstützbarkeit Ihrer Sicherheitsstrategie: 

- Beginnen Sie nach Möglichkeit mit dem geringstmöglichen Satz an Berechtigungen, und fangen Sie an, Berechtigungen einzeln hinzuzufügen, wenn es wirklich notwendig (und gerechtfertigt) ist – im Gegensatz zum umgekehrten Ansatz: Berechtigungen Schritt für Schritt zu entfernen. 

- Verzichten Sie darauf, einzelnen Benutzern Berechtigungen zuzuweisen. Verwenden Sie stattdessen konsistent Rollen (Datenbank- oder Serverrollen). Rollen helfen bei der Berichterstellung und bei der Problembehandlung von Berechtigungen. (Azure RBAC unterstützt nur die Berechtigungszuweisung über Rollen.) 

- Verwenden Sie integrierte Rollen, wenn die Berechtigungen der Rollen genau den erforderlichen Berechtigungen für den Benutzer entsprechen. Sie können Benutzer mehreren Rollen zuweisen. 

- Erstellen und verwenden Sie benutzerdefinierte Rollen, wenn integrierte Rollen zu viele oder unzureichende Berechtigungen gewähren. Typische Rollen, die in der Praxis verwendet werden: 
  - Sicherheitsbereitstellung 
  - Administrator 
  - Entwickler 
  - Supportpersonal 
  - Prüfer 
  - Automatisierte Prozesse 
  - Endbenutzer 

- Beachten Sie, dass die Berechtigungen in SQL Server-Datenbank-Engine auf die folgenden Bereiche angewendet werden können. Je kleiner der Bereich, desto geringer die Auswirkungen der gewährten Berechtigungen: 
  - Azure SQL-Datenbank-Server (spezielle Rollen in der Masterdatenbank) 
  - Datenbank 
  - Schema (siehe auch: [Schemaentwurf für SQL Server: Empfehlungen für den Schemaentwurf unter Sicherheitsaspekten](http://andreas-wolter.com/en/schema-design-for-sql-server-recommendations-for-schema-design-with-security-in-mind/))
  - Objekt (Tabelle, Sicht, Prozedur usw.) 
  > [!NOTE]
  > Es wird nicht empfohlen, Berechtigungen auf Objektebene anzuwenden, da diese Ebene der Gesamtimplementierung unnötige Komplexität hinzufügt. Wenn Sie sich dafür entscheiden, Berechtigungen auf Objektebene zu verwenden, sollten diese eindeutig dokumentiert werden. Gleiches gilt für Berechtigungen auf Spaltenebene, die aus denselben Gründen noch weniger empfehlenswert sind. Die Standardregeln für [DENY](https://docs.microsoft.com/sql/t-sql/statements/deny-object-permissions-transact-sql) gelten nicht für Spalten.

- Führen Sie regelmäßige Überprüfungen mithilfe der [Sicherheitsrisikobewertung (VA)](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) aus, um auf zu umfangreiche Berechtigungen zu testen.

### <a name="implement-separation-of-duties"></a>Implementieren klarer Aufgabentrennung

> [!NOTE]
> Erwähnt in: FedRamp: AC-04, NIST: AC-5, ISO: A.6.1.2, PCI 6.4.2, SOC: CM-3, SDL-3

Bei der Trennung von Aufgaben, auch als „Aufteilung von Aufgaben“ bezeichnet, wird beschrieben, wie Sie sensible Aufgaben in mehrere Unteraufgaben aufteilen, die verschiedenen Benutzern zugewiesen werden, um Datenverletzungen zu verhindern.

**Implementierung**:

- Identifizieren Sie die erforderliche Ebene der Trennung von Aufgaben. Beispiele: 
  - Aufgabentrennung zwischen Entwicklungs-/Test- und Produktionsumgebungen 
  - Sicherheitsbezogene sensible Aufgaben im Vergleich zu Datenbankadministratoraufgaben (DBA) auf Verwaltungsebene und Entwickleraufgaben. 
    - Beispiele: Prüfer, Erstellen einer Sicherheitsrichtlinie für Sicherheit auf Rollenebene (Role-Level Security, RLS), Implementieren von SQL-Datenbank-Objekten mit DDL-Berechtigungen.

- Identifizieren Sie eine umfassende Hierarchie von Benutzern (und automatisierten Prozessen), die auf das System zugreifen.

- Erstellen Sie Rollen entsprechend den erforderlichen Benutzergruppen, und weisen Sie Rollen Berechtigungen zu. 
  - Für Aufgaben auf Verwaltungsebene im Azure-Portal oder über PowerShell-Automation verwenden Sie RBAC-Rollen. Suchen Sie entweder nach einer integrierten Rolle, die mit der Anforderung übereinstimmt, oder erstellen Sie eine benutzerdefinierte RBAC-Rolle mithilfe der verfügbaren Berechtigungen. 
  - Erstellen Sie Serverrollen für serverweite Aufgaben (Erstellen neuer Anmeldungen, Datenbanken) in einer verwalteten Instanz. 
  - Erstellen Sie Datenbankrollen für Aufgaben auf Datenbankebene.

- Für bestimmte sensible Aufgaben sollten Sie die Erstellung spezieller gespeicherter Prozeduren in Erwägung ziehen, die durch ein Zertifikat signiert werden, um Aufgaben im Namen von Benutzern auszuführen. 
  - Beispiel: [Tutorial: Signieren von gespeicherten Prozeduren mit einem Zertifikat](https://docs.microsoft.com/sql/relational-databases/tutorial-signing-stored-procedures-with-a-certificate) 

- Implementieren Sie Transparent Data Encryption (TDE) mit vom Kunden verwalteten Schlüsseln in Azure Key Vault, um die Trennung von Aufgaben zwischen Datenbesitzern und Sicherheitsbesitzern zu ermöglichen. 
  - Lesen Sie dazu den Artikel [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung über das Azure-Portal](../storage/common/storage-encryption-keys-portal.md). 

- Um sicherzustellen, dass ein Datenbankadministrator keine Daten einsehen kann, die als äußerst sensibel eingestuft werden, aber trotzdem Datenbankadministratoraufgaben ausführen kann, können Sie Always Encrypted mit Rollentrennung verwenden. 
  - Weitere Informationen dazu finden Sie in den Artikeln [Übersicht über die Schlüsselverwaltung für Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted), [Schlüsselbereitstellung mit Rollentrennung](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell#KeyProvisionWithRoles) und [Spaltenhauptschlüsselrotation mit Rollentrennung](https://docs.microsoft.com/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell#column-master-key-rotation-with-role-separation). 

- In Fällen, in denen dies nicht durchführbar ist (zumindest nicht ohne große Kosten und Anstrengungen, die das System nahezu unbrauchbar machen können), können Kompromisse durch den Einsatz von kompensierenden Kontrollmechanismen eingegangen werden, etwa: 
  - Menschliches Eingreifen in Prozesse. 
  - Überwachungspfade – Weitere Informationen zur Überwachung finden Sie unter [Überwachen kritischer Sicherheitsereignisse](#audit-critical-security-events).

**Bewährte Methoden:**

- Stellen Sie sicher, dass unterschiedliche Konten für Entwicklungs-/Test- und Produktionsumgebungen verwendet werden. Unterschiedliche Konten helfen dabei, die Trennung von Test- und Produktionssystemen einzuhalten.

- Verzichten Sie darauf, einzelnen Benutzern Berechtigungen zuzuweisen. Verwenden Sie stattdessen konsistent Rollen (Datenbank- oder Serverrollen). Die Verwendung von Rollen hilft bei der Berichterstellung und bei der Problembehandlung von Berechtigungen.

- Verwenden Sie integrierte Rollen, wenn die Berechtigungen genau mit den erforderlichen Berechtigungen übereinstimmen. Wenn die Vereinigungsmenge aller Berechtigungen aus mehreren integrierten Rollen zu einer 100%igen Übereinstimmung führt, können Sie auch mehrere Rollen gleichzeitig zuweisen. 

- Erstellen und verwenden Sie benutzerdefinierte Rollen, wenn integrierte Rollen zu viele oder unzureichende Berechtigungen gewähren. 

- Rollenzuweisungen können auch temporär erfolgen. Dies wird auch als „dynamische Trennung von Aufgaben“ (Dynamic Separation of Duties, DSD) bezeichnet und erfolgt entweder innerhalb von Schritten des SQL Agent-Auftrags in T-SQL oder mithilfe von Azure PIM für RBAC-Rollen. 

- Stellen Sie sicher, dass Datenbankadministratoren keinen Zugriff auf die Verschlüsselungsschlüssel oder Schlüsselspeicher haben, und dass Sicherheitsadministratoren, die Zugriff auf die Schlüssel besitzen, ihrerseits über keinen Zugriff auf die Datenbank verfügen. 

- Stellen Sie immer sicher, dass Sie über einen Überwachungspfad für sicherheitsbezogene Aktionen verfügen. 

- Sie können die Definition der integrierten RBAC-Rollen abrufen, um die verwendeten Berechtigungen anzuzeigen und eine benutzerdefinierte Rolle basierend auf Ausschnitten und Kumulationen dieser über PowerShell zu erstellen. 

- Da jedes Mitglied der db_owner-Datenbankrolle Sicherheitseinstellungen wie Transparent Data Encryption (TDE) oder SLO ändern kann, sollte diese Mitgliedschaft mit Bedacht erteilt werden. Andererseits erfordern viele Aufgaben (etwa das Ändern von Datenbankeinstellungen, z.B. das Ändern von Datenbankoptionen) db_owner-Berechtigungen. Überwachung spielt in jeder Lösung eine wichtige Rolle.

- Es ist nicht möglich, einen db_owner nur durch Berechtigungen an Anzeigen von Benutzerdaten zu hindern. Wenn sich in einer Datenbank hochgradig vertrauliche Daten befinden, kann Always Encrypted verwendet werden, um sicher zu verhindern, dass Benutzer mit db_owner-Berechtigungen oder andere Datenbankadminsistratoren diese anzeigen können.

> [!NOTE]
> Das Erreichen der Trennung von Aufgaben (SoD) stellt für Sicherheits- und Problembehandlungsaufgaben eine Herausforderung dar. Andere Bereiche wie Entwicklungs- und Endbenutzerrollen können einfacher getrennt werden. Die meisten konformitätsbezogenen Kontrollmechanismen ermöglichen die Verwendung alternativer Kontrollfunktionen (z.B. Überwachung), wenn andere Lösungen nicht praktikabel sind.

Für Leser, die sich ausführlicher mit SoD beschäftigen möchten, empfehlen wir die folgenden Ressourcen: 

- Für Azure SQL-Datenbank:  
  - [Steuern und Gewähren des Datenbankzugriffs für SQL-Datenbank und SQL Data Warehouse](sql-database-manage-logins.md)
  - [Engine-Trennung von Aufgaben für den Anwendungsentwickler](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/cc974525(v=sql.100)) 
  - [Trennung von Aufgaben in SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=39269)
  - [Signieren von gespeicherten Prozeduren in SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server)

- Für Azure-Ressourcenverwaltung:
  - [Integrierte Rollen für Azure](../role-based-access-control/built-in-roles.md) 
  - [Benutzerdefinierte Rollen für Azure-Ressourcen](../role-based-access-control/custom-roles.md)
  - [Verwenden von Azure AD Privileged Identity Management für Zugriff mit erhöhten Rechten](https://www.microsoft.com/en-us/itshowcase/using-azure-ad-privileged-identity-management-for-elevated-access)

### <a name="perform-regular-code-reviews"></a>Ausführen von regulären Code Reviews

> [!NOTE]
> Erwähnt in: PCI: 6.3.2, SOC: SDL-3 

Die Trennung von Aufgaben ist nicht auf die Daten in der Datenbank beschränkt, sondern umfasst auch den Anwendungscode. Bösartiger Code kann Sicherheitskontrollen potenziell umgehen. Vor der Bereitstellung von benutzerdefiniertem Code in der Produktion ist es von entscheidender Bedeutung, den Inhalt der Bereitstellung zu überprüfen.

**Implementierung**:

- Verwenden Sie ein Datenbanktool wie Azure Data Studio, das Quellcodeverwaltung unterstützt. 

- Implementieren Sie einen Prozess für die getrennte Codebereitstellung.

- Vor dem Commit in den Hauptbranch muss eine andere Person als der Autor des Codes den Code auf mögliche Risiken durch die Erhöhung von Rechten sowie schädliche Datenänderungen überprüfen, um Schutz vor Betrug und nicht autorisierten Zugriffen sicherzustellen. Dies kann mithilfe von Quellcodeverwaltungsmechanismen erreicht werden.

**Bewährte Methoden:**

- Standardisierung: Es ist hilfreich, eine Standardprozedur zu implementieren, die für alle Codeaktualisierungen befolgt werden muss. 

- Die Sicherheitsrisikobewertung enthält Regeln, die auf übermäßige Berechtigungen, die Verwendung von alten Verschlüsselungsalgorithmen und andere Sicherheitsprobleme innerhalb eines Datenbankschemas prüfen. 

- Weitere Überprüfungen können in einer Qualitätssicherungs- oder Testumgebung mithilfe von Advanced Threat Protection durchgeführt werden. Dabei wird nach Code gesucht, der anfällig für SQL-Einschleusung ist.

- Beispiele dafür, worauf besonders zu achten ist: 
  - Erstellung eines Benutzers oder Ändern von Sicherheitseinstellungen aus einer Bereitstellung einer automatisierten SQL-Codeaktualisierung. 
  - Eine gespeicherte Prozedur, die in Abhängigkeit von den bereitgestellten Parametern einen monetären Wert in einer Zelle auf nicht konforme Weise aktualisiert. 

- Stellen Sie sicher, dass die Person, die die Überprüfung durchführt, eine andere Person als der ursprüngliche Codeautor und mit Code Reviews und sicherer Codierung vertraut ist.

- Stellen Sie sicher, dass alle Quellen für Codeänderungen bekannt sind: Code kann in T-SQL-Skripts vorliegen. Es kann sich um Ad-hoc-Befehle handeln, die ausgeführt oder in Form von Sichten, Funktionen, Triggern und gespeicherten Prozeduren bereitgestellt werden. Er kann auch Teil der Definitionen des SQL-Agent-Auftrags (Schritte) sein bzw. aus SSIS-Paketen, Azure Data Factory oder anderen Diensten ausgeführt werden.

## <a name="data-protection"></a>Schutz von Daten

Datenschutz besteht aus einer Reihe von Funktionen zum Schutz wichtiger Informationen vor einer Gefährdung durch Verschlüsselung oder Verschleierung.

> [!NOTE]
> Microsoft bescheinigt Azure SQL-Datenbank die Konformität mit FIPS 140-2 Level 1. Dies erfolgt nach der Verifizierung der strikten Verwendung von akzeptablen FIPS 140-2 Level 1-Algorithmen und validierten FIPS 140-2 Level 1-Instanzen dieser Algorithmen, einschließlich der Konsistenz mit den erforderlichen Schlüssellängen, der Schlüsselverwaltung, der Schlüsselgenerierung und der Schlüsselspeicherung. Diese Bescheinigung soll es unseren Kunden ermöglichen, auf den Bedarf oder die Anforderung für die Verwendung von validierten FIPS 140-2 Level 1-Instanzen bei der Verarbeitung von Daten oder der Bereitstellung von Systemen oder Anwendungen zu reagieren. Wir definieren die Begriffe „mit FIPS 140-2 Level 1 konform“ und „FIPS 140-2 Level 1-Konformität“, die in der obigen Erklärung verwendet werden, um ihre beabsichtigte Anwendbarkeit auf die Verwendung des unterschiedlichen Begriffs „für FIPS 140-2 Level 1 validiert“ durch die US-amerikanische und die kanadische Regierung zu demonstrieren. 


### <a name="encrypt-data-in-transit"></a>Verschlüsseln von Daten während der Übertragung

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 6, ISO-Kontrollmechanismusfamilie: Kryptografie

Schützt Ihre Daten, während Daten zwischen dem Client und dem Server übertragen werden. Weitere Informationen finden Sie unter [Netzwerksicherheit](#network-security).

### <a name="encrypt-data-at-rest"></a>Verschlüsselung ruhender Daten

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 6, ISO-Kontrollmechanismusfamilie: Kryptografie

Die Verschlüsselung ruhender Daten ist der kryptografische Schutz von Daten, wenn diese in einer Datenbank, einem Protokoll oder in Sicherungsdateien persistent gespeichert werden.

**Implementierung**:

- [Transparent Database Encryption (TDE)](transparent-data-encryption-azure-sql.md) mit vom Dienst verwalteten Schlüsseln ist standardmäßig für alle Datenbanken aktiviert, die nach 2017 in Azure SQL-Datenbank erstellt wurden.
- Wenn die Datenbank in einer verwalteten Instanz als Ergebnis eines Wiederherstellungsvorgangs von einem lokalen Server erstellt wird, wird die TDE-Einstellung der ursprünglichen Datenbank beibehalten. Wenn in der Originaldatenbank TDE nicht aktiviert ist, empfehlen wir, TDE für die verwaltete Instanz manuell zu aktivieren.

**Bewährte Methoden:**

- Speichern Sie keine Daten, für die Verschlüsselung ruhender Daten in der Masterdatenbank erforderlich ist. Die Masterdatenbank kann nicht mit TDE verschlüsselt werden.

- Verwenden Sie von Kunden verwaltete Schlüssel in Azure Key Vault, wenn Sie bessere Transparenz und differenzierte Kontrolle über den TDE-Schutz benötigen. Azure Key Vault bietet die Möglichkeit, Berechtigungen jederzeit zu widerrufen, damit auf die Datenbank nicht mehr zugegriffen werden kann. TDE-Schutzvorrichtungen können zentral mit anderen Schlüsseln verwaltet werden, oder Sie können die TDE-Schutzvorrichtung mithilfe von Azure Key Vault gemäß Ihrem eigenen Zeitplan rotieren.

- Wenn Sie von Kunden verwaltete Schlüssel in Azure Key Vault verwenden, befolgen Sie die Anleitungen in den Artikeln [Richtlinien für das Konfigurieren von TDE mit Azure Key Vault](transparent-data-encryption-byok-azure-sql.md#recommendations-when-configuring-akv) und [Konfigurieren von georedundanter Notfallwiederherstellung mit Azure Key Vault](transparent-data-encryption-byok-azure-sql.md#geo-dr-and-customer-managed-tde).

### <a name="protect-sensitive-data-in-use-from-high-privileged-unauthorized-users"></a>Schützen von sensiblen Daten im Gebrauch vor hochprivilegierten, nicht autorisierten Benutzern

Daten in Gebrauch sind die Daten, die bei der Ausführung von SQL-Abfragen im Speicher des Datenbanksystems gespeichert werden. Wenn Ihre Datenbank sensible Daten speichert, muss Ihr Unternehmen möglicherweise sicherstellen, dass hochprivilegierte Benutzer (etwa Microsoft-Operatoren oder Datenbankadministratoren in Ihrem Unternehmen) daran gehindert werden, die Daten aus dem Speicher des SQL Server-Prozesses zu extrahieren. Sie dürfen auch nicht in der Lage sein, die Klartextdaten bei der Abfrage der Datenbank anzuzeigen.

**Implementierung**:

- Verwenden Sie [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine), um sicherzustellen, dass sensible Daten in Azure SQL-Datenbank nicht als Klartext verfügbar gemacht werden (auch dann nicht, wenn sie sich im Arbeitsspeicher bzw. in Gebrauch befinden). Dies schützt die Daten vor Datenbankadministratoren (DBAs) und Cloudadministratoren (oder böswilligen Benutzern, die sich als hochprivilegierte Benutzer ausgeben können, aber nicht autorisiert sind) und verleiht Ihnen mehr Kontrolle darüber, wer auf Ihre Daten zugreifen kann.

**Bewährte Methoden:**

- Always Encrypted ist kein Ersatz für die Verschlüsselung ruhender Daten (TDE) oder bei der Übertragung (SSL/TLS) und sollte nicht für nicht sensible Daten verwendet werden, um die Auswirkungen auf die Leistung und Funktionalität zu minimieren. Die Verwendung von Always Encrypted in Verbindung mit TDE und TLS wird für einen umfassenden Schutz von ruhenden Daten, während der Übertragung und im Gebrauch empfohlen. 

- Verwalten Sie Always Encrypted-Schlüssel mit Rollentrennung, wenn Sie Always Encrypted verwenden, um Daten vor bösartigen Datenbankadministratoren zu schützen. Mit Rollentrennung erstellt ein Sicherheitsadministrator die physischen Schlüssel, und ein Datenbankadministrator erstellt Spaltenhauptschlüssel- und Spaltenverschlüsselungsschlüssel-Metadatenobjekte, die die physischen Schlüssel beschreiben, in der Datenbank. Während dieses Vorgangs benötigt der Sicherheitsadministrator keinen Zugriff auf die Datenbank, und der Datenbankadministrator benötigt keinen Zugriff auf die physischen Schlüssel in Klartext. 
  - Weitere Informationen finden Sie im Artikel [Verwalten von Schlüsseln mit Rollentrennung](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted#managing-keys-with-role-separation). 

- Speichern Sie die Spaltenhauptschlüssel in Azure Key Vault, um die Verwaltung zu vereinfachen. Vermeiden Sie die Verwendung des Windows-Zertifikatspeichers (und im Allgemeinen Lösungen mit verteiltem Schlüsselspeicher, im Gegensatz zu Lösungen für die zentrale Schlüsselverwaltung), die die Schlüsselverwaltung erschweren. 

- Berücksichtigen Sie sorgfältig die Vor- und Nachteile der Verwendung mehrerer Schlüssel (Spaltenhauptschlüssel oder Spaltenverschlüsselungsschlüssel). Halten Sie die Anzahl der Schlüssel gering, um die Schlüsselverwaltungskosten zu reduzieren. Ein Spaltenhauptschlüssel und ein Spaltenverschlüsselungsschlüssel pro Datenbank sind in der Regel in Steady-State-Umgebungen (nicht in der Mitte einer Schlüsselrotation) ausreichend, es sei denn, Sie verfügen über unterschiedliche Benutzergruppen, die jeweils unterschiedliche Schlüssel verwenden und auf andere Daten zugreifen.  

- Rotieren Sie die Spaltenhauptschlüssel gemäß Ihren Konformitätsanforderungen. Wenn Sie auch Spaltenverschlüsselungsschlüssel rotieren müssen, empfiehlt es sich, Onlineverschlüsselung zu verwenden, um Ausfallzeiten von Anwendungen zu minimieren. 
  - Lesen Sie dazu den Artikel [Überlegungen zu Leistung und Verfügbarkeit](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-column-encryption-using-powershell#performance-and-availability-considerations). 

- Verwenden Sie deterministische Verschlüsselung, wenn Berechnungen (Gleichheit) für Daten unterstützt werden müssen. Verwenden Sie andernfalls Verschlüsselung nach dem Zufallsprinzip. Vermeiden Sie die Verwendung von deterministischer Verschlüsselung für Datasets mit niedriger Entropie oder Datasets mit öffentlich bekannter Verteilung. 

- Wenn Sie Bedenken haben, dass Dritte ohne Ihre Zustimmung legal auf Ihre Daten zugreifen, stellen Sie sicher, dass alle Anwendungen und Tools, die Zugriff auf die Schlüssel und Daten in Klartext besitzen, außerhalb von Microsoft Azure Cloud ausgeführt werden. Ohne Zugriff auf die Schlüssel verfügt der Dritte nicht über die Möglichkeit, die Daten zu entschlüsseln, es sei denn, er umgeht die Verschlüsselung.

- Always Encrypted unterstützt das Erteilen von temporären Zugriffsberechtigungen auf die Schlüssel (und die geschützten Daten) nicht auf einfache Weise. Wenn Sie die Schlüssel z.B. für einen Datenbankadministrator freigeben müssen, um dem Datenbankadministrator das Ausführen einiger Bereinigungsvorgänge für sensible und verschlüsselte Daten zu ermöglichen, besteht die einzige Möglichkeit, den Zugriff auf die Daten für den Datenbankadministrator zuverlässig zu widerrufen, in der Rotation sowohl der Spaltenverschlüsselungsschlüssel als auch der Spaltenhauptschlüssel, die die Daten schützen. Dies ist ein kostspieliger Vorgang. 

- Um auf die Klartextwerte in verschlüsselten Spalten zuzugreifen, muss ein Benutzer über Zugriff auf den CMK verfügen, der die Spalten schützt. Dieser wird im Schlüsselspeicher konfiguriert, der den CMK enthält. Außerdem muss der Benutzer über die Datenbankberechtigungen VIEW ANY COLUMN MASTER KEY DEFINITION und VIEW ANY COLUMN ENCRYPTION KEY DEFINITION verfügen.

### <a name="control-access-of-application-users-to-sensitive-data-through-encryption"></a>Steuern des Zugriffs von Anwendungsbenutzern auf sensible Daten durch Verschlüsselung

Verschlüsselung kann verwendet werden, um sicherzustellen, dass nur bestimmte Anwendungsbenutzer, die Zugriff auf Kryptografieschlüssel besitzen, die Daten anzeigen oder aktualisieren können.

**Implementierung**:

- Verwenden Sie CLE (Column Level Encryption). Weitere Informationen finden Sie im Artikel [Verschlüsseln einer Datenspalte](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data). 
- Verwenden Sie alternativ Always Encrypted, aber beachten Sie dabei die unten aufgeführten Einschränkungen.

**bewährten Methoden**

Bei Verwendung von CLE:

- Steuern Sie den Zugriff auf Schlüssel durch SQL-Berechtigungen und -Rollen. 

- Verwenden Sie AES (AES 256 empfohlen) für die Datenverschlüsselung. Algorithmen wie RC4, DES und TripleDES sind veraltet und sollten aufgrund bekannter Sicherheitsrisiken nicht verwendet werden. 

- Schützen Sie symmetrische Schlüssel mit asymmetrischen Schlüsseln/Zertifikaten (nicht mit Kennwörtern), um die Verwendung von 3DES zu vermeiden. 

- Seien Sie vorsichtig, wenn Sie eine Datenbank mithilfe von CLE über Export-/Importfunktionen (bacpac-Dateien) migrieren. 
  - Weitere Informationen dazu, wie Sie den Verlust von Schlüsseln beim Migrieren von Daten verhindern können, sowie weitere Empfehlungen zu bewährten Methoden finden Sie im Artikel [Empfehlungen zur Verwendung von CLE in Azure SQL-Datenbank](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/).

Denken Sie bei der Verwendung von Always Encrypted daran, dass Always Encrypted in erster Linie dazu dient, die verwendeten sensiblen Daten im Gebrauch vor hochprivilegierten Benutzern von Azure SQL-Datenbank (Cloudbetreiber, Datenbankadministratoren) zu schützen. Lesen Sie dazu [Schützen von sensiblen Daten im Gebrauch vor hochprivilegierten, nicht autorisierten Benutzern](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users). Seien Sie sich der folgenden Herausforderungen bewusst, wenn Sie Always Encrypted verwenden, um Daten vor Anwendungsbenutzern zu schützen:

- Sobald Sie einem Benutzer Zugriff auf vertrauliche Daten gewähren, indem Sie ihm Berechtigungen für den Zugriff auf den Spaltenverschlüsselungsschlüssel und den Spaltenhauptschlüssel erteilen, müssen Sie den Spaltenverschlüsselungsschlüssel rotieren, um diesen Zugriff zuverlässig zu widerrufen. Dies ist ein teurer Vorgang, der eine erneute Verschlüsselung aller Spalten erfordert, die durch den Spaltenverschlüsselungsschlüssel geschützt werden. 

- Standardmäßig verwalten alle Microsoft-Clienttreiber, die Always Encrypted unterstützen, einen globalen Cache (ein Cache pro Anwendung) von Spaltenverschlüsselungsschlüsseln. Sobald ein Clienttreiber einen Klartext-Spaltenverschlüsselungsschlüssel abruft, indem er einen Schlüsselspeicher kontaktiert, der einen Spaltenhauptschlüssel enthält, wird der Klartext-Spaltenverschlüsselungsschlüssel zwischengespeichert, was die Isolierung von Daten von Benutzern einer Mehrbenutzeranwendung erschwert. Wenn Ihre Anwendung bei der Interaktion mit einem Schlüsselspeicher (z. B. Azure Key Vault) die Identität von Endbenutzern annimmt, wird von einer nachfolgenden Abfrage, die denselben Schlüssel erfordert, aber von einem anderen Benutzer ausgelöst wird, der zwischengespeicherte Schlüssel verwendet, nachdem die Abfrage eines Benutzers den Cache mit einem Spaltenverschlüsselungsschlüssel aufgefüllt hat. Der Treiber ruft den Schlüsselspeicher nicht auf und überprüft nicht, ob der zweite Benutzer über die Berechtigung für den Zugriff auf den Spaltenverschlüsselungsschlüssel verfügt. Folglich kann der Benutzer die verschlüsselten Daten auch dann anzeigen, wenn der Benutzer keinen Zugriff auf die Schlüssel besitzt. Um die Isolation von Benutzern in einer Anwendung mit mehreren Benutzern zu erreichen, müssen Sie möglicherweise die Zwischenspeicherung von Spaltenverschlüsselungsschlüsseln deaktivieren. Dies führt zu zusätzlichem Leistungsmehraufwand, da der Treiber den Schlüsselspeicher für jeden Datenverschlüsselungs- oder -entschlüsselungsvorgang kontaktieren muss.

### <a name="protect-data-against-unauthorized-viewing-by-application-users-while-preserving-data-format"></a>Schützen von Daten vor nicht autorisierter Anzeige durch Anwendungsbenutzer unter Beibehaltung des Datenformats
Eine weitere Technik zur Verhinderung der Anzeige von Daten durch unbefugte Benutzer ist die Verschleierung oder Maskierung der Daten unter Beibehaltung der Datentypen und -formate, um sicherzustellen, dass Benutzeranwendungen die Daten weiterhin verarbeiten und anzeigen können.

**Implementierung**:

- Verwenden Sie [dynamische Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking), um Tabellenspalten zu verschleiern.

> [!NOTE]
> Always Encrypted funktioniert nicht mit dynamischer Datenmaskierung. Es ist nicht möglich, dieselbe Spalte zu verschlüsseln und zu maskieren. Dies impliziert, dass Sie den Schutz von Daten in Gebrauch im Vergleich mit dem Maskieren der Daten für Ihre App-Benutzer über dynamische Datenmaskierung priorisieren müssen.

**Bewährte Methoden:**

> [!NOTE]
> Dynamische Datenmaskierung kann nicht verwendet werden, um Daten vor Benutzern mit hohen Berechtigungen zu schützen. Maskierungsrichtlinien gelten nicht für Benutzer mit administrativem Zugriff (z.B. db_owner).

- Gestatten Sie App-Benutzern nicht das Ausführen von Ad-hoc-Abfragen (da Sie dynamische Datenmaskierung möglicherweise umgehen können).  
  - Weitere Informationen finden Sie im Artikel [Umgehen der Maskierung mithilfe von Rückschluss- oder Brute-Force-Verfahren](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking#security-note-bypassing-masking-using-inference-or-brute-force-techniques).  

- Verwenden Sie eine geeignete Zugriffssteuerungsrichtlinie (über SQL-Berechtigungen, Rollen, RLS), um die Benutzerberechtigungen für Aktualisierungen in den maskierten Spalten einzuschränken. Das Erstellen einer Maske für eine Spalte verhindert keine Aktualisierungen dieser Spalte. Obwohl Benutzer beim Abfragen der maskierten Spalte auch maskierte Daten erhalten, können dieselben Benutzer die Daten aktualisieren, wenn sie über die entsprechenden Schreibberechtigungen verfügen.    

-  Dynamische Datenmaskierung behält nicht die statistischen Eigenschaften der maskierten Werte bei. Dies kann sich auf die Abfrageergebnisse auswirken (z.B. auf Abfragen, die Filterprädikate oder Verknüpfungen der maskierten Daten enthalten).

## <a name="network-security"></a>Netzwerksicherheit
Netzwerksicherheit bezieht sich auf Zugriffssteuerungen und bewährte Methoden, um Ihre Daten während der Übertragung in Azure SQL-Datenbank zu sichern.

### <a name="configure-my-client-to-connect-securely-to-azure-sql-database"></a>Konfigurieren des Clients für eine sichere Verbindung mit Azure SQL-Datenbank 
Schützen Sie Clientcomputer und Anwendungen, die eine Verbindung mit Azure SQL-Datenbank herstellen, vor bekannten Sicherheitsrisiken aufgrund einer Abhängigkeit von älteren Protokollen und Verschlüsselungssammlungen.

**Implementierung**:

- Stellen Sie sicher, dass Clientcomputer, die eine Verbindung mit Azure SQL-Datenbank herstellen, [Transport Layer Security (TLS)](sql-database-security-overview.md#transport-layer-security-tls-encryption-in-transit) verwenden.

**Bewährte Methoden:**

- Konfigurieren Sie alle Apps und Tools für das Herstellen einer Verbindung mit SQL-Datenbank mit aktivierter Verschlüsselung. 
  - Encrypt = On, TrustServerCertificate = Off (oder äquivalent mit Nicht-Microsoft-Treibern). 

- Wenn Ihre App einen Treiber verwendet, der TLS nicht unterstützt oder eine ältere Version von TLS unterstützt, ersetzen Sie den Treiber, wenn dies möglich ist. Wenn dies nicht möglich ist, sollten Sie die Sicherheitsrisiken sorgfältig auswerten. 

- Verringern Sie Angriffsvektoren durch Sicherheitsrisiken in SSL 2.0, SSL 3.0, TLS 1.0 und TLS 1.1, indem Sie diese Versionen auf Clientcomputern deaktivieren, die eine Verbindung mit Azure SQL-Datenbank über [TLS-Registrierungseinstellungen (Transport Layer Security)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-10) herstellen. 

- Überprüfen Sie die auf dem Client verfügbaren Verschlüsselungssammlungen über [Verschlüsselungssammlungen in TLS/SSL (Schannel SSP)](https://docs.microsoft.com/windows/desktop/SecAuthN/cipher-suites-in-schannel), und deaktivieren Sie speziell 3DES über [Konfigurieren der Reihenfolge der TLS-Verschlüsselungssammlungen](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order). 

- Wenn Sie eine verwaltete Instanz verwenden, verwenden Sie den Verbindungstyp **Proxy** (Standard), da dies Verschlüsselung von der Serverseite erzwingt. Der Verbindungstyp **Redirect** (Umleitung) unterstützt derzeit keine Verschlüsselungserzwingung und ist nur für private IP-Verbindungen verfügbar. 
  - Weitere Informationen finden Sie unter [Verbindungsarchitektur von Azure SQL: Verbindungsrichtlinie](sql-database-connectivity-architecture.md#connection-policy).
  - Verschlüsselung wird für die Verbindungstypen „Proxy“ und „Redirect“ (Umleitung) erzwungen. 

### <a name="minimize-attack-surface"></a>Minimieren der Angriffsfläche
Minimieren Sie die Anzahl der Features, die von einem böswilligen Benutzer angegriffen werden können, indem Sie Netzwerkzugriffssteuerungen für Azure SQL-Datenbank implementieren.

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 5

**Implementierung**:

Auf einem Azure SQL-Datenbank-Server (mit Singleton-Datenbank oder Pools für elastische Datenbanken):
- Legen Sie den Zugriff auf Azure-Dienste auf OFF (AUS) fest.

- Verwenden Sie VNET-Dienstendpunkte und VNET-Firewallregeln.

- Verwenden Sie Private Link (Vorschau).

In einer verwalteten Instanz:
- Befolgen Sie die Richtlinien unter [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements). 

**Bewährte Methoden:**

- Schränken Sie den Zugriff auf Azure SQL-Datenbank durch Herstellen einer Verbindung mit einem privaten Endpunkt (z.B. mit einem privaten Datenpfad) ein: 
  - Eine verwaltete Instanz kann innerhalb eines virtuellen Netzwerks isoliert werden, um externen Zugriff zu verhindern. Anwendungen und Tools, die sich im gleichen oder in einem virtuellen Peernetzwerk in derselben Region befinden, können direkt darauf zugreifen. Anwendungen und Tools in einer anderen Region können VNET-zu-VNET-Verbindungen oder ExpressRoute-Leitungspeering verwenden, um eine Verbindung herzustellen. Der Kunde sollte Netzwerksicherheitsgruppen (NSG) verwenden, um den Zugriff über Port 1433 ausschließlich auf Ressourcen einzuschränken, die Zugriff auf eine verwaltete Instanz benötigen. 
  - Verwenden Sie für einen SQL-Datenbank-Server (mit Einzeldatenbanken oder Pools für elastische Datenbanken) das Feature [Private Link](sql-database-private-endpoint-overview.md), das eine dedizierte private IP-Adresse für den SQL-Datenbank-Server in Ihrem virtuellen Netzwerk bereitstellt. Sie können auch [VNET-Dienstendpunkte mit VNET-Firewallregeln](sql-database-vnet-service-endpoint-rule-overview.md) verwenden, um den Zugriff auf Ihre SQL-Datenbank-Server einzuschränken.
  - Benutzer von mobilen Geräten sollten Point-to-Site-VPN-Verbindungen verwenden, um eine Verbindung über den Datenpfad herzustellen.
  - Benutzer, die mit Ihrem lokalen Netzwerk verbunden sind, sollten eine Site-to-Site-VPN-Verbindung oder ExpressRoute verwenden, um eine Verbindung über den Datenpfad herzustellen.

- Sie können auf Azure SQL-Datenbank zugreifen, indem Sie eine Verbindung mit einem öffentlichen Endpunkt herstellen (z.B. mithilfe eines öffentlichen Datenpfads). Die folgenden bewährten Methoden sollten in Betracht gezogen werden: 
  - Verwenden Sie für einen SQL-Datenbank-Server [IP-Firewallregeln](sql-database-firewall-configure.md), um den Zugriff nur auf autorisierte IP-Adressen einzuschränken.
  - Verwenden Sie für eine verwaltete Instanz Netzwerksicherheitsgruppen (NSG), um den Zugriff über Port 3342 nur auf die erforderlichen Ressourcen zu beschränken. Weitere Informationen finden Sie unter [Sicheres Verwenden einer verwalteten Azure SQL-Datenbank-Instanz mit öffentlichen Endpunkten](sql-database-managed-instance-public-endpoint-securely.md). 

> [!NOTE]
> Der öffentliche Endpunkt einer verwalteten Instanz ist standardmäßig nicht aktiviert und muss explizit aktiviert werden. Wenn die Unternehmensrichtlinie die Verwendung öffentlicher Endpunkte nicht zulässt, verwenden Sie [Azure Policy](../governance/policy/overview.md), um zu verhindern, dass öffentliche Endpunkte überhaupt aktiviert werden können.

- Einrichten von Azure-Netzwerkkomponenten: 
  - Befolgen Sie die Anleitungen unter [Bewährte Methoden für die Netzwerksicherheit in Azure](../security/fundamentals/network-best-practices.md).
  - Planen Sie die VNET-Konfiguration gemäß den bewährten Methoden, die unter [Azure Virtual Network: Häufig gestellte Fragen (FAQ)](../virtual-network/virtual-networks-faq.md) und im Plan beschrieben werden. 
  - Segmentieren Sie ein VNET in mehrere Subnetze, und weisen Sie dem gleichen Subnetz Ressourcen für eine ähnliche Rolle zu (z.B. Front-End- im Vergleich zu Back-End-Ressourcen).
  - Verwenden Sie [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/security-overview.md), um den Datenverkehr zwischen Subnetzen innerhalb der Azure-VNET-Grenze zu steuern.
  - Aktivieren Sie [Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) für Ihr Abonnement, um den eingehenden und ausgehenden Netzwerkdatenverkehr zu überwachen.

### <a name="configure-power-bi-for-secure-connections-to-azure-sql-database"></a>Konfigurieren von Power BI für sichere Verbindungen mit Azure SQL-Datenbank

**Bewährte Methoden:**

- Verwenden Sie für Power BI Desktop nach Möglichkeit den privaten Datenpfad. 

- Stellen Sie sicher, dass Power BI Desktop mithilfe von TLS 1.2 eine Verbindung herstellt, indem Sie den Registrierungsschlüssel auf dem Clientcomputer gemäß den [TLS-Registrierungseinstellungen (Transport Layer Security)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) festlegen. 

- Beschränken Sie den Datenzugriff für bestimmte Benutzer über [Sicherheit auf Zeilenebene (Row-Level Security, RLS) mit Power BI](https://docs.microsoft.com/power-bi/service-admin-rls). 

- Verwenden Sie für den Power BI-Dienst das [lokale Datengateway](https://docs.microsoft.com/power-bi/service-gateway-onprem), und beachten Sie dabei die [Einschränkungen und Überlegungen](https://docs.microsoft.com/power-bi/service-gateway-deployment-guidance#installation-considerations-for-the-on-premises-data-gateway).

### <a name="configure-app-service-for-secure-connections-to-azure-sql-database"></a>Konfigurieren von App Service für sichere Verbindungen mit Azure SQL-Datenbank

**Bewährte Methoden:**

- Für eine einfache Web-App muss für das Herstellen einer Verbindung über einen öffentlichen Endpunkt **Allow Azure Services** (Azure-Dienste zulassen) auf ON (Ein) festgelegt werden. 

- [Integrieren Sie Ihre App in ein virtuelles Azure-Netzwerk](../app-service/web-sites-integrate-with-vnet.md), einem Verbindung über einen privaten Datenpfad mit einer verwalteten Instanz bereitzustellen. Optional können Sie eine Web-App auch mit [App Service-Umgebungen (ASE)](../app-service/environment/intro.md) bereitstellen. 

- Für eine Web-App mit ASE oder eine VNET-integrierte Web-App, die eine Verbindung mit einer Datenbank in SQL-Datenbank-Server herstellt, können Sie [VNET-Dienstendpunkte und VNET-Firewallregeln](sql-database-vnet-service-endpoint-rule-overview.md) verwenden, um den Zugriff aus einem bestimmten VNET und Subnetz einzuschränken, und dann **Allow Azure Services** (Azure-Dienste zulassen) auf OFF (Aus) festlegen. Sie können ASE auch über einen privaten Datenpfad mit einer verwalteten Instanz verbinden.  

- Stellen Sie sicher, dass Ihre Web-App wie im Artikel [Bewährte Methoden zum Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Service](../security/security-paas-applications-using-app-services.md) beschrieben konfiguriert ist. 

- Installieren Sie die [Web Application Firewall (WAF)](../application-gateway/waf-overview.md), um Ihre Webanwendung vor gängigen Exploits und Sicherheitsrisiken zu schützen.

### <a name="configure-azure-vm-hosting-for-secure-connections-to-azure-sql-database"></a>Konfigurieren von Azure VM-Hosting für sichere Verbindungen mit Azure SQL-Datenbank

**Bewährte Methoden:**

- Verwenden Sie eine Kombination aus Zulassungs- und Ablehnungsregeln für die NSGs von Azure-VMs, um zu steuern, auf welche Regionen von der VM aus zugegriffen werden kann.

- Stellen Sie sicher, dass Ihre VM gemäß dem Artikel [Bewährten Sicherheitsmethoden für IaaS-Workloads in Azure](../security/azure-security-iaas.md) konfiguriert ist.

- Stellen Sie sicher, dass alle VMs einem bestimmten VNET und einem Subnetz zugeordnet sind. 

- Finden Sie heraus, ob Sie die Standardroute 0.0.0.0/Internet gemäß der Anleitung unter [Informationen zu erzwungenem Tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md#about-forced-tunneling) benötigen. 
  - Wenn dies der Fall ist (z.B. bei einem Front-End-Subnetz), behalten Sie die Standardroute bei.
  - Wenn dies nicht der Fall ist (z.B. mittlere Ebene oder Back-End-Subnetz), aktivieren Sie erzwungenes Tunneling, sodass kein Datenverkehr über das Internet in die lokale Umgebung (also standortübergreifend) gelangt. 

- Implementieren Sie [optionale Standardrouten](../virtual-network/virtual-networks-udr-overview.md#optional-default-routes), wenn Sie Peering verwenden oder eine Verbindung mit einer lokalen Umgebung herstellen. 

- Implementieren Sie [benutzerdefinierte Routen](../virtual-network/virtual-networks-udr-overview.md#user-defined), wenn Sie den gesamten Datenverkehr im VNET für die Paketüberprüfung an ein virtuelles Netzwerkgerät senden müssen. 

- Verwenden Sie [VNET-Dienstendpunkte](sql-database-vnet-service-endpoint-rule-overview.md) für den sicheren Zugriff auf PaaS-Dienste wie Azure Storage über das Azure-Backbonenetzwerk. 

### <a name="protect-against-distributed-denial-of-service-ddos-attacks"></a>Schutz vor DDoS-Angriffen (verteilte Denial-of-Service-Angriffe).
DDoS-Angriffe (verteilte Denial-of-Service-Angriffe) werden von böswilligen Benutzern verwendet, um eine Flut von Netzwerkdatenverkehr an Azure SQL-Datenbank zu senden. Das Ziel besteht dabei darin, die Azure-Infrastruktur zu überlasten und damit gültige Anmeldungen und Workloads abzulehnen.

> [!NOTE]
> Erwähnt in: OSA-Methode Nr. 9

**Implementierung**:

DDoS-Schutz wird im Rahmen der Azure-Plattform automatisch aktiviert. Er umfasst ununterbrochene Datenverkehrsüberwachung sowie Risikominderung in Echtzeit von Angriffen auf öffentliche Endpunkte auf Netzwerkebene. 

- Verwenden Sie [Azure DDoS Protection](../virtual-network/ddos-protection-overview.md), um öffentliche IP-Adressen zu überwachen, die in virtuellen Netzwerken bereitgestellten Ressourcen zugeordnet sind.

- Verwenden Sie [Advanced Threat Protection für Azure SQL-Datenbank](sql-database-threat-detection-overview.md), um DOS-Angriffe (Denial-of-Service) auf Datenbanken zu erkennen.

**Bewährte Methoden:**

- Befolgen Sie die unter [Minimieren der Angriffsfläche](#minimize-attack-surface) beschriebenen Verfahren, um Bedrohungen durch DDoS-Angriffe zu minimieren. 

- Die Advanced Threat Protection-Warnung **Brute-Force-SQL-Anmeldeinformationen** hilft, Brute-Force-Angriffe zu erkennen. In einigen Fällen kann die Warnung sogar Workloads für Penetrationstests unterscheiden. 

- Für Azure-VM-Hostinganwendungen, die eine Verbindung mit SQL-Datenbank herstellen: 
  - Befolgen Sie die Empfehlungen zum Einschränken des Zugriffs über Endpunkte mit Internetzugriff in Azure Security Center. 
  - Verwenden Sie VM-Skalierungsgruppen, um mehrere Instanzen der Anwendung auf Azure-VMs auszuführen. 
  - Deaktivieren Sie RDP und SSH aus dem Internet, um Brute-Force-Angriffe zu verhindern. 

## <a name="monitoring-logging-and-auditing"></a>Überwachung, Protokollierung und Überprüfung  
Dieser Abschnitt beschreibt die Möglichkeiten zum Erkennen anomaler Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese missbräuchlich zu nutzen. Außerdem werden bewährte Methoden zum Konfigurieren der Datenbanküberwachung für die Nachverfolgung und Erfassung von Datenbankereignissen beschrieben.

### <a name="protect-databases-against-attacks"></a>Schützen von Datenbanken vor Angriffen 
Mit Advanced Threat Protection können Sie potenzielle Bedrohungen erkennen, sobald diese auftreten, und darauf reagieren, indem Sie Sicherheitswarnungen zu anomalen Aktivitäten einrichten.

**Implementierung**:

- Verwenden Sie [Advanced Threat Protection für SQL](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts), um ungewöhnliche und möglicherweise schädliche Versuchen zu erkennen, auf Datenbanken zuzugreifen oder diese unbefugt zu nutzen, etwa mithilfe von:
  - Angriff mit Einschleusung von SQL-Befehlen.
  - Diebstahl/Offenlegung von Anmeldeinformationen.
  - Missbrauch von Berechtigungen.
  - Datenexfiltration.

**Bewährte Methoden:**

- Konfigurieren Sie [Advanced Data Security](sql-database-advanced-data-security.md#getting-started-with-ads)  für Azure SQL-Datenbank für einen bestimmten SQL-Datenbank-Server oder eine verwaltete Instanz oder für alle SQL-Datenbank-Server und verwalteten Instanzen in einem Abonnement, indem Sie in die [Ebene „Azure Security Center Standard“](../security-center/security-center-pricing.md) wechseln. 

- Für eine vollständige Untersuchung empfiehlt es sich,  [SQL-Datenbank-Überwachung](sql-database-auditing.md) zu aktivieren, um Datenbankereignisse nachzuverfolgen und in ein Überwachungsprotokoll in einem Azure Storage-Konto oder einem Azure Log Analytics-Arbeitsbereich zu schreiben. 

### <a name="audit-critical-security-events"></a>Überwachen kritischer Sicherheitsereignisse
Das Nachverfolgen von Datenbankereignissen kann Ihnen dabei helfen, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können. Außerdem wird die Einhaltung von Compliancestandards ermöglicht und erleichtert. 

**Implementierung**:

- Aktivieren Sie  [SQL-Datenbank-Überwachung](sql-database-auditing.md), um Datenbankereignisse nachzuverfolgen und diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto, Ihrem Log Analytics-Arbeitsbereich (Vorschau) oder in Event Hubs (Vorschau) zu schreiben. 

- Überwachungsprotokolle können in ein Azure Storage-Konto, in einen Log Analytics-Arbeitsbereich für die Nutzung durch Azure Monitor-Protokolle oder in Event Hub für die Nutzung durch einen Event Hub geschrieben werden. Sie können eine beliebige Kombination dieser Optionen konfigurieren, und die Überwachungsprotokolle werden in die jeweils angegebenen Speicherorte geschrieben. 

**Bewährte Methoden:**

- Wenn Sie [SQL-Datenbank-Überwachung](sql-database-auditing.md) auf dem Datenbankserver zum Überwachen von Ereignissen konfigurieren, werden alle vorhandenen und neu erstellten Datenbanken auf diesem Server überwacht.
- Standardmäßig umfasst die Überwachungsrichtlinie alle Aktionen (Abfragen, gespeicherte Prozeduren und erfolgreiche und fehlgeschlagene Anmeldungen) für die Datenbanken, was zu einer großen Anzahl von Überwachungsprotokollen führen kann. Es wird empfohlen, dass Kunden [Überwachung für verschiedene Arten von Aktionen und Aktionsgruppen mithilfe von PowerShell konfigurieren](sql-database-auditing.md#subheading-7), um die Anzahl der überwachten Aktionen zu steuern und um das Risiko von Ereignisverlusten zu minimieren. Dies ermöglicht es Kunden, nur die wirklich benötigten Überwachungsdaten zu erfassen.
- Überwachungsprotokolle können direkt im [Azure-Portal](https://portal.azure.com/) oder aus dem konfigurierten Speicherort genutzt werden. 


> [!NOTE]
> Wenn Sie die Überwachung mit Log Analytics aktivieren, fallen Kosten an, deren Höhe sich nach der Erfassungsrate richtet. Beachten Sie die entsprechenden Kosten bei der Verwendung dieser [Option](https://azure.microsoft.com/pricing/details/monitor/), oder speichern Sie die Überwachungsprotokolle in einem Azure-Speicherkonto. 

**Weitere Ressourcen**:

- [SQL-Datenbanküberwachung](sql-database-auditing.md)
- [SQL Server-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) 

### <a name="secure-audit-logs"></a>Sichern von Überwachungsprotokollen
Um die Trennung von Aufgaben und DBA von Prüfern zu unterstützen, ist es entscheidend, Maßnahmen zu ergreifen, um den Zugriff auf das Speicherkonto einzuschränken. 

**Implementierung**:

- Beim Speichern von Überwachungsprotokollen in Azure Storage müssen Sie sicherstellen, dass der Zugriff auf das Speicherkonto auf die minimalen Sicherheitsprinzipien beschränkt ist, indem Sie den Zugriff auf das Speicherkonto steuern.

- Weitere Informationen finden Sie unter [Autorisierung des Zugriffs auf Azure Storage](../storage/common/storage-auth.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

**Bewährte Methoden:**

- Das Steuern des Zugriffs auf das Überwachungsziel ist ein wesentliches Konzept beim Trennen von DBA von Prüfern. 

- Wenn Sie den Zugriff auf sensible Daten überwachen, sollten Sie die Daten mit Datenverschlüsselung sichern, um die Offenlegung von Informationen für den Prüfer zu vermeiden. Weitere Informationen finden Sie im Abschnitt [Schützen von sensiblen Daten im Gebrauch vor hochprivilegierten, nicht autorisierten Benutzern](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users).

## <a name="security-management"></a>Sicherheitsverwaltung

In diesem Abschnitt werden die verschiedenen Aspekte und bewährten Methoden für die Verwaltung Ihres Datenbanksicherheitsstatus beschrieben. Er enthält bewährte Methoden, um sicherzustellen, dass Ihre Datenbanken so konfiguriert sind, dass sie Sicherheitsstandards erfüllen, um den Zugriff auf potenziell sensible Daten in ihren Datenbanken zu ermitteln, zu klassifizieren und nachzuverfolgen. 

### <a name="ensure-that-the-databases-are-configured-to-meet-security-best-practices"></a>Stellen Sie sicher, dass die Datenbanken für bewährte Sicherheitsmethoden konfiguriert sind. 

Optimieren Sie Ihre Datenbanksicherheit proaktiv, indem Sie potenzielle Datenbanksicherheitsrisiken ermitteln und beheben.

**Implementierung**:

- Aktivieren Sie die [SQL-Sicherheitsrisikobewertung](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) (Vulnerability Assessment, VA), um Ihre Datenbank auf Sicherheitsprobleme zu überprüfen und die Sicherheitsrisikobewertung automatisch regelmäßig für Ihre Datenbanken auszuführen.

**Bewährte Methoden:**

- Führen Sie zunächst eine Sicherheitsrisikobewertung für Ihre Datenbanken aus, und durchlaufen Sie die einzelnen Probleme, indem Sie fehlgeschlagene Überprüfungen korrigieren, die den bewährten Sicherheitsmethoden nicht genügen, und richten Sie dann Baselines für akzeptable Konfigurationen ein, bis die Überprüfung „sauber“ ist (alle Prüfungen werden bestanden).  

- Konfigurieren Sie regelmäßige wiederkehrende Überprüfungen so, dass sie ein Mal pro Woche ausgeführt werden, und legen Sie die relevante Person für den Empfang von Zusammenfassungs-E-Mails fest. 

- Überprüfen Sie die Zusammenfassung der Sicherheitsrisikobewertung nach jedem wöchentlichen Scan. Für alle gefundenen Sicherheitsrisiken sollten Sie die Abweichung vom vorherigen Überprüfungsergebnis auswerten und ermitteln, ob die Überprüfung korrigiert werden soll, oder ob es einen legitimen Grund für die Änderung der Konfiguration gibt.   

- Korrigieren Sie Überprüfungen, und aktualisieren Sie Baselines, sofern relevant, oder erstellen Sie Ticketelemente zum Auflösen von Aktionen und Nachverfolgen dieser Elemente, bis diese aufgelöst wurden. 

**Weitere Ressourcen**:

- [SQL-Sicherheitsrisikobewertung](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) 
- [Mit dem Dienst zur SQL-Sicherheitsrisikobewertung können Sie Datenbankschwachstellen erkennen](sql-vulnerability-assessment.md)

### <a name="identify-and-tag-sensitive-data"></a>Identifizieren und Markieren von sensiblen Daten 

Ermitteln Sie Spalten, die potenziell vertrauliche Daten enthalten. Klassifizieren Sie die Spalten, um erweiterte vertraulichkeitsbasierte Überwachungs- und Schutzszenarien zu nutzen. 

**Implementierung**:

- Verwenden Sie die [SQL-Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md), um sensible Daten in Ihren Datenbanken zu ermitteln, zu klassifizieren, zu markieren und zu schützen. 
  - Zeigen Sie die Klassifizierungsempfehlungen an, die durch die automatische Ermittlung im Dashboard der SQL-Datenermittlung und -klassifizierung erstellt werden, und akzeptieren Sie die relevanten Klassifizierungen, sodass Ihre sensiblen Daten dauerhaft mit Klassifizierungsbezeichnungen markiert werden. 
  - Fügen Sie manuell Klassifizierungen für alle zusätzlichen sensiblen Datenfelder hinzu, die nicht vom automatisierten Mechanismus erkannt wurden. 
- Weitere Informationen finden Sie unter [SQL-Datenermittlung und -klassifizierung](https://docs.microsoft.com/sql/relational-databases/security/sql-data-discovery-and-classification).

**Bewährte Methoden:**

- Überwachen Sie das Klassifizierungsdashboard in regelmäßigen Abständen für eine genaue Bewertung des Klassifizierungszustands der Datenbank. Ein Bericht zum Datenbankklassifizierungsstatus kann exportiert oder ausgegeben werden, um für Kompatibilitäts- und Überwachungszwecke zur Verfügung zu stehen.

- Überwachen Sie den Status der empfohlenen sensiblen Daten fortlaufend in der SQL-Sicherheitsrisikobewertung, indem Sie die Ermittlungsregel für sensible Daten nachverfolgen und Abweichungen in den empfohlenen Spalten für die Klassifizierung identifizieren.  

- Verwenden Sie die Klassifizierung so, dass sie auf die spezifischen Anforderungen Ihrer Organisation zugeschnitten ist. Passen Sie Ihre Information Protection-Richtlinie (Vertraulichkeitsbezeichnungen, Informationstypen, Ermittlungslogik) in der [SQL Information Protection](../security-center/security-center-info-protection-policy.md)-Richtlinie in Azure Security Center an. 

### <a name="track-access-to-sensitive-data"></a>Nachverfolgen von Zugriff auf sensible Daten 
Überwachen Sie, wer auf sensible Daten zugreift, und erfassen Sie Abfragen für sensible Daten in Überwachungsprotokollen.

**Implementierung**:

- Verwenden Sie SQL-Überwachung und -Datenklassifizierung in Kombination. 
  - In Ihrem [SQL-Datenbank-Überwachungsprotokoll](sql-database-auditing.md) können Sie den Zugriff insbesondere auf vertrauliche Daten nachverfolgen und Informationen wie etwa die Daten anzeigen, auf die zugegriffen wurde, sowie deren Vertraulichkeitsbezeichnung (siehe [Überwachen des Zugriffs auf sensible Daten](sql-database-data-discovery-and-classification.md#subheading-3)). 

**Bewährte Methoden:**

- Weitere Informationen finden Sie unter den bewährten Methoden der Abschnitte zur Überwachung und Datenklassifizierung: 
  - [Überwachen kritischer Sicherheitsereignisse](#audit-critical-security-events) 
  - [Identifizieren und Markieren von sensiblen Daten](#identify-and-tag-sensitive-data) 

### <a name="visualize-security-and-compliance-status"></a>Visualisieren des Sicherheits- und Compliancestatus 

Verwenden Sie ein einheitliches Infrastruktur-Sicherheitsverwaltungssystem, das den Sicherheitsstatus ihrer Rechenzentren (einschließlich der SQL-Datenbanken) stärkt. Zeigen Sie eine Liste mit Empfehlungen für die Sicherheit Ihrer Datenbanken und den Compliancestatus an.

**Implementierung**:

- Überwachen Sie SQL-bezogene Sicherheitsempfehlungen und aktive Bedrohungen in [Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/).

## <a name="common-security-threats-and-potential-mitigations"></a>Allgemeine Sicherheitsbedrohungen und potenzielle Risikominderungen

In diesem Abschnitt finden Sie Sicherheitsmaßnahmen zum Schutz vor bestimmten Angriffsvektoren. Es wird erwartet, dass die meisten Risikominderungen erreicht werden können, indem Sie mindestens eine der oben aufgeführten Sicherheitsrichtlinien befolgen.

### <a name="security-threat-data-exfiltration"></a>Sicherheitsbedrohung: Datenexfiltration

„Datenexfiltration“ bezeichnet das nicht autorisierte Kopieren, Übertragen oder Abrufen von Daten von einem Computer oder Server. Weitere Informationen finden Sie in der Definition von [Datenexfiltration](https://en.wikipedia.org/wiki/Data_exfiltration) auf Wikipedia.

Das Herstellen einer Verbindung mit dem Azure SQL-Datenbank-Server über einen öffentlichen Endpunkt stellt ein Datenexfiltrationsrisiko dar, da Kunden ihre Firewalls für öffentliche IP-Adressen öffnen müssen.  

**Szenario 1**: Eine Anwendung auf einer Azure-VM stellt eine Verbindung mit einer Datenbank auf einem Azure SQL-Datenbank-Server her. Ein nicht autorisierter Akteur erhält Zugriff auf den virtuellen Computer und kompromittiert ihn. In diesem Szenario bedeutet Datenexfiltration, dass eine externe Entität, die die nicht autorisierte VM verwendet, eine Verbindung mit der Datenbank herstellt, persönliche Daten kopiert und diese in einem Blobspeicher oder einer anderen SQL-Datenbank in einem anderen Abonnement speichert.

**Szenario 2**: Ein nicht autorisierter DBA. Dieses Szenario wird häufig von sicherheitssensiblen Kunden aus regulierten Branchen ausgelöst. In diesem Szenario kann ein Benutzer mit hohen Berechtigungen Daten aus Azure SQL-Datenbank in ein anderes Abonnement kopieren, das nicht vom Datenbesitzer gesteuert wird.

**Mögliche Risikominderungen**:

Derzeit bietet Azure SQL-Datenbank die folgenden Techniken zum Mindern von Bedrohungen durch Datenexfiltration: 

- Verwenden Sie eine Kombination aus Zulassungs- und Ablehnungsregeln für die NSGs von Azure-VMs, um zu steuern, auf welche Regionen von der VM aus zugegriffen werden kann. 
- Wenn ein Azure SQL-Datenbank-Server (mit Singleton-Datenbank oder Pools für elastische Datenbanken) verwendet wird, legen Sie Folgendes fest:
  - „Allow Azure Services“ (Azure-Dienste zulassen) auf OFF (Aus).
  - Erlauben Sie nur Datenverkehr aus dem Subnetz, das Ihre Azure-VM enthält, indem Sie eine VNET-Firewallregel einrichten.
  - Verwenden von [Private Link](sql-database-private-endpoint-overview.md)
- Für eine verwaltete Instanz wird bei Verwendung des privaten IP-Zugriffs standardmäßig das erste Datenexfiltrationsproblem eines nicht autorisierten virtuellen Computers berücksichtigt. Aktivieren Sie die Funktion für Subnetzdelegierung für ein Subnetz so, dass automatische die restriktivste Richtlinie für ein Subnetz einer verwalteten Instanz festgelegt wird.
- Das Problem eines nicht autorisierte DBAs wird mit einer verwalteten Instanz offensichtlicher, da sie über eine größere Oberfläche verfügt und die Netzwerkanforderungen für Kunden sichtbar sind. Die beste Risikominderung besteht unter diesen Umständen im Anwenden aller Methoden in diesem Sicherheitsleitfaden, um das Szenario mit einem nicht autorisierten DBA von vornherein zu verhindern (nicht nur für die Datenexfiltration). Always Encrypted ist eine Methode, um sensible Daten zu schützen, indem diese verschlüsselt werden und der Schlüssel für den Datenbankadministrator nicht zugänglich ist.

## <a name="security-aspects-of-business-continuity-and-availability"></a>Sicherheitsaspekte von Geschäftskontinuität und Verfügbarkeit

Bei den meisten Sicherheitsstandards wird die Verfügbarkeit von Daten im Hinblick auf die betriebliche Kontinuität behandelt, indem Redundanz und Failoverfunktionen implementiert werden, um Single Points of Failure zu vermeiden. In Notfallszenarien ist es üblich, Sicherungen von Daten- und Protokolldateien zu verwalten. Der folgende Abschnitt enthält eine allgemeine Übersicht über die Funktionen, die in Azure integriert sind, sowie zusätzliche Optionen, die für bestimmte Anforderungen konfiguriert werden können: 

- Azure bietet integrierte Hochverfügbarkeit: [Hochverfügbarkeit und Azure SQL-Datenbank](sql-database-high-availability.md) 

- Die unternehmenskritische Ebene umfasst Failovergruppen, mehrere Verfügbarkeitszonen, vollständige und differenzielle Protokollsicherungen und standardmäßig aktivierte Sicherungen für die Point-in-Time-Wiederherstellung:  
  - [Hochverfügbarkeit und Azure SQL-Datenbank: Zonenredundante Konfiguration](sql-database-high-availability.md#zone-redundant-configuration)
  - [Automatisierte Sicherungen](sql-database-automated-backups.md)
  - [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen: Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

- Zusätzliche Geschäftskontinuitätsfeatures wie automatische Failovergruppen für verschiedene geografische Azure-Standorte können wie folgt konfiguriert werden: [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md)

## <a name="next-steps"></a>Nächste Schritte

- Siehe [Übersicht über die Sicherheitsfunktionen von Azure SQL-Datenbank](sql-database-security-overview.md)