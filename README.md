# DMC Chat — dmcchat.com

A real-time web view of the in-game chat on the **destroymc.net** Minecraft server. Players and spectators could follow server chat from a browser without logging into the game. The site reached **1,500+ users in its first month** (2020) and was monetized with Google AdSense.

This repo is the **React frontend**. It was one part of a larger system I designed, built, and ran in production from 2020 to 2022.

![DMC Chat](https://i.gyazo.com/f9ad434a6b84683ac2eb535e769c33f9.jpg)

## How it worked

```
 Minecraft server (destroymc.net)
          │  in-game chat
          ▼
 Mineflayer.js bot ──POST /api/sendMessage──▶ Node.js server (Express + ws, MongoDB)
                                                   │  WebSocket broadcast
 Discord bot (#server_chat) ──WebSocket──────────▶ │
                                                   ▼
                                       React client (this repo) on dmcchat.com
```

- **Mineflayer.js bot:** joined the server as a player, parsed chat, death, and vote messages, and posted them to the API.
- **Node.js server:** Express REST API plus a `ws` WebSocket server. It stored messages in MongoDB and broadcast each new message to every connected client.
- **Discord bot:** relayed messages from the community's Discord channel into the game and onto the site.
- **React client:** loads the last 15 messages, then streams new ones live over a secure WebSocket.

The server and bots live in separate private repos.

## Frontend features

- Live chat feed over `wss://`, with Minecraft color codes rendered in the browser
- Player avatars (Minotar) and player lookups (PlayerDB)
- A drawer showing the online player list and server statistics (TPS, player count)
- Responsive layout with mobile-specific handling (`react-device-detect`)
- Google Analytics and AdSense integration

## Stack

React 16 · Redux · Material-UI · Create React App · WebSockets · deployed with PM2 behind HTTPS

## Running locally

```
git clone https://github.com/codrum/dmc-chat-client.git
cd dmc-chat-client
npm install
npm start
```

The app opens at http://localhost:3000. It expects the WebSocket server on `ws://localhost:9000`. The production server has been shut down, so the feed stays empty without a local server running.
