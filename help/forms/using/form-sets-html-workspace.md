---
title: Arbeta med formulär i AEM Forms arbetsyta
description: Ett formulär är en samling HTML5-formulär som grupperas och presenteras som en enda formuläruppsättning för slutanvändarna. Lär dig hur du kan arbeta med formatuppsättningar i AEM Forms arbetsyta.
contentOwner: vishgupt
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: User, Developer
exl-id: 4b2c994e-8ca6-42fc-a8b2-96c53e9f9453
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Arbeta med formulär i AEM Forms arbetsyta{#working-with-formsets-in-aem-forms-workspace}

Ett formulär är en samling HTML5-formulär som grupperas och presenteras som en enda formuläruppsättning för slutanvändarna. När slutanvändarna börjar fylla i en formuläruppsättning, överförs de smidigt från ett formulär till ett annat. Formuläruppsättningen kan sedan skickas med bara ett klick. Mer information om formuläruppsättningar och hur du konfigurerar dem finns i [Formuppsättning i AEM Forms](../../forms/using/formset-in-aem-forms.md).

AEM Forms arbetsyta stöder formatuppsättningar. Med formuläruppsättningar kan flera formulär som är kopplade till en tjänst eller process grupperas för att automatisera en affärsprocess och presenteras för slutanvändarna. I ett sådant scenario kan användarna fylla i hela uppsättningen som ett och det finns inget behov av att arkivera, skicka och spåra enskilda formulär eller processer.

## Koppla en formuläruppsättning till startpunkten i en AEM Forms-arbetsyteapp {#attaching-a-formset-to-startpoint-in-an-aem-forms-workspace-app-br}

1. Skapa affärsprocessarbetsflödet i Workbench. Mer information finns i [Workbench-hjälpen](https://www.adobe.com/go/learn_aemforms_workbench_63).
1. Välj **Använd en CRX-resurs** i Presentation &amp; Data i processegenskaperna för startpunkten.

   ![1-3](assets/1-3.png)

1. Klicka på ![Bläddra](assets/browse.png) (Bläddra) bredvid CRX resurssökväg. Dialogrutan Välj formulärresurs visas.

   ![2-1](assets/2-1.png)

1. Klicka på fliken **Formuppsättning**, markera den relevanta formuppsättningen i listan och klicka sedan på **OK**.

1. Distribuera programmet efter uppdatering av andra relevanta processegenskaper.

## Använda formatuppsättningar på arbetsytan i AEM Forms {#using-formset-in-nbsp-aem-forms-workspace}

När en formuläruppsättning är kopplad till en startpunkt kan startpunkten anropas från arbetsytan i AEM Forms, precis som vilken startpunkt som helst.

De åtgärder som stöds för formatuppsättningar via arbetsytan i AEM Forms är:

* Spara som utkast
* Lås
* Abandon
* Skicka
* Lägg till bifogade filer
* Lägg till anteckningar
* Flytta mellan formulär i en formuläruppsättning med knapparna Bakåt eller Nästa

![3-1](assets/3-1.png)

>[!NOTE]
>
>Om du vill förbättra prestandan när du flyttar från föregående och nästa formulär i en formuläruppsättning inaktiveras alla arbetsyteknappar (Bakåt, Nästa, Spara, Skicka och ... (Mer)) tills det aktuella formuläret återges fullständigt.
