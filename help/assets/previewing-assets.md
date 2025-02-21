---
title: Förhandsgranska resurser
description: Lär dig hur du förhandsgranskar resurser, t.ex. videoklipp och bilder, i Dynamic Media genom att använda bildförinställningar och visningsförinställningar.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
feature: Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1369'
ht-degree: 0%

---

# Förhandsgranska resurser med hjälp av programgränssnittet {#previewing-assets}

Du kan använda Förhandsgranska för att se hur en digital resurs som du har överfört ser ut när den visas av en kund i deras egen webbläsare. Standardvisningsprogrammet för inbäddade enheter som är tilldelade resursen används för förhandsvisningen.

Ett visningsprogram är en samling med olika inställningar eller *förinställningar*, till exempel visningsprogrammets visningsstorlek, zoombeteende, färgscheman, kanter och teckensnitt. Dessa förinställningar avgör hur användare visar mediefiler på sina datorskärmar och mobila enheter.

Förutom att använda den dedikerade förhandsvisningsfunktionen för video, snurra och bilduppsättningar kan du även förhandsgranska en resurs med de förinställningar för visningsprogram som du har skapat. Du kan också använda bildförinställningar för att förhandsgranska återgivningar av bilder.

* [Använd bildförinställningar](/help/assets/image-presets.md)
* [Använd förinställningar för visningsprogram](/help/assets/viewer-presets.md)

>[!NOTE]
>
>När du är på en webbsida (Webbplatser) i Adobe Experience Manager kan du inte förhandsgranska resurser i läget **Redigera**. Gå till förhandsgranskningsläget genom att klicka på **[!UICONTROL Preview]** i det övre högra hörnet på sidan.

Information om hur du aktiverar eller inaktiverar visningsförinställningar i användargränssnittet finns i [Hantera visningsförinställningar](/help/assets/managing-viewer-presets.md).

**Så här förhandsgranskar du resurser med programgränssnittet:**

