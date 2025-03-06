---
title: Skapa Flash Builder-program som utför SSO-autentisering med HTTP-tokens
description: Skapa ett klientprogram med Flash Builder som utför SSO-autentisering (single sign on) med HTTP-tokens. Autentisera en användare för en åtgärd en gång och använd autentiseringen för att utföra flera AEM Forms-åtgärder.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: coding
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
exl-id: 6c384e25-f53f-44aa-9043-b9f9f659f987
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1783'
ht-degree: 0%

---

# Skapa Flash Builder-program som utför SSO-autentisering med HTTP-tokens {#creating-flash-builder-applicationsthat-perform-sso-authentication-using-http-tokens}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Du kan skapa ett klientprogram med Flash Builder som utför SSO-autentisering (single sign on) med HTTP-tokens. Anta till exempel att du skapar ett webbaserat program med Flash Builder. Anta sedan att programmet innehåller olika vyer, där varje vy anropar en annan AEM Forms-åtgärd. I stället för att autentisera en användare för varje Forms-åtgärd kan du skapa en inloggningssida där en användare kan autentisera en gång. När användaren har autentiserats kan han eller hon starta flera åtgärder utan att behöva autentisera igen. Om en användare till exempel har loggat in på Workspace (eller ett annat Forms-program) behöver användaren inte autentisera igen.

Även om klientprogrammet innehåller nödvändig programlogik för att utföra SSO-autentisering utför AEM användarhantering den faktiska användarautentiseringen. Om du vill autentisera en användare med HTTP-tokens anropar klientprogrammet Autentiseringshanterarens tjänstens `authenticateWithHTTPToken`-åtgärd. Användarhantering kan autentisera användare med en HTTP-token. För efterföljande fjärr- eller webbtjänstanrop till AEM Forms behöver du inte skicka inloggningsuppgifter för autentisering.

