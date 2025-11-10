1)Write a Python program to manage the borrowing records of books in a library. Implement the following functionalities:
• Compute the average number of books borrowed by all library members.
• Find the book with the highest and lowest number of borrowings in the library.
• Count the number of members who have not borrowed any books (i.e., members having a borrow count of 0).
• Display the most frequently borrowed book (i.e., find the mode of the borrow counts).
-->
import java.util.*;

public class LibraryRecords {

    public static void main(String[] args) {
        int[] borrowCounts = {3, 0, 5, 1, 0, 5, 2};

        // 1. Average number of books borrowed
        double sum = 0;
        for(int count : borrowCounts){
            sum += count;
        }
        double average = sum / borrowCounts.length;

        // 2. Highest and Lowest borrow count
        int max = borrowCounts[0];
        int min = borrowCounts[0];

        for(int count : borrowCounts){
            if(count > max)
                max = count;
            if(count < min)
                min = count;
        }

        // 3. Number of members who borrowed 0 books
        int zeroCount = 0;
        for(int count : borrowCounts){
            if(count == 0)
                zeroCount++;
        }

        // 4. Most frequently borrowed count (Mode)
        HashMap<Integer, Integer> freq = new HashMap<>();
        for(int count : borrowCounts){
            freq.put(count, freq.getOrDefault(count, 0) + 1);
        }

        int mode = borrowCounts[0];
        int maxFreq = 0;

        for(Map.Entry<Integer, Integer> entry : freq.entrySet()){
            if(entry.getValue() > maxFreq){
                maxFreq = entry.getValue();
                mode = entry.getKey();
            }
        }

        // Display Results
        System.out.println("Average books borrowed: " + average);
        System.out.println("Highest borrow count: " + max);
        System.out.println("Lowest borrow count: " + min);
        System.out.println("Number of members who borrowed 0 books: " + zeroCount);
        System.out.println("Most frequently borrowed count (Mode): " + mode);
    }
}
2)In an e-commerce system, customer account IDs are stored in a list. 
You are required to write a program that performs the following operations:

• Linear Search: Check whether a particular customer account ID exists in the list using the linear search technique.

• Binary Search: Implement the binary search algorithm to check whether a customer account ID exists in the list
-->
import java.util.*;

public class CustomerSearch {

    // Linear Search
    public static boolean linearSearch(int[] arr, int key) {
        for (int id : arr) {
            if (id == key) {
                return true; // Found
            }
        }
        return false; // Not found
    }

    // Binary Search (Array must be sorted)
    public static boolean binarySearch(int[] arr, int key) {
        int left = 0, right = arr.length - 1;

        while (left <= right) {
            int mid = (left + right) / 2;

            if (arr[mid] == key) {
                return true; // Found
            } else if (arr[mid] < key) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false; // Not found
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Example list of customer account IDs (Should be sorted for binary search)
        int[] customerIDs = {1001, 1010, 1023, 1034, 1048, 1056, 1077};

        System.out.print("Enter customer account ID to search: ");
        int id = sc.nextInt();

        // Linear Search Result
        if (linearSearch(customerIDs, id)) {
            System.out.println("Linear Search: Account ID Found.");
        } else {
            System.out.println("Linear Search: Account ID Not Found.");
        }

        // Binary Search Result
        if (binarySearch(customerIDs, id)) {
            System.out.println("Binary Search: Account ID Found.");
        } else {
            System.out.println("Binary Search: Account ID Not Found.");
        }

        sc.close();
    }
}
3)In a company, employee salaries are stored in a list as floating-point numbers. 
Write a program that sorts the employee salaries in ascending order using the following two sorting algorithms:
• Selection Sort: Sort the salaries using the selection sort algorithm.
• Bubble Sort: Sort the salaries using the bubble sort algorithm.
After sorting the salaries, the program should display the top five highest salaries in the company.
-->
import java.util.*;

public class SalarySorting {

