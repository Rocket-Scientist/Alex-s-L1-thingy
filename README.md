/*Alexander John Naylor : Josephine Butler College*/

#include <stdio.h>
#include <math.h>
#include <grx20.h>
#define MAX 1000
/*Hopefully draw a line of best fit prgram*/

/*declaring things : variables first then functions*/
char line[7];
float x[MAX], y[MAX], sumx, sumy, sumxy, sumxx, m, y0, xplot, yplot, b, ymax, ymin, xrange, yrange, xaxis, yaxis, k, ystart, yend, xstart, xend, a, y_var, *ybegin, *xbegin, *xfinish, *yfinish;
int n, i, j, xres, yres, q, w, temp, What_to_do, points_are_in;
int establish_number_of_points_required();

void getData(int i);
void print_table();
void origin();
void plot_graph();

main()
{
      sumxy = 0;
      sumy = 0;
      sumx = 0;
      sumxx = 0;
      b = 40;
      xbegin = &xstart;
      ybegin = &ystart;
      xfinish = &xend;
      yfinish = &yend;
      
      printf("This program should draw a line of best fit for 1 to 1001 points you enter\n\n");
      system ("PAUSE");
      
      while (0 == 0) /*While loop so that it always returns to the main menu unless you chose to quit*/
      {
            system("cls");
            printf ("Menu\n1. enter/change points\n2. Show table of results with y var\n3. view facts about points\n4. plot graph of points\n5. quit\nEnter the number proceeding the the operation you would like to perform: "); /*next bit is supossed to make a menu. the wording might not be great but I think it works*/
            scanf ("%d",&What_to_do);
            if ((What_to_do == 2 || What_to_do == 3 || What_to_do == 4) && points_are_in != 1) { /*This is to stop the user trying to use data to do something when no data has been entered*/
                            printf ("\nYou have not yet entered any points for the prgram to use\nTherefore ");
                            What_to_do = 6;
                            }
            system("cls");
            switch(What_to_do) {
                               case 1 : {
                                    if (points_are_in != 1) { /*if statement used so that if they have already entered some values it doesn't ask them to again, goes straight to asking what they want to change*/
                                                      n = establish_number_of_points_required(); /*calls the function to decide number of points the user wants*/
                                                      for (i=0;i<n;i=i+1) {
                                                          getData(i); /*calls the function to to get each point calling it for every point. Doesn't do all in one call because would have to use pointers to allow function to return an array from other function*/
                                                          }
                                                      }
                                    while (What_to_do == 1) { /*while loop used so that more than one point can be changed before going back to main menu*/
                                          system("cls");
                                          printf("Your current points are:\n");
                                          for (i = 0;i <= n-1 ;i = i+1) {
                                              printf("%d.  (%f,%f)\n",i+1,x[i],y[i]);
                                              }
                                          if (points_are_in != 1) { /*if statement used so that if this is the second time they are looking at their points, then they already know they are here to change something, thus no need to ask*/
                                                            printf ("\nAre you happy with these points?\nEnter 1 for NO or 2 for YES: ");
                                                            scanf ("%d",&What_to_do);
                                                            }
                                          if (What_to_do == 1) { /*This if statement, if used, gives the user the oportunity to change a specific point if they want to change one value without having to start over*/
                                                         printf("\nWould you like to:\n1.  Change a specific point\n2.  Change all the points\n3.  Return to main menu\nEnter the value proceeding operation you want to perform: ");
                                                         scanf ("%d", &temp);
                                                         if (temp == 1) {
                                                               printf ("Enter the number which prceedes the point you would like to change: ");
                                                               scanf ("%d", &j);
                                                               printf ("\nEnter x coordinate of the new point: ");
                                                               scanf ("%f",&x[j-1]);
                                                               printf ("Enter y coordinate of the new point: ");
                                                               scanf ("%f",&y[j-1]);
                                                               }
                                                         if (temp == 2) { /*Used to let the user chagne all their points if they want to. Just repeats the first part of case 1. I tried adding a while loop but found it used more lines than this*/
                                                               system("cls");
                                                               n = establish_number_of_points_required();
                                                               for (i=0;i<n;i=i+1) {
                                                                   getData(i);    
                                                                   }
                                                               }
                                                         if (temp == 3) What_to_do = 0;
                                                         }
                                          }
                                    for (i=0; i<n; i=i+1) { /*This for loop sorts the points in ascending order of x values using the bubble sort technique*/
                                        for(j=0; j<n-1; j=j+1) {
                                                 if(x[j]>x[j+1]) {
                                                                 temp = x[j+1];
                                                                 x[j+1] = x[j];
                                                                 x[j] = temp;
                                                                 temp = y[j+1];
                                                                 y[j+1] = y[j];
                                                                 y[j] = temp;
                                                                 }
                                                 }
                                        }
                                    xrange = x[n-1]-x[0]; /*Because I have sorted the results can rely of x values being in order*/
                                    yrange = ymax-ymin;
                                    m = (n*sumxy-sumx*sumy)/(n*sumxx-sumx*sumx);
                                    y0 = (sumy-m*sumx)/n;
                                    points_are_in = 1; /*Means that if the user choses to come back to case 1 after already having entered values, the program rocognises the fact they have already entered some values*/
                                    }
                               break;
                               case 2 : print_table(); /*runs other fnction that displays the table. Makes the menu easier to look at in code having it in anotehr function*/
                               break;
                               case 3 : printf ("Some useful values:\nn=%i\nxmin=%f\nymin=%f\nxmax=%f\nymax=%f\nxrange=%f\nyrange=%f\nsumx=%f\nsumy=%f\nsumxx=%f\nsumxy=%f\n\nYour points best fit the line with:\ngradient=%.3f\ny intercept=%.3f\n", n, x[0], ymin, x[n-1], ymax, xrange, yrange, sumx,sumy,sumxx,sumxy,m,y0);
                               break;
                               case 4 : plot_graph(); /*runs other function to plot the graph for same reasoning as in case 2*/
                               break;
                               case 5 : exit(3);
                               break;
                               default  : printf("this is not a valid input\n");/*If the user doesn't enter an appropriate option for the menu they are told and can try again*/
                               }
            printf ("\n\n");
            system ("PAUSE");
            }
}

