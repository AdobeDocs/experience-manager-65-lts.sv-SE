---
title: Profiler för bearbetning av metadata, bilder och video
description: En profil med en uppsättning regler runt alternativen som ska tillämpas på resurser som överförs till en mapp. Ange vilken metadataprofil och videokodningsprofil som ska användas för de videoresurser som du överför. För bildresurser kan du även ange vilken bildprofil som ska användas för bildresurser så att de beskärs på rätt sätt.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: administering
content-type: reference
docset: aem65
role: User, Admin
feature: Workflow,Asset Management,Renditions
solution: Experience Manager, Experience Manager Assets
exl-id: aa257d33-302c-4a01-be48-e4ff56700bfa
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1336'
ht-degree: 0%

---

# Profiler för bearbetning av metadata, bilder och videor{#profiles-for-processing-metadata-images-and-videos}

En profil är ett recept på vilka alternativ som ska användas för resurser som överförs till en mapp. Du kan till exempel ange vilken metadataprofil och videokodningsprofil som ska användas för videoresurser som du överför. Eller vilken bildprofil som ska användas för bildresurser så att de beskärs ordentligt.

Dessa regler kan omfatta tillägg av metadata, smart beskärning av bilder eller etablering av videokodningsprofiler. I Adobe Experience Manager kan du skapa tre typer av profiler som beskrivs närmare på följande länkar:

* [Metadataprofiler](/help/assets/metadata-config.md#metadata-profiles)
* [Bildprofiler](/help/assets/image-profiles.md)
* [Videoprofiler](/help/assets/video-profiles.md)

Du behöver administratörsbehörighet för att skapa, redigera och ta bort metadata-, bild- eller videoprofiler.

När du har skapat metadata-, bild- eller videoprofilen tilldelar du den till en eller flera mappar som du använder som mål för nyligen överförda resurser.

Ett viktigt koncept när det gäller användningen av profiler i Experience Manager Assets är att de tilldelas mappar. I en profil finns inställningar i form av metadataprofiler, tillsammans med videoprofiler eller bildprofiler. De här inställningarna bearbetar innehållet i en mapp tillsammans med någon av dess undermappar. Det innebär att hur du namnger filer och mappar, hur du ordnar undermappar och hur du hanterar filerna i dessa mappar har stor inverkan på hur resurserna bearbetas av en profil.
Genom att använda konsekventa och lämpliga namngivningsstrategier för filer och mappar samt god metadatapraxis får du ut det mesta av din digitala resurssamling och ser till att rätt filer bearbetas med rätt profil.

>[!NOTE]
>
>Assets som du flyttar från en mapp till en annan bearbetas inte om. Anta till exempel att du har Mapp 1 som har tilldelats profilen A och Mapp 2 som har profilen B tilldelad. Om du flyttar resurser från Mapp 1 till Mapp 2 behåller de flyttade resurserna sin ursprungliga bearbetning från Mapp 1.
>
>Detsamma gäller även när du flyttar resurser mellan två mappar som har samma profil tilldelad.

## Bearbeta resurser igen i en mapp {#reprocessing-assets}

>[!NOTE]
>
>Gäller endast för *dynamiska media - Scene7-läge* i Experience Manager 6.4.6.0 eller senare.

Du kan bearbeta resurser i en mapp som redan har en befintlig bearbetningsprofil som du senare ändrade.

Anta att du har skapat en bildprofil och tilldelat den till en mapp. Bildobjekt som du överförde till mappen fick automatiskt bildprofilen tillämpad på resurserna. Men senare bestämmer du dig för att lägga till en ny smart beskärningsproportion i profilen. I stället för att nu ha markerat och laddat upp resurserna till mappen igen kör du arbetsflödet *Dynamisk medieombearbetning* <!-- *Scene7: Reprocess Assets* -->.

Du kan köra arbetsflödet för ombearbetning på en resurs som bearbetningen misslyckades för första gången. Även om du inte har redigerat en bearbetningsprofil eller använt en bearbetningsprofil kan du ändå köra arbetsflödet för ombearbetning på en mapp med resurser när som helst.

Du kan också justera batchstorleken för arbetsflödet för ombearbetning från standardvärdet 50 resurser upp till 1 000 resurser. När du kör _Scene7: Bearbeta om Assets_ på en mapp grupperas resurserna i grupper och skickas sedan till Dynamic Media-servern för bearbetning. Efter bearbetning uppdateras metadata för varje resurs i hela gruppuppsättningen på Experience Manager. Om gruppstorleken är stor kan bearbetningen fördröjas. Om gruppstorleken är för liten kan det orsaka för många rundresor till Dynamic Media-servern.

Se [Justera gruppstorleken för arbetsflödet för ombearbetning](#adjusting-load).

>[!NOTE]
>
>Om du utför en massmigrering av resurser från Dynamic Media Classic till Experience Manager måste du aktivera migreringsreplikeringsagenten på den dynamiska medieservern. När migreringen är klar kontrollerar du att agenten är inaktiverad.
>
>Publiceringsagenten för migrering måste inaktiveras på Dynamic Media-servern så att arbetsflödet för ombearbetning fungerar som förväntat.

<!-- Batch size is the number of assets that are amalgamated into a single IPS (Dynamic Media's Image Production System) job. When you run the Dynamic Media Reprocess workflow, the job is triggered on IPS. The number of IPS jobs that are triggered is based on the total number of assets in the folder, divided by the batch size. For example, suppose you had a folder with 150 assets and a batch size of 50. In this case, three IPS jobs are triggered. The assets are updated when the entire batch size (50 in our example) is processed in IPS. The job then moves onto the next IPS job, and so on, until complete. If you increase the batch size, you may notice a longer delay with assets getting updated. -->

**Så här bearbetar du resurser i en mapp:**

1. I Experience Manager navigerar du från Assets-sidan till en mapp med resurser som har en bearbetningsprofil tilldelad och som du vill använda arbetsflödet för **[!UICONTROL Dynamic Media Reprocess]**,

   Mappar som redan har tilldelats en bearbetningsprofil visas genom att profilens namn visas direkt under mappnamnet i kortvyn.

1. Välj en mapp.

   * Arbetsflödet hanterar alla filer i den valda mappen rekursivt.
   * Om det finns en eller flera undermappar med resurser i den markerade huvudmappen, bearbetas alla resurser i mapphierarkin om i arbetsflödet.
   * Som en god vana bör du undvika att köra det här arbetsflödet på en mapphierarki som har fler än 1 000 resurser.

1. I den nedrullningsbara listan i det övre vänstra hörnet av sidan väljer du **[!UICONTROL Timeline]**.
1. I närheten av sidans nedre vänstra hörn, till höger om kommentarsfältet, väljer du karikonen ( **^** ).

   ![Bearbeta om resursarbetsflöde 1](/help/assets/assets/reprocess-assets1.png)

1. Välj **[!UICONTROL Start Workflow]**.
1. Välj **[!UICONTROL Dynamic Media Reprocess]** i listrutan **[!UICONTROL Start Workflow]**.
1. (Valfritt) Ange ett namn för arbetsflödet i textfältet **Ange arbetsflödets namn**. Du kan använda namnet för att referera till arbetsflödesinstansen, om det behövs.

   ![Bearbeta resurser igen &#x200B;](/help/assets/assets/reprocess-assets2.png)

1. Välj **[!UICONTROL Start]** och sedan **[!UICONTROL Confirm]**.

   Om du vill övervaka arbetsflödet eller kontrollera förloppet väljer du **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** på Experience Manager huvudkonsolsida. Välj ett arbetsflöde på sidan Arbetsflödesinstanser. Välj **[!UICONTROL Open History]** på menyraden. Du kan också avsluta, göra uppehåll i eller byta namn på ett valt arbetsflöde från samma sida för arbetsflödesinstanser.

### Justera batchstorleken för arbetsflödet för ombearbetning {#adjusting-load}

(Valfritt) Standardbatchstorleken i ombearbetningsarbetsflödet är 50 resurser per jobb. Den optimala batchstorleken styrs av den genomsnittliga tillgångsstorleken och de MIME-typer av resurser som ombearbetningen körs på. Ett högre värde innebär att du har många filer i ett och samma ombearbetningsjobb. Bearbetningsbanderollen ligger alltså kvar på Experience Manager-mediefiler en längre tid. Om den genomsnittliga filstorleken är liten - 1 MB eller mindre - rekommenderar Adobe att du ökar värdet till flera 100, men aldrig mer än 1 000. Om den genomsnittliga filstorleken är stor, till exempel hundratals megabyte, rekommenderar Adobe att du minskar gruppstorleken med upp till 10.

**Om du vill justera gruppstorleken för arbetsflödet för ombearbetning:**

1. I Experience Manager väljer du **[!UICONTROL Adobe Experience Manager]** för att komma åt den globala navigeringskonsolen och sedan **[!UICONTROL Tools]** (hammare) > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. Välj **[!UICONTROL Dynamic Media Reprocess]** i kortvyn eller listvyn på sidan Arbetsflödesmodeller.

   ![Sidan Arbetsflödesmodeller med arbetsflödet för dynamisk medieombearbetning markerat i kortvyn](/help/assets/assets-dm/reprocess-assets7.png)

1. Välj **[!UICONTROL Edit]** i verktygsfältet. En ny flik i webbläsaren öppnar sidan Dynamisk medieombearbetning.
1. På sidan Dynamic Media Reprocess workflow, i det övre högra hörnet, väljer du **[!UICONTROL Edit]** för att låsa upp arbetsflödet.
1. I arbetsflödet väljer du komponenten Scene7 Batch Upload för att öppna verktygsfältet och väljer sedan **[!UICONTROL Configure]** i verktygsfältet.

   ![Komponenten för gruppöverföring i Scene7 &#x200B;](/help/assets/assets-dm/reprocess-assets8.png)

1. Ange följande i dialogrutan **[!UICONTROL Batch Upload to Scene7 – Step Properties]**:
   * I textfälten **[!UICONTROL Title]** och **[!UICONTROL Description]** anger du en ny titel och beskrivning för jobbet, om så önskas.
   * Välj **[!UICONTROL Handler Advance]** om hanteraren ska gå vidare till nästa steg.
   * I fältet **[!UICONTROL Timeout]** anger du tidsgränsen för den externa processen (sekunder).
   * I fältet **[!UICONTROL Period]** anger du ett avsökningsintervall (sekunder) som ska testas för att den externa processen ska slutföras.
   * I **[!UICONTROL Batch field]** anger du det maximala antalet resurser (50-1000) som ska bearbetas i ett batchbearbetningsjobb för en Dynamic Media-server.
   * Välj **[!UICONTROL Advance on timeout]** om du vill fortsätta när tidsgränsen nås. Avbryt markeringen om du vill fortsätta till inkorgen när tidsgränsen nås.

   ![Dialogrutan Egenskaper](/help/assets/assets-dm/reprocess-assets3.png)

1. Välj **[!UICONTROL Done]** i det övre högra hörnet i dialogrutan **[!UICONTROL Batch Upload to Scene7 – Step Properties]**.

1. Välj **[!UICONTROL Sync]** i det övre högra hörnet på sidan för arbetsflödesmodellen för ombearbetning av dynamiska media. När du ser **[!UICONTROL Synced]** synkroniseras arbetsflödets körningsmodell och kan bearbeta resurser i en mapp igen.

   ![Synkronisera arbetsflödesmodellen](/help/assets/assets-dm/reprocess-assets1.png)

1. Stäng webbläsarfliken som visar arbetsflödesmodellen för dynamisk medieombearbetning.

<!--1. Return to the browser tab that has the open Workflow Models page, then press **Esc** to exit the selection.
1. In the upper-left corner of the page, select **[!UICONTROL Adobe Experience Manager]** to access the global navigation console, then select the **[!UICONTROL Tools]** (hammer) icon > **[!UICONTROL General > CRXDE Lite]**.
1. In the folder tree on the left side of the CRXDE Lite page, navigate to the following location:

   `/conf/global/settings/workflow/models/scene7_reprocess_assets/jcr:content/flow/reprocess/metaData`

   ![CRXDE Lite](/help/assets/assets/workflow-models9.png)

1. On the right side of the CRXDE Lite page, in the lower portion, enter the following name, type, and value in its respective field:
    * **[!UICONTROL Name]**: `reprocess-batch-size`
    * **[!UICONTROL Type]**: `Long`
    * **[!UICONTROL Value]**: enter a default value (50-1000) for the batch size
1. In the lower-right corner, select **[!UICONTROL Add]**. The new property appears as the following:

    ![Saving the new property](/help/assets/assets/workflow-models10.png)

1. On the menu bar of the CRXDE Lite page, select **[!UICONTROL Save All]**.
1. In the upper-left corner of the page, select **[!UICONTROL CRXDE Lite]** to return to the main Experience Manager console
1. Repeat steps 1-7 to re-synchronize the new batch size to the Dynamic Media Reprocess workflow model.-->
