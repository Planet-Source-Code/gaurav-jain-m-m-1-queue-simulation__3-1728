<div align="center">

## M/M/1 Queue Simulation


</div>

### Description

Simulation of an M/M/1 queue with the condition that K customers have to enter the queue before

the service starts. once the service starts the arrivals are purely random in nature. if the

server is fast and the queue empties then the server has to wait again for K customers to

arrive. and the process restarts
 
### More Info
 
K, Arrival rate (lamda) and departure rate (mu)

The code is inefficient

a text file called output.txt.. contains 2 columns. first colum is the time at which the data was taken and the second column is the number of customers in the system (both waiting and being served)


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Gaurav Jain](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/gaurav-jain.md)
**Level**          |Intermediate
**User Rating**    |4.8 (19 globes from 4 users)
**Compatibility**  |C, C\+\+ \(general\), Microsoft Visual C\+\+
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__3-1.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/gaurav-jain-m-m-1-queue-simulation__3-1728/archive/master.zip)





### Source Code

```
/* by Gaurav Jain */
/* Simulation of an M/M/1 queue with the condition that 10 customers have to enter the queue before
  the service starts. once the service starts the arrivals are purely random in nature. if the
  server is fast and the queue empties then the server has to wait again for 10 customers to
  arrive. and the process restarts */
/* this has been simulated using 1000 customers */
/* please use gcc -lm to compile it */
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
/*----- Constants ----------------*/
#define BUSY 1
#define IDLE 0
#define MAXIMUM_NUMBER_OF_CUSTOMERS 1000
/*--- Function prototypes -----------------------------------------------*/
double Exponential(double x);
void write_file(double a[][2],int m);
void Add_to_queue(int i,double time);
double arrival(int i,double current_time_index,int lamda);
double bernoulli(float a);
double start_service(double current_time_index,int mu,int lamda);
double departure(double current_time_index,int mu,int lamda);
double arrival_time[1000];
double departure_time[1000];
double service_start_time[1000];
double waiting_time[1000];
double service_time [1000];
double filearray[5000][2];
int main_counter=0;
struct queue{
int	  customer_number;
double arrival_time_in_system;
double start_service_time;
double service_time;
}customer_queue[1000];
typedef struct queue CUST_QUEUE;
struct queue2{
int	  customer_number;
double arrival_time_in_system;
double start_service_time;
double service_time;
}ultimate_queue[1000];
int number_in_queue=0;
int server_status=0;
int queue_head=0;
int main_count=0;
int number_in_system=0;
int customer_count=0;
int customer_no=0;
int k=0,d=0;
//****************************************************************************//
//			 Main program
//****************************************************************************//
void main()
{
 int lamda;  // Mean time between arrivals
 int mu;       // Mean service time
 int i=0,j=0,m=0,z=0;
 double time = 0.0,new_time_index=0.0;      // Simulation time
 double sum=0;
 double average_waiting_time;
 long int n = 0;        // Number of customers in the system
 // Number of customers that have come to the system till now
 double current_time_index=0;
 int max=MAXIMUM_NUMBER_OF_CUSTOMERS;
 int number_before_service_is_started;
 printf("Enter the arrival rate\n");
 scanf("%d",&lamda);
 printf("Enter the service rate\n");
 scanf("%d",&mu);
 printf("enter the value of K (the number of people required in the queue before the service starts\n");
 scanf("%d",&number_before_service_is_started);
 // Main simulation loop
 while (customer_count<max)
 {
	 if(server_status==IDLE){
		/* put the first k customers in the queue */
		 queue_head=0;
		 filearray[d][0]=number_in_system;
		 filearray[d++][1]=current_time_index;
		 for(i=0;i<number_before_service_is_started;i++)
			{
				/* arrival event ...initially k people have to come in */
			 new_time_index=arrival(customer_count,current_time_index,lamda);
			 Add_to_queue(customer_count-1,new_time_index);
			 //-1 because it had been incremented in arrival function
			 current_time_index=new_time_index;
			}
	 /* start service now */
	 server_status=BUSY;
	 current_time_index=start_service(current_time_index,mu,lamda);
	 }
	} //end while
for(i=0;i<MAXIMUM_NUMBER_OF_CUSTOMERS;i++)
{
 departure_time[i]=ultimate_queue[i].service_time+ultimate_queue[i].start_service_time;
 waiting_time[i]= ultimate_queue[i].start_service_time-ultimate_queue[i].arrival_time_in_system;
 sum+=waiting_time[i];
}
average_waiting_time=sum/MAXIMUM_NUMBER_OF_CUSTOMERS;
printf("Average Waiting time=%f",average_waiting_time);
write_file(filearray,MAXIMUM_NUMBER_OF_CUSTOMERS);
}//end main
//****************************************************************************//
//       Begin function to generate an exponential random number
//****************************************************************************//
double Exponential(double x)
{
 double z;
 // a uniform RV (0 < z < 1)
 do
 {
   z = ((double) rand()/RAND_MAX);
 }
 while ((z==0) || (z==1));
 return((-1/x)*log(z));  // formula for exponential Random Variable
}
//****************************************************************************//
//	    Begin function to simulate an arrival event
//****************************************************************************//
//pass the current_time_index and return the arrival time
double arrival(int customer_no,double current_time_index,int lamda)
{
	static int k=0;
	int i;
	double copy=0;
	double random_time;
  //set the arrival time for the 1000 customers in one go
	if(k==0){
		for(i=0;i<MAXIMUM_NUMBER_OF_CUSTOMERS;i++){
		random_time=Exponential(lamda);
		arrival_time[i]=copy+random_time;
		copy=arrival_time[i];
		}
	}
	ultimate_queue[customer_count].customer_number=customer_count;
	ultimate_queue[customer_no].arrival_time_in_system=arrival_time[customer_no];
	customer_no++;
	customer_count++;
	filearray[d][0]=number_in_system;
	filearray[d++][1]=current_time_index;
	number_in_system++;
	main_counter++;
	k++;
	return(arrival_time[customer_no-1]);
}
//****************************************************************************//
//			Begin function to add a customer to queue
//****************************************************************************//
void Add_to_queue(int customer_nomber,double time)
{
	customer_queue[number_in_queue].customer_number=customer_nomber ;
  	customer_queue[number_in_queue].arrival_time_in_system=time;
	number_in_queue++;
}
//****************************************************************************************//
//	    	Begin function to Start the service
//****************************************************************************************//
double start_service(double current_time_index,int mu,int lamda)
{
	double dept_time_index;
	int i;
	while(number_in_queue)
	{
	  /* start servicing the first guy in the queue set his service start_time */
		i=customer_queue[queue_head].customer_number;
		service_start_time[i]=current_time_index;
		customer_queue[queue_head].start_service_time=current_time_index;
		ultimate_queue[main_count].start_service_time=current_time_index;
		//call the departure
		dept_time_index=departure(current_time_index,mu,lamda);
		current_time_index=dept_time_index;
	}
	server_status=IDLE;
	return(current_time_index);
}
//*************************************************************************************//
//	   	    	Function to start the departure
//*************************************************************************************//
double departure(double current_time_index,int mu,int lamda)
{
	int i;
	double dept_time_index,serv_time,new_time_index;
	serv_time=Exponential(mu);
	customer_queue[queue_head].service_time=serv_time;
	ultimate_queue[main_count].service_time=serv_time;
	dept_time_index=serv_time+current_time_index;
	//if an arrival occurs while the customer is being served then signal an arrival
	//and add him to the queue
	while(arrival_time[customer_count]<dept_time_index && customer_count<MAXIMUM_NUMBER_OF_CUSTOMERS){
	new_time_index=arrival(customer_count,current_time_index,lamda);
	Add_to_queue(customer_count-1,new_time_index);
	}
	number_in_queue--;
	filearray[d][0]=number_in_system;
	filearray[d++][1]=current_time_index;
	number_in_system--;
	//shift the queue up by one
	for(i=1;i<MAXIMUM_NUMBER_OF_CUSTOMERS;i++){
	customer_queue[i-1].arrival_time_in_system=customer_queue[i].arrival_time_in_system;
	customer_queue[i-1].customer_number=customer_queue[i].customer_number;
	customer_queue[i-1].service_time=customer_queue[i].service_time;
	customer_queue[i-1].start_service_time=customer_queue[i].start_service_time;
	}
	main_count++;
	return(dept_time_index);
}
//***********************************************************************************//
//	  	    	Begin function to write to a file
//***********************************************************************************//
void write_file(double filearray[][2],int m)
{
 FILE *fp;
 int i;
 fp=fopen("output.txt","w");
 for(i=0;i<m;i++)
 {
  fprintf(fp,"%f\t\t %f\n",filearray[i][1],filearray[i][0]);
 }
 fclose(fp);
}
```

