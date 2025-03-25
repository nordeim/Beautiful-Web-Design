\<think\>
Approach this task of creating a luxury brand website based on the [Guerlain](https://www.guerlain.com/us/en-us) blueprint systematically. You will need to think deeply about the implementation options and create a comprehensive solution that captures the elegance and functionality of a high-end brand website.

First, you will analyze the technical requirements and design philosophy:

1. The Guerlain blueprint emphasizes:
   - Elegant visuals with grid-based layouts and high-resolution imagery
   - Black, gold, and white color scheme that conveys luxury
   - Smooth animations and interactions
   - Advanced features like AR product previews and AI chatbot
   - Performance optimization with server-side rendering

2. Technology stack considerations:
   - Next.js + TypeScript for frontend with SSR capabilities
   - Redux Toolkit for state management
   - Three.js for AR/3D integration
   - Tailwind CSS for styling with custom luxury theme
   - Node.js/Express for any backend needs

3. Key implementation decisions:
   - Should use a modern, component-based architecture
   - Need responsive design that maintains luxury feel on all devices
   - Need proper state management for cart, user preferences, etc.
   - AR integration must be accessible but not intrusive
   - Design system should ensure consistent luxury aesthetic

Now start by planning the file structure and then implement the key components of the website.

The file structure will follow a standard Next.js pattern with organized components, pages, and utilities. You will create reusable UI components for the luxury aesthetic while ensuring performance and accessibility.

Now implement the essential files for this luxury website, starting with configuration files and then creating the UI components, pages, and features.
\<\/think\>

# Luxury Brand Website Implementation

Based on the sample project below as your starting port to create a comprehensive implementation for a luxury brand website using Next.js. Your implementation should include all the necessary files and code to create a beautiful, performant, and feature-rich website that exemplifies luxury branding.

## Project Structure

```
guerlain-redesign/
├── public/               # Static assets
│   ├── images/
│   ├── videos/
│   └── models/           # 3D models for AR
├── src/
│   ├── components/       # Reusable UI components
│   ├── pages/            # Next.js pages
│   ├── store/            # Redux state management
│   ├── styles/           # Global styles
│   └── types/            # TypeScript type definitions
├── .env                  # Environment variables
├── next.config.js        # Next.js configuration
├── tailwind.config.js    # Tailwind CSS configuration
├── tsconfig.json         # TypeScript configuration
└── package.json          # Dependencies and scripts
```

## Core Configuration Files

### package.json

```json
{
  "name": "guerlain-redesign",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "@reduxjs/toolkit": "^2.0.1",
    "@react-three/drei": "^9.88.0",
    "@react-three/fiber": "^8.15.11",
    "framer-motion": "^10.16.5",
    "next": "^14.0.3",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-redux": "^9.0.1",
    "three": "^0.158.0",
    "react-intersection-observer": "^9.5.3"
  },
  "devDependencies": {
    "@types/node": "^20.9.3",
    "@types/react": "^18.2.38",
    "@types/three": "^0.158.3",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.31",
    "tailwindcss": "^3.3.5",
    "typescript": "^5.3.2"
  }
}
```

### tailwind.config.js

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx}",
    "./src/components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        gold: {
          light: '#f5e7b9',
          DEFAULT: '#d4af37',
          dark: '#9e7e28',
        },
        noir: {
          light: '#2c2c2c',
          DEFAULT: '#1a1a1a',
          dark: '#0a0a0a',
        },
        cream: '#f8f5f0',
      },
      fontFamily: {
        serif: ['Playfair Display', 'serif'],
        sans: ['Montserrat', 'sans-serif'],
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.7s ease-out',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(20px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
      },
    },
  },
  plugins: [],
}
```

### next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images: {
    domains: ['images.ctfassets.net', 'cdn.guerlain.com'],
    formats: ['image/avif', 'image/webp'],
  },
  i18n: {
    locales: ['en-us', 'fr', 'zh'],
    defaultLocale: 'en-us',
  },
}

module.exports = nextConfig
```

## Global Styles

### src/styles/globals.css

```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    @apply scroll-smooth;
  }
  
  body {
    @apply font-sans text-noir bg-cream antialiased;
  }
  
  h1, h2, h3, h4, h5, h6 {
    @apply font-serif;
  }
}

@layer components {
  .btn-primary {
    @apply bg-gold text-white py-3 px-8 uppercase tracking-widest text-sm font-medium hover:bg-gold-dark transition-colors duration-300;
  }
  
  .btn-secondary {
    @apply border border-gold text-gold py-3 px-8 uppercase tracking-widest text-sm font-medium hover:bg-gold hover:text-white transition-colors duration-300;
  }
  
  .nav-link {
    @apply uppercase tracking-wider text-sm hover:text-gold transition-colors;
  }
  
  .section-title {
    @apply text-3xl md:text-4xl font-serif font-medium mb-8 relative inline-block after:absolute after:h-[2px] after:w-[60%] after:bg-gold after:left-0 after:-bottom-2;
  }
  
  .product-card {
    @apply group relative overflow-hidden;
  }
  
  .product-card-img {
    @apply w-full transition-transform duration-700 group-hover:scale-105;
  }
  
  .product-name {
    @apply font-serif font-medium text-lg;
  }
  
  .product-price {
    @apply text-gold-dark font-medium;
  }
}

.gold-gradient {
  background: linear-gradient(135deg, #d4af37 0%, #f5e7b9 50%, #d4af37 100%);
}

.text-shadow {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}
```

## Component Implementation

### src/components/layout/Header.tsx

