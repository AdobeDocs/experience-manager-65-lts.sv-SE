---
title: Vanliga frågor och svar om HTML5-blanketter
description: Vanliga frågor och svar om layout, stöd för skript och omfång för HTML5-formulär.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: fc98390c-8517-46fc-9f15-bac5d4327c60
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2007'
ht-degree: 0%

---

# Vanliga frågor och svar om HTML5-blanketter{#frequently-asked-questions-faq-for-html-forms}

Det finns några vanliga frågor och svar om layout, skriptstöd och omfattningen av HTML5-formulär.

## Layout {#layout}

1. Varför visas inte streckkoder och signaturfält i mitt formulär?

   Svar: Streckkoder och signaturfält är inte relevanta i HTML eller mobilscenarier. Dessa fält visas som icke-interaktiva områden. AEM Forms Designer har dock ett nytt signaturskriptfält som kan användas i stället för signaturfält. Du kan också lägga till en [anpassad widget](../../forms/using/custom-widgets.md) för streckkoder och integrera den.

1. Stöds RTF för XFA-textfältet?

   Svar: XFA-fältet, som tillåter multimediematerial i AEM Forms Designer, stöds inte och återges som normal text utan stöd för formatering av texten från användargränssnittet. Dessutom visas XFA-fält med kombinationsegenskaper som ett vanligt fält, även om det fortfarande finns begränsningar för antalet tillåtna tecken baserat på värdet för kombinationssiffror.

1. Finns det några begränsningar för användning av repeterbara delformulär?

   Svar: Upprepningsbara delformulär ska ha ett initialt antal på 1 eller fler. Upprepningsbara delformulär med ett ursprungligt värde på noll stöds inte. Du kan också välja att använda ett repeterbart delformulär och inte visa det när formuläret läses in. Så här uppnår du användningsfallet:

   1. Ange startvärdet för det repeterbara delformuläret till 1.

      ![Inledande antal](assets/intial-count.png)

   1. Använd händelsen initialize för formuläret för att dölja den primära instansen av delformuläret. Koden nedan döljer till exempel den primära instansen av delformuläret vid formulärinitiering. Den verifierar också apptypen för att säkerställa att skriptet bara körs på klientsidan:

      ```javascript
      if ((xfa.host.appType == "HTML 5" || xfa.host.appType == "Exchange-Pro" || xfa.host.appType == "Reader")&&(_RepeatSubform.count == 1)&&(form1.Page1.Subform1.RepeatSubform.Key.rawValue == null)) {
      RepeatSubform.presence = "hidden";
      }
      ```

   1. Öppna skriptet för att lägga till en instans av delformuläret för redigering. Lägg till koden som nedan för att lägga till en instans av delformulärsskriptet.

      Koden nedan kontrollerar den dolda instansen av delformuläret. Om den dolda instansen av delformuläret hittas tar du bort den dolda instansen av delformuläret och infogar en ny instans av delformuläret. Om den dolda instansen av delformuläret inte hittas infogar du bara en ny instans av delformuläret.

      ```javascript
      if (RepeatSubform.presence == "hidden")
      {
      RepeatSubform.instanceManager.insertInstance(0);
      RepeatSubform.instanceManager.removeInstance(1);
      }
      else
      {
      RepeatSubform.instanceManager.addInstance(1);
      }
      ```

   1. Öppna skriptet för att ta bort en instans av delformuläret för redigering. Lägg till koden så här för att ta bort en instans av skriptet Delformulär.

      Antal kodkontroller för delformulären. Om antalet delformulär är 1 döljs delformuläret i stället för att delformuläret tas bort.

      ```javascript
      if (RepeatSubform.instanceManager.count == 1) {
      RepeatSubform.presence = "hidden";
      } else {
      RepeatSubform.instanceManager.removeInstance(RepeatSubform.instanceManager.count - 1);
      }
      ```

   1. Öppna händelsen för att skicka formuläret i förväg för redigering. Lägg till följande skript i händelsen för att ta bort den dolda instansen av skriptet innan du redigerar. Det förhindrar att data i det dolda delformuläret skickas in.

      ```javascript
      if(RepeatSubform.instanceManager.count == 1 && RepeatSubform.presence == "hidden") {
      RepeatSubform.instanceManager.removeInstance(0);
      }
      ```

1. Finns det några begränsningar när det gäller att använda dolda delformulär?

   Svar: Ett dolt delformulär med komplex hierarki som delas upp på flera sidor orsakar layoutproblem. Du kan komma runt problemet genom att markera delformuläret som synligt från början och sedan dölja det i ett initieringsskript baserat på viss logik eller data.

1. Varför viss text trunkeras eller visas felaktigt i HTML5?

   Svar: Om ett Draw- eller Caption-textelement inte har fått tillräckligt med utrymme för att visa innehåll, visas texten som trunkerad i mobil formuläråtergivning. Den här trunkeringen visas även i designvyn i AEM Forms Designer. Även om den här trunkeringen kan hanteras i PDF-filer kan den inte hanteras i HTML5-formulär. Du kan undvika problemet genom att ange tillräckligt med utrymme för Rita eller Bildtext så att den inte kortas av i designläget i AEM Forms Designer.

