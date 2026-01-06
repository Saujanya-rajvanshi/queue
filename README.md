# queue

## INDEX 
- [basic](#basic)
- [queue representaion](#queue-representation)
- [circular queue](#circular-queue)
- [dequeue](#dequeue)

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

## circular queue

- ✅ Efficient memory use (reuses freed space)  
- 🚫 Prevents false overflow in linear queues  
- ⚡ Faster (no shifting needed)  
- 🔄 Ideal for continuous buffering (e.g., scheduling, streaming)  
- 📐 Simple index management with modular arithmetic  
- Simpler Index Management
Uses modular arithmetic:
- rear = (rear + 1) % size
- front = (front + 1) % size

* Circular Queue Using Array (Fixed Capacity)
* Circular Queue (Linked List Implementation)

---

### 🔄 Circular Queue Using Array (Fixed Capacity)

```cpp
#include <iostream>
#define SIZE 5
using namespace std;

class CircularQueue {
    int arr[SIZE];
    int front, rear, count;

public:
    CircularQueue() {
        front = 0;
        rear = -1;
        count = 0;
    }

    bool isEmpty() {
        return count == 0;
    }

    bool isFull() {
        return count == SIZE;
    }

    void enqueue(int x) {
        if (isFull()) {
            cout << "Queue Overflow\n";
            return;
        }
        rear = (rear + 1) % SIZE;
        arr[rear] = x;
        count++;
        cout << "Inserted: " << x << endl;
    }

    int dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return -1;
        }
        int val = arr[front];
        front = (front + 1) % SIZE;
        count--;
        cout << "Removed: " << val << endl;
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

    void display() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return;
        }
        cout << "Queue elements: ";
        for (int i = 0; i < count; i++) {
            cout << arr[(front + i) % SIZE] << " ";
        }
        cout << endl;
    }
};

int main() {
    CircularQueue q;
    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);
    q.enqueue(40);
    q.enqueue(50); // Should trigger overflow
    q.display();
    q.dequeue();
    q.dequeue();
    q.enqueue(60);
    q.enqueue(70); // Should wrap around
    q.display();
    cout << "Front element: " << q.peek() << endl;
    cout << "Queue size: " << q.size() << endl;
    return 0;
}
```

---

---

### 🔗 Circular Queue (Linked List Implementation)

```cpp
#include <iostream>
using namespace std;

class Node {
public:
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

class CircularQueue {
    Node* front;
    Node* rear;
    int count;

public:
    CircularQueue() {
        front = rear = nullptr;
        count = 0;
    }

    bool isEmpty() {
        return front == nullptr;
    }

    void enqueue(int x) {
        Node* temp = new Node(x);
        if (isEmpty()) {
            front = rear = temp;
            rear->next = front;  // circular link
        } else {
            rear->next = temp;
            rear = temp;
            rear->next = front;  // maintain circularity
        }
        count++;
        cout << "Inserted: " << x << endl;
    }

    int dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return -1;
        }
        int val;
        if (front == rear) { // only one element
            val = front->data;
            delete front;
            front = rear = nullptr;
        } else {
            Node* temp = front;
            val = temp->data;
            front = front->next;
            rear->next = front;  // maintain circularity
            delete temp;
        }
        count--;
        cout << "Removed: " << val << endl;
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

    void display() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return;
        }
        cout << "Queue elements: ";
        Node* temp = front;
        do {
            cout << temp->data << " ";
            temp = temp->next;
        } while (temp != front);
        cout << endl;
    }
};

int main() {
    CircularQueue q;
    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);
    q.display();
    q.dequeue();
    q.display();
    q.enqueue(40);
    q.enqueue(50);
    q.display();
    cout << "Front element: " << q.peek() << endl;
    cout << "Queue size: " << q.size() << endl;
    return 0;
}
```

---

###### dequeue
---

### 🔁 Double Ended Queue (Deque)

- **Definition**:  
  A linear data structure that allows **insertion and deletion from both ends** — front and rear.

- **Key Features**:  
  - Generalized form of queue  
  - Can behave like both **stack** and **queue**  
  - Useful for problems needing flexible end operations  
  - Typically implemented using **doubly linked list** or **circular array**

---

### 🧭 Deque as Stack vs Queue

| Mode         | Behavior                          |
|--------------|-----------------------------------|
| As Stack     | Insert/Delete from **one end**    |
| As Queue     | Insert at rear, delete from front |

---

### 🧨 Types of Deques

| Type                   | Insertion Allowed At | Deletion Allowed At | Use Case |
|------------------------|----------------------|----------------------|----------|
| **Input-Restricted**   | One end only         | Both ends            | Controlled input |
| **Output-Restricted**  | Both ends            | One end only         | Controlled output |

---