```tsx
import React, { useState, useEffect } from 'react';
import Link from 'next/link';
import Image from 'next/image';
import { useSelector } from 'react-redux';
import { RootState } from '@/store';

const Header = () => {
  const [isScrolled, setIsScrolled] = useState(false);
  const [menuOpen, setMenuOpen] = useState(false);
  const cartItems = useSelector((state: RootState) => state.cart.items);
  
  useEffect(() => {
    const handleScroll = () => {
      setIsScrolled(window.scrollY > 50);
    };
    
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);
  
  return (
    <header className={`fixed w-full z-50 transition-all duration-300 ${isScrolled ? 'bg-white shadow-md py-2' : 'bg-transparent py-4'}`}>
      <div className="container mx-auto px-4 flex items-center justify-between">
        {/* Logo */}
        <Link href="/" className="relative z-10">
          <Image 
            src={isScrolled ? "/images/logo-dark.png" : "/images/logo-light.png"} 
            alt="Guerlain" 
            width={120} 
            height={40} 
            className="h-8 md:h-10 w-auto"
          />
        </Link>
        
        {/* Desktop Navigation */}
        <nav className="hidden md:flex items-center space-x-8">
          <Link href="/collections" className={`nav-link ${isScrolled ? 'text-noir' : 'text-white'}`}>
            Collections
          </Link>
          <Link href="/fragrances" className={`nav-link ${isScrolled ? 'text-noir' : 'text-white'}`}>
            Fragrances
          </Link>
          <Link href="/skincare" className={`nav-link ${isScrolled ? 'text-noir' : 'text-white'}`}>
            Skincare
          </Link>
          <Link href="/makeup" className={`nav-link ${isScrolled ? 'text-noir' : 'text-white'}`}>
            Makeup
          </Link>
          <Link href="/our-story" className={`nav-link ${isScrolled ? 'text-noir' : 'text-white'}`}>
            Our Story
          </Link>
        </nav>
        
        {/* User actions */}
        <div className="flex items-center space-x-4">
          <button 
            className={`p-2 ${isScrolled ? 'text-noir' : 'text-white'}`}
            aria-label="Search"
          >
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
          </button>
          
          <Link href="/account" className={`p-2 ${isScrolled ? 'text-noir' : 'text-white'}`}>
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
            </svg>
          </Link>
          
          <Link href="/cart" className={`p-2 relative ${isScrolled ? 'text-noir' : 'text-white'}`}>
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
            </svg>
            {cartItems.length > 0 && (
              <span className="absolute -top-1 -right-1 bg-gold text-white text-xs w-5 h-5 flex items-center justify-center rounded-full">
                {cartItems.length}
              </span>
            )}
          </Link>
          
          {/* Mobile menu button */}
          <button 
            className={`p-2 md:hidden ${isScrolled ? 'text-noir' : 'text-white'}`}
            onClick={() => setMenuOpen(!menuOpen)}
            aria-label="Menu"
          >
            <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
            </svg>
          </button>
        </div>
      </div>
      
      {/* Mobile Navigation */}
      {menuOpen && (
        <div className="fixed inset-0 bg-noir z-40 flex flex-col pt-20 pb-6 px-6 md:hidden">
          <button 
            className="absolute top-4 right-4 text-white p-2"
            onClick={() => setMenuOpen(false)}
            aria-label="Close menu"
          >
            <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
          
          <nav className="flex flex-col space-y-6">
            <Link href="/collections" className="nav-link text-white text-xl" onClick={() => setMenuOpen(false)}>
              Collections
            </Link>
            <Link href="/fragrances" className="nav-link text-white text-xl" onClick={() => setMenuOpen(false)}>
              Fragrances
            </Link>
            <Link href="/skincare" className="nav-link text-white text-xl" onClick={() => setMenuOpen(false)}>
              Skincare
            </Link>
            <Link href="/makeup" className="nav-link text-white text-xl" onClick={() => setMenuOpen(false)}>
              Makeup
            </Link>
            <Link href="/our-story" className="nav-link text-white text-xl" onClick={() => setMenuOpen(false)}>
              Our Story
            </Link>
          </nav>
        </div>
      )}
    </header>
  );
};

export default Header;
```

### src/components/product/ProductCard.tsx

```tsx
import React from 'react';
import Image from 'next/image';
import Link from 'next/link';
import { useDispatch } from 'react-redux';
import { addToCart } from '@/store/cartSlice';
import { Product } from '@/types/product';

interface ProductCardProps {
  product: Product;
  variant?: 'default' | 'featured';
}

const ProductCard = ({ product, variant = 'default' }: ProductCardProps) => {
  const dispatch = useDispatch();
  
  const handleAddToCart = (e: React.MouseEvent) => {
    e.preventDefault();
    dispatch(addToCart({ ...product, quantity: 1 }));
  };
  
  if (variant === 'featured') {
    return (
      <div className="group relative overflow-hidden bg-cream">
        <div className="aspect-w-4 aspect-h-5 relative">
          <Image
            src={product.images[0]}
            alt={product.name}
            fill
            className="object-cover object-center transition-transform duration-700 group-hover:scale-105"
            sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
            priority={true}
          />
          <div className="absolute inset-0 bg-gradient-to-t from-noir-dark/70 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end">
            <div className="p-6 w-full">
              <Link href={`/product/${product.slug}`} className="btn-primary w-full block text-center">
                Discover
              </Link>
            </div>
          </div>
        </div>
        <div className="mt-4 px-4 pb-6 text-center">
          <h3 className="font-serif text-xl">{product.name}</h3>
          <p className="text-gray-600 mb-2">{product.category}</p>
          <p className="text-gold-dark font-medium">${product.price.toFixed(2)}</p>
        </div>
      </div>
    );
  }
  
  return (
    <Link href={`/product/${product.slug}`} className="product-card">
      <div className="aspect-w-1 aspect-h-1 relative overflow-hidden">
        <Image
          src={product.images[0]}
          alt={product.name}
          fill
          className="product-card-img object-center object-cover"
          sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
          loading="lazy"
        />
        
        <div className="absolute bottom-4 left-0 right-0 flex justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300 transform translate-y-4 group-hover:translate-y-0">
          <button 
            onClick={handleAddToCart}
            className="bg-gold text-white py-2 px-4 text-sm uppercase tracking-wider hover:bg-gold-dark transition-colors"
            aria-label={`Add ${product.name} to cart`}
          >
            Add to Cart
          </button>
        </div>
      </div>
      
      <div className="mt-4 space-y-1">
        <h3 className="product-name">{product.name}</h3>
        <p className="text-gray-600 text-sm">{product.shortDescription}</p>
        <p className="product-price">${product.price.toFixed(2)}</p>
      </div>
    </Link>
  );
};

export default ProductCard;
```

