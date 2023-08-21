## Bresenhams Algorithm

Bresenham’s line algorithm is a simple and efficient algorithm for drawing lines on an image. It uses only integer addition, subtraction, and bit shifting. In other words, only very cheap operations. The algorithm is based on the observation that the slope of a line between two points can be expressed as a ratio of the change in the y-coordinate to the change in the x-coordinate. This ratio is known as the slope of the line.

Bresenham’s line algorithm works by incrementally plotting the pixels that are closest to the line, one at a time. The algorithm computes the coordinates of the successive points by incrementing the x and y coordinates. An error term is computed at each step to determine if x or y should be incremented. Hence, the line can be drawn without expensive division or multiplication operations. By repeating a simple process, the algorithm can draw a smooth and accurate line.

### source code:
```cpp
#include<graphics.h>

void BLA(int x1,int y1,int x2,int y2){
    int dx=abs(x2-x1), dy=abs(y2-y1);
    int x=x1,y=y1,p;
    int ix=x2>x1 ? 1 : -1 , iy=y2>y1 ? 1 : -1;
    if(dx>dy){
        p=2*dy-dx;
        for(int i=0;i<=dx;i++){
            x+=ix;
            if(p<0) p+=2*dy;
            else{
                y+=iy;
                p+=2*(dy-dx);
            }
            putpixel(x,y,(i/30)%10+1);
            delay(10);
        }
    }
    else{
        p=2*dx-dy;
        for(int i=0;i<=dy;i++){
            y+=iy;
            if(p<0) p+=2*dx;
            else{
                x+=ix;
                p+=2*(dx-dy);
            }
            putpixel(x,y,(i/30)%10+1);
            delay(10);
        } 
    }
}

int main(){
    int gd=DETECT,gm;
    initgraph(&gd,&gm,(char*)"");
    BLA(200,43,124,300);
    getch();
    closegraph();
}
```

### Output:
<video width="320" height="240" controls>
  <source src="src/video/bla.mp4" type="video/mp4">
</video>

## DDA Algorithm
DDA (Digital Differential Analyzer) is a line drawing algorithm used in computer graphics to generate a line segment between two specified endpoints. It is a simple and efficient algorithm that works by using the incremental difference between the x-coordinates and y-coordinates of the two endpoints to plot the line.

The steps involved in DDA line generation algorithm are:

Input the two endpoints of the line segment, (x1,y1) and (x2,y2).
Calculate the difference between the x-coordinates and y-coordinates of the endpoints as dx and dy respectively.
Calculate the slope of the line as m = dy/dx.
Set the initial point of the line as (x1,y1).
Loop through the x-coordinates of the line, incrementing by one each time, and calculate the corresponding y-coordinate using the equation y = y1 + m(x – x1).
Plot the pixel at the calculated (x,y) coordinate.
Repeat steps 5 and 6 until the endpoint (x2,y2) is reached.
DDA algorithm is relatively easy to implement and is computationally efficient, making it suitable for real-time applications. However, it has some limitations, such as the inability to handle vertical lines and the need for floating-point arithmetic, which can be slow on some systems. Nonetheless, it remains a popular choice for generating lines in computer graphics.

### source code:
```cpp
#include<graphics.h>

void DDA(int x1,int y1,int x2,int y2){
    int dy=y2-y1,dx=x2-x1;
    int ss=abs(dx)>abs(dy) ? abs(dx) : abs(dy);
    float x=x1,y=y1,ix=float(dx)/ss,iy=float(dy)/ss;
    for(int i=0;i<=ss;i++){
        putpixel(x,y,(i/30)%10+1);
        x+=ix;
        y+=iy;
        delay(10);
    }
}

int main(){
    int gd=DETECT,gm;
    initgraph(&gd,&gm,(char*)"");
    DDA(620,10,50,300);
    getch();
    closegraph();
}
```
### Output:
<video width="320" height="240" controls>
  <source src="src/video/dda.mp4" type="video/mp4">
</video>

## Mid-point Circle Algorithm
In computer graphics, the mid-point circle drawing algorithm is used to calculate all the perimeter points of a circle. In this algorithm, the mid-point between the two pixels is calculated which helps in calculating the decision parameter.

The value of the decision parameter will decide which pixel should be chosen for drawing the circle.

This algorithm only calculates the points for one octant and the points for other octants are generated using the eight-way symmetry for the circle.

Working of the Mid-Point Circle Drawing Algorithm
The algorithm works in the following way:

Suppose a mid-point with coordinates (x', y') is put in the general equation of the circle is

    x2 + y2 - r2 = 0
gives these three values,

If 0 : the given point lies on the circle boundary, then any pixel can be chosen.
If < 0 : the given point lies inside the circle boundary, then the upper pixel can be chosen.
If > 0 : the given point lies outside the circle boundary, then the lower pixel is chosen.
Let there be two pixels:

One pixel which is outside (A), and the other pixel which is inside (B) the circle boundary,

Let their coordinates be:

    For A: (xk+1, yk) 
    and
    For B: (xk+1, yk-1)
Derivation

Then, the co-ordinates of mid-point be

