---
title: Dataskydd och dataintegritet - Adobe Experience Manager beredskap
description: Läs mer om Adobe Experience Manager stöd för de olika dataskydds- och datasekretessreglerna. Den innehåller EU:s allmänna dataskyddsförordning (GDPR), Kaliforniens konsumentintegritetslag (Consumer Privacy Act) och hur man följer den när man genomför ett nytt AEM-projekt.
solution: Experience Manager, Experience Manager 6.5 LTS
feature: Compliance
role: Developer,Leader,Architect,Data Architect,User
exl-id: 6faf8e4f-ca2a-4d68-a354-fb0aa6c2644b
source-git-commit: 4a93e17da1157253a681bf8b3a38252962d8fb59
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# Adobe Experience Manager beredskap för dataskydd och dataintegritet {#aem-readiness-for-data-protection-and-data-privacy-regulations}

>[!WARNING]
>
>Innehållet i detta dokument utgör inte juridisk rådgivning och är inte avsett som ersättning för juridisk rådgivning.
>
>Kontakta företagets juridiska avdelning för råd om dataskydd och dataintegritet.

>[!NOTE]
>
>Mer information om Adobe svar på sekretessfrågor och vad det innebär för dig som Adobe-kund finns i [Adobe Sekretesscenter](https://www.adobe.com/privacy.html).

Adobe tillhandahåller dokumentation och procedurer (med API:er när sådana finns) som kundsekretessadministratören eller AEM-administratören kan använda för att hantera förfrågningar om dataskydd och datasekretess. Det kan hjälpa er att följa dessa regler. De dokumenterade procedurerna gör att kunderna kan köra förfrågningar manuellt eller genom att anropa API:er, om sådana finns, från en extern portal eller tjänst.

>[!CAUTION]
>
>Informationen som beskrivs här är begränsad till Adobe Experience Manager.
>
>Data från en annan Adobe On-demand-tjänst, tillsammans med eventuella relaterade sekretessförfrågningar, kräver att åtgärder vidtas för den tjänsten.
>
>Mer information finns i [Adobe Sekretesscenter](https://www.adobe.com/privacy.html).

## Introduktion {#introduction}

Instanser av Adobe Experience Manager, och de program som körs på dem, ägs och hanteras av Adobe kunder.

Följaktligen är dataskyddsbestämmelser, som GDPR, CCPA med flera, i huvudsak kundernas ansvar.

Som en kort introduktion innehåller reglerna för datasekretess och skydd nya regler som ska följas av rollerna för:

* Affärsenheter (CCPA) och/eller datakontroller (GDPR)

* Tjänsteleverantörer (CCPA) och/eller dataprocessorer (GDPR)

De viktigaste bestämmelserna i sådana förordningar är

1. En utökad definition av personuppgifter som inkluderar alla unika ID:n, som direkt och indirekt identifierbara data.

2. Förstärkt krav på samtycke.

3. Ökat fokus på raderingsrättigheter (dataradering).

4. Avanmäl dig från försäljning av data.

För Adobe Experience Manager:

* Förekomsterna, och tillämpningarna som körs på dem, ägs och hanteras av kunden.

   * Kunden hanterar regelrollerna, bland annat affärsenheter och tjänsteleverantör, Data Controller och Data Processor.

   * Adobe Experience Platform Privacy Service ingår inte i arbetsflödet för AEM, vilket visas i bilden nedan.

* AEM innehåller dokumentation och procedurer som kundens integritetsadministratör och/eller AEM-administratör kan använda för att utföra förfrågningar om sekretessregler, antingen manuellt eller via API:er, om sådana finns.

* Ingen ny tjänst eller gränssnitt har lagts till.

   * Istället dokumenteras procedurer och API:er för användning av kundgränssnitt/portaler som hanterar förfrågningar om sekretessbestämmelser.

* AEM innehåller inga färdiga verktyg som stöder arbetsflödet för sekretessförfrågningar.

   * Adobe tillhandahåller dokumentation och procedurer för kundens integritetsadministratör och AEM-administratör, så att de manuellt kan köra förfrågningar som rör sekretesslagstiftningen.

Adobe tillhandahåller procedurer för hantering av sekretessförfrågningar relaterade till åtkomst, borttagning och avanmälan för Adobe Experience Manager. Ibland finns det API:er som kan anropas från en kundutvecklad portal eller skript som kan hjälpa till med automatisering.

I följande diagram visas hur ett arbetsflöde för sekretesspolicy kan se ut:

![Dataskydd och sekretess](assets/data-protection-and-privacy-01.png)

## Adobe Experience Manager och regelberedskap {#aem-and-regulatory-readiness}

Se avsnitten nedan för dokumentation om AEM produkter.

## AEM Foundation {#aem-foundation}

Se [Hantera dataskydds- och sekretessförfrågningar för AEM Foundation](/help/sites-administering/handling-gdpr-requests-for-aem-platform.md).

## Samling av AEM-statistik om aggregerad användning {#aem-opting-into-aggregate-usage-statistics-collection}

Se [Samling med aggregerad användningsstatistik](/help/sites-deploying/opt-in-aggregated-usage-statistics.md).

## AEM Sites {#aem-sites}

Se [AEM Sites - Dataskydd och sekretesshantering.](/help/sites-administering/gdpr-compliance-sites.md)

## AEM-integrering med Adobe Target och Adobe Analytics {#aem-integration-with-adobe-target-adobe-analytics}

Dessa Adobe Experience Manager-integreringar är anpassade för dataskydd och sekretess (till exempel GDPR eller CCPA). Inga personuppgifter från Adobe Target eller Adobe Analytics lagras i AEM i samband med integreringarna.

Mer information finns i följande:

* [Adobe Target - Integritetsöversikt](https://developer.adobe.com/target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation/?lang=en)

* [Adobe Analytics arbetsflöde för datasekretess](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/an-gdpr-workflow.html)

## AEM Forms {#aem-forms}

AEM Forms innehåller komponenter och arbetsflöden som samlar in, bearbetar och lagrar data för att samordna affärsprocesser och slutföra digitala transaktioner. Olika komponenter använder olika datalager och tillåter även integrering med anpassade datalager. I följande dokumentation förklaras procedurer och riktlinjer för att komma åt och hantera användardata för att stödja arbetsflöden för dataskydd och sekretess (till exempel GDPR eller CCPA) för en komponent.

* [Forms Portal](/help/forms/using/forms-portal-handling-user-data.md)
* [Korrespondenshantering](/help/forms/using/correspondence-management-handling-user-data.md)
* [Integrering med Adobe Sign](/help/forms/using/integration-adobe-sign-handling-user-data.md)
* [Forms-centrerade arbetsflöden på OSGi](/help/forms/using/forms-workflow-osgi-handling-user-data.md)
* [Forms JEE-arbetsflöden](/help/forms/using/forms-workflow-jee-handling-user-data.md) (endast AEM Forms JEE)
* [Dokumentsäkerhet](/help/forms/using/document-security-handling-user-data.md) (endast AEM Forms JEE)
* [Användarhantering](/help/forms/using/user-management-handling-user-data.md) (endast AEM Forms JEE)
