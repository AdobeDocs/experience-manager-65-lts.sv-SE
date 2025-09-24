---
title: 'Självstudiekurs: Skapa ditt första anpassningsbara formulär'
description: Lär dig skapa interaktiva och responsiva blanketter i affärsklass.
feature: Adaptive Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: caa79def-4a29-4746-9a35-c362ea1c3c0c
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# Självstudiekurs: Skapa ditt första anpassningsbara formulär {#tutorial-create-your-first-adaptive-form}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/create-an-adaptive-form-on-forms-cs/creating-adaptive-form.html).


![01-create-first-adaptive-form-hero-image](assets/01-create-first-adaptive-form-hero-image.png)

## Introduktion {#introduction}

Söker du en mobilvänlig **formulärupplevelse** som förenklar registrering, ökar engagemanget och minskar handläggningstiden, så **anpassningsbara formulär** passar dig perfekt. Adaptiva formulär ger en mobil, automatiserad och analysvänlig formulärupplevelse. Ni kan enkelt skapa formulär som är responsiva och interaktiva till sin natur, använda automatiserade processer för att minska administrativa och repetitiva uppgifter och använda dataanalys för att förbättra och personalisera den upplevelse kunderna har med era formulär.

Den här självstudiekursen ger ett komplett ramverk för att skapa ett anpassningsbart formulär. Självstudiekursen är indelad i ett användningsfall och i flera guider. Varje guide hjälper dig att lära dig mer och lägga till nya funktioner i det adaptiva formulär som skapas i den här kursen. Du har ett fungerande anpassningsbart formulär efter varje guide. Guiden för att skapa ett anpassat formulär är tillgänglig. Efterföljande guider kommer snart. I slutet av den här självstudiekursen kan du göra följande:

* Skapa ett anpassningsbart formulär och en formulärdatamodell.
* Formatera den anpassningsbara formen.
* Använd redigerare för anpassade formulärregler för att skapa affärsregler.
* Testa och publicera ett adaptivt formulär.

![create-adaptive-form-workflow](assets/create-daptive-form-workflow.png)

Resan börjar med att man lär sig hur det fungerar:

En webbplats erbjuder en rad produkter för olika kunder. Kunderna går igenom portalen, väljer ut och beställer produkterna. Alla kunder skapar ett konto och tillhandahåller frakt- och faktureringsadresser. En befintlig kund, Sara Rose, vill lägga till sin leveransadress på webbplatsen. På webbplatsen finns ett onlineformulär där du kan lägga till och uppdatera leveransadresser.

Webbplatsen körs på Adobe Experience Manager (AEM) och använder AEM [!DNL Forms] för datainhämtning och databearbetning. Formuläret för att lägga till och uppdatera adresser är ett anpassat formulär. Webbplatsen lagrar kundinformation i en databas. De använder formuläret för att lägga till och uppdatera adresser för att hämta och visa tillgängliga adresser. De använder också det adaptiva formuläret för att godkänna uppdaterade och nya adresser.

### Förutsättning {#prerequisite}