    // Selection Sort
    public static void selectionSort(double[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            double temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }

    // Bubble Sort
    public static void bubbleSort(double[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    double temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }

    // Display Top 5 Highest
    public static void displayTopFive(double[] arr) {
        System.out.println("Top 5 Highest Salaries:");
        int start = Math.max(0, arr.length - 5);
        for (int i = arr.length - 1; i >= start; i--) {
            System.out.println(arr[i]);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of employees: ");
        int n = sc.nextInt();
        double[] salaries = new double[n];

        System.out.println("Enter salaries:");
        for (int i = 0; i < n; i++)
            salaries[i] = sc.nextDouble();

        // Selection Sort
        double[] selArr = salaries.clone();
        selectionSort(selArr);
        System.out.println("\nAfter Selection Sort (Ascending): " + Arrays.toString(selArr));
        displayTopFive(selArr);

        // Bubble Sort
        double[] bubArr = salaries.clone();
        bubbleSort(bubArr);
        System.out.println("\nAfter Bubble Sort (Ascending): " + Arrays.toString(bubArr));
        displayTopFive(bubArr);
    }
}



                                   GroupB
1)Implement a real-time undo/redo system for a text editing application using a Stack data structure.
The system should support the following operations:
• Make a Change: A new change to the document is made.
• Undo Action: Revert the most recent change and store it for potential redo.
• Redo Action: Reapply the most recently undone action.
• Display Document State: Show the current state of the document after undoing or redoing an action.
-->
import java.util.*;

public class TextEditor {

    static Stack<String> undoStack = new Stack<>();
    static Stack<String> redoStack = new Stack<>();
    static String document = ""; 

    // Make a change
    public static void makeChange(String newText) {
        undoStack.push(document);
        document = document + newText;
        redoStack.clear(); 
    }

    // Undo change
    public static void undo() {
        if (!undoStack.isEmpty()) {
            redoStack.push(document);
            document = undoStack.pop();
        } else {
            System.out.println("No actions to undo.");
        }
    }

    // Redo change
    public static void redo() {
        if (!redoStack.isEmpty()) {
            undoStack.push(document);
            document = redoStack.pop();
        } else {
            System.out.println("No actions to redo.");
        }
    }

    // Display document
    public static void display() {
        System.out.println("Current Document: " + document);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n1. Make Change");
            System.out.println("2. Undo");
            System.out.println("3. Redo");
            System.out.println("4. Display Document");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();
            sc.nextLine();

            switch(choice) {
                case 1:
                    System.out.print("Enter text to add: ");
                    String text = sc.nextLine();
                    makeChange(text);
                    break;
                case 2:
                    undo();
                    break;
                case 3:
                    redo();
                    break;
                case 4:
                    display();
                    break;
                case 5:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid choice.");
            }
        } while(choice != 5);

        sc.close();
    }
}

2)Implement a real-time event processing system using a Queue data structure. 
The system should support the following features:
• Add an Event: When a new event occurs, it should be added to the event queue.
• Process the Next Event: The system should process and remove the event that has been in the queue the longest.
• Display Pending Events: Show all the events currently waiting to be processed.
• Cancel an Event: An event can be canceled if it has not yet been processed.
-->
import java.util.*;

public class EventProcessingSystem {

    static Queue<String> eventQueue = new LinkedList<>();

    // Add an event
    public static void addEvent(String event) {
        eventQueue.add(event);
        System.out.println("Event added: " + event);
    }

    // Process next event
    public static void processEvent() {
        if (eventQueue.isEmpty()) {
            System.out.println("No events to process.");
        } else {
            String event = eventQueue.poll();
            System.out.println("Processed event: " + event);
        }
    }

    // Display pending events
    public static void displayEvents() {
        if (eventQueue.isEmpty()) {
            System.out.println("No pending events.");
        } else {
            System.out.println("Pending Events: " + eventQueue);
        }
    }

    // Cancel an event (if still pending)
    public static void cancelEvent(String event) {
        if (eventQueue.remove(event)) {
            System.out.println("Event canceled: " + event);
        } else {
            System.out.println("Event not found or already processed.");
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n1. Add Event");
            System.out.println("2. Process Next Event");
            System.out.println("3. Display Pending Events");
            System.out.println("4. Cancel Event");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();
            sc.nextLine();

            switch(choice) {
                case 1:
                    System.out.print("Enter event name: ");
                    String event = sc.nextLine();
                    addEvent(event);
                    break;

                case 2:
                    processEvent();
                    break;

                case 3:
                    displayEvents();
                    break;

                case 4:
                    System.out.print("Enter event name to cancel: ");
                    String cancel = sc.nextLine();
                    cancelEvent(cancel);
                    break;

                case 5:
                    System.out.println("Exiting...");
                    break;

                default:
                    System.out.println("Invalid choice.");
            }
        } while(choice != 5);

        sc.close();
    }
}
3)A call center receives incoming calls, and each call is assigned a unique customer ID.
The calls are answered in the order they are received. 
Your task is to simulate the call queue of the call center using a Queue data structure.
The system should provide the following operations:
• addCall(customerID, callTime): Add a call to the queue with the customer ID and the call time (in minutes).
• answerCall(): Answer and remove the first call from the queue.
• viewQueue(): View all calls currently in the queue without removing them.
• isQueueEmpty(): Check if the queue is empty.
-->
import java.util.*;

class Call {
    int customerID;
    int callTime;

    Call(int customerID, int callTime) {
        this.customerID = customerID;
        this.callTime = callTime;
    }

    public String toString() {
        return "CustomerID: " + customerID + ", Call Time: " + callTime + " min";
    }
}

public class CallCenterQueue {

    static Queue<Call> callQueue = new LinkedList<>();

    public static void addCall(int customerID, int callTime) {
        callQueue.add(new Call(customerID, callTime));
        System.out.println("Call added to queue.");
    }

    public static void answerCall() {
        if (callQueue.isEmpty()) {
            System.out.println("No calls to answer.");
        } else {
            Call call = callQueue.poll();
            System.out.println("Answered Call -> " + call);
        }
    }

    public static void viewQueue() {
        if (callQueue.isEmpty()) {
            System.out.println("No calls waiting.");
        } else {
            System.out.println("Current Call Queue:");
            for (Call call : callQueue) {
                System.out.println(call);
            }
        }
    }

