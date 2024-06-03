#include<iostream>
#include<conio.h>
#include<string.h>
#include<iomanip>
#include<fstream>
using namespace std;
class hotel
{
private:
    int roomno;
    char name[20];
    char phno[10];
    public:
    void mainmenu();
    void roomlist();
    void addroom();
    void removeroom();
    void modify();
    void edit();
    void searchcustomer();

     void addcustomer();
     void showdetails();
     int getroom()
     {
         return roomno;
     }
     char* getname()
     {
         return name;
     }
     char* getphone()
     {
         return phno;
     }
};
void hotel:: mainmenu()
{
    int choice;
    system("CLS");
    cout<<"\n\n\t\t\t\xB1\xB1\xB1 MENU \xB1\xB1\xB1\n\n1. List of Rooms\n2. Add Room\n3. Remove room\n4. Edit customer record\n5. Search Customer\n6. Exit\n\n\t\t\t\tEnter Option:";
   cin>>choice;
   switch (choice)
   {
   case 1:
       roomlist();
       break;

   case 2:
       addroom();
       break;

   case 3:
       removeroom();
       break;

   case 4:
       modify();
       break;

   case 5:
       searchcustomer();
        break;

   case 6:
       exit(0);

   default:
       system("cls");
       cout<<"\n\n\n\n\t\t\tWRONG CHOICE!!!!\n\n\n\t\t\t\tPress any key to continue...";
       getch();
   }
}
fstream fp;
hotel h;

void hotel::addcustomer()
{
    cout<<"\n\n  Enter room number: ";
    cin>>roomno;
    if(roomno>0 && roomno<=10)
    {
      cout<<"\n  Enter name: ";
     cin>>name;
      cout<<"\n  Enter phone number: ";
     // cin.ignore();
      //cin.getline(phno,10);
      cin>>phno;
    }
    else
    {
        system("cls");
        cout<<"\n\n\n\n\n\t\t\t ROOM NOT AVAILABLE!!";
    }

}

void hotel:: roomlist()
{
    system("CLS");
    cout<<"\n\n\t\t\t\xB1\xB1\xB1 Room LIst \xB1\xB1\xB1\n";
    fp.open("hotel.dat",ios::in);
    cout<<"\n\n-------------------------------------------------------------------\n";
    cout<<"\nRoom no\t\t"<<setw(3)<<"Name\t\t"<<setw(20)<<" Phone no "<<endl;
    cout<<"\n-------------------------------------------------------------------\n";
    while(fp.read((char*)&h,sizeof(hotel)))
    {
      cout<<roomno<<setw(3)<<"\t\t"<<name<<setw(20)<<"\t\t"<< phno<<endl;
    }
    fp.close();
    cout<<"\n\n\t\tPress any key to go back..";
    getch();
    mainmenu();
}

void hotel::addroom()
{
    system("CLS");
    cout<<"\n\n\t\t\t\xB1\xB1\xB1 ADD ROOM \xB1\xB1\xB1\n";
    fp.open("hotel.dat",ios::out|ios::app);
    h.addcustomer();
    fp.write((char*)&h,sizeof(hotel));
    fp.close();
    cout<<"\n\n\t\tPress any key to go back..";
    getch();
    mainmenu();

}

void hotel:: removeroom()
{
    system("CLS");
    cout<<"\n\n\t\t\t\xB1\xB1\xB1 REMOVE ROOM \xB1\xB1\xB1\n";
    int n,f=0;
    cout<<"\n\nEnter the room no of the customer you want to remove: ";
    cin>>n;
    fp.open("hotel.dat",ios::in|ios::binary);
    fstream demo;
    fstream extra;
    demo.open("temp.dat",ios::out|ios::app|ios::binary);
    fp.seekg(0,ios::beg);
    while(fp.read((char*)&h,sizeof(hotel)))
    {
        if(h.getroom()== n)
        {
            extra.write((char*)&h,sizeof(hotel));
             f=1;
        }
        else
        {
            demo.write((char*)&h,sizeof(hotel));

        }
    }
    demo.close();
    extra.close();
    fp.close();
    remove("hotel.dat");
    rename("temp.dat","hotel.dat");
    if(f==1)
    {
        system("cls");
        cout<<"\n\n\n\n\n\t\t\tRecord deleted!";
    }
    else
    {
       system("cls");
       cout<<"\n\n\n\n\n\t\t\tRecord not found!!";
    }

    cout<<"\n\n\t\tPress any key to go back..";
    getch();
    mainmenu();

}

void hotel::modify()
{
    system("CLS");
    cout<<"\n\n\t\t\t\xB1\xB1\xB1 EDIT CUSTOMER RECORD \xB1\xB1\xB1\n";
    int n;
    int f=0;
    int pos;
    cout<<"\nEnter room no: ";
    cin>>n;
    fp.open("hotel.dat",ios::in|ios::out);
    while(fp.read((char*)&h,sizeof(hotel)) && f==0)
    {
        if(h.getroom()==n)
        {
            h.showdetails();
            cout<<"\n\n\t\tEnter new info: "<<endl;
            h.edit();
            pos= -1*sizeof(h);
            fp.seekp(pos,ios::cur);
            fp.write((char*)&h,sizeof(hotel));
            cout<<"\n\nReccord updated successfully";
            f=1;
        }
    }
    fp.close();
    if(f==0)
    {
        system("cls");
        cout<<"\n\n\t\tRecord not found!!";
    }
    cout<<"\n\n\t\tPress any key to go back..";
    getch();
    mainmenu();
}

void hotel:: edit()
{

    cout<<"\n\nRoom number: "<<roomno;
    cout<<"\n\nModify name: ";
    cin>>name;
    cout<<"\nModify phone number: ";
    cin>>phno;

}
void hotel::searchcustomer()
{
    system("CLS");
    int f=0;
    int n;
    cout<<"\n\n\t\t\t\xB1\xB1\xB1 SEARCH CUSTOMER\xB1\xB1\xB1\n";
    cout<<"\nEnter Room number: ";
    cin>>n;
    fp.open("hotel.dat",ios::in);
    while(fp.read((char*)&h,sizeof(hotel)))
    {
        if(h.getroom()==n)
        {
            h.showdetails();
            f=1;
        }
    }
    fp.close();
    if(f==0)
    {
        system("cls");
        cout<<"\n\nCUSTOMER DOES NOT EXIST!!!!";
        cout<<"\n\n\nPress any key to go back....";
    }
    cout<<"\n\n\t\tPress any key to go back..";
    getch();
    mainmenu();
}

void hotel:: showdetails()
{
    cout<<"\n\nRoom no:  "<<roomno;
    cout<<"\nName     :  "<<name;
    cout<<"\nPhone no :  "<<phno;
}
int main()
{
    cout<<"\n\n\n\n\n";
    cout<<"\n\t\t\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\n";
    cout<<"\n\t\t\xB2\xB2\tHOTEL BOOKING SYSTEM\t    \xB2\xB2\n";
    cout<<"\n\t\t\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\xB2\n";
    cout<<"\n\n\n\n\t\t\t\t\tPress any key to continue...";
    getch();
    h.mainmenu();
    return 0;
}
