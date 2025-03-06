---
title: Anpassa felmeddelanden för HTML5-formulär
description: Lär dig hur du anpassar visningen av felmeddelanden för HTML5-formulär, inklusive hur du ändrar deras position och utseende.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: customization
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 424e7664-7d1c-4f47-83bd-0af3f40e7fa9
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---

# Anpassa felmeddelanden för HTML5-formulär {#customizing-error-messages-for-html-forms}

I HTML5-formulär visas felmeddelandena och varningarna med fast position och utseende (teckensnitt och färg), och bara ett fel visas.

I artikeln beskrivs hur du anpassar felmeddelanden för HTML5-formulär så att du kan göra följande:

* ändra utseendet och placeringen av felmeddelanden. Du kan göra så att ett fel visas högst upp, längst ned och till höger i ett fält.
* visa felmeddelanden för flera fält vid en given tidpunkt.
* visar felet oavsett om ett fält är markerat eller inte.

## Anpassa felmeddelanden  {#customizing-error-messages-nbsp}

Innan du anpassar felmeddelandena hämtar och extraherar du det bifogade paketet (CustomErrorManager-1.0-SNAPSHOT.zip).

När du har extraherat paketet öppnar du mappen CustomErrorManager-1.0-SNAPSHOT. Den innehåller mapparna jcr_root och META-INF. Dessa mappar innehåller de CSS- och JS-filer som behövs för att anpassa felmeddelandet.

[Hämta fil](assets/customerrormanager-1.0-snapshot.zip)

### Anpassa felmeddelandenas placering  {#customizing-the-position-of-error-messages-nbsp}

Om du vill anpassa positionen för felmeddelandet lägger du till en &lt;div>-tagg för varje fel- och varningsfält, placerar &lt;div>-taggen till vänster eller höger och tillämpar CSS-format på &lt;div>-taggen. Detaljerade anvisningar finns i proceduren nedan:

1. Navigera till mappen `CustomErrorManager-1.0-SNAPSHOT` och öppna mappen `etc\clientlibs\mf-custom-error-manager\CustomErrorManager\javascript`.
1. Öppna filen `customErrorManager.js` för redigering. Funktionen `markError` i filen accepterar följande parametrar:

   |   |  |
   |---|---|
   | jqWidget | jqWidget är handtaget för en widget. |
   | msg | innehåller felmeddelandet |
   | type | anger om det är ett fel eller en varning |

1. Vid körningen visas felmeddelanden till höger om fältet. Använd följande kod om du vill att felmeddelandena ska visas överst.

   ```javascript
   markError: function (jqWidget, msg, type) {
               var element = jqWidget.element,                                //Gives the div containing widget
                   pos = $(element).offset(),                          //Calculates the position of the div in the view port
                                                                   msgHeight = xfalib.view.util.TextMetrics.measureExtent(msg).height + 5;  //Calculating the height of the Error Message
                   styles = {};
                   styles.left = pos.left + "px";         // Assign the desired left position using pos.left. Here it is calculated for exact left of the field
                   styles.top = pos.top - msgHeight + "px";  // Assign the desired top position using pos.top. Here it is calculated for top of the field
               if (type != "warning") {
                   if(!jqWidget.errorDiv){
                                                                                   //Adding the warning div if it is not present already
                       jqWidget.errorDiv=$("<div id='customError'></div>").appendTo('body');
                   }
                   jqWidget.$css(jqWidget.errorDiv.get(0), styles); // Applying the styles to the warning div
                   jqWidget.errorDiv.text(msg).show();                     //Showing the warning message
               } else {
                   if(!jqWidget.errorDiv){
                                                                                   //Adding the error div if it is not present already
                       jqWidget.errorDiv=$("<div id='customWarning'></div>").appendTo('body');
                   }
                   jqWidget.$css(jqWidget.errorDiv.get(0), styles); // Applying the styles to the error div
                   jqWidget.errorDiv.text(msg).show();                     //Showing the warning message
               }
   
           },
   ```

1. Spara och stäng filen.
1. Navigera till mappen `CustomErrorManager-1.0-SNAPSHOT` och skapa ett arkiv med mapparna jcr_root och META-INF. Byt namn på arkivet till CustomErrorManager-1.0-SNAPSHOT.zip.
1. Använd pakethanteraren för att överföra och installera paketet.

## Visa felmeddelanden för flera fält  {#display-error-messages-for-multiple-fields-nbsp}

Använd det bifogade paketet för att samtidigt visa felmeddelanden för alla fält. Om du vill visa ett enda felmeddelande använder du standardprofilen.

### Anpassa utseendet på felmeddelanden.  {#customizing-the-appearance-of-error-messages-nbsp}

1. Gå till mappen etc\clientlibs\mf-custom-error-manager\CustomErrorManager\css.

1. Öppna filen sample.css för redigering. CSS-filen innehåller 2 id:n - #customError, #customWarning. Du kan använda dessa id:n för att ändra olika egenskaper som färg och teckenstorlek.

   Använd följande kod för att ändra teckenstorlek och färg för fel-/varningsmeddelanden.

   ```css
   #customError {
   color: #0000FF; // it changes the color of Error Message
   display:none;
   position:absolute;
   opacity:0.85;
   font-size: 24px;  // it changes the font size of Error Message
   z-index:5;
   }
   
   #customWarning {
   color: #00FF00;  // it changes the color of Warning Message
   display:none;
   position:absolute;
   opacity:0.85;
   font-size: 18px;   // it changes the font size of Warning Message
   z-index:5;
   }
   
   Save the changes.
   ```

1. Spara och stäng filen.
1. Navigera till mappen CustomErrorManager-1.0-SNAPSHOT och skapa ett arkiv med mapparna jcr_root och META-INF. Byt namn på arkivet till CustomErrorManager-1.0-SNAPSHOT.zip.
1. Använd pakethanteraren för att överföra och installera paketet.

## Rendera formuläret med den nya profilen.  {#render-the-form-with-the-new-profile-nbsp}

HTML5-formulär använder en standardprofil som inte finns i rutan: `https://&lt;server&gt;/content/xfaforms/profiles/default.html?contentRoot=&lt;xdp location&gt;&template=&lt;name of the xdp&gt;`

Om du vill visa ett formulär med anpassade felmeddelanden återger du formuläret med felprofilen: `https://&lt;server&gt;/content/xfaforms/profiles/error.html?contentRoot=&lt;xdp location&gt;&template=&lt;name of the xdp&gt;`

>[!NOTE]
>
>Det bifogade paketet installerar felprofilen.