### src/components/ar/ARViewer.tsx

```tsx
import React, { useRef, useState, useEffect } from 'react';
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Environment } from '@react-three/drei';
import * as THREE from 'three';
import { useGLTF } from '@react-three/drei';

function Model({ modelPath, scale = 1 }) {
  const gltf = useGLTF(modelPath);
  const modelRef = useRef();
  
  return (
    <primitive 
      ref={modelRef}
      object={gltf.scene} 
      scale={scale} 
      position={[0, 0, 0]}
      rotation={[0, 0, 0]}
    />
  );
}

const ARViewer = ({ modelPath, fallbackImage }) => {
  const [isARSupported, setIsARSupported] = useState(false);
  const [isARMode, setIsARMode] = useState(false);
  
  useEffect(() => {
    // Check if WebXR is supported
    if ('xr' in navigator && 'isSessionSupported' in navigator.xr) {
      navigator.xr.isSessionSupported('immersive-ar').then((supported) => {
        setIsARSupported(supported);
      });
    }
  }, []);
  
  const startARSession = async () => {
    if (isARSupported) {
      setIsARMode(true);
      // In a real implementation, this would launch the AR experience
      alert('AR mode would launch here in a real implementation');
    }
  };
  
  return (
    <div className="ar-viewer">
      <div className="relative h-[500px] w-full bg-gray-100 rounded-lg overflow-hidden">
        {!isARMode && (
          <Canvas camera={{ position: [0, 0, 5], fov: 45 }}>
            <ambientLight intensity={0.5} />
            <spotLight position={[10, 10, 10]} angle={0.15} penumbra={1} />
            <Model modelPath={modelPath} />
            <OrbitControls />
            <Environment preset="studio" />
          </Canvas>
        )}
        
        {isARSupported && (
          <button
            onClick={startARSession}
            className="absolute bottom-4 right-4 bg-gold text-white py-2 px-4 rounded-full flex items-center space-x-2 hover:bg-gold-dark transition-colors"
            aria-label="View in your space"
          >
            <span>View in your space</span>
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" />
            </svg>
          </button>
        )}
      </div>
      
      <div className="text-center mt-4 text-gray-600 text-sm">
        <p>Drag to rotate the model</p>
        {isARSupported && <p>Click "View in your space" to experience in AR</p>}
      </div>
    </div>
  );
};

export default ARViewer;
```

### src/components/ui/ChatbotWidget.tsx

```tsx
import React, { useState, useRef, useEffect } from 'react';
import Image from 'next/image';

type Message = {
  id: string;
  text: string;
  sender: 'user' | 'bot';
  timestamp: Date;
};

const initialMessages = [
  {
    id: '1',
    text: 'Hello! I'm your Guerlain beauty advisor. How can I assist you today?',
    sender: 'bot',
    timestamp: new Date()
  }
];

const ChatbotWidget = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState(initialMessages);
  const [input, setInput] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const messagesEndRef = useRef(null);
  
  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  };
  
  useEffect(() => {
    if (isOpen) {
      scrollToBottom();
    }
  }, [messages, isOpen]);
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!input.trim()) return;
    
    // Add user message
    const userMessage = {
      id: Date.now().toString(),
      text: input,
      sender: 'user',
      timestamp: new Date()
    };
    
    setMessages(prev => [...prev, userMessage]);
    setInput('');
    setIsTyping(true);
    
    try {
      // In a real implementation, this would call an API
      const response = await fetch('/api/chatbot', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ query: input }),
      });
      
      const data = await response.json();
      
      // Add bot response after a short delay for a more natural feel
      setTimeout(() => {
        const botMessage = {
          id: Date.now().toString(),
          text: data.answer,
          sender: 'bot',
          timestamp: new Date()
        };
        
        setMessages(prev => [...prev, botMessage]);
        setIsTyping(false);
      }, 1000);
    } catch (error) {
      console.error('Error fetching chatbot response:', error);
      setIsTyping(false);
    }
  };
  
  return (
    <>
      {/* Chat toggle button */}
      <button
        onClick={() => setIsOpen(!isOpen)}
        className="fixed bottom-6 right-6 z-50 bg-gold text-white rounded-full p-3 shadow-lg hover:bg-gold-dark transition-colors"
        aria-label={isOpen ? "Close chat" : "Open chat"}
      >
        {isOpen ? (
          <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
          </svg>
        ) : (
          <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z" />
          </svg>
        )}
      </button>
      
      {/* Chat window */}
      {isOpen && (
        <div className="fixed bottom-20 right-6 z-50 w-80 sm:w-96 bg-white rounded-lg shadow-2xl flex flex-col max-h-[550px] border border-gray-200">
          {/* Header */}
          <div className="bg-noir text-white p-4 rounded-t-lg flex items-center space-x-3">
            <Image 
              src="/images/logo-icon.png" 
              alt="Guerlain" 
              width={32} 
              height={32} 
              className="rounded-full bg-white p-1"
            />
            <div>
              <h3 className="font-serif">Guerlain Beauty Advisor</h3>
              <p className="text-xs text-gray-300">Ask me about our products</p>
            </div>
          </div>
          
          {/* Messages */}
          <div className="flex-1 overflow-y-auto p-4 space-y-4 bg-gray-50">
            {messages.map(message => (
              <div
                key={message.id}
                className={`flex ${message.sender === 'user' ? 'justify-end' : 'justify-start'}`}
              >
                <div
                  className={`max-w-[85%] p-3 rounded-lg ${
                    message.sender === 'user'
                      ? 'bg-gold-light text-noir-dark rounded-tr-none'
                      : 'bg-white text-noir shadow rounded-tl-none border border-gray-200'
                  }`}
                >
                  {message.text}
                </div>
              </div>
            ))}
            
            {isTyping && (
              <div className="flex justify-start">
                <div className="bg-white text-noir shadow rounded-lg rounded-tl-none border border-gray-200 p-3 max-w-[85%]">
                  <div className="flex space-x-1">
                    <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
                    <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce delay-100"></div>
                    <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce delay-200"></div>
                  </div>
                </div>
              </div>
            )}
            
            <div ref={messagesEndRef} />
          </div>
          
          {/* Input */}
          <form onSubmit={handleSubmit} className="p-3 border-t">
            <div className="flex">
              <input
                type="text"
                value={input}
                onChange={(e) => setInput(e.target.value)}
                placeholder="Type your question..."
                className="flex-1 p-2 border border-gray-300 rounded-l-lg focus:outline-none focus:ring-1 focus:ring-gold"
                disabled={isTyping}
              />
              <button
                type="submit"
                className="bg-gold text-white p-2 rounded-r-lg hover:bg-gold-dark transition-colors disabled:opacity-50"
                disabled={!input.trim() || isTyping}
              >
                <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
                </svg>
              </button>
            </div>
          </form>
        </div>
      )}
    </>
  );
};

export default ChatbotWidget;
```

