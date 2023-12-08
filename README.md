# Inventory-System
// Include Required Libraries 
#include <iostream>
#include <string.h>
#include <iomanip>

// Using Standard Namespace 
using namespace std ;

// Function Declarations
int searchProduct(string* products, double* prices, int* quantities, int &number_products) ;
void product_management(string* products, double* prices, int* quantities,  int &number_products) ;
void SalesReceipt(string* products, double* prices, int* quantities, int &number_products, int &total_items_sold, double &total_profit) ;
void basic_report(int total_items_sold, double total_profit) ;


// Main Program Function 
int main()
{
    // Declaring Relevant Variables for the Program 
    string login_id ;
    string password ;
    int choice ; 
    const int inventorySize = 150 ; 
    int number_of_products = 30 ; 
    int total_items_sold = 0 ;
    double total_profit = 0.0 ;
    bool main_menu = true ;

    // Products Array
    string products[inventorySize] = 
    {   "apple", "banana", "cherries", "bread", "eggs",
        "cheese", "sanitizer", "orange juice", "pasta", "rice",
        "yogurt", "tissues", "detergent", "mug", "milk",
        "hairbrush", "shampoo", "water", "pepsi", "milo",
        "notepad", "butter", "icecream", "pencils", "teabags",
        "coffee", "chips", "salt", "pen", "cookies"
    } ;
    // Pointer to Product Array
    string* ptr_products = products ;

    // Prices Array
    double prices[inventorySize] = 
    {
        2.50, 1.20, 3.00, 2.75, 2.00,
        5.50, 1.80, 6.25, 1.90, 2.10,
        7.00, 8.50, 0.75, 0.60, 0.50,
        0.80, 10.0, 1.20, 1.50, 2.25,
        4.75, 3.25, 4.00, 5.50, 3.75,
        6.00, 2.25, 1.00, 1.10, 7.50
    } ;
    // Pointer to  Price Array 
    double* ptr_prices = prices ;

    // Quantities Array
    int quantities[inventorySize] = 
    {
        34, 70, 40, 60, 60,
        20, 30, 45, 55, 65,
        15, 60, 90, 111, 75,
        85, 67, 58, 130, 110,
        25, 35, 50, 40, 75,
        75, 65, 70, 130, 25
    } ;
    // Pointer to Quantity Array 
    int* ptr_quantities = quantities ;

    // Start of Program

    cout << endl << "Please find below the login details to access the system. For security reasons it is recommended that you change your password and ID later onwards." << endl ;
    cout << endl << "Login ID: employee1" << endl << "Pin: 1580" << endl << endl ;

    cout << "Please enter your Login ID: " ;
    cin >> login_id ;

    cout << "Please enter your password: " ;
    cin >> password ;


    // While loop to make sure user has entered the right login details.

    while((login_id != "employee1") || (password != "1580"))
    {
        cout << endl << "Wrong Login Details! Please try again." << endl << endl ;

        cout << "Please enter your Login ID: " ;
        cin >> login_id ;

        cout << "Please enter your password: " ;
        cin >> password ;
    }
    cout << endl << endl << "WELCOME TO LUMS SUPERSTORE INVENTORY SYSTEM!" << endl << endl ;

    while (main_menu)
    {
        cout << "Main Menu"<< endl ;
        cout << "1. Product Management" << endl ;
        cout << "2. Sales Receipt" << endl ;
        cout << "3. Search Products" << endl ;
        cout << "4. Basic Reporting" << endl ;
        cout << "5. Exit Program" << endl ;
        cout << "Please Select the preffered task: " ;
        cin >> choice ;
        
        while((choice < 1) || (choice > 5))
        {
            cout << "Incorrect choice please select again." << endl ;
            cout << "Please Select the preffered task: " ;
            cin >> choice ;
        }
        
        if(choice == 1)
        {
            product_management(ptr_products, ptr_prices, ptr_quantities, number_of_products) ;
        }
        else if(choice == 2)
        {
            SalesReceipt(ptr_products, ptr_prices, ptr_quantities, number_of_products, total_items_sold, total_profit);
        }
        else if(choice == 3)
        {    
            int index = searchProduct(ptr_products, ptr_prices, ptr_quantities, number_of_products) ;
        }
        else if(choice == 4)
        {
            basic_report(total_items_sold, total_profit);
        }
        else if(choice == 5)
        {
            main_menu = false ;
            cout << endl << "Program ended. Goodbye!" << endl << endl << endl ;
        } 
    }

    return 0 ; 
}
    

