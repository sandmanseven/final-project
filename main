#include "Function.h"

//*************Variable Coeffiencal*******//
class Var{
private:
	string var;
	string value;
public:
	Var(string s):var(s),value(s){}
	void set(string s, double num)
	{
		ostringstream s1;
		s1 << num;
		var = s;
		value = s1.str() + s;
	}
	string getValue()
	{
		return value;
	}
	string getVar()
	{
		return var;
	}
};

//************Constant Number*************//
class Const{
private:
	double num;
	string s;
public:
	Const(double num) :num(num){}
	string getValue()
	{
		s = Double2String(num);
		return s;
	}
};

//***********Parent class*****************//
//****************************************//
//****Common Function and Variable********//
//******for the children class************//
//****************************************//

class Operator{
public:
	class Node
	{
	public:
		Node *leftchild;
		Node *rightchild;
		int flag_priority = 0;
		string s;
		Node(string var) : s(var), rightchild(nullptr), leftchild(nullptr){}
	};
	string Oper;
	string val;
	
	//priority=2 means '+','-'  priority=1 means '*','/' *********//
	//int flag_priority = 0;

	Node *root;

	string getValue()
	{
		return val;
	}
	//*********Output all operator InOrder Trave *************//
	string inOrderTraverse(Node *p)
	{
		//string  ans = "";
		if (p)
		{
			if (p->leftchild)
			{
				if ((p->leftchild->s=="+")||(p->leftchild->s == "-") && (p->leftchild->flag_priority<p->flag_priority))
				{
					ans_order = ans_order + "(";
					inOrderTraverse(p->leftchild);
					ans_order = ans_order + ")";
					//cout << ")";
				}
				else
					inOrderTraverse(p->leftchild);
			}
			ans_order = ans_order + p->s;
			//cout << p->s;
			if (p->rightchild)
			{
				if ((p->leftchild->s == "+") || (p->leftchild->s == "-")&&(p->rightchild->flag_priority<p->flag_priority))
				{
					ans_order = ans_order + "(";
				//	cout << "(";
					inOrderTraverse(p->rightchild);
					ans_order = ans_order + ")";
				//	cout << ")";
				}
				else
					inOrderTraverse(p->rightchild);
			}
		}
		//cout << ans;
		return ans_order;
	}
	void pre()
	{
		Node *p = this->root;
		ans_order = "";
		string s=inOrderTraverse(p);
		cout << s << endl;
	}

	//**************Solve the equation*****************//
	double solve(double eps)
	{
		return Solve(this->val,eps);
	}

	//************Set the x equal a number, then get the result of function********//
	double result(double x)
	{
		double *cof = new double[100];
		for (int i = 0; i < 100; i++)
		{
			cof[i] = 0;
		}
		if (this->val[1]!='o')
		Poly2Array(this->val, cof);
		else
		{
			string s = this->val;
			s.erase(0, 3);
			s = s.erase(s.size() - 1, s.size());
			Poly2Array(s, cof);
		}
		return Result(cof,x);
	}

	//**********************set the function equal a variable, then get the x*******//
	string function(string y)
	{
		string ans = "("+y;
		double *cof = new double[100];
		for (int i = 0; i < 100; i++)
		{
			cof[i] = 0;
		}
		Poly2Array(this->val, cof);
		for (int i = 2; i < 100; i++)
		{
			if (cof[i] != 0)
			{
				return "It's too complex to get answer!!!";
			}
		}
		if (cof[0] > 0)
		{
			ans = ans + "-" + Double2String(cof[0]);
		}
		else if (cof[0] < 0)
		{
			ans = ans + "+" + Double2String(abs(cof[0]));
		}
		ans = ans + ")";
		if (cof[1] == 1)
			return ans;
		if (cof[1] == -1)
		{
			ans.insert(0, "-");
			return ans;
		}
		if (cof[1] == 0)
		{
			return "x can any number!";
		}
		else
		{
			ans = ans + "/" + "(" + Double2String(cof[1]) + ")";
			return ans;
		}
	}


	friend ostream& operator << (ostream &s, Operator oper)
	{
		s << oper.val;
		return s;
	}
	
};

//**********Negative **************//
//*********Const**************//
//*******Variable************//
//*******Operator************//

class Negative : public Operator {
private:
	string s;
public:
	Negative(Const org)
	{
		s = Double2String(String2Double(org.getValue())*-1.0);
		val = s;
	}
	Negative(Var org)
	{
		string tem = org.getValue();
		if (tem[0] != '-')
			s = tem.insert(0, "-");
		else
			s = tem.erase(0, 1);
		val = s;
	}
	Negative(Operator org)
	{
		string tem = org.val;
		for (int i = 0; i < org.val.size(); i++)
		{
			if (tem[i] == '+')
				tem[i] = '-';
			else if (tem[i] == '-')
				tem[i] == '+';
		}
		if (tem[0] != '-')
			s = tem.insert(0, "-");
		else
			s = tem.erase(0, 1);
		val = s;
	}
	string getValue()
	{
		return s;
	}
};

