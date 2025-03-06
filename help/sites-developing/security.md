---
title: Dokumentskydd
description: Programsäkerhet startar under utvecklingsfasen
solution: Experience Manager, Experience Manager Sites
feature: Developing,Security
role: Developer
exl-id: abc2747f-cfd8-4ee1-bbc0-5ad89beb383a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# Dokumentskydd{#security}

Programsäkerhet startar under utvecklingsfasen. Adobe rekommenderar att du tillämpar följande bästa säkerhetspraxis.

## Använd begärandesession {#use-request-session}

I enlighet med principen om lägsta behörighet rekommenderar Adobe att all databasåtkomst görs genom att använda den session som är bunden till användarens begäran och rätt åtkomstkontroll.

## Skydda mot XSS (Cross-Site Scripting) {#protect-against-cross-site-scripting-xss}

Med XSS (Cross-site scripting) kan angripare lägga in kod på webbsidor som visas av andra användare. Säkerhetsluckan kan utnyttjas av skadliga webbanvändare för att kringgå åtkomstkontroller.

AEM tillämpar principen att filtrera allt användardefinierat innehåll vid utskrift. Förhindrande av XSS har högsta prioritet under både utveckling och testning.

XSS-skyddsmekanismen som tillhandahålls av AEM baseras på [AntiSamy Java™ Library](https://wiki.owasp.org/index.php/Category:OWASP_AntiSamy_Project) som tillhandahålls av [OWASP (Open Web Application Security Project)](https://owasp.org/). Standardkonfigurationen för AntiSamy finns på

`/libs/cq/xssprotection/config.xml`

Det är viktigt att du anpassar den här konfigurationen efter dina egna säkerhetsbehov genom att täcka över konfigurationsfilen. Den officiella [dokumentationen för AntiSamy](https://wiki.owasp.org/index.php/Category:OWASP_AntiSamy_Project) ger dig all information du behöver för att implementera säkerhetskraven.

>[!NOTE]
>
>Adobe rekommenderar att du alltid får tillgång till XSS-skydds-API:t med hjälp av [XSSAPI:n från AEM](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/xss/XSSAPI.html).

En brandvägg för ett webbprogram, till exempel [mod_security för Apache](https://www.modsecurity.org), kan dessutom ge tillförlitlig, central kontroll över distributionsmiljöns säkerhet och skydda mot tidigare oidentifierade serveröverskridande skriptattacker (cross-site scripting).

## Tillgång till Cloud Service-information {#access-to-cloud-service-information}

>[!NOTE]
>
>Åtkomstkontrollistorna för Cloud Service Information och de OSGi-inställningar som krävs för att skydda din instans automatiseras som en del av [Produktionsklart läge](/help/sites-administering/production-ready.md). Detta innebär att du inte behöver ändra konfigurationen manuellt, men vi rekommenderar ändå att du granskar dem innan du publicerar distributionen.

När du [integrerar din AEM-instans med Adobe Experience Cloud](/help/sites-administering/marketing-cloud.md) använder du [Cloud Service-konfigurationer](/help/sites-developing/extending-cloud-config.md). Information om dessa konfigurationer, tillsammans med all statistik som samlas in, lagras i databasen. Adobe rekommenderar att du, om du använder den här funktionen, kontrollerar om standardsäkerheten för den här informationen matchar dina krav.

Webbtjänsternas supportmodul skriver statistik och konfigurationsinformation under:

`/etc/cloudservices`

Med standardbehörigheter:

* Författarmiljö: `read` för `contributors`

* Publiceringsmiljö: `read` för `everyone`

## Skydda dig mot attacker med förfalskade begäranden på olika webbplatser {#protect-against-cross-site-request-forgery-attacks}

Mer information om säkerhetsmekanismerna som AEM använder för att mildra CSRF-attacker finns i avsnittet [Sling Referrer Filter](/help/sites-administering/security-checklist.md#protect-against-cross-site-request-forgery) i säkerhetschecklistan och i [dokumentationen för CSRF Protection Framework](/help/sites-developing/csrf-protection.md).
