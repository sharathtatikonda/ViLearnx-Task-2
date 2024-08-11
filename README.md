# ViLearnx-Task-2
    import json

    class InventoryManager:
    def __init__(self):
        self.inventory = {}
        self.load_inventory()

    def load_inventory(self):
        try:
            with open('inventory.json', 'r') as file:
                self.inventory = json.load(file)
        except FileNotFoundError:
            self.inventory = {}

    def save_inventory(self):
        with open('inventory.json', 'w') as file:
            json.dump(self.inventory, file, indent=4)

    def add_product(self, product_id, name, quantity, price):
        if product_id in self.inventory:
            print(f"Product with ID {product_id} already exists.")
        else:
            self.inventory[product_id] = {
                'name': name,
                'quantity': quantity,
                'price': price
            }
            self.save_inventory()
            print(f"Product {name} added successfully.")

    def edit_product(self, product_id, name=None, quantity=None, price=None):
        if product_id not in self.inventory:
            print(f"Product with ID {product_id} not found.")
        else:
            if name is not None:
                self.inventory[product_id]['name'] = name
            if quantity is not None:
                self.inventory[product_id]['quantity'] = quantity
            if price is not None:
                self.inventory[product_id]['price'] = price
            self.save_inventory()
            print(f"Product {product_id} updated successfully.")

    def delete_product(self, product_id):
        if product_id in self.inventory:
            del self.inventory[product_id]
            self.save_inventory()
            print(f"Product {product_id} deleted successfully.")
        else:
            print(f"Product with ID {product_id} not found.")

    def track_inventory_levels(self):
        print("Inventory Levels:")
        for product_id, details in self.inventory.items():
            print(f"ID: {product_id}, Name: {details['name']}, Quantity: {details['quantity']}, Price: ${details['price']:.2f}")

    def low_stock_alert(self, threshold):
        print(f"Low Stock Alerts (Threshold: {threshold}):")
        for product_id, details in self.inventory.items():
            if details['quantity'] < threshold:
                print(f"ID: {product_id}, Name: {details['name']}, Quantity: {details['quantity']}")

    def sales_summary(self):
        total_sales = sum(details['quantity'] * details['price'] for details in self.inventory.values())
        print(f"Total Sales Value: ${total_sales:.2f}")

    def main():
    manager = InventoryManager()

    while True:
        print("\nInventory Manager")
        print("1. Add Product")
        print("2. Edit Product")
        print("3. Delete Product")
        print("4. Track Inventory Levels")
        print("5. Low Stock Alerts")
        print("6. Sales Summary")
        print("7. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            product_id = input("Enter Product ID: ")
            name = input("Enter Product Name: ")
            quantity = int(input("Enter Product Quantity: "))
            price = float(input("Enter Product Price: "))
            manager.add_product(product_id, name, quantity, price)
        elif choice == '2':
            product_id = input("Enter Product ID to Edit: ")
            name = input("Enter New Product Name (leave blank to keep current): ")
            quantity = input("Enter New Product Quantity (leave blank to keep current): ")
            price = input("Enter New Product Price (leave blank to keep current): ")
            quantity = int(quantity) if quantity else None
            price = float(price) if price else None
            manager.edit_product(product_id, name if name else None, quantity, price)
        elif choice == '3':
            product_id = input("Enter Product ID to Delete: ")
            manager.delete_product(product_id)
        elif choice == '4':
            manager.track_inventory_levels()
        elif choice == '5':
            threshold = int(input("Enter Low Stock Threshold: "))
            manager.low_stock_alert(threshold)
        elif choice == '6':
            manager.sales_summary()
        elif choice == '7':
            break
        else:
            print("Invalid choice. Please try again.")

    if __name__ == "__main__":
      main()
