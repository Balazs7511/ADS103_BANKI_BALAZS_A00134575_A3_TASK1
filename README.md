using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Globalization;
using System.Linq;
using System.Security.Cryptography.X509Certificates;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace ADS103_Banki_Balazs_A3_Task1
{
    internal class Program
    {
        static void Main(string[] args)
        {

            List<int> numbers = new List<int>();                            // initializing list named numbers to store random numbers
            BinarySearchTree binarySearchTree = new BinarySearchTree();     // initializing binary search tree to store random numbers
            AVLtree avlTree = new AVLtree();                                // initializing AVL tree to store random numbers
            Random random = new Random();                                   // creating random object of random class to generate random numbers
            Stopwatch stopwatch = new Stopwatch();                          // instantiating stopwatch from stopwatch class
            int totalNumbers = 150000;                                          // number of random numbers to be generated
            int searchNumber;                                               // number to be searched in the data structures
            int r1,r2, r3;                                                  // random numbers to be inserted into the data structures

            for (int i = 0; i < totalNumbers; i++)                          // for loop for generating random numbers
            {

                r1 = r2 = r3 = random.Next(1,999999);                 // assigning the same random numbers to r1, r2 and r3 between 1 & 999.999
                binarySearchTree.Insert(r1);                                // inserting random number into binary search tree
                avlTree.Insert(r2);                                         // inserting random number into AVL tree
                numbers.Add(r3);                                            // adding random number to the list

            }

            Menu();                                                         // calling menu function to display menu options


            void Menu ()                                                    // creating method called menu to interact with user
            {
                Console.WriteLine(" Welcome to the algorithm search application\n");
                Console.WriteLine("Please select an option from the below menu:\n"); // prompting user to select an option
                Console.WriteLine("1. Search for a number in all data structures\n"); // option to search for a number
                Console.WriteLine("2. Exit\n");                                       // option to exit the program
                int option = int.Parse(Console.ReadLine());                           // reading user input and parsing it to integer
                switch (option)                                                  // switch case to perform different operations based on user input
                {
                    case 1:                                                                 // if user selects option 1
                        Console.WriteLine("Enter a number you want to search for :\n"); // prompting user to enter a number
                        searchNumber = int.Parse(Console.ReadLine());                   // reading user input and parsing it to integer
                        stopwatch.Start();                                               // starting the stopwatch to measure time taken for search
                        Thread.Sleep(10000);                    // suspending the current thread for 10000 millisecond
                        if ( numbers.Contains(searchNumber))                        // checking if the number is in the list data structure
                        {
                            Console.WriteLine("The number is in the list \n");  // if the number is in the list, display message
                        }
                        else
                        {
                            Console.WriteLine("The number is not in the list \n"); // if the number is not in the list, display message
                        }
                        stopwatch.Stop();                        // stopping the stopwatch
                        TimeSpan ts1 = stopwatch.Elapsed;                                   // Get the elapsed time as a TimeSpan value
                        string elapsedTime1 = String.Format("{0:00}:{1:00}:{2:00}.{3:00}",  // Formating the TimeSpan value as string
                        ts1.Hours, ts1.Minutes, ts1.Seconds,
                        ts1.Milliseconds / 10);
                        Console.WriteLine("Time taken to search in list : " + elapsedTime1); // displaying time taken to search in list
                        stopwatch.Reset();                                               // resetting the stopwatch

                        stopwatch.Start();                                               // starting the stopwatch to measure time taken for search
                        Thread.Sleep(10000);                    // suspending the current thread for 10000 millisecond
                        binarySearchTree.Search(searchNumber);  // searching for the number in binary search tree by calling search method
                        stopwatch.Stop();                        // stopping the stopwatch
                        TimeSpan ts2 = stopwatch.Elapsed;                                   // Get the elapsed time as a TimeSpan value
                        string elapsedTime2 = String.Format("{0:00}:{1:00}:{2:00}.{3:00}",  // Formating the TimeSpan value as string
                        ts2.Hours, ts2.Minutes, ts2.Seconds,
                        ts2.Milliseconds / 10);
                        Console.WriteLine("Time taken to search in BST : " + elapsedTime2); // displaying time taken to search in list
                        stopwatch.Reset();                                               // resetting the stopwatch

                        stopwatch.Start();                                               // starting the stopwatch to measure time taken for search
                        Thread.Sleep(10000);                    // suspending the current thread for 10000 millisecond
                        avlTree.Search(searchNumber);           // searching for the number in AVL tree by calling search method
                        stopwatch.Stop();                        // stopping the stopwatch
                        TimeSpan ts3 = stopwatch.Elapsed;                                   // Get the elapsed time as a TimeSpan value
                        string elapsedTime3 = String.Format("{0:00}:{1:00}:{2:00}.{3:00}",  // Formating the TimeSpan value as string
                        ts3.Hours, ts3.Minutes, ts3.Seconds,
                        ts3.Milliseconds / 10);
                        Console.WriteLine("Time taken to search in balanced BST : " + elapsedTime3); // displaying time taken to search in list
                        Menu();                                 // calling menu method recursively to display options
                        break;                                  // break statement to exit the switch case
                    case 2:                                     // exit option
                        Environment.Exit(0);                    // exiting the program
                        break;                                  // break statement to exit the switch case
                    default:                                                        // if user enters invalid option
                        Console.WriteLine("Invalid option. Please try again.\n");   // displaying message
                        Menu();                                                    // calling menu method recursively to display options
                        break;                                  // break statement to exit the switch case
                }
            }
        }
        public class Node                       // defining class node to create nodes for the binary search tree and AVL tree
        {   // node class property created below :
            public Node Left { get; set; }      //  left child of the node
            public Node Right { get; set; }     // right child of the node
            public int Value { get; set; }      // value of the node
            public int Height { get; set; }     // height of the node for AVL tree

        }
        public class BinarySearchTree           // defining class binary search tree
        {   // binary search tree class property created below :
            public Node root {  get; set; }     // root of the binary search tree
            public BinarySearchTree()           // constructor for binary search tree
            {                                   // constructor initializes the root to null
                root = null;
            }
            virtual public void Insert(int value)   // creating insert method to insert values into the binary search tree
            {
                root = Insert(root, value);         // assigning value to the root
            }
            virtual public Node Insert(Node node, int value) // creating insert method to insert values into the binary search tree
            {
                if (node == null)                   // if the node is null, create a new node
                {
                    node = new Node();                      // creating a new node
                    node.Value = value;                     // assigning value to the node
                }
                else if (value < node.Value)                // if the value is less than the node value
                {
                    node.Left = Insert(node.Left, value);   // insert the value into the left subtree
                }
                else if (value > node.Value)                // if the value is greater than the node value
                {
                    node.Right = Insert(node.Right, value); // insert the value into the right subtree
                }
                return node;                                // return the node
            }

            virtual public void Search(int value)                           // creating search method to search for a value in the binary search tree 
            {
                Node current = root;                                        // assigning current node to root
                while (current != null)                                     // while loop to traverse the tree if current node is not null
                {
                    if (value == current.Value)                             // if the value is equal to the current node value
                    {
                        Console.WriteLine("The number is in the BST\n");    // display message
                        return;                                             // return from the method no need to search any further
                    }
                    else if (value < current.Value)                         // if the value is less than the current node value
                    {
                        current = current.Left;                             // traverse to the left subtree
                    }
                    else if ( value > current.Value )                       // if the value is greater than the current node value      
                    {
                        current = current.Right;                            // traverse to the right subtree
                    }
                }
                Console.WriteLine("The number is not in the BST\n");  // if  none of the above conditions are met number is not found, display message
            }

        }
        public class AVLtree : BinarySearchTree         // defining class AVL tree which inherits from binary search tree
        {
            //public Node root { get; set; }              // root of the AVL tree

            public int GetBalance(Node node)                            // method of calculating the balance factor of the node
            {
                if (node == null)                                       // if the node is null
                {
                    return 0;                                           // return 0
                }
                return GetHeight(node.Left) - GetHeight(node.Right); 
                // if the node is not null, return the difference between the height of the left and right subtrees   
            }
            public int GetHeight(Node node)                         // method of calculating the height of the node
            {
                if (node == null)                                   // if the node is null
                {
                    return 0;                                       // return 0
                }
                else {                                              // if the node is not null
                    int leftHeight = GetHeight(node.Left);          // get the height of the left subtree & assigning as integer
                    int rightHeight = GetHeight(node.Right);        // get the height of the right subtree & assigning as integer
                    return 1 + Math.Max(leftHeight, rightHeight);   // return the maximum height of the left and right subtrees + 1
                }
            }
            public Node RightRotate(Node y)                         // method of right rotation
            {                                           // right rotation is performed when the left subtree is heavier than the right subtree
                Node x = y.Left;                                                 // assigning left child of y to x
                Node T2 = x.Right;                                               // assigning right child of x to T2
                x.Right = y;                                                     // assigning y to right child of x
                y.Left = T2;                                                     // assigning T2 to left child of y
                y.Height = 1 + Math.Max(GetHeight(y.Left), GetHeight(y.Right));  // updating height of y
                x.Height = 1 + Math.Max(GetHeight(x.Left), GetHeight(x.Right));  // updating height of x
                return x;                                                           // return x as new root
            }
            public Node LeftRotate(Node x)                          // method of left rotation
            {                                   // left rotation is performed when the right subtree is heavier than the left subtree
                Node y = x.Right;                                               // assigning right child of x to y
                Node T2 = y.Left;                                               // assigning left child of y to T2
                y.Left = x;                                                     // assigning x to left child of y
                x.Right = T2;                                                   // assigning T2 to right child of x
                x.Height = 1 + Math.Max(GetHeight(x.Left), GetHeight(x.Right)); // updating height of x
                y.Height = 1 + Math.Max(GetHeight(y.Left), GetHeight(y.Right));  // updating height of y
                return y;                                                       // return y as new root
            }
            public AVLtree()                            // constructor for AVL tree
            {
                root = null;                                // initializing root to null
            }
            override public void Insert(int value)              // creating insert method to insert values into AVL tree
            {
                root = Insert(root, value);                     // assigning value to the root
            }
            override public Node Insert(Node node, int value)   // creating insert method to insert values into AVL tree
            {
                if (node == null)                               // if the node is null
                {
                    node = new Node();                          // creating a new node
                    node.Value = value;                         // assigning value to the node
                    node.Left = null;                           // assigning left child of node to null
                    node.Right = null;                          // assigning right child of node to null
                    node.Height = 1;                            // assigning height of node to 1
                }
                else if (value < node.Value)                    // if the value is less than the node value
                {
                    node.Left = Insert(node.Left, value);       // insert the value into the left subtree calling insert method recursively
                }
                else if (value > node.Value)                    // if the value is greater than the node value
                {
                    node.Right = Insert(node.Right, value);     // insert the value into the right subtree calling insert method recursively
                }
                node.Height = Math.Max(GetHeight(node.Left), GetHeight(node.Right));        // updating height of node
                int balance = GetBalance(node);                 // calculating balance factor of node & assigning to balance variable
                if (balance > 1 && value < node.Left.Value)     // if the balance factor is greater than 1 and value is less than the left child value
                {
                    return RightRotate(node);                   // perform right rotation
                }
                if (balance < -1 && value > node.Right.Value)   // if the balance factor is less than -1 and value is greater than the right child value
                {
                    return LeftRotate(node);                    // perform left rotation
                }
                if (balance > 1 && value > node.Left.Value) // if the balance factor is greater than 1 and value is greater than the left child value
                {
                    node.Left = LeftRotate(node.Left);      // perform left rotation on left child
                    return RightRotate(node);                   // perform right rotation
                }
                if (balance < -1 && value < node.Right.Value) // if the balance factor is less than -1 and value is less than the right child value
                {
                    node.Right = RightRotate(node.Right);   //  perform right rotation on right child
                    return LeftRotate(node);                // perform left rotation
                }
                return node;                                // return node
            }
            override public void Search(int value)                              // creating search method to search for a value in the binary search tree
            {
                Node current = root;                                            // assigning the current node to root
                while (current != null)                                        // while loop to traverse the tree if current node is not null 
                {
                    if (value == current.Value)                                 // if the value is equal to the current node value
                    {                                                               // the number is found in the balanced bst
                        Console.WriteLine("The number is in the balanced BST\n");  // display message
                        return;                                                     // return from the method no need to search any further
                    }
                    else if (value < current.Value)                             // if the value is less than the current node value
                    {
                        current = current.Left;                                 // traverse to the left subtree
                    }
                    else if (value > current.Value)                            // if the value is greater than the current node value             
                    {
                        current = current.Right;                               // traverse to the right subtree
                    }
                }
                Console.WriteLine("The number is not in the balanced BST\n"); // if  none of the above conditions are met number is not found, display message
            }
        }
    }
}
