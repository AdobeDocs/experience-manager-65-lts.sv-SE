---
title: Använd layoutläget för att ändra storlek på komponenter för interaktiv kommunikation
description: Definiera placeringen av komponenterna med det responsiva stödrastret som finns i layoutläget
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 39339f53-be4f-46a0-8c39-fd56a7f7e770
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '854'
ht-degree: 0%

---

# Använd layoutläget för att ändra storlek på komponenter {#use-layout-mode-to-resize-components}

Med gränssnittet Interactive Communication Web Channel kan du ändra storlek på komponenter i layoutläget. Dra och släpp blå punkter i kolumner för att definiera start- och slutpunkterna för placering av komponenter. De blå punkterna visas när du har tryckt på komponenten i det responsiva rutnätet. Det responsiva rutnätet består av 12 lika stora kolumner. Den vita och blå färgskuggningen i alternativa kolumner skiljer den ena kolumnen från den andra.

Du kan använda layoutläget för att ändra storlek på komponenter för alla enhetstyper, som stationära datorer, surfplattor, telefoner och andra mindre enheter. Tabletten hämtar automatiskt layoutkonfigurationen från skrivbordsversionen och de mindre enheterna hämtar layoutkonfigurationen från telefonen. Du kan dock åsidosätta de automatiskt härledda konfigurationerna för att definiera olika konfigurationer för varje enhetstyp.

>[!NOTE]
>
>Om du skapar webbkanalen med [Utskriftskanalen som master](../../forms/using/create-interactive-communication.md) för en interaktiv kommunikation, inkluderar de komponenter som är tillgängliga för storleksändring även de delformulär och fält som genereras automatiskt i webbkanalen med hjälp av utskriftskanalen. Webbkanalen behåller layouten för Print channel-elementen i layoutläge.

## Åtkomst till layoutläge {#access-layout-mode}

Välj **Layout** i listrutan som visas högst upp i redigeringsgränssnittet för interaktiv kommunikation bredvid alternativet **Förhandsgranska** . Formuläret visas i layoutläget.

1. Logga in på AEM författarinstans och gå till **Adobe Experience Manager** > **Forms** > **Forms &amp; Documents**.
1. Skapa en [interaktiv kommunikation](../../forms/using/create-interactive-communication.md) eller öppna en befintlig.
1. Välj **Layout** i listrutan som visas högst upp bredvid alternativet **Förhandsvisa** . Formuläret visas i layoutläget.

   ![Layoutläge för interaktiv kommunikation](assets/layout_mode_ic_new.png)

## Ändra storlek på komponenter {#resize-components}

1. Markera den komponent som du vill ändra storlek på i layoutläget. De blå punkterna visas i början och slutet av det responsiva rutnätet.
1. Dra och släpp de blå punkterna för att definiera placeringen av komponenten i det responsiva rutnätet.

   ![Ändra storlek i layoutläge](assets/layout_mode_resize_new_updated.png)

   Verktygsfältet som visas när du har tryckt på komponenter består av följande alternativ:

   * **Överordnad:** Markera den överordnade komponenten för en komponent.
   * **Flyt till ny rad:** Skifta komponenten till nästa rad om det finns flera komponenter inom samma rad.

   Du kan ångra alla storleksändringar och använda standardlayout på panelen som innehåller storleksändrade komponenter med alternativet **[!UICONTROL Revert breakpoint layout]** ( ![Återställ brytpunkt &#x200B;](assets/reverttopreviouslypublishedversion.png)). Markera den överordnade komponenten för den storleksändrade komponenten för att visa alternativet.

   >[!NOTE]
   >
   >Du kan inte ändra storlek på tabellkolumn, verktygsfält, verktygsfältsknapp och målområdeskomponenter i layoutläget. Använd stilläget om du vill ändra storlek på de här komponenterna.

### Exempel {#example}

**Mål:** Du vill infoga en tabellkomponent och en bildkomponent och placera dem parallellt med varandra i ett interaktivt meddelande.

1. Infoga tabell- och bildkomponenterna i redigeringsläget i webbkanalen i en interaktiv kommunikation. Bildkomponenten visas efter tabellkomponenten.
1. Växla till layoutläge och markera tabellkomponenten. De blå punkter som komponentvisningen ska ändra storlek på i kolumnerna 1 och 12.
1. Dra och släpp den blå punkten i kolumn 12 till kolumn 6 i det responsiva rutnätet.

   ![Definiera tabellens slutpunkt](assets/layout_mode_end_point_table_new.png)

1. Markera på samma sätt bildkomponenten och dra och släpp den blå punkten i kolumn 1 till kolumn 7 i det responsiva rutnätet. Tabellen och bildkomponenterna visas parallellt med varandra.

   ![Tabell och bilden parallellt i layoutläge](assets/table_image_parallel_new.png)

   Du kan markera bildkomponenten och välja alternativet **Flyt till ny rad** i verktygsfältet för att flytta bildkomponenten till nästa rad.

## Ändra storlek på paneler {#resize-panels-layout-mode}

Utför följande steg om du vill ändra storlek på hela panelen i stället för enskilda komponenter:

1. Markera någon av komponenterna på panelen som du vill ändra storlek på, välj ![Markera överordnad](assets/select_parent_icon.svg) och välj det första alternativet i listrutan, om panelen är komponentens omedelbara överordnade.

   De blå punkterna visas i början och slutet av det responsiva rutnätet.

1. Dra och släpp de blå punkterna för att definiera panelens position i det responsiva rutnätet.
Du kan upprepa steg 1 och 2 och välja ![Välj överordnad](assets/float_to_new_line_icon.svg) för att flytta den storleksändrade panelen till nästa rad.

## Definiera layout med flera kolumner för en panel

Utför följande steg för att definiera antalet kolumner för en panel:

1. I läget **[!UICONTROL Edit]** markerar du panelen, väljer ![Konfigurera](assets/configure_icon.png) och väljer **[!UICONTROL Responsive - everything on the page without navigation]** alternativ i listrutan **[!UICONTROL Panel Layout]**.

1. Välj ![Spara](assets/save_icon.svg) om du vill spara egenskaperna.

1. I **[!UICONTROL Layout]**-läget markerar du någon av komponenterna på panelen, väljer ![Markera överordnad](assets/select_parent_icon.svg) och markerar panelen.

1. Välj ![flera kolumner](assets/multi-column.svg) och välj antalet kolumner i listrutan. Antalet kolumner kan vara mellan 1 och 12. Panelen delas upp i en layout med flera kolumner.

![flera kolumner i layoutläge](assets/multi-column-layout.png)

## Inaktivera layoutläget för formulär med gammal responsiv layout {#disable-layout-mode-for-forms-with-old-responsive-layout}

Du kan inaktivera layoutläget för formulär med äldre responsiv layout genom att redigera egenskaperna för mallen som används i formuläret.

Utför följande steg för att inaktivera layoutläget:

1. Välj **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL Templates]** och öppna mallen som används i formuläret i **[!UICONTROL Edit]**-läge.
1. Markera dokumentbehållaren i den vänstra rutan och välj **[!UICONTROL Policy.]**

   ![Inaktivera layoutläge](assets/policy_disable_layout_mode.png)

1. Välj fliken **[!UICONTROL Layout Settings]** och välj **[!UICONTROL Disable Layout Mode]**.
1. Välj ![Spara ändringar](assets/save_icon.png) om du vill spara mallegenskaperna.