//**********Add **************//
//******Const+Const***********//
//*****Const+ Operator *******//
//*****Operator+ Const*********//
//*****Variable+Const*********//
//*****Const+Variable*********//
//*******Variable+Operator****//
//*******Operator+Variable****//
//*******Variable+Variable****//
//*******Operator+Operator****//

class Add :public Operator{
public:

	//***********Constant + Constant**********//
	Add(Const left, Const right)
	{
		root = new Node("+");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
	}

	//*************Constant + Operator***********//
	Add(Const left, Operator right)
	{
		Node *temp = new Node("+");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
		root = temp;
	}

	//******************Operator + Constant***********//
	Add(Operator left, Const right)
	{
		Node *temp = new Node("+");
		temp->leftchild = left.root;
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
		root = temp;
	}

	//******************Variable+Const*************//
	Add(Var left, Const right)
	{
		root = new Node("+");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
	}

	//*****Const+Variable*********//
	Add(Const left, Var right)
	{
		root = new Node("+");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
	}

	//*******Variable+Operator****//
	Add(Var left, Operator right)
	{
		Node *temp = new Node("+");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
		root = temp;
	}

	//*******Operator+Variable****//
	Add(Operator left, Var right)
	{
		Node *temp = new Node("+");
		temp->leftchild = left.root;
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
		root = temp;
	}

	//*******Variable+Variable****//
	Add(Var left, Var right)
	{
		Node *temp = new Node("+");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
		root = temp;
	}

	//*******Operator+Operator****//
	Add(Operator left, Operator right)
	{
		Node *temp = new Node("+");
		temp->leftchild = left.root;
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		val = Combining(left.getValue(), right.getValue());
		root = temp;
	}
};

//**********Sub **************//
//******Const-Const***********//
//*****Const- Operator *******//
//*****Operator- Const*********//
//*****Variable-Const*********//
//*****Const-Variable*********//
//*******Variable-Operator****//
//*******Operator-Variable****//
//*******Variable-Variable****//
//*******Operator-Operator****//

class Sub : public Operator {
public:
	//******Const-Const***********//
	Sub(Const left, Const right)
	{
		root = new Node("-");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 1;
		Operator tem = Negative(right);
		val=Combining(left.getValue(), tem.getValue());
	}

	//*****Const- Operator *******//
	Sub(Const left, Operator right)
	{
		Node *temp = new Node("-");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
		root = temp;
	}
	//*****Operator- Const*********//
	Sub(Operator left, Const right)
	{
		Node *temp = new Node("-");
		temp->leftchild = left.root;
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
		root = temp;
	}

	//*****Variable-Const*********//
	Sub(Var left, Const right)
	{
		root = new Node("-");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
	}

	//*****Const-Variable*********//
	Sub(Const left, Var right)
	{
		root = new Node("-");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
	}

	//*******Variable-Operator****//
	Sub(Var left, Operator right)
	{
		Node *temp = new Node("-");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
		root = temp;
	}

	//*******Operator-Variable****//
	Sub(Operator left, Var right)
	{
		Node *temp = new Node("-");
		temp->leftchild = left.root;
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
		root = temp;
	}

	//*******Variable-Variable****//
	Sub(Var left, Var right)
	{
		Node *temp = new Node("-");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
		root = temp;
	}

	//*******Operator-Operator****//
	Sub(Operator left, Operator right)
	{
		Node *temp = new Node("-");
		temp->leftchild = left.root;
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		Operator tem = Negative(right);
		val = Combining(left.getValue(), tem.getValue());
		root = temp;
	}
};
	

//**********Multiply **************//
//******Const*Const***********//
//*****Const* Operator *******//
//*****Operator* Const*********//
//*****Variable* Const*********//
//*****Const* Variable*********//
//*******Variable* Operator****//
//*******Operator* Variable****//
//*******Variable* Variable****//
//*******Operator* Operator****//
class Multi : public Operator {
public:
	
	//******Const*Const***********//
	Multi(Const left, Const right)
	{
		root = new Node("*");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
	}

	//*****Const* Operator *******//
	Multi(Const left, Operator right)
	{
		Node *temp = new Node("*");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = right.root;
		temp->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
		root = temp;
	}

	//******************Operator * Constant***********//
	Multi(Operator left, Const right)
	{
		Node *temp = new Node("*");
		temp->leftchild = left.root;
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
		root = temp;
	}

	//******************Variable*Const*************//
	Multi(Var left, Const right)
	{
		root = new Node("*");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
	}

	//*****Const+Variable*********//
	Multi(Const left, Var right)
	{
		root = new Node("*");
		root->leftchild = new Node(left.getValue());
		root->rightchild = new Node(right.getValue());
		root->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
	}

	//*******Variable+Operator****//
	Multi(Var left, Operator right)
	{
		Node *temp = new Node("*");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = right.root;
		temp->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
		root = temp;
	}