    public static void isQueueEmpty() {
        if (callQueue.isEmpty()) {
            System.out.println("The call queue is empty.");
        } else {
            System.out.println("The call queue is not empty.");
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n1. Add Call");
            System.out.println("2. Answer Call");
            System.out.println("3. View Queue");
            System.out.println("4. Check if Queue is Empty");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter Customer ID: ");
                    int id = sc.nextInt();
                    System.out.print("Enter Call Time (minutes): ");
                    int time = sc.nextInt();
                    addCall(id, time);
                    break;

                case 2:
                    answerCall();
                    break;

                case 3:
                    viewQueue();
                    break;

                case 4:
                    isQueueEmpty();
                    break;

                case 5:
                    System.out.println("Exiting...");
                    break;

                default:
                    System.out.println("Invalid choice.");
            }

        } while (choice != 5);

        sc.close();
    }
}
4)Create a Student Record Management System using a linked list.
• Use a singly or doubly linked list to store student data (Roll No, Name, Marks).
• Perform operations: Add, Delete, Update, Search, and Sort.
• Display records in ascending or descending order based on marks or roll number.
-->
import java.util.*;

class Student {
    int rollNo;
    String name;
    double marks;
    Student next;

    Student(int rollNo, String name, double marks) {
        this.rollNo = rollNo;
        this.name = name;
        this.marks = marks;
        this.next = null;
    }
}

public class StudentRecordSystem {

    static Student head = null;
    static Scanner sc = new Scanner(System.in);

    // Add Student
    public static void addStudent() {
        System.out.print("Enter Roll No: ");
        int roll = sc.nextInt();
        sc.nextLine();
        System.out.print("Enter Name: ");
        String name = sc.nextLine();
        System.out.print("Enter Marks: ");
        double marks = sc.nextDouble();

        Student newStudent = new Student(roll, name, marks);

        if (head == null) {
            head = newStudent;
        } else {
            Student temp = head;
            while (temp.next != null) temp = temp.next;
            temp.next = newStudent;
        }
        System.out.println("Student Added Successfully.");
    }

    // Display Students
    public static void display() {
        if (head == null) {
            System.out.println("No Records Found.");
            return;
        }
        Student temp = head;
        System.out.println("\nRoll No\tName\tMarks");
        while (temp != null) {
            System.out.println(temp.rollNo + "\t" + temp.name + "\t" + temp.marks);
            temp = temp.next;
        }
    }

    // Search Student by Roll Number
    public static void search() {
        System.out.print("Enter Roll No to Search: ");
        int roll = sc.nextInt();
        Student temp = head;
        while (temp != null) {
            if (temp.rollNo == roll) {
                System.out.println("Record Found: " + temp.rollNo + " " + temp.name + " " + temp.marks);
                return;
            }
            temp = temp.next;
        }
        System.out.println("Record Not Found.");
    }

    // Delete Student
    public static void deleteStudent() {
        System.out.print("Enter Roll No to Delete: ");
        int roll = sc.nextInt();

        if (head == null) {
            System.out.println("No Records.");
            return;
        }

        if (head.rollNo == roll) {
            head = head.next;
            System.out.println("Record Deleted.");
            return;
        }

        Student temp = head;
        while (temp.next != null && temp.next.rollNo != roll) {
            temp = temp.next;
        }

        if (temp.next == null) {
            System.out.println("Record Not Found.");
        } else {
            temp.next = temp.next.next;
            System.out.println("Record Deleted.");
        }
    }

    // Update Student
    public static void updateStudent() {
        System.out.print("Enter Roll No to Update: ");
        int roll = sc.nextInt();
        sc.nextLine();

        Student temp = head;
        while (temp != null) {
            if (temp.rollNo == roll) {
                System.out.print("Enter New Name: ");
                temp.name = sc.nextLine();
                System.out.print("Enter New Marks: ");
                temp.marks = sc.nextDouble();
                System.out.println("Record Updated.");
                return;
            }
            temp = temp.next;
        }
        System.out.println("Record Not Found.");
    }

    // Sort by Marks Ascending
    public static void sortByMarks() {
        if (head == null) return;

        for (Student i = head; i != null; i = i.next) {
            for (Student j = i.next; j != null; j = j.next) {
                if (i.marks > j.marks) {
                    double tempMarks = i.marks; i.marks = j.marks; j.marks = tempMarks;
                    int tempRoll = i.rollNo; i.rollNo = j.rollNo; j.rollNo = tempRoll;
                    String tempName = i.name; i.name = j.name; j.name = tempName;
                }
            }
        }
        System.out.println("Records Sorted by Marks (Ascending).");
    }

    public static void main(String[] args) {

        int choice;
        do {
            System.out.println("\n1. Add Student");
            System.out.println("2. Display Students");
            System.out.println("3. Search Student");
            System.out.println("4. Delete Student");
            System.out.println("5. Update Student");
            System.out.println("6. Sort By Marks Ascending");
            System.out.println("7. Exit");
            System.out.print("Enter Choice: ");
            choice = sc.nextInt();

            switch(choice) {
                case 1: addStudent(); break;
                case 2: display(); break;
                case 3: search(); break;
                case 4: deleteStudent(); break;
                case 5: updateStudent(); break;
                case 6: sortByMarks(); break;
                case 7: System.out.println("Exiting..."); break;
                default: System.out.println("Invalid Choice");
            }
        } while(choice != 7);
    }
}
                      Group c
