# Whatsapp Multi Session - Connect More Whatsapp Sessions in 1 App

Connect your app with Whatsapp

Lightweight library for whatsapp. No need for Selenium or other browsers.

Standing on the [Baileys](https://github.com/WhiskeySockets/Baileys) Library.

## Installation

Install package using npm

```
npm install regeid
```

Then import your code

Use JS Module

```ts
import * as whatsapp from "regeid";
```

or use CommonJS

```ts
const whatsapp = require("regeid");
```

## Session Usage/Examples

Start New Session

```ts
// create session with ID : rege

const session = await whatsapp.startSession("rege");
// Then, scan the QR on the terminal
```

Get All Session ID

```ts
const sessions = whatsapp.getAllSession();
// returns all session IDs that have been created
```

Get Session Data By ID

```ts
const session = whatsapp.getSession("rege");
// restore session data
```

Load Session From Storage / Load Saved Session

```ts
whatsapp.loadSessionsFromStorage();
// Start a saved session without scanning again
```

## Messaging Usage/Examples

Send Text Message

```ts
await whatsapp.sendTextMessage({
  sessionId: "rege", // session ID
  to: "6282269240005", // always add country code (ex: 62)
  text: "Hi There, This is Message from Server!", // message you want to send
});
```

Send Image

```ts
const image = fs.readFileSync("./myimage.png"); // return Buffer
const send = await whatsapp.sendImage({
  sessionId: "rege",
  to: "6282269240005",
  text: "My Image Caption",
  media: image, // can from URL too
});
```

Send Video

```ts
const video = fs.readFileSync("./video.mp4"); // return Buffer
const send = await whatsapp.sendVideo({
  sessionId: "rege",
  to: "6282269240005",
  text: "My Video Caption",
  media: video, // can from URL too
});
```

Send Document File

```ts
const filename = "file.pdf";
const document = fs.readFileSync(filename); // return Buffer
const send = await whatsapp.sendDocument({
  sessionId: "rege",
  to: "6282269240005",
  filename: filename,
  media: document,
  text: "Hei, Check this Document",
});
```

Read a Message

```ts
await whatsapp.readMessage({
  sessionId: "rege",
  key: msg.key,
});
```

Send Typing Effect

```ts
await whatsapp.sendTyping({
  sessionId: "rege",
  to: "6282269240005",
  duration: 3000,
});
```

## Listener Usage/Examples

Add Listener/Callback When Receive a Message

```ts
whatsapp.onMessageReceived((msg) => {
  console.log(`New Message Received On Session: ${msg.sessionId} >>>`, msg);
});
```

Add Listener/Callback When QR Printed

```ts
whatsapp.onQRUpdated(({ sessionId, qr }) => {
  console.log(qr);
});
```

Add Listener/Callback When Session Connected

```ts
whatsapp.onConnected((sessionId) => {
  console.log("session connected :" + sessionId);
});
```

## Handling Incoming Message Examples

```ts
whatsapp.onMessageReceived(async (msg) => {
  if (msg.key.fromMe || msg.key.remoteJid.includes("status")) return;
  await whatsapp.readMessage({
    sessionId: msg.sessionId,
    key: msg.key,
  });
  await whatsapp.sendTyping({
    sessionId: msg.sessionId,
    to: msg.key.remoteJid,
    duration: 3000,
  });
  await whatsapp.sendTextMessage({
    sessionId: msg.sessionId,
    to: msg.key.remoteJid,
    text: "Hello!",
    answering: msg, // for quoting message
  });
});
```

## Save Media Message (Image, Video, Document)

```ts
wa.onMessageReceived(async (msg) => {
  if (msg.message?.imageMessage) {
    // save image
    msg.saveImage("./image.jpg");
  }

  if (msg.message?.videoMessage) {
    // save video
    msg.saveVideo("./video.mp4");
  }

  if (msg.message?.documentMessage) {
    // save document
    msg.saveDocument("./file"); // without extension
  }
});
```

## Optional Configuration Usage/Examples

Set custom credentials directory

```ts
// default dir is "wa_credentials"
whatsapp.setCredentialsDir("my_custom_dir");
// or : credentials/mycreds
```

Set custom Browser
```ts
// default browser is regeidS
whatsapp.setBrowser("macOS");
// or : custom your name
```

## Authors

- [@regeid](https://www.github.com/regeid)

