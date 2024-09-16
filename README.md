***readline: 

***conversionRates



// Predefined currency conversion rates
const conversionRates = {
    USD: 1,
    EUR: 0.85,
    GBP: 0.75
};

***Product Class
class Product {
    constructor(id, name, price, category) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.category = category;
    }
}

***Cart Item Class
class CartItem {
    constructor(product, quantity) {
        this.product = product;
        this.quantity = quantity;
    }

    getTotalPrice() {
        return this.product.price * this.quantity;
    }
}

***Cart Class
class Cart {
    constructor() {
        this.items = [];
    }

    addItem(product, quantity) {
        const existingItem = this.items.find(item => item.product.id === product.id);
        if (existingItem) {
            existingItem.quantity += quantity;
        } else {
            this.items.push(new CartItem(product, quantity));
        }
        console.log(`Added ${quantity} ${product.name} to the cart.`);
    }

    removeItem(productId, quantity) {
        const index = this.items.findIndex(item => item.product.id === productId);
        if (index !== -1) {
            const item = this.items[index];
            if (item.quantity <= quantity) {
                this.items.splice(index, 1);
                console.log(`Removed all ${item.product.name} from the cart.`);
            } else {
                item.quantity -= quantity;
                console.log(`Reduced quantity of ${item.product.name} by ${quantity}.`);
            }
        } else {
            console.log(`Item with ID ${productId} not found in the cart.`);
        }
    }

    viewCart() {
        if (this.items.length === 0) {
            console.log('Your cart is empty.');
            return;
        }

        console.log('Your Cart:');
        let totalBeforeDiscount = 0;
        this.items.forEach(item => {
            const total = item.getTotalPrice();
            totalBeforeDiscount += total;
            console.log(`${item.product.name} - Quantity: ${item.quantity}, Price: ${item.product.price.toFixed(2)} USD, Total: ${total.toFixed(2)} USD`);
        });

        console.log(`Total (before discounts): ${totalBeforeDiscount.toFixed(2)} USD`);
    }

    checkout() {
        let total = 0;
        let totalDiscount = 0;

        console.log('Applying discounts...');
        this.items.forEach(item => {
            let itemTotal = item.getTotalPrice();
            if (item.product.category === 'Fashion' && item.quantity >= 2) {
                const freeItems = Math.floor(item.quantity / 2);
                const discount = freeItems * item.product.price;
                totalDiscount += discount;
                console.log(`Buy 1 Get 1 Free on ${item.product.name} applied. Discount: ${discount.toFixed(2)} USD`);
            }
            if (item.product.category === 'Electronics') {
                const discount = itemTotal * 0.1;
                totalDiscount += discount;
                console.log(`10% Off on ${item.product.name} applied. Discount: ${discount.toFixed(2)} USD`);
            }
            total += itemTotal;
        });

        total -= totalDiscount;
        console.log(`Final Total in USD: ${total.toFixed(2)} USD`);

        return total;
    }
}

***Discount Class (to display available discounts)
class Discount {
    static listDiscounts() {
        console.log('Available Discounts:');
        console.log('1. Buy 1 Get 1 Free on Fashion items');
        console.log('2. 10% Off on Electronics');
    }
}

***Currency Converter Class
class CurrencyConverter {
    static convert(amount, currency) {
        if (!conversionRates[currency]) {
            console.log('Unsupported currency.');
            return amount;
        }

        return amount * conversionRates[currency];
    }
}

***Product Catalog
const productCatalog = [
    new Product('P001', 'Laptop', 1000.00, 'Electronics'),
    new Product('P002', 'Phone', 500.00, 'Electronics'),
    new Product('P003', 'T-Shirt', 20.00, 'Fashion')
];

***Create Cart Instance
const cart = new Cart();

***Command-Line Interface (CLI) Setup
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});


***Commands to help user
const menu = `
Commands:
1. add_to_cart <ProductID> <Quantity>
2. remove_from_cart <ProductID> <Quantity>
3. view_cart
4. list_discounts
5. checkout
6. exit
Enter your command: `;


***HandleInput
function handleCommand(command) {
    const [action, productId, quantity] = command.split(' ');

    switch (action) {
        case 'add_to_cart':
            const productToAdd = productCatalog.find(p => p.id === productId);
            if (productToAdd) {
                cart.addItem(productToAdd, parseInt(quantity));
            } else {
                console.log(`Product with ID ${productId} not found.`);
            }
            break;
        case 'remove_from_cart':
            cart.removeItem(productId, parseInt(quantity));
            break;
        case 'view_cart':
            cart.viewCart();
            break;
        case 'list_discounts':
            Discount.listDiscounts();
            break;
        case 'checkout':
            const total = cart.checkout();
            rl.question('Would you like to view it in a different currency? (yes/no): ', answer => {
                if (answer.toLowerCase() === 'yes') {
                    rl.question('Enter currency (USD, EUR, GBP): ', currency => {
                        const convertedTotal = CurrencyConverter.convert(total, currency);
                        console.log(`Final Total in ${currency}: ${convertedTotal.toFixed(2)} ${currency}`);
                        rl.prompt();
                    });
                } else {
                    rl.prompt();
                }
            });
            break;
        case 'exit':
            rl.close();
            break;
        default:
            console.log('Invalid command.');
            break;
    }
}

console.log(menu);
rl.prompt();
rl.on('line', handleCommand);
