# Readers -writers Problem
## Solution
 In this solution,I have tried to make a starve free solution of Readers -Writers Problem.In this i have  made a  Semaphore preserves the first in first out(FIFO) order when locking and releasing theprocesses( Semaphore uses a FIFO queue to maintain the list of waiting list processes)
 ## Structure of  Semaphore
 ``` cpp
 // The code for a FIFO semaphore.
struct Semaphorequeue{
  int valueans = 1;
  FIFO_Queue* Q = new FIFO_Queue();
}
    
void wait(Semaphorequeue *S,int* pid){
  S->valueans--;
  if(S->valueans < 0){
  S->Q->push(pid);
  block(); 
  }
}
    
void signal(Semaphorequeue *S){
  S->valueans++;
  if(S->valueans <= 0){
  int* PID = S->Q->pop();
  wakeup(PID); 
  }
}



struct FIFO_Queue{
    ProcessBlock* entry, exit;
    int* pop(){
        if(entry == nullptr){
            return -1;          
        }
        else{
            int* val = entry->valueans;
            entry = entry->next;
            if(entry == nullptr)
            {
                exit = nullptr;
            }
            return val;
        }
    }
    void* push(int* val){
        ProcessBlock* pcb = new ProcessBlock();
        pcb->valueans = val;
        if(exit == nullptr){
            entry = exit = n;
            
        }
        else{
            exit->next = pcb;
            exit = pcb;
        }
    }
    
}


struct ProcessBlock{
    ProcessBlock* next;
    int* process_block;
}
```

## Variables used
```cpp
int readcnt = 0;                      
Semaphorequeue allow = new Semaphorequeue();        //seamphore representing the order in which the writer and 
                                        
Semaphorequeue r_writer = new Semaphorequeue();        
Semaphorequeue mutex_readcnt = new Semaphorequeuequeue();     //seamphore required to change the readcnt variable
```
 ## Readers code
 ```cpp
 do{

       wait(turn,pid);              //waiting for its chance  to get executed
       wait(mutex_readcnt,pid);           // access to change readcnt
       readcnt++;                       //update the number of readers trying to access critical section 
       if(readcnt==1)                  
       signal(turn);                       //releasing turn so that the next reader or writer can take
                                           //and can be serviced
       signal(mutex_readcnt);                    //release access to the reader
/* CRITICAL SECTION */

       wait(mutex_readcnt,pid)            //requesting access to change readcnt         
       readcnt--;                       
       if(readcnt==0)                   //if all the reader have finished executing their critical section
        signal(r_writer);                       //releasing access 
       signal(mutex_readcnt);                    //release access  
       

       
}while(true);
```
## Writers Code
```cpp
do{

      wait(turn,pid);              //waiting for its chance to get executed
      wait(r_writer,pid);               //requesting  access to the critical section
      signal(turn,pid);            //releasing turn so that the next reader or writer can take the token
                                 //and can be serviced
/* CRITICAL SECTION */

      signal(r_writer)                         //releasing access to critical section for next reader or writer


}while(true);
```