MP= [((xk+1+xk+1)/2) , ((yk+yk-1) /2)]

(xk +1 , yk-1 / 2)

Now, put MP in the equation of circle x2 + y2 - r2 = 0

(xk +1 )2 + (yk-1 / 2)2 - r2

Let us define this equation as the decision parameter, using the mid-point MP:

Pk = ( xk+1 )2 + ( yk-1 / 2 )2 - r2                                                              -----(1)

 Let us define the successive decision parameter, Pk+1:

Pk+1 = ( xk+1 + 1 )2 + ( yk+1-1 / 2 )2 - r2                                                  -----(2)

Subtracting eq.(1) from eq.(2);

Pk+1- Pk = ( xk+1+1 )2 + ( yk+1-1 / 2 )2- r2 - [ (xk+1)2 + ( yk-1 /2 )2 - r2]

Now put xk+1 = xk+1

             = (xk+1 + 1)2 + (yk+1-1 / 2)2 - r2 - [ (xk+1)2 + (yk-1 / 2)2 - r2 ]

             = (xk+1 + 1)2 - (xk+1)2 + ( yk+1-1 / 2 )2- ( yk-1 / 2 )2

 Pk+1= Pk + 2xk +3 + (yk+1)2 – yk+1- (yk)2 + yk

If Pk < 0:           yk+1 =  yk                (choose point A)

Pk+1= Pk+ 2xk + 3

If Pk > 0:           yk+1 = yk - 1     (choose point B)                                                      

Pk+1 = Pk + 2xk + 2yk + 5

Let us calculate the initial decision parameter (P0) where the initial points will be defined as (0, r) [which is the first point to be plotted of the first octant].

On putting these coordinates in eq. (1) in place of xk and yk, we get:

P0= ( 0 + 1 )2 + ( r-1 / 2 )2 - r2
P0 = 5/4 - r

If r is an integer:
P0 = 1 - r

If r is a floating point:
P0 = 5/4 - r

The Mid-Point Circle Drawing Algorithm
Step 1: Start.

Step 2: Declare x, y, r, xc , yc , P as variables, where (xc , yc) are coordinates of the center.

Step 3: Put x = 0 and y = r

Step 4: Repeat the steps while x ≤ y;

Step 5: Plot (x, y).

Step 6: if (P < 0):

                 Set P = P + 2x + 3

             else if (P >= 0):

                  Set P = P + 2(x-y) + 5

                   y = y - 1

 Step 7: Do x = x + 1

Step 8: End

Formulas used:

For Pk < 0 :
Pk+1=Pk+ 2xk+3

For Pk ≥ 0 :
Pk + 2xk+2yk+5

Advantages:

The mid-point circle algorithm is an efficient algorithm in drawing a circle.
The implementation of the algorithm is easy from the programmer’s point of view.
Disadvantages:

The time consumption of this algorithm is high.

### source code:
```cpp
#include<graphics.h>

void MPC(int h,int k,int r){
    int y=r,p=1-r,j;
    int c[4][2]={{1,1},{1,-1},{-1,1},{-1,-1}};
    for(int x=0;x<=y;x++){
        if(p<0) p+=2*x+1;
        else{
            y--;
            p+=2*(x-y)+1;
        }
        j=1;
        for(int* i:c){
            putpixel(h+x*i[0],k+y*i[1],j);
            putpixel(h+y*i[0],k+x*i[1],j+4);
            j++;
        }
        delay(20);
    }
}

int main(){
    int gd=DETECT,gm;
    initgraph(&gd,&gm,(char*)"");
    MPC(150,120,100);
    getch();
    closegraph();
}
```
### Output:
<video width="320" height="240" controls>
  <source src="src/video/cir.mp4" type="video/mp4">
</video>

## Mid-point Ellipse Algorithm
In computer graphics, the mid-point ellipse algorithm is an incremental method of drawing an ellipse. It is very similar to the mid-point algorithm used in the generation of a circle.

The mid-point ellipse drawing algorithm is used to calculate all the perimeter points of an ellipse. In this algorithm, the mid-point between the two pixels is calculated which helps in calculating the decision parameter. The value of the decision parameter determines whether the mid-point lies inside, outside, or on the ellipse boundary and the then position of the mid-point helps in drawing the ellipse.

Working of the Mid-Point Ellipse Drawing Algorithm
Now, for understanding the proper working of the algorithm, let us consider the elliptical curve in the first quadrant.

Also, consider the equation of ellipse:

    ry2 x2 + rx2 y2 - rx2 ry2 = 0             ...(1)
    where, ry : semi-minor axis
    rx : semi-major axis
In the ellipse each quadrant is divided into two regions: R1 and R2 respectively.

We know that the slope of an ellipse is given by:

    m =dx / dy = - (2 ry2 x /2 rx2 y ) = -1
The region R1 has the value of slope as m<-1 while R2 has the value of slope as m > -1.

The starting point for R1 will be (0, ry) and for R2, the initial points will become the ending points of R1, where the slope becomes greater than -1. Thats why we need to keep in check the value of slope while plotting the points for R1 to know whether we have reached R2 or not.

