---
title: Migrering till Touch UI
description: Läs om hur Adobe Experience Manager migrerar till Touch-gränssnittet och hur det påverkar dig.
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: introduction
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: e12f12862c31cef81b2808897fab5cf8e19dfa86
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 0%

---

# Migrering till Touch UI{#migration-to-the-touch-ui}

Från och med version 6.0 har Adobe Experience Manager (AEM) introducerat ett nytt användargränssnitt som kallas *användargränssnittet med pekskärm* (kallas även *användargränssnittet*). Den är anpassad efter Adobe Experience Cloud och Adobe riktlinjer för användargränssnitt. Det här har blivit standardgränssnittet i AEM med det äldre skrivbordsorienterade gränssnittet som kallas *klassiskt användargränssnitt*.

Om du har använt AEM med ett klassiskt användargränssnitt ska du vidta åtgärder för att migrera instansen. Den här sidan är avsedd att fungera som en språngbräda genom länkar till enskilda resurser.

>[!NOTE]
>
>Ett sådant migreringsprojekt kan få stor effekt på din instans. Se [Hantera projekt - Bästa metoder](/help/managing/best-practices.md) för rekommenderade riktlinjer.

## Grunderna {#the-basics}

Tänk på följande stora skillnader mellan det klassiska gränssnittet och pekgränssnittet när du migrerar:

<table>
 <tbody>
  <tr>
   <td>Klassiskt användargränssnitt</td>
   <td>Pekaktiverat användargränssnitt</td>
  </tr>
  <tr>
   <td>Beskrivs i JCR-databasen som en nodstruktur. Alla noder som representerar ett element i användargränssnittet kallas <em>ExtJS-widget</em> och återges på klientsidan av <code>ExtJS</code>.</td>
   <td>Beskrivs också i JCR-databasen som en nodstruktur. I det här fallet refererar dock alla noder till en Sling-resurstyp (Sling-komponent) som ansvarar för återgivningen. Gränssnittet renderas alltså (i stort) på serversidan.</td>
  </tr>
  <tr>
   <td><p><code>sling:resourceType</code></p>
    <ul>
     <li>används inte</li>
    </ul> </td>
   <td><code>sling:resourceType</code>
    <ul>
     <li>används</li>
     <li>till exempel <br /> <code>cq/gui/components/authoring/dialog</code><br /> </li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>Dialognoder:</p>
    <ul>
     <li>Namn: <code>dialog</code></li>
     <li><code>jcr:primaryType</code>: <code>cq:Dialog</code></li>
    </ul> </td>
   <td><p>Dialognoder:</p>
    <ul>
     <li>Namn: <code>cq:dialog</code></li>
     <li><code>jcr:primaryType</code>: <code>nt:unstructured</code></li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>JavaScript:</p>
    <ul>
     <li>Imperativa delar bäddas in direkt med avlyssnare eller hanteras i klientlibs.</li>
    </ul> </td>
   <td><p>JavaScript:</p>
    <ul>
     <li>Imperativa delar kan inte bäddas in i dialogdefinition; ansvarsfördelning.</li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>Händelsehantering:</p>
    <ul>
     <li>Dialogrutewidgetar refererar direkt till JavaScript-kod.</li>
    </ul> </td>
   <td><p>Händelsehantering:</p>
    <ul>
     <li>JavaScript observerar dialoghändelser.</li>
    </ul> </td>
  </tr>
  <tr>
   <td>Återgivning utförd av klienten:
    <ul>
     <li>Klienten skapar gränssnittskomponenterna dynamiskt.</li>
     <li>Klientförfrågningar (Pull) komponentdefinition (som JSON) från servern.</li>
    </ul> </td>
   <td>Återgivning utförd av servern:
    <ul>
     <li>Klienten begär sidor tillsammans med det relaterade användargränssnittet.</li>
     <li>Servern skickar (push) gränssnittet som HTML-dokument. Använder Coral UI-komponenter.<br /> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

Med andra ord innebär migrering av ett avsnitt i användargränssnittet från det klassiska gränssnittet till pekgränssnittet att en *ExtJS-widget* porteras till en *Sling-komponent*. För att underlätta detta baseras pekgränssnittet på GRE-ramverket (Granite UI Framework), som redan innehåller vissa Sling-komponenter för användargränssnittet (kallas GRE UI-komponenter).

Kontrollera status och relaterade rekommendationer innan du börjar:

* [Rekommendationer för användargränssnitt för kunder](/help/sites-deploying/ui-recommendations.md)

Grundläggande information om hur du utvecklar användargränssnittet för pekskärmar är en stabil grund:

* [AEM Touch-aktiverade användargränssnitt](/help/sites-developing/touch-ui-concepts.md)
* [Struktur för det användargränssnitt som kan användas med AEM Touch](/help/sites-developing/touch-ui-structure.md)

## Migrerar sidredigering {#migrating-page-authoring}

Dialogrutor är en viktig faktor när du migrerar komponenter:

* [Utveckla AEM-komponenter](/help/sites-developing/developing-components.md) (med det beröringsaktiverade användargränssnittet)
* [Migrera från en klassisk komponent](/help/sites-developing/developing-components.md#migrating-from-a-classic-component)
* [AEM moderniseringsverktyg](/help/sites-developing/modernization-tools.md) - hjälper dig att konvertera dialogrutorna för dina klassiska användargränssnittskomponenter till användargränssnitt

   * Det finns ett kompatibilitetslager med pekfunktion för att öppna en klassisk användargränssnittsdialogruta i en&quot;Touch UI wrapper&quot;, men det har begränsad funktionalitet och rekommenderas inte på lång sikt.

* [Anpassa dialogrutefält i Touch-gränssnittet](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-customizing-dialog-fields-in-touch-ui.html)
* [Skapa en ny GRE-fältkomponent](/help/sites-developing/granite-ui-component.md)
* [Anpassa sidredigering](/help/sites-developing/customizing-page-authoring-touch.md) (med det beröringsaktiverade användargränssnittet)

## Migrera konsoler {#migrating-consoles}

Du kan också anpassa konsolerna:

* [Anpassa konsolerna](/help/sites-developing/customizing-consoles-touch.md) (för det beröringsaktiverade användargränssnittet)

## Relaterade överväganden {#related-considerations}

Även om det inte är direkt relaterat till en migrering till pekgränssnittet finns det relaterade problem som är värda att ta hänsyn till samtidigt, eftersom de också rekommenderas:

* [Mallar](/help/sites-developing/templates.md) - [Redigerbara mallar](/help/sites-developing/page-templates-editable.md)
* [Kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTML](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html)

>[!NOTE]
>
>Se även [Utveckla - Bästa metoder](/help/sites-developing/best-practices.md).

## Ytterligare resurser {#further-resources}

Mer information om hur du utvecklar AEM finns i resurssamlingen under:

* [Utveckla användarhandbok](/help/sites-developing/getting-started.md)
* [Bevilja gränssnittsdokumentation](https://developer.adobe.com/experience-manager/reference-materials/6-5/granite-ui/api/jcr_root/libs/granite/ui/index.html)
* [Självstudiekurser och videofilmer för AEM 6.5-webbplatser](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html)
* [Getting Started Developing AEM Sites - WKND Tutorial](/help/sites-developing/getting-started.md)
* [AEM Gems](https://experienceleague.adobe.com/docs/events/experience-manager-gems-recordings/overview.html)
* [AEM Modernization Tools](https://opensource.adobe.com/aem-modernize-tools/)

>[!CAUTION]
>
>AEM Moderniseringsverktyg är en del av vår community och stöds inte eller garanteras inte av Adobe.