1. Jag observerar layoutproblem med saknat innehåll eller överlappande innehåll. Vad är orsaken?

   Svar: Om det finns ett element av typen Rita text eller Rita bild tillsammans med ett annat överlappande element på samma plats (till exempel en rektangel), visas inte innehållet i Rita text om det kommer senare i dokumentordningen (i vyn AEM Forms Designer Hierarki). PDF har stöd för genomskinliga lager, men HTML/webbläsare har inte stöd för genomskinliga lager.

1. Varför visas vissa teckensnitt i HTML-formuläret på ett annat sätt än de som används när formuläret utformas?

   Svar: HTML5 Forms tillåter inte inbäddning av teckensnitt (till skillnad från PDF forms där teckensnitt är inbäddade i formuläret). För att HTML-versionen av ett formulär ska kunna återges som förväntat måste teckensnitten vara tillgängliga i CRX-databasen (AEM Content Repository) på AEM Forms-servern och på den dator där AEM Designer är installerat. När teckensnitten inte är tillgängliga i CRX-databasen på AEM Forms-servern eller på den plats där AEM Designer är installerat, återges formuläret med reservteckensnitt.

1. Stöds vAlign- och hAlign-attribut i HTML-formulär?

   Svar: Ja, attributen vAlign och hAlign stöds. Attributet vAlign stöds inte i Internet Explorer och i flerradsfält.

1. Har HTML5-blanketter stöd för hebreiska tecken?

   Svar: HTML5-formulär stöder hebreiska tecken i alla webbläsare utom Microsoft Internet Explorer.

1. Har HTML5-formulär några begränsningar för numeriska fält?

   Svar: Ja, HTML5-formulär har några begränsningar. Om antalet siffror är fler än antalet som anges i bildsatsen, lokaliseras inte siffrorna och visas på engelska.

1. Varför är HTML-blanketter större än PDF forms?

   Svar: Många mellanliggande datastrukturer och objekt som blankettdom, datatilldom och layoutdom krävs för att återge en XDP-fil till ett HTML-formulär.

   För PDF forms har Adobe Acrobat en inbyggd XTG-motor för att skapa mellanliggande datastrukturer och objekt. Acrobat hanterar också layout och skript.

   För HTML5-formulär har webbläsarna ingen inbyggd XTG-motor för att skapa mellanliggande datastrukturer och objekt från rå XDP-byte. För HTML5-formulär genereras därför mellanliggande strukturer på servern och skickas till klienten. På klienten använder JavaScript-baserade skript- och layoutmotorer dessa mellanliggande strukturer.

   Storleken på den mellanliggande strukturen beror på storleken på den ursprungliga XDP-filen och de data som sammanfogas med XDP-filen.

1. Finns det några begränsningar när det gäller att använda tabeller i min xdp?

   Svar: Komplexa tabeller orsakar problem vid återgivning.

   * Avsnitt (SubformSet) inuti en tabell stöds inte.
   * Tabellhuvuds- och sidfotsrader i vissa tabeller markeras för upprepning. Att dela upp sådana tabeller på flera sidor kan få vissa problem.

1. Har tillgängliga tabeller några begränsningar?

   Svar: Ja, tillgängliga tabeller har följande begränsningar:

   * Kapslade tabeller och delformulär i en tabell stöds inte.
   * Rubriker stöds bara för tabellens övre och vänstra kolumner. Huvuden stöds inte för element i mellantabeller. Du kan använda rubriker på flera rad- och kolumnrubriker, förutsatt att alla sådana rader och kolumner finns tillsammans med den översta raden eller kolumnen längst till vänster i tabellen.
   * `Rowspan` och `colspan` från en slumpmässig plats i tabellen stöds inte.

   * Du kan inte lägga till eller ta bort instanser av rader som innehåller element med ett radintervallvärde som är större än 1.

1. Vilken läsordning har skärmläsare verktygstips och bildtexter?

   Svar:
   * När både bildtext och verktygstips finns, läses den enda bildtexten. Om bildtexten inte är tillgänglig läses funktionsbeskrivningen. Du kan också ange prioritet för läsning i en XDP-fil med hjälp av formulärdesignern
   * När du håller muspekaren över ett element visas verktygstipset. Om funktionsbeskrivningen inte är tillgänglig visas taltext. Om taltext inte är tillgänglig visas fältnamnet.

1. När du håller pekaren över ett fält visas ett verktygstips. Hur inaktiverar man det?

   Svar: Om du vill inaktivera verktygstipset när du hovrar väljer du inget på hjälpmedelspanelen i Designer.

1. I Designer kan en användare konfigurera anpassade utseendeegenskaper för alternativknappar och kryssrutor. Tar HTML5-formulär hänsyn till anpassade utseendeegenskaper när formulären återges?

   Svar: HTML5-formulär ignorerar de anpassade utseendeegenskaperna för alternativknappar och kryssrutor. Alternativknapparna och kryssrutorna visas enligt specifikationerna för den underliggande webbläsaren.

