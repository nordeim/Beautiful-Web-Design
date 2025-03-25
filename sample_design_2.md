First, we need to update the `package.json` file to include all the necessary dependencies mentioned in the design document.

```language:package.json
{
  // ... existing code ...
  "dependencies": {
    // ... existing dependencies ...
    "@headlessui/react": "^2.2.0",
    "@prisma/client": "^6.5.0",
    "@react-three/fiber": "^9.1.0",
    "@reduxjs/toolkit": "^2.6.1",
    "axios": "^1.8.4",
    "bcryptjs": "^3.0.2",
    "dotenv": "^16.4.7",
    "framer-motion": "^12.5.0",
    "next-auth": "^4.24.11",
    "react-redux": "^9.2.0",
    "redux": "^5.0.1",
    "three": "^0.174.0"
  },
  "devDependencies": {
    // ... existing devDependencies ...
  }
}
```
This update adds dependencies for UI components (`@headlessui/react`, `framer-motion`), 3D/AR (`three`, `@react-three/fiber`), state management (`redux`, `react-redux`, `@reduxjs/toolkit`), backend and database related packages (`@prisma/client`, `bcryptjs`, `dotenv`, `axios`, `next-auth`).

Next, let's configure TailwindCSS in `tailwind.config.js` to include the luxury brand's color scheme.

```language:tailwind.config.js
// ... existing code ...
module.exports = {
  content: ["./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      colors: {
        gold: "#d4af37",
        black: "#1a1a1a",
      },
    },
  },
  plugins: [],
};
```
This adds `gold` and `black` colors to the TailwindCSS theme, which are central to the luxury aesthetic.

Now, let's update the global styles in `src/app/globals.css` (or `src/styles/globals.css` if you are using the older `pages` directory). We will add the Tailwind directives and a custom CSS layer for luxury styles as suggested in the design document.

```language:src/app/globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .luxury-gradient {
    background: linear-gradient(135deg, #1a1a1a 0%, #4d4d4d 100%);
  }
  .gold-text {
    color: #d4af37;
  }
}
```
This adds the basic Tailwind directives and defines custom CSS classes like `luxury-gradient` and `gold-text` within the `@layer components` directive for reusability.

Let's create the `Hero` section component as `src/components/Hero.tsx`.

```language:src/app/components/Hero.tsx
```typescript
// src/app/components/Hero.tsx
const Hero = () => (
  <div className="relative h-screen overflow-hidden">
    <video
      autoPlay
      loop
      muted
      className="absolute top-0 left-0 w-full h-full object-cover"
    >
      <source src="/videos/fragrance.mp4" type="video/mp4" />
    </video>
    <div className="absolute inset-0 bg-black opacity-40"></div>
    <div className="relative z-10 flex h-full items-center justify-center text-white">
      <h1 className="text-5xl font-bold px-4 text-center">
        Discover the Art of Fragrance
      </h1>
    </div>
  </div>
);
export default Hero;
```
This component implements a full-screen hero section with a video background and a text overlay, as described in the design document. You will need to place a video file named `fragrance.mp4` in the `public/videos/` directory.

Now, let's create the `ARViewer` component as `src/components/ARViewer.tsx`.

```language:src/app/components/ARViewer.tsx
```typescript
// src/app/components/ARViewer.tsx
import { Canvas } from "@react-three/fiber";
import { OrbitControls, Environment } from "@react-three/drei";

const ARViewer = ({ modelPath }) => (
  <Canvas className="h-[500px]">
    <ambientLight intensity={0.5} />
    <spotLight position={[10, 10, 10]} angle={0.15} />
    <OrbitControls />
    <Environment preset="studio" />
    <mesh>
      <primitive object={modelPath} />
    </mesh>
  </Canvas>
);
export default ARViewer;
```
This component sets up a basic AR viewer using `react-three-fiber` and `drei`. It includes orbit controls and environment lighting for a simple 3D model visualization. You'll need to pass the `modelPath` prop to specify the 3D model to be loaded.

