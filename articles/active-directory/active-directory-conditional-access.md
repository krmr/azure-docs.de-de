---
title: Bedingter Zugriff mit Azure Active Directory | Microsoft Docs
description: Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: markusvi
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/21/2016
ms.author: markvi

---
# Bedingter Zugriff mit Azure Active Directory
In jedem Unternehmen ist das Sichern des Zugriffs auf Unternehmensressourcen von größter Bedeutung. Mit der Einführung von Clouddiensten und mobilen Geräten hat sich die Art des Zugriffs der Benutzer auf Unternehmensressourcen maßgeblich geändert. Die Verbreitung persönlicher und unternehmenseigener Geräte erfordert einen neuen Ansatz für den Zugriff auf Unternehmensressourcen und die Sicherheit.

## Gründe für den bedingten Zugriff
Die Steuerungsfunktionen für den bedingten Zugriff in Azure Active Directory bieten einfache Methoden, mit denen Unternehmen Ressourcen in der Cloud und lokal schützen können. Richtlinien für bedingten Zugriff können mit der mehrstufigen Authentifizierung Schutz davor bieten, dass Anmeldeinformationen gestohlen oder ausgespäht werden. Sie können Richtlinien für bedingten Zugriff auch dazu nutzen, Unternehmensdaten zu schützen. Beispielsweise können Sie nur den Geräten Zugriff auf sensible Dienste gewähren, die bei einem System für die mobile Geräteverwaltung wie Microsoft Intune registriert sind.

