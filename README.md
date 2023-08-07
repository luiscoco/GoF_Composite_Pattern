# GoF_Composite_Pattern

Treats individual objects and compositions of objects uniformly, forming a tree-like structure.

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        // Create individual food items
        IMenuComponent steak = new MenuItem("Steak", 20.0);
        IMenuComponent salad = new MenuItem("Salad", 8.0);
        IMenuComponent dessert = new MenuItem("Dessert", 6.0);

        // Create a category and add individual items to it
        MenuCategory mainCourseCategory = new MenuCategory("Main Course");
        mainCourseCategory.AddItem(steak);
        mainCourseCategory.AddItem(salad);

        // Create another category and add an individual item to it
        MenuCategory dessertCategory = new MenuCategory("Desserts");
        dessertCategory.AddItem(dessert);

        // Create the root category and add subcategories to it
        MenuCategory menu = new MenuCategory("Menu");
        menu.AddItem(mainCourseCategory);
        menu.AddItem(dessertCategory);

        // Display the menu and calculate the total cost
        menu.Display(0);
        double totalCost = CalculateTotalCost(menu);
        Console.WriteLine("Total Cost: $" + totalCost);
    }

    static double CalculateTotalCost(IMenuComponent menuComponent)
    {
        double total = 0.0;

        if (menuComponent is MenuItem menuItem)
        {
            total += menuItem.Price;
        }
        else if (menuComponent is MenuCategory menuCategory)
        {
            foreach (var item in menuCategory.GetMenuItems())
            {
                total += CalculateTotalCost(item);
            }
        }

        return total;
    }
}

public interface IMenuComponent
{
    string Name { get; }
    double Price { get; }
    void Display(int depth);
}

public class MenuItem : IMenuComponent
{
    public string Name { get; private set; }
    public double Price { get; private set; }

    public MenuItem(string name, double price)
    {
        Name = name;
        Price = price;
    }

    public void Display(int depth)
    {
        Console.WriteLine(new string('-', depth) + " " + Name + " - $" + Price);
    }
}

public class MenuCategory : IMenuComponent
{
    private List<IMenuComponent> menuItems = new List<IMenuComponent>();

    public string Name { get; private set; }

    public double Price => throw new NotImplementedException();

    public MenuCategory(string name)
    {
        Name = name;
    }

    public void AddItem(IMenuComponent item)
    {
        menuItems.Add(item);
    }

    public void RemoveItem(IMenuComponent item)
    {
        menuItems.Remove(item);
    }

    public List<IMenuComponent> GetMenuItems()
    {
        return menuItems;
    }

    public void Display(int depth)
    {
        Console.WriteLine(new string('-', depth) + " " + Name);

        foreach (var item in menuItems)
        {
            item.Display(depth + 2);
        }
    }
}
```