	//*******Operator+Variable****//
	Multi(Operator left, Var right)
	{
		Node *temp = new Node("+");
		temp->leftchild = left.root;
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
		root = temp;
	}

	//*******Variable+Variable****//
	Multi(Var left, Var right)
	{
		Node *temp = new Node("*");
		temp->leftchild = new Node(left.getValue());
		temp->rightchild = new Node(right.getValue());
		temp->flag_priority = 2;
		val = Multiplies(left.getValue(), right.getValue());
		root = temp;
	}

	//*******Operator+Operator****//
	Multi(Operator left, Operator right)
	{
		Node *temp = new Node("+");
		temp->leftchild = left.root;
		temp->rightchild = right.root;
		temp->flag_priority = 1;
		val = Multiplies(left.getValue(), right.getValue());
		root = temp;
	}
};

class Div : public Operator {
public:
	template<class T>
	Div(T left, T right)
	{
		val = "(" + left.getValue() + ")/(" + right.getValue() + ")";
	}
};
class Diff : public Operator {
public:
	template <class T>
	Diff(T poly)
	{
		val=Differential(poly.getValue());
	}
};

class Integ : public Operator {
public:
	template <class T>
	Integ(T poly)
	{
		val = Integral(poly.getValue());
	}
};

class Cos : public Operator {
public:
	template <class T>
	Cos(T poly)
	{
		val = "cos(" +( poly.getValue()) + ")";
		//flag_porit
	}
};

class Sin : public Operator {
public:
	template <class T>
	Sin(T poly)
	{
		val = "sin(" +( poly.getValue()) + ")";
	}
};

int main()
{
	
	Operator e=Add(Const(-5),Const(3));
	cout << "e: " <<e << endl;
	
	Operator e1 = Add(Const(5), e);
	cout << "e1: " << e1 << endl;
	
	Operator e2 = Add(Var("x"), Const(5));
	cout << "e2: " << e2 << endl;
	
	
	Operator e3 = Add(Const(5),Var("x"));
	cout << "e3: " << e3 << endl;
	
	Var v("x");
	v.set("x", 5);
	
	Operator e4 = Add(Const(-5), v);
	cout << "e4: " << e4 << endl;
	
	Operator e5 = Add(v, e4);
	cout << "e5: " << e5 << endl;
	
	Operator e6 = Add(e3, v);
	cout << "e6: " << e6 << endl;
	
	Operator e7 = Add(v, v);
	cout << "e7: " << e7 << endl;

	Operator e8 = Add(e5, e6);
	cout << "e8: " << e8 << endl;
	
	Operator e9 = Negative(Const(5));
	cout << "e9: " << e9 << endl;

	Operator e10 = Negative(v);
	cout << "e10: " << e10 << endl;
	
	Operator e11 = Negative(e5);
	cout << "e11: " << e11 << endl;

	Operator e12 = Sub(Const(5), Const(3));
	cout << "e12: " << e12 << endl;

	cout << "e8: ";
	e8.pre();

	Operator e13 = Multi(Const(5), Const(9));
	cout << "e13: " << e13 << endl;

	Operator e14 = Multi(Const(5), e3);
	cout << "e14: " << e14 << endl;
	
	Operator e15 = Multi(e3, Const(5));
	cout << "e15: " << e15 << endl;

	Operator e16 = Multi(v, Const(3));
	cout << "e16: " << e16 << endl;

	Operator e17 = Multi(Const(5), v);
	cout << "e17: " << e17 << endl;

	Operator e18 = Multi(v, e3);
	cout << "e18: " << e18 << endl;

	Operator e19 = Multi(e3, v);
	cout << "e19: " << e19 << endl;

	Operator e20 = Multi(e3, e3);
	cout << "e20: " << e20 << endl;

	Operator e21 = Multi(v, v);
	cout << "e21: " << e21 << endl;

	Operator e22 = Diff(e20);
	cout << "e22: " << e22 << endl;

	Operator e23 = Diff(Const(5));
	cout << "e23: " << e23 << endl;
	
	Operator e24 = Diff(v);
	cout << "e24: " << e24 << endl;

	Operator e25 = Integ(e20);
	cout << "e25: " << e25 << endl;

	cout << "e20: x=" << e20.solve(10e-16) << endl;


	cout << "when x=1 the result of Polynomial(e20) is: " << e11.result(1) << endl;

	cout << "the x of e11 can be solved as: ";
	cout << e11.function("y") << endl;

	Operator e26 = Cos(e11);
	cout << "e26: " << e26 << endl;

	cout << "when x=1 the result of Polynomial(e26) is: " << cos(e26.result(1)) << endl;

	Operator e27 = Sin(e11);
	cout << "e27: " << e27 << endl;

	cout << "when x=1 the result of Polynomial(e27) is: " << sin(e27.result(1)) << endl;

	Operator e28 = Div(e11, e20);
	cout << "e28: " << e28 << endl;


	return 0;
}
