---
title: Skapa en språkrot med det klassiska användargränssnittet
description: Lär dig hur du skapar en språkrot i Adobe Experience Manager med det klassiska användargränssnittet.
contentOwner: Guillaume Carlino
feature: Language Copy
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: c6e00da5-804f-46cf-b7a9-52e667574394
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Skapa en språkrot med det klassiska användargränssnittet{#creating-a-language-root-using-the-classic-ui}

I följande procedur används det klassiska användargränssnittet för att skapa en språkrot för en plats. Mer information finns i [Skapa en språkrot](/help/sites-administering/tc-prep.md#creating-a-language-root).

1. I webbplatskonsolen väljer du webbplatsens rotsida i webbplatsträdet. ([http://localhost:4502/siteadmin#](http://localhost:4502/siteadmin#))
1. Lägg till en ny underordnad sida som representerar webbplatsens språkversion:

   1. Klicka på Ny > Ny sida.
   1. Ange titel och namn i dialogrutan. Namnet måste ha formatet `<language-code>` eller `<language-code>_<country-code>`, till exempel en, en_US, en_us, en_GB, en_gb.

      * Den språkkod som stöds är en kod med två bokstäver och gemener enligt ISO-639-1
      * Den landskod som stöds är gemen eller versal, tvåbokstavskod enligt ISO 3166

   1. Markera mallen och klicka på Skapa.

   ![newpagefr](assets/newpagefr.png)

1. I webbplatskonsolen väljer du webbplatsens rotsida i webbplatsträdet.
1. Välj Språkkopia på Verktyg-menyn.

   ![toolslanguagecopy](assets/toolslanguagecopy.png)

   I dialogrutan Språkkopia visas en matris med tillgängliga språkversioner och webbsidor. Ett x i en språkkolumn betyder att sidan är tillgänglig på det språket.

   ![languageDialog](assets/languagecopydialog.png)

1. Om du vill kopiera en befintlig sida eller ett befintligt sidträd till en språkversion, markerar du cellen för sidan i språkkolumnen. Klicka på pilen och välj den typ av kopia som ska skapas.

   I följande exempel kopieras sidan för utrustning/solglasögon/irian till den franska språkversionen.

   ![languagePodDilogListruta](assets/languagecopydilogdropdown.png)

   | Typ av språkkopia | Beskrivning |
   |---|---|
   | auto | Använder beteendet från överordnade sidor |
   | ignorera | Skapar inte en kopia av den här sidan och dess underordnade sidor |
   | `<language>+` (t.ex. franska+) | Kopierar sidan och alla underordnade sidor från det språket |
   | `<language>` (till exempel franska) | Kopierar endast sidan från det språket |

1. Stäng dialogrutan genom att klicka på OK.
1. Klicka på Ja i nästa dialogruta för att bekräfta kopian.
