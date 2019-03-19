---
title: Queue in Array
date: 2019-03-16
tags: ds
---

### 队列的数组实现
---

```
class Queue {
public:
    Queue(int c = 10);
    ~Queue();
    bool enqueue(int data);
    int dequeue();
    int front();
    bool isEmpty();
    bool isFull();
    int size();
private:
    int *queue_;
    int front_;
    int rear_;
    int capacity_;
```
<!--more-->

变量 | 功能
---|---
| 前端： front | 构造队列：Queue| 
| 后端： rear | 构造队列：Queue| 
| 存储数组： queue | 入队：enqueue| 
| 容量： capacity | 出队：dequeue| 
| | 判空：isEmpty| 
| | 判满：isFull| 
| | 大小：capacity| 
| | 查看队首：front| 

- 模板类接口实现：
- 用数组是现实时常实现为循环队列，以便实现O(1)入队和出队，使空间合理被应用。
- 空队列： front == rear
- 满队列： (rear + 1) % size == front （牺牲一个位置）

```
template <typename T>
class Queue {
public:
    Queue(int c = 10);
    ~Queue();
    bool enqueue(T data);
    T dequeue();
    T front();
    bool isEmpty();
    bool isFull();
    int size();
private:
    T *queue_;
    int front_;
    int rear_;
    int capacity_;
};

template <typename T>
Queue<T>::Queue(int c):
    capacity_(c),
    front_(0),
    rear_(0),
    queue_(NULL)
{
    queue_ = new T [c];
    std::cout << "capacity:" << c << std::endl;
}
 
template <typename T>
Queue<T>::~Queue()
{
    if(queue_) {
        delete [] queue_;
    }
}
 
template <typename T>
bool Queue<T>::enqueue(T data)
{
    if(isFull()) {
        return false;
    }
    queue_[rear_] = data;
    rear_ = (rear_ + 1) % capacity_;
    return true;
}
 
template <typename T>
T Queue<T>::dequeue()
{
    T data;
    if(isEmpty()) {
        throw std::out_of_range("Queue::dequeue() - Empty queue");
    }
    data = queue_[front_];
    front_ = (front_ + 1) % capacity_;
    return data;
}
 
template <typename T>
T Queue<T>::front()
{
    if(isEmpty()) {
        throw std::out_of_range("Queue::front() - Empty queue");
    }
    return queue_[front_];
}
 
template <typename T>
bool Queue<T>::isEmpty()
{
    return (front_ == rear_);
}
 
template <typename T>
bool Queue<T>::isFull()
{
    return (((rear_+1) % capacity_) == front_);
}
 
template <typename T>
int Queue<T>::size()
{
    return ((rear_ -  front_ + capacity_) % capacity_);
}
```