// Product Management Function
void product_management(string* products, double* prices, int* quantities,  int &number_products)
{
    string answer ; 

    cout << endl << "Would you like to add a product to the system, edit an existing product, or delete a product from the system?" << endl ;
    cout << "Please enter a for add, e for edit, and d for delete: " ;

    cin >> answer ; 

    if (answer == "a") 
    {
        cout << endl << "Please enter the name of the product you would like to add: " ;
        cin >> *(products + number_products) ;

        cout << "Please enter the price of this product: " ;
        cin >> *(prices + number_products) ;

        cout << "Please enter the quantity of this product: " ;
        cin >> *(quantities + number_products) ;

        cout << endl << *(products + number_products) << " has been successfully added to the inventory." << endl << endl ;
        number_products++ ;
    }
    else if (answer == "e")
    {
        int index = searchProduct(products, prices, quantities, number_products) ;
        
        if (index >= 0)
        {
            cout << "Please enter the new name for this product: " ;
            cin >> *(products + index) ;

            cout << "Please enter the new price for this product: " ;
            cin >> *(prices + index) ;

            cout << "Please enter the new quantity for this product: " ;
            cin >> *(quantities + index) ;

            cout << endl << "Product information was successfully updated." << endl << endl ;
        }
    }
    else if (answer == "d")
    {
        int index = searchProduct(products, prices, quantities, number_products) ;
        if (index >= 0)
        {
            char reply ;

            cout << "Are you sure you want to delete this product from the inventory? All data related to it will be deleted. " << endl ;
            cout << "Please enter y for yes and n for no: " ;
            cin >> reply ;
            
            if (reply == 'y')
            {
                *(products + index) = *(products + number_products - 1) ;
                *(prices + index) = *(prices + number_products - 1) ;
                *(quantities + index) = *(quantities + number_products - 1) ;

                cout << endl << "Product was successfully removed from the inventory." << endl << endl ;
            }
            else if (reply == 'n')
            {
                cout << endl ;
            }
            else if (reply != 'n')
            {
                cout << "Invalid Reply!" << endl ;
                return ;
            }
        }

    }
    else
    {
        cout << "Invalid Choice!" << endl ;
    }
}

// Sales Reciept Function
void SalesReceipt(string* products, double* prices, int* quantities, int &number_products, int &total_items_sold, double &total_profit)
{
    // Array To Store Products In The Cart
    const int total_products = 150 ;
    string cart[total_products][3] ; 
    double totalCost = 0 ;

    while(true)
    {
        string choice ;
        int quantity ;
        double quantity_price ;

        cout << "\nProducts Available:\n";

        for (int i = 0; i < number_products; i++) 
        {
            cout << *(products + i) << " - $" << fixed << setprecision(2) << *(prices + i) << " (Quantity: " << *(quantities + i) << ")" << endl ;
        }

        cout << endl << "Enter product name to add to cart (or 'exit' to end): " ;
        cin >> choice ;

        if (choice == "exit") 
        {
            break ;
        }

        int index = -1 ;
        for (int i = 0; i < number_products; i++) 
        {
            if (*(products + i) == choice) 
            {
                index = i ;
                break ;
            }
        }

        if (index != -1) 
        {
            cout << "Please enter the quantity of this item: " ;
            cin >> quantity ;

            while (!(quantity > 0 && quantity <= *(quantities + index)))
            {
                cout << "Incorrect Quantity! Please refer to the quantity of items." << endl ;
                cout << "Please enter the quantity of this item again: " ;
                cin >> quantity ;
            }

            total_items_sold += quantity ;
            quantity_price = *(prices + index) * quantity ; 
            total_profit += quantity_price ;

            cart[index][0] = choice ;
            cart[index][1] = to_string(quantity) ;
            cart[index][2] = to_string(quantity_price) ;

            totalCost += *(prices + index) * quantity ;
            *(quantities + index) -= quantity ; 

            cout << choice << " added to cart.\n" ;
        }
        else
        {
            cout << "Product not found!\n" ;
        }
    }

    cout << "\nSales Receipt:\n\n" ;
    cout << setw(15) << "ITEMS" << setw(15) << "QUANTITY" << setw(15) << "PRICE" << endl << endl ; 

    for (int i = 0; i < total_products; i++)
    {
        if (!cart[i][0].empty()) 
        {
            cout << setw(15) << cart[i][0] << setw(15) << cart[i][1] << "\t\t$" << cart[i][2] << fixed << setprecision(2) << endl ;
        }
    }

    cout << "\nTotal Cost: $" << totalCost << endl << endl ;
}

// Product Search Function
int searchProduct(string* products, double* prices, int* quantities, int &number_products)
{
    string product ;

    cout << endl << "Enter the product to search for: " ;
    cin.ignore(); //ignores spaces and new lines
    getline(cin, product);

    for (int i = 0; i < number_products; i++)
    {
        if (*(products + i) == product) 
        {
            cout << endl << "Your product was successfully found!" << endl ;
            cout << "NAME: " << *(products + i) << "  Price: " << *(prices + i) << "  Quantity: " << *(quantities + i) << endl << endl ;
            int index = i ;
            return index;
        }
    }

    cout << endl << "Product not found." << endl << endl ;
    return -1 ;
}

// Basic Reporting Function
void basic_report(int total_items_sold, double total_profit)
{
    cout << endl << "The total items sold today were " << total_items_sold << " and the total profit was " << total_profit << endl << endl ;
}
