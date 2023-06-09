---
layout: archive
permalink: /C++/input_output
title: "Input and Output in C++ "
author_profile: true

header:
  image: "/images/tower3.jpeg"
  
---


Creating a file to output to (.txt)


```cpp
#include <iostream>
#include <fstream>


using namespace std;

int main()
{
    ofstream file;

    file.open("file_name.txt");  

    file.close();

}
```


Another way to Open/Create a file:

```cpp
#include <iostream>
#include <fstream>


using namespace std;

int main()
{
    ofstream file ("file_name.txt");
    file.close();
}
```


Lets have an example of writing to a txt file we created and use a vector with a for loop

```cpp
#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

int main()
{
    ofstream file ("hello.txt"); 

    vector <string> names;

    names.push_back("Devin");
    names.push_back("Lebron");
    names.push_back("Kobe");
    names.push_back("Jordan");

    for (string name : names )
    {
        file << name << endl;

    }

}
```


What if we dont want to overwrite the text in our file? We can do this:

```cpp
#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

int main()
{
    ofstream file ("hello.txt", ios::app); 

    vector <string> names;

    names.push_back("Wade");
    names.push_back("Bosh");

    for (string name : names )
    {
        file << name << endl;

    }

    file.close();

}

```


We can also check if the file was sucesfully opened using an "if" statement:


```cpp
#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

int main()
{
    ofstream file ("hello.txt", ios::app); 

    if(file.is_open())   

    {
        cout << "Sucessfully Opened the File\n";

    }


    vector <string> names;

    names.push_back("Wade");
    names.push_back("Bosh");

    for (string name : names )
    {
        file << name << endl;

    }

    file.close();

}

```

Opening a file by asking for its name in the input:

```cpp
#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

int main()


{
    string filename;
    cin >> filename;

    ofstream file (filename, ios::app); 

    if(file.is_open())   

    {
        cout << "Sucessfully Opened the File\n";

    }

    vector <string> names;

    names.push_back("Wade");
    names.push_back("Bosh");

    for (string name : names )
    {
        file << name << endl;

    }

    file.close();

}
```


Lets now read from files!

this example here is using a tacos.txt that we created above.


```cpp
#include <iostream>
#include <fstream>
#include <vector>


using namespace std;

int main()
{
    ifstream file ("tacos.txt"); // wont open if file doesnt exist

    vector <string> names;

    string input; // where names are being stored tempoary

    while(file >> input )   // will evaluate as true if the read is sucessful (return file)
    {
        names.push_back(input);

    }

    for(string name : names )
    {
        cout << name << endl;
    }
    return 0;


}
```


There are other ways to do input.

Using .get() method:

```cpp
#include <iostream>
#include <fstream>
#include <vector>


using namespace std;

int main()
{
    ifstream file ("tacos.txt"); 

    char temp = file.get(); 

    cout << temp << "\n";

    return 0;

}  
```


Another way is getline()

Will return the first line in the file

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main()
{
ifstream file ("tacos.txt"); 

string line;

getline(file,line );  

cout << line << "\n";

return 0;

}

```