We will do the derivation for each region:

Derivation for Region 1:

Let us consider two pixels: one which is outside(A) and the other which is inside(B) respectively.

Let their coordinates be:

For A; ( xk+1, yk )

For B; ( xk+1, yk-1 )

Value for their mid-point will be:

Mp= [ ( (xk+1 + xk+1) / 2 ) ,( (yk + yk-1) / 2) ]

     = ( xk+1 , yk-1 / 2 )

Putting Mp in eqn (1):

ry2 (xk+1)2 + rx2 ( yk-1 / 2 )2 - rx2 ry2

Let us define this statement as our decision variable:

Pk= ry2 ( xk+1 )2 + rx2 ( yk-1 / 2 )2 - rx2 ry2                       -----(2)

Successive parameter can be defined as:

Pk+1­ = ry2 ( xk+1+1 )2 + rx2 ( yk+1-1 / 2 )2- rx2 ry2               -----(3)

Now, subtract eqn (2) from eqn (3);

Pk+1 ­- Pk = [ ry2 ( xk+1 + 1 )2 + rx2 ( yk+1-1 / 2 )2- rx2 ry2 ] - [ ry2 (xk+1)2+ rx2( yk-1 / 2 )2- rx2 ry2 ]                                                                      

= ry2 [ (xk+1+1)2 - (xk+1)2 ] + rx2 [ ( yk+1-1 / 2 )2 - ( yk-1 / 2 )2 ]

As the x-coordinate remains same in both the pixels, put xk+1 = xk+1

Pk+1­- Pk = ry2 [ (xk+1+1)2 - (xk+1)2 ] + rx2 [ ( yk+1-1 / 2 )2 - ( yk-1 / 2)2 ]

= ry2 [ 2xk + 3 ] + rx2 [ (yk+1)2 - yk+1 - (yk)2 + yk ]

Pk+1 =­ Pk + ry2 [ 2xk + 3 ] + rx2 [ (yk+1)2 - yk+1 - (yk)2 + yk ]

If Pk < 0 :    use yk+1 = yk                  (Choose point A)

Pk+1 =­ Pk+ ry2 [ 2xk + 3 ]

If Pk ≥ 0:    use yk+1 = yk -1                (Choose point B)

Pk+1 =­ Pk + ry2 [ 2xk + 3 ] + 2 [ 1 - yk ]

Initial Decision Parameter

Put initial point(0,ry) in eq.(2)

P0= ry2 (0+1)2 + rx2( ry-1 /2)2 - rx2 ry2
P0 = ry2 + rx2/4 - ry rx2

Derivation for Region 2:

Let us consider two pixels: one which is outside(P) and the other which is inside(Q) respectively.

Let their coordinates be:

For P; (xk+1, yk-1)

For Q; (xk, yk-1)

Value for their mid-point will be:

Mp= [ ( (xk+1+ xk) / 2 ), ( (yk-1 yk-1) / 2 ) ]

     = ( xk+1 / 2 , yk-1 )

Putting Mp in eq.(1):

ry2 (xk+1 / 2)2 + rx2 (yk-1)2- rx2 ry2

Let us define this statement as our decision variable:

P2k = ry2 ( xk+1 /2)2 + rx2 (yk-1)2 - rx2 ry2                                ----(4)

Successive parameter can be defined as:

P2k+1­= ry2 ( xk+1 + 1 / 2 )2 + rx2 ( yk+1-1 )2 - rx2 ry2                    ----(5)

Now, subtract eq.(4) from eq.(5);

P2k+1­- P2k = [ ry2 (xk+1+1/2)2 + rx2 (yk+1-1)2- rx2ry2 ] - [ ry2 (xk+1/2)2+ rx2 (yk-1)2 - rx2 ry2]                                                                      

= ry2 [ (xk+1+1 / 2 )2 - ( xk+1 / 2 )2 ] + rx2 [ (yk+1-1)2 - (yk-1)2]

 As the y-coordinate remains same in both the pixels, put yk+1 = yk-1

P2k+1­- P2k = ry2 [ (xk+1)2 + xk+1 - (xk)2 - xk ]+ rx2 [ 3 - 2yk ]

P2k+1 =­ P2k + ry2 [ (xk+1)2 + xk+1 - (xk)2 - xk ]+ rx2 [ 3 - 2yk ]

If P2k > 0:    use xk+1= xk                  (Choose point Q)

P2k+1 =­ P2k - 2 yk+1 rx2 + rx2

If P2k ≤ 0:    use xk+1 = xk +1                (Choose point P)

P2k+1 =­ P 2k+ 2ry2 [2 xk+1] - 2 yk+1 rx2 + rx2

Initial Decision Parameter:

Putting the ending point of region R1

Where, m > -1 i.e. (x, y) in eq.(4)

P20 = ry2 ( x+1 / 2 )2 + rx2 (y-1)2 - rx2 ry2

Algorithm
Step 1: Start

Step 2: Declare rx , ry , x , y , m , dx , dy , P , P2.

Step 3: Initialize initial point of region1 as

                 x=0  ,   y = ry

