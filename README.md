# task-1

class Product:
    def _init_(self, name, price):
        self.name = name
        self.price = price
        self.quantity = 0
        self.wrapped = False

    def calculate_total_price(self):
        return self.price * self.quantity

class Cart:
    def _init_(self):
        self.products = []

    def add_product(self, product, quantity, wrapped=False):
        product.quantity += quantity
        product.wrapped = wrapped
        self.products.append(product)

    def calculate_subtotal(self):
        subtotal = sum(product.calculate_total_price() for product in self.products)
        return subtotal

    def apply_discounts(self):
        total_quantity = sum(product.quantity for product in self.products)

        flat_10_discount = 10 if self.calculate_subtotal() > 200 else 0

        bulk_5_discount = sum(product.calculate_total_price() * 0.05 for product in self.products if product.quantity > 10)

        bulk_10_discount = self.calculate_subtotal() * 0.1 if total_quantity > 20 else 0

        tiered_50_discount = 0
        if total_quantity > 30:
            for product in self.products:
                if product.quantity > 15:
                    tiered_50_discount += (product.quantity - 15) * (product.price * 0.5)

        discounts = {
            "flat_10_discount": flat_10_discount,
            "bulk_5_discount": bulk_5_discount,
            "bulk_10_discount": bulk_10_discount,
            "tiered_50_discount": tiered_50_discount
        }

        applicable_discounts = {k: v for k, v in discounts.items() if v > 0}
        if applicable_discounts:
            max_discount = max(applicable_discounts, key=applicable_discounts.get)
            return max_discount, applicable_discounts[max_discount]
        else:
            return "No discount applied", 0

    def calculate_shipping_fee(self):
        total_quantity = sum(product.quantity for product in self.products)
        return (total_quantity // 10) * 5

    def calculate_gift_wrap_fee(self):
        return sum(1 for product in self.products if product.wrapped)

    def display_cart(self):
        print("Product Name\tQuantity\tTotal Price")
        for product in self.products:
            print(f"{product.name}\t\t{product.quantity}\t\t{product.calculate_total_pri()}"

        subtotal = self.calculate_subtotal()
        discount_name, discount_amount = self.apply_discounts()
        print("\nSubtotal:", subtotal)
        print("Discount:", discount_name, "-", discount_amount)

        shipping_fee = self.calculate_shipping_fee()
        gift_wrap_fee = self.calculate_gift_wrap_fee()
        print("Shipping fee:", shipping_fee)
        print("Gift wrap fee:", gift_wrap_fee)

        total = subtotal - discount_amount + shipping_fee + gift_wrap_fee
        print("Total:", total)

product_A = Product("A", 20)
product_B = Product("B", 40)
product_C = Product("C", 50)

cart = Cart()

quantity_A = int(input("Enter quantity of product A: "))
wrapped_A = input("Is product A wrapped as a gift? (yes/no): ").lower() == "yes"
cart.add_product(product_A, quantity_A, wrapped_A)

quantity_B = int(input("Enter quantity of product B: "))
wrapped_B = input("Is product B wrapped as a gift? (yes/no): ").lower() == "yes"
cart.add_product(product_B, quantity_B, wrapped_B)

quantity_C = int(input("Enter quantity of product C: "))
wrapped_C = input("Is product C wrapped as a gift? (yes/no): ").lower() == "yes"
cart.add_product(product_C, quantity_C, wrapped_C)
cart.display_cart()