## Voraussetzungen
Der bedingte Zugriff in Azure Active Directory ist eine Funktion von [Azure AD Premium](http://www.microsoft.com/identity). Alle Benutzer, die beim Zugreifen auf eine Anwendung eine Richtlinie für bedingten Zugriff verwenden, müssen über eine Azure AD Premium-Lizenz verfügen. Weitere Informationen zur Nutzung finden Sie im [Unlicensed User report](https://aka.ms/utc5ix) (Bericht zu nicht lizenzierten Benutzern).

## Wie wird die bedingte Zugriffskontrolle durchgesetzt?
Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory beim Authentifizieren des Benutzers die von Ihnen ausgewählten besonderen Bedingungen, bevor der Zugriff auf eine Anwendung gewährt wird. Sobald diese Zugriffsanforderungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.

![](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## Bedingungen
* **Gruppenmitgliedschaft**: Steuern Sie die Zugriffsebene für einen Benutzer basierend auf seiner Mitgliedschaft in einer Gruppe.
* **Standort**: Verwenden Sie den Standort des Benutzers, um eine mehrstufige Authentifizierung auszulösen und Steuerelemente zu blockieren, wenn ein Benutzer sich nicht in einem vertrauenswürdigen Netzwerk befindet.
* **Geräteplattform**: Verwenden Sie den Typ der Geräteplattform, z.B. iOS, Android, Windows Mobile und Windows, als Bedingung für die Anwendung der Richtlinie.
* **Gerät aktiviert**: Der Status „Gerät aktiviert/Gerät deaktiviert“ wird während der Auswertung der Geräterichtlinie überprüft. Wenn ein verlorenes oder gestohlenes Gerät im Verzeichnis deaktiviert wird, kann es nicht mehr verwendet werden, um Richtlinienanforderungen zu erfüllen.
* **Anmeldungs- und Benutzerrisiko**: Richtlinien zu Risiken beim bedingten Zugriff stehen mit [Azure AD Identity Protection](active-directory-identityprotection.md) zur Verfügung und bieten erweiterten Schutz basierend auf Risikoereignissen und ungewöhnlichen Anmeldeaktivitäten.

## Steuerelemente
* **Multi Factor Authentication (MFA)**: Sie können per MFA eine sichere Authentifizierung erzwingen. MFA kann durch Azure MFA oder einen lokalen MFA-Anbieter über die Active Directory-Verbunddienste (AD FS) bereitgestellt werden. MFA unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der Zugriff auf die Anmeldeinformationen eines gültigen Benutzers erlangt hat.
* **Blockieren**: Bedingungen wie z.B. der Benutzerstandort können zum Blockieren des Benutzerzugriffs herangezogen werden. Beispielsweise kann der Zugriff blockiert werden, wenn sich ein Benutzer nicht in einem vertrauenswürdigen Netzwerk befindet.
* **Kompatible Geräte**: Auf Geräteebene können Sie Richtlinien festlegen, die erzwingen, dass nur Computern der Zugriff gewährt wird, die der Domäne beigetreten sind oder – im Fall von Mobilgeräten – bei einer Anwendung zur Verwaltung mobiler Geräte (Mobile Device Management [MDM]) registriert sind und die Kompatibilitätsvoraussetzungen erfüllen. Beispielsweise kann Microsoft Intune die Kompatibilität von Geräten überprüfen und einen Bericht an Azure Active Directory senden, um die Gerätekompatibilität für den Anwendungszugriff zu erzwingen. Ausführliche Anleitungen dazu, wie Microsoft Intune zum Schutz von Apps und Daten eingesetzt werden kann, finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Sie können über Microsoft Intune auch Datenschutz für verlorene oder gestohlene Geräte erzwingen. Weitere Informationen finden Sie unter [Schützen Ihrer Daten mit vollständigem oder selektivem Löschen über Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## Anwendungen
* Die Zugriffsebene, die Sie mit diesen Richtlinien festlegen können, kann auf Anwendungen und Dienste in der Cloud oder lokal angewendet werden. Die Richtlinie wird direkt auf die Website oder den Dienst angewendet. Die Richtlinie wird dann für den Browserzugriff und für Anwendungen durchgesetzt, die auf den Dienst zugreifen. Die Liste mit den Diensten, die angewendet werden können, ist hier angegeben.

## Gerätebasierter bedingter Zugriff
Sie können den Zugriff auf Anwendungen auch auf Geräte beschränken, die bei Azure AD registriert sind und bestimmte Bedingungen erfüllen. Dies ist hilfreich, um Organisationsressourcen vor dem Zugriff durch gültige Benutzer über folgende Geräte zu schützen:

* Unbekannte/nicht verwaltete Geräte
* Geräte, die die von Ihrer Organisation definierten Sicherheitsrichtlinien nicht erfüllen

Richtlinien können basierend auf den folgenden Anforderungen festgelegt werden:

* **In die Domäne eingebundene Geräte**: Sie können eine Richtlinie festlegen, um den Zugriff auf Geräte zu beschränken, die einer lokalen Active Directory-Domäne beigetreten und außerdem bei Azure AD registriert sind. Diese Richtlinie gilt für Desktops, Laptops oder Unternehmens-Tablets unter Windows, die zu einer lokalen Active Directory-Domäne gehören und bei Azure AD registriert sind. Weitere Informationen zum Einrichten der automatischen Registrierung von in die Domäne eingebundenen Geräten bei Azure AD finden Sie unter [Einrichten der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten bei Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Kompatible Geräte**: Sie können eine Richtlinie festlegen, um den Zugriff auf Geräte zu beschränken, die vom Verwaltungssystem im Verzeichnis als **kompatibel** gekennzeichnet wurden. Diese Richtlinie stellt sicher, dass nur Geräten der Zugriff gewährt wird, die Sicherheitsrichtlinien erfüllen, wie z.B. das Erzwingen der Dateiverschlüsselung auf einem Gerät. Diese Richtlinie kann verwendet werden, um den Zugriff über folgende Geräte zu beschränken:
  
  * **In die Windows-Domäne eingebundene Geräte**, die von System Center Configuration Manager (Current Branch) in einer hybriden Konfiguration verwaltet werden.
  * **Mobile Arbeits- oder persönliche Geräte unter Windows 10**, die über Microsoft Intune oder ein unterstütztes MDM-Drittanbietersystem verwaltet werden.
  * **iOS- und Android-Geräte**, die über Microsoft Intune verwaltet werden.

Wenn Benutzer auf eine durch die gerätebasierte Richtlinie zum bedingten Zugriff geschützte Anwendung zugreifen möchten, müssen sie dies über ein Gerät tun, das diese Richtlinie erfüllt. Wird ein Gerät verwendet, das die Richtlinie nicht erfüllt, wird der Zugriff verweigert.

Informationen zum Konfigurieren von gerätebasierten Richtlinien für bedingten Zugriff in Azure AD finden Sie unter [Konfigurieren von gerätebasierten Richtlinien für bedingten Zugriff zur Steuerung des Zugriffs auf über Azure Active Directory verbundene Anwendungen](active-directory-conditional-access-policy-connected-applications.md).

## Verzeichnis der Artikel zum bedingten Zugriff in Azure Active Directory
In der folgenden Inhaltszuordnung werden Dokumente aufgeführt, in denen Sie weitere Informationen über das Aktivieren des bedingten Zugriffs in Ihrer aktuellen Bereitstellung finden.

### MFA und Standortrichtlinien
* [Erste Schritte mit bedingtem Zugriff auf verbundene Azure AD-Apps basierend auf Gruppen-, Standort- und MFA-Richtlinien](active-directory-conditional-access-azuread-connected-apps.md)
* [Unterstützte Anwendungsarten](active-directory-conditional-access-supported-apps.md)

### Gerätebasierter bedingter Zugriff
* [Festlegen von gerätebasierten Richtlinien für bedingten Zugriff zur Steuerung des Zugriffs auf über Azure Active Directory verbundene Anwendungen](active-directory-conditional-access-policy-connected-applications.md)
* [Einrichten der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten bei Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Benutzerkorrektur beim Zugreifen auf Anwendungen mit gerätebasiertem bedingtem Azure AD-Zugriffsschutz](active-directory-conditional-access-device-remediation.md)
* [Schützen von Daten auf verlorenen oder gestohlenen Geräten mit Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### Schützen von Ressource basierend auf dem Anmelderisiko
[Azure AD Identity Protection](active-directory-identityprotection.md)

### Zusätzliche Informationen
* [Häufig gestellte Fragen zum bedingten Zugriff](active-directory-conditional-faqs.md)
* [Technische Referenz](active-directory-conditional-access-technical-reference.md)

<!---HONumber=AcomDC_0928_2016-->