Step 4: Calculate P= ry2 + rx2 / 4 - ry rx2  

                               dx = 2 ry2 x

                               dy = 2 rx2 y        

Step 5: Update values of dx and dy after each iteration.

Step 6: Repeat steps while (dx < dy):

            Plot (x,y)

             if(P < 0)

             Update x = x+1 ;

             P += ­ ry2 [2x + 3 ]

             Else

              Update x = x + 1

                           y= y - 1

Step 7: When dx ≥ dy, plot region 2:

Step 8: Calculate P2 = ry2 ( x+1 / 2)2 + rx2 (y -1)2- rx2ry2

Step 9: Repeat till (y > 0)

               If (P2 > 0)

                Update y = y-1        (x will remain same)

                              P2 =­ P2 -2 y rx2 + rx2

                 else

                 x = x+1

                 y = y-1

                 P2=­ P2+ 2 ry2 [2x] -2 y rx2 + rx2

Step 10: End

Advantages:

The mid-point ellipse algorithm has simple and easy implementation as it only includes addition operations.
Disadvantages:

This algorithm is time consuming.

### source code:
```cpp
#include<graphics.h>

void MPE(int h,int k,int a,int b){
    int a2=a*a,b2=b*b;
    int c[4][2]={{1,1},{1,-1},{-1,1},{-1,-1}},j;
    int x=0,y=b,p=b2-b*a2+a2/4;
    int px=0 ,py=2*a2*b;
    while(px<py){
        j=1;
        for(int* i:c) {
            putpixel(h+x*i[0],k+y*i[1],j);
            j++;
        }
        x++;
        px+=2*b2;
        p+=b2+px;
        if(p>=0){
            y--;
            py-=2*a2;
            p-=py;
        }
        delay(20);
    }
    x=a,y=0,p=a2-a*b2+b2/4;
    py=0 ,px=2*b2*a;
    while(py<px){
        j=5;
        for(int* i:c) {
            putpixel(h+x*i[0],k+y*i[1],j);
            j++;
        }
        y++;
        py+=2*a2;
        p+=a2+py;
        if(p>=0){
            x--;
            px-=2*b2;
            p-=px;
        }
        delay(20);
    }
}

int main(){
    int gd=DETECT,gm;
    initgraph(&gd,&gm,(char*)"");
    MPE(320,200,100,150);
    getch();
    closegraph();
}
```
### Output:
<video width="320" height="240" controls>
  <source src="src/video/ell.mp4" type="video/mp4">
</video>

## Cohen Sutherland Line Clipping Algorithm
In this algorithm, we are given 9 regions on the screen. Out of which one region is of the window and the rest 8 regions are around it given by 4 digit binary.  The division of the regions are based on (x_max, y_max) and (x_min, y_min).

The central part is the viewing region or window, all the lines which lie within this region are completely visible. A region code is always assigned to the endpoints of the given line.

To check whether the line is visible or not.

Formula to check binary digits:- TBRL which can be defined as top, bottom, right, and left accordingly.                                                                                              
Algorithm
Steps:

1) Assign the region codes to both endpoints.

2) Perform OR operation on both of these endpoints.

3) if  OR = 0000,

            then it is completely visible (inside the window).

   else

           Perform AND operation on both these endpoints.

                      i) if  AND ≠ 0000,

                                  then the line is invisible and not inside the window. Also, it can’t be considered for clipping.

                     ii) else

                                 AND = 0000, the line is partially inside the window and considered for clipping.

4)  After confirming that the line is partially inside the window, then we find the intersection with the boundary of the window. By using the following formula:-

                                       Slope:- m= (y2-y1)/(x2-x1)

     a) If the line passes through top or the line intersects with the top boundary of the window.

                                           x = x + (y_wmax – y)/m

                                           y = y_wmax

     b) If the line passes through the bottom or the line intersects with the bottom boundary of the window.

                                          x = x + (y_wmin – y)/m

                                          y = y_wmin

     c) If the line passes through the left region or the line intersects with the left boundary of the window.

                                          y = y+ (x_wmin – x)*m

                                          x = x_wmin

     d) If the line passes through the right region or the line intersects with the right boundary of the window.

                                         y = y + (x_wmax -x)*m

                                         x = x_wmax  

5) Now, overwrite the endpoints with a new one and update it.

6) Repeat the 4th step till your line doesn’t get completely clipped

Given a set of lines and a rectangular area of interest, the task is to remove lines that are outside the area of interest and clip the lines which are partially inside the area.

Input : Rectangular area of interest (Defined by
        below four values which are coordinates of
        bottom left and top right)
        x_min = 4, y_min = 4, x_max = 10, y_max = 8
       
        A set of lines (Defined by two corner coordinates)
        line 1 : x1 = 5, y1 = 5, x2 = 7, y2 = 7
        Line 2 : x1 = 7, y1 = 9, x2 = 11, y2 = 4
        Line 2 : x1 = 1, y1 = 5, x2 = 4, y2 = 1 

Output : Line 1 : Accepted from (5, 5) to (7, 7)
         Line 2 : Accepted from (7.8, 8) to (10, 5.25)
         Line 3 : Rejected