## Redux Store Implementation

### src/store/index.ts

```typescript
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './cartSlice';
import userReducer from './userSlice';
import uiReducer from './uiSlice';

export const store = configureStore({
  reducer: {
    cart: cartReducer,
    user: userReducer,
    ui: uiReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### src/store/cartSlice.ts

```typescript
import { createSlice, PayloadAction } from '@reduxjs/toolkit';
import { Product } from '@/types/product';

interface CartItem extends Product {
  quantity: number;
}

interface CartState {
  items: CartItem[];
  isCartOpen: boolean;
}

const initialState: CartState = {
  items: [],
  isCartOpen: false,
};

const cartSlice = createSlice({
  name: 'cart',
  initialState,
  reducers: {
    addToCart: (state, action: PayloadAction<CartItem>) => {
      const existingItemIndex = state.items.findIndex(
        item => item.id === action.payload.id
      );
      
      if (existingItemIndex >= 0) {
        state.items[existingItemIndex].quantity += action.payload.quantity;
      } else {
        state.items.push(action.payload);
      }
      
      // Open cart when adding items
      state.isCartOpen = true;
    },
    removeFromCart: (state, action: PayloadAction<string>) => {
      state.items = state.items.filter(item => item.id !== action.payload);
    },
    updateQuantity: (state, action: PayloadAction<{ id: string; quantity: number }>) => {
      const item = state.items.find(item => item.id === action.payload.id);
      if (item) {
        item.quantity = action.payload.quantity;
      }
    },
    clearCart: (state) => {
      state.items = [];
    },
    toggleCart: (state) => {
      state.isCartOpen = !state.isCartOpen;
    },
    closeCart: (state) => {
      state.isCartOpen = false;
    },
  },
});

export const {
  addToCart,
  removeFromCart,
  updateQuantity,
  clearCart,
  toggleCart,
  closeCart,
} = cartSlice.actions;

export default cartSlice.reducer;
```

## Page Implementation

### src/pages/index.tsx

```tsx
import React from 'react';
import Layout from '@/components/layout/Layout';
import HeroSection from '@/components/ui/HeroSection';
import ProductCard from '@/components/product/ProductCard';
import Link from 'next/link';
import Image from 'next/image';
import { useInView } from 'react-intersection-observer';

// Mock data for featured products
const featuredProducts = [
  {
    id: '1',
    name: 'Mon Guerlain',
    slug: 'mon-guerlain',
    price: 135.00,
    shortDescription: 'Eau de Parfum',
    description: 'Mon Guerlain, the House's new fragrance, is a tribute to today's femininity - a strong, free and sensual femininity.',
    images: ['/images/product/mon-guerlain.jpg'],
    category: 'Fragrances',
    collections: ['Bestsellers'],
    in_stock: true,
  },
  {
    id: '2',
    name: 'Abeille Royale',
    slug: 'abeille-royale-double-r-serum',
    price: 195.00,
    shortDescription: 'Double R Serum',
    description: 'Abeille Royale Double R Serum, Guerlain's 1st dual renewing and repairing serum.',
    images: ['/images/product/abeille-royale.jpg'],
    category: 'Skincare',
    collections: ['New Arrivals'],
    in_stock: true,
  },
  {
    id: '3',
    name: 'Rouge G',
    slug: 'rouge-g-lipstick',
    price: 55.00,
    shortDescription: 'Customizable Lipstick',
    description: 'Rouge G de Guerlain, the first customizable lipstick, reveals a new, sumptuous design.',
    images: ['/images/product/rouge-g.jpg'],
    category: 'Makeup',
    collections: ['Bestsellers'],
    in_stock: true,
  },
];