establish_number_of_points_required() {
                                      i=1;
                                      while (i == 1) { /*While loop so that they have to keep entering a number of values they want until it is suitble, or they can quit*/
                                            printf ("Enter the number of points you would like to use: ");
                                            scanf("%d",&n);
                                            if (n<2 || 1000<n) { /*If they enter a desired number of points outside the length of the array or fewer than 2 points then they are made aware and given the chance to change their mind*/
                                                    printf ("\nWARNING!\nThe program is limited to having the number of values between 1 and 1001\n\nOptions now:\n1. Choose a different number of points\n2. Quit\nEnter the value which precceds the operation you would like to perform: ");
                                                    scanf ("%d",&i);
                                                    system("cls");
                                                    }
                                            else i = 3;
                                            }
                                      if (i == 2) exit(1);
                                      return n;
                                      }

void getData(int i)  {
     printf ("\nEnter x coordinate %.0d: ", i+1);
     scanf ("%f",&x[i]);
     printf ("Enter y coordinate %.0d: ", i+1);
     scanf ("%f",&y[i]);
     sumx = sumx+x[i];
     sumy = sumy + y[i];
     sumxx = sumxx +(x[i] * x[i]);
     sumxy = sumxy +(y[i] * x[i]);
     if (i == 0) { /*Next 5 lines used to find ymax and ymin. Not one for xmax and xmin as the x values are later ordered so xmax and xmin will alwasy be the last and first x values*/
           ymax = y[i];
           ymin = y[i];
           }
     if (ymax < y[i]) ymax = y[i];
     if (ymin > y[i]) ymin = y[i];
     }

void print_table() {
     printf ("                    Table of results:\n\n   x          y          y on best fit line          y var\n");
     for (i=0; i<n; i=i+1) {
         a = x[i]*m+y0;
         y_var = a-y[i];
         printf("%5.1f%11.1f%20.1f%20.1f\n",x[i],y[i],a,y_var);
         }
     }