Cohen-Sutherland algorithm divides a two-dimensional space into 9 regions and then efficiently determines the lines and portions of lines that are inside the given rectangular area.

The algorithm can be outlines as follows:- 

Nine regions are created, eight "outside" regions and one 
"inside" region.

For a given line extreme point (x, y), we can quickly
find its region's four bit code. Four bit code can 
be computed by comparing x and y with four values 
(x_min, x_max, y_min and y_max).

If x is less than x_min then bit number 1 is set.
If x is greater than x_max then bit number 2 is set.
If y is less than y_min then bit number 3 is set.
If y is greater than y_max then bit number 4 is set
Cohen-Sutherland algorithm outline

There are three possible cases for any given line. 

Completely inside the given rectangle : Bitwise OR of region of two end points of line is 0 (Both points are inside the rectangle)
Completely outside the given rectangle : Both endpoints share at least one outside region which implies that the line does not cross the visible region. (bitwise AND of endpoints != 0).
Partially inside the window : Both endpoints are in different regions. In this case, the algorithm finds one of the two points that is outside the rectangular region. The intersection of the line from outside point and rectangular window becomes new corner point and the algorithm repeats.

### source code:
```cpp
#include<graphics.h>

void CSL_clipping(int x[],int y[]){
    int b[2]={0,0};
    for(int i=0;i<2;i++){
        if(x[i]<80) b[i]+=8;
        if(x[i]>560) b[i]+=4;
        if(y[i]<60) b[i]+=2;
        if(y[i]>420) b[i]+=1;
    }  
    // both points lies is same side out of screen
    if(b[0]&b[1]) {
        int x1[2]={0,0},y1[2]={0,0};
        x=x1;
        y=y1;   
        return;
    } 
    
    float m=(float)(y[1]-y[0])/(x[1]-x[0]);
    while(b[0]|b[1]){
        for(int i=0;i<2;i++){
            if(b[i]&1) { 
                x[i]+=(420-y[i])/m;
                y[i]=420;
            }
            else if(b[i]&2) { 
                x[i]+=(60-y[i])/m;
                y[i]=60;
            }
            else if(b[i]&4) { 
                y[i]+=m*(560-x[i]);
                x[i]=560;
            }
            else if(b[i]&8) { 
                y[i]+=m*(80-x[i]);
                x[i]=80;
            }
        }  
        for(int i=0;i<2;i++) b[i]=0;
        for(int i=0;i<2;i++){
            if(x[i]<80) b[i]+=8;
            if(x[i]>560) b[i]+=4;
            if(y[i]<60) b[i]+=2;
            if(y[i]>420) b[i]+=1;
        }
    }      
}

int main(){
    int gd=DETECT,gm;
    initgraph(&gd,&gm,(char *)"");

    int x[2]={13,430},y[2]={21,430};
    rectangle(80,60,560,420);

    setcolor(GREEN);
    line(x[0],480-y[0],x[1],480-y[1]);
    getch();

    CSL_clipping(x,y);

    cleardevice();
    setcolor(WHITE);
    rectangle(80,60,560,420);
    setcolor(BLUE);
    line(x[0],480-y[0],x[1],480-y[1]);
    
    getch();
    closegraph();
}
```
### Output:
<video width="320" height="240" controls>
  <source src="src/video/ell.mp4" type="video/mp4">
</video>

##  Liang Barsky Line Clipping Algorithm
The Liang-Barsky algorithm is a line clipping algorithm. This algorithm is more efficient than Cohen–Sutherland line clipping algorithm and can be extended to 3-Dimensional clipping. This algorithm is considered to be the faster parametric line-clipping algorithm. The following concepts are used in this clipping:

The parametric equation of the line.
The inequalities describing the range of the clipping window which is used to determine the intersections between the line and the clip window.
The parametric equation of a line can be given by,

X = x1 + t(x2-x1)
Y = y1 + t(y2-y1)
Where, t is between 0 and 1.

Then, writing the point-clipping conditions in the parametric form:

xwmin <= x1 + t(x2-x1) <= xwmax
ywmin <= y1 + t(y2-y1) <= ywmax 
The above 4 inequalities can be expressed as,

tpk <= qk 
Where k = 1, 2, 3, 4 (correspond to the left, right, bottom, and top boundaries, respectively).

The p and q are defined as,

p1 = -(x2-x1),  q1 = x1 - xwmin (Left Boundary) 
p2 =  (x2-x1),  q2 = xwmax - x1 (Right Boundary)
p3 = -(y2-y1),  q3 = y1 - ywmin (Bottom Boundary) 
p4 =  (y2-y1),  q4 = ywmax - y1 (Top Boundary)  
When the line is parallel to a view window boundary, the p value for that boundary is zero.
When pk < 0, as t increase line goes from the outside to inside (entering).
When pk > 0, line goes from inside to outside (exiting).
When pk = 0 and qk < 0 then line is trivially invisible because it is outside view window.
When pk = 0 and qk > 0 then the line is inside the corresponding window boundary.