* Konfigurera en [AEM-författarinstans](https://experienceleague.adobe.com/docs/experience-manager-65-lts/content/implementing/deploying/deploying/deploy.html#author-and-publish-installs)
* Installera [AEM Forms-tillägget](../../forms/using/installing-configuring-aem-forms-osgi.md) på författarinstansen.
* Hämta JDBC-databasdrivrutin (JAR-fil) från databasprovidern. Exemplen i självstudien är baserade på databasen [!DNL MySQL] och använder databasdrivrutinen [!DNL Oracle's] [MySQL JDBC ](https://dev.mysql.com/downloads/connector/j/5.1.html).

* Konfigurera en databas som innehåller kunddata med fälten som visas nedan. En databas behövs inte för att skapa ett anpassningsbart formulär. I den här självstudien används en databas för att visa formulärdatamodell och beständighetsfunktioner i AEM [!DNL Forms].

![adaptiveformdata](assets/adaptiveformdata.png)

## Steg 1: Skapa ett anpassat formulär {#step-create-an-adaptive-form}

![03-create-adaptive-form-main-image_small](assets/03-create-adaptive-form-main-image_small.png)

Adaptiva former är ny generation, engagerande, responsiva, dynamiska och anpassningsbara till sin natur. Med hjälp av anpassningsbara formulär kan ni leverera personaliserade och målinriktade upplevelser. AEM [!DNL Forms] har en WYSIWYG-redigerare som du kan dra och släppa för att skapa anpassningsbara formulär. Mer information om adaptiva formulär finns i [Introduktion till utveckling av adaptiva formulär](../../forms/using/introduction-forms-authoring.md).

Mål:

* Skapa ett anpassningsbart formulär där kunden kan lägga till en leveransadress.
* Layoutfält i ett anpassat formulär som visar och accepterar information från en kund.
* Skapa en Skicka-åtgärd för att skicka ett e-postmeddelande som innehåller formulärinnehåll.
* Förhandsgranska och skicka ett anpassat formulär.

[![Se guiden](assets/see-the-guide-sm.png)](create-adaptive-form.md)

## Steg 2: Skapa formulärdatamodell {#step-create-form-data-model}

![05-create-form-data-model-main_small](assets/05-create-form-data-model-main_small.png)

Med en formulärdatamodell kan du koppla ett anpassningsbart formulär till olika datakällor. Exempel: AEM användarprofil, RESTful-webbtjänster, SOAP-baserade webbtjänster, OData-tjänster och relationsdatabaser. En formulärdatamodell är ett enhetligt datarepresentationsschema för affärsenheter och tjänster som är tillgängliga i anslutna datakällor. Du kan använda formulärdatamodellen med ett adaptivt formulär för att hämta, uppdatera, ta bort och lägga till data i anslutna datakällor.

Mål:

* Konfigurera webbplatsens databasinstans ([!DNL MySQL]-databas) som en datakälla.
* Skapa formulärdatamodellen med [!DNL MySQL]-databasen som en datakälla.
* Lägg till datamodellsobjekt så att du kan skapa datamodellen.
* Konfigurera läs- och skrivtjänster för formulärdatamodellen.
* Testa formulärdatamodellen och konfigurerade tjänster med testdata.

[![Se guiden](assets/see-the-guide-sm.png)](create-form-data-model.md)

## Steg 3: Använd regler för anpassningsbara formulärfält {#step-apply-rules-to-adaptive-form-fields}

![07-apply-rules-to-adaptive-form_small](assets/07-apply-rules-to-adaptive-form_small.png)

Anpassade formulär ger en redigerare som kan skriva regler för adaptiva formulärobjekt. Dessa regler definierar åtgärder som ska utlösas av formulärobjekt baserat på förinställda villkor, användarindata och användaråtgärder i formuläret. Det bidrar till att säkerställa att blankettifyllnaden blir korrekt och snabbare.

Mål:

* Skapa och tillämpa regler för anpassningsbara formulärfält.
* Använd regler för att aktivera datamodelltjänster för formulär för att uppdatera data till databasen.

[![Se guiden](assets/see-the-guide-sm.png)](apply-rules-to-adaptive-form-fields.md)

## Steg 4: Formatera en anpassad blankett {#step-style-your-adaptive-form}

![adapative-form-styling](/help/forms/using/assets/09-style-your-adaptive-form-small.png)

Anpassade formulär innehåller teman och en [redigerare](../../forms/using/themes.md) för att skapa teman för de adaptiva formulären. Ett tema innehåller formatinformation för komponenter och paneler, och du kan återanvända ett tema i olika former. Format innehåller egenskaper som bakgrundsfärger, lägesfärger, genomskinlighet, justering och storlek. När du använder temat i formuläret återspeglas det angivna formatet i motsvarande komponenter i formuläret. Anpassningsbara formulär har även stöd för infogad formatering för format som är specifika för ett formulär.

Mål:

* Använd ett av temana i ett anpassat formulär.
* Skapa ett tema för anpassningsbara formulär med temaredigeraren.
* Använd Web Fonts i ett anpassat tema.

[![Se guiden](assets/see-the-guide-sm.png)](style-your-adaptive-form.md)

## Steg 5: Publicera ditt anpassningsbara formulär {#step-publish-your-adaptive-form}

![12-publish-your-adaptive-form-_small](assets/12-publish-your-adaptive-form-_small.png)

Du kan publicera anpassningsbara formulär som ett fristående formulär (enkelsidigt program), inkludera på AEM [sajtsida](/help/forms/using/embed-adaptive-form-aem-sites.md) eller lista på en AEM [!DNL Site] med [Forms Portal](../../forms/using/introduction-publishing-forms.md) .

Mål:

* Publicera det adaptiva formuläret som en AEM-sida.
* Bädda in det adaptiva formuläret på en AEM [!DNL Sites]-sida.
* Bädda in det adaptiva formuläret på en extern webbsida (en icke-AEM-webbsida som ligger utanför AEM).

[![Se guiden](assets/see-the-guide-sm.png)](publish-your-adaptive-form.md)
