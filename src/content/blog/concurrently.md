---
title: 'Mastering Concurrency in Node.js'
description: 'How to run multiple commands simultaneously with concurrently.'
pubDate: 'Dec 06 2025'
heroImage: ''
---

# Running Multiple Commands with Concurrently

When developing full-stack applications, you often need to run a backend server and a frontend development server simultaneously. Dealing with multiple terminal tabs can be annoying. This is where `concurrently` comes in.

## Installation

First, install the package as a development dependency:

```bash
npm install concurrently --save-dev
```

## Usage

You can use it in your `package.json` scripts. For example, if you have a `server` script and a `client` script:

```json
"scripts": {
  "server": "nodemon server.js",
  "client": "npm start --prefix client",
  "dev": "concurrently \"npm run server\" \"npm run client\""
}
```

Now, running `npm run dev` will start both processes in the same terminal window.

## Customization

You can customize the output to make it easier to read:

- `--names`: Give names to your processes.
- `--prefixColors`: Assign colors to each process output.

```bash
concurrently --names "API,APP" --prefixColors "blue,magenta" "npm run server" "npm run client"
```

This simple tool fits perfectly into a minimal workflow, keeping your terminal clean and your focus sharp.
