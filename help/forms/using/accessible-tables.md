---
title: Skapa tillgängliga komplexa tabeller i HTML5-formulär
description: Lär dig hur du skapar tillgängliga tabeller i HTML5-formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
discoiquuid: 3504afe1-abf5-4fbf-a0d2-e093361764bd
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: e6e6c08a-3bed-4713-a0e0-2a02607c7fc7
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Skapa tillgängliga komplexa tabeller i HTML5-formulär {#create-accessible-complex-tables-in-html-forms}

Standardimplementeringen av tabeller i HTML5 Forms använder HTML DIV-element för att återge en tabell. Återgivning innebär att ARIA-roller används för att uppfylla tillgänglighetskraven.

För att undvika tillgänglighetsproblem med skärmläsare som inte har fullt stöd för de ARIA-roller som används med datatabeller, erbjuder HTML5 Forms en alternativ återgivning för tabellerna. Tabellerna är baserade på det nya tabellformatet som introducerades i Designer och som även stöder:

* Radrubriker
* Radomfång

Om du vill använda det nya formatet i HTML5 Forms markerar du tabellen som komplex. Om du vill markera tabellen som komplex lägger du till taggen `extras` i XML-källan för tabelldelformuläret enligt följande:

```xml
</extras>
 <text name="complexTable">1</text>
 </extras>
```

Tabellerna som markeras som *complexTable* följer den inbyggda HTML-renderingen och ger bättre hjälpmedelsstöd för vissa skärmläsare.  Om du vill skapa ett radintervall markerar du celler i följd i en tabell i samma kolumn, högerklickar på markeringen och klickar sedan på **[!UICONTROL Merge Cells]**.

>[!NOTE]
>
>Det går bara att skapa ett radintervall för celler längst till vänster.

Om du vill markera en rad som radhuvud markerar du alla celler i raden, högerklickar på markeringen och klickar sedan på **[!UICONTROL Mark Header]**.

Om du vill markera en cell som kolumnrubrik markerar du en cell i kolumnen, högerklickar på markeringen och klickar sedan på **[!UICONTROL Mark Header]**.

Begränsningar i det nya formatet *AccessibleTable*:

* Brist på stöd för utökningsbara fält om rowspan används i tabellen
* Inget stöd för kapslade tabeller (tabeller i tabellceller)
* Stödet för radutvidgning är begränsat till rubrikrader och rubrikceller
* Stödet begränsas till vanliga tabeller
* Inget stöd för dataförifyllningar i tabeller med radutvidgning > 1
