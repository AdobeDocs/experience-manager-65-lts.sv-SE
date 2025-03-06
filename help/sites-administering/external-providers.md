---
title: Analyser med externa leverantörer
description: Lär dig hur du konfigurerar en egen instans av Generic Analytics-kodfragment för att definiera en ny tjänstkonfiguration.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: 73f40fd7-69b9-436c-b6b4-a7d6bfbaae6f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---

# Analyser med externa leverantörer {#analytics-with-external-providers}

Analyser kan ge dig viktig och intressant information om hur webbplatsen används.

Det finns olika färdiga konfigurationer för integrering med rätt tjänst, till exempel:

* [Adobe Analytics](/help/sites-administering/adobeanalytics.md)
* [Adobe Target](/help/sites-administering/target.md)

Du kan också konfigurera din egen instans av **Generic Analytics-kodfragment** för att definiera en ny tjänstkonfiguration.

Informationen samlas sedan in av små kodfragment som läggs till på webbsidorna. Till exempel:

>[!CAUTION]
>
>Omsluter inte skript i `script`-taggar.

```
var _gaq = _gaq || [];
_gaq.push(['_setAccount', 'UA-XXXXX-X']);
_gaq.push(['_trackPageview']);

(function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'https://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
})();
```

Sådana fragment gör det möjligt att samla in data och generera rapporter. De faktiska data som samlas in beror på providern och vilket kodfragment som används. Exempel på statistik är:

* hur många besökare som helst
* hur många sidor som besöktes
* söktermer som används
* landningssidor

>[!CAUTION]
>
>Demonsplatsen Geometrixx-Outdoor är konfigurerad så att attributen i Sidegenskaper läggs till i HTML-källkoden (precis ovanför `</html>`-sluttaggen) i motsvarande `js` -skript.
>
>Om din `/apps` inte ärver från standardsidkomponenten ( `/libs/foundation/components/page`) måste du (eller dina utvecklare) se till att motsvarande `js`-skript inkluderas, till exempel genom att ta med `cq/cloudserviceconfigs/components/servicescomponents` eller använda en liknande mekanism.
>
>Utan detta kommer ingen av tjänsterna (Generic, Analytics, Target och så vidare) att fungera.

## Skapa en tjänst med ett allmänt kodfragment {#creating-a-new-service-with-a-generic-snippet}

För den grundläggande konfigurationen:

1. Öppna konsolen **Verktyg**.
1. Expandera **Cloud Services Configurations** från den vänstra rutan.
1. Dubbelklicka på **Generic Analytics-kodfragment** för att öppna sidan:

   ![Kodavsnitt för allmän analys](assets/analytics_genericoverview.png)

1. Klicka på + för att lägga till en ny konfiguration med dialogrutan. Tilldela ett namn, till exempel Google Analytics:

   ![Skapa konfiguration](assets/analytics_addconfig.png)

1. Klicka på **Skapa**, dialogrutan för kodfragment öppnas omedelbart - klistra in lämpligt JavaScript-fragment i fältet:

   ![Redigera komponenten](assets/analytics_snippet.png)

1. Klicka på **OK** för att spara.

## Använda din nya tjänst på sidor {#using-your-new-service-on-pages}

När du har skapat tjänstkonfigurationen måste du konfigurera sidorna som krävs för att använda den:

1. Navigera till sidan.
1. Öppna **Sidegenskaperna** från sidosparken och sedan fliken **Cloud Services** .
1. Klicka på **Lägg till tjänst** och välj sedan önskad tjänst. Exempelvis **Kodavsnittet för allmän analys**:

   ![Lägger till en molntjänst](assets/analytics_selectservice.png)

1. Klicka på **OK** för att spara.
1. Du återgår till fliken **Molntjänster**. Utdraget **Generic Analytics** visas nu med meddelandet `Configuration reference missing`. Använd listrutan för att välja en specifik tjänstinstans. Till exempel Google-analytics:

   ![Lägger till molntjänstkonfiguration](assets/analytics_selectspecificservice.png)

1. Klicka på **OK** för att spara.

   Utdraget visas nu om du visar Page Source för sidan.

   När en viss tid har gått kan du visa den insamlade statistiken.

   >[!NOTE]
   >
   >Om konfigurationen är kopplad till en sida som har underordnade sidor, ärvs tjänsten även av dessa sidor.
