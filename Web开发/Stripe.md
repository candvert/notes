```ts
import Stripe from 'stripe';

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!);

const products = await stripe.products.list({ limit: 10 });
const prices = await stripe.prices.list({ limit: 10 });

console.log(products.data);
console.log(prices.data);
```