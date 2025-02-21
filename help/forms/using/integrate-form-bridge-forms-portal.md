---
title: Integrera Form Bridge med en anpassad portal för HTML5-blanketter
description: Du kan använda API:t för FormBridge för att hämta eller ange värden för formulärfält från HTML-sidan och skicka formuläret.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# Integrera Form Bridge med en anpassad portal för HTML5-blanketter{#integrating-form-bridge-with-custom-portal-for-html-forms}

FormBridge är ett HTML5 Form Bridge-API som gör att du kan interagera med ett formulär. Mer information om API-referens för FormBridge finns i [API-referens för FormBridge](/help/forms/using/form-bridge-apis.md).

Du kan använda API:t för FormBridge för att hämta eller ange värden för formulärfält från HTML-sidan och skicka formuläret. Du kan till exempel använda API för att skapa en guideliknande upplevelse.

Ett befintligt HTML-program kan använda FormBridge API för att interagera med ett formulär och bädda in det på HTML-sidan. Du kan använda följande steg för att ange värdet för ett fält med hjälp av API:t för Form Bridge.

## Integrera HTML5-formulär på en webbsida {#integrating-html-forms-to-a-web-page}

1. **Välj en profil eller skapa en profil**

   1. I CRX DE-gränssnittet går du till: `https://'[server]:[port]'/crx/de`.
   1. Logga in med administratörsbehörighet.
   1. Skapa en profil eller välj en befintlig profil.

      Mer information om hur du skapar en profil finns i [Skapa en profil](/help/forms/using/custom-profile.md).

1. **Ändra HTML-profilen**

   Inkludera XFA-miljön, XFA-språkbiblioteket och XFA-formulärfragment från HTML i profilåtergivaren, utforma webbsidan och placera formuläret inuti webbsidan.

   Använd till exempel följande kodfragment om du vill skapa en app med två inmatningsfält och ett formulär som visar interaktionen mellan formuläret och en extern app.

   ```xml
   <%@ page session="false"
                  contentType="text/html; charset=utf-8"%><%
   %><%@ taglib prefix="cq" uri="https://www.day.com/taglibs/cq/1.0" %><%
   %><!DOCTYPE html>
   <html manifest="${param.offlineSpec}">
       <head>
          <cq:include script="formRuntime.jsp"/>
           <!-- Portal Scripts and Styles -->
          <cq:include script="portalheader.jsp"/>
       </head>
       <body>
           <div id="leftdiv" >
               <div id="leftdivcontentarea">
                   <!-- Portal Body -->
                 <cq:include script="portalbody.jsp"/>
               </div>
           </div>
           <div id="rightdiv">
               <div id="formBody">
               <cq:include script="config.jsp"/>
               <!-- Form body -->
               <cq:include script="formBody.jsp"/>
               <!  --To assist in page transitions -- add navigation, based on scrolling -->
               <cq:include  script="../nav/scroll/nav_footer.jsp"/>
               <cq:include script="footer.jsp"/>
               </div>
           </div>
       </body>
   </html>
   ```

   >[!NOTE]
   >
   >**Rad 9** innehåller ytterligare JSP-referens för CSS-format och JavaScript-filer för att utforma sidan.
   >
   >
   >Taggen &lt;div id=&quot;rightdiv&quot;> på **rad 18** innehåller HTML-fragmentet för XFA-formuläret.
   >
   >
   Sidan är formaterad i två behållare: **left** och **right**. Den högra behållaren har formuläret. Den vänstra behållaren har två inmatningsfält och en del av den externa HTML-sidan.
   >
   >
   Följande skärmbild visar hur formuläret visas i en webbläsare.

   ![portal](assets/portal.jpg)

   Vänster sida är en del av **HTML-sidan**. Den högra sidan som innehåller fälten är **xfa-formuläret**.

1. **Åtkomst till formulärfält från sidan**

   Följande är ett exempelskript som du kan lägga till för att ange värden i ett formulärfält.

   Om du till exempel vill ställa in **EmployeeName** med värdena i fälten **First Name** och **Last Name** anropar du funktionen **window.formBridge.setFieldValue** .

   På samma sätt kan du läsa värdet genom att anropa API:t **window.formBridge.getFieldValue** .

   ```javascript
   $(function() {
               $(".input").blur(function() {
                   window.formBridge.setFieldValue(
                               'xfa.form.form1.#subform[0].EmployeeName',
                                $("#lname").val()+' '+$("#fname").val()
                              )
                   });
           });
   ```
