

#include<stdio.h>
#include<math.h>
#include<graphics.h>

using namespace std;


class polygon
{
	float p[10][10], m , n;  // p is the size of polygon  // m is number of vertices // n is no. of columns
public:
	void getpoly();
	void drawpoly();
	void displaypoly();
	polygon mat_mul(polygon);
	polygon operator+(polygon);		// Translation
	polygon operator*(polygon);		// Scaling
	polygon operator/(polygon);		// Clockwise rotation
	polygon operator-(polygon);		// Anticlockwise rotation
};

void polygon :: getpoly()
{
	int i,j;
	cout<<"Enter no of vertices :";
	cin>>m;
	n=3;                     // n is 3 means ,there are 3 columns in matrix that is 3rd column is dummy which is used in homogeneous cordinate nd its value is 1
	cout<<"Enter polygon coordinates";
	for(i=0;i<m;i++)    // i to get the elements of row
	{
		cin>>p[i][0]>>p[i][1];    // p[i][0]  p[i][1] p[i][2]
		p[i][2]=1;           //  3rd column is dummy which is used in homogeneous cordinate nd its value is 1
	}

}

void polygon :: displaypoly()
{
	int i,j;
	cout<<"The polygon is as follows "<<endl;
	for(i=0;i<m;i++)         // i is to get elements of rows
	{
		for(j=0;j<n;j++)    // j is used to get the elements of column
		{
			cout<<p[i][j]<<"  ";   //p[i][j=0]
		                               // p[i][j=1]
                                               // p[i][j=2]   // p is the acutal matrix of polygon                                             
}
		cout<<endl;
	}
}

void polygon :: drawpoly()
{
int i;
line(320,2,320,438); // used to draw x and y coordinate
line(2,240,638,240); // as 0,0 starts from top left corner of screen, // these line segments are used to show proper 2D transf.
                     // 320 2                   // 2   240
                     // 320 438                 // 638 240
for( i=0;i<m-1;i++)           // Display polgon.i.e starting point and ending point of polygon is same
    line(p[i][0]+320,240-p[i][1],p[i+1][0]+320,240-p[i+1][1]);    // p[i][0]+320       240-p[i][1],
line(p[0][0]+320,240-p[0][1],+320+p[i][0],240-p[i][1]);           // p[i+1][0]+320     240-p[i+1][1]
}

polygon polygon :: mat_mul(polygon p2)             // Matrix multiplication.
{
	int i,j,k;
	polygon p3;  

	p3.m=this->m;   //current value of m is same for p3
    p3.n=p2.n;      // value of columns in p2 is same for p3
	cout<<endl;
	for(i=0;i<p3.m;i++)   // no. of vertices
		for(j=0;j<3;j++)  // no. of columns (n=3)
		{
			p3.p[i][j]=0;
			for(k=0;k<3;k++)      
				p3.p[i][j]+=this->p[i][k]*p2.p[k][j];
		}

	 return p3;
}

polygon polygon :: operator+(polygon p2)
{
	polygon p3;
	int tx, ty; //translation factor
	p3.m=this->m;
	p3.n=3;

	cout<<"Enter Translation factors : ";      // tx, ty
	cin>>tx>>ty ;
	p2.p[0][0]=1; p2.p[0][1]=0; p2.p[0][2]=0;            // Translation matrix. p[0][0]=1  p[0][1]=0   p[0][2]=0
	p2.p[1][0]=0; p2.p[1][1]=1; p2.p[1][2]=0;            //                     p[1][0]=0  p[1][1]=1   p[1][2]=0
	p2.p[2][0]=tx; p2.p[2][1]=ty; p2.p[2][2]=1;          //                     p[2][0]=tx p[2][1]=ty  p[2][2]=1

	
// p.p2= p3  p is original matrix, p2 is translation matrix and p3 is final translated matrix

        p3=this->mat_mul(p2);
	return p3;
}


polygon polygon :: operator*(polygon p2)
{
	polygon p3;
	int sx, sy;
	p3.m=this->m;
	p3.n=3;

	cout<<"Enter Scaling factors : ";  
	cin>>sx>>sy ;  
	p2.p[0][0]=sx; p2.p[0][1]=0; p2.p[0][2]=0;            // Scaling matrix.  sx    0    0
	p2.p[1][0]=0; p2.p[1][1]=sy; p2.p[1][2]=0;            //                  0    sy    0
	p2.p[2][0]=0; p2.p[2][1]=0; p2.p[2][2]=1;             //                  0     0    1

	p3=this->mat_mul(p2);
	return p3;
}

polygon polygon :: operator/(polygon p2)
{
	polygon p3;
	float ang, radian;

	p3.m=this->m;
	p3.n=3;

	cout<<"   Angle : ";
	cin>>ang;
	radian=ang*(3.14/180);

	p2.p[0][0]=cos(radian); p2.p[0][1]=-sin(radian); p2.p[0][2]=0;         // Rotation matrix.  cos   -sin	 0
	p2.p[1][0]=sin(radian); p2.p[1][1]=cos(radian); p2.p[1][2]=0;          //                   sin    cos   0
	p2.p[2][0]=0; p2.p[2][1]=0; p2.p[2][2]=1;                              //                    0      0    1

	p3=this->mat_mul(p2);
	return p3;
}


polygon polygon :: operator-(polygon p2)
{
	polygon p3;
	float ang, radian;

	p3.m=this->m;
	p3.n=3;

	cout<<"   Angle : ";
	cin>>ang;
	radian=ang*(3.14/180);

	p2.p[0][0]=cos(radian); p2.p[0][1]=sin(radian); p2.p[0][2]=0;            // Rotation matrix.  cos  sin  0
	p2.p[1][0]=-sin(radian); p2.p[1][1]=cos(radian); p2.p[1][2]=0;           //                  -sin  cos  0
	p2.p[2][0]=0; p2.p[2][1]=0; p2.p[2][2]=1;                                //                    0   0    1

	p3=this->mat_mul(p2);
	return p3;
}

int main()
{
	polygon p1, p2, p3;
	int gd=DETECT,gm, sw;


	p1.getpoly();
	p1.displaypoly();
	initgraph(&gd,&gm,NULL) ;
	p1.drawpoly();

	do{
			cout<<"\t\t **** MENU ****";
			cout<<"\n1. TRANSLATION";
			cout<<"\n2. SCALING";
			cout<<"\n3. CLOCKWISE ROTATION";
			cout<<"\n4. ANTICLOKWISE ROTATION";
			cout<<"\nPlease! Enter your choice : ";
			cin>>sw;
			switch(sw)
			{
			  case 1 :

					p3=p1+p2; // Translation
					p3.displaypoly();
					p3.drawpoly();

				    break ;
			  case 2 :
					p3=p1*p2; // Scaling
					p3.displaypoly();
					p3.drawpoly();

				    break;
			  case 3 :
					p3=p1/p2; // Clockwise rotation
					p3.displaypoly();
					p3.drawpoly();
				    break ;
			  case 4 :
					p3=p1-p2; // Anticlockwise rotation
					p3.displaypoly();
					p3.drawpoly();
				    break;
			}
			}while(sw!=5);

	getch();

	return 0;
}

