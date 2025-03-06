---
title: Ändra språkområdet för användargränssnittet i AEM Forms arbetsyta
description: Så här ändrar du arbetsytan i AEM Forms för att lokalisera text, komprimerade kategorier, köer och processer samt datumväljaren i gränssnittet.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 3f919e4d-0535-4816-8762-9c0088e47a2c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# Ändra språkområdet för användargränssnittet i AEM Forms arbetsyta{#changing-the-locale-of-aem-forms-workspace-user-interface}

AEM Forms arbetsyta har stöd för engelska, franska, tyska och japanska. Det gör det även möjligt att lokalisera användargränssnittet för AEM Forms-arbetsytan till andra språk.

Så här lokaliserar du användargränssnittet för AEM Forms-arbetsytan till det språk du vill:

* Lokalisera texten på arbetsytan i AEM Forms.
* Lokalisera dolda kategorier, köer och processer.
* Lokalisera datumväljare

Innan du utför stegen ovan måste du följa stegen som beskrivs i [Allmänna steg för anpassning av AEM Forms-arbetsytan](../../forms/using/generic-steps-html-workspace-customization.md).

>[!NOTE]
>
>Information om hur du ändrar språk för inloggningsskärmen på arbetsytan i AEM Forms finns i [Skapa en inloggningsskärm](../../forms/using/creating-new-login-screen.md).

## Lokalisera text {#localizing-text}

Utför följande steg så att du kan lägga till stöd för språket *New* och webbläsarens språkkod *nw*.

1. Logga in på CRXDE Lite.
CRXDE Lite standardwebbadress är `https://'[server]:[port]'/lc/crx/de/index.jsp`.
1. Navigera till platsen `apps/ws/locales` och skapa en mapp `nw.`
1. Kopiera filen `translation.json` från platsen `/apps/ws/locales/en-US` till platsen `/apps/ws/locales/nw`.
1. Navigera till `/apps/ws/locales/nw` och öppna `translation.json` för redigering. Gör språkområdesspecifika ändringar i filen translation.json.

   Följande exempel innehåller filen translation.json för engelska och franska i arbetsytan AEM Forms.

   ![translation_json_in_en](assets/translation_json_in_en.png) ![translation_json_in_fr](assets/translation_json_in_fr.png)

## Lokalisera komprimerade kategorier, köer och processer {#localizing-collapsed-categories-queues-and-processes}

