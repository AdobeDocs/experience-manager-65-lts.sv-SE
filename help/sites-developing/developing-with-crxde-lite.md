---
title: Utveckla med CRXDE Lite
description: CRXDE Lite är inbäddat i Adobe Experience Manager (AEM) och gör att du kan utföra standardutvecklingsuppgifter i webbläsaren
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
exl-id: a022067a-3bbe-4bce-9d49-b813fcbf0c6f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2113'
ht-degree: 0%

---

# Utveckla med CRXDE Lite{#developing-with-crxde-lite}

I det här avsnittet beskrivs hur du utvecklar ditt Adobe Experience Manager-program (AEM) med CRXDE Lite.

Mer information om olika utvecklingsmiljöer finns i översiktsdokumentationen.

CRXDE Lite är inbäddat i AEM och gör att du kan utföra standardutvecklingsuppgifter i webbläsaren. Med CRXDE Lite kan du skapa ett projekt, skapa och redigera filer (som .jsp och .java), mappar, mallar, komponenter, dialogrutor, noder, egenskaper och paket när du loggar.
CRXDE Lite rekommenderas när du inte har direktåtkomst till AEM-servern. Eller när du utvecklar ett program genom att utöka eller ändra körklara komponenter och Java™-paket, eller när du inte behöver en dedikerad felsökare, kodkomplettering och syntaxmarkering.

>[!NOTE]
>
>Från och med AEM 6.5.5.0 går det inte längre att få anonym åtkomst till CRXDE Lite.
>Användarna dirigeras om till inloggningsskärmen.


>[!NOTE]
>
>Adobe rekommenderar att du använder [AEM Developer Tools för Eclipse](/help/sites-developing/aem-eclipse.md) och [AEM HTL Brackets Extension](/help/sites-developing/aem-brackets.md) under projektutvecklingen.

## Komma igång med CRXDE Lite {#getting-started-with-crxde-lite}

Så här kommer du igång med CRXDE Lite:

1. Installera AEM.
1. Ange `https://<host>:<port>/crx/de` i webbläsaren. Som standard är det `https://localhost:4502/crx/de`.
1. Ange ditt **användarnamn** och **lösenord**. Som standard är det `admin` och `admin`.

1. Klicka på **OK**.

CRXDE Lite användargränssnitt ser ut så här i din webbläsare:

![chlimage_1-18](assets/crx-interface.jpg)

Nu kan du använda CRXDE Lite för att utveckla dina applikationer.

## Översikt över användargränssnittet {#overview-of-the-user-interface}

CRXDE Lite har följande funktioner:

<table>
 <tbody>
  <tr>
   <td>Övre växlingsfält</td>
   <td>Växla snabbt mellan CRXDE Lite, Package Manager och Package Share.</td>
  </tr>
  <tr>
   <td>Widgeten Nodbana</td>
   <td><p>Visar sökvägen till den markerade noden.</p> <p>Du kan också använda den för att hoppa till en nod, ange sökvägen manuellt eller klistra in den någon annanstans och trycka på Retur.</p> <p>Den har även stöd för att söka efter noder med ett specifikt nodnamn. Ange namnet på noden som du vill söka efter och vänta (eller tryck på söksymbolen till höger). Du kan prova att ange strängen <em>ek</em> i widgeten för att se hur den fungerar. Om en viss nod eller noder läses in i utforskarrutan visas listan och du kan välja sökvägen och trycka på Retur för att navigera till den. Det fungerar bara för noder som läses in i CRXDE-klientprogrammet i webbläsaren. Om du vill söka i hela databasen använder du Verktyg och sedan Fråga.</p> </td>
  </tr>
  <tr>
   <td>Utforskarfönster</td>
   <td><p>Visar ett träd med alla noder i databasen.</p> <p>Klicka på en nod så att du kan visa dess egenskaper på fliken <strong>Egenskaper</strong>. När du har klickat på en nod kan du välja en åtgärd i verktygsfältet. Klicka på noden igen för att byta namn på den.</p> <p>Trädnavigeringsfilter (binokulär ikon): gör att du kan filtrera noderna i databasen som namnet innehåller indatatexten för. Det gäller endast noder som har lästs in lokalt.<br /> </p> </td>
  </tr>
  <tr>
   <td>Redigeringsruta</td>
   <td><p>Fliken <strong>Hem</strong>: Här kan du söka efter innehåll och/eller dokumentation och få åtkomst till utvecklarresurser (dokumentation, utvecklarblogg, kunskapsbas) och support (Adobe hemsida och supportcenter).<br /> </p> <p>Dubbelklicka på en fil i rutan <strong>Utforskaren</strong> så att du kan visa dess innehåll. Exempel: en .jsp- eller .java-fil. Du kan sedan ändra den och spara ändringarna.</p> <p>När en fil har redigerats i rutan <strong>Redigera</strong> är följande verktyg tillgängliga i verktygsfältet:<br /> </p> - <strong>Visa i trädet: </strong>visar filen i databasträdet.<br /> - <strong>Sök/ersätt ...</strong>: sök eller ersätt.<br /> <br /> Dubbelklicka på statusraden i rutan <strong>Redigera</strong> så öppnas dialogrutan <strong>Gå till rad</strong> där du kan ange ett visst radnummer att gå till.<br /> </td>
  </tr>
  <tr>
   <td>Fliken Egenskaper <br /> </td>
   <td>Visar egenskaperna för den nod som du har markerat. Du kan lägga till nya eller ta bort befintliga egenskaper.<br /> </td>
  </tr>
  <tr>
   <td>Fliken Åtkomstkontroll</td>
   <td><p>Visa behörigheter baserat på sökväg, databasnivå eller huvudnamn.</p> <p>Behörigheterna delas upp i</p> <p>- <strong>Tillämplig åtkomstkontrollprincip</strong>: De principer som kan tillämpas på markeringen.</p> <p>- <strong>Lokala åtkomstkontrollprinciper</strong>: De principer som används lokalt i markeringen.</p> <p>- <strong>Effektiva åtkomstkontrollprinciper</strong>: De principer som används för markeringen kan anges lokalt eller ärvs från överordnade noder.</p> <p>Obs! För att kunna se åtkomstkontrollsinformationen alls måste användaren som är inloggad på CRXDE Lite ha läsbehörighet till ACL-poster. Den anonyma användaren kan inte se den här informationen som standard - logga in som administratör för att se informationen, till exempel.</p> </td>
  </tr>
  <tr>
   <td>Fliken Replikering</td>
   <td><p>Visa nodens replikeringsstatus. Du kan replikera och replikera borttagningen av noden.</p> </td>
  </tr>
  <tr>
   <td>Konsolflik<br /> </td>
   <td><p><strong>Serverloggar</strong>:</p> <p>Visar loggmeddelanden. Du kan konfigurera loggnivån, rensa konsolen, fästa vid den valda rullningspositionen och aktivera eller inaktivera visningen av meddelanden.<br /> </p> <p><strong>Versionskontroll</strong>:</p> <p>Visar versionskontrollmeddelanden.<br /> </p> </td>
  </tr>
  <tr>
   <td>Fliken Build Info <br /> </td>
   <td>Visar information när ett paket byggs.<br /> </td>
  </tr>
  <tr>
   <td>Uppdatera <br /> </td>
   <td>Uppdaterar markeringen. Ändringar från andra användare uppdateras i din vy av databasen. De ändringar du har gjort påverkas inte.<br /> </td>
  </tr>
  <tr>
   <td>Spara alla</td>
   <td><p><strong>Spara alla</strong>:<br /> </p> <p>Sparar alla ändringar du har gjort. Tills du klickar på Spara är ändringarna temporära och försvinner när du avslutar konsolen.</p> <p><strong>Återgå</strong>:</p> <p>Ignorerar alla ändringar som du har gjort på den valda noden sedan den senaste sparaåtgärden och läser sedan in databasens status för den valda noden igen.</p> <p><strong>Återställ alla</strong>:</p> <p>Ignorerar alla ändringar som du har gjort i hela databasen sedan den senaste sparåtgärden och läser sedan in databasens status igen.</p> </td>
  </tr>
  <tr>
   <td>Skapa..<br /> </td>
   <td><p>Listruta för att skapa följande under den valda noden:<br /> </p> <p>- <strong>Nod</strong>: en nod med en godtycklig nodtyp<br /> </p> <p>- <strong>Fil</strong>: nod:file och undernoden nt:resource</p> <p>- <strong>Mapp</strong>: nt:mappnod</p> <p>- <strong>Mall</strong>: AEM-mall</p> <p>- <strong>komponent</strong>: AEM-komponent</p> <p>- <strong>Dialog</strong>: AEM-dialogruta</p> </td>
  </tr>
  <tr>
   <td>Ta bort<br /> </td>
   <td>Tar bort den markerade noden.<br /> </td>
  </tr>
  <tr>
   <td>Kopiera</td>
   <td>Kopierar den markerade noden.<br /> </td>
  </tr>
  <tr>
   <td>Klistra in<br /> </td>
   <td>Klistrar in den kopierade noden under den markerade noden.<br /> </td>
  </tr>
  <tr>
   <td>Flytta..<br /> </td>
   <td>Flyttar den markerade noden till den nod som anges i dialogrutan.</td>
  </tr>
  <tr>
   <td>Byt namn..<br /> </td>
   <td>Byter namn på den markerade noden.<br /> </td>
  </tr>
  <tr>
   <td>Blandningar...<br /> </td>
   <td>Gör att du kan lägga till blandningstyper i nodtypen. Blandningstyperna används oftast för att lägga till avancerade funktioner som versionshantering, åtkomstkontroll, referenser och låsning till noden.</td>
  </tr>
  <tr>
   <td>Verktyg<br /> </td>
   <td><p>Listruta med följande verktyg:</p> <p>- <strong>Serverkonfiguration ...</strong>: för åtkomst till Felix Console.</p> <p>- <strong>Fråga ...</strong>: för att fråga databasen.</p> <p>- <strong>Behörigheter ...</strong>: för att öppna behörighetshantering, där du kan visa och lägga till behörigheter.</p> <p>- <strong>Testa åtkomstkontroll ...</strong>: en plats där du kan testa behörigheten för en viss sökväg och/eller huvudnamn.</p> <p>- <strong>Exportera nodtyp</strong>: om du vill exportera nodtyper i systemet som slutnotation.</p> <p>- <strong>Importera nodtyp ...</strong>: Om du vill importera nodtyper med slutnotation.</p> <p>- <strong>Installera SiteCatalyst-felsökaren..</strong>: instruktioner om hur du installerar Analytics-felsökaren.</p> </td>
  </tr>
  <tr>
   <td>Inloggningswidget<br /> </td>
   <td><p>Visar de inloggade användarna och arbetsytan som de är inloggade på, till exempel admin@crx.default.</p> <p>Klicka på den för att logga in eller logga in igen som en specifik användare. Om du inte anger en arbetsyta att logga in på loggas du in på standardarbetsytan, crx.default.</p> <p>Om du vill bläddra i databasen som en anonym användare använder du <strong>anonym</strong> som inloggningsnamn och eventuellt lösenord (till exempel ett mellanslag eller en punkt).<br /> </p> <p>Om din auktorisering inte längre är giltig (t.ex. har upphört att gälla) visas <strong>Obehörig - inloggning..</strong> i inloggningswidgeten. Klicka på den för att logga in igen.</p> </td>
  </tr>
 </tbody>
