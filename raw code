#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#define RESET "\x1b[0m" // ANSI escape codes for formatting
#define RED "\x1b[31m"
#define GREEN "\x1b[32m"
#define CYAN "\x1b[36m"
#define YELLOW "\x1b[33m"
#define BLUE "\x1b[34m"
#define MAGENTA "\x1b[35m"
#define BLINK "\x1b[5m"
#define BLACK "\x1b[30m"
#ifdef _WIN32
#define clrscr system("cls");
#else
#define clrscr system("clear");
#endif
void clrbfr();
int item_count();
void FREE();
void update_file();
void file_to_menu();
void initial_menu();
void display_menu();
void add_item();
void remove_item();
void price_update();
void take_order();
void view_order();
void total_bill();
void intro();
void admin_panel();
void change_pass();
void create_menu();
void load_admin_pass();
void admin_login();
void change_pass();
void general();
int main();

FILE *file;
FILE *pass;
char bfrpass[100];

struct item
{
    char name[50];
    float price;
    struct item *next;
} *head = NULL, *tail = NULL;
typedef struct item item;

struct order
{
    char name[50];
    int amount;
    float price;
    struct order *next;
} *ordrhd = NULL, *ordrtl = NULL;
typedef struct order order;

void clrbfr() // clears the buffers
{
    while (getchar() != '\n')
        ;
}

int item_count() // needed by take_order & update_file
{
    int cnt = 0;
    item *temp = head;
    while (temp != NULL)
    {
        cnt++;
        temp = temp->next;
    }
    return cnt;
}

void FREE()
{
    item *tempit = head;
    while (tempit != NULL)
    {
        head = head->next;
        free(tempit);
        tempit = head;
    }
    order *tempor = ordrhd;
    while (tempor != NULL)
    {
        ordrhd = ordrhd->next;
        free(tempor);
        tempor = ordrhd;
    }
}

void update_file()
{
    file = fopen("amader_project.txt", "w");
    int loop = item_count();
    item *temp = head;
    while (loop--)
    {
        fputs(temp->name, file);
        fputc(',', file);
        fprintf(file, "%.2f", temp->price);
        fputc('\n', file);
        temp = temp->next;
    }
    fclose(file);
}

void file_to_menu() // get called from second compilation
{
    file = fopen("amader_project.txt", "r");
    char buffer[100];
    int count = 0;
    while (fgets(buffer, sizeof(buffer), file))
    {
        count++;
    }
    rewind(file);
    while (count--)
    {
        item *temp = malloc(sizeof(item));

        fscanf(file, " %[^,]", temp->name);
        fseek(file, 1, SEEK_CUR);
        fscanf(file, " %f", &temp->price);
        fseek(file, 1, SEEK_CUR);
        temp->next = NULL;
        if (head == NULL)
        {

            head = temp;
            tail = head;
        }
        else
        {
            tail->next = temp;
            tail = temp;
        }
    }
    fclose(file);
}

void initial_menu() // prepopulating the inventory
{
    item *temp = NULL;
    for (int i = 0; i < 10; i++)
    {
        if (head == NULL)
        {
            head = malloc(sizeof(item));
            head->next = NULL;
            tail = head;
        }
        else
        {
            temp = malloc(sizeof(item));
            temp->next = NULL;
            tail->next = temp;
            tail = temp;
        }
    }
    temp = head;
    strcpy(temp->name, "Espresso");
    temp->price = 2.5;
    temp = temp->next;

    strcpy(temp->name, "Cappuccino");
    temp->price = 3.0;
    temp = temp->next;

    strcpy(temp->name, "Latte");
    temp->price = 3.25;
    temp = temp->next;

    strcpy(temp->name, "Mocha");
    temp->price = 3.5;
    temp = temp->next;

    strcpy(temp->name, "Macchiato");
    temp->price = 4.5;
    temp = temp->next;

    strcpy(temp->name, "Chicken Wings");
    temp->price = 8.99;
    temp = temp->next;

    strcpy(temp->name, "Spring Rolls");
    temp->price = 9.99;
    temp = temp->next;

    strcpy(temp->name, "Loaded Nachos");
    temp->price = 11.99;
    temp = temp->next;

    strcpy(temp->name, "Mozzarella Sticks");
    temp->price = 7.99;
    temp->next = NULL;
    tail = temp;
}

void display_menu()
{
    clrscr
    intro();
    printf(CYAN "---Available Dishes---\n");
    item *temp = head;
    int i = 0;
    while (temp != NULL)
    {
        i++;
        printf("%d. %s - $%.2f\n", i, temp->name, temp->price);
        temp = temp->next;
    }
    printf(RED "\nPress enter to  continue - ");
    clrbfr();
    printf("\n" RESET);
}