1)Implement a hash table of size 10 using the division method as the hash function.
Resolve collisions using chaining (linked lists). Provide the following operations:
insert(key, value): Insert a key–value pair.
search(key): Return the value associated with a key (or indicate not found).
delete(key): Remove the key–value pair from the hash table. 
-->
import java.util.*;

class HashTableChaining {
    private static class Entry {
        int key;
        String value;
        Entry(int key, String value) { this.key = key; this.value = value; }
    }

    private final int SIZE = 10;
    private final List<LinkedList<Entry>> buckets;

    public HashTableChaining() {
        buckets = new ArrayList<>(SIZE);
        for (int i = 0; i < SIZE; i++) buckets.add(new LinkedList<>());
    }

    private int hash(int key) {
        int h = key % SIZE;
        return (h < 0) ? h + SIZE : h; // ensure non-negative
    }

    // Insert or update
    public void insert(int key, String value) {
        int idx = hash(key);
        LinkedList<Entry> bucket = buckets.get(idx);
        for (Entry e : bucket) {
            if (e.key == key) { // update
                e.value = value;
                return;
            }
        }
        bucket.add(new Entry(key, value));
    }

    // Search
    public String search(int key) {
        int idx = hash(key);
        for (Entry e : buckets.get(idx)) {
            if (e.key == key) return e.value;
        }
        return null; // not found
    }

    // Delete; returns true if removed
    public boolean delete(int key) {
        int idx = hash(key);
        Iterator<Entry> it = buckets.get(idx).iterator();
        while (it.hasNext()) {
            if (it.next().key == key) {
                it.remove();
                return true;
            }
        }
        return false;
    }

    // Utility: print table
    public void printTable() {
        for (int i = 0; i < SIZE; i++) {
            System.out.print(i + ": ");
            for (Entry e : buckets.get(i)) {
                System.out.print("(" + e.key + "," + e.value + ") -> ");
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        HashTableChaining ht = new HashTableChaining();

        // Inserts
        ht.insert(15, "Alice");
        ht.insert(25, "Bob");     // collision with 15 (same bucket when SIZE=10)
        ht.insert(35, "Carol");   // same bucket again
        ht.insert(7,  "Dan");
        ht.insert(42, "Eve");

        System.out.println("Initial table:");
        ht.printTable();

        // Search
        System.out.println("\nSearch 25 -> " + ht.search(25));
        System.out.println("Search 99 -> " + ht.search(99));

        // Update existing key
        ht.insert(25, "Bob (updated)");
        System.out.println("\nAfter updating key 25:");
        ht.printTable();

        // Delete
        System.out.println("\nDelete 35 -> " + ht.delete(35));
        System.out.println("Delete 99 -> " + ht.delete(99));

        System.out.println("\nFinal table:");
        ht.printTable();
    }
}
2)Design and implement a hash table of fixed size. Use the division method for the hash function and
resolve collisions using linear probing. Allow the user to perform the following operations:
• Insert a key
• Search for a key
• Delete a key
• Display the table
-->
import java.util.*;

public class LinearProbingHashTable {

    static int SIZE = 10;
    static Integer[] hashTable = new Integer[SIZE];

    // Hash Function
    public static int hash(int key) {
        return key % SIZE;
    }

    // Insert Key using Linear Probing
    public static void insert(int key) {
        int index = hash(key);

        for (int i = 0; i < SIZE; i++) {
            int newIndex = (index + i) % SIZE;
            if (hashTable[newIndex] == null || hashTable[newIndex] == -1) {
                hashTable[newIndex] = key;
                System.out.println("Key inserted.");
                return;
            }
        }
        System.out.println("Hash Table is Full. Cannot Insert.");
    }

    // Search Key
    public static void search(int key) {
        int index = hash(key);

        for (int i = 0; i < SIZE; i++) {
            int newIndex = (index + i) % SIZE;
            if (hashTable[newIndex] == null) break;
            if (hashTable[newIndex] != -1 && hashTable[newIndex] == key) {
                System.out.println("Key found at index: " + newIndex);
                return;
            }
        }
        System.out.println("Key not found.");
    }

    // Delete Key
    public static void delete(int key) {
        int index = hash(key);

        for (int i = 0; i < SIZE; i++) {
            int newIndex = (index + i) % SIZE;
            if (hashTable[newIndex] == null) break;
            if (hashTable[newIndex] != -1 && hashTable[newIndex] == key) {
                hashTable[newIndex] = -1; // Mark as deleted
                System.out.println("Key deleted.");
                return;
            }
        }
        System.out.println("Key not found.");
    }

