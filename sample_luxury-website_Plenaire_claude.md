# Luxury Beauty E-commerce Website Design

I'll create a comprehensive web application for a luxury beauty brand inspired by the Plenaire design specification. Let me walk through my approach to build this high-end e-commerce platform.

## System Architecture Overview

The website will be built with:

- **Frontend**: React.js with TypeScript for type safety and better maintainability
- **Backend**: Node.js with Express.js for RESTful API endpoints
- **Database**: PostgreSQL for relational data storage
- **Authentication**: JWT-based user authentication
- **Payment Processing**: Stripe integration for secure checkout
- **Styling**: CSS-in-JS with styled-components for the luxury aesthetic

## Database Schema Design

Let's start by designing a robust PostgreSQL database schema:

```sql
-- Users table for authentication and profile information
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  phone VARCHAR(20),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Products table for store inventory
CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  price DECIMAL(10, 2) NOT NULL,
  image_url VARCHAR(255),
  inventory_count INTEGER DEFAULT 0,
  category VARCHAR(100),
  tags TEXT[],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Wishlists for user-saved products
CREATE TABLE wishlists (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  product_id INTEGER REFERENCES products(id) ON DELETE CASCADE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(user_id, product_id)
);

-- Orders table for purchase tracking
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE SET NULL,
  status VARCHAR(50) NOT NULL DEFAULT 'pending',
  total_amount DECIMAL(10, 2) NOT NULL,
  shipping_address JSONB,
  billing_address JSONB,
  payment_intent_id VARCHAR(255),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Order items for product quantities in each order
CREATE TABLE order_items (
  id SERIAL PRIMARY KEY,
  order_id INTEGER REFERENCES orders(id) ON DELETE CASCADE,
  product_id INTEGER REFERENCES products(id) ON DELETE SET NULL,
  quantity INTEGER NOT NULL,
  price_at_purchase DECIMAL(10, 2) NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Contact inquiries for customer support
CREATE TABLE contact_inquiries (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL,
  subject VARCHAR(255),
  message TEXT NOT NULL,
  status VARCHAR(50) DEFAULT 'unread',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

## Frontend Implementation

Now I'll implement key frontend components following the Plenaire design spec.

### Header Component

```tsx
// src/components/layout/Header.tsx
import React, { useState, useEffect } from 'react';
import { Link, useLocation } from 'react-router-dom';
import styled from 'styled-components';
import { useAuth } from '../../contexts/AuthContext';
import { useCart } from '../../contexts/CartContext';

const HeaderContainer = styled.header<{ scrolled: boolean }>`
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  background: ${props => props.scrolled 
    ? 'linear-gradient(180deg, rgba(0,0,0,0.85), rgba(0,0,0,0.75))' 
    : 'transparent'};
  padding: 20px;
  transition: all 0.3s ease-in-out;
`;

const HeaderContent = styled.div`
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
`;

const Logo = styled.div`
  img {
    height: 40px;
  }
`;

const NavMenu = styled.nav`
  ul {
    display: flex;
    list-style: none;
    gap: 30px;
  }

  li a {
    color: #fff;
    text-decoration: none;
    text-transform: uppercase;
    font-size: 14px;
    letter-spacing: 1px;
    transition: color 0.2s ease;

    &:hover {
      color: #f5a623;
    }
  }
`;

