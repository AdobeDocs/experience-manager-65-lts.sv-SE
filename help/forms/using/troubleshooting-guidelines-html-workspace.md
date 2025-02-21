---
title: Felsökningsriktlinjer för arbetsytan i AEM Forms
description: Aktivera loggar och använd felsökningsfunktionen i webbläsaren för att felsöka AEM Forms arbetsyta.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 0%

---

# Felsökningsriktlinjer för arbetsytan i AEM Forms {#troubleshooting-guidelines-for-aem-forms-workspace}

I den här artikeln beskrivs hur du felsöker arbetsytan i AEM Forms genom att aktivera loggning och använda felsökningsfunktionen i en webbläsare. Här förklaras också några vanliga problem som du kan stöta på när du använder AEM Forms arbetsyta och deras temporära lösningar.

## Det går inte att installera AEM Forms-arbetsytans paket {#unable-to-install-aem-forms-workspace-package}

Öppna arbetsytan i AEM Forms när du har installerat korrigeringen. Om felet Ingen resurs hittades öppnar du CRX Package Manager och installerar om paketet `adobe-lc-workspace-pkg-<version>.zip`.

Utför följande steg om ett fel `javax.jcr.nodetype.ConstraintViolationException: OakConstraint0025: Authorizable property rep:authorizableId may not be removed` uppstår när paketet installeras:

1. Logga in på CRXDE Lite. Standardwebbadressen är `https://[localhost]:'port'/lc/crx/de/index.jsp`
1. Ta bort följande nod:

   `/home/groups/P/PERM_WORKSPACE_USER`

1. Gå till Package Manager. Standardwebbadressen är `https://[localhost]:'port'/lc/crx/packmgr/index.jsp.`
1. Sök efter och installera paketet `adobe-lc-workspace-pkg-[version].zip`.
1. Starta om programservern.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

## Logga på arbetsytan i AEM Forms {#aem-forms-workspace-nbsp-logging}

Du kan generera loggar på olika nivåer för att optimera felsökningen. I ett komplext program kan till exempel loggning på komponentnivå hjälpa till att felsöka och felsöka specifika komponenter.

I AEM Forms:

* Om du vill få loggningsinformation om en viss komponentfil lägger du till `/log/<ComponentFile>/<LogLevel>` i URL:en och trycker på `Enter`. All loggningsinformation för komponentfilen på den angivna loggnivån skrivs ut på konsolen.

* Om du vill hämta loggningsinformation för alla komponentfiler lägger du till `/log/all/trace` i URL:en och trycker på `Enter`.

* Loggformat: `<Component file> <Date>:<Time>: <Log Level> : <Log Message>`

>[!NOTE]
>
>Som standard är loggnivån för alla komponenter inställd på INFO.

* Loggnivån som angetts av användaren bevaras endast för den webbläsarsessionen. När användaren uppdaterar sidan ställs loggnivån in på sitt ursprungliga värde för alla komponenter.

### Lista över komponentfiler på arbetsytan i AEM Forms {#list-of-component-files-in-nbsp-aem-forms-workspace}

<table>
 <tbody>
  <tr>
   <td><p>allcategoryModel</p> </td>
   <td><p>processinstanceModel</p> </td>
   <td><p>tasklistModel</p> </td>
  </tr>
  <tr>
   <td><p>appnavigationModel</p> </td>
   <td><p>processInstanceView</p> </td>
   <td><p>aktivitetslistaVisa</p> </td>
  </tr>
  <tr>
   <td><p>appnavigationView</p> </td>
   <td><p>procesnamelistModel</p> </td>
   <td><p>taskModel</p> </td>
  </tr>
  <tr>
   <td><p>ccategoryListModel</p> </td>
   <td><p>procesnamelistView</p> </td>
   <td><p>taskView</p> </td>
  </tr>
  <tr>
   <td><p>categoryListView</p> </td>
   <td><p>processnameModel</p> </td>
   <td><p>teamqueuesView</p> </td>
  </tr>
  <tr>
   <td><p>categoryModel</p> </td>
   <td><p>processnameView</p> </td>
   <td><p>todoView</p> </td>
  </tr>
  <tr>
   <td><p>categoryView</p> </td>
   <td><p>sökmallarinformationVisa</p> </td>
   <td><p>trackingView</p> </td>
  </tr>
  <tr>
   <td><p>favoritkategoriModell</p> </td>
   <td><p>sharequeueModel</p> </td>
   <td><p>uisettingsModel</p> </td>
  </tr>
  <tr>
   <td><p>filterlistView</p> </td>
   <td><p>sharequeueView</p> </td>
   <td><p>uisettingsView</p> </td>
  </tr>
  <tr>
   <td><p>filterView</p> </td>
   <td><p>startpointlistModel</p> </td>
   <td><p>userinfoModel</p> </td>
  </tr>
  <tr>
   <td><p>outofofficeModel</p> </td>
   <td><p>startpointlistView</p> </td>
   <td><p>userinfoView</p> </td>
  </tr>
  <tr>
   <td><p>outtofofficeView</p> </td>
   <td><p>startpointModel</p> </td>
   <td><p>usersearchModel</p> </td>
  </tr>
  <tr>
   <td><p>preferencesView</p> </td>
   <td><p>startpointView</p> </td>
   <td><p>usersearchView</p> </td>
  </tr>
  <tr>
   <td><p>procesinstancehistoryView</p> </td>
   <td><p>startProcessView</p> </td>
   <td><p>wserrorModel</p> </td>
  </tr>
  <tr>
   <td><p>processinstancelistModel</p> </td>
   <td><p>starprocessView</p> </td>
   <td><p>wserrorView</p> </td>
  </tr>
  <tr>
   <td><p>procesinstancelistView</p> </td>
   <td><p>uppgifterinformationVisa</p> </td>
   <td><p>wsmessageView</p> </td>
  </tr>
 </tbody>