export default function Home() {
  const [heroRef, heroInView] = useInView({ triggerOnce: true });
  const [sectionRef1, inView1] = useInView({ triggerOnce: true, threshold: 0.1 });
  const [sectionRef2, inView2] = useInView({ triggerOnce: true, threshold: 0.1 });
  
  return (
    <Layout>
      {/* Hero Section */}
      <div ref={heroRef}>
        <HeroSection
          title="The Art of Beauty"
          subtitle="Crafting luxury fragrances and skincare since 1828"
          backgroundImage="/images/hero.jpg"
          videoSrc="/videos/guerlain-brand.mp4"
          buttonText="Discover Our Collections"
          buttonLink="/collections"
          height="full"
        />
      </div>
      
      {/* Featured Collection */}
      <section className="py-20 bg-cream" ref={sectionRef1}>
        <div className="container mx-auto px-6">
          <div className={`text-center mb-12 transform transition-transform duration-1000 ${inView1 ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}`}>
            <h2 className="text-3xl md:text-4xl font-serif mb-4">Discover Our Bestsellers</h2>
            <p className="text-gray-600 max-w-2xl mx-auto">
              Iconic creations that have captivated generations with their exceptional quality and timeless elegance.
            </p>
          </div>
          
          <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
            {featuredProducts.map((product) => (
              <div 
                key={product.id}
                className={`transform transition-all duration-1000 delay-${parseInt(product.id) * 200} ${inView1 ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}`}
              >
                <ProductCard product={product} variant="featured" />
              </div>
            ))}
          </div>
          
          <div className="text-center mt-12">
            <Link href="/collections/bestsellers" className="btn-secondary">
              View All Bestsellers
            </Link>
          </div>
        </div>
      </section>
      
      {/* Brand Story Section */}
      <section className="py-20 bg-noir text-white" ref={sectionRef2}>
        <div className="container mx-auto px-6">
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
            <div className={`transform transition-all duration-1000 ${inView2 ? 'translate-x-0 opacity-100' : '-translate-x-10 opacity-0'}`}>
              <h2 className="text-3xl md:text-4xl font-serif mb-6">Our Legacy</h2>
              <p className="mb-6 text-gray-300">
                Since 1828, Guerlain has been exploring, innovating and enhancing. 
                In fragrance, skincare, and makeup, Guerlain has created iconic products 
                that are timeless classics loved by generations.
              </p>
              <p className="mb-8 text-gray-300">
                Our master perfumers have crafted over 1,100 fragrances, each reflecting 
                the artistic spirit and exceptional quality that defines the House of Guerlain.
              </p>
              <Link href="/our-story" className="btn-primary">
                Explore Our Heritage
              </Link>
            </div>
            
            <div className={`relative h-[500px] transform transition-all duration-1000 delay-300 ${inView2 ? 'translate-x-0 opacity-100' : 'translate-x-10 opacity-0'}`}>
              <Image 
                src="/images/guerlain-heritage.jpg" 
                fill
                className="object-cover object-center rounded-lg"
                alt="Guerlain Heritage"
                sizes="(max-width: 768px) 100vw, 50vw"
              />
            </div>
          </div>
        </div>
      </section>
      
      {/* Newsletter Section */}
      <section className="py-16 bg-gold-light">
        <div className="container mx-auto px-6 text-center">
          <h2 className="text-3xl font-serif mb-6">Join the Guerlain Experience</h2>
          <p className="max-w-2xl mx-auto mb-8 text-noir">
            Subscribe to receive exclusive updates, personalized recommendations, and early access to new collections.
          </p>
          <form className="max-w-md mx-auto flex">
            <input
              type="email"
              placeholder="Your email address"
              className="flex-grow py-3 px-4 focus:outline-none focus:ring-1 focus:ring-gold"
              aria-label="Email address"
            />
            <button type="submit" className="bg-noir text-white py-3 px-6 uppercase text-sm tracking-wider hover:bg-noir-light transition-colors">
              Subscribe
            </button>
          </form>
        </div>
      </section>
    </Layout>
  );
}
```

### src/pages/product/[slug].tsx

```tsx
import React, { useState } from 'react';
import { GetStaticPaths, GetStaticProps } from 'next';
import Layout from '@/components/layout/Layout';
import Image from 'next/image';
import ARViewer from '@/components/ar/ARViewer';
import { useDispatch } from 'react-redux';
import { addToCart } from '@/store/cartSlice';
import Link from 'next/link';
import ProductGrid from '@/components/product/ProductGrid';

// Mock data for a product
const product = {
  id: '1',
  name: 'Mon Guerlain',
  slug: 'mon-guerlain',
  price: 135.00,
  shortDescription: 'Eau de Parfum',
  description: 'Mon Guerlain, the House's new fragrance, is a tribute to today's femininity - a strong, free and sensual femininity, inspired by Angelina Jolie. The notes of a Guerlain perfume created as a portrait of this extraordinary muse. Carla Lavender from Provence, Sambac Jasmine from India, Album Sandalwood from Australia and Vanilla Tahitensis from Papua New Guinea.',
  images: [
    '/images/product/mon-guerlain-1.jpg',
    '/images/product/mon-guerlain-2.jpg',
    '/images/product/mon-guerlain-3.jpg',
  ],
  category: 'Fragrances',
  collections: ['Bestsellers'],
  tags: ['Floral', 'Woody', 'Fresh'],
  ingredients: 'Alcohol, Parfum (Fragrance), Aqua (Water), Linalool, Limonene, Coumarin, Ethylhexyl Methoxycinnamate, Ethylhexyl Salicylate, Butyl Methoxydibenzoylmethane, BHT, Citral, Citronellol, Geraniol, Farnesol, CI 14700 (Red 4), CI 19140 (Yellow 5), CI 60730 (Ext. Violet 2).',
  how_to_use: 'Spray on pulse points: wrists, behind the ears, and on the neck.',
  volume: '50ml',
  in_stock: true,
  ar_model: '/models/mon-guerlain.gltf',
};

