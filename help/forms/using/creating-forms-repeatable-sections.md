---
title: Skapa formulär med repeterbara avsnitt
description: Upprepningsbara avsnitt är paneler som kan läggas till eller tas bort dynamiskt i ett formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
discoiquuid: 01724ca0-6901-45e7-b045-f44814ed574e
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 3578d81f-25c6-483b-a2ff-75ccb077ca97
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1137'
ht-degree: 0%

---

# Skapa formulär med repeterbara avsnitt {#creating-forms-with-repeatable-sections}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

Upprepningsbara avsnitt är paneler som kan läggas till eller tas bort dynamiskt i ett formulär.

När du t.ex. ansöker om ett jobb, ger den jobbsökande tidigare anställningsinformation som företagsnamn, roll, projekt och annan information. Information om alla arbetsgivare kräver olika men likartade sektioner. I ett sådant scenario tillhandahåller anställningsblanketten en arbetsgivaravdelning och ger också möjlighet att dynamiskt lägga till fler sådana avsnitt. Dessa avsnitt, som läggs till dynamiskt, kallas upprepningsbara avsnitt.

Du kan använda någon av följande metoder för att skapa repeterbara paneler:

## Använda Instanshanteraren via skript  {#using-instance-manager-via-scripts-nbsp}

1. Markera en panel i redigeringsläget och välj sedan ![cmpr](assets/cmppr.png). Aktivera **Gör panelen upprepningsbar** under Egenskaper i sidofältet. Ange värden för fälten **[!UICONTROL Maximum]** och **[!UICONTROL Minimum]**.

   Fältet Maximum anger det maximala antalet gånger en panel kan visas på sidan. Du kan ange -1 i fältet Maximalt antal om du vill att panelen ska visas ett obegränsat antal gånger.

   Fältet Minimum anger det minsta antalet gånger en panel visas i formuläret. Om du anger fältet Minsta antal till noll senare kan du ta bort alla instanser via skript när återgivningen är klar.

   >[!NOTE]
   >
   >Om du vill skapa en panel som inte är upprepningsbar anger du ett värde i fältet Maximum och Minimum. Dragspelslayouten stöder inte -1 i fältet Maximalt antal. Du kan ange ett högt tal för att ge begreppet oändligt värde.

1. Panelens överordnade panel, som ska upprepas, bör innehålla knappar för att lägga till och ta bort för att hantera instanser av de repeterbara panelerna. Följ de här stegen för att infoga knappar i det överordnade objektet och aktivera skript på knapparna:

   1. Dra och släpp en knappkomponent från sidofältet till panelens överordnade. Markera komponenten och välj ![edit-rules](assets/edit-rules.png). Reglerna för knappen öppnas i regelredigeraren.
   1. Klicka på **Skapa** i regelredigeringsfönstret.

      Välj **Visuell redigerare** på raden Formulärobjekt och funktioner.

      1. I regelområdet, under WHEN, väljer du läge **är klickat**.
      1. Under THEN:

         * Om du vill skapa en knapp för att lägga till en panel väljer du **Lägg till instans** och drar och släpper panelen med ![växlingspanelen](assets/toggle-side-panel.png) eller markerar den med **Släpp objekt eller välj här.**
         * Om du vill skapa en knapp för en borttagningspanel väljer du **Ta bort instans** och drar och släpper panelen med hjälp av ![växlingspanelen](assets/toggle-side-panel.png) eller markerar den med **Släpp objekt eller välj här.**

      Välj **Kodredigeraren** på raden Formulärobjekt och funktioner. Klicka på **Redigera regler** och i kodområdet:

      * Om du vill skapa en knapp för att lägga till panel anger du `this.panel.instanceManager.addInstance()`
      * Om du vill skapa en knapp för borttagningspanelen anger du `this.panel.instanceManager.removeInstance(this.panel.instanceIndex)`

      Klicka på **Klar**.

      >[!NOTE]
      >
      >Om ett fält tillhör en repeterbar panel kan du inte komma åt det direkt med dess namn i skripten. Om du vill komma åt fältet anger du den repeterbara instans som fältet tillhör med API:t `instances` i `InstanceManager`. Syntaxen som ska användas för `instances`-API:t i `InstanceManager` är:
      >
      >
      >`<panelName>.instanceManager.instances[<instanceNumber>].<fieldname>`
      >
      >
      >Du kan till exempel skapa ett anpassat formulär med en upprepningsbar panel med en textruta. När du fyller i formuläret i förväg med tre repeterbara textrutor behöver du XML-koden nedan:
      >
      >
      >`<panel1><textbox1>AA1</panel1></textbox1>`
      >
      >
      >`<panel1><textbox1>AA2</panel1></textbox1>`
      >
      >
      >`<panel1><textbox1>AA3</panel1></textbox1>`
      >
      >
      >Om du vill läsa AA1-data anger du:
      >
      >
      >`Panel1.instanceManager.instances[0].textbox.value`
      >
      >
      >Om du vill läsa AA2-data anger du:
      >
      >
      >`Panel1.instanceManager.instances[1].textbox.value`
      >
      >
      >Mer information finns i Klass: InstanceManager#instances i [AEM Forms Java API-referens](https://adobe.com/go/learn_aemforms_documentation_63).

      >[!NOTE]
      >
      >När alla instanser av en panel tas bort från ett adaptivt formulär kan du lägga till en instans av den borttagna panelen med syntaxen _panelName för att fånga instanshanteraren på panelen och använda API:t addInstance för instanshanteraren för att lägga till den borttagna instansen. Till exempel _panelName.addInstance(). En instans av den borttagna panelen läggs till.

## Använda dragspelslayouten för den överordnade panelen   {#using-the-accordion-layout-for-the-parent-panel-nbsp}

