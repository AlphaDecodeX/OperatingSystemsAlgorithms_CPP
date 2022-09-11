# OS Concepts and Algorithms

### 1. Semaphores implementation in C++

```
Methods used
  - std::mutex mtx; std::condition_variable cv;
  - std::unique_lock<std::mutex> lock(mtx);
  - cv.notify_one(); cv.wait(lock);
```

```CPP
class Semaphore{
private:
    int count;
    mutex mtx;
    condition_variable cv;
public:
    Semaphore(){}
    Semaphore(int a): count(a){}; // used initializer list here

    void setCount(int a){
        count = a;
    }

    inline void signal(){
        unique_lock<mutex> lock(mtx);
        count++;
        if(count<=0){
            cv.notify_one();
        }
    }
    inline void wait(){
        unique_lock<mutex> lock(mtx);
        count--;
        while(count<0){
            cv.wait(lock);
        }
    }
};

```

- NOTE: To work with Semaphores in CPP #include <semaphore.h> file and following methods are available there:-
```
  - sem_t foo_sem;
  - sem_init(&foo_sem, 0, 1);
  - sem_wait(&foo_sem); 
  - sem_post(&bar_sem); 
```

---------

### 2. Peterson's Solution to Critical Section

```
Methods used:-
  - std::thread t(function, params);
  - t.join();
```

```CPP
#include <bits/stdc++.h>
using namespace std;

int flag[2] = {0,0};
int turn;
int c = 100;

void printThread(int i){
	while(c--){
		flag[i] = 1;
		turn = i;

		while(turn == i && flag[i]==1);
		cout<<"Thread "<<(i+1)<<" running"<<endl;		
		flag[i] = 0;
	}
	return;
}


signed main(){
	thread t1(printThread, 0);
	thread t2(printThread, 1);

	t1.join(); t2.join();

	return 0;
}

```

```
Limitations:-
  - Peterson's solution is limited to two threads only
  - Because of turn variable, Order of threads will change according to the value of turn
  - Solution to C.S problem should be mutually exclusive, independent (Order of threads shouldn't effect) and Bounded Waiting
  - Here we are able to solve the all problems but only for two threads
```
-------