// Mock data for related products
const relatedProducts = [
  {
    id: '2',
    name: 'Aqua Allegoria',
    slug: 'aqua-allegoria-flora-cherrysia',
    price: 115.00,
    shortDescription: 'Eau de Toilette',
    description: 'Aqua Allegoria is a collection of fresh fragrances inspired by nature.',
    images: ['/images/product/aqua-allegoria.jpg'],
    category: 'Fragrances',
    in_stock: true,
  },
  {
    id: '3',
    name: 'La Petite Robe Noire',
    slug: 'la-petite-robe-noire',
    price: 125.00,
    shortDescription: 'Eau de Parfum',
    description: 'La Petite Robe Noire is a fruity floral gourmand Eau de Parfum.',
    images: ['/images/product/la-petite-robe-noire.jpg'],
    category: 'Fragrances',
    in_stock: true,
  },
  {
    id: '4',
    name: 'L\'Homme Idéal',
    slug: 'l-homme-ideal',
    price: 110.00,
    shortDescription: 'Eau de Parfum',
    description: 'L\'Homme Idéal is an Eau de Parfum with strength and freshness.',
    images: ['/images/product/homme-ideal.jpg'],
    category: 'Fragrances',
    in_stock: true,
  },
];

export default function ProductPage() {
  const [quantity, setQuantity] = useState(1);
  const [selectedImageIndex, setSelectedImageIndex] = useState(0);
  const [showAR, setShowAR] = useState(false);
  const [activeTab, setActiveTab] = useState('description');
  const dispatch = useDispatch();
  
  const handleAddToCart = () => {
    dispatch(addToCart({ ...product, quantity }));
  };
  
  return (
    <Layout title={`${product.name} | Guerlain`} description={product.shortDescription}>
      <div className="container mx-auto px-4 py-12 md:py-20">
        <nav className="text-sm mb-8">
          <ol className="flex items-center">
            <li className="flex items-center">
              <Link href="/" className="text-gray-500 hover:text-gold transition-colors">Home</Link>
              <span className="mx-2 text-gray-400">/</span>
            </li>
            <li className="flex items-center">
              <Link href={`/${product.category.toLowerCase()}`} className="text-gray-500 hover:text-gold transition-colors">{product.category}</Link>
              <span className="mx-2 text-gray-400">/</span>
            </li>
            <li><span className="text-gold">{product.name}</span></li>
          </ol>
        </nav>
        
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-12">
          {/* Product Images */}
          <div>
            <div className="relative aspect-[4/5] bg-white rounded-lg overflow-hidden">
              {showAR ? (
                <ARViewer modelPath={product.ar_model} />
              ) : (
                <>
                  <Image
                    src={product.images[selectedImageIndex]}
                    fill
                    className="object-contain"
                    alt={product.name}
                    sizes="(max-width: 1024px) 100vw, 50vw"
                    priority={true}
                  />
                  
                  {product.ar_model && (
                    <button
                      onClick={() => setShowAR(true)}
                      className="absolute bottom-4 right-4 bg-white text-noir py-2 px-4 rounded-full flex items-center space-x-2 hover:bg-gold hover:text-white transition-colors"
                    >
                      <span>View in 3D</span>
                      <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M9 19l3 3m0 0l3-3m-3 3V10" />
                      </svg>
                    </button>
                  )}
                </>
              )}
            </div>
            
            {!showAR && (
              <div className="mt-6 flex gap-4">
                {product.images.map((image, index) => (
                  <button
                    key={index}
                    onClick={() => setSelectedImageIndex(index)}
                    className={`rounded-md overflow-hidden border-2 ${selectedImageIndex === index ? 'border-gold' : 'border-transparent'}`}
                  >
                    <div className="relative w-20 h-20">
                      <Image
                        src={image}
                        fill
                        className="object-cover"
                        alt={`${product.name} - Image ${index + 1}`}
                        sizes="80px"
                      />
                    </div>
                  </button>
                ))}
              </div>
            )}
            
            {showAR && (
              <button
                onClick={() => setShowAR(false)}
                className="mt-4 text-gold hover:text-gold-dark transition-colors"
              >
                Back to Product Images
              </button>
            )}
          </div>
          
          {/* Product Info */}
          <div>
            <h1 className="text-3xl md:text-4xl font-serif mb-2">{product.name}</h1>
            <p className="text-gray-600 mb-4">{product.shortDescription}</p>
            
            <div className="mb-6">
              <span className="text-2xl font-medium text-gold-dark">${product.price.toFixed(2)}</span>
            </div>
            
            <div className="mb-8">
              <div className="flex items-center mb-6">
                <label htmlFor="quantity" className="mr-4 text-gray-700">Quantity:</label>
                <div className="flex border border-gray-300">
                  <button
                    onClick={() => setQuantity(Math.max(1, quantity - 1))}
                    className="px-3 py-2 bg-gray-100 hover:bg-gray-200 transition-colors"
                    aria-label="Decrease quantity"
                  >
                    -
                  </button>
                  <input
                    id="quantity"
                    type="number"
                    min="1"
                    value={quantity}
                    onChange={(e) => setQuantity(parseInt(e.target.value) || 1)}
                    className="w-16 text-center py-2 border-x border-gray-300"
                  />
                  <button
                    onClick={() => setQuantity(quantity + 1)}
                    className="px-3 py-2 bg-gray-100 hover:bg-gray-200 transition-colors"
                    aria-label="Increase quantity"
                  >
                    +
                  </button>
                </div>
              </div>
              
              <button
                onClick={handleAddToCart}
                className="btn-primary w-full sm:w-auto mb-4"
                disabled={!product.in_stock}
              >
                {product.in_stock ? 'Add to Cart' : 'Out of Stock'}
              </button>
            </div>
            
            {/* Product Tabs */}
            <div className="border-t border-b border-gray-200 mb-8">
              <div className="flex overflow-x-auto">
                <button
                  onClick={() => setActiveTab('description')}
                  className={`py-4 px-6 font-medium text-sm focus:outline-none ${activeTab === 'description' ? 'text-gold border-b-2 border-gold' : 'text-gray-500 hover:text-noir'}`}
                >
                  Description
                </button>
                <button
                  onClick={() => setActiveTab('ingredients')}
                  className={`py-4 px-6 font-medium text-sm focus:outline-none ${activeTab === 'ingredients' ? 'text-gold border-b-2 border-gold' : 'text-gray-500 hover:text-noir'}`}
                >
                  Ingredients
                </button>
                <button
                  onClick={() => setActiveTab('how_to_use')}
                  className={`py-4 px-6 font-medium text-sm focus:outline-none ${activeTab === 'how_to_use' ? 'text-gold border-b-2 border-gold' : 'text-gray-500 hover:text-noir'}`}
                >
                  How to Use
                </button>
              </div>
            </div>
            
            <div className="product-content">
              {activeTab === 'description' && (
                <div className="prose prose-lg">
                  <p>{product.description}</p>
                  <p className="mt-4">
                    <strong>Volume:</strong> {product.volume}
                  </p>
                </div>
              )}
              {activeTab === 'ingredients' && (
                <div className="prose prose-lg">
                  <p>{product.ingredients}</p>
                </div>
              )}
              {activeTab === 'how_to_use' && (
                <div className="prose prose-lg">
                  <p>{product.how_to_use}</p>
                </div>
              )}
            </div>
            
            {product.tags && (
              <div className="mt-8">
                <div className="flex flex-wrap gap-2">
                  {product.tags.map((tag) => (
                    <span key={tag} className="bg-gray-100 text-gray-700 text-xs px-3 py-1 rounded-full">
                      {tag}
                    </span>
                  ))}
                </div>
              </div>
            )}
          </div>
        </div>
        
        {/* Related Products */}
        <div className="mt-20">
          <h2 className="text-2xl md:text-3xl font-serif mb-8">You May Also Like</h2>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
            {relatedProducts.map((product) => (
              <div key={product.id}>
                <ProductCard product={product} />
              </div>
            ))}
          </div>
        </div>
      </div>
    </Layout>
  );
}

