#include <iostream>
#include <unordered_map>
#include <fstream>
#include <string>
#include <algorithm>


//File Name
char file[]="Dictionary.txt";

using namespace std;

void startScreen()
{
	cout<<endl;
	cout<<"				Dectionary			"<<endl;
	cout<<"				==========			"<<endl;

	cout<<endl;
	cout<<" 1- Search "<<endl;
	cout<<" ================================== "<<endl;
	cout<<" 2- Add a New Word "<<endl;
	cout<<" ================================== "<<endl;
	cout<<" 3- Edit Meaning of a Word "<<endl;
	cout<<" ================================== "<<endl;
	cout<<" 4- Display All Dictionary's words "<<endl;
	cout<<" ================================== "<<endl;
	cout<<endl;
	cout<<" --> ";
}

//Read to Memory function
void Path_from_file (string fileName /*File_Name*/ , unordered_map<string,string> & word)
{
	//Create Ifstream Object To Read From File 
	ifstream read(fileName);

	//Vars
	char key[100];
	char value[10000];

	if(!read.is_open())
	{
		cout<<"File Not Open !! "<<endl;
	}
	while(!read.eof())
	{
		read.getline(key,100,'\n');
		read.getline(value,10000,'\n');

		//insert in Map
		word.insert(make_pair(key,value));

	}//end Reading From File To Memory
	read.close();
}

//Lower Words Functio
void Small(string & word)
{
	for(int i=0 ; i<word.size() ; i++ )
	{
		word[i]=towlower(word[i]);
	}
}

//Function Contain
bool Contain(unordered_map<string , string > word , string key)
{
	unordered_map<string , string >::iterator it; 

	it=word.find(key);

	if(it!=word.end())
	{
		return true;
	}
	else 
	{
		return false;
	}
}

//Add Function
void Add( unordered_map<string , string> & word , string key)
{
	//Variabels
	string value;

	//Function Contain to Check wether the word exist or not
	if(Contain(word,key))
	{
		cout<<endl;
		cout<<"                     Word Is already Existed           "<<endl;
		cout<<endl;
	}
	else
	{
		cout<<"Enter Its Meanning : ";
		getline(cin,value);

		word.insert(make_pair(key,value));
	}
}

//Search Function( Map , Iterator )
void Search (unordered_map<string , string > &word)//&: to Call word(Unordered_Map) by Reference When search for not save word (else Condtion) Save it After That
{
	//Vars
	string _word , x ;

	unordered_map <string , string >::iterator it;

	cout<<"Enter The Word : ";
	getline(cin,_word);
	Small(_word);

	it=word.find(_word);

	if(it!=word.end())
	{
		cout<<it->first<<" : "<<it->second<<endl;
	}
	else
	{
		while(true)
		{
			cout<<"Word Not Founded !! , Do You Want to Save. it (Yes/No) : ";
			getline(cin,x);
			//Fnc Small To low Alphabets
			Small(x);
			if(x=="yes")
			{
				Add(word,_word);
				break;
			}
			else if(x=="no")
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

	}

}

//Function Write to File
void Write_to_file(string fileName , unordered_map<string , string > word )
{
	ofstream write(file);

	unordered_map <string , string >::iterator it;

	for(it=word.begin() ; it!=word.end() ; it++ )
	{
		write<<it->first<<endl;
		write<<it->second<<endl;
	}
	write.close();
}

//Edit Function
void Edit(unordered_map<string , string > & word )
{
	//Vars
	string key , value;
	unordered_map<string , string>::iterator it;

	cout<<"Enter the Word to Edit its Meainning : "; getline(cin,key);
	//Fnc Small
	Small(key);

	it=word.find(key);

	if(it!=word.end())
	{
		cout<<"Enter Its Meanning : "; getline(cin,value);
		word.erase(key);
		word[key]=value;
	}
	else
	{
		cout<<endl;
		cout<<"                     Word Not Found !!            "<<endl;
		cout<<endl;
	}

}

//Display Function
void Display(unordered_map<string , string > & word)
{
	//Iterator
	unordered_map<string ,string >::iterator it;

	vector<pair<string,string>> _vector;
	vector<pair<string,string>>::iterator i;
	vector<pair<string,string>>::iterator j;

	//Assign Words & values to Vector From the unordered Map
	for(it=word.begin() ; it!=word.end() ; it++ )
	{
		_vector.push_back(make_pair(it->first , it->second));
	}

	//Sorting 
	for(i=_vector.begin() ; i!=_vector.end() ; i++ )
	{
		for(j=i ; j!=_vector.end() ; j++ )
		{
			if(i->first > j->first)
			{
				swap(i->first , j->first);
				swap(i->second , j->second);
			}
		}
	}

	//Display Result
	for(i=_vector.begin() ; i!=_vector.end() ; i++ )
	{
		cout<<" "<<i->first<<" : "<<i->second<<endl;
	}
	cout<<endl;
}