</table>

## Skapa en mapp {#creating-a-folder}

Så här skapar du en mapp med CRXDE Lite:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan högerklickar du på den mapp som du vill skapa mappen under, väljer **Skapa ...** och sedan **Skapa mapp ...**.

1. Ange mappen **Namn** och klicka på **OK**.

1. Klicka på **Spara alla** för att spara ändringarna på servern.

## Skapa en mall {#creating-a-template}

Så här skapar du en mall med CRXDE Lite:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan högerklickar du på den mapp där du vill skapa mallen, väljer **Skapa ...** och sedan **Skapa mall ...**.

1. Ange mallens **etikett**, **titel**, **beskrivning**, **resurstyp** och **rankning**. Klicka på **Nästa**.

1. Det här steget är valfritt: ange **tillåtna sökvägar**. Klicka på **Nästa**

1. Det här steget är valfritt: ange **Tillåtna överordnade**. Klicka på **Nästa**.

1. Det här steget är valfritt: ange **Tillåtna underordnade**. Klicka på **OK**.

1. Klicka på **Spara alla** för att spara ändringarna på servern.

Det skapar:

* En nod av typen `cq:Template` med mallegenskaper

* En underordnad nod av typen `cq:PageContent` med egenskaper för sidinnehåll

Du kan lägga till egenskaper i mallen: se avsnittet [Skapa en egenskap](#creating-a-property).

## Skapa en komponent {#creating-a-component}

Funktionen som beskrivs här är bara tillgänglig om CQ5 är installerat, det vill säga om nodtypen `cq:Component` är tillgänglig i databasen.

Så här skapar du en komponent med CRXDE Lite:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan högerklickar du på den mapp där du vill skapa komponenten, väljer **Skapa ...** och sedan **Skapa komponent ...**.

1. Ange komponentens **etikett**, **rubrik**, **beskrivning**, **superresurstyp** och **grupp**. Klicka på **Nästa**.

1. Det här steget är valfritt: ange komponentegenskaperna **Är behållare,** **Ingen dekoration**, **Cellnamn** och **Dialogsökväg**. Klicka på **Nästa**.

1. Det här steget är valfritt: ange komponentegenskapen **Tillåtna överordnade**. Klicka på **Nästa**.

1. Det här steget är valfritt: ange komponentegenskapen **Tillåtna underordnade**. Klicka på **OK**.

1. Klicka på **Spara alla** för att spara ändringarna på servern.

Det skapar:

* En nod av typen `cq:Component`
* Komponentegenskaper
* Ett .jsp-komponentskript

## Skapa en dialogruta {#creating-a-dialog}

Skapa en dialog med CRXDE Lite:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan högerklickar du på den komponent där du vill skapa dialogrutan, väljer **Skapa..** och sedan **Skapa dialogruta..**.

1. Ange **etiketten** och **titeln**. Klicka på **OK**.

1. Klicka på **Spara alla** l för att spara ändringarna på servern.

En dialogruta med följande struktur skapas:

`dialog[cq:Dialog]/items[cq:Widget]/items[cq:WidgetCollection]/tab1[cq:Panel]`

Nu kan du anpassa dialogrutan efter dina behov genom att ändra egenskaper eller skapa noder.

Du kan också använda Dialogruteredigeraren för att redigera en dialogruta. Om du dubbelklickar på dialognoden i CRXDE Lite öppnas redigeraren. Mer information finns i [Dialogruteredigeraren](/help/sites-developing/dialog-editor.md).

## Skapa en nod {#creating-a-node}

Så här skapar du en nod med CRXDE Lite:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan högerklickar du på den nod där du vill skapa noden, väljer **Skapa ...** och sedan **Skapa nod ...**.
1. Ange **Namn** och **Typ**. Klicka på **OK**.
1. Klicka på **Spara alla** för att spara ändringarna på servern.

Nu kan du anpassa noden efter dina behov genom att ändra egenskaper eller skapa noder.

>[!NOTE]
>
>De flesta redigeringsåtgärderna, inklusive Skapa nod, sparar alla ändringar i minnet och lagrar dem bara i databasen när de sparas (med knappen &quot;Spara alla&quot;). Vissa åtgärder, till exempel move, sparas dock automatiskt.
>
>Valideringen av om den nyskapade noden tillåts av den överordnade nodens nodtyp utförs också av JCR-databasen först när ändringarna sparas. Om du får ett felmeddelande när du sparar en nod kontrollerar du om innehållsstrukturen är giltig (du kan till exempel inte skapa en `nt:unstructured`-nod som underordnad `nt:folder`-nod).

## Skapa en egenskap {#creating-a-property}

Så här skapar du en egenskap med CRXDE Lite:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan markerar du den nod där du vill lägga till den nya egenskapen.
1. På fliken **Egenskaper** i den nedre rutan anger du **Namn**, **Typ** och **Värde**. Klicka på **Lägg till**.

1. Klicka på **Spara alla** för att spara ändringarna på servern.

## Skapa ett skript {#creating-a-script}

Skapa ett skript:

1. Öppna CRXDE Lite i webbläsaren.
1. I navigeringsrutan högerklickar du på den komponent där du vill skapa skriptet, väljer **Skapa ...** och sedan **Skapa fil ...**.

1. Ange filen **Namn** med filnamnstillägget. Klicka på **OK**.

1. Den nya filen öppnas som en flik i rutan Redigera.
1. Redigera filen.
1. Klicka på **Spara alla** för att spara ändringarna.

## Exportera och importera nodtyper {#exporting-and-importing-node-types}

Med CRXDE Lite kan du importera och/eller exportera nodtypsdefinitioner i [CND-notationen (Compact Namespace and Node Type Definition)](https://jackrabbit.apache.org/jcr/node-type-notation.html).

Så här exporterar du en nodtypsdefinition:

1. Öppna CRXDE Lite i webbläsaren.
1. Välj önskad nod.
1. Välj **Verktyg** och sedan **Exportera nodtyp**.

1. Definitionen visas i slutet av webbläsaren. Spara informationen, om det behövs.

Så här importerar du en nodtypsdefinition:

1. Öppna CRXDE Lite i webbläsaren.
1. Välj **Verktyg** och sedan **Importera nodtyp...**.

1. Ange CND-notation för definitionen i textrutan.
1. Markera **Tillåt uppdatering** om du uppdaterar en befintlig definition.
1. Klicka på **Importera**.

## Loggning {#logging}

Med CRXDE Lite kan du visa filen `error.log` som finns i filsystemet på `<crx-install-dir>/crx-quickstart/server/logs` och filtrera den med rätt loggnivå. Gör så här:

1. Öppna CRXDE Lite i webbläsaren.
1. Välj **Serverloggar** på fliken **Konsol** längst ned i fönstret i den nedrullningsbara menyn till höger.

1. Klicka på ikonen **Stopp** för att visa meddelandena.

Du kan:

* Justera loggparametrarna i Felix Console genom att klicka på ikonen **Loggningskonfigurationer** .
* Rensa meddelandena genom att klicka på ikonen **Pensel** .
* Fäst meddelandet vid markeringen genom att klicka på ikonen **Fäst** .
* Aktivera eller inaktivera visningen av meddelanden genom att klicka på ikonen **Stoppa** .

## Åtkomstkontroll {#access-control}

>[!NOTE]
>
>Mer information finns i [Administrera användare, grupper och åtkomsträttigheter](/help/sites-administering/user-group-ac-admin.md).
