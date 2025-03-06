---
title: Konfigurera Ångra för sidredigering
description: Lär dig hur du konfigurerar Ångra-stöd för sidredigering i AEM.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Configuring
role: Admin
exl-id: efeda84f-e04f-4cbd-898c-4754dc29e008
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Konfigurera Ångra för sidredigering{#configuring-undo-for-page-editing}

[OSGi-tjänsten](/help/sites-deploying/configuring-osgi.md) **Day CQ WCM Undo Configuration** ( `com.day.cq.wcm.undo.UndoConfigService`) visar flera egenskaper som styr beteendet för kommandona Ångra och Gör om för redigering av sidor.

## Standardkonfiguration {#default-configuration}

I en standardinstallation definieras standardinställningarna som egenskaper på noden `sling:OsgiConfig`:

`/libs/wcm/core/config.author/com.day.cq.wcm.undo.UndoConfig`

Den här noden innehåller `cq.wcm.undo.whitelist`- och `cq.wcm.undo.blacklist`-egenskaper. För de andra egenskaperna används standardvärdena.

>[!CAUTION]
>
>Du ***får*** inte ändra något i sökvägen `/libs`.
>
>Detta beror på att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan mycket väl skrivas över när du använder en snabbkorrigering eller ett funktionspaket).

## Konfigurera Ångra och Gör om {#configuring-undo-and-redo}

Du kan konfigurera dessa OSGi-tjänstegenskaper för din egen instans.

>[!NOTE]
>
>När du arbetar med AEM finns det flera metoder för att hantera konfigurationsinställningarna för sådana tjänster. Mer information och rekommenderade tillvägagångssätt finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

I följande lista visas egenskaperna så som de visas i webbkonsolen, följt av namnet på motsvarande OSGi-parameter, tillsammans med en beskrivning och standardvärdet (där det är lämpligt):

* **Aktivera**
( `cq.wcm.undo.enabled`)

   * **Beskrivning**: Avgör om sidförfattare kan ångra och göra om ändringar.
   * **Standard**: `Selected`
   * **Typ**: `Boolean`

* **Sökväg**
( `cq.wcm.undo.path`)

   * **Beskrivning**: Databassökvägen för beständiga binära ångra-data. När författare ändrar binära data, t.ex. bilder, behålls den ursprungliga versionen av dessa data här. När ändringar av binära data ångras återställs dessa binära ångra-data till sidan.
   * **Standard**: `/var/undo`
   * **Typ**: `String`

  >[!NOTE]
  >
  >Som standard kan bara administratörer komma åt noden `/var/undo`. Författare kan bara ångra och göra om åtgärder för binärt innehåll efter att de har fått behörighet att komma åt binära ångra-data.

* **Min. giltighet**
( `cq.wcm.undo.validity`)

   * **Beskrivning**: Den kortaste tiden som binära ångra-data lagras i timmar. Efter den här tidsperioden är binära data tillgängliga för borttagning för att spara diskutrymme.
   * **Standard**: `10`
   * **Typ**: `Integer`

* **Steg**
( `cq.wcm.undo.steps`)

   * **Beskrivning**: Det maximala antalet sidåtgärder som lagras i ångra-historiken.
   * **Standard**: `20`
   * **Typ**: `Integer`

* **Persistence**
( `cq.wcm.undo.persistence`)

   * **Beskrivning**: Den klass som består av ångra-historik. Två beständiga klasser finns:

      * `CQ.undo.persistence.WindowNamePersistence`: Visar historik med egenskapen window.name.
      * `CQ.undo.persistence.CookiePersistance`: Alltid historik med cookies.

   * **Standard**: `CQ.undo.persistence.WindowNamePersistence`
   * **Typ**: `String`

* **Persistensläge**
( `cq.wcm.undo.persistence.mode`)

   * **Beskrivning**: Avgör när ångringshistorik sparas. Välj det här alternativet om du vill behålla ångra-historiken efter varje sidredigering. Avmarkera det här alternativet om du bara vill behålla när en sidinläsning sker (till exempel navigerar användaren till en annan sida).

     Om du vill ångra-historiken används webbläsarresurserna. Om användarens webbläsare svarar långsamt på sidredigeringar kan du försöka med att behålla ångra-historiken när sidan läses in igen.

   * **Standard**: `Selected`
   * **Typ**: `Boolean`

* **Markörläge**
( `cq.wcm.undo.markermode`)

   * **Beskrivning**: Anger den visuella referenspunkt som ska användas för att ange vilka stycken som ska påverkas när en Ångra eller Gör om inträffar. Följande värden är giltiga:

      * flash: Markeringsindikatorn för styckena blinkar tillfälligt.
      * markera: Stycket är markerat.

   * **Standard**: `flash`
   * **Typ**: `String`

* **Bra komponenter**
( `cq.wcm.undo.whitelist`)

   * **Beskrivning**: En lista över komponenter som du vill ska påverkas av kommandona Ångra och Gör om. Lägg till komponentsökvägar i den här listan när de fungerar korrekt med ångra/gör om. Lägg till en asterisk (&amp;ast;) för att ange en grupp komponenter:

      * Följande värde anger den grundläggande textkomponenten:

        `foundation/components/text`

      * Följande värde anger alla grundkomponenter:

        `foundation/components/*`

   * När Ångra eller Gör om utfärdas till en komponent som inte finns i den här listan visas ett meddelande om att kommandot kan vara otillförlitligt.

   * **Standard**: Egenskapen är ifylld med många komponenter som tillhandahålls av AEM.
   * **Typ**: `String[]`

* **Felaktiga komponenter**
( `cq.wcm.undo.blacklist`)

   * **Beskrivning**: En lista över komponenter och/eller komponentåtgärder som du inte vill ska påverkas av kommandot Ångra. Lägg till komponenter och komponentåtgärder som inte fungerar som de ska med kommandot Ångra:

      * Lägg till en komponentsökväg när du inte vill ha någon av komponentens åtgärder i ångra-historiken, till exempel `collab/forum/components/post`
      * Lägg till ett kolon (:) och en åtgärd i sökvägen när du vill att den specifika åtgärden ska utelämnas från ångra-historiken (andra åtgärder fungerar korrekt), till exempel `collab/forum/components/post:insertParagraph.`

  >[!NOTE]
  >
  >När en åtgärd finns i listan läggs den fortfarande till i ångra-historiken. Användare kan inte ångra åtgärder som finns tidigare än en **Dålig komponent**-åtgärd i ångra-historiken.

   * Vanliga åtgärdsnamn är:

      * `insertParagraph`: Komponenten läggs till på sidan.
      * `removeParagraph`: Komponenten har tagits bort.
      * `moveParagraph`: Stycket flyttas till en annan plats.
      * `updateParagraph`: Styckeegenskaperna har ändrats.

   * **Standard**: Egenskapen har fyllts i med flera komponentåtgärder.
   * **Typ**: `String[]`