Using the following conditions, the position of line can be determined:

Condition	Position of line
pk = 0	parallel to the clipping boundaries
pk = 0 and qk < 0	completely outside the boundary
pk = 0 and qk >= 0	inside the parallel clipping boundary
pk < 0	line proceeds from outside to inside
pk > 0	line proceeds from inside to outside
Parameters t1 and t2 can be calculated that define the part of line that lies within the clip rectangle.
When,

pk < 0, maximum(0, qk/pk) is taken.
pk > 0, minimum(1, qk/pk) is taken.
If t1 > t2, the line is completely outside the clip window and it can be rejected. Otherwise, the endpoints of the clipped line are calculated from the two values of parameter t.

Algorithm –

Set tmin=0, tmax=1.
Calculate the values of t (t(left), t(right), t(top), t(bottom)),
(i) If t < tmin ignore that and move to the next edge.
(ii) else separate the t values as entering or exiting values using the inner product.
(iii) If t is entering value, set tmin = t; if t is existing value, set tmax = t.
If tmin < tmax, draw a line from (x1 + tmin(x2-x1), y1 + tmin(y2-y1)) to (x1 + tmax(x2-x1), y1 + tmax(y2-y1))
If the line crosses over the window, (x1 + tmin(x2-x1), y1 + tmin(y2-y1)) and (x1 + tmax(x2-x1), y1 + tmax(y2-y1)) are the intersection point of line and edge.

### source code:
```cpp
#include <graphics.h>

void LiangBarsky(int x1, int y1, int x2, int y2, int xmin, int ymin, int xmax, int ymax) {
    int dx = x2 - x1;
    int dy = y2 - y1;
    float p[4], q[4];

    p[0] = -dx;
    p[1] = dx;
    p[2] = -dy;
    p[3] = dy;

    q[0] = x1 - xmin;
    q[1] = xmax - x1;
    q[2] = y1 - ymin;
    q[3] = ymax - y1;

    float u1 = 0, u2 = 1;

    for (int i = 0; i < 4; i++) {
        if (p[i] == 0 && q[i] < 0) {
            // The line is parallel to the clipping boundary and outside
            return;
        }

        float u = q[i] / p[i];

        if (p[i] < 0) {
            u1 = (u > u1) ? u : u1;
        } else if (p[i] > 0) {
            u2 = (u < u2) ? u : u2;
        }
    }

    if (u1 > u2) {
        // Line is completely outside the clipping window
        return;
    }

    int new_x1 = x1 + u1 * dx;
    int new_y1 = y1 + u1 * dy;
    int new_x2 = x1 + u2 * dx;
    int new_y2 = y1 + u2 * dy;

    line(new_x1, new_y1, new_x2, new_y2);
}

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, (char *)"");

    int xmin = 100, ymin = 80, xmax = 580, ymax = 440;
    int x1 = 80, y1 = 431, x2 = 600, y2 = 20;

    rectangle(xmin, ymin, xmax, ymax);
    setcolor(GREEN);
    line(x1, 480 - y1, x2, 480 - y2);
    getch();

    cleardevice();
    setcolor(WHITE);
    rectangle(xmin, ymin, xmax, ymax);
    setcolor(BLUE);

    LiangBarsky(x1, 480 - y1, x2, 480 - y2, xmin, ymin, xmax, ymax);
    
    getch();
    closegraph();
}
```
##  2D Transformations
2D transformations are a fundamental concept in computer graphics and geometry that involve altering the position, size, orientation, or shear of objects in a two-dimensional (2D) space. These transformations are commonly used to manipulate and manipulate images, shapes, or graphical elements in various applications, including computer graphics, video games, and design software. Here are some of the most common 2D transformations:

Translation: Translation involves moving an object from one location to another in a 2D plane. It is defined by a pair of values (tx, ty), where tx represents the horizontal shift and ty represents the vertical shift. The new position of a point (x, y) after translation is (x + tx, y + ty).

Scaling: Scaling changes the size of an object in the x and y directions. It is defined by scaling factors (sx, sy). If sx and sy are both greater than 1, the object grows (enlargement); if they are both less than 1, the object shrinks (reduction). Scaling can also involve non-uniform scaling when sx and sy are different.

Rotation: Rotation involves rotating an object by a specified angle θ around a fixed point (usually the origin). The new coordinates (x', y') of a point (x, y) after rotation are calculated using trigonometric functions:
   x' = x * cos(θ) - y * sin(θ)
   y' = x * sin(θ) + y * cos(θ)

Reflection: Reflection flips an object over a specified axis, such as the x-axis or y-axis. Reflection can also be achieved over an arbitrary line by a combination of translation and rotation.

Shearing: Shearing is a transformation that distorts the shape of an object by skewing it in one or both directions. There are two types of shearing: horizontal (x-shear) and vertical (y-shear).

Composite Transformations: Multiple transformations can be combined to create complex effects. The order in which transformations are applied can affect the final result, as transformations are not commutative.

Homogeneous Coordinates: In computer graphics, transformations are often represented using homogeneous coordinates, which extend 2D coordinates to 3D by adding a third coordinate (usually denoted as w). This allows for easier handling of translations and perspective transformations.