void add_item()
{
    clrscr
    intro();
    item *temp = malloc(sizeof(item));
    tail->next = temp;
    tail = temp;
    tail->next = NULL;
    printf(BLUE "Name of Item - ");
    scanf(" %[^\n]", tail->name);
    printf("Price of Item - ");
    while (1)
    {
        if (scanf(" %f", &temp->price) != 1)
        {
            printf("Invalid inputs. Enter with digits\n");
            clrbfr();
            continue;
        }
        else
        {
            if (temp->price <= 0)
                printf("Enter a proper value\n");
            else
                break;
        }
    }
    clrbfr();
    update_file();
    printf("---Item added---\n");
    printf(RED "\nPress enter to continue - ");
    clrbfr();
    printf("\n" RESET);
}

void remove_item()
{
    clrscr
    intro();
    int flag = 0;
    printf(BLUE "Removed item name (Mind the Caps) - ");
    char name[50];
    scanf(" %[^\n]", name);
    clrbfr();
    item *temp = head, *prev = NULL;
    while (temp != NULL)
    {
        int check;
        check = strcmp(name, temp->name); // returns 0 if equal
        if (check == 0)
        {
            if (temp == head)
            {
                head = head->next;
                free(temp);
                flag = 1;
                break;
            }
            else if (temp->next == NULL)
            {
                prev->next = NULL;
                tail = prev;
                free(temp);
                flag = 1;
                break;
            }
            else
            {
                prev->next = temp->next;
                free(temp);
                flag = 1;
                break;
            }
        }
        prev = temp;
        temp = temp->next;
    }
    if (flag)
        printf("---Item removed---\n");
    else
        printf(RED "Item not found!!!Operation failed!\n");

    update_file();
    printf(RED "\nPress enter to continue - ");
    clrbfr();
    printf("\n" RESET);
}

void price_update()
{
    clrscr
    intro();
    int flag = 0;
    printf(BLUE "Updated item name(Mind the Caps) - ");
    char name[50];
    scanf(" %[^\n]", name);
    clrbfr();
    item *temp = head;
    while (temp != NULL)
    {
        int check;
        check = strcmp(name, temp->name); // returns 0 if equal
        if (check == 0)
        {
            printf("Updated price - ");
            while (1)
            {
                if (scanf("%f", &temp->price) != 1)
                {
                    printf("Invalid inputs. Enter valid digits.\n");
                    clrbfr();
                }
                else if (temp->price <= 0)
                {
                    printf("Enter a proper value\n");
                    clrbfr();
                }
                else
                {
                    clrbfr();
                    break;
                }
            }
            flag = 1;
            break;
        }
        temp = temp->next;
    }
    if (flag)
        printf("---Price updated---\n");
    else
        printf(RED "\nItem not found!!!Operation Failed!\n");

    update_file();
    printf(RED "\nPress enter to continue - ");
    clrbfr();
    printf("\n" RESET);
}

void take_order()
{
    clrscr
    intro();
    order *tempor = NULL;
    item *tempit = head;

    // clearing previous order
    if (ordrhd != NULL)
    {
        tempor = ordrhd;
        while (ordrhd != NULL)
        {
            ordrhd = ordrhd->next;
            free(tempor);
            tempor = ordrhd;
        }
    }

    int i, j = item_count();
    while (1)
    {
        printf(MAGENTA "Enter menu item ID (0 to exit/ 111 for menu): ");
        scanf(" %d", &i);
        clrbfr();
        if (i >= 0 && i <= j || i == 111)
        {
            if (i == 0)
                break;
            else if (i == 111)
                display_menu();
            else
            {
                if (ordrhd == NULL)
                {
                    ordrhd = malloc(sizeof(order));
                    ordrtl = ordrhd;
                    tempor = ordrhd;
                    i--;
                    while (i--)
                    {
                        tempit = tempit->next;
                    }
                    strcpy(tempor->name, tempit->name);
                    tempor->price = tempit->price;
                    tempor->next = NULL;
                    printf("Enter quantity - ");
                    while (1)
                    {
                        scanf(" %d", &tempor->amount);
                        clrbfr();
                        if (tempor->amount > 0 && tempor->amount <= 10)
                        {
                            break;
                        }
                        else
                        {
                            printf("Max 10 order at a time. Try again - ");
                        }
                    }
                    tempit = head;
                }
                else
                {
                    i--;
                    while (i--)
                    {
                        tempit = tempit->next;
                    }
                    tempor = malloc(sizeof(order));
                    strcpy(tempor->name, tempit->name);
                    tempor->price = tempit->price;
                    tempor->next = NULL;
                    ordrtl->next = tempor;
                    ordrtl = tempor;

                    printf("Enter quantity - ");
                    while (1)
                    {
                        scanf(" %d", &tempor->amount);
                        if (tempor->amount > 0 && tempor->amount <= 10)
                        {
                            clrbfr();
                            break;
                        }
                        else
                        {
                            clrbfr();
                            printf("Max 10 order at a time. Try again - ");
                        }
                    }
                    tempit = head;
                }
            }
        }
        else
        {
            int count = item_count();
            printf("Invalid input. Press (1 - %d)\n", count);
        }
    }
    printf("---Order Taken---\n");
    printf(RED "\nPress enter to continue - ");
    clrbfr();
    printf("\n" RESET);
}