1. Från **[!UICONTROL Adobe Experience Manager]** på sidan **[!UICONTROL Navigation]** väljer du **[!UICONTROL Assets]** och sedan **[!UICONTROL Files]** för att komma åt resurser.
1. I den nedrullningsbara listan **[!UICONTROL View]** i det övre högra hörnet på sidan väljer du **[!UICONTROL List View]**.
1. (Valfritt) Använd kolumnen **[!UICONTROL Type]** för att sortera resurserna efter den typ som du vill förhandsgranska.
1. Klicka på titelnamnet (inte miniatyrbilden) för resursen som du vill förhandsgranska i kolumnen **[!UICONTROL Title]**.
1. Beroende på vilken resurstyp du klickade på gör du något av följande:


   <table>
    <tbody>
      <tr>
      <td><strong>Resurstyp som du klickade på</strong><br /> </td>
      <td><strong>Kan du förhandsgranska mediefilen i en viss återgivning?</strong></td>
      <td><strong>Kan du förhandsgranska resurser i ett visningsprogram?</strong></td>
      <td> </td>
      </tr>
      <tr>
      <td><p>3D</p> </td>
      <td>Nej</td>
      <td>Ja</td>
      <td><p><strong>Förhandsgranska en 3D-resurs i Dimensional Viewer</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Visare</strong> i listan och välj sedan Dimensional Viewer.</li>
      <li>Välj <strong>Återställ</strong> om du vill återställa bilden till den ursprungliga zoomningen.</li>
      <li>Välj <strong>Helskärm</strong> om du vill maximera visningsprogrammet på visningsenheten.</li>
      </ul>
      <p><strong>Navigera i 3D-scenen</strong></p>
      <ul>
      <li><p><strong>Vrid din 3D-kamera</strong> - rotera vyn runt 3D-scenen och objekten.</p> Mus: vänsterklicka + dra </p> Touch-screen: Tryck på + dra</p></li>
      <li><p><strong>Panorera kameran</strong> - Panorera vyn åt vänster, åt höger, uppåt och nedåt.</p> Mus: högerklicka + dra </p> Pekskärm: Tryck med två fingrar och dra</p></li>
      <li><p><strong>Zooma kameran</strong> - Zooma kameran så att du kan flytta in och ut från områden i 3D-scenen.</p> Mus: Rullhjul </p> Pekskärm: Fingernypa</p></li>
      <li><p><strong>Ange kameran igen</strong> - Rotera vyn runt 3D-scenen och objekten.</p> Mus: Dubbelklicka </p> Pekskärm: Dubbelmarkera</li></ul></td>
      </tr>
      <tr>
      <td><p>Bild</p> </td>
      <td>Ja</td>
      <td>Ja</td>
      <td><p><strong>Så här förhandsgranskar du en resurs i en viss återgivning</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Återgivningar </strong> i listan och välj sedan en viss återgivning som du vill förhandsgranska.</li>
      </ul> <p><strong>Så här förhandsgranskar du en resurs i ett visst visningsprogram</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Visare</strong> i listan och välj sedan ett visningsprogram som du vill använda för resursen.</li>
      </ul> <p>Använd ikonerna <strong>+</strong> och <strong>-</strong> för att öka respektive minska zoomningen för den markerade bilden. Välj <strong>Återställ</strong> om du vill återställa bilden till den ursprungliga zoomningen.<br /> Om du använder en pekskärm dubbelmarkerar du bilden för att zooma in steg. När du har uppnått maximal zoom dubbelmarkerar du bilden igen för att återställa zoomläget. Dra över bilden för att panorera.</p> </td>
      </tr>
      <tr>
      <td>Multimedia</td>
      <td>Ja</td>
      <td>Ja</td>
      <td><p><strong>Så här förhandsgranskar du en resurs i en viss återgivning</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Återgivningar </strong> i listan och välj sedan en viss återgivning som du vill förhandsgranska.</li>
      </ul> <p>Om du väljer en videoåtergivning med högre upplösning för förhandsgranskning kan videon bli trunkerad. Problemet beror på att återgivningsförhandsvisningen visar exakt den upplösning som dina kunder kommer att se alla i kontexten för det inbäddade visningsprogrammet som används för förhandsgranskningen.</p> <p>När du förhandsgranskar en adaptiv videouppsättning på resursnivå grupperas återgivningarna i en uppspelningsupplevelse. Det innebär att den adaptiva videon har rätt storlek för visning och uppspelning med den bästa upplösningen i visningsenhetens kontext och anslutningshastigheten.<br /> </p> <p><strong>Förhandsgranska en resurs i ett visst visningsprogram</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Visare</strong> i listan och välj sedan ett visningsprogram som du vill använda för resursen.</li>
      </ul> </td>
      </tr>
      <tr>
      <td>Bilduppsättning</td>
      <td>Nej</td>
      <td>Ja</td>
      <td><p><strong>Förhandsgranska en resurs i ett visst visningsprogram</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Visare</strong> i listan och välj sedan ett visningsprogram som du vill använda för resursen.</li>
      </ul> <p>Använd ikonerna <strong>+</strong> och <strong>-</strong> så att du kan öka respektive minska zoomningen för den markerade bilden. Välj <strong>Återställ</strong> om du vill återställa bilden till den ursprungliga zoomningen.<br /> Om du använder en pekskärm dubbelmarkerar du bilden för att zooma in steg. När du har uppnått maximal zoom dubbelmarkerar du bilden igen för att återställa zoomläget. Dra över bilden för att panorera.</p> </td>
      </tr>
      <tr>
      <td>Rotation</td>
      <td>Nej</td>
      <td>Ja</td>
      <td><p><strong>Förhandsgranska en resurs i ett visst visningsprogram</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Visare</strong> i listan och välj sedan ett visningsprogram som du vill använda för resursen.</li>
      </ul> <p>Använd ikonerna <strong>+</strong> och <strong>-</strong> för att öka respektive minska zoomningen för den markerade bilden. Välj <strong>Återställ</strong> om du vill återställa bilden till den ursprungliga zoomningen.<br /> Om du använder en pekskärm dubbelmarkerar du bilden för att zooma in steg. När du har uppnått maximal zoom dubbelmarkerar du bilden igen för att återställa zoomläget. Dra över bilden för att panorera.</p> </td>
      </tr>
      <tr>
      <td>Blandad medieuppsättning</td>
      <td>Nej</td>
      <td>Ja</td>
      <td><p><strong>Förhandsgranska en resurs i ett visst visningsprogram</strong></p>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj <strong>Visare</strong> i listan och välj sedan ett visningsprogram som du vill använda för resursen.</li>
      </ul> <p>Använd ikonerna <strong>+</strong> och <strong>-</strong> för att öka respektive minska zoomningen för den markerade bilden. Välj <strong>Återställ</strong> om du vill återställa bilden till den ursprungliga zoomningen.<br /> Om du använder en pekskärm dubbelmarkerar du bilden för att zooma in steg. När du har uppnått maximal zoom dubbelmarkerar du bilden igen för att återställa zoomläget. Dra över bilden för att panorera.</p> </td>
      </tr>
      <tr>
      <td>Carousel set</td>
      <td>Nej</td>
      <td>Ja</td>
      <td><strong>Om du vill förhandsgranska en resurs i ett visst visningsprogram</strong>
      <ul>
      <li>Klicka på ikonen i det övre vänstra hörnet av sidan så att listrutan visas. Välj ett visningsprogram som du vill använda för resursen.</li>
      </ul> </td>
      </tr>
      <tr>
      <td>360-video<br /> </td>
      <td>Ja</td>
      <td>Ja</td>
      <td><p><strong>Så här förhandsgranskar du en resurs i en viss återgivning</strong></p>
      <ul>
      <li>I närheten av sidans övre vänstra hörn markerar du ikonen så att listrutan visas. Välj <strong>Återgivningar</strong> och välj sedan den återgivning som du vill förhandsgranska.</li>
      </ul> <p><strong>Så här förhandsgranskar du en resurs i ett visst visningsprogram</strong></p>
      <ul>
      <li>I närheten av sidans övre vänstra hörn markerar du ikonen så att listrutan visas. Välj <strong>Visare</strong> och välj sedan ett visningsprogram som du vill använda för resursen.</li>
      </ul> <p>Använd ikonerna <strong>+</strong> och <strong>-</strong> för att öka respektive minska zoomningen för den markerade bilden. Välj <strong>Återställ</strong> om du vill återställa bilden till den ursprungliga zoomningen.<br /> Om du använder en pekskärm dubbelmarkerar du bilden för att zooma in steg. När du har uppnått maximal zoom dubbelmarkerar du bilden igen för att återställa zoomläget. Dra över bilden för att panorera.</p> </td>
      </tr>
    </tbody>
    </table>

## Förhandsgranska resurser med ett tangentbord {#keyboard-navigation-asset-preview}

1. Navigera från Assets användargränssnitt till en mapp som innehåller en resurs som du vill förhandsgranska.

1. Tryck på `<Tab>`-tangenten eller piltangenterna på tangentbordet för att markera resursen i mappen.

1. Tryck på `<Enter>` så att du kan öppna den markerade resursen i förhandsgranskningsläget.

1. Gör något av följande:

   * Om du vill zooma in trycker du på `<Tab>` för att flytta fokus till ikonen för inzoomning (+) och sedan på `<Enter>` en eller flera gånger för att zooma in inkrementellt.
   * Om du vill zooma ut trycker du på `<Tab>` för att flytta fokus till ikonen för att zooma ut (-) och sedan på `<Enter>` en eller flera gånger för att zooma ut stegvis.
   * Om du vill flytta vyn för en *zoomad*-resurs vågrätt eller lodrätt trycker du på respektive piltangent.
   * Tryck på `<Shift>` + `<Tab>` så att du kan återställa vyn och placera fokus tillbaka på resursen.
