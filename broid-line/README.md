[npm]: https://img.shields.io/badge/npm-broid-green.svg?style=flat
[npm-url]: https://www.npmjs.com/org/broid

[node]: https://img.shields.io/node/v/@broid/line.svg
[node-url]: https://nodejs.org

[deps]: https://img.shields.io/badge/dependencies-checked-green.svg?style=flat
[deps-url]: #integrations

[tests]: https://img.shields.io/travis/broidHQ/integrations/master.svg
[tests-url]: https://travis-ci.org/broidHQ/integrations

[bithound]: https://img.shields.io/bithound/code/github/broidHQ/integrations.svg
[bithound-url]: https://www.bithound.io/github/broidHQ/integrations

[bithoundscore]: https://www.bithound.io/github/broidHQ/integrations/badges/score.svg
[bithoundscore-url]: https://www.bithound.io/github/broidHQ/integrations

[nsp-checked]: https://img.shields.io/badge/nsp-checked-green.svg?style=flat
[nsp-checked-url]: https://nodesecurity.io

[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![bithound][bithound]][bithound-url]
[![bithoundscore][bithoundscore]][bithoundscore-url]
[![nsp-checked][nsp-checked]][nsp-checked-url]

# Broid Line Integration

Broid Integrations is an open source project providing a suite of Activity Streams 2 libraries for unified communications among a vast number of communication platforms.

> Connect your App to Multiple Messaging Channels with  One OpenSource Language.

[![gitter](https://badges.gitter.im/broidHQ/broid.svg)](https://t.broid.ai/c/Blwjlw?utm_source=github&utm_medium=readme&utm_campaign=top&link=gitter)
[![slackin](https://slackin.broid.ai/badge.svg)](https://slackin.broid.ai)

## Message types supported

| Simple | Image | Video | Buttons | Location | Phone number |
|:------:|:-----:|:-----:|:-------:|:--------:|:------------:|
|   ✅    |   ✅   |   ✅   |    ✅    |    ✅     |              |

_Phone number is platform limitation._

## Getting started

### Install

```bash
npm install --save @broid/line
```

### Connect to Line

```javascript
const BroidLine = require('@broid/line');

const line = new BroidLine({
  token: "<channel_secret>",
  tokenSecret: "<channel_access_token>",
  username: "<channel_id>",
  http: {
    host: "127.0.0.1",
    port: 8080
  }
});

line.connect()
  .subscribe({
    next: data => console.log(data),
    error: err => console.error(`Something went wrong: ${err.message}`),
    complete: () => console.log('complete'),
  });
```

Line can also be used with your existing express setup.

```javascript
const BroidLine = require('@broid/line');
const express = require("express");

const line = new BroidLine({
  token: "<channel_secret>",
  tokenSecret: "<channel_access_token>",
  username: "<channel_id>"
});

const app = express();
app.use("/line", line.getRouter());

line.connect()
  .subscribe({
    next: data => console.log(data),
    error: err => console.error(`Something went wrong: ${err.message}`),
    complete: () => console.log('complete'),
  });

app.listen(8080);
```

**Options available**

| name             | Type     | default    | Description  |
| ---------------- |:--------:| :--------: | --------------------------|
| serviceID        | string   | random     | Arbitrary identifier of the running instance |
| logLevel         | string   | `info`     | Can be : `fatal`, `error`, `warn`, `info`, `debug`, `trace` |
| token            | string   |            | Your Channel Secret |
| tokenSecret      | string   |            | Your Channel Access Token |
| username         | string   |            | Your Channel ID |
| http             | object   | `{ "port": 8080, "http": "0.0.0.0" }` | WebServer options (`host`, `port`) |

### Quick tips

- The Reply token can be found in object.context
- In One-one chat, the target object is fill with the actor informations.
- Image and Video buffer are not supported so the url will be fill with https://buffer_not_supported.broid.ai
- Because Line doesn't provide informations about the sender in Group, Context.
The actor is object is fill with fake informations.
- Line support only Image, Video https url

### Receive a message

```javascript
line.listen()
  .subscribe({
    next: data => console.log(`Received message: ${data}`),
    error: err => console.error(`Something went wrong: ${err.message}`),
    complete: () => console.log('complete'),
  });
```

## Buttons supported

| mediaType           | Action types  | Content of value property |
| ------------------- |:-------------:| --------------------------|
| text/html           | Action.URI      | URL to be opened in the built-in browser. |
|                     | Action.POSTBACK | Text of message which client will sent back as ordinary chat message. |

### Post a message

To send a message, the format should use the [broid-schemas](https://github.com/broidHQ/integrations/tree/master/broid-schemas).

```javascript
const formatted_message = {
  "@context": "https://www.w3.org/ns/activitystreams",
  "type": "Create",
  "generator": {
    "id": "f6e92eb6-f69e-4eae-8158-06613461cf3a",
    "type": "Service",
    "name": "line"
  },
  "object": {
    "type": "Note",
    "content": "hello world",
    "context": {
      "type": "Object",
      "content": "<reply_token>"
    }
  },
  "to": {
    "type": "Person",
    "id": "U1a2bb4a2fe413ea1c81ad6310c03d624"
  }
};

line.send(formatted_message)
  .then(() => console.log("ok"))
  .catch(err => console.error(err));
```

## Examples of messages

You can find examples of sent and received messages at [Broid-Schemas](https://github.com/broidHQ/integrations/tree/master/broid-schemas).

## Contributing to Broid

See [CONTRIBUTE.md](../CONTRIBUTE.md)

## Copyright & License

Copyright (c) 2016-2017 Broid.ai

This project is licensed under the AGPL 3, which can be
[found here](https://www.gnu.org/licenses/agpl-3.0.en.html).