void view_order()
{
    clrscr
    intro();
    printf(MAGENTA "---Current Order---\n");
    int i = 0;
    order *temp = ordrhd;
    while (temp != NULL)
    {
        i++;
        printf("%d. %s x%d\n", i, temp->name, temp->amount);
        temp = temp->next;
    }
    printf("\n" RESET);
}

void total_bill()
{
    float bill = 0;
    order *temp = ordrhd;
    while (temp != NULL)
    {
        bill += temp->price * temp->amount;
        temp = temp->next;
    }
    printf(BLUE "---Total Bill - $%.2f\n", bill);
    printf("___A percentage of this amount will be donated in humanitarian cause for the people of Gazza___\n");
    printf("Donated amount = $%.3f\n", bill * .005);
    printf(RED "\nPress enter to continue - ");
    clrbfr();

    printf("\n");
}

void intro()
{
    printf(GREEN "\t\t|----------------------------------------|\n");
    printf("\t\t|          Cafe Management System        |\n");
    printf("\t\t|----------------------------------------|\n");
    printf("\t\t|                ~ WELCOME               |\n");
    printf("\t\t|----------------------------------------|\n");
    printf("\t\t|     Created by      " RED" R00T SHAHED " GREEN "             |\n");
    printf("\t\t|----------------------------------------|\n\n\n" RESET);
}

void create_menu()
{
    if (access("amader_project.txt", F_OK)) // returns 0 if exists, -1 if dont
    {
        initial_menu(); // first compilation
    }
    else
    {
        file_to_menu();
    }
}

void load_admin_pass()
{
    if (access("admin_pass.txt", F_OK) == 0)
        pass = fopen("admin_pass.txt", "r");
    else
    {
        pass = fopen("admin_pass.txt", "w+");
        fprintf(pass, "Error404");
        rewind(pass);
    }

    char c;
    int i = 0;
    while ((c = fgetc(pass)) != EOF)
    {
        bfrpass[i] = c;
        i++;
    }
    bfrpass[i] = '\0';
    fclose(pass);
}

void admin_login()
{
    clrscr
    intro();
    load_admin_pass(); // loaded in bfrpass
    char password[100];
    printf(MAGENTA "Enter password - ");
    int flag = 0;
    while (1)
    {
        scanf(" %[^\n]", password);
        getchar();
        if (flag)
        {
            int chek1 = 0, chek2 = 0;
            chek1 = strcmp(password, "x");
            chek2 = strcmp(password, "X");
            if (chek1 == 0 || chek2 == 0)
            {
                printf("Cancelled!\n" RESET);
                printf(RED "\nPress enter to continue - " RESET);
                clrbfr();
                break;
            }
        }
        flag++;
        int comp;
        comp = strcmp(password, bfrpass);
        if (comp == 0)
        {
            printf("---Login Successfull!---\n" RESET);
            printf(RED "\nPress enter to continue - " RESET);
            clrbfr();
            admin_panel();
            break;
        }
        else
        {
            printf("Incorrect Password!\n");
            printf("Try again(or enter 'x' to cancel) - ");
        }
    }

    printf(RESET "\n");
}

