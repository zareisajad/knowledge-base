### use client

There’s this concept of **server-side rendering (SSR)** and **client-side rendering (CSR)**.

- **Server** = the machine where your app is deployed (like Vercel, a VPS, or any Node.js server).
- **Client** = the user's **browser**.

Next.js, by default, **renders components on the server** (SSR) — this is good for SEO, speed, and sending less JavaScript to the browser.

###### When do we use 'use client'?

If your file uses:

- React hooks (`useEffect`, `useState`, `useRef`, etc.)
- Browser-only APIs (`localStorage`, `window`, etc.)
- Interactivity (click events, input fields, etc.)

Then Next.js can’t render it on the server — it has to run in the browser. So you must add: 'use client';

#### What does Next.js do?

###### For Server Components (default behavior)

- It runs your component on the **server**.
- It generates **pure HTML**.
- It sends this HTML to the browser (super fast, great for SEO).
- Page is visible almost instantly.

###### For Client Components (with `'use client'`)

- It **can’t render the component’s output on the server** because it needs browser-only stuff like `useState`.

- So instead:
    - It may send a **placeholder or shell HTML** (not fully populated).
    - Then it ships the **entire component's JS** to the browser.
    - In the browser, React **hydrates** it — which means it
        - Re-runs the component
        - Sets up state, event listeners, interactivity

---
###### Why is this good for SEO?

Because **server-rendered components output full HTML** immediately — so:
- Search engines can see content right away.
- Users don’t wait for JavaScript to load to see something.
- You avoid the “blank page” problem common in React SPAs.

> We cant just benefit the SEO / instant load just by using next.js.
    we have to be actually very aware of using 'use client' and SSR components to benefit that.
	If I want my pages to load asap. I should keep that page SSR 

###### How to achieve fast load and good SEO in Next.js:

1. keep main pages SSR
2. only mark interactive parts as 'use client'
3. dont add use client to page or layouts unless you must
4. use async/await or fetch in server components for data 

> we have to create our main pages (optimized for SEO) from bunch of client side components.
> in nextjs world this is the best practice to split pages to server components and client components

```tsx

export default async function ProductPage({ params }) {
  const id = params.id;
  const product = await fetchProduct(id); // runs on server

  return (
    <div>
      <ProductHeader product={product} /> {/* server component */}
      <ProductDetails product={product} /> {/* server component */}
      <AddToCartControls productId={product.id} /> {/* client component */}
    </div>
  );
}
```

### createContext()

Context helps you manage **shared state and logic** across your app **without manually passing props** through every component level.

#### Why it’s useful:

Before, I was passing data like `cart` or `setCart` from **parent → child → grandchild**, which got messy and hard to maintain in larger apps.

Now with `CartContext`:

- I create a central "cart brain" using `createContext`.
- I wrap my app (or just part of it) using `<CartProvider>` in a `layout.tsx`.
- Any component inside that layout tree can use `useCart()` to **access or update the cart** — no need for prop drilling.

#### How it works:

- `createContext()` defines the shared data and shape.
- `CartProvider` stores and manages the actual state.
- `useCart()` is a helper hook that gives access to the state.

#### createContext Example snippet

```tsx
'use client'; // we already know what it does :)

import { createContext, useContext, useState, ReactNode } from 'react';

// 1. Define the shape of the context
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

// 2. Create the context
const ThemeContext = createContext<ThemeContextType | null>(null);

// 3. Create the Provider component
export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  const toggleTheme = () => {
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 4. Custom hook for easier usage
export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used inside ThemeProvider');
  }
  return context;
};

```


---

wanted to display an error on Cart items (updating qty) I did that like :
```tsx

const [qtyLimitErrors, setQtyLimitErrors] = useState(false);

```

then I set it to true (show error) when user hits the limit. 
but the problem is that it shows the error on all items! 

hreres the soloution:

```tsx
const [qtyLimitErrors, setQtyLimitErrors] = useState<Record<string, boolean>>({});
```

here we say in typescript that this thing will have a key whic is string and a value which is boolean.
this basically make a state like this :
 ```tsx
{
  "101-0": false, // "101-0" is my cart key
  "202-1": true,
}
```
then we update the state like this:
```tsx
setQtyLimitErrors((prev) => ({ ...prev, [cartKey]: true }));
```
.

**another cool thing is "!!" **

```tsx
showQtyLimitError={!!qtyLimitErrors[item.cartKey]}
```
what it does is it turns the state to a real boolean. 
JSX expects a boolean for conditional rendering. Using `!!` ensures:
- If value is `true` → `true`
- If value is `false` or `undefined` (or any falsy) → `false`