Let's create the `Chatbot` component as `src/components/Chatbot.tsx`.

```language:src/app/components/Chatbot.tsx
```typescript
// src/app/components/Chatbot.tsx
import { useState } from "react";

const Chatbot = () => {
  const [query, setQuery] = useState("");
  const [response, setResponse] = useState("");

  const handleSend = async () => {
    const res = await fetch("/api/chatbot", {
      method: "POST",
      body: JSON.stringify({ query }),
    });
    const data = await res.json();
    setResponse(data.answer);
  };

  return (
    <div className="fixed bottom-4 right-4 bg-white p-4 rounded-lg">
      <input
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        className="border p-2 mr-2"
      />
      <button onClick={handleSend} className="bg-gold text-white px-4 py-2">
        Ask
      </button>
      <div className="mt-2">{response}</div>
    </div>
  );
};
export default Chatbot;
```
This component provides a basic chatbot UI with an input field and a send button. It sends the user query to the `/api/chatbot` endpoint (which you'll need to create in `src/app/api/chatbot/route.ts` for the app directory or `src/pages/api/chatbot.ts` for the pages directory) and displays the response.

Now, let's update the `src/app/page.tsx` to use these components and structure the homepage.

```language:src/app/page.tsx
```typescript
// src/app/page.tsx
import Hero from "@/app/components/Hero";
// import ARViewer from "@/app/components/ARViewer"; // Uncomment if you want to add ARViewer to the homepage
// import Chatbot from "@/app/components/Chatbot";   // Uncomment if you want to add Chatbot to the homepage

export default function Home() {
  return (
    <div>
      <Hero />
      {/* <ARViewer modelPath="/models/perfume-bottle.gltf" /> */} {/* Uncomment to include ARViewer */}
      {/* <Chatbot /> */} {/* Uncomment to include Chatbot */}
      <main className="p-8">
        <section className="mb-16">
          <h2 className="text-3xl font-bold mb-8">Featured Products</h2>
          {/* Product listing will go here */}
        </section>
        <section className="mb-16">
          <h2 className="text-3xl font-bold mb-8">About Guerlain</h2>
          {/* About section content */}
        </section>
        <section>
          <h2 className="text-3xl font-bold mb-8">Contact Us</h2>
          {/* Contact form or information */}
        </section>
      </main>
    </div>
  );
}
```
This updated `page.tsx` replaces the default content with the `Hero` component and adds sections for featured products, about information, and contact details. I've commented out the `ARViewer` and `Chatbot` imports and usage for now, you can uncomment them to include these components on the homepage. You would also need to place a 3D model file named `perfume-bottle.gltf` in the `public/models/` directory if you uncomment `ARViewer`.

To set up Redux, you would need to create a store and potentially a cart slice as described in the design document. Here's an example of `src/store/cartSlice.ts`:

```language:src/app/store/cartSlice.ts
```typescript
// src/app/store/cartSlice.ts
import { createSlice } from "@reduxjs/toolkit";

const cartSlice = createSlice({
  name: "cart",
  initialState: { items: [] },
  reducers: {
    addItem: (state, action) => {
      state.items.push(action.payload);
    },
  },
});

export const { addItem } = cartSlice.actions;
export default cartSlice.reducer;
```
And configure the store in `src/store/store.ts`:

```language:src/app/store/store.ts
```typescript
// src/app/store/store.ts
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './cartSlice';

export const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```
Then you would need to wrap your `App` component in `src/app/layout.tsx` with a Redux Provider to make the store available to your components.

For the backend and database setup with Prisma and Docker, you would follow the steps outlined in the design document to initialize Prisma, define the schema, and set up Docker Compose for PostgreSQL. These steps involve using command-line tools and creating configuration files as described in the `sample_luxury-website_gpt4o.md` document.

Please remember to install the new dependencies by running `npm install` after updating `package.json`.

This provides a starting point for implementing the luxury brand website design. You can further expand on these components and features to fully realize the design outlined in `sample_luxury-website_gpt4o.md`.
