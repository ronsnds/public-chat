# Public Chat

A simple real-time public chat application built with [Vue 3](https://vuejs.org/) and [Firebase Firestore](https://firebase.google.com/docs/firestore).  
Users can send and view messages instantly in a shared chat room.

## Features

- Real-time messaging using Firebase Firestore
- Modern UI with Vue 3 and Vite
- Responsive and clean design

## Project Structure

```
.
├── index.html
├── package.json
├── firebase.ts
├── src/
│   ├── App.vue
│   ├── main.ts
│   ├── style.css
│   ├── components/
│   └── assets/
│       └── vue.svg
└── ...
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18+ recommended)
- [pnpm](https://pnpm.io/) or [npm](https://www.npmjs.com/)

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/ronsnds/public-chat.git
   cd public-chat
   ```

2. Install dependencies:
   ```sh
   pnpm install
   # or
   npm install
   ```

### Running the App

Start the development server:
```sh
pnpm dev
# or
npm run dev
```
Open [http://localhost:5173](http://localhost:5173) in your browser.

### Building for Production

```sh
pnpm build
# or
npm run build
```

### Preview Production Build

```sh
pnpm preview
# or
npm run preview
```

## Configuration

- Firebase configuration is set in [`firebase.ts`](firebase.ts).
- Environment variables can be placed in a `.env` file.

## License

MIT

