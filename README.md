# AccountManagement
간단한 계좌관리 c++

#source code
    
include<iostream>  
using namespace std;   
int autoid=1000000; //자동 계좌번호를 삽입하기위에 전역변수 정의   
class Account //계좌 정보 클래스 정의  
{  
protected:  //상속받은 클래스들은 허용   
 int id;    //계좌번호   
 char *name ; //계좌주  
 double balance;//잔액   
public:     
 Account(char *name, double balance);//생성자   
 Account(const Account &c);//복사 생성자   
 ~Account();// 소멸자    
 const int getid() const;//id리턴     
 const char* getname() const;//이름 리턴     
 int getbalance();//잔액보여주기      
 virtual void deposit(const double money) ; //입금  ->함수 오러라이딩을 위해 가상함수 사용한다.    
 void withdraw(const double money);//출금    
 void enquiry() const; //조회    
};    
    
Account::Account(char *name, double balance=0) //생성자 잔액을 입력안하면 초기로 0을 입력한다.     
{      
  this->id=autoid++;//계좌번호 삽입   
  this->name=new char[strlen(name)+1];// 이름공간 동적할당   
  strcpy(this->name,name); //이름 복사    
  if(balance<=0)//잔액입력 에러처리     
   this->balance =0; //잔액의 값을 음수로 하면 무조건 계좌의 값을 0으로 한다.      
  else     
   this->balance=balance; //정상 잔액 입력(양수)     
}     
 Account::Account(const Account &c) //복사 생성자     ->const 지정     
 {     
  this->id=autoid;//계좌번호 삽입     
  this->name=new char[strlen(c.name)+1];// 이름공간 동적할당     
  strcpy(this->name,c.name); //이름 복사     
  if(c.balance<=0)//잔액입력 에러처리     
   this->balance =0; //잔액의 값을 음수로 하면 무조건 계좌의 값을 0으로 한다.      
  else     
   this->balance=c.balance; //정상 잔액 입력(양수)     
 }     
 Account::~Account()//소멸자     
 {     
  delete[] name;//계좌주 동적 메모리 소멸     
 }     
 const int Account::getid() const //id 리턴     -> 계좌번호는 바꿀 필요가 없으므로 const화     
 {     
  return id;     
 }     
 const char* Account::getname() const//이름 리턴  -> 이름은 수정할 필요 없으므로  const화     
 {     
  return name;     
 }     
 int Account::getbalance(){//잔액 리턴     
  return balance;     
 }     
 void Account::deposit(const double money) //입금 ->money의 값 변경 필요 없다.     
 {     
  if(money<=0) //에러처리     
   cout << "입금이 실패하였습니다. " << endl;     
  else {     
   balance = balance + money; //잔액에 입금액 더한다.     
      cout<<name <<" 계좌에 " <<money<<"원 입금이 완료되었습니다."<< endl ;     
  }     
 }     
 void Account::withdraw(const double money)//출금 ->money변경 불필요     
 {     
  if(money<=0||money>balance)//에러처리     
   cout << "출금 할 수 없습니다." << endl;     
  else {     
   balance = balance - money;//잔액에 출금액 뺀다.     
   cout<<money<<" 출금이 완료되었습니다."<< endl ;     
  }     
 }     
 void Account::enquiry() const  //조회    ->const로 상수화!     
 {     
  cout << "------------------" <<endl;     
  cout << "계좌번호 : " << id <<endl;     
  cout << "이    름 : " << name <<endl;     
  cout << "잔    액 : " << balance <<endl;     
 }     

 class Credit : public Account //신용계좌 ->일반계좌를 상속받는다.     
 {     
 public: //퍼블릭접근          
  Credit(char *name, double balance); //생성자          
  void deposit(const double money);   //입금함수 ->오버라이딩     
 };     
     
 Credit::Credit(char *name, double balance=0)  //신용계좌 생성자 정의     
  : Account(name,balance) //Account생성자호출     
 {     
  if (balance>0)     
  this->balance+=(balance*0.01); //계좌 생성될때 처음 입금되는 값의 1프로의 이자를 더한다.      
 }     
     
 void Credit::deposit(const double money) //신용계좌 입금      
 {     
 if(money<=0) //에러처리     
   cout << "입금이 실패하였습니다. " << endl;     
  else {     
   balance = balance + (money*1.01); //잔액에 입금액과 이자 1프로를 더한다.     
      cout<<name <<" 계좌에 "<<money<<"원과 이자"<<0.01*money<<"원 입금이 완료되었습니다."<< endl ;     
  }     
 }     
     
 class Donation : public Account  // 기부계좌 ->일반계좌를 상속 받는다.     
 {     
 public:     
 Donation(char *name, double balance); //생성자     
  void deposit(const double money);  //입금함수 ->오버라이딩     
 };     
     
 Donation::Donation(char *name, double balance=0) //기부계좌 생성자 정의     
  : Account(name,balance)  //Account생성자호출     
 {     
  if (balance>0)     
  this->balance-=(balance*0.01); //계좌 생성될때 처음 입금되는 값의 1프로를 기부한다.      
 }     
     
 void Donation::deposit(const double money) //기부계좌 입금      
 {     
 if(money<=0) //에러처리     
   cout << "입금이 실패하였습니다. " << endl;     
  else {     
   balance = balance + (money*0.99); //잔액에 입금액을 더한후 1프로 기부값을 잔액에서 뺀다.     
      cout <<name <<" 계좌에 "<<money<<"원 입금완료되었고,"<<0.01*money<<"원을 기부하였습니다."<< endl ;      
  }     
 }      
     