</table>

### Loggnivåer som är tillgängliga på arbetsytan i AEM Forms {#log-levels-available-in-nbsp-aem-forms-workspace}

* FATAL
* FEL
* VARNING
* INFO
* FELSÖKNING
* TRACE
* AV

## Felsökningsinformation för webbläsare {#debugging-information-for-browsers}

Skript och format kan felsökas i olika webbläsare.

* **Felsökning i IE**: Om du vill felsöka AEM Forms-arbetsytan i IE går du till [https://learn.microsoft.com/en-us/office/dev/add-ins/testing/debug-add-ins-using-f12-tools-ie](https://learn.microsoft.com/en-us/office/dev/add-ins/testing/debug-add-ins-using-f12-tools-ie).

* **Felsökning i Chrome**: Använd kortkommandot Ctrl+Skift+I om du vill öppna felsökaren i Chrome. Mer information finns i: [https://developer.chrome.com/docs/extensions/mv3/tut_debugging/](https://developer.chrome.com/docs/extensions/mv3/tut_debugging/).

* **Felsökning i Firefox**: Det finns flera tillägg för felsökning av skript och format i Firefox. Firebug är till exempel ett sådant felsökningsverktyg ([https://getfirebug.com](https://getfirebug.com)).

## Vanliga frågor {#faqs}

1. PDF-formuläret återges inte eller skickas inte i Google Chrome.

   1. Installera plugin-programmet Adobe® Reader®.
   1. Öppna chrome://plugins i Chrome för att visa tillgängliga plugin-program.
   1. Inaktivera plugin-programmet Chrome PDF Viewer och aktivera plugin-programmet Adobe Reader.

1. SWF form eller Guide återges inte i Google Chrome.

   1. Öppna chrome://plugins i Chrome för att visa tillgängliga plugin-program.
   1. Se mer om Adobe Flash® Player.
   1. Inaktivera PepperFlash under Adobe Flash Player-plugin.

1. Jag har anpassat AEM Forms-arbetsytan men kan inte se ändringarna.

   Rensa webbläsarens cacheminne och öppna sedan arbetsytan i AEM Forms.

1. Vad behöver användaren göra för att formuläret ska kunna återges i HTML när det öppnas på skrivbordet?

   Markera alternativknappen HTML för standardprofil i tilldelningssteget när du använder Workbench.

1. Den bifogade filen visas inte när du klickar på den.

   Aktivera popup-fönster i webbläsaren om du vill visa bilagor.

1. En användare är inloggad i ett formulärprogram. Om användaren försöker logga in på arbetsytan kanske den inte läses in om användaren inte har behörighet till arbetsytan.

   Logga ut från det andra formulärprogrammet och logga sedan in på arbetsytan.

1. HTML-formulär, som använder Processegenskaper i sin design, visar Skicka-knappen i formuläret när de återges på arbetsytan i AEM Forms.

   När du utformar formulär läggs en Skicka-knapp till i formuläret när du använder Processegenskaper. Om den återges som en PDF-fil på arbetsytan i AEM Forms är knappen Skicka inte synlig för slutanvändaren. När du återger som ett HTML-formulär på arbetsytan i AEM Forms visas knappen Skicka för slutanvändaren. Om du klickar på knappen Skicka i formuläret initieras ingen åtgärd. Slutför uppgiften genom att klicka på Skicka längst ned på arbetsytan i AEM Forms utanför formuläret.