void origin() { /*This function is so that if the user's points do not need the origin inthe graph, they can still have the origin if they want it*/
     if (x[0] > 0.0|| x[n-1] < 0.0 || ymin > 0.0 || ymax < 0.0) { /*This if statement is to prevent the program asking if the user wants the origin in their graph if it will be their anyway*/
              printf ("Would you like to include the origin in your graph?\nEnter 1 for YES or 2 for NO: ");
              scanf ("%d",&i);
              }
     if (i == 1 && ymin > 0) *ybegin = 0; /*next 7 lines used to define the start and end values of the x and y axis on the graph*/
     else *ybegin = ymin;
     if (i == 1 && ymax < 0) *yfinish = 0;
     else *yfinish = ymax;
     if (i == 1 && x[0] > 0) *xbegin = 0;
     else *xbegin = x[0];
     if (i == 1 && x[n-1] < 0) *xfinish = 0;
     else *xfinish = x[n-1];
     }

void plot_graph() {
     origin(); /*calls the origin function. kept as a seperate function in case when modifying the program chosing to have the orgin wants to be  different option on the menu*/
     GrSetMode(GR_default_graphics);
     xres=GrScreenX();
     yres=GrScreenY();
     if (i == 1 || (yend > 0 && xend > 0)) { /*If statement runs if the user wants the origin*/
           if (ystart >= 0) xaxis = yres-3*b/4; /*For all places wehre the border I have chosen to make the order 3 times larger on the bottom and on the left hand side than on the top and right hand side. This is to fit the numbers of the axis on*/
           else xaxis = yres-(3*b/4)+(ystart*(yres-b)/(yend-ystart));
           if (xstart >= 0) yaxis = 3*b/4;
           else yaxis = (3*b/4)-(xstart*(xres-b)/(xend-xstart));
           }
     else { /*else statement used if the user doesn't want the origin*/
          yaxis = xres - b/4;
          xaxis = b/4;
          }
     GrLine (yaxis,yres-3*b/4, yaxis, b/4, 15); /*This line and the one afer are to draw the x and y axis*/
     GrLine (3*b/4,xaxis, xres-b/4, xaxis, 15);
     j=-1;
     for (i=b/4;i<=(yres-3*b/4);i=(i+(yres-b)/10)) { /*This for loop is to add tick marks and numbers along the y axis. There are always 10 tick marks, the numbers on them are what change for different data sets*/
         j=j+1;
         k = yend-(yend-ystart)*j/10;
         GrLine (yaxis,i,yaxis-3,i,15);
         sprintf (line,"%.1f",k);
         GrTextXY(yaxis-30,i-4,line,15,0);
         }
     j=-1;
     for (i=3*b/4;i<=(xres-b/4);i=(i+(xres-b)/10)) {/*This for loop is to add tick marks and numbers along the x axis. There are always 10 tick marks, the numbers on them are what change for different data sets*/
         j=j+1;
         k = xstart+((xend-xstart)*j/10);
         GrLine (i,xaxis,i,xaxis+3,15);
         sprintf (line,"%.1f",k);
         GrTextXY(i-20,xaxis+8,line,15,0);
         }
     GrLine((3*b/4), yres - ((3*b/4) + ((m*(xstart)+y0)-ystart) * ((yres - b) / (yend-ystart))), xres-(b/4), yres - ((3*b/4) + ((m*(xend)+y0)-ystart) * ((yres - b) / (yend-ystart))), 15); /*Draws the line of best fit. So long as it calculates the y values for each point itself rather than having them calculated earlier. This is to save lines*/
     for (i=0;i<n;i=i+1) { /*This for loop is the plot the points on the graph*/
         xplot = (3*b/4) + (x[i]-xstart) * ((xres - b) / (xend-xstart));
         yplot = yres - ((3*b/4) + (y[i]-ystart) * ((yres - b) / (yend-ystart)));
         GrCircle(xplot,yplot, 2,14);
         }  
     }