På arbetsytan i AEM Forms används bilder för att visa rubriker för kategorier, köer och processer. Du behöver ett utvecklingspaket för att lokalisera dessa rubriker. Mer information om hur du skapar ett utvecklingspaket finns i [Skapa AEM Forms-arbetsytekod.](introduction-customizing-html-workspace.md#building-html-workspace-code)

I följande steg antas de nya lokaliserade bildfilerna vara *Categories_nw.png*, *Queue_nw.png* och *Processes_nw.png*. Bildernas rekommenderade bredd bör vara 19 pixlar.

>[!NOTE]
>
>För att hitta språkkoden för webbläsaren. Öppna `https://'[server]:[port]'/lc/libs/ws/Locale.html`.

![collapsing_panels_image](assets/collapsing_panels_image.png)

Så här översätter du bilderna:

1. Placera bildfilerna i mappen */apps/ws/images* med en WebDAV-klient.
1. Navigera till */apps/ws/css*. Öppna *newStyle.css* för redigering och lägg till följande poster:

   ```css
   #categoryListBar .content.nw {
        background: #3e3e3e url(../images/Categories_nw.png) no-repeat 10px 10px;
    }
   
   #filterListBar .content.nw {
       background: #3e3e3e url(../images/Queues_nw.png) no-repeat 10px 10px;
   }
   
   #processNameListBar .content.nw {
       background: #3e3e3e url(../images/Processes_nw.png) no-repeat 10px 10px;
   }
   ```

1. Utför alla semantiska ändringar som anges i artikeln [Workspace Customization](../../forms/using/introduction-customizing-html-workspace.md).
1. Navigera till mappen *js/runtime/utility* och öppna filen *usersession.js* för redigering.
1. Leta reda på koden som visas i det ursprungliga kodblocket och lägg till villkoret *lang !==&#39;nw&#39;* to the if statement:

   ```javascript
   // Orignal code
   setLocale = function () {
           var lang = $.trim(i18n.lng());
           if (lang === null || lang === '' || (lang !== 'fr-FR' && lang !== 'de-DE' && lang !== 'ja-JP')) {
               window.lcWorkspace.locale = 'en-US';
           } else {
               window.lcWorkspace.locale = lang;
           }
       }
   ```

   ```javascript
   //new code
    setLocale = function () {
           var lang = $.trim(i18n.lng());
           if (lang === null || lang === '' || (lang !== 'fr-FR' && lang !== 'de-DE' && lang !== 'ja-JP' && lang !== 'nw')) {
               window.lcWorkspace.locale = 'en-US';
           } else {
               window.lcWorkspace.locale = lang;
           }
       }
   ```

## Lokaliserar datumväljaren {#localizing-date-picker}

Du behöver ett utvecklingspaket för att lokalisera API:t *datepicker*. Mer information om hur du skapar ett utvecklingspaket finns i [Skapa AEM Forms-arbetsytekod](introduction-customizing-html-workspace.md#building-html-workspace-code).

1. Hämta och extrahera [jQuery-gränssnittspaketet](https://jqueryui.com/download/all/), navigera till *&lt;gränssnittspaket för extraherad jquery>*\jquery-ui-1.10.2.zip\jquery-ui-1.10.2\ui\i18n.
1. Kopiera filen jquery.ui.datepicker-nw.js för språkkod nu till apps/ws/js/libs/jqueryui och gör språkspecifika ändringar i filen.
1. Navigera till `apps/ws/js` och öppna filen `jquery.ui.datepicker-nw.js` för redigering.
1. Skapa ett alias för `jquery.ui.datepicker-nw.js.` I filen main.js. Koden som skapar ett alias för filen `jquery.ui.datepicker-nw.js` är:

   ```javascript
   jqueryuidatepickernw : pathprefix + 'libs/jqueryui/jquery.ui.datepicker-nw'
   ```

1. Använd aliaset `jqueryuidatepickernw` om du vill inkludera filen `jquery.ui.datepicker-nw.js` i alla filer som använder datumväljaren. Datepicker används i följande filer:

   * `js/runtime/views/outofoffice.js`
   * `js/runtime/views/searchtemplatedetails.js`

   I exempelkoden nedan visas hur du lägger till posten för jquery.ui.datepicker-nw.js:

   ```json
   //Original Code
   define([
       'jquery',
       'underscore',
       'backbone',
       'jqueryui',
       'jqueryuidatepickerja',
       'jqueryuidatepickerde',
       'jqueryuidatepickerfr',
       'slimscroll',
       'usersearchview',
       'logmanagerutil',
       'loggerutil'
   ], function ($, _, Backbone, jQueryUI, jQueryUIDatePickerJA, jQueryUIDatePickerDE, jQueryUIDatePickerFR, slimScroll, UserSearch, LogManager, Logger) {
   ```

   ```json
   // Code with Date Picker alias for new language
   define([
       'jquery',
       'underscore',
       'backbone',
       'jqueryui',
       'jqueryuidatepickerja',
       'jqueryuidatepickerde',
       'jqueryuidatepickerfr',
       'jqueryuidatepickernw', // Date Picker alias
       'slimscroll',
       'usersearchview',
       'logmanagerutil',
       'loggerutil'
   ], function ($, _, Backbone, jQueryUI, jQueryUIDatePickerJA, jQueryUIDatePickerDE, jQueryUIDatePickerFR, jQueryUIDatePickerNW, slimScroll, UserSearch, LogManager, Logger) {
   ```

1. I alla filer som använder API:t för datumväljaren ändrar du standardinställningarna för API:t för datumväljaren. API:t för datumväljaren används i följande filer:

   * apps\ws\js\runtime\views\searchtemplatedetails.js
   * apps\ws\js\runtime\views\outofoffice.js

   Ändra följande kod för att lägga till den nya språkinställningen:

   ```javascript
   if (locale === 'ja-JP') {
      $.datepicker.setDefaults($.datepicker.regional.ja);
   } else if (locale === 'de-DE') {
      $.datepicker.setDefaults($.datepicker.regional.de);
   } else if (locale === 'fr-FR') {
      $.datepicker.setDefaults($.datepicker.regional.fr);
   } else {
      $.datepicker.setDefaults($.datepicker.regional['']);
   }
   ```

   ```javascript
   if (locale === 'ja-JP') {
       $.datepicker.setDefaults($.datepicker.regional.ja);
   } else if (locale === 'de-DE') {
       $.datepicker.setDefaults($.datepicker.regional.de);
   } else if (locale === 'fr-FR') {
       $.datepicker.setDefaults($.datepicker.regional.fr);
   } else if (locale === 'nw') {
       $.datepicker.setDefaults($.datepicker.regional.nw);
   } else {
       $.datepicker.setDefaults($.datepicker.regional['']);
   }
   ```
