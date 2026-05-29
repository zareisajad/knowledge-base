A good rule:

- `shared/` = reusable across domains
- `features/products/` = product business logic + product UI
- `features/cart/` = cart-specific UI

You should move something into `shared/` only after:

1. you reused it 2-3 times
2. the abstraction becomes obvious
3. it no longer carries domain assumptions


```
src/
├── app/
│
├── features/
│   ├── auth/
│   ├── cart/
│   ├── categories/
│   ├── checkout/
│   ├── navigation/
│   ├── products/ # inside each directory
|	|   |components/
|	|	|utils/
|	|	|types/
|	|	|hooks/
│   ├── profile/
│   └── search/
│
├── shared/
│   ├── components/
│   ├── lib/
│   ├── types/
│   ├── ui/
│   └── utils/
│
├── providers/
│
├── styles/
│
└── middleware.ts
```