const Header = () => {
  const [scrolled, setScrolled] = useState(false);
  const { user } = useAuth();
  const { cartItems } = useCart();
  
  useEffect(() => {
    const handleScroll = () => {
      setScrolled(window.scrollY > 50);
    };
    
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);
  
  return (
    <HeaderContainer scrolled={scrolled}>
      <HeaderContent>
        <Logo>
          <Link to="/">
            <img src="/assets/logo.svg" alt="Plenaire" />
          </Link>
        </Logo>
        
        <NavMenu>
          <ul>
            <li><Link to="/">Home</Link></li>
            <li><Link to="/products">Products</Link></li>
            <li><Link to="/about">About</Link></li>
            <li><Link to="/contact">Contact</Link></li>
          </ul>
        </NavMenu>
        
        {/* User and cart buttons would go here */}
      </HeaderContent>
    </HeaderContainer>
  );
};

export default Header;
```

### Product Card Component

```tsx
// src/components/products/ProductCard.tsx
import React from 'react';
import { Link } from 'react-router-dom';
import styled from 'styled-components';
import { useAuth } from '../../contexts/AuthContext';
import { useWishlist } from '../../contexts/WishlistContext';
import { useCart } from '../../contexts/CartContext';

const Card = styled.div`
  background-color: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  
  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
  }
`;

const ImageContainer = styled.div`
  position: relative;
  overflow: hidden;
  height: 250px;
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
  }
  
  ${Card}:hover & img {
    transform: scale(1.05);
  }
`;

const ImageOverlay = styled.div`
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(245, 166, 35, 0.2);
  opacity: 0;
  transition: opacity 0.3s ease;
  
  ${Card}:hover & {
    opacity: 1;
  }
`;

const WishlistButton = styled.button<{ isInWishlist: boolean }>`
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(255, 255, 255, 0.8);
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: background 0.2s ease;
  z-index: 1;
  
  i {
    color: ${props => props.isInWishlist ? '#f5a623' : '#333'};
  }
`;

const ProductCard = ({ product }) => {
  // Component implementation
  return (
    <Card>
      <Link to={`/products/${product.id}`}>
        <ImageContainer>
          <img src={product.image_url} alt={product.name} />
          <ImageOverlay />
          <WishlistButton 
            isInWishlist={false}
            onClick={(e) => {
              e.preventDefault();
              // Toggle wishlist
            }}
          >
            <i className="fa-heart"></i>
          </WishlistButton>
        </ImageContainer>
      </Link>
      
      {/* Product details and add to cart button */}
    </Card>
  );
};
```

### Authentication Implementation

```tsx
// src/contexts/AuthContext.tsx
import React, { createContext, useContext, useState, useEffect } from 'react';
import api from '../services/api';

interface User {
  id: number;
  email: string;
  first_name: string;
  last_name: string;
}

interface AuthContextType {
  user: User | null;
  isAuthenticated: boolean;
  loading: boolean;
  login: (email: string, password: string) => Promise<void>;
  register: (userData: any) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const token = localStorage.getItem('token');
    
    if (token) {
      api.get('/auth/me')
        .then(response => {
          setUser(response.data);
        })
        .catch(() => {
          localStorage.removeItem('token');
        })
        .finally(() => {
          setLoading(false);
        });
    } else {
      setLoading(false);
    }
  }, []);
  
  const login = async (email: string, password: string) => {
    const response = await api.post('/auth/login', { email, password });
    localStorage.setItem('token', response.data.token);
    setUser(response.data.user);
  };
  
  const register = async (userData: any) => {
    const response = await api.post('/auth/register', userData);
    localStorage.setItem('token', response.data.token);
    setUser(response.data.user);
  };
  
  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{
      user,
      isAuthenticated: !!user,
      loading,
      login,
      register,
      logout
    }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => useContext(AuthContext)!;
```

## Backend API Implementation

Now let's implement key backend controllers:

```javascript
// src/controllers/authController.js
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const db = require('../models/db');

const generateToken = (id) => {
  return jwt.sign({ id }, process.env.JWT_SECRET, {
    expiresIn: '30d',
  });
};

exports.register = async (req, res) => {
  try {
    const { firstName, lastName, email, password } = req.body;
    
    // Check if user exists
    const userExists = await db.query(
      'SELECT * FROM users WHERE email = $1',
      [email]
    );
    
    if (userExists.rows.length > 0) {
      return res.status(400).json({ message: 'User already exists' });
    }
    
    // Hash password
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(password, salt);
    
    // Create user
    const result = await db.query(
      'INSERT INTO users (email, password_hash, first_name, last_name) VALUES ($1, $2, $3, $4) RETURNING id, email, first_name, last_name',
      [email, hashedPassword, firstName, lastName]
    );
    
    const user = result.rows[0];
    
    res.status(201).json({
      user,
      token: generateToken(user.id),
    });
  } catch (error) {
    console.error(error);
    res.status(500).json({ message: 'Server error' });
  }
};

// Login and getMe controller methods would follow
```

### Stripe Integration for Checkout

```javascript
// src/controllers/checkoutController.js
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
const db = require('../models/db');

