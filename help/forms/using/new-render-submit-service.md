---
title: Ny renderings- och skicka-tjänst
description: Definiera renderings- och skicka-tjänster i Workbench för att återge XDP-formulär som HTML eller PDF beroende på vilken enhet det kommer åt.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '891'
ht-degree: 0%

---

# Ny renderings- och skicka-tjänst{#new-render-and-submit-service}

## Introduktion {#introduction}

När du definierar en `AssignTask`-åtgärd i Workbench anger du ett visst formulär (XDP- eller PDF-formulär). Ange även en uppsättning renderings- och Skicka-tjänster via en åtgärdsprofil.

En XDP kan återges som ett PDF-formulär eller ett HTML-formulär. De nya funktionerna innefattar möjligheten att

* Återge och skicka ett XDP-formulär som HTML
* Återge och skicka ett XDP-formulär som PDF på datorn och som HTML på mobila enheter (till exempel en iPad)

### Ny HTML Forms-tjänst {#new-html-forms-service}

Den nya tjänsten HTML Forms använder den nya funktionen i Forms som stöd för återgivning av XDP-formulär som HTML. Den nya tjänsten HTML Forms har följande metoder:

```java
/*
 * Generates a URL (for the HTML Form) to be passed to client, given a TaskContext.
 * The output of this API is something like this - /lc/content/xfaforms/profiles/default.ws.html?ContentRoot=repository://Applications/MyApplication/MyFolder&template=MyForm.xdp
 * @param taskContext task context
 * @param profileName Forms servlet URL.
 * @return form URL string
 */
public String generateFormURL(TaskContext taskContext, String profileName);

/*
 * Render the XDP Form as HTML. Can be used directly for updating the runtimeMap in render.
 * It adds the following keys to the map -
 * hint:new html form = true
 * newHTMLFormURL = the URL returned after calling 'generateFormURL' API.
 * @param TaskContext taskContext
 * @param profileName Forms servlet URL.
 * @param runtimeMap runtime map<string,object> associated with form rendering.
 * return runtimeMap
 */
public Map<String, Object> renderHTMLForm (TaskContext taskContext, String profileName, Map<String,Object> runtimeMap);
```

Mer information om profiler för mobilformulär finns i [Skapa en anpassad profil](/help/forms/using/custom-profile.md).

## Nya HTML Form Render &amp; Submit Processes {#new-html-form-render-amp-submit-processes}

För varje AssignTask-åtgärd anger du en renderings- och en Submit-process med formuläret. Dessa processer anropas av TaskManager `renderForm` och `submitForm` API:er för att tillåta anpassad hantering. Semantics of these processes for New HTML Form:

### Återge ett nytt HTML-formulär {#render-a-new-html-form}

Den nya processen att återge HTML har, precis som vid alla återgivningsprocesser, följande I/O-parametrar -

Indata - `taskContext`

Utdata - `runtimeMap`

Utdata - `outFormDoc`

Den här metoden simulerar det exakta beteendet för `renderHTMLForm`-API:t i NewHTMLFormsService. API:t `generateFormURL` anropas för att hämta URL:en för HTML-återgivning av formuläret. Därefter fylls runtimeMap i med följande nyckel eller värden:

new html form = true

newHTMLFormURL = den URL som returneras efter anrop av `generateFormURL` API.

### Skicka ett nytt HTML-formulär {#submit-a-new-html-form}

Den här processen för att skicka in ett nytt HTML-formulär fungerar med följande I/O-parametrar -

Indata - `taskContext`

Utdata - `runtimeMap`

Utdata - `outputDocument`

Processen ställer in `outputDocument` till `inputDocument`hämtad från `taskContext`.

## Standardprocesser för återgivning och överföring samt åtgärdsprofiler {#default-render-or-submit-processes-and-action-profiles}

Med standardtjänsterna Återgivning och Skicka kan du återge PDF-filer på en dator och HTML på mobila enheter (iPad).

### Standardåtergivningsformulär {#default-render-form}

Den här processen återger ett XDP-formulär på flera plattformar, sömlöst. Processen hämtar användaragenten från `taskContext` och använder data för att anropa processen för att återge antingen HTML eller PDF.

![default-render-form](assets/default-render-form.png)

