---
title: Konfigurera Adobe PDF-inställningar
description: Lär dig hur du konfigurerar Adobe PDF-inställningar som visas på sidan Adobe PDF-inställningar. Du kan använda någon av de fördefinierade PDF-inställningarna eller skapa egna.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_pdf_generator
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: PDF Generator
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 41a8a4b0-cb39-40a6-82b6-085f2c635e0c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '7415'
ht-degree: 0%

---

# Konfigurera Adobe PDF-inställningar{#configuring-adobe-pdf-settings}

På sidan Inställningar i Adobe PDF visas de konverteringsinställningar som du kan ange för dina källor. Du kan använda någon av de fördefinierade PDF-inställningarna eller skapa egna. Inställningarna för PDF avgör exakt hur filer konverteras och vilka PDF-strukturer och -funktioner de ger. Adobe PDF-inställningarna kallades tidigare Distiller®-parametrar eller jobbalternativ.

På sidan Inställningar i Adobe PDF kan du göra följande:

* Visa de fördefinierade PDF-inställningarna. (Se [Om de fördefinierade PDF-inställningarna](configuring-pdf-settings.md#about-the-predefined-pdf-settings).)
* Skapa en PDF-inställning eller redigera en som du har skapat tidigare. (Se [Lägga till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).)
* Ange standardinställningar för PDF. (Se [Ändra standardinställningarna](/help/forms/using/admin-help/configuring-file-type-settings.md#change-the-default-settings))
* Överför en PDF-inställningsfil till servern. (Se [Överför PDF-inställningar](configuring-pdf-settings.md#upload-pdf-settings).)
* Ta bort anpassade PDF-inställningar. (Se [Ta bort PDF-inställningar](configuring-pdf-settings.md#delete-pdf-settings).)
* Ladda upp och ladda ned prolog- och epilogfiler. (Se [Överför och hämtar prolog- och epilogfiler](configuring-pdf-settings.md#uploading-and-downloading-prologue-and-epilogue-files).)

Adobe PDF-inställningarna gäller endast för PDFMaker-baserade konverteringar. De innehåller följande konverteringar:

* Microsoft Word-dokument (DOC, DOCX, RTF, TXT)
* Microsoft Excel-dokument (XLS, XLSX)
* Microsoft PowerPoint-dokument (PPT, PPTX)
* Microsoft Project-dokument (MPP)
* Microsoft Visio-dokument (VSD)

>[!NOTE]
>
>Om du använder OpenOffice för att konvertera ovanstående format används inte Adobe PDF-inställningarna.

## Om fördefinierade PDF-inställningar {#about-the-predefined-pdf-settings}

PDF Generator har flera fördefinierade PDF-inställningar som du kan använda. Du kan inte ändra dessa fördefinierade inställningar, men du kan skapa en inställning som baseras på en befintlig inställning genom att redigera inställningen och spara den under ett nytt namn.

**Högkvalitetsutskrift:** Skapar PDF-filer för högkvalitativa utdata. Den här inställningen:

* nedsamplar färgbilder och gråtonsbilder med 300 dpi
* nedsamplar monokroma bilder med 1 200 dpi
* skriver ut till en högre bildupplösning
* använder andra inställningar för att bevara maximal information om originaldokumentet.

Dessa PDF-filer kan öppnas i Adobe Acrobat 5 och Adobe Acrobat Reader® 5 eller senare.

**Stora sidor:** Skapar PDF-dokument som är lämpliga för visning och utskrift av ingenjörsritningar som är större än 200 x 200 tum. Skapade PDF-dokument kan öppnas i Adobe Acrobat Professional och Acrobat Standard, version 7 eller senare samt Adobe Reader 7 eller senare.

**PDF/A-1B 2005 CMYK/PDF/A-1B 2005 RGB:** Kontrollerar om inkommande jobb uppfyller ISO-standarden för långtidsarkivering av elektroniska dokument och skapar PDF/A-filer endast om de är kompatibla. De här filerna används främst för arkivering. Kompatibla filer kan bara innehålla text, rasterbilder och vektorobjekt. De kan inte innehålla kryptering och skript. Dessutom måste alla teckensnitt bäddas in så att dokumenten kan öppnas och visas som de skapats. PDF/A-1b använder PDF 1.4 och konverterar alla färger till antingen CMYK eller RGB, beroende på vilken standard du väljer. PDF-filer som skapas med den här inställningsfilen kan öppnas i Acrobat 5 och Acrobat Reader 5 eller senare. Mer information om PDF/A finns i Adobe och branschstandarder.

**PDF/X-1a 2001:** Kontrollerar inkommande jobb för PDF/X-1a-kompatibilitet och skapar PDF-filer endast om de är kompatibla. PDF/X-1a är en ISO-standard för utbyte av grafiskt innehåll. PDF/X-1a kräver att alla teckensnitt är inbäddade, att rätt PDF-rutor anges och att färg visas som antingen CMYK eller dekorfärger. PDF-filer som uppfyller PDF/X-1a-kraven är avsedda för ett visst utdatavillkor, t.ex. webboffsettryck enligt specifikationerna för webbförskjutningspublikationer. Mer information om PDF/X finns i Adobe och branschstandarder.

**PDF/X-3 2002:** Kontrollerar inkommande jobb för PDF/X-3-kompatibilitet och skapar PDF-filer endast om de är kompatibla. Precis som PDF/X-1a är PDF/X-3 en ISO-standard för utbyte av bildinnehåll. Den största skillnaden är att PDF/X-3 har stöd för enhetsoberoende färg.

**Tryckkvalitet:** Skapar PDF-filer för tryckning med hög kvalitet (t.ex. på en fotosättare eller en plåtsättare). I det här fallet behöver du inte tänka på filstorleken. Målet är att bibehålla all den information i en PDF-fil som ett tryckeri eller en prepress-leverantör behöver för att skriva ut dokumentet på rätt sätt. Den här uppsättningen alternativ:

* nedsamplar färgbilder och gråtonsbilder med 300 dpi
* nedsamplar monokroma bilder med 1 200 dpi
* bäddar in deluppsättningar av alla teckensnitt som används i dokumentet
* ger högre bildupplösning,
* roterar inte sidor automatiskt baserat på orienteringen av text- eller DSC-kommentarer
* använder andra inställningar för att bevara maximal information om originaldokumentet.

Utskriftsjobb misslyckas om de har teckensnitt som inte kan bäddas in. Dessa PDF-filer kan öppnas i Acrobat 5 och Acrobat Reader 5 eller senare.

>[!NOTE]
>
>Innan du skapar en PDF-fil som ska skickas till en skrivare eller prepress-leverantör bör du fastställa utdataupplösning och andra inställningar eller begära en .joboptions-fil med rekommenderade inställningar. Du kan behöva anpassa Adobe PDF-inställningarna för en viss leverantör och sedan bifoga dem som en .joboptions-fil.

**Minsta filstorlek:** Skapar PDF-filer för visning på webben eller i ett intranät, eller för distribution via ett e-postsystem för visning på skärmen. Den här uppsättningen alternativ använder komprimering, nedsampling och en relativt låg bildupplösning. Alla färger konverteras till sRGB och teckensnitt bäddas inte in om det inte behövs. Den optimerar också filer för byte-visning. Dessa PDF-filer kan öppnas i Acrobat 5 och Acrobat Reader 5.0 eller senare.

**Standard:** Skapar PDF-filer som ska skrivas ut på skrivare eller digitala kopiatorer, publiceras på en cd-skiva eller skickas till en kund som publiceringskorrektur. Den här uppsättningen alternativ använder komprimering och nedsampling för att minska filstorleken. Den bäddar också in deluppsättningar av alla teckensnitt som används i filen, konverterar alla färger till sRGB och skriver ut till en medelhög upplösning för att skapa en korrekt återgivning av originaldokumentet. Observera att deluppsättningar av Microsoft Windows-teckensnitt inte bäddas in som standard. Dessa PDF-filer kan öppnas i Acrobat 5 och Acrobat Reader 5.0 eller senare.

## Lägga till eller redigera PDF-inställningar {#add-or-edit-pdf-settings}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

PDF-inställningarna avgör exakt hur filer konverteras och vilka PDF-strukturer och -funktioner de ger. Definiera en ny PDF-inställning eller redigera en som du har skapat tidigare. Du kan inte ändra fördefinierade inställningar, men du kan skapa en inställning som baseras på en befintlig inställning genom att redigera inställningen och spara den under ett nytt namn.

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Adobe PDF-inställningar.
1. Klicka antingen på Ny eller på namnet på en befintlig inställning.
1. Fyll i den obligatoriska informationen i följande avsnitt på sidan Ny/Redigera Adobe PDF-inställning:

[Allmänna alternativ](configuring-pdf-settings.md#general-options)

[Bildalternativ](configuring-pdf-settings.md#images-options)

[Alternativ för teckensnitt](configuring-pdf-settings.md#fonts-options)

[Färgalternativ](configuring-pdf-settings.md#color-options)

[Avancerade alternativ](configuring-pdf-settings.md#advanced-options)

[Standardrapportering och regelefterlevnadsalternativ](configuring-pdf-settings.md#standards-reporting-and-compliance-options)

[Alternativ för inledande vy](configuring-pdf-settings.md#initial-view-options)

   Om du vill gå till ett annat avsnitt klickar du på länken på webbsidan eller använder knapparna Nästa och Föregående.

1. När du är klar med informationen i alla avsnitt klickar du på Spara eller Spara som och anger ett namn för inställningen.

## Överför PDF-inställningar {#upload-pdf-settings}

Du kan ha PDF-inställningar tillgängliga på PDF Generator-servern genom att överföra dem från en lokal dator eller en nätverksplats.

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Adobe PDF-inställningar och sedan på Överför.
1. På sidan Överför Adobe PDF-inställningar klickar du på Bläddra, letar upp PDF-inställningsfilen och klickar på Öppna.
1. Klicka på OK och sedan på OK igen.

## Ta bort PDF-inställningar {#delete-pdf-settings}

Du kan ta bort PDF-inställningar permanent om de inte längre behövs.

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Adobe PDF-inställningar.
1. Markera kryssrutan bredvid inställningen som ska tas bort. Du kan välja flera inställningar.
1. Klicka på Ta bort och klicka på Ta bort igen på sidan Ta bort bekräftelse.

## Allmänna alternativ {#general-options}

Använd de allmänna alternativen för att ange vilken version av Acrobat som ska användas för filkompatibilitet och andra fil- och enhetsalternativ. Instruktioner om hur du får åtkomst till de allmänna alternativen finns i [Lägg till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

### Filalternativ {#file-options}

**Kompatibilitet:** Kompatibilitetsnivån för PDF-filen. För dokument som ska distribueras i stor omfattning bör du överväga att välja Acrobat 4 (PDF 1.3) eller Acrobat 5 (PDF 1.4) för att säkerställa att alla användare kan visa och skriva ut dokumentet. Om du skapar filer med Acrobat 5-kompatibilitet eller senare kanske de inte är kompatibla med tidigare versioner av Acrobat. I följande underavsnitt visas några av skillnaderna mellan PDF-filer som har skapats med olika nivåer av Acrobat-kompatibilitet.

<table>
 <tbody>
  <tr>
   <th><p>Acrobat 4 (PDF 1.3)</p> </th>
   <th><p>Acrobat 5 (PDF 1.4)</p> </th>
   <th><p>Acrobat 6 (PDF 1.5)</p> </th>
   <th><p>Acrobat 7 (PDF 1.6) och Acrobat 8 (PDF 1.7)</p> </th>
  </tr>
  <tr>
   <td><p>Kan öppnas med Acrobat 3.0 och Acrobat Reader 3.0 eller senare.</p> </td>
   <td><p>Kan öppnas med Acrobat 3.0 och Acrobat Reader 3.0 eller senare. Funktioner som är specifika för senare versioner kan gå förlorade eller inte visas.</p> </td>
   <td><p>De flesta kan öppnas med Acrobat 4 och Acrobat Reader 4.0 eller senare. Funktioner som är specifika för senare versioner kan gå förlorade eller inte visas.</p> </td>
   <td><p>De flesta kan öppnas med Acrobat 4 och Acrobat Reader 4.0 eller senare. Funktioner som är specifika för senare versioner kan gå förlorade eller inte visas.</p> </td>
  </tr>
  <tr>
   <td><p>Får inte innehålla teckningar som använder live-genomskinlighetseffekter. Genomskinlighet måste förenklas innan du konverterar till PDF 1.3.</p> </td>
   <td><p>Stöder användning av live-genomskinlighet i teckningar. (Acrobat Distiller förenklar genomskinlighet.)</p> </td>
   <td><p>Stöder användning av live-genomskinlighet i teckningar. (Acrobat Distiller förenklar genomskinlighet.)</p> </td>
   <td><p>Stöder användning av live-genomskinlighet i teckningar. (Acrobat Distiller förenklar genomskinlighet.)</p> </td>
  </tr>
  <tr>
   <td><p>Lager stöds inte.</p> </td>
   <td><p>Lager stöds inte.</p> </td>
   <td><p>Lagren bevaras när du skapar PDF-filer från program som stöder generering av PDF-dokument med lager, t.ex. Adobe Illustrator® CS eller Adobe InDesign® CS eller senare.</p> </td>
   <td><p>Lagren bevaras när du skapar PDF-filer från program som stöder generering av PDF-dokument med lager, som Illustrator CS eller InDesign CS eller senare.</p> </td>
  </tr>
  <tr>
   <td><p>DeviceN-färgrymd med 8 grundfärger stöds.</p> </td>
   <td><p>DeviceN-färgrymd med 8 grundfärger stöds.</p> </td>
   <td><p>DeviceN-färgrymd med upp till 31 grundfärger stöds.</p> </td>
   <td><p>DeviceN-färgrymd med upp till 31 grundfärger stöds.</p> </td>
  </tr>
  <tr>
   <td><p>Du kan bädda in flerbyteteckensnitt. (Distiller konverterar teckensnitten vid inbäddning.)</p> </td>
   <td><p>Du kan bädda in flerbyteteckensnitt.</p> </td>
   <td><p>Du kan bädda in flerbyteteckensnitt.</p> </td>
   <td><p>Du kan bädda in flerbyteteckensnitt.</p> </td>
  </tr>
  <tr>
   <td><p>40-bitars RC4-säkerhet stöds.</p> </td>
   <td><p>128-bitars RC4-säkerhet stöds.</p> </td>
   <td><p>128-bitars RC4-säkerhet stöds.</p> </td>
   <td><p>128-bitars RC4- och 128-bitars AES-säkerhet (Advanced Encryption Standard) stöds.</p> </td>
  </tr>
 </tbody>
</table>

**Objektnivåkomprimering:** Konsoliderar små objekt (som inte är komprimeringsbara själva) till strömmar som sedan kan komprimeras effektivt.

**Av:** Komprimerar ingen strukturell information i PDF-dokumentet. Välj det här alternativet om du vill att användarna ska kunna visa, navigera och interagera med bokmärken och annan strukturell information med Acrobat 5 eller senare.

**Endast taggar:** Komprimerar strukturell information i PDF-dokumentet. Om du använder det här alternativet skapas en PDF-fil som kan öppnas och skrivas ut med Acrobat 5. Användare kan inte visa hjälpmedel, struktur eller taggad PDF-information i Acrobat 5 eller Acrobat Reader 5.0, men de kan visa informationen i Acrobat 6 och Adobe Reader 6.0.

**Rotera sidor automatiskt:** Anger automatisk rotation av sidor baserat på textens eller DSC-kommentarernas orientering. På vissa sidor (t.ex. sidor som innehåller tabeller) kan användaren behöva vända dem åt sidan för att kunna läsa dem. Välj Individuellt om du vill rotera varje sida baserat på textriktningen på sidan. Välj Kollektivt efter fil om du vill rotera alla sidor i dokumentet baserat på orienteringen för den mesta texten.

>[!NOTE]
>
>Om Bearbeta DSC-kommentarer är markerat i de avancerade inställningarna och om %%orienteringskommentarer för visning inkluderas, prioriteras dessa kommentarer när sidorienteringen bestäms.

**Bindning:** Anger om en PDF-fil med bindning på vänster eller höger sida ska visas. Den här inställningen påverkar visningen av sidor i layouten Motstående sida - Löpande och visningen av miniatyrbilder sida vid sida.

**Upplösning:** Anger emulering för en skrivares upplösning för indatafiler som justerar deras beteende enligt upplösningen för den skrivare de skriver ut på. För de flesta indatafiler ger en högre upplösningsinställning större men högkvalitativa PDF-filer, och en lägre inställning ger mindre men mindre PDF-filer. Det vanligaste är att upplösningen avgör antalet steg i en övertoning eller blandning. Du kan ange ett värde mellan 72 och 4 000. Använd den här inställningen som standard om du inte tänker skriva ut PDF-filen till en viss skrivare och vill emulera upplösningen som definierats i den ursprungliga indatafilen.

>[!NOTE]
>
>Om du ökar upplösningsinställningen ökar filstorleken och kan göra att det tar något längre tid att bearbeta vissa filer.

**Alla sidor eller Sidor från:** Anger vilka sidor som ska konverteras. Lämna rutan Till tom om du vill skapa ett intervall från det sidnummer du anger i rutan Från till slutet av filen.

**Optimera för snabb webbvisning:** Strukturerar om filen för sidvis hämtning (byte-hämtning) från webbservrar. Det här alternativet komprimerar text och teckningar, oavsett vad du har valt som komprimeringsinställningar på fliken Bilder. Komprimering ger snabbare åtkomst och visning när filen hämtas från webben eller ett nätverk. Som standard är det här alternativet inte aktiverat.

### Standardsidstorlek {#default-page-size}

Alternativen för Standardsidstorlek anger vilken sidstorlek som ska användas när ingen sidstorlek har angetts i originalfilen. Vanligtvis innehåller Adobe PostScript-filer den här informationen, förutom för Encapsulated PostScript-filer (EPS) som ger en begränsningsramstorlek men inte en sidstorlek. Den största tillåtna sidstorleken är 31 800 000 cm i båda riktningarna. De här alternativen konfigurerar standardsidstorleken:

**Bredd:** Sidans bredd

**Höjd:** Sidans höjd

**Enheter:** Enheter som ska användas för bredd- och höjdinställningarna

## Bildalternativ {#images-options}

Med alternativen för Bilder anger du komprimering och omsampling för bilder. Du kan experimentera med de här alternativen för att hitta en lämplig balans mellan filstorlek och bildkvalitet. Instruktioner om hur du får åtkomst till bildinställningarna finns i [Lägg till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

Dessa alternativ konfigurerar färgbilder, gråtonsbilder och monokroma bilder:

**Nedsampla:** Ange ett värde för varje typ av bild. Om du vill nedsampla färgbilder, gråskalebilder eller monokroma bilder kombinerar PDF Generator pixlar i ett exempelområde så att en större pixel skapas. Ange upplösningen för utdataenheten i dpi och ange en upplösning i dpi i rutan För bilder ovan. För bilder med en upplösning över detta tröskelvärde kombinerar PDF Generator pixlar efter behov för att minska bildens upplösning (pixlar per tum) till den angivna dpi-inställningen. Om du vill stänga av nedsampling väljer du Av. Här är alternativen:

**Genomsnittlig nedsampling till:** Medelvärdet för pixlarna i ett samplingsområde och ersätter hela området med den genomsnittliga pixelfärgen vid den angivna upplösningen.

**Bikubisk nedsampling till:** Använder ett viktat genomsnitt för att bestämma pixelfärgen och ger vanligtvis bättre resultat än den enkla metoden för nedsampling. Bikubisk är den långsammaste men mest exakta metoden och ger de mjukaste tonövergångarna.

**Delsampling till:** Markerar en pixel i mitten av exempelområdet och ersätter hela området med den pixeln med den angivna upplösningen. Delsampling minskar konverteringstiden avsevärt jämfört med nedsampling, men det resulterar i bilder som är mindre mjuka och kontinuerliga.

Upplösningsinställningen för färg och gråskala bör vara 1,5 till 2 gånger rastertätheten som filen skrivs ut med. (Om du inte går under den här rekommenderade upplösningsinställningen påverkas inte bilder som inte innehåller några raka linjer, geometriska eller upprepade mönster av lägre upplösning.) Upplösningen för monokroma bilder bör vara densamma som för utdataenheten. Om du sparar en monokrom bild med en upplösning på över 1 500 dpi ökar filstorleken utan att bildkvaliteten förbättras nämnvärt.

Tänk också på om användarna behöver förstora en sida. Om du till exempel skapar ett PDF-dokument av en karta bör du använda en högre bildupplösning så att användarna kan zooma in på kartan.

>[!NOTE]
>
>Omsampling av monokroma bilder kan ge oväntade visningsresultat, till exempel att ingen bild visas. Om det här problemet inträffar inaktiverar du omsampling och konverterar filen igen. Det här problemet inträffar troligast vid delsampling och inträffar oftast vid bikubisk nedsampling.

Tabellen innehåller olika typer av skrivare och deras upplösning mätt i dpi, deras standardrastertäthet mätt i linjer per tum (lpi) och en omsamplingsupplösning för bilder som mäts i pixlar per tum (ppi). Om du till exempel vill skriva ut till en laserskrivare med 600 dpi anger du 170 för upplösningen och samplar om bilderna på.

<table>
 <tbody>
  <tr>
   <th><p>Skrivarupplösning</p> </th>
   <th><p>Standardlinjeraster</p> </th>
   <th><p>Bildupplösning</p> </th>
  </tr>
  <tr>
   <td><p>300 dpi (laserskrivare)</p> </td>
   <td><p>60 lpi</p> </td>
   <td><p>120 ppi</p> </td>
  </tr>
  <tr>
   <td><p>600 dpi (laserskrivare)</p> </td>
   <td><p>85 lpi</p> </td>
   <td><p>170 ppi</p> </td>
  </tr>
  <tr>
   <td><p>1 200 dpi (fotosättare)</p> </td>
   <td><p>120 lpi</p> </td>
   <td><p>240 ppi</p> </td>
  </tr>
  <tr>
   <td><p>2 400 dpi (fotosättare)</p> </td>
   <td><p>150 lpi</p> </td>
   <td><p>300 ppi</p> </td>
  </tr>
 </tbody>
</table>

**Komprimering:** Ange ett värde som ska användas för färg-, gråskale- och monokroma bilder. För färg- och gråskalebilder anger du även bildkvaliteten:

* För färg- och gråskalebilder väljer du ZIP om du vill använda komprimering som fungerar bra på bilder som har stora enfärgade områden eller upprepade mönster. Exempel är skärmbilder, enkla bilder som skapats med färgprogram och monokroma bilder som innehåller upprepade mönster. Välj JPEG, från minimal kvalitet till maximal, om du vill använda komprimering som passar för gråskale- eller färgbilder, till exempel halvtonsfotografier som innehåller fler detaljer än vad som kan återges på skärmen eller i tryck. Välj Automatisk (JPEG) för att automatiskt fastställa den bästa kvaliteten för färg- och gråskalebilder.
* För monokroma bilder väljer du CCITT Grupp 4, CCITT Grupp 3, ZIP, JPEG200, Automatisk (JPEG2000) eller Run Length-komprimering.

Kontrollera att monokroma bilder skannas som monokroma och inte som gråskalebilder. Skannad text sparas ibland som gråskalebilder som standard. Gråskaletext som komprimerats med komprimeringsmetoden JPEG är inte tydlig och kan vara oläslig.

**Bildkvalitet:** Konfigurerar bildkvaliteten för färg- och gråskalebilder. Alternativen är minimum, low, medium, high och maximum.

**Kantutjämning till gråskala:** Jämnar ut taggiga kanter i monokroma bilder. Välj 2 bitar, 4 bitar eller 8 bitar för att ange 4, 16 eller 256 gråtoner. (Kantutjämning kan göra små tecken oskarpa eller tunna linjer tunna.)

>[!NOTE]
>
>Komprimering av text och teckningar är alltid aktiverat.

**Bildprincip:** Ange en profil för färgbilder, gråskalebilder och monokroma bilder. Om bildupplösningen ligger under den angivna upplösningen kan du fortfarande välja att fortsätta (Ignorera), ange ett varningsmeddelande eller avbryta jobbet.

## Alternativ för teckensnitt {#fonts-options}

Teckensnittsalternativen anger vilka teckensnitt som ska bäddas in i en PDF-fil och om en deluppsättning av de tecken som ska bäddas in i PDF-filen ska bäddas in. Instruktioner om hur du kommer åt teckensnittsalternativen finns i [Lägg till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

>[!NOTE]
>
>När du kombinerar PDF-filer med samma teckensnittsdelmängd försöker PDF Generator kombinera deluppsättningarna med teckensnitt.

**Bädda in alla teckensnitt:** Bäddar in alla teckensnitt som används i filen. Teckensnittsinbäddning krävs för PDF/X-kompatibilitet.

**Skapa delmängd av inbäddade teckensnitt när procent av de tecken som används
Är mindre än:** Om du väljer det här alternativet anger du ett tröskelvärde i procent om du bara vill bädda in en delmängd av teckensnitten. Om tröskelvärdet till exempel är 35 och mindre än 35 % av tecknen används bäddas endast dessa tecken in i PDF Generator. Endast teckensnitt med rätt behörighetsbitar bäddas in.

**När inbäddning misslyckas:** Anger hur PDF Generator ska svara om det inte går att hitta ett teckensnitt att bädda in när en fil bearbetas. Du kan låta PDF Generator ignorera begäran och ersätta teckensnittet, varna dig och ersätta teckensnittet eller avbryta bearbetningen av det aktuella jobbet.

**Source:** Sökvägen för de teckensnitt som används i PDF Generator.

### Ange vilka teckensnitt som ska bäddas in {#specify-which-fonts-to-embed}

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Adobe PDF-inställningar.
1. Klicka på Ny eller klicka på namnet på en inställning.
1. Klicka på Teckensnitt och avmarkera Bädda in alla teckensnitt.
1. Välj en teckensnittskälla i listan Teckensnittskälla och klicka på Gå för att uppdatera listan med teckensnitt i rutan till vänster.
1. Klicka på ett teckensnitt i rutan till vänster. Klicka sedan på Lägg till bredvid lämplig ruta för att flytta den till listan Inkludera alltid eller Inkludera aldrig. Upprepa för varje teckensnitt. Ctrl-klicka om du vill markera flera teckensnitt som ska flyttas.
1. Om du vill ta bort ett teckensnitt från listan Inkludera alltid eller Inkludera aldrig, markerar du det och klickar på Ta bort bredvid motsvarande ruta. Den här åtgärden tar inte bort teckensnittet från systemet, utan bara bort referensen till det i listan.
1. Om det teckensnitt du vill ange inte visas skriver du namnet i rutan Lägg till teckensnitt och klickar sedan på Inkludera alltid eller Inkludera aldrig. Teckensnittsnamn får inte innehålla alfanumeriska tecken.

>[!NOTE]
>
>Ett TrueType-teckensnitt kan innehålla en inställning som teckensnittsdesignern har lagt till som förhindrar att teckensnittet bäddas in i PDF-filer.

>[!NOTE]
>
>Teckensnitt hämtas från Windows-systemets teckensnittscache och en systemomstart krävs för att uppdatera cachen. När du har angett kundens teckensnittskatalog måste du starta om datorn där AEM-formulär är installerade.

## Färgalternativ {#color-options}

Färgalternativen anger all färghanteringsinformation för PDF Generator. Instruktioner om hur du får åtkomst till färgalternativen finns i [Lägga till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

### Adobe Color-inställningar {#adobe-color-settings}

**Inställningsfil:** Den här listan innehåller en lista med färginställningar som också används i vanliga grafikprogram, till exempel Adobe Photoshop och Adobe Illustrator. Den färginställning du väljer avgör vilka andra Adobe-färginställningar som finns på den här sidan. Om du t.ex. väljer en annan inställning än Ingen, fördefinieras och nedtonas alla andra alternativ än de som används för enhetsberoende data. Du kan bara redigera färghanteringsprofiler och inställningar för arbetsfärgrymder om du väljer Ingen för inställningsfilen.

### Färghanteringsprofiler {#color-management-policies}

Om du valde Ingen för inställningsfilen anger området Färghanteringsprofiler hur PDF Generator konverterar ohanterad färg i en PostScript-fil.

**Ändra inte färg:** Ändrar inte enhetsberoende färger och enhetsoberoende färger bevaras som närmast motsvarande i PDF. Det här alternativet är användbart för tryckerier som har kalibrerat alla sina enheter, använt informationen för att ange färg i filen och endast utformat för dessa enheter.

**Tagga allt för färghantering:** Bäddar in en internationell färgkonsortier-profil när du bearbetar filer och kalibrerar färger i bilderna, vilket gör färgerna i de resulterande PDF-filerna enhetsoberoende om du valde kompatibilitet med Acrobat 4 (PDF 1.3) eller senare. Enhetsberoende färgrymder i filer (RGB, Gråskala och CMYK) konverteras dock till enhetsoberoende färgrymder (CalRGB, CalGray och LAB).

**Tagga endast bilder för färghantering:** Bäddar endast in ICC-profiler i bilder, inte i text eller bilder, när du bearbetar filer om du har valt kompatibilitet med Acrobat 4 (PDF 1.3). Det här alternativet förhindrar att svart text genomgår några färgförändringar. Enhetsberoende färgrymder i bilder (RGB, Gråskala och CMYK) konverteras dock till enhetsoberoende färgrymder (CalRGB, CalGray och LAB). Text och bilder konverteras inte.

**Konvertera alla färger till sRGB eller Konvertera alla färger till
CMYK:** Kalibrerar färgen i filen, vilket gör färghanteringen oberoende av varandra, som Tagga allt för färghantering. Om du har valt kompatibilitet med Acrobat 4 (PDF 1.3) eller senare och konverterar till sRGB konverteras CMYK- och RGB-bilderna till sRGB.

Oavsett vilket kompatibilitetsalternativ du väljer ändras inte gråskalebilder. Detta minskar vanligtvis storleken och ökar visningshastigheten för PDF-filer eftersom mindre information behövs för att beskriva RGB-bilder än för att beskriva CMYK-bilder. Eftersom RGB är den inbyggda färgrymden som används på bildskärmar behövs ingen färgkonvertering vid visning, vilket gör att du snabbt kan visa bilden på webben. Det här alternativet rekommenderas om PDF-filen ska användas online eller med skrivare med låg upplösning.

**Dokumentåtergivningsmetod:** Metoden för att mappa färger mellan färgrymder. Resultatet av en viss metod beror på färgrymdernas profiler. Vissa profiler ger till exempel identiska resultat med olika metoder. Dessa alternativ är tillgängliga:

>[!NOTE]
>
>I samtliga fall kan återgivningar ignoreras eller åsidosättas av färghanteringsåtgärder som utförs när PDF-filen har skapats.

**Bevara:** Innebär att avsikten anges i utdataenheten i stället för i PDF-filen. I många utdataenheter är Relativa färgvärden standardmetoden.

**Perceptuell:** Behåller de relativa färgvärdena bland de ursprungliga pixlarna när de mappas till målfärgomfånget. Den här metoden bevarar den visuella relationen mellan färgerna, även om själva färgvärdena kan ändras.

**Mättnad:** Behåller de relativa mättnadsvärdena för de ursprungliga pixlarna. Den här metoden lämpar sig för affärsgrafik där det inte är lika viktigt att ha en exakt relation mellan färger som en klar mättad färg.

**Relativa färgvärden:** Mappar om vitpunkten i källfärgrymden till vitpunkten i målfärgrymden.

**Absoluta färgvärden:** Inaktiverar matchning av vita och svarta punkter vid konvertering av färger. Den här metoden rekommenderas inte såvida du inte måste bevara signaturfärger, t.ex. de som används i varumärken eller logotyper.

### Arbetsytor {#working-spaces}

För alla värden i listan under Färghanteringsprofiler, utom Ändra inte färg, väljer du bland listorna i arbetsyteområdet vilka ICC-profiler som ska användas för att definiera och kalibrera färgrymderna Gråskala, RGB och CMYK i bearbetade PDF-filer. Dessa alternativ är tillgängliga:

**Grå:** Definierar färgrymden för alla gråskalebilder i filer. Det här alternativet är endast tillgängligt om du väljer Tagga allt för färghantering eller Tagga endast bilder för färghantering. ICC-standardprofilen för grå bilder är Grå gamma 2.2. Du kan också välja Ingen om du inte vill att gråskalebilder ska konverteras.

**RGB:** Definierar färgrymden för alla RGB-bilder i filer. Standardvärdet, sRGB IEC61966-2.1, är vanligtvis ett bra val eftersom det håller på att bli en branschstandard och många utdataenheter känner igen det. Du kan också välja Ingen om du inte vill att RGB-bilder ska konverteras.

**CMYK:** Definierar färgrymden för alla CMYK-bilder i filer. Standardvärdet är U.S. Web Coated (SWOP) v2. Du kan också välja Ingen om du inte vill att CMYK-bilder ska konverteras.

>[!NOTE]
>
>Om du väljer Ingen för alla tre arbetsfärgrymderna får du samma effekt som om du väljer Ändra inte färg.

**Bevara CMYK-värden för kalibrerade CMYK-färgmodeller:** När det här alternativet är markerat behandlas enhetsoberoende CMYK-värden som enhetsberoende (DeviceCMYK) värden, enhetsoberoende färgmodeller ignoreras och PDF/X-1a-filer använder värdet för Konvertera alla färger till CMYK. När det här alternativet är avmarkerat konverteras enhetsoberoende färgrymder till CMYK om färghanteringsprofilen är inställd på Konvertera alla färger till CMYK.

### Enhetsberoende data {#device-dependent-data}

De här alternativen gäller om du arbetar med dokument som har skapats med avancerade dokument- och grafikprogram, till exempel Adobe Illustrator och Adobe InDesign. Mer information finns i dokumentationen som medföljde programmet.

Överföringsfunktioner används för en konstnärlig effekt och för att anpassa efter specifikationerna för en viss utdataenhet. En fil som är avsedd för utskrift på en viss fotosättare kan t.ex. innehålla överföringsfunktioner som kompenserar för den punktförstoring som är inbyggd i skrivaren.

**Bevara underfärgsborttagning och svartfärgsgenerering:** Behåller dessa inställningar om de finns i PostScript-filen. Svartfärgsgenerering beräknar mängden svart färg som ska användas när du försöker återge en viss färg. Underfärgsborttagning minskar mängden cyan, magenta och gula komponenter för att kompensera för mängden svart som den svarta genereringen har lagt till. Eftersom det använder mindre tryckfärg används vanligtvis UCR för tidningspapper och obestruket papper.

**När överföringsfunktioner hittas:** Avgör vad som ska göras när överföringsfunktioner hittas:

**Bevara:** Bevarar överföringsfunktionerna som traditionellt används för att kompensera för punktförstoring eller punktförminskning som kan inträffa när en bild överförs till film. Punktförstoring uppstår när tryckfärgspunkter som utgör en utskriven bild är större (t.ex. på grund av spridning på papper) än i rasterskärmen. Punktförminskning uppstår när tryckpunkterna är mindre. Med det här alternativet behålls överföringsfunktionerna som en del av filen och tillämpas på filen när filen skrivs ut.

**Använd:** behåller inte överföringsfunktionen utan använder den på filen, vilket ändrar filens färger. Det här alternativet är användbart när du vill skapa färgeffekter i en fil. Som standard är det här alternativet markerat för nya inställningar.

**Ta bort:** Tar bort alla överföringsfunktioner som används. Ta bort de överföringsfunktioner som används såvida inte PDF-filen skrivs ut på samma enhet som PostScript-källfilen skapades för.

**Bevara rasterinformation:** Bevarar rasterinformation i filer. Rasterinformation består av punkter som styr hur mycket bläckhalvtonsenheter som finns på en viss plats på papperet. Genom att variera punktstorleken och densiteten skapas en illusion av variationer av grått eller kontinuerliga färger. För en CMYK-bild används fyra raster, ett för varje tryckfärg som används i utskriftsprocessen.

I traditionell tryckproduktion skapas ett raster genom att en raster placeras mellan en film och bilden och sedan exponeras för filmen. Elektroniska motsvarigheter, t.ex. i Adobe Photoshop, gör det möjligt för användare att ange rasterattribut innan de producerar film eller papper. Rasterinformation är avsedd att användas med en viss utdataenhet.

## Avancerade alternativ {#advanced-options}

De avancerade alternativen anger vilka DSC-kommentarer (Document Structuring Conventions) som ska behållas i PDF-filen och hur andra alternativ som påverkar konverteringen från PostScript ska ställas in. I en PostScript-fil innehåller DSC-kommentarer information om filen (t.ex. originalprogrammet, skapandedatum och sidorientering). De innehåller också en struktur för sidbeskrivningar i filen (t.ex. start- och slutsatser för ett prologavsnitt). DSC-kommentarer kan vara användbara när dokumentet ska skrivas ut eller tryckas. Instruktioner om hur du får åtkomst till de avancerade alternativen finns i [Lägg till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

När du arbetar med de avancerade alternativen kan det vara bra att ha en förståelse för PostScript-språket och hur det översätts till PDF. (Se [Adobe PostScript 3](https://www.adobe.com/products/postscript/main.html).)

**Tillåt att PostScript-filen åsidosätter Adobe PDF-inställningar:** Använder inställningar som lagras i en PostScript-fil i stället för den aktuella Adobe PDF-inställningsfilen. Innan du bearbetar en PostScript-fil kan du placera parametrar i filen för att kontrollera följande aspekter:

* komprimering av text och grafik
* nedsampling och kodning av provbilder
* bädda in Type 1-teckensnitt och instanser av Type 1 Multiple Master-teckensnitt

**Tillåt PostScript XObjects:** PostScript XObjects lagrar information som visas på många sidor i samma fil, till exempel en bakgrundsbild eller sidhuvud- och sidfotsinformation. Om du använder PostScript XObjects kan utskriften bli snabbare men skrivarminnet behöver mer minne. Om du vill förhindra att PostScript XObjects skapas avmarkerar du det här alternativet om du skapar PDF-filer med Acrobat 5 (PDF 1.4) eller senare kompatibilitet.

**Konvertera övertoningar till jämna toner:** Konverterar övertoningar till jämna toner för Acrobat 4 och senare, vilket gör PDF-filer mindre och eventuellt förbättrar kvaliteten på slutresultatet. PDF Generator konverterar övertoningar från Adobe Illustrator, Adobe InDesign, Adobe FreeHand MX, CorelDraw, Quark Xpress och Microsoft PowerPoint.

**Konvertera jämna linjer till kurvor:** Minskar mängden kontrollpunkter som används för att skapa kurvor i CAD-ritningar, vilket ger mindre PDF-filer och snabbare skärmåtergivning.

**Bevara Level 2 Copypage-semantik:** Använder copypage-operatorn som är definierad i LanguageLevel 2 PostScript i stället för i LanguageLevel 3 PostScript. Om du har en PostScript-fil och väljer det här alternativet kopierar en copypage-operator sidan. Om det här alternativet inte är markerat utförs motsvarigheten till en showpage-åtgärd, men grafikläget initieras inte om.

**Bevara övertryckningsinställningar:** Behåller alla övertryckningsinställningar i filer som konverteras till PDF. Övertryckta färger är två eller flera tryckfärger som skrivs ut ovanpå varandra. När en cyantryckfärg t över en gul tryckfärg blir resultatet en grön färg. Utan övertryck skrivs det underliggande gula inte ut, vilket resulterar i en cyanfärg.

**Standardvärdet för övertryck är inte noll övertryck:** Förhindrar att övertryckta objekt utan CMYK-värden blockerar CMYK-objekt som finns under dem. Effekten uppnås genom att du infogar OPM 1-grafiktillståndsparametern i PDF-filen där operatorn Setoverprint finns.

**Spara Adobe PDF-inställningar i PDF-fil:** Bäddar in inställningsfilen som används för att skapa PDF-filen. Du kan öppna och visa inställningsfilen (som har filtillägget .joboptions) i dialogrutan Bifogade filer i Acrobat. Inställningsfilen för Adobe PDF blir ett objekt i trädet EmbeddedFiles i PDF-filen.

**Spara JPEG-originalbilder i PDF om möjligt:** Bearbetar komprimerade JPEG-bilder (bilder som redan är komprimerade med DCT-kodning) utan att de komprimeras om. Om det här alternativet är markerat dekomprimeras JPEG-bilder i PDF Generator för att säkerställa att de inte är skadade. Giltiga bilder komprimeras dock inte om, vilket innebär att originalbilden inte bearbetas. När det här alternativet är markerat förbättras prestandan eftersom bara dekomprimering (inte omkomprimering) inträffar och bilddata och metadata bevaras.

**Spara Portable Job Ticket i PDF-fil:** Bevarar en PostScript-jobbbiljett i en PDF-fil. Jobbbiljetten innehåller information om PostScript-filen, t.ex. sidstorlek, upplösning och svällningsinformation, i stället för information om innehåll. Den här informationen kan användas senare i ett arbetsflöde eller för utskrift av PDF.

**Använd Prolog.ps och Epilog.ps:** Skickar en prolog- och epilogfil med varje jobb. Dessa filer har många syften. Prologgfiler kan till exempel redigeras för att ange försättsblad. Epilogue-filer kan redigeras för att lösa en rad procedurer i en PostScript-fil. Du kan överföra eller hämta filerna. (Se Överföra och hämta prolog- och epilogfiler.)

**Bearbeta DSC-kommentarer:** Underhåller DSC-information från en PostScript-fil. Dessa underalternativ är tillgängliga:

**Logga DSC-varningar:** Visar varningsmeddelanden om problematiska DSC-kommentarer under bearbetningen och lägger till dem i en loggfil.

**Bevara EPS-information från DSC:** Bevarar information, t.ex. ursprungligt program och skapandedatum för en EPS-fil. Om det här alternativet är avmarkerat ändras sidans storlek och centreras baserat på det övre vänstra hörnet av det övre vänstra objektet och det nedre högra hörnet av det nedre högra objektet på sidan.

**Bevara OPI-kommentarer:** Bevarar information som krävs för att ersätta en FPO-bild (For Placement Only) eller -kommentar med den högupplösta bilden på servrar som stöder OPI-versionerna 1.3 och 2.0 (Open Prepress Interface).

**Bevara dokumentinformation från DSC:** Bevarar information som titel, skapandedatum och tid. När du öppnar en PDF-fil i Acrobat visas den här informationen på panelen Dokumentegenskaper.

**Ändra storlek på sida och centrera teckningar för EPS-filer:** Centrerar en EPS-bild och ändrar storleken på sidan så att den passar bilden. Det här alternativet gäller endast jobb som består av en enda EPS-fil.

## Standardrapportering och regelefterlevnadsalternativ {#standards-reporting-and-compliance-options}

PDF Generator kan kontrollera dokumentinnehållet i en PostScript-fil för att säkerställa att de uppfyller standardvillkoren för PDF/X-1a, PDF/X-3 eller PDF/A innan PDF-filen skapas. För PDF/X-kompatibla filer kan du även kräva att PostScript-filen uppfyller ytterligare kriterier genom att välja andra alternativ under&quot;Standardsrapportering och efterlevnad&quot;. Vilka alternativ som är tillgängliga beror på vilken standard du väljer.

PDF/X-kompatibla filer används främst som ett standardiserat format för utbyte av PDF-filer som är avsedda för högupplöst tryckproduktion. Om du inte skapar ett PDF-dokument för tryckproduktion kan du ignorera PDF/X-standarderna.

PDF/A-kompatibla filer används främst för arkivering. Eftersom långsiktig arkivering är målet får dokumentet bara innehålla det som behövs för att öppna och visa dokumentet under dokumentets avsedda livstid. PDF/A-kompatibla filer kan till exempel bara innehålla text, rasterbilder och vektorobjekt. De kan inte innehålla kryptering och skript. Dessutom måste alla teckensnitt bäddas in så att dokumenten kan öppnas och visas som de skapats. Med andra ord är PDF/A-kompatibla dokument *tunnare* än sina PDF/X-motsvarigheter, som är avsedda för avancerad produktion.

>[!NOTE]
>
>Om du konfigurerar en bevakad mapp för att skapa PDF/A-kompatibla filer måste du se till att du inte lägger till skydd i mappen. PDF/A-standarden tillåter inte kryptering.

Instruktioner om hur du får åtkomst till standardrapportering och kompatibilitetsalternativ finns i [Lägg till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

**Kompatibilitetsstandard:** Välj en standard för att skapa en rapport som anger om filen uppfyller kraven och, om inte, vilka problem som påträffades. När kompatibiliteten på sidan Allmänna inställningar är inställd på Acrobat 4.0 är följande alternativ aktiverade. Om kompatibiliteten är inställd på Acrobat 5.0 är endast alternativen i Acrobat 5.0 tillgängliga. När Kompatibilitet är inställt på ett alternativt alternativ är följande alternativ nedtonade:

* PDF/X-1a (Acrobat 4.0-kompatibel)
* PDF/X-3 (Acrobat 4.0-kompatibel)
* PDF/X-1a (Acrobat 5.0-kompatibel)
* PDF/X-3 (Acrobat 5.0-kompatibel)
* PDF/A-1b (Acrobat 5.0-kompatibel)

### Alternativ för PDF/X-standarder {#options-for-pdf-x-standards}

**När inte kompatibel:** Anger om PDF-filen ska skapas om PostScript-filen inte uppfyller PDF/X-kraven. Det här alternativet är tillgängligt när kompatibilitetsstandard på sidan Standarderapporter och Efterlevnad är inställd på ett annat alternativ än Ingen.

**Fortsätt:** Skapar en PDF-fil.

**Avbryt jobb:** Skapar bara en PDF-fil om PostScript-filen uppfyller PDF/X-kraven för de valda rapportalternativen och i övrigt är giltig. Om båda rapportalternativen för PDF/X har valts och PostScript-filen bara uppfyller en uppsättning av PDF/X-kriterierna (till exempel PDF/X-3) skapar PDF Generator den kompatibla filen.

**Om varken TrimBox eller ArtBox har angetts:** Tillgängligt när kompatibilitetsstandard på sidan Standarderapporter och Efterlevnad har angetts till ett annat alternativ än Ingen.

**Rapportera som fel:** Flaggar PostScript-filen som inkompatibel om något av rapportalternativen har valts och en TrimBox eller ArtBox saknas på någon sida.

**Ange TrimBox efter MediaBox med förskjutningar:** Beräknar värden i punkter för TrimBox baserat på förskjutningarna för MediaBox för respektive sida om varken TrimBox eller ArtBox har angetts. TrimBox är alltid lika liten eller mindre än den omgivande medierutan.

**Om BleedBox inte har angetts:** Tillgängligt när kompatibilitetsstandard på sidan Standarderapporter och Regelefterlevnad har angetts till ett annat alternativ än Ingen.

**Ange BleedBox efter MediaBox:** Använder MediaBox-värden för BleedBox om BleedBox inte har angetts.

**Ange BleedBox efter TrimBox med förskjutningar:** Beräknar värden i punkter för BleedBox baserat på förskjutningarna för TrimBox på respektive sida om BleedBox inte har angetts. BleedBox är alltid lika stor eller större än den inneslutna TrimBox.

**Standardvärden om de inte har angetts i dokumentet:** Det här alternativet är tillgängligt när kompatibilitetsstandard på sidan Standarderapporter och kompatibilitetssida har angetts till ett annat alternativ än Ingen.

**Profilnamn för utdatametod:** Anger det karakteriserade utskriftsvillkoret som dokumentet förbereds för. Om ett dokument inte anger något OutputIntent-namn används det valda värdet på den här menyn i PDF Generator. Du kan välja ett av namnen som anges eller ange ett namn i det angivna utrymmet. Om arbetsflödet kräver att utdatametoden anges i dokumentet väljer du Ingen. Dokument som inte uppfyller kraven klarar inte kompatibilitetskontrollen.

**Identifierare för utdatavillkor:** Anger det referensnamn som anges av registret för profilnamnet för utdatametoden.

**Utdatavillkor:** Beskriver det avsedda utskriftsvillkoret. Den här posten kan vara användbar för mottagaren av PDF-dokumentet.

**Registernamn (URL):** Anger webbadressen för mer information om registret. URL:en anges automatiskt för ICC-registernamn.

**Svälld:** Anger dokumentets svällningsstatus. PDF/X-kompatibilitet kräver värdet True eller False. Om svällningsläget inte anges i dokumentet används det värde som anges här. Om arbetsflödet kräver att svällningsläget anges i dokumentet väljer du Lämna odefinierad. Dokument som inte uppfyller kraven klarar inte kompatibilitetskontrollen.

### Alternativ för PDF/A-standard {#options-for-pdf-a-standard}

Dessa alternativ aktiveras när kompatibiliteten (under Allmänt) är inställd på Acrobat 4 (PDF 1.3) eller Acrobat 5 (PDF 1.4).

**När inte kompatibel:** Anger om PDF-filen ska skapas om PostScript-filen inte uppfyller PDF/A-kraven.

**Fortsätt:** Skapar en PDF-fil även om PostScript-filen inte uppfyller standardkraven.

**Avbryt jobb:** Skapar bara en PDF-fil om PostScript-filen uppfyller PDF/A-kraven och i övrigt är giltig.

**Profilnamn för utdatametod:** Anger det karakteriserade utskriftsvillkor som dokumentet har förberetts för och som krävs för PDF/A-kompatibilitet. Om arbetsflödet kräver att dokumentet anger information om utdatametod väljer du Ingen. Dokumentet kommer inte att klara kompatibilitetskontrollen om den här informationen inte anges.

**Utdatavillkor:** Beskriver det avsedda utskriftsvillkoret. Denna post är inte obligatorisk, men kan användas för att ge användbar information till den avsedda mottagaren av PDF-dokumentet.

## Alternativ för inledande vy {#initial-view-options}

Dessa alternativ är ordnade i tre områden: Dokumentalternativ, Fönsteralternativ och Alternativ för användargränssnitt. Instruktioner om hur du får åtkomst till alternativen för den inledande vyn finns i [Lägg till eller redigera PDF-inställningar](configuring-pdf-settings.md#add-or-edit-pdf-settings).

Om du vill använda något av alternativen väljer du Ange inställningar för inledande vy.

### Dokumentalternativ {#document-options}

Dokumentalternativen styr utseendet på dokumentet i dokumentfönstret, t.ex. förstoringsnivån och hur det rullas.

**Visa:** Avgör vilka rutor och flikar som visas i programfönstret som standard. Panelen Bokmärken och sidan öppnar dokumentfönstret och fliken Bokmärken visas.

**Sidlayout:** Avgör om dokumentet ska visas på en sida, motstående sida, löpande sida eller kontinuerligt motstående sida.

**Förstoring:** Anger den zoomnivå som används för att visa dokumentet när det öppnas. I standardinställningen används det användarkonfigurerade förstoringsvärdet i inställningarna för Acrobat eller Adobe Reader.

**Öppna på sidnummer:** Anger sidan som dokumentet öppnas på, vilket vanligtvis är sidan 1.

>[!NOTE]
>
>Om du anger Standard för förstorings- och sidlayoutalternativen används de enskilda användarinställningarna i inställningarna för sidvisning i Acrobat eller Adobe Reader.

### Fönsteralternativ {#window-options}

Fönsteralternativen avgör hur fönstret justeras i skärmområdet när en användare öppnar dokumentet. Alternativen har dock ingen effekt när ett PDF-dokument visas i en webbläsare.

**Ändra storlek på fönster till startsida:** Justerar dokumentfönstret så att det passar runt öppningssidan, enligt alternativen som du valde under Dokumentalternativ.

**Centrera fönster på skärmen:** Placerar fönstret i mitten av skärmområdet.

**Öppna i helskärmsläge:** Maximerar dokumentfönstret och visar dokumentet utan menyrad, verktygsfält eller fönsterkontroller.

**Visa:** Filnamnet visar filnamnet i fönstrets namnlist. Dokumenttiteln visar dokumentets titel i fönstrets namnlist.

### Alternativ för användargränssnitt {#user-interface-options}

Alternativen för användargränssnittet avgör vilka kontroller som visas eller döljs när användaren öppnar dokumentet.

**Dölj menyrad:** Om du väljer det här alternativet döljs menyraden

**Dölj verktygsfält:** Om du väljer det här alternativet döljs verktygsfälten

**Dölj fönsterkontroller:** Om det här alternativet är markerat döljs fönsterkontrollerna

>[!NOTE]
>
>Om du döljer menyraden och verktygsfältet kan användare inte använda kommandon och markeringsverktyg om de inte känner till kortkommandona när de öppnar filen i Acrobat.

## Överföra och ladda ned prolog- och epilogfiler {#uploading-and-downloading-prologue-and-epilogue-files}

Prologgfiler används för att lägga till anpassad PostScript-kod som körs i början av alla PostScript-jobb som bearbetas. Epilogue-filer används för att lägga till anpassad PostScript-kod som körs i slutet av varje PostScript-jobb. Du kan hämta prolog- och epilogfiler från servern för att spara dem lokalt. Du kan hämta filerna för att konfigurera dem oberoende av varandra eller för att överföra dem till en annan plats eller till en annan dator.

Dessa filer har många syften. Prologgfiler kan t.ex. redigeras för att ange försättsblad. Du kan redigera epilogfiler för att lösa en rad procedurer i en PostScript-fil. Du kan också välja och överföra de prolog- och epilogfiler som ska skickas med varje jobb.

### Ladda ned en prolog- eller epilogfil {#download-a-prologue-or-epilogue-file}

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Adobe PDF-inställningar.
1. Klicka på Ny eller klicka på namnet på en inställning.
1. Klicka på Avancerat och klicka sedan på Hämta bredvid alternativet Använd Prolog.ps och Epilogue.ps.
1. Klicka på Prolog.ps eller Epilogue.ps på sidan Hämta prolog- och e-postfiler och klicka på Spara.

### Överföra en prolog- eller epilogfil {#upload-a-prologue-or-epilogue-file}

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Adobe PDF-inställningar.
1. Klicka på Ny eller klicka på namnet på en inställning.
1. Klicka på Avancerat och klicka sedan på Överför bredvid alternativet Använd Prolog.ps och Epilogue.ps.
1. Klicka på Bläddra på sidan Överför prologg och e-postfiler för att välja en prologfil eller en epilogfil.
1. Leta reda på filen och klicka på Öppna.
1. Om du vill använda filen måste du se till att Använd Prolog.ps och Epilogue.ps är markerat under Avancerat på sidan Ny/redigera Adobe PDF-inställning.
1. Klicka på Spara

>[!NOTE]
>
>PDF Generator stöder endast prolog- och epilogfiler för konvertering av PostScript- och Encapsulated PostScript-filer till PDF.
