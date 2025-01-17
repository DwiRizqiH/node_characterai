# Character AI Unofficial Node API

> Node.js client for the unofficial [Character AI API](https://character.ai/), an awesome website which brings characters to life with AI!

## Intro

This repository is inspired by [RichardDorian's unofficial node API](https://github.com/RichardDorian/node-character.ai/).
Though, I found it hard to use and it was not really stable and archived. So I remade it in javascript.

**This project is not affiliated with Character AI in any way! It is a community project.**
The purpose of this project is to bring and build projects powered by Character AI.

If you like this project, please check their [website](https://character.ai/).

## Features

* 👍 Fully written in Javascript and CommonJS (for max compatibility and ease of use)
* ⌚ Asynchronous requests
* 🗣️ Use conversations or use the API to fetch information
* 🧸 Easy to use
* 🔁 Active development
* 👤 Guest & token login support

## Installation

```bash
npm install node_characterai
```

## Usage

Basic guest authentication and message:

```javascript
const CharacterAI = require("node_characterai");
const characterAI = new CharacterAI();

(async () => {
  // Authenticating as a guest (use `.authenticateWithToken()` to use an account)
  await characterAI.authenticateAsGuest();

  // Place your character's id here
  const characterId = "8_1NyR8w1dOXmI1uWaieQcd147hecbdIK7CeEAIrdJw";

  const chat = await characterAI.createOrContinueChat(characterId);

  // Send a message
  const response = await chat.sendAndAwaitResponse("Hello discord mod!", true);

  console.log(response);
  // Use `response.text` to use it as a string
})();
```

## Using an Access Token

Some parts of the API (like managing a conversation) require you to be logged in using an `accessToken` and an `idToken`.

To get it, you can open your browser, go to the [Character.AI website](https://character.ai) in `localStorage`.

> [!IMPORTANT]  
> If you are using old versions of the package and are getting a `Authentication token is invalid`, you now need an `idToken` to authenticate (as of update `1.2.4` and higher). See below.
> 
> **If you are using something that is using the package and has not updated to the latest version in a while, make sure to update the package by doing `npm i` or manually copying the files or open a respective issue to their package (if they have one).**

---
### ⚠️ WARNING: DO NOT share your access token and id token to anyone you do not trust or if you do not know what you're doing. 
#### _Anyone with your access token and id token could have access to your account without your consent. Do this at your own risk._
---

### On PC:
1. Open the Character.AI website in your browser (https://beta.character.ai)
2. Open the developer tools (<kbd>F12</kbd>, <kbd>Ctrl+Shift+I</kbd>, or <kbd>Cmd+J</kbd>)
3. Go to the `Application` tab
4. Go to the `Storage` section and click on `Local Storage`
5. Look for the `@@auth0spajs@@::dyD3gE281MqgISG7FuIXYhL2WEknqZzv::https://auth0.character.ai/::openid profile email offline_access` key
6. Open the body, copy the access token, store it somewhere and then copy the id token.

![Access_Token](https://github.com/realcoloride/node_characterai/assets/108619637/9c830ed3-a28d-4fc0-8220-44c33bf8bf58)

### On Mobile:

1. Open the Character.AI website in your browser (https://beta.character.ai)
2. Open the URL bar, write `javascript:` (case sensitive) and paste the following:
```javascript
(function () {const localStorageKeys = Object.keys(window.localStorage).filter((key) => key.startsWith("@@"));if (localStorageKeys.length === 0) {alert("You need to log in first!");return;}const storageInformation = JSON.parse(window.localStorage[localStorageKeys[0]]).body;const accessToken = storageInformation.access_token;const idToken = storageInformation.id_token;document.documentElement.innerHTML = `<div><p>Here is your access token:</p><input value="${accessToken}" readonly><p>Here is your id token:</p><input value="${idToken}" readonly><p><strong>Do not share this with anyone unless you know what you are doing! Those are your personal access token and id token. If stolen or requested by someone you don't trust, they could access your account without your consent; if so, please close the page immediately.</strong></p><button id="copy" onclick="navigator.clipboard.writeText('${accessToken}'); alert('Copied to clipboard!')">Copy access token to clipboard</button><button id="copy" onclick="navigator.clipboard.writeText('${idToken}'); alert('Copied to clipboard!')">Copy id token to clipboard</button><button onclick="window.location.reload();">Refresh the page</button></div>`;accessToken = null; idToken = null;})();
```
3. The following page should appear:
![Access_Token_Mobile](https://github.com/realcoloride/node_characterai/assets/108619637/50181353-0e55-448f-87e9-e4d3b6457319)
4. Click the respective buttons to copy your access token or id token to your clipboard.
---

When using the package, you can:
* Login as guest using `authenticateAsGuest()` - *for mass usage or testing purposes*
* Login with your account or a token using `authenticateWithToken()` - *for full features and unlimited messaging*

## Finding your character's ID

You can find your character ID in the URL of a Character's chat page.

For example, if you go to the chat page of the character `Discord Moderator` you will see the URL `https://beta.character.ai/chat?char=8_1NyR8w1dOXmI1uWaieQcd147hecbdIK7CeEAIrdJw`.

The last part of the URL is the character ID:
![Character_ID](https://i.imgur.com/nd86fN4.png)

## Image Interactions
### WARNING: This part is currently experimental, if you encounter any problem, open an [**Issue**](https://github.com/realcoloride/node_characterai/issues).

🖼️ Character AI has the ability to generate and interpret images in a conversation. Some characters base this concept into special characters, or maybe use it for recognizing images, or to interact with a character and give it more details on something: *the possibilities are endless*.

💁 Most of the Character AI image features can be used like so:

```javascript
// Most of these functions will return you an URL to the image
await chat.generateImage("dolphins swimming in green water");

// If no mime type (file extension) is specified, the script will automatically detect it
await chat.uploadImage("https://www.example.com/image.jpg");
await chat.uploadImage("./photos/image.jpg");

// Other supported types are Buffers, Readable Streams, File Paths, and URLs
await chat.uploadImage(imageBuffer);

// Including the image relative path is necessary to upload an image
await chat.sendAndAwaitResponse({
  text: "What is in this image?",
  image_rel_path: "https://www.example.com/coffee.jpg",
  image_description: "This is coffee.",
  image_description_type: "HUMAN" // Set this if you are manually saying what the AI is looking at
}, true);
```
*Props to @creepycats for implementing most of this stuff out*

## Troubleshooting

|**Problem**|Answer|
|-------|------|
|❌ **Token was invalid**|Make sure your token is actually valid and you copied your entire token (its pretty long) or, you have not updated the package.|
|🤨 **ID token is missing.**|Read [this](https://github.com/realcoloride/node_characterai?tab=readme-ov-file#using-an-access-token), if problems still persist, feel free to open an Issue.|
|⚠️ **The specified Chromium path for puppeteer could not be located**|On most systems, puppeteer will automatically locate Chromium. But on certain distributions, the path has to be specified manually. This warning occurs if `node_characterai` could not locate Chromium on linux (*/usr/bin/chromium-browser*), and will error if puppeteer cannot locate it automatically. See [this](#specifying-chromiums-path) for a fix.|
|😮 **Why are chromium processes opening?**|This is because as of currently, the simple fetching is broken and I use puppeteer (a chromium browser control library) to go around cloudflare's restrictions.|
|👥 **`authenticateAsGuest()` doesn't work**|See issue [#14](https://github.com/realcoloride/node_characterai/issues/14).|
|🦒 **Hit the max amount of messages?**|Sadly, guest accounts only have a limited amount of messages before they get limited and forced to login. See below for more info 👇|
|🪐 **How to use an account to mass use the library?**|You can use **conversations**, a feature introduced in `1.0.0`, to assign to users and channels. **To reproduce a conversation, use OOC (out of character) to make the AI think you're with multiple people.** __See an example here:__ ![chrome_RDbmXXtFNl](https://user-images.githubusercontent.com/108619637/224778145-284dd89e-7960-499c-b0f0-0deca419c578.png)![chrome_BgF8crPvqC](https://user-images.githubusercontent.com/108619637/224778153-c2a42a26-c5f7-4148-9644-34353482833e.png) (Disclaimer: on some characters, their personality will make them ignore any OOC request).|
|📣 **Is this official?**|No, this project is made by a fan of the website and is unofficial. *To support the developers, please check out [their website](https://beta.character.ai)*.|
|😲 **Did something awesome with `node_characterai`?**|Please let me know!|
|✉️ **Want to contact me?**|See my profile|
|💡 **Have an idea?**|Open an issue in the [**Issues**](https://github.com/realcoloride/node_characterai/issues) tab|
|➕ **Other issue?**|Open an issue in the [**Issues**](https://github.com/realcoloride/node_characterai/issues) tab|

## In-depth troubleshooting
#### **🤚 Before you scroll, please know that:**
* In the `Client` class, you can access the `Requester` class and define puppeteer or other variables related to how CharacterAI will work using `characterAI.requester.(property)`. *Also, anything here is subject to change, so make sure to update the package frequently.*

### 💳 About CharacterAI+
#### **"I am a member of cai+, how do I use it?"**

Change the property `.usePlus` from the requester and if needed, change `.forceWaitingRoom`.

For example:
```javascript
// Default is `false`
characterAI.requester.usePlus = true;
```

### 🧭 About Puppeteer
Around a few months ago, the package only required the `node-fetch` module to run. The package was made using simple API requests.

*However, over time, Cloudflare started fighting against scraping and bots, which also made it almost impossible for anyone to use this package.*

**This is where in versions 1.1 and higher, puppeteer is used (which uses a chromium browser) to make requests with the API.**

### ⚙️ How to change Puppeteer settings
 **👉 IMPORTANT: do the changes *before* you initialize your client!**

In the CharacterAI class, you can access the requester and define the `.puppeteerPath` variable or other arguments, and the properties include *(and are subject to change in future versions)*:
```javascript
// Chromium executable path (in some linux distributions, /usr/bin/chromium-browser)
puppeteerPath;
// Default arguments for when the browser launches
puppeteerLaunchArgs;
// Boolean representing the default timeout (default is 30000ms)
puppeteerNoDefaultTimeout;
// Number representing the default protocol timeout
puppeteerProtocolTimeout;
```

##### Specifying Chromium's path
🐧 For linux users, if your puppeteer doesn't automatically detect the path to Chromium, you will need to specify it manually.

To do this, you just need to set `puppeteerPath` to your Chromium path:
```javascript
characterAI.puppeteerPath = "/path/to/chromium-browser";
```

On Linux, you can use the `which` command to find where Chromium is installed:
```bash
$ which chromium-browser # or whatever command you use to launch chrome
```

💡 I recommend that you __frequently__ update this package for bug fixes and new additions.

## Disclaimer
##### ❤️ This project is updated frequently, **always check for the latest version for new features or bug fixes**.

🚀 If you have an issue or idea, let me know in the [**Issues**](https://github.com/realcoloride/node_characterai/issues) section.

📜 If you use this API, you also bound to the terms of usage of their website.

*(real)coloride - 2023-2024, Licensed MIT.*
