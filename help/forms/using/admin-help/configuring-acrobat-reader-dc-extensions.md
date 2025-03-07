---
title: Konfigurera Acrobat Reader DC-tillägg för datainhämtning
description: Lär dig hur du konfigurerar Acrobat Reader DC-tillägg för datainhämtning.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_acrobat_reader_dc_extensions
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms,Document Services,Reader Extensions
hide: true
hidefromtoc: true
exl-id: f9b01de7-1de5-43aa-bcc3-b15719bfa5c0
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Konfigurera Acrobat Reader DC-tillägg för datainhämtning {#configuring-acrobat-reader-dc-extensions-for-data-capture}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Om användare av dina AEM-formulär använder datainhämtningsfunktionen i Content Services (Borttagen) rekommenderar vi att du skapar en roll med skrivskyddad åtkomst för dessa användare.

***Obs!**Adobe® LiveCycle® Content Services ES (utgått) är ett innehållshanteringssystem som installeras med LiveCycle. Det gör det möjligt för användarna att utforma, hantera, övervaka och optimera humancentrerade processer. Supporten för innehållstjänster (borttaget) upphör 2014-12-31. Se [Adobe livscykeldokument](https://helpx.adobe.com/support/programs/eol-matrix.html).*

För datainhämtning krävs att du tilldelar en användarroll för att få åtkomst till SampleReaderExtensionsCredential. Du kan tilldela den förvalda rollen Pålitlighetsadministratör. Tänk dock på att den här rollen ger allmänna, icke-administrativa administratörsbehörigheter som styr PKI Trust-inställningarna och hanterar PKI-autentiseringsuppgifter, vilket kan äventyra säkerheten i dina AEM-formulär i en produktionsmiljö. Vi rekommenderar att systemadministratören för AEM-formulär skapar en roll som endast ger skrivskyddad åtkomst till Trust Store och tilldelar den nya rollen till icke-administratörsanvändare som använder datainhämtning.

## Skapa en roll för datainhämtningsanvändare {#create-a-role-for-data-capture-users}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Rollhantering och sedan på Ny roll.
1. Ange rollnamnet (t.ex. Datainsamlingsanvändare) och beskrivningen i fälten och klicka sedan på Nästa.
1. På skärmen Rollbehörigheter klickar du på Sök behörigheter och väljer sedan Läs autentiseringsuppgifter i listan över tillgängliga behörigheter.
1. Klicka på OK och sedan på Slutför.

## Tilldela datainhämtningsrollen {#assign-the-data-capture-role}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Rollhantering och sedan på Sök.
1. Klicka på användarrollen för datainhämtning som du skapade.
1. Klicka på Sök efter användare/grupper på fliken Rollanvändare/grupper.
1. På skärmen Sök efter användare och grupper klickar du på Sök, väljer de användare som behöver användarrollen för datainhämtning och klickar sedan på OK.
1. Klicka på Spara på skärmen Redigera roll.
