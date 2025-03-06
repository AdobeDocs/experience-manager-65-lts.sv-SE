---
title: Konfigurera Xcode-projektet och bygg iOS-appen
description: Beskriver hur du skapar AEM Forms standardprogram för iOS.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: c5092e61-c3f9-4770-91be-247e6a02cdb4
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Konfigurera Xcode-projektet och bygg iOS-appen{#set-up-the-xcode-project-and-build-the-ios-app}

AEM Forms tillhandahåller den fullständiga källkoden för AEM Forms-appen. Källan innehåller alla komponenter som behövs för att skapa en anpassad AEM Forms-app. Källkodsarkivet, `adobe-lc-mobileworkspace-src-<version>.zip`, är en del av paketet `adobe-aemfd-forms-app-src-pkg-<version>.zip` för programvarudistribution.

Så här hämtar du programkällan för AEM Forms:

1. Öppna [Programvarudistribution](https://experience.adobe.com/downloads). Du behöver en Adobe ID för att logga in på Software Distribution.
1. Välj **[!UICONTROL Adobe Experience Manager]** som finns på rubrikmenyn.
1. I avsnittet **[!UICONTROL Filters]**:
   1. Välj **[!UICONTROL Forms]** i listrutan **[!UICONTROL Solution]**.
   2. Välj version och typ för paketet. Du kan också använda alternativet **[!UICONTROL Search Downloads]** för att filtrera resultaten.
1. Välj det paketnamn som gäller för ditt operativsystem, välj **[!UICONTROL Accept EULA Terms]** och välj **[!UICONTROL Download]**.
1. Öppna [Pakethanteraren](https://experienceleague.adobe.com/docs/experience-manager-65-lts/administering/contentmanagement/package-manager.html) och klicka på **[!UICONTROL Upload Package]** för att överföra paketet.
1. Markera paketet och klicka på **[!UICONTROL Install]**.

1. Om du vill hämta källkodsarkivet öppnar du `https://<server>:<port>/crx/de/content/forms/mobileapps/src/adobe-lc-mobileworkspace-src-<version>.zip` i webbläsaren.
Källpaketet hämtas till din enhet.

Följande bild visar det extraherade innehållet i `adobe-lc-mobileworkspace-src-<version>.zip`.

![mws-content](assets/mws-content.png)

Följande tabellinformation innehåller information om innehållet i mappen `adobe-lc-mobileworkspace-src-[version]/ios`.

<table>
 <tbody>
  <tr>
   <th><p>Katalog</p> </th>
   <th><p>Innehåll</p> </th>
  </tr>
  <tr>
   <td><p><code>CordovaLib</code></p> </td>
   <td><p>PhoneGap SDK 6.4.0</p> </td>
  </tr>
  <tr>
   <td><p><code>AEM Forms</code></p> </td>
   <td><p>Resurser, plugin-program för PhoneGap och programmets huvudmodul</p> </td>
  </tr>
  <tr>
   <td><p><code>AEM Forms.xcodeproj</code></p> </td>
   <td><p>Xcode project for AEM Forms app</p> </td>
  </tr>
  <tr>
   <td><p><code>www</code></p> </td>
   <td><p>HTML, CSS, bilder och JavaScript-filer för AEM Forms-appprojekt</p> </td>
  </tr>
 </tbody>
</table>

Mer information om kodsignering och hur du lägger till enheter i iOS Provisioning Portal finns i [iOS Code Signing Setup, Process och Troubleshooting](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html).

## Bygg AEM Forms standardapp {#set-up-the-xcode-project}

1. Utför följande steg för att konfigurera ett projekt i Xcode och ange en signeringsidentitet:

   Logga in på en Mac-dator där Xcode och iOS SDK är installerade och konfigurerade.

1. Kopiera arkivet `adobe-lc-mobileworkspace-src-<version>.zip` från hämtningsmappen till `[User_Home]/Projects/`.
1. Extrahera arkivet i katalogen `[User_Home]/Projects/[your-project]`.
1. Navigera till katalogen ` [User_Home]/Projects/ `[ditt projekt]`/adobe-lc-mobileworkspace-src-[version]/ios`.
1. Öppna projektet `AEM Forms.xcodeproj` i Xcode.
1. Klicka på **AEM Forms** och välj **AEM Forms** under **MÅL**. Välj fliken **Skapa inställningar**, leta upp avsnittet **Kodsigneringsberättigande** och gör något av följande i fälten Felsök och Frigör:

   * Lämna fälten ospecificerade för att skapa en Workspace-standardapp för mobiler
   * Ange de fält som ska användas enligt beskrivningen i [Skapa en säker AEM Forms-app för iOS](/help/forms/using/building-secure-mobile-workspace-app.md) för att skapa en säker AEM Forms-app.

1. Klicka på **Alla** på fliken **Bygginställningar** och sedan på **Kombinerad**.
1. Expandera **Kodsignering** från listan **Inställningar**.
1. Välj lämplig signatur för **Kodsigneringsidentitet**. Mer information om hur du skapar nya signaturer finns i [Skapa och hämta provisioneringsprofiler för utveckling](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/CreatingYourTeamProvisioningProfile/CreatingYourTeamProvisioningProfile.html).
1. Se till att samma signatur är markerad för **Felsök**, **Versionsinformation** och **Alla iOS SDK**.
1. Ersätt följande kod i filen `AEM Forms-info.plist`:

   ```xml
   <key>NSAppTransportSecurity</key>
   <dict>
   <key>NSAllowsArbitraryLoads</key>
   <true/>
   </dict>
   ```

   med följande när `yourserver.com` ersätts med ett lämpligt värdnamn för servern.

   ```xml
   <key>NSAppTransportSecurity</key>
   <dict>
   <key>NSExceptionDomains</key>
   <dict>
   <key>yourserver.com</key>
   <dict>
   <!-Include to allow subdomains->
   <key>NSIncludesSubdomains</key>
   <true/>
   <!-Include to allow HTTP requests->
   <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
   <true/>
   <!-Include to support forward secrecy->
   <key>NSExceptionRequiresForwardSecrecy</key>
   <false/>
   <!-Include to specify minimum TLS version->
   <key>NSTemporaryExceptionMinimumTLSVersion</key>
   <string>TLSv1.1</string>
   </dict>
   </dict>
   </dict>
   ```

   >[!NOTE]
   >
   >Det här steget krävs bara om AEM Forms-appen behöver ansluta till en server som inte uppfyller kraven för App Transport Security.

1. Under **PROJEKT** väljer du **AEM Forms** och kontrollerar att rätt signatur har valts för **Kodsigneringsidentitet**, **Felsök**, **Versionsinformation** och **Alla iOS SDK**.
1. Anslut en etablerad iPad till en Mac-dator.
1. Välj den tilldelade enheten för **AEM Forms** -projektet.

   ![iPad](assets/ipad.png)

   En etablerad enhet, iPad Air 2, har valts.

1. Välj **Produkt** > **Rengör**.
1. Välj **Produkt** > **Version**.

## Skapa installationsprogrammet för AEM Forms-appen {#build-the-installer-for-the-mobile-workspace-app}

Du måste arkivera Xcode-projektet för att skapa installationsprogrammet (en IPA-fil) och en egenskapslista (en PLIST-fil). Egenskapslistfilen innehåller konfigurationsinformation för det värdbaserade interna programmet, till exempel namnet och appens värdplats. Mer information om egenskapslistfilen finns i [Om informationsegenskapslistfiler](https://developer.apple.com/library/ios/#documentation/general/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html).

1. Anslut en etablerad iPad till en Mac-dator. Mer information om hur du etablerar en iPad finns i [Skapa och hämta provisioneringsprofiler för utveckling](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/CreatingYourTeamProvisioningProfile/CreatingYourTeamProvisioningProfile.html)
1. Välj den tilldelade enheten för **AEM Forms** -projektet.

   ![ipad-1](assets/ipad-1.png)

   En etablerad enhet, iPad Air 2, har valts.

1. Välj **Produkt** > **Rengör**.
1. Välj **Produkt** > **Version**.
1. Välj **Produkt** > **Arkiv**.
1. I Sorteraren - Arkiv väljer du det senaste arkivet för ditt projekt och klickar på **Distribuera**.
1. Välj **Spara för företag eller Ad-hoc-distribution** som distributionsmetod och klicka på **Nästa**.
1. Välj lämplig **kodsigneringsidentitet** och klicka på **Nästa**. Klicka på **Tillåt** för att använda signaturen.
1. Ange appens namn och välj **Spara för företagsdistribution**.
1. Ange programmets **program-URL**. Om du till exempel vill ha appen på en CRX-server anger du URL:en `https://[LC_host]:'port'/lc/content/distribution/mobileworkspace/APP_NAME.ipa`.
1. Ange AEM Forms i fältet **Titel**.
1. Klicka på **Spara** och stäng Xcode.

   En installationsfil, `AEM Forms.ipa`, och egenskapslistefil, `AEM Forms-info.plist`, skapas på den angivna platsen.

1. Öppna filen `AEM Forms-info.plist` i en redigerare.
1. Ersätt alla blanksteg i URL:en för din .ipa-fil med %20.
1. Spara och stäng filen `AEM Forms-info.plist`.
