---
title: Exportera till CSV
description: Exportera information om sidorna till en CSV-fil på det lokala systemet
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: ccd2ad37-7708-4422-9724-145628f36afc
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 6%

---

# Exportera till CSV{#export-to-csv}

Med **Skapa CSV-rapport** kan du exportera information om dina sidor till en CSV-fil på ditt lokala system.

* Den hämtade filen kallas `export.csv`
* Innehållet beror på de egenskaper du väljer.
* Du kan definiera banan tillsammans med exportens djup.

>[!NOTE]
>
>Hämtningsfunktionen och standarddestinationen för webbläsaren används.

Med guiden **Skapa CSV-export** kan du välja:

* Egenskaper att exportera
   * Metadata
      * Namn
      * Ändrad
      * Publicerad
      * Mall
      * Arbetsflöde
   * Översättning
      * Översatt
   * Analyser
      * Sidvyer
      * Unika besökare
      * Tid på sidan
* Djup
   * Överordnad sökväg
   * Endast direktunderordnade
   * Ytterligare nivåer av barn
   * Nivåer

Den resulterande `export.csv`-filen kan öppnas i Excel eller något annat kompatibelt program.

![etc-01](assets/etc-01.png)

Alternativet **Skapa CSV-rapport** är tillgängligt när du bläddrar i konsolen **Platser** (i listvyn): det är ett alternativ i den nedrullningsbara menyn **Skapa**:

![etc-02](assets/etc-02.png)

Så här skapar du en CSV-export:

1. Öppna konsolen **Platser** och navigera till önskad plats om det behövs.
1. I verktygsfältet väljer du **Skapa** och sedan **CSV-rapport** för att öppna guiden:

   ![etc-03](assets/etc-03.png)

1. Välj de egenskaper som ska exporteras.
1. Välj **Skapa**.
