# reaction-jsdoc

## requirements

The Core API Documentation uses [JSDoc](http://usejsdoc.org/) and the [docdash](https://github.com/clenemt/docdash) theme to build its static site.

Install requirements with:

```sh
npm install jsdoc
npm install docdash
```

## how to build docs

Make sure you have a `jsdoc.json` configuration file in the `reaction` repository. Then run:

```sh
jsdoc --verbose --configure jsdoc.json
```

See [here](#example-jsdoc-configuration) for an example configuration file.

## how to write docs

![image](https://user-images.githubusercontent.com/1203639/30391482-b94e7666-9876-11e7-9336-3a306825fb76.png)


### documenting a file

In every file, following `import`:

```/**
 * @file Shopify connector webhook methods
 *       contains methods for creating and deleting webhooks providing
 *       synchronization between a Shopify store and a Reaction shop
 * @module connectors/shopify/webhooks
 */
```

### documenting a function

Document a function by adding comments above the function definition with the following tags:

#### required:
- `@method` name
- `@summary` can use Markdown here
- `@param` {type} name - description, use `[]` square brackets around param for optional params
- `@return` {type} name - description, or `@return {undefined}` 

#### optional:
- `@async`
- `@public` or `@private`
- `@default`
- `@deprecated` since version number
- `@since` version number
- `@todo` any todo notes here

#### other things:
* `@ignore` - if you don't want the function to output docs
* Lowercase type names, like `object`, `string`

Example of a method:

**examples:**

```js
/**
 * @method quantityProcessing
 * @summary perform calculations admissibility of adding product to cart
 * @param {object} product - product to add to Cart
 * @param {number} itemQty - qty to add to cart, defaults to 1, deducts from inventory
 * @since 1.10.1
 * @return {number} quantity - revised quantity to be added to cart
 */
```

```js
/**
 * Meteor method for creating a shopify webhook for the active shop
 * See: https://help.shopify.com/api/reference/webhook for list of valid topics
 * @async
 * @method connectors/shopify/webhooks/create
 * @param {object} options Options object
 * @param {string} options.topic - the shopify topic to subscribe to
 * @param {string} [options.absoluteUrl] - Url to send webhook requests - should only be used in development mode
 * @return {undefined}
 */
```

```js
/**
 * Transforms a Shopify product into a Reaction product.
 * @private
 * @method createReactionProductFromShopifyProduct
 * @param  {object} options Options object 
 * @param  {object} options.shopifyProduct the Shopify product object
 * @param  {string} options.shopId The shopId we're importing for
 * @param  {[string]} options.hashtags An array of hashtags that should be attached to this product.
 * @return {object} An object that fits the `Product` schema
 *
 * @todo consider abstracting private Shopify import helpers into a helpers file
 */
```
 
**Documentation:**

<h4 class="name" id="quantityProcessing"><span class="type-signature"></span>quantityProcessing<span class="signature">(product, variant, itemQty)</span><span class="type-signature"> → {Number}</span></h4>

<p class="summary">perform calculations admissibility of adding product to cart</p>

<dl class="details">
  <dt class="tag-source">Source:</dt>
  <dd class="tag-source">
    <ul class="dummy">
      <li>
        <a href="server_methods_core_cart.js.html">server/methods/core/cart.js</a>, <a href="server_methods_core_cart.js.html#line19">line 19</a>
      </li>
    </ul>
  </dd>
  <dt class="tag-since">Since:</dt>
  <dd class="tag-since">
    <ul class="dummy">
      <li>1.10.1</li>
    </ul>
  </dd>  
</dl>

<h5>Parameters:</h5>

<table class="params">
  <thead>
    <tr>
      <th>Name</th>
      <th>Type</th>
      <th>Default</th>
      <th class="last">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="name"><code>product</code></td>
      <td class="type">
        <span class="param-type">object</span>
      </td>
      <td class="default"></td>
      <td class="description last"><p>product to add to Cart</p></td>
    </tr>
    <tr>
      <td class="name"><code>variant</code></td>
      <td class="type">
        <span class="param-type">Object</span>
      </td>
      <td class="default"></td>
      <td class="description last"><p>product variant</p></td>
    </tr>
    <tr>
      <td class="name"><code>itemQty</code></td>
      <td class="type">
        <span class="param-type">number</span>
      </td>
      <td class="default">
        <code>1</code>
      </td>
      <td class="description last"><p>qty to add to cart, defaults to 1, deducts from inventory</p></td>
    </tr>
  </tbody>
</table>

See the resulting documentation live: https://reactioncommerce.github.io/reaction-jsdoc/global.html#quantityProcessing

### documenting React components

Document a React component's props with `@property` tags:

```js
/**
  * @summary React component for displaying a not-found view
  * @param {Object} props - React PropTypes
  * @property {String|Object} className - String className or `classnames` compatible object for the base wrapper
  * @property {String|Object} contentClassName - String className or `classnames` compatible object for the content wrapper
  * @property {String} i18nKeyMessage - i18n key for message
  * @return {Node} React node containing not-found view
  */
 ```

Document a Class with a one-line description of the class:

```js
/** Class representing the Products React component
  * @summary PropTypes for Product React component
  * @property {Function} loadMoreProducts - load more products callback
 */
```

More on documenting classes from JSDoc: http://usejsdoc.org/howto-es2015-classes.html

### organizing modules

Any file that a developer will require using `import` should be defined as a module with `@module` at the top of the file:

```
/**
 * Accounts module.
 * @module ./accounts
 */
```

More on organizing modules from JSDoc: http://usejsdoc.org/howto-es2015-modules.html

## example jsdoc configuration

Create a `jsdoc.json` file in the `reaction` repository. This file configures what files will be parsed for JSDoc comments, what theme to use and what plugins to use.

```js
{
  "plugins": ["plugins/markdown", "plugins/summarize"],
  "markdown": {
    "parser": "gfm"
  },
  "source": {
    "exclude": [
      ".git",
      ".meteor",
      "node_modules",
      "private",
      "client/plugins.js",
      "server/plugins.js",
      "custom/client",
      "custom/server",
      "public/custom",
      "private/custom",
      "imports/plugins/custom"
    ],
    "includePattern": ".+\\.js(x|doc)?$"
  },
  "opts": {
    "template": "/reaction-jsdoc/node_modules/docdash",
    "encoding": "utf8",
    "destination": "/reaction-jsdoc/",
    "recurse": true,
    "verbose": true
  },
  "sourceType": "module",
  "templates": {
      "cleverLinks": false,
      "monospaceLinks": false,
      "default": {
        "includeDate": false
      }
  },
  "docdash": {
    "static": [true],
    "sort": [false]
  },
  "tags": {
    "allowUnknownTags": true,
    "dictionaries": ["jsdoc"]
  }
}
```
