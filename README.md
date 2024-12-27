import java.util.*;

// Abstract class for general Item
abstract class Item {
    private String id;
    private String name;
    private double price;
    private int quantity;

    public Item(String id, String name, double price, int quantity) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public double getTotalValue() {
        return price * quantity;
    }

    public abstract void displayDetails();
}

// Class for Perishable Items
class PerishableItem extends Item {
    private String expirationDate;

    public PerishableItem(String id, String name, double price, int quantity, String expirationDate) {
        super(id, name, price, quantity);
        this.expirationDate = expirationDate;
    }

    public String getExpirationDate() {
        return expirationDate;
    }

    @Override
    public void displayDetails() {
        System.out.printf("ID: %s, Name: %s, Price: %.2f, Quantity: %d, Expiration Date: %s\n", 
                getId(), getName(), getPrice(), getQuantity(), expirationDate);
    }
}

// Class for Non-Perishable Items
class NonPerishableItem extends Item {
    private String category;

    public NonPerishableItem(String id, String name, double price, int quantity, String category) {
        super(id, name, price, quantity);
        this.category = category;
    }

    public String getCategory() {
        return category;
    }

    @Override
    public void displayDetails() {
        System.out.printf("ID: %s, Name: %s, Price: %.2f, Quantity: %d, Category: %s\n", 
                getId(), getName(), getPrice(), getQuantity(), category);
    }
}

// Inventory class to manage items
class Inventory {
    private List<Item> items;

    public Inventory() {
        items = new ArrayList<>();
    }

    public void addItem(Item item) {
        items.add(item);
    }

    public void removeItem(String id) {
        items.removeIf(item -> item.getId().equals(id));
    }

    public void displayAllItems() {
        for (Item item : items) {
            item.displayDetails();
        }
    }

    public double calculateTotalInventoryValue() {
        double totalValue = 0;
        for (Item item : items) {
            totalValue += item.getTotalValue();
        }
        return totalValue;
    }

    public Item searchItemById(String id) {
        for (Item item : items) {
            if (item.getId().equals(id)) {
                return item;
            }
        }
        return null;
    }
}

// Main application class
public class InventoryManagementSystem {
    public static void main(String[] args) {
        Inventory inventory = new Inventory();
        Scanner scanner = new Scanner(System.in);
        boolean running = true;

        while (running) {
            System.out.println("\nInventory Management System");
            System.out.println("1. Add Perishable Item");
            System.out.println("2. Add Non-Perishable Item");
            System.out.println("3. Remove Item");
            System.out.println("4. Display All Items");
            System.out.println("5. Search Item By ID");
            System.out.println("6. Calculate Total Inventory Value");
            System.out.println("7. Exit");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter ID: ");
                    String pId = scanner.nextLine();
                    System.out.print("Enter Name: ");
                    String pName = scanner.nextLine();
                    System.out.print("Enter Price: ");
                    double pPrice = scanner.nextDouble();
                    System.out.print("Enter Quantity: ");
                    int pQuantity = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter Expiration Date: ");
                    String expirationDate = scanner.nextLine();

                    PerishableItem perishableItem = new PerishableItem(pId, pName, pPrice, pQuantity, expirationDate);
                    inventory.addItem(perishableItem);
                    System.out.println("Perishable item added successfully.");
                    break;

                case 2:
                    System.out.print("Enter ID: ");
                    String npId = scanner.nextLine();
                    System.out.print("Enter Name: ");
                    String npName = scanner.nextLine();
                    System.out.print("Enter Price: ");
                    double npPrice = scanner.nextDouble();
                    System.out.print("Enter Quantity: ");
                    int npQuantity = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter Category: ");
                    String category = scanner.nextLine();

                    NonPerishableItem nonPerishableItem = new NonPerishableItem(npId, npName, npPrice, npQuantity, category);
                    inventory.addItem(nonPerishableItem);
                    System.out.println("Non-Perishable item added successfully.");
                    break;

                case 3:
                    System.out.print("Enter ID of item to remove: ");
                    String removeId = scanner.nextLine();
                    inventory.removeItem(removeId);
                    System.out.println("Item removed successfully.");
                    break;

                case 4:
                    inventory.displayAllItems();
                    break;

                case 5:
                    System.out.print("Enter ID to search: ");
                    String searchId = scanner.nextLine();
                    Item foundItem = inventory.searchItemById(searchId);
                    if (foundItem != null) {
                        foundItem.displayDetails();
                    } else {
                        System.out.println("Item not found.");
                    }
                    break;

                case 6:
                    double totalValue = inventory.calculateTotalInventoryValue();
                    System.out.printf("Total Inventory Value: %.2f\n", totalValue);
                    break;

                case 7:
                    running = false;
                    System.out.println("Exiting the system. Goodbye!");
                    break;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }

        scanner.close();
    }
}
