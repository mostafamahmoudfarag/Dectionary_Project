#include <iostream>
#include "Header.h"
#include <unordered_map>
#include <fstream>
#include <string>


using namespace std;

int main()
{
	//Create Map & iterator
	unordered_map <string , string > word;

	//Read From File
	Path_from_file(file , word);

	//To Exit
	string choice;
	bool flag=false;

	while(true)
	{
		flag=false;

		//Start Screen : Program
		startScreen();

		//Selection
		int select;
		cin>>select;
		cin.ignore();

		if(select==1)
		{
			Search(word);
		}
		else if(select==2)
		{
			//Variabels
			string key ;

			cout<<"Enter the Word : ";
			getline(cin,key);
			//Fnc
			Small(key);

			////Fnc 
			//Contain(word,it,key);

			Add(word , key );
		}
		else if(select==3)
		{
			Edit(word);
		}
		else if(select==4)
		{
			Display(word);
		}
		else 
		{
			cout<<endl;
			cout<<"                     Un Known Answer !! "<<endl;
			cout<<endl;
		}

		
		
		while(true)
		{
			cout<<"Do You Want To Exit  (Yes/No) : ";
			getline(cin,choice);
			Small(choice);
			if(choice=="yes")
			{
				Write_to_file(file,word);
				flag=true;
				break;
			}
			else if(choice=="no")
			{
				break;
			}
			else
			{
				cout<<endl;
				cout<<"                     Un Known Answer !! "<<endl;
				cout<<endl;
			}
		}

		if(flag==true)
		{
			break;
		}
	}


}