    // Display Table
    public static void display() {
        System.out.println("\nHash Table:");
        for (int i = 0; i < SIZE; i++) {
            System.out.println(i + " -> " + hashTable[i]);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice, key;

        do {
            System.out.println("\n1. Insert");
            System.out.println("2. Search");
            System.out.println("3. Delete");
            System.out.println("4. Display");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter key to insert: ");
                    key = sc.nextInt();
                    insert(key);
                    break;
                case 2:
                    System.out.print("Enter key to search: ");
                    key = sc.nextInt();
                    search(key);
                    break;
                case 3:
                    System.out.print("Enter key to delete: ");
                    key = sc.nextInt();
                    delete(key);
                    break;
                case 4:
                    display();
                    break;
                case 5:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid choice.");
            }
        } while (choice != 5);

        sc.close();
    }
}
3)Implement a hash table using extendible hashing. The hash table should dynamically expand
when the number of keys in a bucket exceeds a certain threshold. Perform the following operations:

• Insert(key): Insert key-value pairs into the hash table
• Search(key): Search for the value associated with a given key
• Delete(key): Delete a key-value pair from the hash table
-->
import java.util.*;

class Bucket {
    int localDepth;
    List<Integer> keys = new ArrayList<>();
    List<String> values = new ArrayList<>();

    Bucket(int depth) {
        this.localDepth = depth;
    }

    boolean isFull(int bucketSize) {
        return keys.size() >= bucketSize;
    }
}

public class ExtendibleHashing {

    static int globalDepth = 1;
    static int bucketSize = 2; // Threshold of keys in each bucket
    static List<Bucket> directory = new ArrayList<>();

    // Initialize directory with 2 buckets
    static {
        directory.add(new Bucket(globalDepth));
        directory.add(new Bucket(globalDepth));
    }

    public static int hash(int key) {
        return key & ((1 << globalDepth) - 1);
    }

    public static void splitBucket(int index) {
        Bucket oldBucket = directory.get(index);
        int newLocalDepth = oldBucket.localDepth + 1;
        Bucket newBucket = new Bucket(newLocalDepth);

        // Update old bucket depth
        oldBucket.localDepth = newLocalDepth;

        // If bucket local depth > global depth, double directory
        if (newLocalDepth > globalDepth) {
            int oldSize = directory.size();
            globalDepth++;
            for (int i = 0; i < oldSize; i++) {
                directory.add(directory.get(i));
            }
        }

        // Reassign directory pointers
        for (int i = 0; i < directory.size(); i++) {
            if (directory.get(i) == oldBucket) {
                if ((i >> (newLocalDepth - 1) & 1) == 1) {
                    directory.set(i, newBucket);
                }
            }
        }

        // Rehash keys
        List<Integer> tempKeys = new ArrayList<>(oldBucket.keys);
        List<String> tempValues = new ArrayList<>(oldBucket.values);
        oldBucket.keys.clear();
        oldBucket.values.clear();

        for (int i = 0; i < tempKeys.size(); i++) {
            insert(tempKeys.get(i), tempValues.get(i));
        }
    }

    public static void insert(int key, String value) {
        int index = hash(key);
        Bucket bucket = directory.get(index);

        if (!bucket.isFull(bucketSize)) {
            bucket.keys.add(key);
            bucket.values.add(value);
        } else {
            splitBucket(index);
            insert(key, value); // Reinsert after split
        }
    }

    public static void search(int key) {
        int index = hash(key);
        Bucket bucket = directory.get(index);
        for (int i = 0; i < bucket.keys.size(); i++) {
            if (bucket.keys.get(i) == key) {
                System.out.println("Key found. Value = " + bucket.values.get(i));
                return;
            }
        }
        System.out.println("Key not found.");
    }

    public static void delete(int key) {
        int index = hash(key);
        Bucket bucket = directory.get(index);
        for (int i = 0; i < bucket.keys.size(); i++) {
            if (bucket.keys.get(i) == key) {
                bucket.keys.remove(i);
                bucket.values.remove(i);
                System.out.println("Key deleted.");
                return;
            }
        }
        System.out.println("Key not found.");
    }

    public static void display() {
        System.out.println("\nGlobal Depth: " + globalDepth);
        for (int i = 0; i < directory.size(); i++) {
            Bucket b = directory.get(i);
            System.out.println("Dir[" + i + "] -> LD: " + b.localDepth + "  Keys: " + b.keys);
        }
    }

    public static void main(String[] args) {
        insert(5, "A");
        insert(7, "B");
        insert(13, "C");
        insert(9, "D");
        insert(21, "E");

        display();
        search(13);
        delete(7);
        display();
    }
}
                               Group D  
1)Consider a particular area in your city. Note the popular locations A, B, C, etc.,
in that area. Assume these locations represent nodes of a graph. If there is a route between two locations,
it is represented as a connection (edge) between the nodes.
Find the order in which these locations are visited starting from location A using:
BFS (Breadth First Search)
DFS (Depth First Search)
Represent the graph:
• Using an adjacency matrix to perform DFS
• Using an adjacency list to perform BFS
-->
import java.util.*;

public class GraphTraversal {