void admin_panel()
{
    while (1)
    {
        clrscr
        intro();
        printf(CYAN "-------ADMIN-------\n");
        printf(RED "1. Update menu\n");
        printf("2. Update password\n");
        printf("3. Logout & return\n");
        printf("4. Exit the program\n");
        printf("Pick your choice - ");
        int ch;
        while (1)
        {
            if (scanf(" %d", &ch) != 1)
            {
                printf("Invalid choice. Try again - ");
                clrbfr();
                continue;
            }
            else
            {
                if (ch < 0 || ch > 4)
                {
                    printf("Invalid choice. Try again - ");
                    continue;
                }
                else
                    break;
            }
        }
        switch (ch)
        {
        case 1:
            clrscr
            intro();
            printf(YELLOW "1. Update Price\n");
            printf("2. Add an Item\n");
            printf("3. Remove an Item\n");
            printf("4. View menu\n");
            printf("5. Cancel\n");
            printf("Enter your choice - ");
            int choice;
            while (1)
            {
                if (scanf(" %d", &choice) != 1)
                {
                    printf("Please Enter a Valid Digit ");
                    clrbfr();
                    continue;
                }
                else
                {
                    if (choice < 0 || choice > 5)
                    {
                        printf("Invalid Choice. Try again - ");
                        clrbfr();
                        continue;
                    }
                    else
                    {
                        clrbfr();
                        if (choice == 5)
                        {
                            printf("---Cancelled---\n");
                            printf(RED "\nPress enter to continue - ");
                            clrbfr();
                            printf("\n" RESET);
                            break;
                        }
                        switch (choice)
                        {
                        case 1:
                            price_update();
                            break;
                        case 2:
                            add_item();
                            break;
                        case 3:
                            remove_item();
                            break;
                        case 4:
                            display_menu();
                            break;
                        }
                        break;
                    }
                }
            }
            break;
        case 2:
            change_pass();
            break;
        case 3:
            clrscr
            intro();
            printf(MAGENTA "---Logged out successfully!---\n");
            printf(RED "\nPress Enter to continue - " RESET);
            clrbfr();
            clrbfr();
            return; // return to main menu
        case 4:
            update_file();
            FREE();
            clrscr
            intro();
            printf(BLACK "---Exiting the Program---\n\n");
            exit(0);
        }
    }
}

void change_pass()
{
    clrscr
    intro();
    FILE *ptr;
    ptr = fopen("admin_pass.txt", "w");
    printf(MAGENTA "Enter new password - ");
    char buffer[100];
    scanf(" %[^\n]", buffer);
    getchar();
    fputs(buffer, ptr);
    fclose(ptr);
    printf("---Password changed successfully!---\n" RESET);
    printf(RED "\nPress enter to continue - " RESET);
    clrbfr();
}

void general()
{
    while (1)
    {
        clrscr
        intro();
        printf(CYAN "----Customer Panel----\n");
        printf("1. Display Menu\n");
        printf("2. New Order\n");
        printf("3. Display Order\n");
        printf("4. Logout & Return\n");
        printf("5. Exit\n");
        printf("Pick your choice : " RESET);

        int i;
        while (1)
        {
            if (scanf(" %d", &i) != 1)
            {
                printf(RED "Invalid choice. Please Try again.\n" RESET);
                clrbfr();
                continue;
            }
            else
            {
                if (i < 1 || i > 5)
                {
                    printf(RED "Invalid choice. Please Try again.\n" RESET);
                    clrbfr();
                    continue;
                }
                else
                {
                    clrbfr();
                    break;
                }
            }
        }
        printf("\n");
        if (i == 5)
        {
            update_file();
            FREE();
            clrscr
            intro();
            printf(BLACK "---Exiting the Program---\n\n");
            exit(0);
        }
        switch (i)
        {
        case 1:
            display_menu();
            break;
        case 2:
            take_order();
            break;
        case 3:
            view_order();
            total_bill();
            break;
        case 4:
            clrscr
            intro();
            printf(RED "Enter admin password - ");
            char password[100];
            scanf(" %[^\n]", password);
            clrbfr();
            load_admin_pass(); // loaded in bfrpass
            int check = 0;
            check = strcmp(password, bfrpass);
            if (check == 0)
            {
                printf("---Logged out successfully!---\n");
                printf("\nPress enter to continue - ");
                clrbfr();
                return;
            }
            else
            {
                printf("---Authorization failed!---\n");
                printf("\nPress enter to continue - ");
                clrbfr();
            }
            break;
        }
    }
}

int main()
{
    create_menu();
    while (1)
    {
        clrscr
        intro();
        printf(RED "---System main menu---\n");
        printf("1. Admin Access\n");
        printf("2. Customer Panel\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        char ch;
        while (scanf(" %c", &ch) == 1)
        {
            clrbfr();
            if (ch == '1' || ch == '2' || ch == '3')
                break;
            else
                printf("Invalid choice. Please try again - ");
        }
        printf("\n");
        switch (ch)
        {
        case '1':
            admin_login();
            break;
        case '2':
            clrscr
            intro();
            printf(RED "---General Access Granted---\n");
            printf("Press enter to continue - " RESET);
            clrbfr();
            general();
            break;
        default:
            clrscr
            FREE();
            intro();
            printf(BLACK "---Exiting the Program---\n\n");
            return 0;
        }
    }
}
