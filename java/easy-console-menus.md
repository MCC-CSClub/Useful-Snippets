## Description
This class is a simple menu system which allows you to create menus in an easy way. It features an incredibly simple set of methods, and is useful for quick menus with minimal effort. Example usage is as follows:

```java
Menu menu = new Menu();
menu.setTitle("Please select an option: ");
menu.addItem("Option 1");
menu.addItem("Option 2");
menu.addItem("Option 3");
menu.addItem("Exit");

int choice;
do
{
	Menu.printMenu(menu, Menu.Style.NUMBERED);
	choice = menu.getChoiceFromUser(keyboard);
	
	if (choice == 0) // Option 1
	{
		// do stuff
	}
	else if (choice == 1) // Option 2
	{
		// do stuff
	}
	else if (choice == 2) // Option 3
	{
		// do stuff
	}
} while(choice != 3);

// they exited the menu, ether clean up or do other things.
```

## Code
```java
/**
 * Menu.java : Create beautiful menus with non-specialized code
 * 
 * @author Michael W Flaherty
 * @version 1.0
 */

package edu.miracosta.cs113;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Scanner;

public class Menu
{
	
	/**
	 * Styles the menu can be drawn with
	 */
	public enum Style
	{
		/**
		 * Numbered. 1), 2), ...
		 */
		NUMBERED, 
		/**
		 * Alphabetical. a), b), ...
		 */
		ALPHABETICAL, 
		
		/**
		 * Blank. No style.
		 */
		BLANK
	}
	
	/* Instance Variables */
	private String title;
	private ArrayList<String> items;
	
	/* Getters & Setters */
	
	/**
	 * Gets the menu title
	 * 
	 * @return title
	 */
	public String getTitle()
	{
		return this.title;
	}
	
	/**
	 * Sets the menu title
	 * 
	 * @param title text used
	 */
	public void setTitle(String title)
	{
		this.title = title;
	}
	
	/**
	 * Adds an item to the bottom of the menu
	 * 
	 * @param item text used to indicate that item
	 * @return index in which the item was added
	 */
	public int addItem(String item)
	{
		items.add(item);
		return items.size() - 1;
	}
	
	/**
	 * Removes an item from the menu
	 * 
	 * @param index index to remove
	 */
	public void removeItem(int index)
	{
		items.remove(index);
	}
	
	/**
	 * Draws the menu using the given style
	 * 
	 * @param style theme used to draw the menu
	 * @return String[] with each line of the menu
	 */
	public String[] drawMenu(Style style)
	{
		String[] arr = new String[items.size() + 1];
		
		arr[0] = this.getTitle();
		
		int prefix = 0;
		for (int i = 0; i < items.size(); i++)
		{
			if (style == Style.NUMBERED)
			{
				prefix = i+1;
			}
			else if (style == Style.ALPHABETICAL)
			{
				prefix = 'a' + i;
			}
			
			if (style == Style.ALPHABETICAL)
			{
				arr[i+1] = ((char)prefix) + ") " + items.get(i); 
			}
			else if (style == Style.NUMBERED)
			{
				arr[i+1] = (prefix) + ") " + items.get(i); 
			}
			else
			{
				arr[i] = items.get(i); 
			}
		}
		
		return arr;
	}
	
	/**
	 * Asks the user for the menu selection and returns index chosen
	 * 
	 * @param keyboard initialized Scanner object
	 * @return index chosen by user
	 */
	public int getChoiceFromUser(Scanner keyboard)
	{
		String input;
		int intInput = 0;
		char charInput = 0;
		
		boolean isInt;
		
		System.out.print("Please enter your selection: ");
		input = keyboard.nextLine();
	
		try
		{
			intInput = Integer.parseInt(input);
			isInt = true;
		}
		catch (Exception e)
		{
			charInput = input.charAt(0);
			isInt = false;
		}
		
		if (isInt)
		{
			return intInput - 1;
		}
		else
		{
			return (int)charInput - 'a';
		}
	}
	
	/* Constructors */
	/**
	 * Default constructor for a menu.
	 */
	public Menu()
	{
		items = new ArrayList<String>();
	}
	
	/**
	 * Constructor which loads menu items from an arary.
	 * 
	 * @param array String[] array
	 */
	public Menu(String[] array)
	{
		items = new ArrayList<String>();

		for (String item : array)
		{
			items.add(item);
		}
	}
	
	/* Helper Methods */
	
	/**
	 * Outputs a menu with a given style
	 * 
	 * @param menu Menu object
	 * @param style Style to be drawn
	 */
	public static void printMenu(Menu menu, Style style)
	{
		String[] output = menu.drawMenu(style);
		for (String line : output)
		{
			System.out.println(line);
		}
	}

	/* Required methods */
	/**
	 * Returns a string representation of the object
	 * 
	 * @return String representation
	 */
	public String toString()
	{
		String output = "";
		for (String item : items)
		{
			output += item + "\n";
		}
		return output;
	}
	
	/**
	 * Checks for complete equality between objects.
	 * 
	 * @return true if equal
	 */
	public boolean equals(Object obj)
	{
		if (obj == null || obj.getClass() != this.getClass())
		{
			return false;
		}
		else
		{
			Menu menu = (Menu)obj;
			
			/* Menus will be equal if they're output is equal */
			return Arrays.equals(menu.drawMenu(Style.ALPHABETICAL), this.drawMenu(Style.ALPHABETICAL));
		}
	}
}
```