# queue

## INDEX 
- [basic](#basic)
- [queue representaion](#queue-representation)

## basic 
---

### 📚 Queues — Key Concepts

- **Definition**:  
  A **queue** is a non-primitive, linear data structure that allows:
  - **Insertion** at one end → called the **rear**
  - **Deletion** at the other end → called the **front**

- **FIFO Principle**:  
  Queue follows **First-In-First-Out (FIFO)** — the first element inserted is the first to be removed.


## queue representation

- `enqueue()`
- `dequeue()`
- `peek()`
- `size()`
- `isEmpty()`
- `isFull()` (only for array-based queue)

---

### 📦 1. Queue Using Array (Fixed Capacity)

```cpp
#include <iostream>
#define MAX 100
using namespace std;

class ArrayQueue {
    int arr[MAX];
    int front, rear, count;

public:
    ArrayQueue() {
        front = 0;
        rear = -1;
        count = 0;
    }

    void enqueue(int x) {
        if (isFull()) {
            cout << "Queue is full\n";
            return;
        }
        rear = (rear + 1) % MAX;
        arr[rear] = x;
        count++;
    }

    int dequeue() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return -1;
        }
        int val = arr[front];
        front = (front + 1) % MAX;
        count--;
        return val;
    }

    int peek() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return -1;
        }
        return arr[front];
    }

    int size() {
        return count;
    }

    bool isEmpty() {
        return count == 0;
    }

    bool isFull() {
        return count == MAX;
    }
};
```

---

### 🔗 2. Queue Using Linked List (Dynamic Capacity)

```cpp
#include <iostream>
using namespace std;

class Node {
public:
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

class LinkedQueue {
    Node *front, *rear;
    int count;

public:
    LinkedQueue() {
        front = rear = nullptr;
        count = 0;
    }

    void enqueue(int x) {
        Node* temp = new Node(x);
        if (rear == nullptr) {
            front = rear = temp;
        } else {
            rear->next = temp;
            rear = temp;
        }
        count++;
    }

    int dequeue() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return -1;
        }
        Node* temp = front;
        int val = temp->data;
        front = front->next;
        if (front == nullptr) rear = nullptr;
        delete temp;
        count--;
        return val;
    }

    int peek() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return -1;
        }
        return front->data;
    }

    int size() {
        return count;
    }

    bool isEmpty() {
        return front == nullptr;
    }

    // Not applicable for linked list, but can be added if needed
    bool isFull() {
        return false;
    }
};
```

---

Would you like a test driver to demonstrate both implementations in action?