    // ----- DFS using Adjacency Matrix -----
    static void dfs(int[][] adjMatrix, boolean[] visited, int start) {
        visited[start] = true;
        System.out.print((char)(start + 'A') + " ");

        for(int i = 0; i < adjMatrix.length; i++) {
            if(adjMatrix[start][i] == 1 && !visited[i]) {
                dfs(adjMatrix, visited, i);
            }
        }
    }

    // ----- BFS using Adjacency List -----
    static void bfs(List<List<Integer>> adjList, int start) {
        Queue<Integer> queue = new LinkedList<>();
        boolean[] visited = new boolean[adjList.size()];

        queue.add(start);
        visited[start] = true;

        while(!queue.isEmpty()) {
            int node = queue.poll();
            System.out.print((char)(node + 'A') + " ");

            for(int neighbor : adjList.get(node)) {
                if(!visited[neighbor]) {
                    queue.add(neighbor);
                    visited[neighbor] = true;
                }
            }
        }
    }

    public static void main(String[] args) {

        // Example Graph with Nodes A, B, C, D, E
        int n = 5;

        // Adjacency Matrix for DFS
        int[][] adjMatrix = {
                //A B C D E
                {0,1,1,0,0}, // A
                {1,0,0,1,0}, // B
                {1,0,0,1,1}, // C
                {0,1,1,0,1}, // D
                {0,0,1,1,0}  // E
        };

        System.out.println("DFS (Using Adjacency Matrix) starting from A:");
        boolean[] visited = new boolean[n];
        dfs(adjMatrix, visited, 0);

        // Adjacency List for BFS
        List<List<Integer>> adjList = new ArrayList<>();
        for(int i = 0; i < n; i++) adjList.add(new ArrayList<>());

        adjList.get(0).add(1); // A-B
        adjList.get(0).add(2); // A-C
        adjList.get(1).add(3); // B-D
        adjList.get(2).add(3); // C-D
        adjList.get(2).add(4); // C-E
        adjList.get(3).add(4); // D-E

        System.out.println("\n\nBFS (Using Adjacency List) starting from A:");
        bfs(adjList, 0);
    }
}
2)A pizza shop receives multiple orders from several locations. Assume that one pizza delivery boy needs to deliver pizzas to nearby locations,
which are represented using a graph. The time required to travel between two locations represents the edge weight.
Solve the problem of delivering pizza to all customers in the minimum possible time. Use an appropriate data structure.
-->
import java.util.*;

class Node implements Comparable<Node> {
    int vertex, time;

    Node(int vertex, int time) {
        this.vertex = vertex;
        this.time = time;
    }

    public int compareTo(Node other) {
        return this.time - other.time;
    }
}

public class PizzaDelivery {

    static void dijkstra(List<List<Node>> graph, int start) {
        int n = graph.size();
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[start] = 0;

        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.add(new Node(start, 0));

        while (!pq.isEmpty()) {
            Node current = pq.poll();

            for (Node neighbor : graph.get(current.vertex)) {
                int newDist = dist[current.vertex] + neighbor.time;
                if (newDist < dist[neighbor.vertex]) {
                    dist[neighbor.vertex] = newDist;
                    pq.add(new Node(neighbor.vertex, newDist));
                }
            }
        }

        System.out.println("Minimum time to deliver pizza to each location:");
        for (int i = 0; i < n; i++) {
            System.out.println("Location " + (char)(i + 'A') + ": " + dist[i] + " minutes");
        }
    }

    public static void main(String[] args) {

        int n = 5; // Example: A, B, C, D, E
        List<List<Node>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

        // Example graph (Location, Time)
        // A-B(10), A-C(3), C-B(1), C-D(8), B-D(2), D-E(5)
        graph.get(0).add(new Node(1, 10));
        graph.get(0).add(new Node(2, 3));
        graph.get(2).add(new Node(1, 1));
        graph.get(2).add(new Node(3, 8));
        graph.get(1).add(new Node(3, 2));
        graph.get(3).add(new Node(4, 5));

        // Start delivery from location A (index 0)
        dijkstra(graph, 0);
    }
}
3)Implement various operations on a Binary Search Tree (BST), such as insertion, deletion, display, and search.
-->
import java.util.*;

class Node {
    int data;
    Node left, right;

    Node(int data) {
        this.data = data;
        left = right = null;
    }
}

public class BSTOperations {

    static Node root = null;

    // Insert a node
    public static Node insert(Node root, int data) {
        if (root == null) {
            return new Node(data);
        }
        if (data < root.data)
            root.left = insert(root.left, data);
        else if (data > root.data)
            root.right = insert(root.right, data);
        return root;
    }

    // Search a node
    public static boolean search(Node root, int key) {
        if (root == null) return false;
        if (root.data == key) return true;
        return (key < root.data) ? search(root.left, key) : search(root.right, key);
    }

    // Find minimum value node
    public static Node findMin(Node root) {
        while (root.left != null)
            root = root.left;
        return root;
    }

    // Delete a node
    public static Node delete(Node root, int key) {
        if (root == null) return null;

        if (key < root.data) {
            root.left = delete(root.left, key);
        } else if (key > root.data) {
            root.right = delete(root.right, key);
        } else {
            // Node to delete found
            if (root.left == null)
                return root.right;
            else if (root.right == null)
                return root.left;

            Node minNode = findMin(root.right);
            root.data = minNode.data;
            root.right = delete(root.right, minNode.data);
        }
        return root;
    }

