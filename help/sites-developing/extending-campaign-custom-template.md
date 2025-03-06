---
title: Skapa en anpassad AEM-sidmall med Adobe Campaign-formulärkomponenter
description: Skapa en anpassad sidmall som använder Adobe Campaign Form-komponenter
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 59a79455-c108-4f4b-93c1-d8c6f23aec88
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 2%

---

# Skapa en anpassad AEM-sidmall med Adobe Campaign-formulärkomponenter{#creating-custom-aem-page-template-with-adobe-campaign-form-components}

På den här sidan beskrivs hur du skapar en anpassad sidmall som använder [Adobe Campaign Form](/help/sites-authoring/adobe-campaign-components.md) -komponenter genom att undersöka hur Geometrixx-mallen för utomhusbruk (`/apps/geometrixx-outdoors/components/page_campaign_profile`) implementeras, och den pekar dig på viktig information som du kan behöva när du skapar en egen anpassad mall.

>[!NOTE]
>
>[E-post och formulärexempel är bara tillgängliga i Geometrixx](/help/sites-developing/we-retail.md). Ladda ned exempelinnehåll för Geometrixx från Package Share.

>[!CAUTION]
>
>E-postkomponenterna i AEM har tagits bort. På grund av e-postens natur, som sammanfogar innehåll och format, kommer de e-postkomponenter som AEM tillhandahåller att vara färdiga att användas endast i begränsad omfattning av kunderna, eftersom de måste implementera anpassade format i de komponenter som behövs för projekten.
>
>E-postkomponenter kan implementeras på projektnivå och de borttagna e-postkomponenterna i AEM visar hur man kan uppnå detta. Använd dock inte de här inaktuella komponenterna i projekt.


Om du vill skapa en anpassad AEM-sidmall med hjälp av Adobe Campaign Form-komponenter måste du ha följande:

1. **Korrigera resourceSuperType**

   Kontrollera att sidkomponenten ärver från `mcm/campaign/components/profile`.

   Detta krävs för att servletarna ska kunna hämta och spara information

   * `com.day.cq.mcm.campaign.servlets.TemplateListServlet`
   * `com.day.cq.mcm.campaign.servlets.SaveProfileServlet`

   ![chlimage_1-201](assets/chlimage_1-201.png)

1. **ClientContext-inställningar**

   När du tittar på klientkontextinställningarna ( `/etc/designs/geometrixx-outdoors/jcr:content/page_campaign_profile`) ser du följande inställningar:

   * ClientContext pekar på `/etc/clientcontext/campaign`
   * Det finns även en extra *config*-nod.

   ![chlimage_1-202](assets/chlimage_1-202.png)

1. **head.jsp (/apps/geometrixx-outdoors/components/page_campaign_profile/head.jsp)**

   I **head.jsp** ser du följande rader som använder **clientcontext-config** och **cloudservice-kroken**:

   ```
   <cq:include path="config" resourceType="cq/personalization/components/clientcontext_optimized/config"/>
   <sling:include path="contexthub" resourceType="granite/contexthub/components/contexthub"/>
   <cq:include script="/libs/cq/cloudserviceconfigs/components/servicelibs/servicelibs.jsp"/>
   ```

1. **body.jsp (/apps/geometrixx-outdoors/components/page_campaign_profile/body.jsp)**

   I **body.jsp** läses molntjänsterna in längst ned på sidan:

   ```
   <cq:include path="cloudservices" resourceType="cq/cloudserviceconfigs/components/servicecomponents"/>
   ```

1. **Egenskaper för kampanjsida**

   Om du vill kunna välja en Adobe Campaign-mall utökas sidegenskaperna med fliken **Campaign** :

   `/apps/geometrixx-outdoors/components/page_campaign_profile/dialog/items/tabs/items/campaign`

   ![chlimage_1-203](assets/chlimage_1-203.png)

1. **Mallinställningar**.

   I mallen ( `/apps/geometrixx-outdoors/templates/campaign_profile/jcr:content`) visas följande standardvärden:

   | **acMapping** | mapRecipient (för Adobe Campaign 6.1), profile (för Adobe Campaign Standard) |
   |---|---|
   | **acTemplateId** | mail |

   ![chlimage_1-204](assets/chlimage_1-204.png)