Matrix Representation: 2D transformations can be represented using transformation matrices. For example, a translation can be represented as a 3x3 matrix, where the translation values are in the rightmost column.

### source code:
```cpp
#include<graphics.h>
#include<cmath>

void get_xy_matrix(int x[],int y[],int xy[][4]){
    for(int i=0;i<4;i++){
        xy[0][i]=x[i];
        xy[1][i]=y[i];
        xy[2][i]=1;
    }
}

void mat_mul(float a[][3],int b[][4],int res[][4]){
    for(int i=0;i<3;i++)
        for(int j=0;j<4;j++){
            res[i][j]=0;
            for(int k=0;k<3;k++) res[i][j]+=round(a[i][k]*b[k][j]);
        }
}

void display(int r[][4]){
    for(int j=0;j<4;j++) line(320+r[0][j],240-r[1][j],320+r[0][(j+1)%4],240-r[1][(j+1)%4]);
}

void clearscreen(){
    cleardevice();
    setcolor(WHITE);
    line(0,240,640,240);
    line(320,0,320,480);
    setcolor(GREEN);
}

void translate(int res[][4],int xy_mat[][4],float h,float k){
    float t_mat[3][3]={{1,0,h},{0,1,k},{0,0,1}};
    mat_mul(t_mat,xy_mat,res);
}

void scale(int res[][4],int xy_mat[][4],float sx,float sy){
    float t_mat[3][3]={{sx,0,0},{0,sy,0},{0,0,1}};
    mat_mul(t_mat,xy_mat,res);
}

void rotate(int res[][4],int xy_mat[][4],float ang){
    ang=3.14159*ang/180;
    float t_mat[3][3]={{(float)cos(ang),-(float)sin(ang),0},{(float)sin(ang),(float)cos(ang),0},{0,0,1}};
    mat_mul(t_mat,xy_mat,res);
}

void reflect(int res[][4],int xy_mat[][4],float m,float c){
    int temp_mat[3][4];
    float ang=(float)atan(m)*180/3.14159;
    translate(temp_mat,xy_mat,0,-c);
    rotate(res,temp_mat,-ang);
    for(int i=0;i<4;i++) res[1][i]=-res[1][i];
    rotate(temp_mat,res,ang);
    translate(res,temp_mat,0,c);
}

int main(){
    int gd=DETECT,gm;
    initgraph(&gd,&gm,(char*)"");
    int x[4]={15,-120,120,-50},y[4]={20,80,110,70};
    int xy_mat[3][4],res[3][4],temp_mat[3][4];
    float m,n;
    get_xy_matrix(x,y,xy_mat);
    clearscreen();
    display(xy_mat);
    // Translation
    getch();
    for(int i=0;i<100;i++){
        m=(float)50*i/100;
        n=(float)-200*i/100;
        translate(res,xy_mat,m,n);
        clearscreen();
        display(res);
        delay(50);
    }
    for(int i=0;i<100;i++){
        m=(float)-100*i/100;
        n=(float)100*i/100;
        translate(temp_mat,res,m,n);
        clearscreen();
        display(temp_mat);
        delay(50);
    }
    //  Scalling
    getch();
    for(int i=0;i<100;i++){
        m=(float)1.2*i/100;
        n=(float)2*i/100;
        scale(res,xy_mat,m,n);
        clearscreen();
        display(res);
        delay(50);
    }
    //  Rotation
    getch();
    for(int i=0;i<100;i++){
        m=(float)360*i/100;
        rotate(res,xy_mat,m);
        clearscreen();
        display(res);
        delay(50);
    }
    //  Reflection 1
    getch();
    {
        reflect(res,xy_mat,1,0);
        clearscreen();
        display(xy_mat);
        setcolor(RED);
        getch();
        line(80,480,560,0);
        getch();
        setcolor(BLUE);
        display(res);
    }
    //  Reflection 2
    getch();
    {
        reflect(res,xy_mat,-1,0);
        clearscreen();
        display(xy_mat);
        setcolor(RED);
        getch();
        line(560,480,80,0);
        getch();
        setcolor(BLUE);
        display(res);
    }
    //  Reflection 3
    getch();
    {
        reflect(res,xy_mat,4,-100);
        clearscreen();
        display(xy_mat);
        setcolor(RED);
        getch();
        line(405,0,285,480);
        getch();
        setcolor(BLUE);
        display(res);
    }
    getch();
    closegraph();
}
```
### Output:
<video width="320" height="240" controls>
  <source src="src/video/tra.mp4" type="video/mp4">
  Translation
</video>

<video width="320" height="240" controls>
  <source src="src/video/rot.mp4" type="video/mp4">
  Rotation
</video>

<video width="320" height="240" controls>
  <source src="src/video/scal.mp4" type="video/mp4">
  Scaling
</video>

## Bezier Curves

Bezier curves are smooth, curved shapes used in computer graphics and design. They are defined by control points and come in two common forms: quadratic (3 control points) and cubic (4 control points). These curves are widely used for creating various shapes and paths in digital design and modeling.
There are several types of Bezier curves, but the most common ones are quadratic and cubic Bezier curves:

Quadratic Bezier Curve: This type of Bezier curve is defined by three control points: two endpoints and one control point in between. The curve starts at the first endpoint, ends at the second endpoint, and is influenced by the position of the control point. It forms a parabolic shape.
Cubic Bezier Curve: A cubic Bezier curve is defined by four control points: two endpoints and two control points. Like the quadratic Bezier curve, it starts at the first endpoint and ends at the second endpoint. The positions of the two control points determine the shape of the curve, and it's capable of creating more complex curves than the quadratic version.


Bezier curves are widely used in graphic design software, vector graphics, animation, and many other areas where smooth curves are needed. They provide a simple and intuitive way to define and manipulate curved shapes, making them an essential tool for designers and digital artists. Additionally, Bezier curves serve as the foundation for more advanced curve types, such as B-splines and NURBS (Non-uniform Rational B-splines), which offer even more control and flexibility in creating complex curves and surfaces.

### source code:
```cpp
#include <graphics.h>

int calculateBezierPoint(int P0, int P1, int P2, float t) {
    float u = 1 - t;
    float tt = t * t;
    float uu = u * u;
    float uuu = uu * u;
    float ttu = tt * u;
    return (int)(P0 * uuu + 3 * P1 * uu * t + 3 * P2 * u * tt + P2 * tt * t);
}

int main() {
    int gd = DETECT, gm;
    initgraph(&gd,&gm,(char*)"");
    int P0x = 100, P0y = 100; 
    int P1x = 200, P1y = 300;
    int P2x = 400, P2y = 100;
    float t;
    for (t = 0.0; t <= 1.0; t += 0.001) {
        int x = calculateBezierPoint(P0x, P1x, P2x, t);
        int y = calculateBezierPoint(P0y, P1y, P2y, t);
        putpixel(x, y, WHITE);
    }
    getch();
    closegraph();
}
```
## Flood Filling
Flood filling is a computer graphics technique used to fill closed regions with a specific color or pattern. It's often used in image editing software to fill areas bounded by lines or curves, and in computer games to implement mechanics like terrain painting or character coloring.

The basic idea of flood filling is to start from a seed point (a point within the area you want to fill) and gradually expand the filled region by examining neighboring pixels and determining if they should be filled as well. Here's a high-level overview of the algorithm:

1. Choose a seed point within the closed region you want to fill.
2. Get the color of the seed point and the target fill color.
3. Check if the seed point's color is already the target fill color. If it is, you're done.
4. Initialize a data structure to keep track of pixels to be filled (a stack or a queue).
5. Push or enqueue the seed point onto the data structure.
6. While the data structure is not empty:
   a. Pop or dequeue a pixel from the data structure.
   b. If the pixel's color matches the color of the seed point, change its color to the target fill color.
   c. Check the neighboring pixels (up, down, left, and right). If their color matches the original color, push or enqueue them onto the data structure.
7. Repeat step 6 until the data structure is empty. The entire region will be filled with the target color.

There are a few variations of the flood fill algorithm, including:

4-Connected vs. 8-Connected: In 4-connected flood fill, you only consider the pixels directly above, below, left, and right of the current pixel. In 8-connected flood fill, you also consider the diagonally adjacent pixels.

Scanline-Based Flood Fill: This variant works by scanning scanlines (rows) of the image and filling spans of adjacent pixels with the target color. It's often used for regions with well-defined boundaries.

Boundary Fill and Seed Fill: Boundary fill starts filling from the boundary of a region and moves inward, while seed fill starts from a point inside the region and expands outward.

Recursive vs. Iterative: Flood fill can be implemented using recursion or iteration. Recursive implementations are generally simpler to understand but may run into issues with stack overflow for large regions. Iterative implementations use data structures like stacks or queues to avoid recursion.

It's important to consider certain factors when implementing flood fill, such as handling antialiasing, handling boundaries, and avoiding infinite loops in cyclic patterns.

Flood filling is a fundamental technique in computer graphics and has various applications beyond basic image editing, including in the fields of computer-aided design (CAD), computer-aided manufacturing (CAM), and computer vision.

### source code:
```cpp
#include <graphics.h>

void flood(int x, int y, int new_col, int old_col) {
    if (getpixel(x, y) == old_col) {
        putpixel(x, y, new_col);
        flood(x + 1, y, new_col, old_col);
        flood(x - 1, y, new_col, old_col);
        flood(x, y + 1, new_col, old_col);
        flood(x, y - 1, new_col, old_col);
    }
}

int main() {
    int gd=DETECT, gm;
    initgraph(&gd, &gm, (char*) "");
    int left,right;
    int top = left = 50, bottom = right = 300;
    rectangle(left, top, right, bottom);

    int x = 51, y = 51;
    int newcolor = 11, oldcolor = 0;

    flood(x, y, newcolor, oldcolor);
    getch();

    return 0;
}
```
<video width="320" height="240" controls>
  <source src="src/video/floodfill.mp4" type="video/mp4">
</video>