const int MAX = 100;// 관리할 계좌수 지정   -> const로 상수화!     
class AccManagement  //계좌관리클래스 정의     
{     
 Account* a[MAX]; //관리할 계좌들을 담을 맴버변수     
 int index;//인덱스     
public:     
 AccManagement() :index(0){}; //기본생성자 index를 0으로 초기화     
 ~AccManagement()     
 {     
  for(int i=0;i<index;i++)     
   delete a[i];     
 }     
 void make(Account* const c)//계좌 추가   -> 객체Account c를 const 지정      
 {     
  a[index]=c;     
  cout << "계좌가 생성되었습니다."<<endl;     
  index++; //index값하나 올려준다.     
 }     
 void print(int id) const //계좌조회-> id로 조회     
              //수정할 필요 없으므로 const화       
 {          
  int temp = 0;//사실 거짓을 판단하기 위해 정의     
  for(int i=0;i<index;i++) //전체를 조회하여 계좌번호가 같은 것을 찾는다.     
  {     
   if ((a[i]->getid()) == id) //같은 계좌번호가 있다면 출력     
   {     
    temp=1; //찾으면 1     
    a[i]->enquiry(); //Account의 enguiry출력멤버함수 사용     
    break; //찾으면 순환문을 나간다.     
   }     
  }      
  if (temp==0) //못찾으면 출력!!! temp값은 그대로 0     
  {cout <<"찾는 계좌가 없습니다."<<endl;}     
 }     
 void print(char *name) const //계좌조회-> 이름으로 조회->같은이름의 계좌는 전부 조회     
               //수정할 필요 없으므로 const화       
 {          
  int temp = 0;//사실 거짓을 판단하기 위해 정의     
  for(int i=0;i<index;i++) //전체를 조회하여 이름 같은 것을 찾는다.     
  {     
   if (strcmp(a[i]->getname(),name)==0)//같은 이름이 있다면 출력     
   {     
    temp=1; //찾으면 1     
    a[i]->enquiry(); //Account의 enguiry출력멤버함수 사용     
   }     
  }      
  if (temp==0) //못찾으면 출력한다.     
  {cout <<"찾는 계좌가 없습니다."<<endl;}     
       
 }     
 void printall() const // 모든계좌출력    ->수정할 필요없으므로 const지정     
 {     
  cout<<endl<<endl;     
  cout<<" 전체 계좌 리스트 " <<endl;     
  for(int i=0;i<index;i++)     
   a[i]->enquiry();  //순환문으로 전체 생성된 계좌를 출력     
  cout<<endl<<endl;     
 }     
 void mgm_deposit(const int id,const double money) //입금 ->계좌번호와 입금액을 인자값으로 받는다.     
                                                                      //인자값인 id와 money 수정 불필요             
 {          
  int temp = 0;//사실 거짓을 판단하기 위해 정의     
  for(int i=0;i<index;i++) //전체에서 같은 계좌번호를 찾는다.     
  {     
   if ((a[i]->getid()) == id)//같은 계좌가 있다면 입금 실행     
   {     
    temp=1; //찾으면 1     
    a[i]->deposit(money); //Account의 입금멤버함수 이용     
    break;//찾으면 순환문을 나간다.     
   }     
  }      
  if (temp==0) //못찾으면 출력한다.          
  {cout <<"계좌가 없어서 입금할 수 없습니다."<<endl;     
 }     
 void mgm_withdrow(const int id,const double money) //출금 ->계좌번호와 출금액을 인자값으로 받는다.     
                                                                             //인자값인 id와 money 수정 불필요     
 {     
  int temp = 0;//사실 거짓을 판단하기 위해 정의     
  for(int i=0;i<index;i++) //전체에서 같은 계좌번호를 찾는다.     
  {     
   if ((a[i]->getid()) == id)//같은 계좌가 있다면 출금 실행     
   {     
    temp=1; //찾으면 1     
    a[i]->withdraw(money); //Account의 출금멤버함수 이용     
    break;//찾으면 순환문을 나간다.     
   }     
  }      
  if (temp==0) //못찾으면 출력한다.     
  {cout <<"계좌가 없어서 입금할 수 없습니다."<<endl;}     
       
 }     
};     
void main()     
{     
 AccManagement object; //계좌를 관리하는 컨트롤클래스 생성     
 object.make(new Account("김성훈",1000)); //일반계좌생성     
 object.make(new Credit("김성훈1",1000)); //신용계좌생성     
 object.make(new Donation("김성훈2",1000)); //기부계좌생성     
 object.printall(); // 모든 계좌 출력     
 cout<<" =========계좌별 각각 같은 금액을 입금했을때======="<< endl <<endl ;     
 object.mgm_deposit(1000000,1000); //일반계좌 더할때      
 object.mgm_deposit(1000001,1000); //신용계좌 더할때          
 object.mgm_deposit(1000002,1000); //기부계좌 더할때     
 object.printall(); // 모든 계좌 출력     
}     
     