    // Display BST (Inorder Traversal)
    public static void display(Node root) {
        if (root != null) {
            display(root.left);
            System.out.print(root.data + " ");
            display(root.right);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice, value;

        do {
            System.out.println("\n1. Insert");
            System.out.println("2. Delete");
            System.out.println("3. Search");
            System.out.println("4. Display (Inorder)");
            System.out.println("5. Exit");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter value to insert: ");
                    value = sc.nextInt();
                    root = insert(root, value);
                    break;

                case 2:
                    System.out.print("Enter value to delete: ");
                    value = sc.nextInt();
                    root = delete(root, value);
                    break;

                case 3:
                    System.out.print("Enter value to search: ");
                    value = sc.nextInt();
                    System.out.println(search(root, value) ? "Found" : "Not Found");
                    break;

                case 4:
                    System.out.println("BST (Inorder Traversal): ");
                    display(root);
                    System.out.println();
                    break;

                case 5:
                    System.out.println("Exiting...");
                    break;

                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 5);

        sc.close();
    }
}
4)Construct an expression tree from the given prefix expression (for example: +-*a bc / d e f).
Traverse the expression tree using post-order traversal (non-recursive), and then delete the entire tree.
-->
import java.util.*;

class Node {
    char data;
    Node left, right;

    Node(char data) {
        this.data = data;
        this.left = this.right = null;
    }
}

public class ExpressionTree {

    // Function to check if character is operator
    static boolean isOperator(char c) {
        return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
    }

    // Construct expression tree from prefix expression
    public static Node constructTree(String prefix) {
        Stack<Node> stack = new Stack<>();

        // Scan expression from right to left
        for (int i = prefix.length() - 1; i >= 0; i--) {
            char c = prefix.charAt(i);

            // Operand -> Create node and push
            if (!isOperator(c)) {
                stack.push(new Node(c));
            } else {
                // Operator -> Pop two nodes
                Node left = stack.pop();
                Node right = stack.pop();
                Node newNode = new Node(c);
                newNode.left = left;
                newNode.right = right;
                stack.push(newNode);
            }
        }
        return stack.pop();
    }

    // Non-recursive postorder traversal (using two stacks)
    public static void postOrderNonRecursive(Node root) {
        if (root == null) return;

        Stack<Node> s1 = new Stack<>();
        Stack<Node> s2 = new Stack<>();

        s1.push(root);
        while (!s1.isEmpty()) {
            Node temp = s1.pop();
            s2.push(temp);

            if (temp.left != null)
                s1.push(temp.left);
            if (temp.right != null)
                s1.push(temp.right);
        }

        System.out.print("Postorder Traversal: ");
        while (!s2.isEmpty()) {
            System.out.print(s2.pop().data + " ");
        }
        System.out.println();
    }

    // Delete entire tree
    public static Node deleteTree(Node root) {
        root = null;  // Garbage collector will free memory
        System.out.println("Expression Tree Deleted.");
        return root;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter Prefix Expression: ");
        String prefix = sc.nextLine().replace(" ", ""); // remove spaces

        Node root = constructTree(prefix);
        postOrderNonRecursive(root);

        root = deleteTree(root);
    }
}
5)A list stores city names and their populations. Use a Binary Search Tree (BST) for implementation.
Provide features to:
• Add a new city
• Delete a city
• Update a city’s population
• Display all city names in ascending/descending order
Also, determine how many maximum comparisons are required to search for a particular city.
-->
import java.util.*;

class CityNode {
    String name;
    int population;
    CityNode left, right;

    CityNode(String name, int population) {
        this.name = name;
        this.population = population;
        left = right = null;
    }
}

public class CityBST {

    static CityNode root = null;

    // Insert a city
    public static CityNode insert(CityNode root, String name, int pop) {
        if (root == null) return new CityNode(name, pop);

        if (name.compareTo(root.name) < 0)
            root.left = insert(root.left, name, pop);
        else if (name.compareTo(root.name) > 0)
            root.right = insert(root.right, name, pop);

        return root;
    }

    // Search city and return comparisons count
    public static int search(CityNode root, String name) {
        int comparisons = 0;
        CityNode temp = root;

        while (temp != null) {
            comparisons++;
            if (name.equals(temp.name)) {
                System.out.println("City Found: " + temp.name + " | Population: " + temp.population);
                System.out.println("Comparisons Required: " + comparisons);
                return comparisons;
            }
            if (name.compareTo(temp.name) < 0)
                temp = temp.left;
            else
                temp = temp.right;
        }
        System.out.println("City Not Found.");
        System.out.println("Comparisons Required: " + comparisons);
        return comparisons;
    }

    // Find min node
    public static CityNode findMin(CityNode root) {
        while (root.left != null)
            root = root.left;
        return root;
    }

