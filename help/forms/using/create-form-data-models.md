---
title: Skapa formulärdatamodell
description: Lär dig skapa formulärdatamodeller med eller utan konfigurerade datakällor.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: integration
docset: aem65
feature: Form Data Model
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: b27fceaf-38f8-433e-96c6-4f98bafa31af
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Skapa formulärdatamodell{#create-form-data-model}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html?lang=sv-SE).


![hjältebild](do-not-localize/data-integration.png)

AEM Forms dataintegrering ger ett intuitivt användargränssnitt för att skapa och arbeta med formulärdatamodeller. En formulärdatamodell bygger på datakällor för utbyte av data, men du kan skapa en formulärdatamodell med eller utan en datakälla. Det finns två sätt att skapa en utifrån datamodell beroende på om du har konfigurerat datakällor:

* **Använda förkonfigurerade datakällor**: Om du har konfigurerat datakällor enligt beskrivningen i [Konfigurera datakällor](../../forms/using/configure-data-sources.md) kan du markera dem när du skapar en formulärdatamodell. Den hämtar alla datamodellsobjekt, egenskaper och tjänster från de valda datakällorna som är tillgängliga för användning i formulärdatamodellen.

* **Utan datakällor**: Om du inte har konfigurerat datakällor för formulärdatamodellen kan du fortfarande skapa den utan datakällor. Du kan använda formulärdatamodellen för att skapa adaptiva formulär och interaktiv kommunikation och testa dem med exempeldata. När datakällor är tillgängliga kan du binda formulärdatamodellen till datakällor, som automatiskt återspeglas i de tillhörande adaptiva formulären och interaktiva kommunikationerna.

>[!NOTE]
>
>Du måste vara medlem i båda grupperna **fdm-author** och **forms-user** för att kunna skapa och arbeta med formulärdatamodellen. Kontakta AEM-administratören om du vill bli medlem i grupperna.

## Skapa formulärdatamodell {#data-sources}

Kontrollera att du har konfigurerat de datakällor som du vill använda i formulärdatamodellen enligt beskrivningen i [Konfigurera datakällor](../../forms/using/configure-data-sources.md). Så här skapar du en formulärdatamodell som baseras på konfigurerade datakällor:

1. Gå till **[!UICONTROL Forms > Data Integrations]** i AEM-författarinstans.
1. Välj **[!UICONTROL Create > Form Data Model]**.
1. I dialogrutan Skapa formulärdatamodell:

   * Ange ett namn för formulärdatamodellen.
   * (**Valfritt**) Ange rubrik, beskrivning och taggar för formulärdatamodellen.
   * (**Valfritt och endast tillämpligt om datakällor har konfigurerats**) Markera kryssruteikonen bredvid fältet **[!UICONTROL Data Source Configuration]** och välj konfigurationsnoden där molntjänster för de datakällor som du vill använda finns. Den begränsar listan med datakällor som är tillgängliga för val på nästa sida till de som är tillgängliga i den valda konfigurationsnoden. Alla JDBC-databaser och AEM användarprofildatakällor listas som standard. Om du inte väljer en konfigurationsnod visas datakällor från alla konfigurationsnoder.

   Välj **[!UICONTROL Next]**.

1. (**Gäller endast om datakällor har konfigurerats**) Skärmen **[!UICONTROL Select Datasource]** visar eventuella tillgängliga datakällor. Välj de datakällor som du vill använda i formulärdatamodellen.
1. Välj **[!UICONTROL Create]** och välj **[!UICONTROL Open]** i bekräftelsedialogrutan för att öppna formulärdatamodellens redigerare.

Låt oss granska de olika komponenterna i användargränssnittet för formulärdatamodellsredigeraren.

![En formulärdatamodell med tre datakällor - en RESTful-tjänst, AEM-användarprofil och en RDBMS](assets/fdm-ui.png)

**A. Datakällor** Visar datakällor i en formulärdatamodell. Expandera en datakälla om du vill visa dess datamodellsobjekt och -tjänster.

**B. Uppdatera Source-definitioner för data** Hämtar alla ändringar i datakälldefinitioner från konfigurerade datakällor och uppdaterar dem på fliken Datakällor i formulärdatamodellens redigerare.

**C. Modell** Innehållsområde där tillagda datamodellsobjekt visas.

**D. Tjänster** Innehållsområde där tillagda datakällåtgärder eller -tjänster visas.

**E. Verktygsfält** Verktyg för att arbeta med formulärdatamodell. I verktygsfältet visas fler alternativ beroende på vilket objekt som är markerat i formulärdatamodellen.

**F. Lägg till markerade** Lägger till markerade datamodellsobjekt och tjänster i formulärdatamodellen.

Mer information om formulärdatamodellredigerare och hur du kan arbeta med den för att redigera och konfigurera formulärdatamodellen finns i [Arbeta med formulärdatamodell](../../forms/using/work-with-form-data-model.md).

## Uppdatera datakällor {#update}

Gör följande för att lägga till eller uppdatera datakällor till en befintlig formulärdatamodell.

1. Gå till **[!UICONTROL Forms > Data Integrations]**, välj den formulärdatamodell i vilken du vill lägga till eller uppdatera datakällor och välj **[!UICONTROL Properties]**.
1. Gå till fliken **[!UICONTROL Update Source]** i egenskaperna för formulärdatamodellen.

   På fliken Uppdatera Source:

   * Välj bläddringsikonen i fältet **[!UICONTROL Context-Aware Configuration]** och välj en konfigurationsnod där molnkonfigurationen för den datakälla som du vill lägga till finns. Om du inte väljer en nod visas molnkonfigurationer som bara finns i noden `global` när du väljer **[!UICONTROL Add Sources]**.

   * Om du vill lägga till en ny datakälla väljer du **[!UICONTROL Add Sources]** och markerar de datakällor som ska läggas till i formulärdatamodellen. Alla datakällor som har konfigurerats i `global` och den valda konfigurationsnoden (om det finns någon) visas.

   * Om du vill ersätta en befintlig datakälla med en annan datakälla av samma typ, väljer du ikonen **[!UICONTROL Edit]** för datakällan och väljer i listan över tillgängliga datakällor.
   * Om du vill ta bort en befintlig datakälla väljer du ikonen **[!UICONTROL Delete]** för datakällan. Ikonen Ta bort är inaktiverad om ett datamodellsobjekt i datakällan läggs till i formulärdatamodellen.

   ![fdm-properties](assets/fdm-properties.png)

1. Välj **[!UICONTROL Save & Close]** om du vill spara uppdateringarna.

>[!NOTE]
>
>När du lägger till nya datakällor eller uppdaterar befintliga datakällor i en formulärdatamodell måste du uppdatera bindningsreferenserna, beroende på vad som är lämpligt, i anpassningsbara formulär och interaktiv kommunikation som använder den uppdaterade formulärdatamodellen.

## Nästa steg {#next-steps}

Nu har du en formulärdatamodell med nya datakällor. Därefter kan du redigera formulärdatamodellen för att lägga till och konfigurera datamodellsobjekt och -tjänster, lägga till associationer mellan datamodellsobjekt, redigera egenskaper, lägga till anpassade datamodellsobjekt och egenskaper, generera exempeldata osv.

Mer information finns i [Arbeta med formulärdatamodell](../../forms/using/work-with-form-data-model.md).