På en panel finns olika layoutalternativ. Alternativet Layout för dragspelsdesign har stöd för upprepningsbara paneler. Utför följande steg på en repeterbar panel med alternativet Layout för dragspelsdesign:

1. Välj ![cmpr](assets/cmppr.png) på den överordnade panelen som ska upprepas. Du kan se egenskaperna i sidlisten. I listrutan **Layout** väljer du **Dragspel**.
1. Välj ![cmppr](assets/cmppr.png) på en panel som ska upprepas. Panelegenskaperna visas i sidofältet. Aktivera fliken **Gör panelen upprepningsbar** och ange ett värde för fälten **Maximum** och **Minimum**.

   Nu kan du använda plusknappen (+) och borttagningsknappen ( ![delete-panel](assets/delete-panel.png)) för att lägga till och ta bort panelerna.

## Använda upprepade delformulär från formulärmallen (XDP/XSD) {#using-repeating-subforms-from-form-template-xdp-xsd}

Upprepningsbart delformulär liknar de repeterbara panelerna i Adaptiv Forms. Gör så här i AEM Forms Designer för att skapa ett upprepande delformulär:

1. Markera det överordnade delformuläret för det delformulär som du vill upprepa på paletten Hierarki.
1. Klicka på fliken Delformulär på paletten Objekt och välj Flödat i listan Innehåll.
1. Markera delformuläret som ska upprepas.
1. Klicka på fliken Delformulär på paletten Objekt och välj antingen Placerat eller Flödat i listan Innehåll.
1. Klicka på fliken Bindning och välj Upprepa delformulär för varje dataobjekt.
1. Om du vill ange det minsta antalet upprepningar väljer du Minsta antal och skriver ett tal i den associerade rutan. Om det här alternativet är inställt på 0 och inga data anges för objekten i delformuläret vid datasammanfogning, placeras inte delformuläret när formuläret återges.
1. Om du vill ange maximalt antal upprepningar av delformulär väljer du Max och skriver ett tal i den tillhörande rutan. Om du inte anger något värde i rutan Max är antalet delformulärrepetitioner obegränsat.
1. Om du vill ange ett visst antal upprepningar av delformulär, oavsett datamängd, väljer du Antal initialer och skriver ett tal i den tillhörande rutan. Om du väljer det här alternativet och antingen inga data är tillgängliga eller det finns färre data än det angivna värdet för antal initialer, placeras tomma instanser av delformuläret fortfarande i formuläret.
1. Lägg till två knappar i det överordnade delformuläret - en för att lägga till en instans och en för att ta bort en instans av det repeterbara delformuläret. Mer information finns i [Skapa en åtgärd](https://help.adobe.com/en_US/AEMForms/6.1/DesignerHelp/WS107c29ade9134a2c74572b5612a87ca2b56-8000.2.html#WS107c29ade9134a2c-1f74d86012a87d4fe55-8000.2).
1. Länka formulärmallen till det anpassade formuläret. Mer information finns i [Skapa ett anpassat formulär baserat på en mall](/help/forms/using/creating-adaptive-form.md#create-an-adaptive-form-based-on-a-template).
1. Använd knapparna i steg 9 för att lägga till och ta bort delformulär.

Den bifogade ZIP-filen innehåller ett exempel på upprepningsbart delformulär.

[Hämta fil](assets/samplerepeatablesubform.zip)

## Använda upprepade inställningar för ett XML-schema (XSD) {#using-repeat-settings-of-an-xml-schema-xsd-br}

Du kan skapa upprepningsbara paneler från ett XML-schema och från egenskapen minOcCours &amp; maxOccurs för alla komplexa tytelement. Mer information om XML-schema finns i [Skapa adaptiva formulär med XML-schema som formulärmodell](/help/forms/using/adaptive-form-xml-schema-form-model.md).

I följande kod använder panelen `SampleType`egenskapen minOcCours &amp; maxOccurs.

```xml
<?xml version="1.0" encoding="utf-8" ?>
    <xs:schema targetNamespace="https://adobe.com/sample.xsd"
                    xmlns="https://adobe.com/sample.xsd"
                    xmlns:xs="https://www.w3.org/2001/XMLSchema"
                >

        <xs:element name="sample" type="SampleType"/>

        <xs:complexType name="SampleType">
            <xs:sequence>
                <xs:element name="leaderName" type="xs:string" default="Enter Name"/>
                <xs:element name="assignmentStartDate" type="xs:date"/>
                <xs:element name="gender" type="GenderEnum"/>
                <xs:element name="noOfProjectsAssigned" type="IntType"/>
                <xs:element name="assignmentDetails" type="AssignmentDetails"
                                            minOccurs="0" maxOccurs="10"/>
            </xs:sequence>
        </xs:complexType>

        <xs:complexType name="AssignmentDetails">
            <xs:attribute name="name" type="xs:string" use="required"/>
            <xs:attribute name="durationOfAssignment" type="xs:unsignedInt" use="required"/>
            <xs:attribute name="numberOfMentees" type="xs:unsignedInt" use="required"/>
             <xs:attribute name="descriptionOfAssignment" type="xs:string" use="required"/>
             <xs:attribute name="financeRelatedProject" type="xs:boolean"/>
       </xs:complexType>
  <xs:simpleType name="IntType">
            <xs:restriction base="xs:int">
            </xs:restriction>
        </xs:simpleType>
  <xs:simpleType name="GenderEnum">
            <xs:restriction base="xs:string">
                <xs:enumeration value="Female"/>
                <xs:enumeration value="Male"/>
            </xs:restriction>
        </xs:simpleType>
    </xs:schema>
```

>[!NOTE]
>
>För icke-dragbar layout använder du adaptiva komponenter för att lägga till och ta bort instanser.