### Standardformulär för att skicka {#default-submit-form}

Den här processen skickar ett XDP-formulär sömlöst på flera plattformar. Den hämtar användaragenten från `taskContext` och använder data för att anropa processen för att skicka antingen HTML eller PDF.

![default-submit-form](assets/default-submit-form.png)

## Byta återgivning av mobilformulär från PDF till HTML {#switch-the-rendering-of-mobile-forms-from-pdf-to-html}

Webbläsare drar gradvis tillbaka stödet för NPAPI-baserade plugin-program, inklusive plugin-program för Adobe Acrobat och Adobe Acrobat Reader. Du kan ändra återgivning av mobilformulär från PDF till HTML genom att följa dessa steg:

1. Logga in i Workbench som en giltig användare.
1. Välj **Arkiv** > **Hämta program**.

   Dialogrutan Hämta program visas.

1. Markera de program som du vill ändra återgivningen av mobilformulär för och klicka på **OK**.
1. Öppna den process som du vill ändra återgivningen för.
1. Öppna målstartpunkten/målaktiviteten, gå till avsnittet Presentation &amp; Data och klicka på **Hantera åtgärdsprofiler**.

   Dialogrutan Hantera åtgärdsprofiler visas.
1. Ändra standardåtergivningsprofilskonfigurationer från PDF till HTML och klicka på **OK**.
1. Checka in processen.
1. Upprepa stegen för att ändra återgivningen för andra processer.
1. Distribuera det program som är relevant för de processer du har ändrat.

### Standardåtgärdsprofil {#default-action-profile}

Standardåtgärdsprofilen återgav XDP-formuläret som PDF. Det här beteendet har nu ändrats så att standardåtergivningsformuläret och standardskickningsformuläret används.

Några vanliga frågor om åtgärdsprofiler är följande:

![gen_question_b_20](assets/gen_question_b_20.png) **Vilka återgivnings-/sändningsprocesser kommer att vara tillgängliga i kartongen?**

* Återgivningsguide (stödlinjer är inaktuella)
* Återge formulärguide
* Återge PDF-formulär
* Återge HTML-formulär
* Återge nytt HTML-formulär (nytt)
* Standardåtergivningsformulär (nytt)

Och likvärdiga inskickningsprocesser.

![gen_question_b_20](assets/gen_question_b_20.png) **Vilka åtgärdsprofiler kommer att vara tillgängliga från kartongen?**

För XDP Forms:

* Standard (återge/skicka med de nya standardprocesserna för återgivning/sändning)

![gen_question_b_20](assets/gen_question_b_20.png) **Vad behöver processdesignern göra för att formuläret ska kunna återges i HTML på en enhet och i PDF på en dator?**

Ingenting. Standardåtgärdsprofilen väljs automatiskt och återgivningsläget hanteras automatiskt.

![gen_question_b_20](assets/gen_question_b_20.png) **Vad behöver göras för att formuläret ska kunna återges i HTML på en dator?**

Användaren måste välja HTML alternativknapp som standardprofil.

![gen_question_b_20](assets/gen_question_b_20.png) **Kommer uppgraderingen att påverkas om du ändrar standardbeteendet för åtgärdsprofilen?**

Ja, eftersom de tidigare återgivnings- och skicketjänsterna som är kopplade till standardåtgärdsprofilen var olika, behandlas dessa som en anpassning av befintliga formulär. När du klickar på **Återställ standardvärden** anges i stället standardtjänsterna för återgivning och sändning.

Om du har ändrat den befintliga renderings- eller Skicka formulär-tjänsten från PDF eller skapat anpassade tjänster (till exempel custom1) och nu vill använda samma funktioner för HTML-rendering. Du måste replikera den nya renderings- eller skicka-tjänsten (till exempel custom2) och tillämpa liknande anpassningar på dessa. Nu kan du ändra åtgärdsprofilen för XDP-filen så att den börjar använda anpassade2-tjänster i stället för custom1 för rendering eller sändning.

Vad behöver processdesignern göra för att formuläret ska kunna återges i HTML på en enhet och i PDF på en dator?
Vad behöver processdesignern göra för att formuläret ska kunna återges i HTML på en enhet och i PDF på en dator?
