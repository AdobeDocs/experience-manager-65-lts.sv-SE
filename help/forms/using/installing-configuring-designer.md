---
title: Installera och konfigurera Designer
description: Designer finns som fristående installationsprogram och medföljer också Workbench. Lär dig hur du installerar fristående Designer.
role: Admin, User, Developer
feature: Forms Designer,Designer
solution: Experience Manager, Experience Manager Forms
exl-id: 526bbc59-62c3-4e6d-a938-e368d07fe6b0
source-git-commit: 060bb23d64a90f0b2da487ead4c672cbf471c9a8
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# Installera och konfigurera Designer{#installing-and-configuring-designer}

## Krav {#pre-requisites}

+++ För 64-bitars AEM Forms Designer (rekommenderas)

* Installera 64-bitarsversionen av [Visual C++ 2019 Redistributable (x64)](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170). Kontrollera att tidigare nämnda omdistribuerbara körtidspaket är installerade innan du startar installationen.
* En användare med administratörsbehörighet för att installera eller avinstallera AEM Forms Designer.

+++

+++ För 32-bitars AEM Forms Designer

* Installera 32-bitarsversionen av [Visual C++ 2019 Redistributable (x64)](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170). Kontrollera att tidigare nämnda omdistribuerbara körtidspaket är installerade innan du startar installationen.
* En användare med administratörsbehörighet för att installera eller avinstallera AEM Forms Designer.

+++

>[!NOTE]
>
>* 64-bitarsversionen av designern introducerades med AEM 6.5 Forms Service Pack 19 (6.5.19.0).
>* 32-bitarsversionen av designern har tagits bort sedan [AEM Forms Service Pack 21 (6.5.21.0)](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases) släpptes.
> * De plattformar som stöds för Forms Designer är anpassade efter de plattformar som stöds av AEM Forms. [Klicka här](/help/sites-deploying/technical-requirements.md) om du vill veta mer om vilka plattformar som stöds för Forms Designer

Mer information om hur du installerar Forms Designer finns på [Frågor och svar](#fandq).

## Installera AEM Forms Designer {#install-designer}

Designer finns som fristående installationsprogram och medföljer också WorkBench. Om du använder ett fristående installationsprogram för AEM Forms Designer utför du följande steg:

1. Avinstallera den tidigare versionen av AEM Forms Designer, om den redan är installerad.
1. Hämta 64-bitars AEM Forms Designer (rekommenderas) eller 32-bitars AEM Forms Designer beroende på dina behov.

   >[!NOTE]
   > 
   >* 32-bitars Forms Designer är schemalagd att fasas ut i version 20 (6.5.20.0) av AEM 6.5 Forms Service Pack 20. Adobe rekommenderar att du uppgraderar till 64-bitars Forms Designer.
   >* 64-bitars Forms Designer är endast tillgängligt för AEM 6.5 Forms Service Pack 19 (6.5.19.0) eller senare versioner.
   >* Adobe Experience Manager 6.5 Forms Service Pack 15 (6.5.15.0) och Forms Designer-versionen innehåller även Service Pack-versionen. Exempel: för Service Pack 15 är versionsnumret 6.5.15.2022112.1.0. I det här exemplet är 6.5.15 Service Pack-version.

1. Starta installationsprogrammet för AEM Forms Designer genom att dubbelklicka på setup.exe.
1. Fortsätt och ange dina uppgifter och serienumret på Personalization-skärmen.

   >[!NOTE]
   >
   >* Hämta din licensnyckel för Forms Designer från [Adobe licenswebbplats](https://licensing.adobe.com/).

1. Om du godkänner licensavtalet fortsätter du genom att klicka på Nästa.
1. (Valfritt) ändra standardinstallationssökvägen om du vill installera Designer på en valfri plats. Klicka på Nästa.
1. Klicka på Föregående om du vill ändra några inställningar. Klicka på Installera om du vill installera Designer.
1. Klicka på Slutför när installationen är klar.

Du kan också installera AEM Forms Designer via kommandoraden i passivt eller tyst läge.

* Passiv installation via kommandoraden: Installationsprogrammet visar en förloppsindikator som anger att installationen pågår, men inga meddelanden eller felmeddelanden visas. När du har startat programmet kan du inte avbryta installationen.

```shell
msiexec /i "<absolute path>\Designer.msi" /passive SERIALNUMBER=****-****-****-****-****-****
```

* Tyst kommandoradsinstallation: Installationsprogrammet kör installationen utan att visa ett användargränssnitt. Inga uppmaningar, meddelanden eller dialogrutor visas. När du har startat programmet kan du inte avbryta installationen.

```shell
msiexec /i "<absolute path>\Designer.msi" /quiet SERIALNUMBER=****-****-****-****-****-****
```

## Uppdatera AEM Forms Designer {#update-forms-designer}

Det finns två fall när den senaste versionen av AEM Forms Designer 6.5.16.0 uppdateras:

* **Fall 1**: När användaren har en tidigare version av AEM Forms Designer än 6.5.15.0.
* **Fall 2**: När användaren har 6.5.15.0 AEM Forms Designer-version.

+++**När användaren har en tidigare version av AEM Forms Designer än 6.5.15.0.**

Om du använder ett fristående installationsprogram för AEM Forms Designer utför du följande steg:

1. Innan du installerar **AEM Forms Designer6.5.16.0** måste användare avinstallera tidigare versioner.
1. Hämta och installera [AEM Forms Designer 6.5.15.0](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html) från AEM formulärversionssida.
1. När installationen av **AEM Forms Designer6.5.15.0** har slutförts hämtar och installerar du [ AEM Forms Designer 6.5.16.0](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html) genom att dubbelklicka på den hämtade installationsfilen.

+++

+++**När användaren har 6.5.15.0 AEM Forms Designer version**

Om du använder ett fristående installationsprogram för AEM Forms Designer utför du följande steg:
1. Hämta den senaste versionen av AEM Forms Designer från [programdistributionsportalen](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html).
1. Installera den senaste versionen av AEM Forms Designer genom att dubbelklicka på den hämtade installationsfilen.

+++

## Frågor och svar {#fandq}

* **Kan en användare uppgradera eller installera 64-bitars designer direkt?**
   * Ja, man kan uppgradera eller installera 64-bitarsdesignern direkt. Installera det fullständiga installationsprogrammet för [SP19](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack/fd/Designer-Patch/sp19_x64/aemforms_designer_6_5_0_wwe_win.zip)-designern och använd den efterföljande designer-korrigeringsversionen.

     >[!NOTE]
     > Innan du uppgraderar till 64-bitars designer måste du först avinstallera 32-bitars designer om det finns.

* **Kan användare ha både 32-bitars och 64-bitars installerat på datorn?**
   * Nej, 32- och 64-bitarsinstallationer fungerar inte på samma dator. Användaren kan antingen ha en 32-bitars designer eller en 64-bitars designer.

* **Hur kontrollerar man om en användare arbetar med 64-bitars designer eller 32-bitars designer?**
   * Det finns två sätt att kontrollera Forms Designer:

      1. Öppna Designer, gå till Hjälp, klicka på Om designer så visas versionsinformation för designer tillsammans med bitinformationen. 64-bitarsversionen visas i slutet av den version som visas här:
         `6.5.21.20240522.1.161 | 64 bit`
      1. Öppna Designer. Överst till vänster ser du en ikon med 64-bitarsinformation med produktnamnet.