>[!NOTE]
>
>Innan du läser det här avsnittet bör du känna till hur du anropar AEM Forms med Remoting. (Se [Anropa AEM Forms med AEM Forms Remoting](/help/forms/developing/invoking-aem-forms-using-remoting.md#invoking-aem-forms-using-remoting).)

Följande korta AEM Forms-process med namnet `MyApplication/EncryptDocument` anropas efter att en användare har autentiserats med enkel inloggning. (Mer information om den här processen, till exempel in- och utdatavärden, finns i [Exempel på kortlivade processer](/help/forms/developing/aem-forms-processes.md).)

![cf_cf_encryptdocumentProcess2](assets/cf_cf_encryptdocumentprocess2.png)

>[!NOTE]
>
>Processen bygger inte på någon befintlig AEM Forms-process. Om du vill följa med i kodexemplen som beskriver hur du anropar den här processen skapar du en process med namnet `MyApplication/EncryptDocument` med hjälp av workbench. (Se [Använda Workbench](https://www.adobe.com/go/learn_aemforms_workbench_63).)

Klientprogrammet som skapats med Flash Builder interagerar med användarhanterarens säkerhetstjänst som konfigurerats på `/um/login` och `/um/logout`. Det innebär att klientprogrammet skickar en begäran till URL:en `/um/login` under start för att fastställa användarens status. Användarhanteraren svarar sedan med användarstatus. Klientprogrammet och säkerhetstjänsten för användarhanteraren kommunicerar med HTTP.

**Format för förfrågan**

Säkerhetsservern kräver följande indatavariabler:

* `um_no_redirect` - Det här värdet måste vara `true`. Den här variabeln följer alla begäranden som görs till användarhanterarens säkerhetstjänst. Det hjälper även säkerhetstjänsten att skilja på inkommande begäran från en flex-klient eller andra webbprogram.
* `j_username` - Det här värdet är användarens inloggningsidentifierarvärde enligt inloggningsformuläret.
* `j_password` - Det här värdet är användarens motsvarande lösenord enligt inloggningsformuläret.

Värdet `j_password` krävs bara för autentiseringsbegäranden. Om lösenordsvärdet inte anges kontrollerar säkerhetsservern om kontot du använder redan är autentiserat. I så fall kan du fortsätta, men säkerhetsservern autentiserar dig inte igen.

>[!NOTE]
>
>För att i18n ska kunna hanteras på rätt sätt måste dessa värden vara i POST-format.

**Svarsformat**

Säkerhetstjänsten som konfigurerats på `/um/login` svarar med formatet `URLVariables`. I det här formatet är utdata för innehållstypen text/plain. Utdata innehåller namnvärdespar avgränsade med ett et-tecken (&amp;). Svaret innehåller följande variabler:

* `authenticated` - Värdet är antingen `true` eller `false`.
* `authstate` - Det här värdet kan innehålla något av följande värden:

   * `CREDENTIAL_CHALLENGE` - Det här läget indikerar att användarhanteraren inte kan fastställa användarens identitet på något sätt. Användarnamnet och lösenordet krävs för att autentiseringen ska kunna utföras.
   * `SPNEGO_CHALLENGE` - Det här läget behandlas på samma sätt som `CREDENTIAL_CHALLENGE`.
   * `COMPLETE` - Det här tillståndet anger att användarhanteraren kan autentisera användaren.
   * `FAILED` - Det här tillståndet anger att användarhanteraren inte kunde autentisera användaren. Som svar på det här läget kan Flex-klienten visa ett felmeddelande för användaren.
   * `LOGGED_OUT` - Det här tillståndet anger att användaren har loggat ut.

* `assertionid` - Om tillståndet var `COMPLETE` innehåller det användarens `assertionId`-värde. Ett klientprogram kan hämta `AuthResult` för användaren.

**Inloggningsprocess**

När ett klientprogram startas kan du göra en POST-begäran till säkerhetsservern `/um/login`. Exempel: `https://<your_serverhost>:<your_port>/um/login?um_no_redirect=true`. När begäran når användarhanterarens säkerhetstjänst utför den följande steg:

1. Den söker efter en cookie med namnet `lcAuthToken`. Om användaren redan har loggat in i ett annat Forms-program finns denna cookie. Om cookien hittas valideras innehållet.
1. Om huvudbaserad enkel inloggning är aktiverad letar servern efter konfigurerade rubriker för att fastställa användarens identitet.
1. Om SPNEGO är aktiverat försöker servern initiera SPNEGO och försöker avgöra användarens identitet.

Om säkerhetsservern hittar en giltig token som matchar en användare kan du med säkerhetsservern fortsätta och svara med `authstate=COMPLETE`. Annars svarar säkerhetsservern med `authstate=CREDENTIAL_CHALLENGE`. I följande lista förklaras dessa värden:

* `Case authstate=COMPLETE`: Anger att användaren är autentiserad och att värdet `assertionid` innehåller användarens kontrollidentifierare. Klientprogrammet kan nu ansluta till AEM Forms. Servern som konfigurerats för den URL:en kan hämta `AuthResult` för användaren genom att anropa metoden `AuthenticationManager.authenticate(HttpRequestToken)`. Instansen `AuthResult` kan skapa användarhanterarkontexten och lagra den i sessionen.
* `Case authstate=CREDENTIAL_CHALLENGE`: Anger att säkerhetsservern kräver användarens autentiseringsuppgifter. Som svar kan klientprogrammet visa inloggningsskärmen för användaren och skicka de inhämtade autentiseringsuppgifterna till säkerhetsservern (till exempel `https://<your_serverhost>:<your_port>/um/login?um_no_redirect=true&j_username=administrator&j_password=password)`). Om autentiseringen lyckas svarar säkerhetsservern med `authstate=COMPLETE`.

Om autentiseringen fortfarande inte lyckas svarar säkerhetsservern med `authstate=FAILED`. För att svara på det här värdet kan klientprogrammet visa ett meddelande för att hämta inloggningsuppgifterna igen.

>[!NOTE]
>
>Medan `authstate=CREDENTIAL_CHALLENGE` rekommenderas att klienten skickar de inhämtade autentiseringsuppgifterna till säkerhetsservern i POST-format.

**Utloggningsprocess**

När ett klientprogram loggar ut kan du skicka en begäran till följande URL:

`https://<your_serverhost>:<your_port>/um/logout?um_no_redirect=true`

När användarhanterarens säkerhetsserver tar emot den här begäran tar den bort cookien `lcAuthToken` och svarar med `authstate=LOGGED_OUT`. När klientprogrammet har tagit emot det här värdet kan programmet utföra rensningsåtgärder.

## Skapa ett klientprogram som autentiserar AEM-formuläranvändare med enkel inloggning {#creating-a-client-application-that-authenticates-aem-forms-users-using-sso}

Ett exempel på klientprogram skapas för att visa hur du skapar ett klientprogram som utför SSO-autentisering. Följande bild visar stegen som klientprogrammet utför för att autentisera en användare med enkel inloggning.

![cf_cf_flexsso](assets/cf_cf_flexsso.png)

I föregående bild beskrivs det programflöde som inträffar när klientprogrammet startas.

1. Klientprogrammet utlöser händelsen `applicationComplete`.
1. Anropet till `ISSOManager.singleSignOn` görs. Klientprogrammet skickar en begäran till säkerhetstjänsten för användarhanteraren.
1. Om säkerhetsservern autentiserar användaren skickar `ISSOManager` `SSOEvent.AUTHENTICATION_SUCCESS`. Som svar visar klientprogrammet huvudsidan. I det här exemplet anropar huvudsidan den kortlivade AEM Forms-processen MyApplication/EncryptDocument.
1. Om säkerhetsservern inte kan avgöra om användaren är giltig begär programmet inloggningsuppgifter igen. Klassen `ISSOManager` skickar händelsen `SSOEvent.AUTHENTICATION_REQUIRED`. Inloggningssidan visas i klientprogrammet.
1. Autentiseringsuppgifterna på inloggningssidan skickas till metoden `ISSOManager.login`. Om autentiseringen lyckas leder det till steg 3. Annars utlöses händelsen `SSOEvent.AUTHENTICATION_FAILED`. Klientprogrammet visar inloggningssidan och ett felmeddelande.

### Skapa klientprogrammet {#creating-the-client-application}

Klientprogrammet består av följande filer:

* `SSOStandalone.mxml`: Den MXML-huvudfil som representerar klientprogrammet. (Se [Skapa filen SSOStandalone.mxml](creating-flash-builder-applications-perform.md#creating-the-ssostandalone-mxml-file).)
* `um/ISSOManager.as`: Visa åtgärder som rör enkel inloggning (SSO). (Se [Skapa filen ISSOManager.as](creating-flash-builder-applications-perform.md#creating-the-issomanager-as-file).)
* `um/SSOEvent.as`: `SSOEvent` skickas för SSO-relaterade händelser. (Se [Skapa filen SSOEvent.as](creating-flash-builder-applications-perform.md#creating-the-ssoevent-as-file).)
* `um/SSOManager.as`: Hanterar SSO-relaterade åtgärder och skickar lämpliga händelser. (Se [Skapa filen SSOManager.as](creating-flash-builder-applications-perform.md#creating-the-ssomanager-as-file).)
* `um/UserManager.as`: Innehåller programlogik som anropar tjänsten Authentication Manager med hjälp av dess WSDL. (Se [Skapa filen UserManager.as](creating-flash-builder-applications-perform.md#creating-the-usermanager-as-file).)
* `views/login.mxml`: Representerar inloggningsskärmen. (Se [Skapa filen login.mxml](creating-flash-builder-applications-perform.md#creating-the-login-mxml-file).)
* `views/logout.mxml`: Representerar utloggningsskärmen. (Se [Skapa filen logOut.mxml](creating-flash-builder-applications-perform.md#creating-the-logout-mxml-file).)
* `views/progress.mxml`: Representerar en förloppsvy. (Se [Skapa filen progress.mxml](creating-flash-builder-applications-perform.md#creating-the-progress-mxml-file).)
* `views/remoting.mxml`: Representerar vyn som anropar den kortlivade AEM Forms-processen MyApplication/EncryptDocument med hjälp av fjärrkommunikation. (Se [Skapa filen remoting.mxml](creating-flash-builder-applications-perform.md#creating-the-remoting-mxml-file).)

Följande bild ger en visuell representation av klientprogrammet.

![cf_cf_sso_project](assets/cf_cf_sso_project.png)

>[!NOTE]
>
>Observera att det finns två paket som heter um och vyer. När du skapar klientprogrammet måste du placera filerna i deras egna paket. Se även till att du lägger till filen adobe-remoting-provider.swc i projektets klassökväg. (Se [Inkludera AEM Forms Flex-biblioteksfilen](/help/forms/developing/invoking-aem-forms-using-remoting.md#including-the-aem-forms-flex-library-file).)

### Skapa filen SSOStandalone.mxml {#creating-the-ssostandalone-mxml-file}

Följande kod representerar filen SSOStandalone.mxml.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <mx:Application
                 layout="absolute"
                 applicationComplete="initApp()"
                 height="400" width="550"
                 xmlns:v="views.*"
                 backgroundColor="#EDE8F0" viewSourceURL="srcview/index.html">
     <mx:Script>
         <![CDATA[
             import mx.utils.URLUtil;
             import um.SSOEvent;
             import mx.core.UIComponent;
             import um.SSOManager;
             import mx.rpc.events.ResultEvent;
             import mx.utils.ObjectUtil;
             import mx.controls.Alert;
 
             [Bindable]
             private var _serverURL:String;
 
             private var _ssoManager:SSOManager;
 
             private var _progress:UIComponent;
 
             private var _loginPage:UIComponent;
 
             private function initApp():void{
                 _serverURL = determineServerUrl();
                 _ssoManager = new SSOManager(_serverURL);
 
                 _ssoManager.addEventListener(SSOEvent.AUTHENTICATION_FAILED,loginHandler);
                 _ssoManager.addEventListener(SSOEvent.AUTHENTICATION_SUCCESS,loginHandler);
                 _ssoManager.addEventListener(SSOEvent.AUTHENTICATION_REQUIRED,loginHandler);
                 _ssoManager.addEventListener(SSOEvent.LOGOUT_COMPLETE,loginHandler);
                 _ssoManager.addEventListener(SSOEvent.AUTHENTICATION_FAULT,loginHandler);
 
                 trace("[Main] Add the required event handlers for authentication");
                 _ssoManager.singleSignOn();
 
                 showBusy();
             }
 
             private function determineServerUrl():String
             {
                 var s:String ;
                 var appUrl:String = Application.application.url;
                 var givenUrl:String  = ExternalInterface.call("serverUrl.toString");
                 trace("[Main] Application url ["+appUrl+"] Given url ["+givenUrl+"]");
                 if(appUrl != null && appUrl.search("^http") != -1){
                     s = appUrl;
                 }
                 if(s == null){
                     s = givenUrl;
                 }
                 if(s== null){
                     s = "https://hiro-xp:8080/";
                 }
                 s = URLUtil.getFullURL(s,"/");
                 trace("[Main] Would be using ["+s+"] as serverUrl");
                 return s;
             }
 
             private function loginHandler(event:SSOEvent):void
             {
                 trace("[Main] Handling event "+event.type);
                 switch(event.type)
                 {
                     case SSOEvent.AUTHENTICATION_FAILED:
                         viewContent.selectedChild = login;
                         login.showLoginFailed();
                         break;
                     case SSOEvent.AUTHENTICATION_SUCCESS:
                         viewContent.selectedChild = remoting;
                         break;
                     case SSOEvent.AUTHENTICATION_REQUIRED:
                         viewContent.selectedChild = login;
                         break;
                     case SSOEvent.LOGOUT_COMPLETE:
                         viewContent.selectedChild = logout;
                         break;
                     case SSOEvent.AUTHENTICATION_FAULT:
                         Alert.show("Error doing authentication. Root error ["+event.rootEvent+"]","Authentication Fault",Alert.OK);
                 }
             }
 
             public function get ssoManager():SSOManager
             {
                 return _ssoManager;
             }
 
             public function showBusy():void
             {
                 viewContent.selectedChild = progress;
             }
 
             public function get serverUrl():String
             {
                 return _serverURL;
             }
 
         ]]>
     </mx:Script>
     <mx:ViewStack x="0" y="0" id="viewContent" >
         <v:login id="login" />
         <v:remoting id="remoting"  />
         <v:progress id="progress" />
         <v:logout id="logout"/>
     </mx:ViewStack>
 </mx:Application>
 
```

### Skapa filen ISSOManager.as {#creating-the-issomanager-as-file}

Följande kod representerar filen ISSOManager.as.

```java
 package um
 {
     import flash.events.IEventDispatcher;
 
     /**
      * The <code>ISSOManager</code> expose operations related to Single Sign On (SSO) in AEM Forms
      * environment. The application should register appropriate <code>SSOEvent</code> handlers prior
      * to calling any of the following operations
      */
     public interface ISSOManager extends IEventDispatcher
     {
         /**
          * Tries to validate whether the user has an already existing session or not (SSO Scenarios). The application
          * may call this method during the initialization. In general this call would lead to one of the
          * following events getting dispatched
          * <ul>
          * <li>SSOEvent.AUTHENTICATION_SUCCESS - If a SSO session was found and valid
          * <li>SSOEvent.AUTHENTICATION_REQUIRED - No SSO session was found and as such authentication is required in
          * the form of username and password.
          * <li>SSOEvent.AUTHENTICATION_FAULT - Some error has occured while connecting to the server
          * </ul>
          */
         function singleSignOn():void;
 
         /**
          * Authenticates the user using username and password. It may lead to one of the following events
          * <ul>
          * <li>SSOEvent.AUTHENTICATION_SUCCESS - The authentication is successful and a session is established
          * <li>SSOEvent.AUTHENTICATION_FAILED - Authentication has failed
          * </ul>
          */
         function login(username:String, password:String):void;
 
         /**
          * Terminates the current session and logs out the user.
          */
         function logout():void;
 
         /**
          * Get the assertionId for the logged in user
          */
         function get assertionId():String;
     }
 }
```

### Skapa filen SSOEvent.as {#creating-the-ssoevent-as-file}

Följande kod representerar filen SSOEvent.as.

```java
 package um
 {
     import flash.events.Event;
 
     /**
      * The <code>SSOEvent</code> is dispatched for SSO related events
      */
     public class SSOEvent extends Event
     {
         /**
          * This type of event would be dispatched when the Authentication process is successful. Authentication
          * might have been done with SSO or username and password. As a response to this event the application
          * can show the welcome page to the user
          * The application may want to perform specific check for permission/role so as to verify the user is allowed.
          * So as a response to this event the application would do those checks and then only show the welcome page
          */
         public static const AUTHENTICATION_SUCCESS:String = "authenticationSuccess";
 
         /**
          * This type of event would be dispatched when authentication fails using the username, password.
          * As a response to this type of event an application can show an error message to the user.
          * This event would only happen when authentication is done using username and password and NOT in
          * SSO case.
          */
         public static const AUTHENTICATION_FAILED:String = "authenticationFailed";
 
         /**
          * This type of event would be dispatched when authentication using SSO is not achieved. And due to
          * that we require the user's username and password for authentication. As a response to this event
          * the application can show the login page to the user.
          */
         public static const AUTHENTICATION_REQUIRED:String = "authenticationRequired";
 
         /**
          * This type of event would be dispatched when logout is complete. As a response to this event the
          * application may show a logout page informing the user that he has been logged out. Or the application
          * can take the user back to login page
          */
         public static const LOGOUT_COMPLETE:String = "logoutComplete";
 
         /**
          * This type of event would be dispatched when ever there is a problem in doing Authentication. The root cause
          * can be obtained from the <code>rootEvent</code>.
          */
         public static const AUTHENTICATION_FAULT:String = "authenticationFault";
 
         private var _rootEvent:Event;
 
         public function SSOEvent(type:String, rootEvent:Event=null)
         {
             super(type,true,false);
             _rootEvent = rootEvent;
         }
 
         /**
          * The root event. If current event type is <code>AUTHENTICATION_FAULT</code> then it would be an
          * <code>IOErrorEvent</code> in other cases it would be complete event. Its basic use is to extract the root
          * cause if there is an authentication fault.
          */
         public function get rootEvent():Event
         {
             return _rootEvent;
         }
     }
 }
```

### Skapa filen SSOManager.as {#creating-the-ssomanager-as-file}

Följande kod representerar filen SSOManager.as.

```java
 package um
 {
     import flash.events.Event;
     import flash.events.EventDispatcher;
     import flash.events.IOErrorEvent;
     import flash.external.ExternalInterface;
     import flash.net.URLLoader;
     import flash.net.URLLoaderDataFormat;
     import flash.net.URLRequest;
     import flash.net.URLVariables;
 
     import mx.utils.ObjectUtil;
 
     /**
      * Manages the SSO related operations and dispatches appropriate events. It would connect to the UM Filter/Servlet
      * at <code>um/login</code> The UM response would be of form of url encoded variables. It would look for
      * <code>authstate</code> value in the response and depending on that it would proceed.
      *
      * <p>If there is an IO_Error while initial attempt to UM then it would assume it as a 401 response. And it would
      * be assumed that SPNEGO based authenticatin is not working and therefore user would be shown a login page.
      */
     public class SSOManager extends EventDispatcher implements ISSOManager
     {
         private static const SSO_URL:String = "um/login";
         private static const SSO_LOGOUT_URL:String = "um/logout";
         private static const AUTH_COOKIE_NAME:String = "lcAuthToken";
 
         private var _serverUrl:String;
         private var _assertionId:String;
 
         /**
          * Constructs an SSOManager with the given server url.
          *
          * @param serverUrl - The uri of the server to connect to. it must be without any context path for example,
          * http://localhost:8080/. The SSOManager would directly append the path of UM exposed SSO url to it
          * for its operations
          */
         public function SSOManager(serverUrl:String)
         {
             _serverUrl = serverUrl;
         }
 
         public function singleSignOn():void
         {
             sendRequest(SSO_URL,true);
         }
 
         public function login(username:String, password:String):void
         {
             sendRequest(SSO_URL,false,
                 function(request:URLRequest,vars:URLVariables):void
                 {
                     vars.j_username = username;
                     vars.j_password = password;
                 }
             );
         }
 
         public function logout():void
         {
             sendRequest(SSO_LOGOUT_URL);
         }
 
         public function get assertionId():String
         {
             return _assertionId;
         }
 
 
 
         /**
          * Connects to the UM security service.
          */
         private function sendRequest(relativeUrl:String,authenticationRequest:Boolean=false, requestProcessor:Function=null):void
         {
             var loader:URLLoader = new URLLoader();
             loader.dataFormat = URLLoaderDataFormat.VARIABLES;
             var request:URLRequest = new URLRequest(_serverUrl + relativeUrl);
             trace("[SSOmanager] Contacting ["+request.url+"]");
             var vars:URLVariables = new URLVariables();
             vars.um_no_redirect = "true";
             request.data = vars;
             if(requestProcessor != null){
                 requestProcessor(request,vars);
             }
 
             loader.addEventListener(Event.COMPLETE,authHandler);
             //if its an authentication request then only treat io error as a possible 401
             //for others treat them as faults
             if(authenticationRequest){
                 loader.addEventListener(IOErrorEvent.IO_ERROR,httpAuthenticationHandler);
             }else{
                 loader.addEventListener(IOErrorEvent.IO_ERROR,authFaultHandler);
             }
             trace("[SSOmanager] Sending request "+ ObjectUtil.toString(request));
             loader.load(request);
         }
 
         private function authHandler(event:Event):void
         {
             var loader:URLLoader = URLLoader(event.target);
             var response:URLVariables = URLVariables(loader.data);
             trace("[SSOmanager] Processing response ["+ObjectUtil.toString(response)+"]");
             handleAuthResult(response["authstate"],response);
         }
 
         /**
          * Handles the IOErrorEvent. Flash would dispatch IOEvent in response to HTTP 401.
          * There is no way to distinguish it from the genuine IOError.
          */
         private function httpAuthenticationHandler(event:IOErrorEvent):void
         {
             trace("[SSOmanager] Processing IOErrorEvent ["+ObjectUtil.toString(event)+"]");
             handleAuthResult("CREDENTIAL_CHALLENGE");
 
         }
 
         /**
          * Dispatches appropriate <code>SSOEvent</code> on the basis of the <code>authstate</code>
          * value of the response.
          * The response is url encoded in for of
          * <pre>
          * authenticated=false&authstate=SPNEGO_CHALLENGE
          * </pre>
          * Depending on <code>authstate</code> the SSOEvent is dispatched
          */
         private function handleAuthResult(authState:String,response:URLVariables = null):void
         {
             trace("[SSOmanager] processing state "+authState);
             switch(authState)
             {
                 case "FAILED"  :
                     dispatchEvent(new SSOEvent(SSOEvent.AUTHENTICATION_FAILED));
                     break;
                 case "COMPLETE" :
                     _assertionId = response ? response["assertionid"] : null;
                     dispatchEvent(new SSOEvent(SSOEvent.AUTHENTICATION_SUCCESS));
                     break;
                 case "CREDENTIAL_CHALLENGE" :
                     dispatchEvent(new SSOEvent(SSOEvent.AUTHENTICATION_REQUIRED));
                     break;
                 case "LOGGED_OUT" :
                     dispatchEvent(new SSOEvent(SSOEvent.LOGOUT_COMPLETE));
                     break;
                 default:
                     dispatchEvent(new SSOEvent(SSOEvent.AUTHENTICATION_REQUIRED));
                     break;
             }
         }
 
         private function authFaultHandler(event:Event):void
         {
             dispatchEvent(new SSOEvent(SSOEvent.AUTHENTICATION_FAULT,event));
         }
 
     }
 }
```

### Skapar filen UserManager.as {#creating-the-usermanager-as-file}

Följande kod representerar filen UserManager.as.

```java
 package um
 {
     import flash.events.Event;
     import mx.rpc.soap.WebService;
     import mx.rpc.soap.Operation;
     import mx.rpc.IResponder;
     import mx.rpc.events.FaultEvent;
     import mx.rpc.events.ResultEvent;
     import mx.rpc.soap.LoadEvent;
 
     public class UserManager
     {
         private var _ssoManager:ISSOManager;
         private var _serverUrl:String;
 
         public function UserManager(ssoManager:ISSOManager,serverUrl:String)
         {
             _serverUrl = serverUrl;
             _ssoManager = ssoManager;
         }
 
         public function retrieveAssertion(responder:IResponder):String
         {
             var assertionId:String = _ssoManager.assertionId;
             if(!assertionId)
             {
                 trace("[UserManager] AssertionId not found");
                 return null;
             }
 
             var ws:WebService = new WebService();
             var wsdl:String = _serverUrl+'soap/services/AuthenticationManagerService?wsdl&lc_version=8.2.1';
             ws.loadWSDL(wsdl);
             ws.addEventListener(LoadEvent.LOAD,
                 function(event:Event):void
                 {
                     trace("[UserManager] WSDL loaded");
                     var authenticate:Operation = ws.authenticateWithHttpToken as Operation;
                     authenticate.resultFormat = "e4x";
                     authenticate.addEventListener(ResultEvent.RESULT,
                         function(event:Event):void
                         {
                             responder.result(event);
                         }
                     );
                     authenticate.send({assertionId:assertionId});
                 }
             );
 
             ws.addEventListener(FaultEvent.FAULT,
                 function(event:Event):void
                 {
                     responder.fault(event);
                 }
             );
             return null;
         }
     }
 }
```

### Skapa filen login.mxml {#creating-the-login-mxml-file}

Följande kod representerar filen login.mxml.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <mx:Canvas  width="500" height="400">
     <mx:Script>
         <![CDATA[
             import mx.core.Application;
             public function showLoginFailed():void
             {
                 loginMessage.text = "Username or Password incorrect";
             }
 
             private function doLogin():void
             {
                 Application.application.ssoManager.login(j_username.text,j_password.text);
                 Application.application.showBusy();
             }
 
         ]]>
     </mx:Script>
 
     <mx:VBox height="113" width="244" x="128" y="144" horizontalAlign="center" verticalGap="10">
         <mx:HBox width="100%">
             <mx:HBox width="100%" verticalAlign="middle" horizontalAlign="center" height="32">
                 <mx:Label text="Username" fontWeight="bold"/>
                 <mx:TextInput id="j_username"/>
             </mx:HBox>
         </mx:HBox>
         <mx:HBox width="100%" height="33" horizontalAlign="center" horizontalGap="10" verticalAlign="middle">
             <mx:Label text="Password" fontWeight="bold"/>
             <mx:TextInput displayAsPassword="true" id="j_password"/>
         </mx:HBox>
         <mx:Button label="Login" click="doLogin()"/>
     </mx:VBox>
     <mx:Text x="128" y="122" id="loginMessage" width="230" height="14"/>
     <mx:Label x="154" y="65" text="AEM Forms SSO Demo" fontFamily="Georgia" fontSize="20" color="#0A0A0A"/>
 </mx:Canvas>
 
```

### Skapa filen logOut.mxml {#creating-the-logout-mxml-file}

Följande kod representerar filen logOut.mxml.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <mx:Canvas  width="500" height="400">
     <mx:Label x="97" y="188" text="You have successfully logged out from the application"/>
 
 </mx:Canvas>
 
```

### Skapa filen progress.mxml {#creating-the-progress-mxml-file}

Följande kod representerar filen progress.mxml.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <mx:Canvas >
     <mx:Label x="151" y="141" text="Wait...."/>
     <mx:SWFLoader source="LoadingCircle.swf" width="50" height="50" horizontalCenter="0" verticalCenter="0"/>
 </mx:Canvas>
```

### Skapa filen remoting.mxml {#creating-the-remoting-mxml-file}

Följande kod representerar filen remoting.mxml som anropar `MyApplication/EncryptDocument`-processen. Eftersom ett dokument skickas till processen finns programlogik som ansvarar för att skicka ett säkert dokument till AEM Forms i den här filen. (Se [Skicka säkra dokument för att anropa processer med Remoting](/help/forms/developing/invoking-aem-forms-using-remoting.md#passing-secure-documents-to-invoke-processes-using-remoting).)

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <mx:Canvas  width="664" height="400" creationComplete="initializeChannelSet()" xmlns:views="views.*">
     <mx:Script>
 
         <![CDATA[
 
             import mx.rpc.livecycle.DocumentReference;
             import flash.net.FileReference;
             import flash.net.URLRequest;
             import flash.events.Event;
             import flash.events.DataEvent;
             import mx.messaging.ChannelSet;
             import mx.messaging.channels.AMFChannel;
             import mx.rpc.events.ResultEvent;
             import mx.collections.ArrayCollection;
             import mx.rpc.AsyncToken;
             import um.UserManager;
             import mx.rpc.events.ResultEvent;
             import mx.rpc.events.FaultEvent;
             import mx.core.Application;
             import mx.rpc.Responder;
             import mx.utils.ObjectUtil;
 
             // Classes used in file retrieval
             private var fileRef:FileReference = new FileReference();
             private var docRef:DocumentReference = new DocumentReference();
             private var parentResourcePath:String = "/";
             //private var serverPort:String = "'[server]:[port]'";
             private var serverPort:String = "'[server]:[port]'";
             private var now1:Date;
             private var userManager:UserManager;
 
             // Define a ChannelSet object.
             public var cs:ChannelSet;
 
             // Holds information returned from AEM Forms
             [Bindable]
             public var progressList:ArrayCollection = new ArrayCollection();
 
 
             // Set up channel set to invoke AEM Forms.
             // This must be done before calling any service or process, but only
             // once for the entire application.
             private function initializeChannelSet():void {
                 cs = new ChannelSet();
                 cs.addChannel(new AMFChannel("remoting-amf", "https://" + serverPort + "/remoting/messagebroker/amf"));
                 EncryptDocument.channelSet = cs;
 
             //Get the user that is authenticated
             userManager = new UserManager(Application.application.ssoManager,Application.application.serverUrl);
             userManager.retrieveAssertion(
                     new mx.rpc.Responder(
                         function(event:ResultEvent):void
                         {
                             var name:String = XML(event.currentTarget.lastResult)..*::authenticatedUser.*::userid.text();
                             username.text = "Welcome "+name;
                         },
                         function(event:FaultEvent):void
                         {
                             mx.controls.Alert.show(event.fault.faultString,'Error')
                         }
                     )
                 );
 
             }
 
             // Call this method to upload the file.
             // This creates a file picker and lets the user select a PDF file to pass to the EncryptDocument process.
             private function uploadFile():void {
                 fileRef.addEventListener(Event.SELECT, selectHandler);
                 fileRef.addEventListener(DataEvent.UPLOAD_COMPLETE_DATA,completeHandler);
                 fileRef.browse();
             }
 
             // Gets called for selected file. Does the actual upload via the file upload servlet.
             private function selectHandler(event:Event):void {
                 var authTokenService:RemoteObject = new RemoteObject("LC.FileUploadAuthenticator");
                 authTokenService.addEventListener("result", authTokenReceived);
                 authTokenService.channelSet = cs;
                 authTokenService.getFileUploadToken();
             }
 
             private function authTokenReceived(event:ResultEvent):void
             {
                 var token:String = event.result as String;
                 var request:URLRequest = DocumentReference.constructRequestForUpload("https://hiro-xp:8080", token);
 
                 try
                 {
                     fileRef.upload(request);
                 }
                 catch (error:Error)
                 {
                     trace("Unable to upload file.");
                 }
             }
 
             // Called once the file is completely uploaded.
             private function completeHandler(event:DataEvent):void {
 
                 // Set the docRefs url and referenceType parameters
                 docRef.url = event.data as String;
                 docRef.referenceType=DocumentReference.REF_TYPE_URL;
                 executeInvokeProcess();
             }
 
             //This method invokes the EncryptDocument process
             public function executeInvokeProcess():void {
                 //Create an Object to store the input value for the EncryptDocument process
                 now1 = new Date();
 
                 var params:Object = new Object();
                 params["inDoc"]=docRef;
 
                 // Invoke the EncryptDocument process
                 var token:AsyncToken;
                 token = EncryptDocument.invoke(params);
                 token.name = name;
             }
 
 
             // This method handles a successful conversion invocation
             public function handleResult(event:ResultEvent):void
             {
 
                 //Retrieve information returned from the service invocation
                 var token:AsyncToken = event.token;
                 var res:Object = event.result;
                 var dr:DocumentReference = res["outDoc"] as DocumentReference;
                 var now2:Date = new Date();
 
                 // These fields map to columns in the DataGrid
                 var progObject:Object = new Object();
                 progObject.filename = token.name;
                 progObject.timing = (now2.time - now1.time).toString();
                 progObject.state = "Success";
                 progObject.link = "<a href='" + dr.url + "'> open </a>";
                 progressList.addItem(progObject);
             }
 
 
             private function resultHandler(event:ResultEvent):void {
             // Do anything else here.
 
             }
 
             private function logout():void
             {
                 Application.application.ssoManager.logout();
                 Application.application.showBusy();
             }
 
 
         ]]>
 
     </mx:Script>
 
     <mx:RemoteObject id="EncryptDocument" destination="MyApplication/EncryptDocument" result="resultHandler(event);">
             <mx:method name="invoke" result="handleResult(event)"/>
     </mx:RemoteObject>
 
 
     <!--//This consists of what is displayed on the webpage-->
     <mx:Panel id="lcPanel" title="EncryptDocument  (Deprecated for AEM forms) AEM Forms Remoting Example"
           height="25%" width="25%" paddingTop="10" paddingLeft="10" paddingRight="10"
           paddingBottom="10">
         <mx:Label width="100%" color="blue"
                   id="username"/>
 
         <mx:DataGrid x="10" y="0" width="500" id="idProgress" editable="false"
                          dataProvider="{progressList}" height="231" selectable="false" >
         <mx:columns>
                 <mx:DataGridColumn headerText="Filename" width="200" dataField="filename" editable="false"/>
                 <mx:DataGridColumn headerText="State" width="75" dataField="state" editable="false"/>
                 <mx:DataGridColumn headerText="Timing" width="75" dataField="timing" editable="false"/>
                 <mx:DataGridColumn headerText="Click to Open" dataField="link" editable="false" >
                 <mx:itemRenderer>
 
                         <mx:Component>
                         <mx:Text x="0" y="0" width="100%" htmlText="{data.link}"/>
                         </mx:Component>
                     </mx:itemRenderer>
             </mx:DataGridColumn>
         </mx:columns>
     </mx:DataGrid>
     <mx:Button label="Select File" click="uploadFile()" />
     <mx:Button label="Logout"  click="logout()" />
     </mx:Panel>
 </mx:Canvas>
 
 
```

### Ytterligare information {#additional-information}

I följande avsnitt finns ytterligare information som beskriver kommunikationen mellan klientprogrammet och säkerhetstjänsten för användarhanteraren.

### En ny autentisering sker {#a-new-authentication-occurs}

I så fall försöker användaren logga in från ett klientprogram till AEM Forms för första gången. (det finns ingen föregående session där användaren är inblandad.) I händelsen `applicationComplete` anropas metoden `SSOManager.singleSignOn` som skickar en begäran till användarhanteraren.

`GET /um/login?um%5Fno%5Fredirect=true HTTP/1.1`

Användarhanterarens säkerhetstjänst svarar med följande värde:

`HTTP/1.1 200 OK`

`authenticated=false&authstate=CREDENTIAL_CHALLENGE`

Som svar på det här värdet skickas ett `SSOEvent.AUTHENTICATION_REQUIRED`-värde. Klientprogrammet visar därför en inloggningsskärm för användaren. Autentiseringsuppgifterna skickas tillbaka till användarhanterarens säkerhetstjänst.

`GET /um/login?um%5Fno%5Fredirect=true&j%5Fusername=administrator&j%5Fpassword=password HTTP/1.1`

Användarhanterarens säkerhetstjänst svarar med följande värde:

```verilog
 HTTP/1.1 200 OK
 Set-Cookie: lcAuthToken=53630BC8-F6D4-F588-5D5B-4668EFB2EC7A; Path=/
 authenticated=true&authstate=COMPLETE&assertionid=53630BC8-F6D4-F588-5D5B-4668EFB2EC7A
```

Därför skickas `authstate=COMPLETE the SSOEvent.AUTHENTICATION_SUCCESS`. Klientprogrammet kan utföra ytterligare bearbetning om det behövs. En logg som till exempel spårar datumet och tiden som användaren autentiserades kan skapas.

### Användaren är redan autentiserad {#the-user-is-already-authenticated}

I det här fallet har användaren redan loggat in på AEM Forms och sedan navigerar till klientprogrammet. Klientprogrammet ansluter till säkerhetstjänsten för användarhanteraren under start.

```verilog
 GET /um/login?um%5Fno%5Fredirect=true HTTP/1.1
 Cookie: JSESSIONID=A4E0BCC2DD4BCCD3167C45FA350BD72A; lcAuthToken=53630BC8-F6D4-F588-5D5B-4668EFB2EC7A
```

Eftersom användaren redan är autentiserad finns cookie-filen för användarhanteraren och skickas till säkerhetsservern för användarhanteraren. Servern hämtar sedan värdet `assertionId` och verifierar om det är giltigt. Om den är giltig returneras `authstate=COMPLETE`. Annars returneras `authstate=CREDENTIAL_CHALLENGE`. Här följer ett typiskt svar:

```verilog
 HTTP/1.1 200 OK
        authenticated=true&authstate=COMPLETE&assertionid=53630BC8-F6D4-F588-5D5B-4668EFB2EC7A
```

I det här fallet visas inte användaren på någon inloggningsskärm utan direkt på en välkomstskärm.
