# QuBit Universal Variables Specification Version 0.9 (DRAFT)

QuBit Universal Variables are our suggested way to structure the data presented on your pages. With QuBit Universal Variables, our aim is to help you easily access the pieces of data you need on your pages from your containers.

Below you will see 7 different main objects:
* [User](#user): Details of the logged in user, or the visitor
* [Page](#page): Details of the page type and category
* [Product](#product): Details of the product the user is currently viewing
* [Basket](#basket): Details of the products the user has added to the
* [Transaction](#transaction): Details of a purchase or transaction
* [Search](#search): Details of a search result
* [Recommendation](#recommendation): Details of a recommendataion

QuBit OpenTag recommends creating the releavant JavaScript object on your page prior to the OpenTag container script. This will assure the values are present on the page when a script tries to access them.

If a page does not have the variables that are mentioned below, simply do not even declare them. For example, if your pages only have category and no subcategory, just declare your category. Likewise, if you feel the need to extend the variables below or feel like renaming them, please do so; However, please keep in mind the new variable names or the edited ones, because in order to access them from your scripts in your container, you will need to indicate the new variable names.

Below for each object and variable you will see a data type: String or Number; along with examples and comments about what they represent. Please review them carefully.  When you implement them, please make sure that the resultant generated code is be valid JavaScript.

## Namespace

All universal variable should be assigned to `window` object under `universal_variable` object.

example:

``` javascript
window.universal_variable = {};
```


## Version

A version variable defines the current specificaiton version is used.

```javascript
window.universal_variable.version = "0.9"
```

## Page

The Page Universal Variable describes a page type with category or sub-category. It may be created on any page. 

Specification:

``` javascript
window.universal_variable = {
	page: {
		// {String}
		// The type of page this is, i.e: home, product, category, search,
		// basket, checkout, confirmation
		category: "product",

		// {String}
		// A more detailed description of the page, eg: if Category is
		// "category", subCategory may be "Mens Shirts"
		sub_category: "Mens Shirts",

		// {String}
		// Optional, describe a version of the page if the page
		// has different versions for testing
		version: ""
	}
}
```

or

``` javascript
window.universal_variable.page = {
	// {String}
	// The type of page this is, i.e: home, product, category, search,
	// basket, checkout, confirmation
	category: "product",

	// {String}
	// A more detailed description of the page, eg: if Category is
	// "category", subCategory may be "Mens Shirts"
	sub_category: "Mens Shirts"
}
```

## User

The User Universal Variable describes a user. It may be created on any page.

Specification:
``` javascript
window.universal_variable = {
	user: {
		// {String}
		// The name of the user
		name: "Name",

		// {String}
		// The name with which the user uses to login
		username: "username",

		// {String}
		// An internal user identifier
		user_id: "",

		// {String} 
		// The email address of the logged in user
		email: "user@example.com",

		// {String}
		// User preferred Language from a language standard list
		// ISO language code list ISO 639-1
		language: "en-gb",

		// {Boolean}
		// true if the user is a returning user, otherwise false
		returning: true,
		
		// {Number}
		// Facebook id of the logged in user
		facebook_id: 12345678901232345,

		// {String}
		// Twitter id
		twitter_id: "myid"

	}
}
```

## Product

The Product universal variable describes a single product information. `Product` is usually displayed in a number of pages, or even sections with in a page. The Product Universal Variable provides a single specification to desribe a product information. 

When the variable is used with `product` key, it represents a single product page. It can also composite with other universal variables:
* [`product` variable](#product) itself may also have an array of products as sub products
* [`basket` variable](#basket) may have an array of products as items in the basket
* [`transaction` variable](#transaction) may have an array of products as purchasd products
* [`search` variable](#search) may have an array of products as search results
* [`recommendation` variable](#recommendation) may have an array of products as recommended items for purchase

Specification:
``` javascript
window.universal_variable = {
	product: {
		// {String}
		// The identifier for the item that is being viewed. This is product ID, NOT SKU id
		id: "ABC123",

		// {String}
		// The name of the product that is being viewed
		name: "XYZShoes",

		// {String}
		// The manufactuter of the product that is being viewed
		manufacturer: "Acme Corp",

		// {String}
		// The category of the product that is being viewed
		category: "Shoe",

		// {String}
		// The sub-category of the product that is being viewed
		sub_category: "Trainers",

		// {Array} of {Product Universal Variable}
		// An array of sub products
		sub_products: [Product, Product, Product, ...],

		// {Array} of {SKU} object
		// An SKU code for each item that is being viewed - these should be unique for items
		// which differ by color or size. This is for the case where multiple SKUs
		// are selectable
		skus: [
			{
				id: 123,
				color: "",
				size: "",
				stock: 10
			}
		],

		// {String}
		// The current selected product SKU
		selected_sku: {
			
			id: 123,

			// {String}
			// A text describes color of the item
			color: "",
			
			size: "",

			// {Number} 
			// A number indicates the stock avalability. Set the value to 0 if the item is out of stock
			stock: 10,

			// {Price}
			// price object, The cost of a single unit of the item that is being viewed
			// { GBP: 123.00, EUR: 150.00 }
			unit_price: 123.00,

			// {Number}
			// The number of units of this item that are selected
			quantity: 1,


			// {Price}
			// The price of the item taking into account any sales or special
			// circumstances
			unit_sale_price: { GBP: 100.00, EUR: 130.00 }

		},

		// Color
		color: "WHITE",

		// {Price}
		// price object, The cost of a single unit of the item that is being viewed
		// { GBP: 123.00, EUR: 150.00 }
		unit_price: { GBP: 123.00, EUR: 150.00 },

		// {Number}
		// The price of the item taking into account any sales or special
		// circumstances
		unit_sale_price: { GBP: 123.00, EUR: 150.00 },

		// {Number}
		// The voucher code entered (only necessary if different from transaction)
		voucher: "MYVOUCHER"
	}
}
```

## Basket

The Basket Universal Variable describes the current state of the a user's shopping basket. It also requires a list of [Product Universal Variable](#product) in the `item` array.

Specification:
``` javascript
window.universal_variable = {
	basket: {
		// {String}
		// The standard letter code in capitals for the currency type in which the
		// order is being paid, eg: EUR, USD, GBP
		selected_currency: "GBP",

		// {Price}
		// A valid number with the total cost of the basket including any known tax per
		// item, but not including shipping or discounts
		subtotal: { GBP: 123.00, EUR: 150.00 },

		// {Number}
		// A valid number with the total amount of potential tax included in the order
		tax: 12.00,

		// {Number}
		// A valid number with the total amount of potential shipping costs included in
		// the order
		shipping_cost: 1.00,

		// {Number}
		// A valid number with the total cost of the basket including any known tax,
		// shipping and discounts
		total: 123.00,

		// {Array} of {Product Universal Variable}
		// An array of products
		items: [Product, Product, Product, ...]
	}
}
```


## Transaction
Transaction Universal Variable describes a completed purchase. It also requires a list of [Product Universal Variable](#product) in the `item` array.

Specification:
```javascript
window.universal_variable = {
	transaction: {
		// {String}
		// A unique identifier for the order
		order_id: "WEB123456",

		// {String}
		// The standard letter code in captials for the currency type in which
		// the order is being paid, eg EUR, USD, GBP
		currency: "GBP",

		// {Number}
		// A valid number with the total amount the order,
		// but not including shipping or discounts
		subtotal: 123.00,

		// {Boolean}
		// A boolean true or false to indicate whether subtotal includes tax
		subtotal_include_tax: true,

		// {Number}
		// A valid number with the total amount of tax included in the order
		tax: 10.00,

		// {Number}
		// A valid number with the total amount of shipping costs included in the
		// order
		shipping_cost: 0.00,

		// {Number}
		// A valid number with the total cost including tax, shipping and discounts
		total: 130.00,

		delivery: {		
			// {String}
			// The city to which the order is to be dispatched
			city: "London",

			// {String}
			// The state to which the order is to be dispatched
			state: "London",

			// {String}
			// The post code to which the order is to be dispatched
			postcode: "SW1 1AB",
			
			// {String}
			// The country to which the order is to be dispatched
			country: "UK",
		},


		billing: {		
			// {String}
			// The city to which the billing is set
			city: "London",

			// {String}
			// The state to which the billing is set
			state: "London",

			// {String}
			// The postcode to which the billing is set
			postcode: "SW1 1AB",
			
			// {String}
			// The country to which the billing is set
			country: "UK",
		},

		// {String}
		// The voucher code entered
		voucher: "MYVOUCHER",

		// {Number}
		// A valid number with the total amount of discount due to the voucher entered
		voucher_discount: 0.00,

		// {Array} of {Product Universal Variable}
		// An array of product
		items: [Product, Product, Product, ...]
	}
}
```

## Search

Search Universal Variable describes a search results. It contains a list of [Product Universal Variable](#product)

Specification:
```javascript
window.universal_variable = {
	search: {
		items: [Product, Product, Product, ...]
	}
}
```

## Recommendation

Recommendation Universal Variable describes a recommended product list. It contains a list of [Product Universal Variable](#product)

Specification:
```javascript
window.universal_variable = {
	recommendation: {
		items: [Product, Product, Product, ...]
	}
}
```
