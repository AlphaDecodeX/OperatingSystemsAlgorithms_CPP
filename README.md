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


