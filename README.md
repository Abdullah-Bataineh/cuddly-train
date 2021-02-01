#include <iostream>
#include <string>
using namespace std;

class StringValidator {
  public:
  virtual ~StringValidator() {};
  virtual bool isValid(string input) = 0;
};

class LengthValidator : public StringValidator {
  /*
  	LengthValidator: which will consider a string valid only if it's longer than the required minimum and shorter than the required maximum.
  	1. Write the constructor of the class
  	2. Make the class non-Abstract.
  */
  private:
  int min_length, max_length;  
   public:
  LengthValidator(int a,int b){
   min_length=a;
   max_length=b;
  }
   bool isValid(string input){
     int x=input.length();
     if(x>=min_length && x<=max_length)
       return true;
     else 
       return false;
   }
};

class PatternValidator : public StringValidator 
{  
  /*
  	PatternValidator: which will consider string valid only if the string matches the supplied pattern. You should implement the following rules:
    	- have at least one upper-case letter.
        - have at least one lower-case letter.
        - have at least one digit.
        - have at least one special character.
        
    In this class only make it non-Abstract.
  */
 public:
   bool isValid(string input)
   {
     int uc=0,lc=0,d=0,c=0;
     int x=input.length();
     for(int i=0;i<x;i++)
     {
       if(isupper(input[i]))
         uc++;
       if(islower(input[i]))
         lc++;
       if(isdigit(input[i]))
         d++;
       if(ispunct(input[i]))
         c++;
       }
     if(uc==0||lc==0||d==0||c==0)
       return false;
     else 
       return true;
   }  
};
 
class PasswordValidator : public StringValidator {
  /*
  	PasswordValidator: which will consider if a password is valid or not.
    Note: the length of th password should be eight characters long at least and fifteen characters long at most. 
    
    	1. compose the type of validator.
        2. Write the constructor of the class.
  		3. Make the class non-Abstract
  */
  LengthValidator lv;
  PatternValidator pv;
  public:
  PasswordValidator():lv(8,15){}
  bool isValid(string input){
    return (lv.isValid(input)&&pv.isValid(input));
  }
  //4. write the implementation of encrypt function.
  //In this function a key value which is "7" is subtracted from the ASCII value of the characters.
  void encrypt(char *p) {
  for(int i=0;p[i]!='\0';i++){
    char c=p[i]-7;
    cout<<c;}
  }
};

//Write the implementation of the non-member function printValid, which is return if the given password is valid or not. 
bool printValid(StringValidator &validator,string password) {
return(validator.isValid(password));  
}

int main() {
  int MAXSIZE = 15;
  char password[MAXSIZE];
  cin >> password;
  
  PasswordValidator validator;
  
  cout << "The password \"" << password << "\" is ";
  if(printValid(validator, password)){
    cout << "valid\n"
     	 << "The password after encryption: ";
    validator.encrypt(password);
  }
  else
    cout << "invalid\n";
    
  
  return 0;
}
