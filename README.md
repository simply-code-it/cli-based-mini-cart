# Mini E-commerce Cart System (Command-Line Application)

This project implements a Mini E-commerce Cart System using Node.js, designed to be a command-line application. Users can interact with the system to add items to a cart, apply discounts, view the total price in different currencies, and checkout.

# Features
* Add items to a cart with specific quantities.
* Remove items from the cart or adjust their quantities.
* View current cart items and total cost.
* Apply dynamic discounts to items.
* Support for multiple currencies (USD, EUR, GBP).
* Checkout to calculate the final price after discounts and currency conversion.


# 1. Product
This class represents a product that can be added to the cart.
## Attributes:
* id (string): Unique identifier for the product.
* name (string): Name of the product.
* price (float): Price of the product in USD.
* category (string): Category of the product (e.g., Electronics, Fashion).

# 2. CartItem
This class represents an item in the shopping cart. It holds a reference to the Product and stores the quantity of that product added to the cart.
## Attributes:
* product (Product): The product object added to the cart.
* quantity (number): The quantity of the product.
## Methods:
* getTotalPrice(): Returns the total price for the product in the cart (price * quantity).

# 3. Cart
This class manages the shopping cart. It is responsible for adding and removing items, viewing the cart, and calculating the total cost (before and after applying discounts).
## Attributes:
* items (Array of CartItem): List of all items in the cart.
## Methods:
* addItem(product, quantity): Adds a product to the cart with a specified quantity.
* removeItem(productId, quantity): Removes a product from the cart or reduces its quantity.
* viewCart(): Displays the current items in the cart along with their details (name, quantity, price, total cost).
* checkout(): Applies available discounts and calculates the final total price in USD.

# 4. Discount
This class represents the discount system. It currently supports two types of discounts:
* Buy 1 Get 1 Free for Fashion category items.
* 10% Off on Electronics category items.
## Methods:
* listDiscounts(): Lists all available discounts that can be applied at checkout.

# 5. CurrencyConverter
This class handles currency conversion. It supports fixed conversion rates between USD and other currencies (EUR, GBP).
## Methods:
* convert(amount, currency): Converts the amount from USD to the specified currency.

# Command-Line Interface (CLI)
The system is designed to be used from the command line. Users can interact with it by typing specific commands to add items, view the cart, apply discounts, and checkout.
## Supported Commands:
* add_to_cart <ProductID> <Quantity>: Adds a product to the cart.
* remove_from_cart <ProductID> <Quantity>: Removes or reduces the quantity of a product in the cart.
* view_cart: Displays the current cart contents and total price (before discounts).
* list_discounts: Lists all available discounts that can be applied.
* checkout: Applies available discounts, calculates the total, and asks if the user wants to view the total in a different currency.
* exit: Will close the terminal(application).
* help: List all the commands for the user.


# Pre-populated Product Catalog
The system comes pre-populated with the following products:
* Laptop (Electronics): 1000.00 USD
* Phone (Electronics): 500.00 USD
* T-Shirt (Fashion): 20.00 USD

# How to Run
* Clone this repository.
* Navigate to the project directory.
* Run the application: node src/index.js
* Use the command-line to interact with the system using the supported commands.
