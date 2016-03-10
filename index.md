---
layout: default
---
# [Shopify](https://www.shopify.com) JavaScript Buy SDK

The JS Buy SDK is a lightweight library that allows you to build ecommerce into any website.
It is based on Shopify's API and provides the ability to retrieve products and collections from your shop,
add products to a cart, and checkout.

Using the JS Buy SDK, you can:

- fetch information about a single product or a collection of products
- create a shopping cart
- allow customers to select options and quantities
- generate a checkout URL for a single product or an entire cart


## Including the Buy SDK

```html
<script src="cdn.shopify.com/scripts/js_buy.js">
```

## Creating a Shop Client

The Client is the primary interface through which you make requests using the JS Buy SDK.
You will need your `myshopify.com` domain, API key, and channelId to create your client and
begin making requests. [Where do I find my API Key and channelId?](#)

```js
var shopClient = ShopifyBuy.buildClient({
  apiKey: '123',
  myShopifyDomain: 'haris-mahmood',
  channelId: '123'
});
```

> **Note**: You will need to publish the product/collection you wish to interact with to the
> "Buy Button" channel in Shopify

## Making a request

You can now call a `fetch` method on your client to retrieve products or collections.
All `fetch` methods return a promise which will return a `Cart` or `Collection` model for `fetchProduct`
or `fetchCollection`, or an array of `Cart` or `Collection` models for `fetchAllProducts` or `fetchAllCollection`.

To request an individual resource, you will need to pass that resource's ID as the first argument.

```js
shopClient.fetchProduct(1234)
  .then(function (product) {
    console.log(product);
  })
  .catch(function () {
    console.log('Request failed');
  });
```

## Creating a Checkout

To generate a checkout link, you will need to create a cart model.

```js
var cart;
shopClient.create('checkout').then(function (cart) {
  cart = cart;
})
```

### Adding items to a cart

Cart items are stored in the `Cart.attrs.line_items` array. To add items to a cart,
you must call the client's `update` method and pass along the cart with updated line items.
The `update` call will return a promise which returns the updated model.

```js
cart.attr.line_items.push({
  variant_id: 123,
  quantity: 1
});

client.update('checkouts', cart).then(function (cart) {
  cart = cart;
});

```

### Creating a checkout URL

You can generate a checkout URL for a given cart at any time by retrieving the `Cart.attrs.web_url`.

```js
var checkout = window.open();
checkout.location = cart.attrs.web_url;
```