export async function getStaticProps() {
  // In a real implementation, this would fetch data from a CMS or API
  return {
    props: {
      product,
      relatedProducts,
    },
    revalidate: 60 * 60, // Revalidate every hour
  };
}

export async function getStaticPaths() {
  // In a real implementation, this would return paths for all products
  return {
    paths: [{ params: { slug: 'mon-guerlain' } }],
    fallback: 'blocking',
  };
}
```

## Deployment Configuration

### Dockerfile

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app

ENV NODE_ENV production

# Create a non-root user
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# Copy necessary files from builder stage
COPY --from=builder /app/next.config.js ./
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

# Set correct permissions
RUN chown -R nextjs:nodejs /app

USER nextjs

EXPOSE 3000

CMD ["node", "server.js"]
```

### docker-compose.yml

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: guerlain-website
    ports:
      - "3000:3000"
    restart: always
    env_file:
      - .env
    networks:
      - guerlain-network

networks:
  guerlain-network:
    driver: bridge
```

This implementation provides a comprehensive, elegant, and modern luxury brand website based on the Guerlain blueprint below. The design emphasizes luxury aesthetics with the black, gold, and white color scheme while incorporating advanced features like AR product visualization and an AI-powered chatbot.

The website is built with performance and accessibility in mind, using Next.js for server-side rendering and optimized loading through techniques like image optimization and lazy loading. The modular component structure ensures maintainability and scalability.

---
### **1. Introduction - Guerlain blueprint**  
Guerlain’s current website (https://www.guerlain.com/us/en-us) exemplifies luxury branding through elegant visuals, intuitive navigation, and immersive storytelling. This paper proposes a redesign that retains its core strengths while integrating advanced features like augmented reality (AR), AI-driven personalization, and performance optimizations. The technical design emphasizes scalability, accessibility, and cutting-edge interactivity, supported by a modern tech stack and detailed implementation guidelines.

---

### **2. Analysis of Current Design**  
#### **2.1 Layout and Visual Hierarchy**  
- **Strengths**: The site uses a grid-based layout with high-resolution imagery, emphasizing product aesthetics. Hero sections feature full-screen videos or carousels to highlight collections [[1]].  
- **Opportunities**: Implement dynamic grid layouts (e.g., masonry) for product listings to improve visual variety.  

#### **2.2 Color Scheme and Typography**  
- **Current Palette**: Black, gold, and white convey luxury.  
- **Improvement**: Introduce gradient overlays and subtle animations on hover to enhance depth [[2]].  

#### **2.3 UI/UX Features**  
- **Outstanding Features**: Smooth scroll animations, product quick-view modals, and a sticky header with a search bar.  
- **Gaps**: Limited mobile AR integration and lack of AI-driven recommendations.  

#### **2.4 Navigation and Accessibility**  
- **Current Flow**: Mega-menu dropdowns for categories (e.g., Fragrances, Skincare).  
- **Improvement**: Add a breadcrumb trail and voice-based navigation for accessibility compliance (WCAG 2.1) [[3]].  

---

### **3. Proposed Improvements**  
#### **3.1 Core Features**  
1. **AR Product Previews**: Use Three.js and WebAR to allow users to visualize perfumes/skincare in real-world settings.  
2. **AI Chatbot**: Integrate a GPT-4-powered assistant for personalized skincare recommendations.  
3. **Dynamic Personalization**: Leverage user behavior analytics (via Mixpanel) to tailor product suggestions.  

#### **3.2 Technical Enhancements**  
- **Performance**: Lazy loading for images, server-side rendering (Next.js), and CDN integration (Cloudflare).  
- **Security**: OAuth2 authentication, HTTPS enforcement, and GDPR-compliant cookie consent.  

---

### **4. Technical Design Document**  
#### **4.1 Technology Stack**  
| Component       | Technology                | Purpose                                  |  
|-----------------|---------------------------|------------------------------------------|  
| Frontend        | Next.js + TypeScript      | SSR, static site generation              |  
| State Management| Redux Toolkit             | Global state for cart/authentication    |  
| Backend         | Node.js + Express         | REST API for product/catalog management |  
| Database        | PostgreSQL                | Transactional data (orders, users)       |  
| CMS             | Contentful                | Manage blog posts and marketing content |  
| AR Integration  | Three.js + WebAR          | 3D product visualization                 |  
| AI Chatbot      | Dialogflow + GPT-4 API    | NLP-driven customer support              |  

#### **4.2 Key Code Snippets**  
**Product Card Component (React/TypeScript):**  
```tsx
const ProductCard: React.FC<Product> = ({ id, name, price, imageUrl }) => {
  const [isHovered, setIsHovered] = useState(false);
  
  return (
    <div 
      className="product-card" 
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <img 
        src={imageUrl} 
        alt={name} 
        className={`rounded-lg transition-transform ${isHovered ? 'scale-110' : ''}`}
      />
      <h3 className="text-lg font-semibold">{name}</h3>
      <p className="text-gray-600">${price}</p>
    </div>
  );
};
```

**AR Integration (Three.js):**  
```javascript
// Initialize 3D scene for product visualization
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ alpha: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Load 3D model
const loader = new GLTFLoader();
loader.load('/models/perfume-bottle.gltf', (gltf) => {
  scene.add(gltf.scene);
  animate();
});

