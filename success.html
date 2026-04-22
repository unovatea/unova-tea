// netlify/functions/create-checkout.js
// This runs on Netlify's servers — your Stripe secret key never touches the browser.

const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

exports.handler = async (event) => {
  // Only allow POST
  if (event.httpMethod !== 'POST') {
    return { statusCode: 405, body: 'Method Not Allowed' };
  }

  let body;
  try {
    body = JSON.parse(event.body);
  } catch {
    return { statusCode: 400, body: 'Invalid JSON' };
  }

  const { items, customerEmail, customerName, businessName, phone, shipping } = body;

  // Validate minimum order
  const subtotal = items.reduce((sum, item) => sum + item.price, 0);
  if (subtotal < 100) {
    return {
      statusCode: 400,
      body: JSON.stringify({ error: 'Minimum order of $100 not met.' }),
    };
  }

  // Build Stripe line items from the order
  // Each item: { name, price (in dollars), qty, description }
  const lineItems = items.map((item) => ({
    price_data: {
      currency: 'usd',
      unit_amount: Math.round(item.price * 100), // Stripe uses cents
      recurring: { interval: 'month' },
      product_data: {
        name: item.name,
        description: item.description,
      },
    },
    quantity: 1,
  }));

  // Add shipping as a line item if applicable
  if (shipping > 0) {
    lineItems.push({
      price_data: {
        currency: 'usd',
        unit_amount: Math.round(shipping * 100),
        recurring: { interval: 'month' },
        product_data: {
          name: 'Monthly Delivery',
          description: 'Standard shipping fee',
        },
      },
      quantity: 1,
    });
  }

  try {
    const session = await stripe.checkout.sessions.create({
      mode: 'subscription',
      payment_method_types: ['card'],
      line_items: lineItems,
      customer_email: customerEmail,
      metadata: {
        business_name: businessName,
        customer_name: customerName,
        phone: phone || '',
      },
      subscription_data: {
        metadata: {
          business_name: businessName,
          source: 'unova-tea-portal',
        },
      },
      success_url: `${process.env.URL}/success.html?session_id={CHECKOUT_SESSION_ID}`,
      cancel_url: `${process.env.URL}/`,
      billing_address_collection: 'required',
      phone_number_collection: { enabled: true },
    });

    return {
      statusCode: 200,
      body: JSON.stringify({ url: session.url }),
    };
  } catch (err) {
    console.error('Stripe error:', err.message);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: err.message }),
    };
  }
};