    // Delete city
    public static CityNode delete(CityNode root, String name) {
        if (root == null) return null;

        if (name.compareTo(root.name) < 0)
            root.left = delete(root.left, name);
        else if (name.compareTo(root.name) > 0)
            root.right = delete(root.right, name);
        else {
            // Node found
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;

            CityNode minNode = findMin(root.right);
            root.name = minNode.name;
            root.population = minNode.population;
            root.right = delete(root.right, minNode.name);
        }
        return root;
    }

    // Update population
    public static void update(CityNode root, String name, int newPop) {
        CityNode temp = root;

        while (temp != null) {
            if (name.equals(temp.name)) {
                temp.population = newPop;
                System.out.println("Population Updated Successfully.");
                return;
            }
            if (name.compareTo(temp.name) < 0)
                temp = temp.left;
            else
                temp = temp.right;
        }
        System.out.println("City Not Found.");
    }

    // Display Ascending (Inorder)
    public static void displayAscending(CityNode root) {
        if (root != null) {
            displayAscending(root.left);
            System.out.println(root.name + " - " + root.population);
            displayAscending(root.right);
        }
    }

    // Display Descending
    public static void displayDescending(CityNode root) {
        if (root != null) {
            displayDescending(root.right);
            System.out.println(root.name + " - " + root.population);
            displayDescending(root.left);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;
        String city;
        int pop;

        do {
            System.out.println("\n1. Add City");
            System.out.println("2. Delete City");
            System.out.println("3. Update Population");
            System.out.println("4. Search City");
            System.out.println("5. Display Ascending Order");
            System.out.println("6. Display Descending Order");
            System.out.println("7. Exit");
            System.out.print("Enter Choice: ");
            choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {
                case 1:
                    System.out.print("Enter City Name: ");
                    city = sc.nextLine();
                    System.out.print("Enter Population: ");
                    pop = sc.nextInt();
                    root = insert(root, city, pop);
                    break;

                case 2:
                    System.out.print("Enter City Name to Delete: ");
                    city = sc.nextLine();
                    root = delete(root, city);
                    break;

                case 3:
                    System.out.print("Enter City Name to Update: ");
                    city = sc.nextLine();
                    System.out.print("Enter New Population: ");
                    pop = sc.nextInt();
                    update(root, city, pop);
                    break;

                case 4:
                    System.out.print("Enter City Name to Search: ");
                    city = sc.nextLine();
                    search(root, city);
                    break;

                case 5:
                    System.out.println("\nCities (Ascending):");
                    displayAscending(root);
                    break;

                case 6:
                    System.out.println("\nCities (Descending):");
                    displayDescending(root);
                    break;

                case 7:
                    System.out.println("Exiting...");
                    break;

                default:
                    System.out.println("Invalid Choice.");
            }
        } while (choice != 7);
    }
}
6)Read a propositional logic formula.
Write a function that reads the formula and constructs its binary expression tree. 
Then state the time complexity of the function.
-->
import java.util.*;

class Node {
    String value;
    Node left, right;

    Node(String value) {
        this.value = value;
        left = right = null;
    }
}

public class LogicExpressionTree {

    // Operator precedence
    static int precedence(String op) {
        switch (op) {
            case "~": return 3; // NOT
            case "&": return 2; // AND
            case "|": return 1; // OR
            case ">": return 0; // IMPLICATION
        }
        return -1;
    }

    // Convert infix formula to postfix using stack
    static List<String> infixToPostfix(String expr) {
        Stack<String> stack = new Stack<>();
        List<String> output = new ArrayList<>();

        for (int i = 0; i < expr.length(); i++) {
            char c = expr.charAt(i);

            if (Character.isLetter(c)) {
                output.add(c + "");
            } 
            else if (c == '(') {
                stack.push(c + "");
            } 
            else if (c == ')') {
                while (!stack.isEmpty() && !stack.peek().equals("("))
                    output.add(stack.pop());
                stack.pop();
            } 
            else { // operator
                String op = c + "";
                while (!stack.isEmpty() && precedence(op) <= precedence(stack.peek()))
                    output.add(stack.pop());
                stack.push(op);
            }
        }

        while (!stack.isEmpty())
            output.add(stack.pop());

        return output;
    }

    // Build Expression Tree from postfix
    static Node buildTree(List<String> postfix) {
        Stack<Node> stack = new Stack<>();

        for (String token : postfix) {
            if (Character.isLetter(token.charAt(0))) {
                stack.push(new Node(token));
            } else {
                Node node = new Node(token);
                if (!token.equals("~")) { // Binary operator
                    node.right = stack.pop();
                    node.left = stack.pop();
                } else { // Unary operator
                    node.right = stack.pop();
                }
                stack.push(node);
            }
        }
        return stack.pop();
    }

    // Postorder Traversal (Print Tree)
    static void postOrder(Node root) {
        if (root == null) return;
        postOrder(root.left);
        postOrder(root.right);
        System.out.print(root.value + " ");
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter propositional formula: ");
        String formula = sc.nextLine().replace(" ", "");

        List<String> postfix = infixToPostfix(formula);
        Node root = buildTree(postfix);

        System.out.print("Postorder expression (Tree representation): ");
        postOrder(root);
        System.out.println();
    }
}
