# Grammar
**painter** &rarr; draw **shape**   
**shape** &rarr; circle | square | triangle | line

# Scanner
```c
#include <bits/stdc++.h>
using namespace std;

//Tokens
enum  tokens {
    draw,
    triangle,
    square,
    line,
    space,
    Error,
    End
};

class Scanner {

    ifstream input;
    ofstream output;
    char ch;

public:

    Scanner(string in , string ot) {
        input.open(in);
        output.open(ot);
    }

    tokens getToken() {

        string token ;
        if (input.get(ch)) {

            if (isspace(ch))
                return space;

            while (isalpha(ch)) {
                token += ch;
                if(!input.get(ch))
                    break;
            }
            input.putback(ch);

            if ( token == "draw" )
                return draw;
            else if ( token == "square" )
                return square;
            else if ( token == "line" )
                return line;
            else if ( token == "triangle" )
                return triangle;
            else
                return Error;

        } else
            return End;
    }


    void setToken() {

        tokens token = getToken();

        while ( token != End ) {
            if ( token ==  draw )
                output << "<draw>\n";
            else if ( token == square )
                output << "<square>\n";
            else if ( token == triangle )
                output << "<triangle>\n";
            else if ( token == line )
                output << "<line>\n";
            else if ( token == space )
                output << "<space>\n";
            else if ( token == Error )
                output << "<Error>\n";

            token = getToken();
        }
        output << "<End>";
    }
};

int main() {

    Scanner sc("input.txt" , "tokens.txt");
    sc.setToken();

    return 0;
}
```
## set the input into the file
**let's try to write a valid input**    
![Alt text](/images/setInputInFile.gif)
## Run it
![Alt text](/images/runIt.gif)
## the output
![Alt text](/images/tokens.gif)
# Parser
```c
#include <bits/stdc++.h>
using namespace std;

//Tokens
enum  tokens {
    draw,
    triangle,
    square,
    line,
    space,
    Error,
    End
};

class Parser {

    ifstream fileOfTokens;
    string figure;
    tokens next;

public:

    Parser(string Tokens) {
        fileOfTokens.open(Tokens);
    }

    bool isMatch(tokens token) {

        if (token == next) {
            next = getToken();
            return true;
        }
            return false;
    }

    bool shape() {
        return  isMatch(triangle) || isMatch(line) || isMatch(square);
    }

    bool painter() {

        if(!isMatch(draw)) {
            cout << "Expected 'draw'";
            return false;
        }
        else if(!isMatch(space))
            return false;
        if(!shape()) {
            cout << "Expected  shape to drawing";
            return false;
        }

        return isMatch(End) ;
    }

    tokens getToken() {

        string token;
        getline(fileOfTokens , token);

        if ( token == "<draw>" )
            return draw;
        else if ( token == "<square>" ) {
            figure = "square";
            return square;
        } else if ( token == "<line>" ) {
            figure = "line";
            return line;
        } else if ( token == "<triangle>" ) {
            figure = "triangle";
            return triangle;
        } else if ( token == "<space>" )
            return space;
        else if( token == "<Error>")
            return Error;
        else
            return End;
    }

    bool isValid() {
        next = getToken();
        return painter();
    }

    void drawing() {

        if(figure == "line")
            cout << "___________________";
        else if (figure == "square") {

            cout << " ________ \n";
            cout << "|        |\n";
            cout << "|        |\n";
            cout << "|        |\n";
            cout << "|________|\n";

        } else if (figure == "triangle") {

            cout << "*\n";
            cout << "***\n";
            cout << "*****\n";
            cout << "*******\n";
            cout << "*********\n";
        }
    }
};

int main() {

    Parser sc("F:\\compiler\\tokens.txt");
    if (sc.isValid())
        sc.drawing();

    return 0;
}

```