1. När ett HTML5-formulär öppnas i en webbläsare som stöds justeras inte kanten på de fält som placeras intill korrekt, eller så visas delformulär som överlappande. När samma HTML5-formulär förhandsgranskas i Forms Designer ser inte fält och layout feljusterade ut och delformulär visas på rätt plats. Hur löser jag problemet?

   Svar: När ett delformulär är inställt på att flöda innehåll och delformuläret har ett dolt ramelement, justeras inte kanten på de fält som placeras intill korrekt eller så visas delformulär som överlappande. Du kan lösa problemet genom att ta bort eller kommentera dolda &lt;border>-element från motsvarande XDP. Följande &lt;border>-element markeras som en kommentar:

   ```xml
               <!--<border>
                  <edge presence="hidden"/>
                  <corner thickness="0.175mm" presence="hidden"/>
               </border> -->
   ```

1. Varför fungerar inte skärmläsare korrekt med fältobjektet Datum/tid?

   Svar: Skärmläsare stöder inte datum-/tidsfält. Du kan dock manuellt ange datum/tid för fältet så att skärmläsaren läser det. Använd verktygstips eller skärmläsartext för att instruera användaren att manuellt välja datum/tid för fältet.

1. Har HTML5-formulär stöd för visningsmönster för flytande fält?

   Svar: HTML5-formulär har inte stöd för visningsmönster för flytande fält.

1. Vilket format har datumfältet i HTML5 Forms?
Svar: Datumfältet accepterar ISO-formatet ÅÅÅÅ-MM-DD. Om du anger ett datum i något annat format accepterar inte datumfältet formateringen förrän användaren tabbar ut ur fältet.

### Skript {#scripting}

1. Finns det några begränsningar i JavaScript-implementeringen för HTML Forms?

   Svar:

   * Stödet för xfa.connectionSet-skriptet är begränsat. För connectionSet stöds endast anrop på serversidan av webbtjänsten. Mer information finns i [Skriptstöd](/help/forms/using/scripting-support.md).
   * Det finns inget stöd för $record och $data i klientskript. Men om skripten skrivs i ett formReady, layoutReady-block fungerar skripten fortfarande eftersom dessa händelser körs på serversidan.
   * XFA Draw-elementspecifika skript som att ändra Draw-texten (eller Bildtext om det finns fält) stöds inte.

1. Finns det några begränsningar för att använda FormCalc?

   Svar: Endast en delmängd av FormCalc-skripten är för närvarande implementerade. Mer information finns i [Skriptstöd](/help/forms/using/scripting-support.md).

1. Finns det någon rekommenderad namnkonvention och finns det några reserverade nyckelord att undvika?

   Svar:
   * I AEM Forms Designer rekommenderar vi att du inte börjar namnet på ett objekt (till exempel ett delformulär eller ett textfält) med ett understreck (_). Om du vill använda understreck i början av namnet lägger du till ett prefix efter understrecket_&lt;prefix>&lt;objektnamn>.
   * Alla HTML5-formulär-API:er är reserverade nyckelord. Använd ett namn som inte är identiskt med [HTML5-formulär-API:er](/help/forms/using/scripting-support.md) för anpassade API:er/funktioner.

1. Har HTML5-formulär stöd för flytande fält?

   Svar: Ja, HTML5 Forms stöder flytande fält. Om du vill aktivera flytande fält lägger du till följande egenskap i återgivningsprofilen:

   >[!NOTE]
   >
   >Som standard är fälten inte aktiverade för flytande. Du kan använda Forms Designer för att ställa in den flytande egenskapen för fälten.

   1. Öppna CRXde lite och navigera till noden `/content/xfaforms/profiles/default`.
   1. Lägg till egenskapen `mfDataDependentFloatingField` av typen String och ange värdet för egenskapen till `true`.
   1. Klicka på **Spara alla**. Nu aktiveras de flytande fälten för HTML Forms med den uppdaterade återgivningsprofilen.

      >[!NOTE]
      >
      >Om du vill aktivera flytande fält för ett visst formulär utan att uppdatera återgivningsprofilen skickar du egenskapen mfDataDependentFloatingField=true som en URL-parameter.

1. Körs initieringsskriptet och formulärready-händelsen flera gånger i HTML5-formulär?

   Svar: Ja, initieringsskripten och händelser som är klara för formulär körs flera gånger, minst en gång på servern och en gång på klientsidan. Det rekommenderas att skriva skript som initialize eller form:ready-händelser baserat på viss affärslogik (formulär- eller fältdata) så att åtgärden utförs baserat på data och idempotent (om data är samma).

### Utforma XDP {#designing-xdp}

1. Finns det några reserverade nyckelord i HTML5-formulär?

   Svar: Alla HTML5-formulär-API:er är reserverade nyckelord. Använd ett namn som inte är identiskt med [HTML5-formulär-API:er](/help/forms/using/scripting-support.md) för anpassade API:er/funktioner. Förutom reserverade nyckelord bör du lägga till ett unikt prefix efter understrecket om du använder objektnamn som börjar med ett understreck (_). Genom att lägga till ett prefix undviker du eventuella konflikter med interna API:er för HTML5-formulär. Exempel: `_fpField1`