exports.createPaymentIntent = async (req, res) => {
  try {
    const { amount, currency = 'usd' } = req.body;
    
    // Create a PaymentIntent
    const paymentIntent = await stripe.paymentIntents.create({
      amount: Math.round(amount * 100), // Convert to cents
      currency,
      automatic_payment_methods: {
        enabled: true,
      },
    });
    
    res.json({
      clientSecret: paymentIntent.client_secret,
    });
  } catch (error) {
    console.error('Error creating payment intent:', error);
    res.status(500).json({ message: 'Failed to create payment intent' });
  }
};
```

### Order Management

```javascript
// src/controllers/orderController.js
const db = require('../models/db');

exports.createOrder = async (req, res) => {
  try {
    const { payment_intent_id, items, total_amount, shipping_address, billing_address } = req.body;
    const userId = req.user.id;
    
    // Start a transaction
    await db.query('BEGIN');
    
    // Create order
    const orderResult = await db.query(
      `INSERT INTO orders 
       (user_id, status, total_amount, shipping_address, billing_address, payment_intent_id) 
       VALUES ($1, $2, $3, $4, $5, $6)
       RETURNING id`,
      [
        userId,
        'paid',
        total_amount,
        shipping_address,
        billing_address,
        payment_intent_id
      ]
    );
    
    const orderId = orderResult.rows[0].id;
    
    // Create order items
    for (const item of items) {
      await db.query(
        `INSERT INTO order_items 
         (order_id, product_id, quantity, price_at_purchase) 
         VALUES ($1, $2, $3, $4)`,
        [orderId, item.id, item.quantity, item.price]
      );
    }
    
    // Commit transaction
    await db.query('COMMIT');
    
    res.status(201).json({
      id: orderId,
      message: 'Order created successfully',
    });
  } catch (error) {
    // Rollback transaction on error
    await db.query('ROLLBACK');
    console.error('Error creating order:', error);
    res.status(500).json({ message: 'Failed to create order' });
  }
};

// Methods for getUserOrders, getOrderById, and cancelOrder would follow
```

## Checkout Flow Implementation

```tsx
// src/pages/CheckoutPage.tsx
import React, { useState, useEffect } from 'react';
import { useNavigate } from 'react-router-dom';
import styled from 'styled-components';
import { loadStripe } from '@stripe/stripe-js';
import { Elements, CardElement, useStripe, useElements } from '@stripe/react-stripe-js';
import { useCart } from '../contexts/CartContext';
import api from '../services/api';

// Load Stripe
const stripePromise = loadStripe('pk_test_your_stripe_public_key');

const CheckoutPage = () => {
  const [clientSecret, setClientSecret] = useState('');
  const { cartItems, getCartTotal } = useCart();
  const navigate = useNavigate();
  
  useEffect(() => {
    // Redirect if cart is empty
    if (cartItems.length === 0) {
      navigate('/cart');
      return;
    }
    
    // Create payment intent when component mounts
    const getPaymentIntent = async () => {
      try {
        const response = await api.post('/checkout/create-payment-intent', {
          amount: getCartTotal(),
          currency: 'usd',
        });
        setClientSecret(response.data.clientSecret);
      } catch (err) {
        console.error('Error creating payment intent:', err);
      }
    };
    
    getPaymentIntent();
  }, [cartItems, getCartTotal, navigate]);
  
  return (
    <div>
      <h1>Checkout</h1>
      
      {clientSecret && (
        <Elements stripe={stripePromise} options={{ clientSecret }}>
          <CheckoutForm />
        </Elements>
      )}
    </div>
  );
};

// CheckoutForm component implementation would follow
```

## Testing Strategy

For a robust implementation, we'll implement comprehensive testing:

1. Unit tests for critical components and utility functions
2. Integration tests for API endpoints
3. End-to-end tests for key user flows:
   - User registration and login
   - Product browsing and filtering
   - Adding/removing from cart
   - Checkout process
   - Order management

## Design Implementation Following Plenaire Aesthetic

The implementation will feature:

- Clean, minimalist design with ample white space
- Subtle hover animations on interactive elements
- Color gradients for depth and visual interest
- Responsive design for all screen sizes
- Consistent typography and visual language throughout

The product grid will utilize CSS Grid with hover effects that subtly scale images and reveal overlays, creating an engaging browsing experience consistent with the luxury brand positioning.
