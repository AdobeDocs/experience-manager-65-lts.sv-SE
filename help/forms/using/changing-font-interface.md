---
title: Ändra teckensnitt i gränssnittet
description: Så här ändrar du teckensnitt i användargränssnittet selektivt.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Ändra teckensnitt i gränssnittet{#changing-the-font-on-the-interface}

Du kan ändra vilket teckensnitt som visas på arbetsytan i AEM Forms. Teckensnitt som används i ett visst avsnitt i användargränssnittet definieras i motsvarande avsnitt i formatmallen. Du kan ändra teckensnitten i användargränssnittet selektivt.

Följ de [allmänna stegen för anpassning av AEM Forms-arbetsytan](../../forms/using/generic-steps-html-workspace-customization.md) och, beroende på dina krav, följ stegen för att anpassa CSS, HTML eller båda.

1. Ändra eller lägg till teckensnittsfamiljen i en befintlig stil.
1. Ändra eller lägg till teckensnittsfamiljen för HTML-elementet.
1. Lägg till ett format och använd det för elementet HTML.

Om du till exempel vill ändra teckensnittet för den översta navigeringsfältets ankartext till Courier New följer du de här stegen:

1. Logga in på CRXDE Lite via `https://'[server]:[port]'/lc/crx/de/index.jsp`.
1. Gör något av följande:

   1. Om du vill ändra font-family i en befintlig stil lägger du till följande i filen newStyle.css på /apps/ws/css.

      ```css
      #topnav a {
         font-family: "Courier New";
      }
      ```

   1. Om du vill lägga till teckensnittsfamiljen för HTML-elementet kopierar du filen `/libs/ws/js/runtime/templates/appnavigation.html` till `/apps/ws/js/runtime/templates/appnavigation.html`.

      Uppdatera /apps/ws/js/runtime/templates/appnavigation.html så här:

      ```jsp
      <li class="process"><a href="#" title="<%= $.t('index.header.topnav.startprocess.detail')%>" style="font-family:Courier New;" ><%= $.t('index.header.topnav.startprocess.name')%></a></li>
      <li class="todo"><a href="#/todo" title="<%= $.t('index.header.topnav.todo.detail')%>" style="font-family:Courier New;" ><%= $.t('index.header.topnav.todo.name')%></a></li>
      <li class="track"><a href="#/tracking" title="<%= $.t('index.header.topnav.tracking.detail')%>" style="font-family:Courier New;" ><%= $.t('index.header.topnav.tracking.name')%></a></li>
      <li class="preference"><a href="#/preferences" title="<%= $.t('index.header.topnav.preferences.detail')%>" style="font-family:Courier New;" ><%= $.t('index.header.topnav.preferences.name')%></a></li>
      ```

      Öppna /apps/ws/js/registry.js för redigering och ersättning av `text!/lc/libs/ws/js/runtime/templates/appnavigation.html` med `text!/lc/apps/ws/js/runtime/templates/appnavigation.html`.

   1. Om du vill lägga till en stil som definierar teckensnittsfamiljen lägger du till följande i filen newStyle.css på /apps/ws/css.

      ```css
      .myNewFontStyle a {
         font-family: "Courier New";
      }
      ```

      Lägg till följande i filen appnavigation.html på /apps/ws/js/runtime/templates om du vill lägga till teckensnittsfamiljen för elementet HTML.

      ```jsp
      <div id="topnav" class="myNewFontStyle">
          <ul>
              <li class="process"><a href="#" title="<%= $.t('index.header.topnav.startprocess.detail')%>" ><%= $.t('index.header.topnav.startprocess.name')%></a></li>
              <li class="todo"><a href="#/todo" title="<%= $.t('index.header.topnav.todo.detail')%>"><%= $.t('index.header.topnav.todo.name')%></a></li>
              <li class="track"><a href="#/tracking" title="<%= $.t('index.header.topnav.tracking.detail')%>" ><%= $.t('index.header.topnav.tracking.name')%></a></li>
              <li class="preference"><a href="#/preferences" title="<%= $.t('index.header.topnav.preferences.detail')%>" ><%= $.t('index.header.topnav.preferences.name')%></a></li>
          </ul>
      </div>
      ```

1. Starta om arbetsytan och rensa webbläsarcachen så att ändringarna syns.

![change_font_before](assets/change_font_before.png)

Övre navigeringsfält före teckensnittsanpassning

![change_font_after](assets/change_font_after.png)

Det övre navigeringsfältet efter teckensnittsanpassning på den första fliken
