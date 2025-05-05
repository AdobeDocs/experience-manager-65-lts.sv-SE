---
title: Synkronisera adaptiv Forms med XFA-formulärmallar
description: Lär dig synkronisera formulär med XFA-/XDP-filer. Fälten i formulär som synkroniserats med ändringar som gjorts i motsvarande fält i XFA-/XDP-filerna återanvänds.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 24b7d3e5-7755-45f5-b4ea-fb61f25cf806
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1226'
ht-degree: 0%

---

# Synkronisera adaptiv Forms med XFA-formulärmallar{#synchronizing-adaptive-forms-with-xfa-form-templates}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Introduktion {#introduction}

Du kan skapa ett adaptivt formulär baserat på en XFA-formulärmall ( `*.XDP` fil). Tack vare detta återanvändande kan du behålla din investering i befintliga XFA-formulär. [Skapa ett adaptivt formulär baserat på en mall](../../forms/using/creating-adaptive-form.md#p-create-an-adaptive-form-based-on-an-xfa-form-template-p) om du vill ha information om hur du använder en XFA-formulärmall för att skapa ett adaptivt formulär.

Du kan återanvända fält från XDP-filen i ditt adaptiva formulär. Dessa fält kallas för binda fält. Egenskaperna för de bindade fälten (till exempel skript, etiketter och visningsformat) kopieras från XDP-filen. Du kan också välja att åsidosätta värdet för vissa av dessa egenskaper.

Med AEM Forms kan du synkronisera fälten i de adaptiva formulären med eventuella ändringar som senare görs i motsvarande fält i XDP-filen. I den här artikeln beskrivs hur du kan aktivera synkroniseringen.

![Du kan dra fält från ett XFA-formulär till ett anpassat formulär](assets/drag-drop-xfa.gif.gif)

I AEM Forms redigeringsmiljö kan du dra fält från ett XFA-formulär (vänster) till ett anpassat formulär (höger)

## Förutsättningar {#prerequisites}

Om du vill använda informationen i den här artikeln bör du känna till följande områden:

* [Skapa ett anpassat formulär](../../forms/using/creating-adaptive-form.md)

* XFA (XML Forms Architecture)

Om du vill använda resurserna som ett exempel i artikeln hämtar du exempelpaketet enligt beskrivningen i nästa avsnitt, [Exempelpaket](../../forms/using/synchronizing-adaptive-forms-xfa.md#p-sample-package-p).

## Exempelpaket {#sample-package}

I artikeln används ett exempel som visar hur du synkroniserar det adaptiva formuläret med en uppdaterad XFA-formulärmall. Resurserna som används i exemplet är tillgängliga i ett paket som kan hämtas från avsnittet [Hämtningar](../../forms/using/synchronizing-adaptive-forms-xfa.md#p-downloads-p) i den här artikeln.

När du har överfört paketet kan du visa de här resurserna i användargränssnittet för AEM Forms.

Installera paketet med pakethanteraren: `https://<server>:<port>/crx/packmgr/index.jsp`

Paketet innehåller följande resurser:

1. `sample-form.xdp`: XFA-formulärmallen används som exempel

1. `sample-xfa-af`: Det adaptiva formuläret som baseras på filen sample-form.xdp. Det här anpassningsbara formuläret innehåller dock inga fält. I nästa steg lägger du till innehåll i det här anpassade formuläret.

### Lägg till innehåll i anpassningsbara formulär {#add-content-to-adaptive-form-br}

1. Gå till https://&lt;server>:&lt;port>/aem/forms.html. Ange dina autentiseringsuppgifter om du tillfrågas.
1. Öppna exemplet-af-xfa för redigering i redigeringsläge.
1. Välj fliken Datamodellobjekt i innehållsläsaren i sidlisten. Dra NumericField1 och TextField1 till det adaptiva formuläret.
1. Ändra titeln för NumericField1 från **Numeriskt fält** till **AF numeriskt fält.**

>[!NOTE]
>
>I föregående steg skrev du över en egenskap för ett fält i XDP-filen. Den här egenskapen synkroniseras därför inte om motsvarande egenskap i XDP-filen redigeras senare.

## Identifiera ändringar i XDP-fil {#detecting-changes-in-xdp-file}

När en XDP-fil eller ett fragment ändras flaggas alla adaptiva formulär som är baserade på XDP-filen eller fragmentet av AEM Forms UI.

När du har uppdaterat en XDP-fil måste du överföra den igen i AEM Forms-gränssnittet för att ändringarna ska flaggas.

Låt oss till exempel uppdatera filen `sample-form.xdp` med följande steg:

1. Navigera till `https://<server>:<port>/projects.html.` Ange dina autentiseringsuppgifter om du uppmanas till det.
1. Klicka på fliken Forms till vänster.
1. Hämta filen `sample-form.xdp` på den lokala datorn. XDP-filen hämtas som en `.zip`-fil, som kan extraheras med valfritt fildekomprimeringsverktyg.

1. Öppna filen `sample-form.xdp` och ändra titeln för fältet TextField1 från **Textfält** till **Mitt textfält**.

1. Överför filen `sample-form.xdp` tillbaka till AEM Forms-gränssnittet.

Om en XDP-fil uppdateras visas en ikon i redigeraren när du redigerar de adaptiva formulären baserat på XDP-filen. Den här ikonen anger att det adaptiva formuläret inte är synkroniserat med XDP-filen. I följande bild ser du ikonen bredvid i sidlisten.

![Ikon som visar att det adaptiva formuläret inte är synkroniserat med XDP-filen](assets/sync-af-xfa.png)

## Synkronisera adaptiva formulär med den senaste XDP-filen {#synchronizing-adaptive-forms-with-the-latest-xdp-file}

När ett adaptivt formulär som inte är synkroniserat med XDP-filen öppnas för redigering nästa gång visas följande meddelande: **Schema/formulärmall för det adaptiva formuläret har uppdaterats. `Click Here` om du vill basera om den med den nya versionen.**

När du klickar på meddelandet synkroniseras fälten i det adaptiva formuläret med motsvarande fält i XDP-filen.

Öppna `sample-xfa-af` i redigeringsläget för det exempel som används i den här artikeln. Meddelandet visas längst ned i det anpassade formuläret.

![Meddelande som uppmanar dig att synkronisera det adaptiva formuläret med XDP-filen](assets/sync-af-xfa-1.png)

### Egenskaperna uppdateras {#updating-the-properties}

Alla egenskaper som kopierades från XDP-filen till det adaptiva formuläret uppdateras, förutom de egenskaper som explicit åsidosatts i det adaptiva formuläret (från komponentdialogrutan) av författaren. Listan över egenskaper som har uppdaterats är tillgänglig i serverloggarna.

Om du vill uppdatera egenskaperna i det adaptiva exempelformuläret klickar du på länken (märkt `"Click Here"`) i meddelandet. Titeln på TextField1 ändras från **Textfält** till **Textfält**.

![update-property](assets/update-property.png)

>[!NOTE]
>
>Etiketten för det numeriska AF-fältet ändrades inte eftersom du hade åsidosatt den här egenskapen från dialogrutan för komponentegenskaper, vilket beskrivs i [Lägg till innehåll i adaptiva formulär](../../forms/using/synchronizing-adaptive-forms-xfa.md#p-add-content-to-adaptive-form-br-p).

### Lägga till nya fält från XDP-fil i anpassat formulär   {#adding-new-fields-from-xdp-file-to-adaptive-form-nbsp}

Alla fält som läggs till senare i den ursprungliga XDP-filen visas på fliken Formulärhierarki och du kan dra de nya fälten till det anpassade formuläret.

Du behöver inte klicka på länken i felmeddelandet för att uppdatera fälten på fliken Formulärhierarki.

### Borttagna fält i XDP-fil {#deleted-fields-in-xdp-file}

Om ett fält som tidigare kopierats till ett adaptivt formulär tas bort från en XDP-fil visas ett felmeddelande i redigeringsläget om att fältet inte finns i XDP-filen. I så fall tar du bort fältet manuellt från anpassat formulär eller tar bort egenskapen `bindRef` i komponentdialogrutan.

Följande steg visar det här användningsflödet för resurserna i exemplet som används i den här artikeln:

1. Uppdatera filen `sample-form.xdp` och ta bort NumericField1.
1. Överför filen `sample-form.xdp` i AEM Forms-gränssnittet
1. Öppna det anpassningsbara formuläret `sample-xfa-af` för redigering. Följande felmeddelande visas: Schema/Formulärmall för det adaptiva formuläret har uppdaterats. `Click Here` om du vill basera om den med den nya versionen.

1. Klicka på länken (med namnet `Click Here`) i meddelandet. Ett felmeddelande visas som anger att fältet inte längre finns i XDP-filen.

![Ett fel uppstod när du tog bort ett element i XDP-filen](assets/no-element-xdp.png)

Fältet som har tagits bort är också markerat med en ikon som anger ett fel i fältet.

![Felikon i fältet](assets/error-field.png)

>[!NOTE]
>
>De fält i det adaptiva formuläret som har en felaktig bindning (ett ogiltigt `bindRef`-värde i redigeringsdialogrutan) betraktas också som borttagna fält. Om författaren inte åtgärdar dessa fel och publicerar det adaptiva formuläret behandlas fältet som ett vanligt obundet adaptivt formulärfält och inkluderas i det obundna avsnittet i XML-utdatafilen.

## Nedladdningar {#downloads}

Innehållspaket för exemplet i den här artikeln

[Hämta fil](assets/sample-xfa-af-sync-1.0.zip)
