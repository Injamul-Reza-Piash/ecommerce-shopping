[ecomSiteShopping.cpp](https://github.com/user-attachments/files/34099775/ecomSiteShopping.cpp)
#include <iostream>
#include <string>
#include <limits>
using namespace std;

// MyProfile stores the personal details of a customer.
// Encapsulation: private data is accessed through public methods.
class MyProfile {
private:
    string name, email, mobile;
    int age;

public:
    void registerCustomer() {
        cout << "\n=====*** Customer Registration ***=====\n";

        cout << "Enter your name:";
        getline(cin, name);

        cout << "Enter your email:";
        getline(cin, email);

        cout << "Enter your mobile number:";
        getline(cin, mobile);

        cout << "Enter your age: ";
        if(!(cin >> age)){
            throw "Invalid age. Please enter a number.";
        }
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        // Exception handling: minimum eligible age is 16.
        if(age < 16){
            throw "You must be at least 16 years old to shop online.";
        }

        cout << "\nRegistration successful!\n";
        }


    void displayProfile() const {
        cout << "\nCustomer Name : " << name
             << "\nEmail         : " << email
             << "\nMobile Number : " << mobile
             << "\nAge           : " << age << '\n';
             }
};


// Abstraction: abstract base class for e-commerce websites.
class Ecommerce {
protected:
    string siteName;

public:
    Ecommerce(string site) {
        siteName = site;
    }

    // Pure virtual functions provide a common interface.
    virtual void calculatePrice() = 0;
    virtual void displayDetails() const = 0;

    virtual ~Ecommerce() {}
};

// Inheritance: Bagdoom inherits from Ecommerce.
class Bagdoom : public Ecommerce {
private:
    int shariQuantity;
    int panjabiQuantity;
    double total;

public:
    Bagdoom() : Ecommerce("bagdoom.com") {
        shariQuantity = 2;
        panjabiQuantity = 3;
        total = 0;
        }


    void calculatePrice() override {
        total = (3000 * shariQuantity) +(2000 * panjabiQuantity);

        }


    void displayDetails() const override {
        cout << "\n===== " << siteName << " =====\n";

        cout << "\nShari Details"
             << "\nColor: Black"
             << "\nPrice per piece: Tk. 3000"
             << "\nQuantity: " << shariQuantity
             << "\nSubtotal: Tk. "
             << 3000 * shariQuantity << '\n';

        cout << "\nPanjabi Details"
             << "\nColor: White"
             << "\nSize: L"
             << "\nPrice per piece: Tk. 2000"
             << "\nQuantity: " << panjabiQuantity
             << "\nSubtotal: Tk. "
             << 2000 * panjabiQuantity << '\n';

        cout << "\nTotal Payable Price: Tk. "
             << total << '\n';

             }
};


//------------------------------------------------------------------
// Inheritance: Daraz inherits from Ecommerce.
class Daraz : public Ecommerce {
private:
    double price;
    double vat;
    double total;

public:
    Daraz() : Ecommerce("daraz.com") {
        price = 22000;
        vat = 0.05;
        total = 0;
        }

    void calculatePrice() override {
        total = price + (price * vat);
        }

    void displayDetails() const override {
        cout << "\n===== " << siteName << " =====\n";

        cout << "\nProduct: Mobile"
             << "\nBrand: Samsung"
             << "\nModel: S125"
             << "\nQuantity: 1"
             << "\nPrice: Tk. " << price
             << "\nVAT (5%): Tk. " << price * vat
             << "\nTotal Payable Price: Tk. "
             << total << '\n';
             }
};

// Inheritance: Bikroy inherits from Ecommerce.
class Bikroy : public Ecommerce {
private:
    double price;
    double vat;
    double total;

public:
    Bikroy() : Ecommerce("bikroy.com") {
        price = 67000;
        vat = 0.07;
        total = 0;
        }


    void calculatePrice() override {
        total = price + (price * vat);
        }

    void displayDetails() const override {
        cout << "\n===== " << siteName << " =====\n";

        cout << "\nProduct: TV"
             << "\nBrand: Sony"
             << "\nType: Smart QLED"
             << "\nSize: 43 inches"
             << "\nQuantity: 1"
             << "\nPrice: Tk. " << price
             << "\nVAT (7%): Tk. " << price * vat
             << "\nTotal Payable Price: Tk. "
             << total << '\n';
    }
};

int main() {
    try {
        MyProfile customer;
        customer.registerCustomer();
        customer.displayProfile();

        cout << "\n===== Eid-ul-Fitr Online Shopping =====\n";
        cout << "1. Bagdoom.com\n";
        cout << "2. Daraz.com\n";
        cout << "3. Bikroy.com\n";
        cout << "Choose a website (1-3): ";

        int choice;
        if (!(cin >> choice)) {
            throw "Invalid choice. Enter a number from 1 to 3.";
        }

        // Polymorphism: a base-class pointer selects
        // an object of the appropriate derived class.
        Ecommerce* shop = nullptr;

        if (choice == 1) {
            shop = new Bagdoom();
        }
        else if (choice == 2) {
            shop = new Daraz();
        }
        else if (choice == 3) {
            shop = new Bikroy();
        }
        else {
            throw "Invalid selection. Please choose 1, 2, or 3.";
        }

        shop->calculatePrice();
        shop->displayDetails();

        delete shop;

        cout << "\nThank you for shopping.\n";
    }
    catch (const char* errorMessage) {
        // Exception handling: display errors instead of crashing.
        cout << "\nError: " << errorMessage << '\n';
    }

    return 0;
}