function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
}
```

---

### **5. Development Environment Setup**  
1. **Prerequisites**:  
   - Node.js 18.x  
   - Docker Desktop  
   - PostgreSQL 14.x  

2. **Local Setup**:  
   ```bash
   # Clone repository
   git clone https://github.com/guerlain-redesign/web.git

   # Install dependencies
   cd web
   npm install

   # Start PostgreSQL via Docker
   docker-compose up -d

   # Run development server
   npm run dev
   ```

---

### **6. Testing and Deployment**  
- **Testing**: Use Jest for unit tests and Cypress for E2E testing.  
- **Deployment**: CI/CD via GitHub Actions; host on Vercel (frontend) and AWS EC2 (backend).  

---

### **7. Conclusion**  
This redesign balances Guerlain’s legacy of luxury with modern web capabilities, ensuring a seamless, engaging user experience. By adopting the outlined tech stack and design principles, the new site will drive higher conversion rates and customer satisfaction.  

---
**Step-by-Step Guide to Building the Improved Guerlain Website with Next.js**  
*(Based on the research paper blueprint and web_search references)*  

---

### **1. Project Setup**  
**1.1 Initialize Next.js**  
```bash
npx create-next-app@latest guerlain-redesign  
cd guerlain-redesign  
npm install  
```  
[[7]]  

**1.2 Install Dependencies**  
```bash
npm install redux react-redux @reduxjs/toolkit three @react-three/fiber  
npm install --save-dev tailwindcss postcss autoprefixer  
npx tailwindcss init -p  
```  
[[2]][[5]]  

**1.3 Folder Structure**  
```
guerlain-redesign/  
├── public/          # Static assets (images, 3D models)  
├── src/  
│   ├── components/  # Reusable UI components  
│   ├── pages/       # Next.js pages (SSR/SSG)  
│   ├── store/       # Redux slices and configuration  
│   └── styles/      # Global CSS and Tailwind  
```  
[[6]]  

---

### **2. Core Design Implementation**  
**2.1 Luxe Color Scheme & Typography**  
```css
/* styles/globals.css */
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
[[2]]  

**2.2 Hero Section with Video Background**  
```tsx
// src/components/Hero.tsx
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
```  
[[3]]  

---

### **3. Advanced Features**  
**3.1 AR Product Visualization**  
```tsx
// src/components/ARViewer.tsx
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
```  
[[1]][[4]]  

**3.2 AI-Powered Chatbot**  
```tsx
// src/components/Chatbot.tsx
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
```  
*(Backend API route uses Dialogflow/GPT-4 integration)*  

---

### **4. Performance & Accessibility**  
**4.1 Lazy Loading Images**  
```tsx
// src/components/ProductCard.tsx
const ProductCard = ({ product }) => (
  <div className="group">
    <img
      src={product.image}
      alt={product.name}
      loading="lazy"
      className="transition-transform group-hover:scale-110"
    />
    <h3 className="text-lg font-semibold mt-2">{product.name}</h3>
  </div>
);
```  
[[8]]  

**4.2 ARIA Compliance**  
```tsx
<button 
  aria-label="Add to Cart" 
  className="bg-gold text-white px-4 py-2"
>
  Add to Cart
</button>
```  
[[3]]  

---

### **5. State Management (Redux)**  
**5.1 Cart Slice**  
```typescript
// src/store/cartSlice.ts
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
[[5]]  

---

### **6. Deployment**  
**6.1 Docker Configuration**  
```dockerfile
# Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```  
[[4]]  
