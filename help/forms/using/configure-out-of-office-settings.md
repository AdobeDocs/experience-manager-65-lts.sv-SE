---
title: Konfigurera inställningar för frånvaro
description: Lär dig hur du konfigurerar frånvaroinställningar till din Adobe Experience Manager Forms-instans.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Konfigurera frånvaroinställningar {#configure-out-of-office-settings}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/create-form-centric-workflows/configure-out-of-office-settings.html) |
| AEM 6.5 | Den här artikeln |

Om du tänker vara utanför kontoret kan du ange vad som ska hända med artiklar som har tilldelats dig för den perioden.

Du kan ange startdatum och -tid och slutdatum och sluttid så att dina inställningar som inte är på kontoret börjar gälla. Om du befinner dig i en annan tidszon än servern används den tidszon som klienten har.

Du kan ange en standardperson som alla dina objekt skickas till. Du kan också ange undantag för objekt från specifika processer som ska skickas till en annan användare eller behållas i inkorgen tills du kommer tillbaka. Om den utsedda personen även är utanför kontoret, skickas objektet till användaren som han/hon har utsett. Om objektet inte kan tilldelas till en användare som inte är utanför kontoret finns objektet kvar i din inkorg.

Du kan dela upp objektdelegering baserat på arbetsflödesmodellerna. Du kan till exempel tilldela ett objekt som är relaterat till arbetsflöde A till användare A och tilldela ett objekt som är relaterat till arbetsflöde B till användare B.


>[!NOTE]
>
>* När du aktiverar inställningen Frånvarande finns alla objekt som är tillgängliga i Inkorgen innan du aktiverar inställningen kvar i Inkorgen. Endast objekt som tagits emot efter att inställningen aktiverats delegeras.
>* När du inaktiverar inställningen Frånvarande tilldelas de delegerade objekten inte tillbaka till dig automatiskt. Du kan använda anspråksfunktionen för att tilldela objekt till dig.
>* När Användare A delegerar objekt till Användare B och Användare B delegerar vidare till Användare C, tilldelas objekten bara Användare C och inte Användare B.
>* När det finns en slinga i uppdraget stannar uppgifterna kvar hos den ursprungliga användaren. När Användare A till exempel delegerar objekt till Användare B till Användare C, Användare C delegerar till Användare D och Användare D delegerar till Användare B skapas en slinga. I sådana fall behålls objektet av den ursprungliga användaren. Användare A är ursprunglig användare i ovanstående exempel.

## Aktivera inställningen Frånvarande för ditt konto {#enable-out-of-office}

Utför följande steg för att aktivera inställningen Frånvarande för ditt konto och delegera dina inkorgsobjekt till en annan användare:

1. Logga in på din AEM-instans. Markera ikonen ![Inkorg](assets/bell.svg) och välj **[!UICONTROL View All]**. En lista över dina inkorgsobjekt visas.
1. Markera ikonen ![Visa väljare](assets/viewlist.svg) eller ![Visa väljare](assets/calendar.svg) bredvid knappen **[!UICONTROL Create]** och välj **[!UICONTROL Settings]**. Dialogrutan Inställningar visas.
1. Öppna fliken **[!UICONTROL Out of Office]** i inställningsdialogrutan.
1. Markera knappen **[!UICONTROL Enable/Disable]** om du vill aktivera inställningen Frånvarande.
1. Ange **[!UICONTROL Start Time]** och **[!UICONTROL End Time]** för inställningen. Objekten delegeras endast under den angivna perioden. Lämna fältet **[!UICONTROL End Time]** tomt om du vill delegera objekt för en obegränsad tidsperiod.
1. Markera kryssrutan **[!UICONTROL Forward my items during this period]**. Om du inte markerar alternativet och inte anger en tilldelad användare vidarebefordras inte objekten till någon användare. Även om du är borta och inställningen är aktiverad finns objekten kvar i Inkorgen.
1. Välj **[!UICONTROL Add Assignee]**. Ange en användare i fältet **[!UICONTROL Assignee]** så att du kan delegera objekten. Ange **[!UICONTROL Workflow Model]** så att du kan delegera till den angivna användaren. Du kan välja mer än en arbetsflödesmodell.

   Om du dessutom vill tilldela alla objekt, oavsett arbetsflödesmodell, till en viss användare väljer du **[!UICONTROL All Workflows]** i listrutan Arbetsflödesmodell. <br>

   Om du vill tilldela objekt till en viss användare för alla arbetsflödesmodeller utom ett fåtal, väljer du **[!UICONTROL All Workflows]** i listrutan Arbetsflödesmodell, väljer **[!UICONTROL + Add Exceptions]** och anger vilka arbetsflödesmodeller som ska utelämnas.
   <br>

   Upprepa steget så att du kan lägga till fler tilldelningar. <br>

   >[!NOTE]
   >
   >Tilldelningsordningen är viktig. När ett objekt tilldelas till en användare som har aktiverat inställningen för frånvaro utvärderas objektet mot den angivna listan över tilldelningar i ordningen som tilldelningarna läggs till. När ett objekt matchar villkoren tilldelas det till den som tilldelats objektet och nästa som tilldelats kontrolleras inte.

1. Välj **[!UICONTROL Save]**. Inställningen börjar gälla vid angivet startdatum och angiven starttid. Om du loggar in när du inte är på kontoret beaktas du inte på kontoret förrän du ändrar dina inställningar.

Nu tilldelas objekt som du har tilldelats under frånvaroperioden automatiskt till den angivna tilldelade personen.
![Utanför kontoret](assets/out-of-office.png)

>[!NOTE]
>
>(Endast för Forms-centrerade arbetsflödesobjekt) Aktivera alternativet **Tillåt att tilldelad delegerar med inställningen Frånvarande** i steget **Tilldela uppgift** i arbetsflödet. Endast objekt som har det tidigare nämnda alternativet aktiverat delegeras till andra användare.

## Begränsningar {#limitations}

* Det går inte att tilldela objekt till en grupp.
* Det går för närvarande inte att aktivera frånvaro för projektaktiviteter.
