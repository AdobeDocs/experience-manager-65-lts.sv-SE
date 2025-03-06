---
title: Skapa ett anpassat formulär med hjälp av en uppsättning anpassningsbara formulär
description: Med AEM Forms kan du sammanföra adaptiva formulär och ta fram ett enda stort anpassat formulär och förstå dess funktioner.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 1e9926ca-70ef-4777-8a79-f608df80ada1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 0%

---

# Skapa ett anpassat formulär med hjälp av en uppsättning anpassningsbara formulär{#create-an-adaptive-form-using-a-set-of-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Ökning {#overview}

I ett arbetsflöde, till exempel ett program för att öppna ett bankkonto, fyller dina användare i flera formulär. I stället för att be dem fylla i en uppsättning formulär kan du stapla formulären tillsammans och skapa ett stort formulär (överordnat formulär). När du lägger till ett anpassat formulär i det större formuläret läggs det till som en panel (underordnat formulär). Du lägger till en uppsättning med underordnade formulär för att skapa ett överordnat formulär. Du kan visa eller dölja paneler baserat på användarindata. Knappar i det överordnade formuläret, till exempel skicka och återställ, skriver över knapparna i det underordnade formuläret. Om du vill lägga till ett anpassat formulär i det överordnade formuläret kan du dra och släppa det anpassningsbara formuläret från resursläsaren (som adaptiva formulärfragment).

Tillgängliga funktioner är:

* Oberoende redigering
* Visa/dölj lämpliga formulär
* Lazy loading

Funktioner som oberoende redigering och lat inläsning ger prestandaförbättringar jämfört med att använda enskilda komponenter för att skapa det överordnade formuläret.

>[!NOTE]
>
>Du kan inte använda XFA-baserade adaptiva formulär/fragment som underordnade eller överordnade formulär.

## Bakom scenen {#behind-the-scenes}

Du kan lägga till XSD-baserade adaptiva formulär och fragment i det överordnade formuläret. Det överordnade formulärets struktur är densamma som [alla adaptiva formulär](../../forms/using/prepopulate-adaptive-form-fields.md). När du lägger till ett anpassat formulär som ett underordnat formulär läggs det till som en panel i det överordnade formuläret. Data i ett bundet underordnat formulär lagras under `data`roten i `afBoundData`-avsnittet i det överordnade formulärets XML-schema.

Kunderna fyller t.ex. i en ansökningsblankett. De två första fälten i formuläret är namn och identitet. XML:

```xml
<afData>
    <afUnboundData>
        <data />
    </afUnboundData>
    <afBoundData>
        <data xmlns:xfa="https://www.xfa.org/schema/xfa-data/1.0/">
            <applicantName>Sarah Rose</applicantName>
            <applicantId>1234</applicantId>
        </data>
    </afBoundData>
</afData>
```

Du lägger till ett annat formulär i programmet som gör att dina kunder kan fylla i sin kontorsadress. Schemaroten för det underordnade formuläret är `officeAddress`. Använd `bindref` `/application/officeAddress` eller `/officeAddress`. Om `bindref` inte anges läggs det underordnade formuläret till som `officeAddress`-underträdet. Se XML för formuläret nedan:

```xml
<afData>
    <afUnboundData>
        <data />
    </afUnboundData>
    <afBoundData>
        <data xmlns:xfa="https://www.xfa.org/schema/xfa-data/1.0/">
            <applicantName>Sarah Rose</applicantName>
            <applicantId>1234</applicantId>
            <officeAddress>
                <addressLine>1, Geometrixx City</addressLine>
                <zip>11111</zip>
            </officeAddress>
        </data>
    </afBoundData>
</afData>
```

Om du infogar ett annat formulär som gör att dina kunder kan ange en hemadress använder du `bindref` `/application/houseAddress or /houseAddress.`XML:en ser ut så här:

```xml
<afData>
    <afUnboundData>
        <data />
    </afUnboundData>
    <afBoundData>
        <data xmlns:xfa="https://www.xfa.org/schema/xfa-data/1.0/">
            <applicantName>Sarah Rose</applicantName>
            <applicantId>1234</applicantId>
            <officeAddress>
                <addressLine>1, Geometrixx City</addressLine>
                <zip>11111</zip>
            </officeAddress>
            <houseAddress>
                <addressLine>2, Geometrixx City</addressLine>
                <zip>11111</zip>
            </houseAddress>
        </data>
    </afBoundData>
</afData>
```

Om du vill behålla samma underrotnamn som schemaroten ( `Address` i det här exemplet) använder du indexerade bindrefs.

Använd t.ex. bindrefs `/application/address[1]`, `/address[1]`, `/application/address[2]` eller `/address[2]`. Formulärets XML är:

```xml
<afData>
    <afUnboundData>
        <data />
    </afUnboundData>
    <afBoundData>
        <data xmlns:xfa="https://www.xfa.org/schema/xfa-data/1.0/">
            <applicantName>Sarah Rose</applicantName>
            <applicantId>1234</applicantId>
            <address>
                <addressLine>1, Geometrixx City</addressLine>
                <zip>11111</zip>
            </address>
            <address>
                <addressLine>2, Geometrixx City</addressLine>
                <zip>11111</zip>
            </address>
        </data>
    </afBoundData>
</afData>
```

Du kan ändra standardunderträdet för det adaptiva formuläret/fragmentet med egenskapen `bindRef`. Med egenskapen `bindRef` kan du ange sökvägen som pekar på en plats i trädstrukturen i XML-schemat.

Om det underordnade formuläret är obundet lagras dess data under `data`roten i `afUnboundData`-avsnittet i det överordnade formulärets XML-schema.

Du kan lägga till ett anpassat formulär som ett underordnat formulär flera gånger. Kontrollera att `bindRef` har ändrats på rätt sätt så att varje instans av det adaptiva formuläret pekar på en annan underrot under dataroten.

>[!NOTE]
>
>Om olika formulär/fragment mappas till samma underrot skrivs data över.

## Lägga till ett adaptivt formulär som ett underordnat formulär med hjälp av resursläsaren {#adding-an-adaptive-form-as-a-child-form-using-asset-browser}

Utför följande steg för att lägga till ett anpassat formulär som ett underordnat formulär med hjälp av resursläsaren.

1. Öppna det överordnade formuläret i redigeringsläge.
1. Klicka på **Assets** ![assets-browser](assets/assets-browser.png) i sidlisten. Under Assets väljer du **Adaptivt formulär** i listrutan.
   [![Välja anpassat formulär under Assets](assets/asset.png)](assets/asset-1.png)

1. Dra och släpp det adaptiva formulär som du vill lägga till som ett underordnat formulär.
   [![Dra och släpp det adaptiva formuläret på din plats](assets/drag-drop.png)](assets/drag-drop-1.png)Det adaptiva formuläret som du släpper läggs till som ett underordnat formulär.
