ADJACENCY MATRIX

import java.util.*;

public class adjacency_matrix {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        adjacency_matrix ob = new adjacency_matrix();
        System.out.println("enter the number of vertices");
        int vertices = sc.nextInt();
        int arr[][] = new int[vertices][vertices];
        int choice = 1;
        while (choice > 0) {
            System.out.println("enter the edge from a to b");
            int a = sc.nextInt(), b = sc.nextInt();
            ob.insert(arr, a, b);
            System.out.println("enter 1 if u want to insert further");
            choice = sc.nextInt();

        }
        for (int i = 0; i < vertices; i++) {
            for (int j = 0; j < vertices; j++)
                System.out.print(arr[i][j] + " ");
            System.out.println();
        }
        int v = 2;
        boolean visited[] = new boolean[vertices];
        ob.dfs(arr, 2, vertices,visited);
    }
    void dfs(int arr[][],int v,int vertices,boolean visited[])
    {

        System.out.println(v);
        visited[v]=true;
        for (int i=0;i<vertices;i++)
        {
            if(arr[v][i]==1 && visited[i]==false)
            {
                visited[i]=false;
                dfs(arr, i, vertices, visited);
            }

        }


    }
    void bfs(int arr[][], int v, int vertices)
    {
        boolean visited[] = new boolean[vertices];
        visited[v] = true;
        Queue<Integer> q = new LinkedList<Integer>();
        q.add(v);
        while (!q.isEmpty()) {
            v = q.poll();
            System.out.println(v);
            for (int i = 0; i < vertices; i++) {
                if (arr[v][i] == 1 && visited[i] == false) {
                    q.add(i);
                    visited[i] = true;

                }
            }
        }
    }

    void insert(int arr[][], int a, int b) {
        arr[a][b] = 1;

    }
}

ASSIGNMENT PROBLEM

import java.util.*;

public class assignment {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        assignment ob = new assignment();
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>();
        int arr[][] = {{20, 15, 18, 20, 25}, {18, 20, 12, 14, 15}, {21, 23, 25, 27, 25}, {17, 18, 21, 23, 20}, {18, 18, 16, 19, 20}};
        int min = Integer.MAX_VALUE;
        int pos = 0;
        int n = 5;
        boolean pick[][] = new boolean[5][5];
        ob.find(arr, min, pos, n, pick, pq);
        System.out.print(pq.peek());

    }

    void find(int arr[][], int min, int pos, int n, boolean pick[][], PriorityQueue<Integer> pq) {
        int pos_min = 0;
        int temp_min = Integer.MAX_VALUE;
        if (pos == n)
            return;
        for (int i = 0; i < n; i++) {
            if (is_valid(arr, i, pos, pick)) {
                pick[pos][i] = true;
                min = minimum(arr, n, i, pos, pick);

                pq.add(min);
                if (min <= temp_min) {
                    temp_min = min;
                    pos_min = i;
                }
                pick[pos][i] = false;
            }
        }
        if (pos != n - 1)
            pq.poll();
        pick[pos][pos_min] = true;
        min = Integer.MAX_VALUE;
        temp_min = Integer.MAX_VALUE;
        find(arr, min, pos + 1, n, pick, pq);

    }

    boolean is_valid(int arr[][], int i, int pos, boolean pick[][]) {
        for (int check = 0; check < pos; check++) {
            if (pick[check][i] == true) {
                return false;
            }
        }
        return true;
    }

    int minimum(int arr[][], int n, int i, int pos, boolean pick[][]) {
        int min = 0;
        for (int k = 0; k < pos; k++) {
            for (int j = 0; j < n; j++) {
                if (pick[k][j] == true)
                    min = min + arr[k][j];
            }
        }
        int add = 1;
        min = min + arr[pos][i];
        for (int k = pos + 1; k < n; k++) {
            int temp_min = Integer.MAX_VALUE;
            for (int j = 0; j < n; j++) {
                if (arr[k][j] < temp_min) {
                    for (int check = 0; check < pos + 1; check++) {
                        if (pick[check][j] == true) {
                            add = 0;
                            break;
                        }
                    }
                    if (add == 1)
                        temp_min = arr[k][j];
                    add = 1;
                }
            }
            min = min + temp_min;
        }
        return min;
    }
}

BINARY TREE

import java.util.Scanner;
import java.util.Stack;
import java.util.Queue;
import java.util.LinkedList;

public class binary_tree {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        binary_tree ob = new binary_tree();
        node n = null;
      /*  n = ob.insert(n);
        ob.inorder(n);
        System.out.println();
        System.out.println("enter the value");
        int val = sc.nextInt();
        System.out.println(ob.search(n, val));
       ob.iterpostorder(n);*/
        System.out.println("enter the given postfix expression");
        String exp = sc.next();
        n = ob.evaluate(exp);
        ob.inorder(n);
        System.out.println();
        int answer=ob.bodmas(n);
System.out.println(answer);
    }
    int toint(char s)
    {
      return (int)s-48;
    }
    int bodmas(node n)
    {
        if (n==null)
            return 0;
       if (n.left==null && n.right==null)
           return toint(n.c);
       int left=bodmas(n.left);
        int right=bodmas(n.right);
        if (n.c=='+')
            return left+right;
        if (n.c=='-')
            return left-right;
        if (n.c=='*')
            return left*right;

            return left/right;

    }

    node evaluate(String str) {
        Stack<node> s = new Stack<node>();
        for (int i = 0; i < str.length(); i++) {
            while (operand(str.charAt(i))) {
                node temp = new node();
                temp.c = str.charAt(i++);
                s.push(temp);
            }
            node temp = new node();
            temp.c = str.charAt(i);
            node pop1 = s.pop();

            node pop2 = s.pop();


            temp.right = pop1;
            temp.left = pop2;
            s.push(temp);

        }
        return s.pop();
    }

    boolean operand(char car) {
        if (((int) car >= 48 && (int) car <= 57))
            return true;
        else return false;
    }

    void iterpostorder(node n) {
        Stack<node> stk = new Stack<>();
        node current = n;
        while (stk.size() > 0) {
            if (!(stk.empty())) {

            }


        }
    }

    void iterpreorder(node n) {
        Stack<node> nodeStack = new Stack<node>();
        nodeStack.push(n);


        while (nodeStack.empty() == false) {
            node mynode = nodeStack.pop();
            System.out.print(mynode.data + " ");

            if (mynode.right != null) {
                nodeStack.push(mynode.right);
            }
            if (mynode.left != null) {
                nodeStack.push(mynode.left);
            }
        }

    }

    void iterinorder(node n) {
        Stack<node> s = new Stack<>();
        node current = n;
        while (current != null || s.size() > 0) {
            while (current != null) {
                s.push(current);
                current = current.left;
            }
            current = s.pop();
            System.out.print(current.data + " ");
            current = current.right;
        }

    }

    void iterlevelorder(node n) {
        Queue<node> q = new LinkedList<node>();
        q.add(n);
        while (!(q.isEmpty())) {
            node current = q.poll();
            System.out.println(current.data);
            if (current.left != null)
                q.add(current.left);
            if (current.right != null)
                q.add(current.right);
        }

    }

    void inorder(node n) {
        if (n == null)
            return;
        inorder(n.left);
        System.out.print(n.c + " ");
        inorder(n.right);

    }

    boolean search(node n, int val) {
        if (n == null)
            return false;
        else if (n.data == val)
            return true;
        boolean leftres = search(n.left, val);
        if (leftres)
            return true;
        boolean rightres = search(n.right, val);
        return rightres;
    }

    node insert(node n) {
        Scanner sc = new Scanner(System.in);
        if (n == null) {
            System.out.println("enter the value to be inserted ");
            node temp = new node();
            temp.data = sc.nextInt();
            if (temp.data == -1)
                return null;
            n = temp;

        }
        n.left = insert(n.left);
        n.right = insert(n.right);
        return n;
    }

}

BST

import java.util.Scanner;

public class bst {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        bst ob = new bst();
        node n = null;
        int choice = 1;
        while (choice > 0) {

            System.out.println("enter the value to be inserted ");
            int val = sc.nextInt();
            n = ob.insert(n, val);
            System.out.println("enter the choice ");
            choice = sc.nextInt();
        }
        ob.inorder(n);
        System.out.println();
       /* System.out.println("enter the node to be deleted");
        int val=sc.nextInt();
        n=ob.delete(n,val);*/
       int c=ob.leaf(n);
       System.out.println(c);
    }
    int leaf(node n)
    {
        if (n == null)
            return 0;
        if(n.left==null && n.right==null)
            return (1);
        int left =leaf(n.left);
         int right=leaf(n.right);


return (right+left);
    }



    int count(node n) {
        if (n == null)
            return 0;
        else {
            int lheight = count(n.left);
            int rheight = count(n.right);

            return (rheight + lheight + 1);

        }

    }

    node insert(node n, int val) {

        if (n==null) {
            node temp=new node();
            temp.data=val;
            n = temp;
            return n;
        }
        if (val<n.data)
            n.left=insert(n.left,val);
                    else
            n.right=insert(n.right,val);
                    return n;



    }


    int height(node n) {
        if (n == null)
            return 0;
        else {
            int rheight = height(n.left);
            int lheight = height(n.right);
            if (lheight > rheight)
                return (lheight + 1);
            else return (rheight + 1);
        }
    }


    void inorder(node n) {
        if (n == null)
            return;
        inorder(n.left);
        System.out.print(n.data + " ");
        inorder(n.right);
    }

    node delete(node n, int val)
    {if (n == null)
        return n;
    else if (val < n.data)
        n.left = delete(n.left, val);
    else if (val > n.data)
        n.right = delete(n.right, val);
    else {
        if (n.left == null)
            return n.right;
        else if (n.right == null)
            return n.left;
        n.data = minValue(n.right);
        n.right = delete(n.right, n.data);
    }
        return n;
    }





    int minValue(node n) {
        int minv = n.data;
        while (n.left != null) {
            minv = n.left.data;
            n = n.left;
        }
        return minv;
    }
}



CHAIN MAP

import java.util.Iterator;
import java.util.Scanner;

public class chain_map {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        System.out.println("enter the number of objects");
        int num = sc.nextInt();
        node n[] = new node[num];
        for (int i = 0; i < num; i++)
            n[i] = null;
        chain_map ob = new chain_map();
        int choice = 1;

        while (choice > 0) {
            System.out.println("enter the value with the key to be inserted");
            int val = sc.nextInt(), key = sc.nextInt();
            ob.insert(n, val, key);
            ob.display(n);
            System.out.println("enter 1 if u want to insert further");
            choice = sc.nextInt();
        }
    }
    void display(node n[]) {
        int k = 0;
        while (k != n.length) {
            if (n[k] != null) {
                node head = n[k];
                System.out.println("values at node " + k + " with key " + n[k].data + " is ");
                n[k] = n[k].next;
                while (n[k] != null) {
                    System.out.println("->" + n[k].data);
                    n[k] = n[k].next;
                }
                n[k] = head;
            }
            k++;
        }
    }

    void insert(node n[], int val, int key) {
        int address = key % n.length;
        if (n[address] == null) {
            node temp = new node();
            temp.data = key;
            temp.next = new node();
            n[address] = temp;
            temp = new node();
            temp.data = val;
            n[address].next = temp;
        } else {
            node head = n[address];
            while (n[address].next != null) {
                n[address] = n[address].next;
            }
            node temp = new node();
            temp.data = val;
            n[address].next = temp;

            n[address] = head;

        }

    }
}

CHECK CYCLE

import java.util.Scanner;

public class check_cycle {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        check_cycle ob = new check_cycle();

        node n = null;
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println(" 1 to insert into  list with no cycles 2 if you want the list with cycle");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n = ob.insert(n, val);
                    ob.display(n);
                    break;
                }
                case 2: {
                    System.out.println("enter the node at which you want the cycle ");
                    int num = sc.nextInt();
                    n = ob.insert_cycle(n, num);

                }
            }
        }
        ob.function(n);
    }

    node insert(node n, int val) {
        node temp = new node();
        temp.data = val;
        temp.next = null;
        node head = n;
        if (n == null) {
            n = temp;
            return n;
        } else {
            while (n.next != null)
                n = n.next;
            n.next = temp;
        }
        return head;
    }


    void display(node n) {
        if (n != null) {
            node head = n;
            if (head.next == null)
                System.out.println(head.data);
            else {
                while (head.next != null) {
                    System.out.print(head.data + "->");
                    head = head.next;
                }
                System.out.print(head.data);
            }
        }
    }

    node insert_cycle(node n, int val) {
        node head = n;
        for (int iter_i = 1; iter_i < val; iter_i++) {
            n = n.next;
        }
        node temp = n;
        while (n.next != null)
            n = n.next;
        n.next = temp;
        return head;
    }

    void function(node n) {
        int counter = 0;
        node head = n;
        node temp_head = n;
        n = n.next;
        while (counter == 0) {
            if (n == null || n.next == null) {
                counter = 1;
                System.out.println("link list has no cycles");
                break;
            } else if (n.next == n) {
                System.out.println("system has cycles");
                break;
            } else
                while (temp_head != n) {
                    if (temp_head == n.next) {
                        counter = 1;
                        System.out.println("list has cycles");
                        break;
                    }
                    temp_head = temp_head.next;
                }
            temp_head = head;
            n = n.next;
        }

    }
}



CIRCULAR NODE HEADER

import java.util.Scanner;

public class circular_node_header {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        circular_node_header ob = new circular_node_header();
        node n1 = new node();
        node n2 = new node();
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println("enter your choice 1 to insert in first node");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n1 = ob.insert(n1, val);
                    ob.display(n1);
                    break;
                }
                case 2: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n2 = ob.insert(n2, val);
                    ob.display(n2);
                    break;
                }
            }
        }
       n1= ob.merge(n1,n2);
        ob.display_merge(n1);
    }

    node insert(node n, int val) {
        node temp = new node();
        temp.data = val;
        temp.next = null;

        node head = n;
        if (n.next == null) {
            n.next = temp;
            n.next.next = n.next;
            head.data++;
            return n;
        } else {
            n = n.next;
            while (n.next != head.next)
                n = n.next;
            n.next = temp;
            n.next.next = head.next;
            head.data++;
        }
        return head;
    }

    void display(node n) {
        node head = n;
        if (n.data == 1)
            System.out.println(n.next.data);
        else {
            n = n.next;
            while (n.next != head.next) {
                System.out.println(n.data);
                n = n.next;
            }
            System.out.println(n.data);
        }
    }

    node merge(node n1, node n2) {
        node result = null;
node result_head=null;
        node head1 = n1;
        node head2 = n2;
        n1 = n1.next;
        n2 = n2.next;
        while ((head1.data != 0) && (head2.data != 0)) {
            if (result == null) {
                if (n1.data < n2.data) {
                    node temp=new node();
                    temp.data=n1.data;
                    temp.next=null;
                    result= temp;
                    n1 = n1.next;
                    head1.data--;
                } else {
                    node temp=new node();
                    temp.data=n2.data;
                    temp.next=null;
                    result=temp;

                    n2 = n2.next;
                    head2.data--;
                }
                 result_head = result;
            } else {
                if (n1.data < n2.data) {
                    node temp = new node();
                    temp.data = n1.data;
                    temp.next = null;
                    head1.data--;
                    result.next = temp;
                    result = result.next;
                    n1 = n1.next;
                } else {
                    node temp = new node();
                    temp.data = n2.data;
                    temp.next = null;
                    head2.data--;
                    result.next = temp;
                    result = result.next;
                    n2 = n2.next;
                }
            }

        }
        if (head1.data != 0) {
            while (head1.data != 0) {
                node temp = new node();
                temp.data = n1.data;
                head1.data--;
                result.next = temp;
                result = result.next;
                n1 = n1.next;
            }
        } else {
            while (head2.data != 0) {
                node temp = new node();
                temp.data = n2.data;
                head2.data--;
                result.next = temp;
                result = result.next;
                n2 = n2.next;
            }
        }
return result_head;
    }
    void display_merge(node n) {
        if (n != null) {
            node head = n;
            if (head.next == null)
                System.out.println(head.data);
            else {
                while (head.next != null) {
                    System.out.print(head.data+"->");
                    head = head.next;
                }
                System.out.print(head.data);
            }
        }
    }
}




DIJSTRA ALGORITHM

import java.util.Scanner;
public class dijkstra {
    public static void main(String args[])
    {
        Scanner sc=new Scanner(System.in);
        dijkstra ob=new dijkstra();
        System.out.println("enter the number of vertices");
        int v=sc.nextInt();
        int graph[][]={ { 0, 4, 0, 0, 0, 0, 0, 8, 0 },
                { 4, 0, 8, 0, 0, 0, 0, 11, 0 },
                { 0, 8, 0, 7, 0, 4, 0, 0, 2 },
                { 0, 0, 7, 0, 9, 14, 0, 0, 0 },
                { 0, 0, 0, 9, 0, 10, 0, 0, 0 },
                { 0, 0, 4, 14, 10, 0, 2, 0, 0 },
                { 0, 0, 0, 0, 0, 2, 0, 1, 6 },
                { 8, 11, 0, 0, 0, 0, 1, 0, 7 },
                { 0, 0, 2, 0, 0, 0, 6, 7, 0 } };
        boolean visited[]=new boolean[v];
        int src=0;
        ob.shortest_path(graph,visited,v,src);

    }
    void shortest_path(int graph[][],boolean visited[],int v,int src)
    {

        int dist[]=new int[v];
        for (int i = 0; i < v; i++)
            dist[i] = Integer.MAX_VALUE;
        dist[src]=0;
        for(int i=0;i<v-1;i++)
        {
            int selected_vertex=min(dist,visited,v);
            visited[selected_vertex]=true;
            for (int j = 0; j < v; j++)
                if (!visited[j] && graph[selected_vertex][j] != 0 && dist[selected_vertex] != Integer.MAX_VALUE && dist[selected_vertex] + graph[selected_vertex][j] < dist[j])
                    dist[j] = dist[selected_vertex] + graph[selected_vertex][j];
        }
printSolution(dist,v);
    }
    void printSolution(int dist[],int v)
    {
        System.out.println("Vertex \t\t Distance from Source");
        for (int i = 0; i < v; i++)
            System.out.println(i + " \t\t " + dist[i]);
    }

    int min(int dist[],boolean visited[],int v)
    {
       int  min=Integer.MAX_VALUE;int min_index=-1;
       for(int i=0;i<v;i++)
       {
         if(!(visited[i]) && dist[i]<=min) {
             min = dist[i];
             min_index = i;
         }
       }
       return min_index;
    }
}




DOUBLY CIRCULAR LIST

import java.util.Scanner;

public class doubly_circular_list {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        doubly_circular_list ob = new doubly_circular_list();
        node n1 = new node();
        node n2 = new node();
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println("enter your choice 1 to insert 2 to delete");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    while (val > 0) {
                        int temp_val = val % 10;
                        n1 = ob.insert(n1, temp_val);
                        val = val / 10;
                    }

                    break;
                }
                case 2: {

                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    while (val > 0) {
                        int temp_val = val % 10;
                        n2 = ob.insert(n2, temp_val);

                        val = val / 10;
                    }
                    break;
                }
            }

        }
        ob.add(n1, n2);

    }

    node insert(node n, int val) {
        node head = null;
        if (n.data == 0) {
            node temp = new node();
            temp.data = val;
            temp.next = null;
            n.data = 1;
            n.next = temp;
            head = n;
            n = n.next;
            n.next = head.next;
            n.previous = n.next;
            return head;
        } else {
            head = n;
            n = n.next;
            while (n.next != head.next)
                n = n.next;
            node temp = new node();
            temp.data = val;
            temp.next = null;
            n.next = temp;
            (n.next).previous = n;
            n = n.next;
            n.next = head.next;
            (head.next).previous = n;
            head.data++;
            return head;
        }

    }



        void display(node n) {
            if (n != null) {
                node head = n;
                if (head.next == null)
                    System.out.println(head.data);
                else {
                    while (head.next != null) {
                        System.out.print(head.data+"->");
                        head = head.next;
                    }
                    System.out.print(head.data);
                }
            }
        }


    void add(node n1, node n2) {
        node head1 = n1;
        node head2 = n2;
        int carry = 0;
        node result = null;
        node result_head = null;
        n1 = n1.next;
        n2 = n2.next;
        while ((head1.data != 0) && (head2.data != 0)) {
            if (result_head == null) {
                node temp = new node();
                temp.data =(n1.data + n2.data + carry)%10;
                carry = (n1.data + n2.data + carry)/10;
                temp.next = null;
                head1.data--;
                head2.data--;
                n1 = n1.next;
                n2 = n2.next;

                result_head=temp;


            } else {
                node temp = new node();
                temp.data = (n1.data + n2.data + carry)%10;
                carry = (n1.data + n2.data + carry)/10;
                temp.next = result_head;
                result_head = temp;
                n1 = n1.next;
                n2 = n2.next;
                head1.data--;
                head2.data--;;
            }
        }
        if (head1.data != 0) {
            while (head1.data != 0) {

                node temp = new node();
                temp.data = (n1.data+carry)%10;
                carry=(n1.data+carry)/10;
                head1.data--;
                temp.next=result_head;
                result_head= temp;
                n1 = n1.next;
            }
        } else {
            while (head2.data != 0) {
                node temp = new node();
                temp.data = (n2.data+carry)%10;
                carry=(n2.data+carry)/10;
                head2.data--;
                temp.next=result_head;
                result_head= temp;
                n2 = n2.next;
            }
        }
    display(result_head);
    }
}



EQUAL TREES

import java.util.Scanner;

public class equal_trees {
    static int i=0;
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        equal_trees ob = new equal_trees();
        node n1 = null;
        node n2 = null;
        System.out.println("enter the data in the first tree ");
        n1 = ob.insert(n1);
        System.out.println();
        System.out.println("enter the data in the second tree ");
        n2 = ob.insert(n2);
        System.out.println();
        System.out.println("elements of the first tree  is");
        ob.inorder(n1);
        System.out.println();
        System.out.println("elements of the second tree  is");
        ob.inorder(n2);
        boolean t=ob.function(n1,n2);
        System.out.println(t);
    }

    node insert(node n) {
        Scanner sc = new Scanner(System.in);
        if (n == null) {
            System.out.println("enter the value to be inserted ");
            node temp = new node();
            temp.data = sc.nextInt();
            if (temp.data == -1)
                return null;
            n = temp;

        }
        n.left = insert(n.left);
        n.right = insert(n.right);
        return n;
    }

    void inorder(node n) {
        if (n == null)
            return;
        inorder(n.left);
        System.out.print(n.data + "");
        inorder(n.right);


    }
    boolean function(node n1,node n2)
    {
        if(n1==null && n2==null)
            return true;
        else if(n1!=null && n2==null)
            return false;
        else if(n1==null && n2!=null)
            return false;
        else if(n1.data==n2.data)
            return true;
        boolean left=function(n1.left,n2.left);
        if(!left)
            return false;
            boolean right=function(n1.right,n2.right);
                    return right;



    }

}

GREATEDT COMMON DIVISOR

import java.util.*;

public class gcd {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        gcd ob = new gcd();
        System.out.println("enter the 2 numbers first big then small");
        int max = sc.nextInt();
        int min = sc.nextInt();
        // int gcd = ob.function1(max,min);
        //System.out.println(gcd);
        Queue<Integer> stk1 = ob.primefactors(max);
        Queue<Integer> stk2 = ob.primefactors(min);
        int product = 1;
        int a = stk1.poll();
        int b = stk2.poll();
        int t = 1;
        while (t == 1) {
            if (a == b) {
                product = product * a;
                if (stk1.isEmpty() || stk2.isEmpty())
                    break;
                else
                    a = stk1.poll();
                b = stk2.poll();

                break;
            } else if (a > b) {
                while (stk2.peek() < a)
                    b = stk2.poll();
                if (!stk2.isEmpty())
                    b = stk2.poll();
            } else if (a < b) {
                while (stk1.peek() < b)
                    a = stk1.poll();
                if (!stk1.isEmpty())
                    a = stk1.poll();
            }

        }
        System.out.println("the product is  " + product);
    }

  /*  int function(int max, int min) {
        if (min == 0)
            return max;
        else
            return function(min, max % min);

    }

    int function1(int max, int min) {
        for (int i = min; i > 0; i--) {
            if (max % i == 0 && min % i == 0)
                return i;

        }
        return 1;
    }*/

    Queue<Integer> primefactors(int n) {
        Queue<Integer> stk = new LinkedList<>();
        LinkedList<Integer> list = new LinkedList<Integer>();

        for (int i = 2; i <= n / 2; i++) {
            list.addLast(i);
        }
        for (int i = 2; i <= n / 2; i++) {
            for (int j = 2 * i; j <= n; j = j + i) {
                if (list.contains(j))
                    list.removeFirstOccurrence(j);
            }

        }
        System.out.println(list);
        Iterator itr = list.listIterator();
        while (itr.hasNext()) {
            while (n % list.peek() == 0) {
                stk.add(list.peek());
                n /= list.peek();
            }
            list.poll();
        }

        return stk;
    }
}

GRAPHS

import java.util.*;

public class graphs {
    public static void main(String args[]) {
        ArrayList<ArrayList<Integer>> adj = new ArrayList<ArrayList<Integer>>(6);
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < 6; i++)
            adj.add(new ArrayList<Integer>());
        graphs ob = new graphs();
        int choice = 1;
        while (choice > 0) {
            System.out.println("enter the edge from a to b");
            int u = sc.nextInt();
            int v = sc.nextInt();
            ob.insert(adj, u, v);

            choice = sc.nextInt();
        }

        for (int i = 0; i < adj.size(); i++) {
            System.out.println("\nAdjacency list of vertex" + i);
            for (int j = 0; j < adj.get(i).size(); j++) {
                System.out.print(" -> " + adj.get(i).get(j));
            }
            System.out.println();
        }
        int v = 2;
        boolean visited[] = new boolean[6];

        ob.topological_sort(adj, 0, visited);


    }

    void topological_sort(ArrayList<ArrayList<Integer>> adj, int v, boolean visited[]) {
        Stack<Integer> stk = new Stack<Integer>();

        for (int j = 0; j < visited.length; j++)
            if (visited[j] == false)
                topological(adj, j, visited, stk);
        while (!stk.empty())
            System.out.print(stk.pop() + " ");

    }

    void topological(ArrayList<ArrayList<Integer>> adj, int v, boolean visited[], Stack<Integer> stk) {
        Iterator<Integer> i = adj.get(v).listIterator();
        visited[v] = true;

        while (i.hasNext()) {
            int n = i.next();
            if (!visited[n])
                topological(adj, n, visited, stk);

        }

        stk.push(v);

    }

    void bfs(ArrayList<ArrayList<Integer>> adj, int v, boolean visited[]) {
        Queue<Integer> q = new LinkedList<Integer>();
        q.add(v);

        while (!q.isEmpty()) {

            int del = q.poll();
            if (!visited[del])
                System.out.println(del);
            visited[v] = true;
            Iterator<Integer> i = adj.get(del).listIterator();
            while (i.hasNext()) {
                int n = i.next();
                if (!visited[n]) {
                    System.out.println(n);
                    visited[n] = true;
                    q.add(n);
                }
            }

        }
    }

    void dfs(ArrayList<ArrayList<Integer>> adj, int v, boolean visited[]) {
        visited[v] = true;
        System.out.println(v);


        Iterator<Integer> i = adj.get(v).listIterator();
        while (i.hasNext()) {
            int n = i.next();
            if (!visited[n])
                dfs(adj, n, visited);
        }


    }

    void insert(ArrayList<ArrayList<Integer>> adj, int u, int v) {
        adj.get(u).add(v);
        adj.get(v).add(u);
    }
}

HAMILTON

import java.util.*;

public class hamilton {
    public static void main(String args[]) {
        ArrayList<ArrayList<Integer>> adj = new ArrayList<ArrayList<Integer>>(6);
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < 6; i++)
            adj.add(new ArrayList<Integer>());
        hamilton ob = new hamilton();
        ob.insert(adj, 0, 1);
        ob.insert(adj, 0, 3);
        ob.insert(adj, 3, 2);
        ob.insert(adj, 2, 1);
        ob.insert(adj, 0, 2);
        ob.insert(adj, 3, 4);
        ob.insert(adj, 2, 4);
        ob.insert(adj, 4, 5);
        ob.insert(adj, 1, 5);

        for (int i = 0; i < adj.size(); i++) {
            System.out.println("\nAdjacency list of vertex" + i);
            for (int j = 0; j < adj.get(i).size(); j++) {
                System.out.print(" -> " + adj.get(i).get(j));
            }
            System.out.println();
        }
        int v = 0;
        boolean visited[] = new boolean[6];
        Stack<Integer> stk = new Stack<Integer>();
        ob.insert(adj, v, visited, stk);
    }

    void insert(ArrayList<ArrayList<Integer>> adj, int v, boolean visited[], Stack<Integer> stk) {
        visited[v] = true;
        stk.push(v);
        Iterator<Integer> i = adj.get(v).listIterator();
        while (i.hasNext()) {
            int n = i.next();
            if (visited[n] == false)
                insert(adj, n, visited, stk);
        }
        if (stk.size() == 6 && adj.get(stk.peek()).contains(0))
            System.out.println(stk);
        stk.pop();
        visited[v] = false;
        return;
    }

    void insert(ArrayList<ArrayList<Integer>> adj, int u, int v) {
        adj.get(u).add(v);
        adj.get(v).add(u);
    }
}

HASHMAP

import java.util.Iterator;
import java.util.Scanner;

public class hash_map {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        System.out.println("enter the number of objects");
        int num = sc.nextInt();
        node n[] = new node[num];
        for (int i = 0; i < num; i++)
            n[i] = null;
        hash_map ob = new hash_map();
        int choice = 1;

        while (choice > 0) {
            System.out.println("enter the value with the key to be inserted");
            int val = sc.nextInt(), key = sc.nextInt();
            ob.insert(n, val, key);
            ob.display(n);
            System.out.println("enter 1 if u want to insert further");
            choice = sc.nextInt();
        }
        System.out.println("enter the element to be found");
        int ele = sc.nextInt();
        boolean presesnt = ob.find(n, ele);
        System.out.println(presesnt);


    }

    boolean find(node n[], int ele) {
        int k = 0;
        while (k != n.length && n[k] != null)
            if (n[k++].data == ele)
                return true;
        return false;
    }

    void display(node n[]) {
        int k = 0;
        while (k != n.length ) {
            if(n[k]!=null)
            System.out.println("value at node "+k+" is "+n[k].data + "->" + n[k].next.data);
            k++;
        }
    }

    void insert(node n[], int val, int key) {
       int address=key%n.length;
       if (n[address]==null)
       {
           node temp=new node();
           temp.data=key;
           temp.next=new node();
           n[address]=temp;
           temp=new node();
           temp.data=val;
           n[address].next=temp;
       }
       else {
           int k=0;
           while (n[k] != null)
           {
             ++k;
           }
           node temp=new node();
           temp.data=key;
           n[k]=temp;
           temp=new node();
           temp.data=val;
           n[k].next=temp;

       }

    }
}

HEAPSORT


import java.util.Scanner;

public class heap_sort {
    public static void main(String args[]) {
        int front = 1;
        int size = 0;
        Scanner sc = new Scanner(System.in);
        heap_sort ob = new heap_sort();
        System.out.println("enter the number of elements");
        int n = sc.nextInt();
        int heap[] = new int[n];
        int choice = 1;
        while (choice > 0) {
            System.out.println("enter the element to insert");
            int ele = sc.nextInt();
            size = ob.insert(heap, size, ele);
            choice = sc.nextInt();
        }
        for (int i = 1; i <= size / 2; i++) {
            System.out.print(" PARENT : " + heap[i]
                    + " LEFT CHILD : " + heap[2 * i]
                    + " RIGHT CHILD :" + heap[2 * i + 1]);
            System.out.println();
        }
        System.out.println();
       ob.delete(heap,size,front);
        for (int i = 1; i <= size / 2; i++) {
            System.out.print(" PARENT : " + heap[i]
                    + " LEFT CHILD : " + heap[2 * i]
                    + " RIGHT CHILD :" + heap[2 * i + 1]);
            System.out.println();
        }

    }

    int insert(int heap[], int size, int ele) {
        if (size > heap.length)
            return size;
        heap[++size] = ele;
        int parent = size / 2;
        int current = size;
        while (heap[current] < heap[parent]) {
            swap(heap, current, parent);
            current = current / 2;
            parent = parent / 2;
        }
        return size;
    }

    void delete(int heap[], int size, int front) {
        heap[size]=0;
        heap[front] = heap[size--];
        heapify(heap, size, front);
    }

    void swap(int heap[], int a, int b) {
        int temp =heap[ a];
        heap[a] = heap[b];
        heap[b] = temp;
    }

    void heapify(int heap[], int size, int current) {
        if (current < Math.floor(size / 2)) {
            if (heap[current * 2] < heap[current] || heap[current * 2 + 1] < heap[current]) {
                if (heap[current * 2] < heap[current * 2 + 1]) {
                    swap(heap, current * 2, current);
                    heapify(heap, size, current * 2);
                } else
                    swap(heap, current * 2 + 1, current);
                heapify(heap, size, current * 2 + 1);
            }
        }
    }

}

MAX HEAP


import java.util.Scanner;

public class max_heap {
    public static void main(String args[]) {
        max_heap ob = new max_heap();
        Scanner sc = new Scanner(System.in);
        System.out.println("enter the size");
        int n = sc.nextInt();
        int heap[] = new int[10];
        heap[0] = Integer.MAX_VALUE;
        int choice = 1;
        int size = 0;
        for(int i=1;i<=9;i++)
            heap[i]=sc.nextInt();
        ob.array_heapify(heap,heap.length-1);
      /*  while (choice > 0) {
            System.out.println("enter the number to insert");
            int ele = sc.nextInt();
            choice = sc.nextInt();
            size = ob.insert(ele, heap, size);
        }*/
        ob.print(heap, heap.length-1);
        System.out.println();
        size=ob.delete(heap,heap.length-1);
        ob.print(heap, heap.length-2);
    }

    void print(int heap[], int size) {
        for (int i = 1; i <= size / 2; i++) {
            System.out.print("parent :" + heap[i] + " left subchild :" + heap[2 * i] + " right subchild :" + heap[2 * i + 1]);
            System.out.println();
        }
    }

    int delete(int heap[], int size) {
        if (size > 0) {
            heap[1] = heap[size--];
            heapify(heap,1,size);
        }
return size;
    }
    void array_heapify(int heap[],int size)
    {
        for (int pos = (size / 2); pos >= 1; pos--) {
            heapify(heap,pos,size);
        }
    }

    void heapify(int heap[], int current, int size) {
        if (current <= size / 2) {
            if (heap[current] < heap[current * 2 + 1] || heap[current] < heap[current * 2]) {
                if (heap[current * 2] > heap[current * 2 + 1]) {
                    swap(heap, current * 2, current);
                    heapify(heap, current * 2, size);
                } else
                    swap(heap, current * 2 + 1, current);
                {
                    heapify(heap, current * 2 + 1, size);
                }
            }
        }
    }



    int insert(int ele, int heap[], int size) {
        if (size >= heap.length)
            return size;
        else {
            heap[++size] = ele;
            int current = size;
            int parent = size / 2;
            while (heap[current] > heap[parent]) {
                swap(heap, current, parent);
                current = current / 2;
                parent = parent / 2;
            }
        }
        return size;
    }

    void swap(int heap[], int a, int b) {
        int temp = heap[a];
        heap[a] = heap[b];
        heap[b] = temp;
    }
}



MERGE SORT

import java.util.Scanner;

public class merge_sort {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        merge_sort ob = new merge_sort();
        System.out.println("enter the number of elements");
        int n = sc.nextInt();
        System.out.println("enter the number of elements");
        int arr[] = new int[n];
        for (int i = 0; i < n; i++)
            arr[i] = sc.nextInt();
        int low = 0, high = n - 1;
        ob.sort(arr, low, high);
        for (int i = 0; i < arr.length; i++)
            System.out.print(arr[i] + " ");
    }

    void sort(int arr[], int low, int high) {

        if (low < high) {
            int mid = (low + high) / 2;
            sort(arr, low, mid);
            sort(arr, mid + 1, high);
            merge(arr, low, mid, high);
        }


    }

    void merge(int arr[], int low, int mid, int high) {
        int size_a = mid - low + 1;
        int a[] = new int[size_a];
        int size_b = high - mid;
        int k = low;
        int b[] = new int[size_b];
        for (int i = 0; i < size_a; i++)
            a[i] = arr[k++];
        for (int i = 0; i < size_b; i++)
            b[i] = arr[k++];
        int i = 0, j = 0;
        while (i < size_a && j < size_b) {
            if (a[i] < b[j])
                arr[low++] = a[i++];
            else
                arr[low++] = b[j++];
        }
        while (i < size_a)
            arr[low++] = a[i++];
        while (j < size_b)
            arr[low++] = b[j++];

    }
}

MODE

import java.util.Scanner;

public class mode {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        mode ob = new mode();
        System.out.println("enter the total no of elements");
        int n = sc.nextInt();
        System.out.println("enter the list of numbers  in sorted order");
        int arr[] = new int[n];
        for (int iter_i = 0; iter_i < n; iter_i++)
            arr[iter_i] = sc.nextInt();
        ob.function(arr);
    }

    void function(int arr[]) {
        int max_num = arr[0], max_freq = 1;
        for (int i = 0; i < arr.length; ) {
            int frequency = 1;
            int number = arr[i];
            for (int j = i + 1; j <= arr.length; j++) {

                if (  j<arr.length && arr[j] == number)
                    frequency = frequency + 1;
                else {
                    if (max_freq < frequency) {
                        max_freq = frequency;
                        max_num = arr[i];
                    }
                    i = j;
                    break;

                }
            }
        }
        System.out.println("max frequency" + max_freq);
        System.out.println("max num" + max_num);

    }
}
 
BASIC NODE 

public class node
{
    int data;
    char c;
    node next;
    node previous;
    node left;
    node right;


}

PERMUTATION

import java.util.Scanner;

public class permutation {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        permutation ob = new permutation();
        System.out.println("enter the number of elements ");
        int n = sc.nextInt();
        int arr[] = new int[n];
        System.out.println("enter the elements");
        for (int i = 0; i < n; i++)
            arr[i] = sc.nextInt();
        int pos = 0;
        ob.generate(pos, n, arr);
    }

    void generate(int pos, int n, int arr[]) {
        if (pos == n) {
            for (int i = 0; i < n; i++)
                System.out.print(arr[i]);
        }
        else { int k=pos;
        while (k<n)
        {
         exchange(arr,k,pos);

        }
        }

    }

        void exchange ( int arr[], int a, int b){
            int temp = arr[a];
            arr[a] = arr[b];
            arr[b] = temp;
    }
}

QUICK SORT

import javax.swing.plaf.synth.SynthTextAreaUI;
import java.util.Scanner;

public class pivot_sort {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        pivot_sort ob = new pivot_sort();
        System.out.println("enter the number of elements");
        int n = sc.nextInt();
        int arr[] = new int[n];
        System.out.println("enter the elements");
        for (int i = 0; i < n; i++)
            arr[i] = sc.nextInt();
        int pivot = 0;
        ob.sort(arr, 0, arr.length - 1);
        for (int i = 0; i < arr.length; i++)
            System.out.print(arr[i] + " ");
    }

    void sort(int arr[], int low, int high) {
        if (low < high) {
            int pivot = function(arr, low, high);
            sort(arr, low, pivot - 1);
            sort(arr, pivot + 1, high);

        }
    }

    int function(int arr[], int low, int high) {
        int pivot = arr[low];
        int i = low+1, j = high;


     do {
            while (i < high && arr[i ] < pivot)
                i++;
            while (arr[j] > pivot && j > low)
                j--;
            swap(arr, i, j);

        }  while (i < j);
            if (i > j)
            swap(arr, i, j);

        swap(arr, low, j);
        return j;
    }

    void swap(int arr[], int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}




POLYNOMIAL LINKED LIST

import java.util.Scanner;

public class polynomials {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        polynomials ob = new polynomials();
        node n1 = new node();
        node n2 = new node();
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println("enter your choice 1 to insert in first node");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the degree of polynomial");
                    int degree = sc.nextInt();
                    System.out.println("enter the value to be inserted");
                    for (int i = 0; i < degree; i++) {
                        int val = sc.nextInt();
                        n1 = ob.insert(n1, val);

                    }
                    ob.display(n1);
                    break;
                }
                case 2: {
                    System.out.println("enter the degree of polynomial");
                    int degree = sc.nextInt();
                    System.out.println("enter the value to be inserted");
                    for (int i = 0; i < degree; i++) {
                        int val = sc.nextInt();
                        n2 = ob.insert(n2, val);

                    }
                    ob.display(n2);
                    break;
                }
            }
        }
        n1 = ob.multiply(n1, n2);
        ob.display_merge(n1);
    }

    node insert(node n, int val) {
        node temp = new node();
        temp.data = val;
        temp.next = null;

        node head = n;
        if (n.next == null) {
            n.next = temp;
            n.next.next = n.next;
            head.data++;
            return n;
        } else {
            n = n.next;
            while (n.next != head.next)
                n = n.next;
            n.next = temp;
            n.next.next = head.next;
            head.data++;
        }
        return head;
    }

    void display(node n) {
        node head = n;
        if (n.data == 1)
            System.out.println(n.next.data);
        else {
            n = n.next;
            System.out.print(n.data + " ");
            n = n.next;
            int i = 1;
            while (n.next != head.next) {
                System.out.print("+" + n.data + "x" + "^" + (i++) + " ");
                n = n.next;
            }
            System.out.println("+" + n.data + "x" + i);
        }
    }

    node multiply(node n1, node n2) {
        node head1 = n1;
        node head2 = n2;
        int count = head2.data;
        n1 = n1.next;
        n2 = n2.next;
        node result = new node();
        node result_head = result;
        node temp_res = result;
        for (int i = 0; i < 5; i++) {
            node temp = new node();
            temp.data = 0;
            temp.next = null;
            result.next = temp;
            result=result.next;
        }


        while (head1.data != 0) {
            head1.data--;
            result = temp_res;
            while (head2.data != 0) {
                node temp = new node();
                    temp.data = result.data + n1.data * n2.data;
                    result.data = temp.data;
                    result = result.next;

                n2 = n2.next;
                head2.data--;
            }
            head2.data = count;
            temp_res = temp_res.next;
            n1 = n1.next;

        }
        return result_head;
    }



    void display_merge(node n) {
        if (n != null) {
            node head = n;
            if (head.next == null)
                System.out.println(head.data);
            else {
                while (head.next != null) {
                    System.out.print(head.data + "->");
                    head = head.next;
                }
                System.out.print(head.data);
            }
        }
    }
}




N QUEENS ALGORITHM

import java.util.Scanner;

public class queens {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        queens ob = new queens();
        System.out.println("enter the n queens in nxn chessboard");
        int n = sc.nextInt();
        int arr[][] = new int[n][n];
        int pos = 0;
        ob.insert(arr, n, pos);


    }

    void insert(int arr[][], int n, int pos) {
        if (pos == n) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++)
                    System.out.print(arr[i][j] + " ");
                System.out.println();
            }
            System.out.println();
            return;
        }

        for (int i = 0; i < n; i++) {
            if (isvalid(arr, n, pos, i)) {
                arr[pos][i] = 1;
                insert(arr, n, pos + 1);
            } else
                continue;

            for (int k = 0; k < n; k++)
                arr[pos][k] = 0;

        }
    }

    boolean isvalid(int arr[][], int n, int pos, int i) {
        for (int j = 0; j < pos; j++)
            if (arr[j][i] == 1)
                return false;

        for (int j = 0; j < pos; j++) {
            for (int k = 0; k < n; k++) {
                if (arr[j][k] == 1) {
                    int a = j;
                    int b = k;
                    while (a <= pos && b < n) {
                        a = a + 1;
                        b = b + 1;
                        if (a == pos && b == i)
                            return false;
                    }
                    a = j;
                    b = k;
                    while (a <= pos && b > 0) {
                        a = a + 1;
                        b = b - 1;
                        if (a == pos && b == i)
                            return false;
                    }
                }
            }
        }

        return true;
    }
}




QUEUE LIST HEADER

import java.util.Scanner;

public class queue_list_header{
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        queue_list_header ob = new queue_list_header();
        node n = new node();
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println("enter your choice 1 to insert 2 to delete");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n = ob.insert(n, val);
                    ob.display(n);
                    break;
                }
                case 2: {
                    n = ob.delete(n);
                    ob.display(n);
                    break;
                }
            }
        }
    }

    node insert(node n, int val) {
        node temp = new node();
        temp.data = val;
        temp.next = null;

        node head = n;
        if (n.next== null) {
           n.next=temp;
           head.data++;
            return n;
        } else {

            while (n.next != null)
                n = n.next;
            n.next = temp;
            head.data++;
        }
        return head;
    }

    node delete(node n) {
        if(n.next==null)
            return n;
        if(n.next.next==null) {
            n.data = 0;
            n.next = null;
            return n;
        }
       else if (n.data!=0)
       {
           n.next=n.next.next;
           n.data--;
           return n;

       }

       return  n;
    }

    void display(node n) {
        if (n != null) {
            node head = n;
            if (head.next == null)
                System.out.println(head.data);
            else {
                while (head.next != null) {
                    System.out.print(head.data+"->");
                    head = head.next;
                }
                System.out.print(head.data);
            }
        }
    }
}



SETS

import java.util.Scanner;
public class sets {
    public static void main(String args[])
    {
        Scanner sc=new Scanner(System.in);
        sets ob=new sets();
        System.out.println("enter the number of bits");
        int n=sc.nextInt();
        int arr[]=new int[n];
        int pos=0;
        ob.function(arr,pos,n);

    }
    void function(int arr[],int pos,int n)
    {
        if (pos==n) {
            for (int i = 0; i < n; i++)
                System.out.print(arr[i] + " ");
            System.out.println();
        }
      else
        {
            arr[pos] = 0;
            function(arr, pos+1, n);
            arr[pos] = 1;
            function(arr, pos+1, n);
        }
    }
}


STACK LIST

import java.util.Scanner;

public class stack_list {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        stack_list ob = new stack_list();
        node n = null;
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println("enter your choice 1 to insert 2 to delete");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n = ob.insert(n, val);
                    ob.display(n);
                    break;
                }
                case 2: {
                    n = ob.delete(n);
                    ob.display(n);
                    break;
                }
            }
        }
        n = ob.reverse(n);
        ob.display(n);
    }

    node insert(node n, int val) {
        node temp = new node();
        temp.data = val;
        temp.next = null;
        node head = n;
        if (n == null) {
            n = temp;
            return n;
        } else {
            while (n.next != null)
                n = n.next;
            n.next = temp;
        }
        return head;
    }

    node delete(node n) {
        node head = n;
        if (n != null) {
            if (n.next == null) {
                n = null;
                return n;
            } else {

                node min=n;
                while (n != null) {
                    if (min.data>n.data)
                        min=n;
                    n = n.next;
                }
                n=head;
                while ( n!=null  && n.next!=min)
                    n=n.next;
                if (n==null)
                return head.next;
                else
                    n.next=min.next;

            }
        }
        return head;
    }

    void display(node n) {
        if (n != null) {
            node head = n;
            if (head.next == null)
                System.out.println(head.data);
            else {
                while (head.next != null) {
                    System.out.print(head.data + "->");
                    head = head.next;
                }
                System.out.print(head.data);
            }
        }
    }

    node reverse(node n) {
        if (n == null || n.next == null)
            return n;

        node temp=reverse(n.next);
        n.next.next=n;
        n.next=null;
        return temp;
    }

}



SUBSET SUM ALGORITHM

import java.util.Scanner;

public class subset_sum {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        subset_sum ob = new subset_sum();
        System.out.println("enter the number of elements");

        int arr[] = {3, 5, 6, 7};
        int d = 15;
        int sum = 0;
        int pos = 0;
        boolean pick[] = new boolean[4];
        ob.search(pick, arr, sum, pos, d);
    }

    void print(int arr[], boolean pick[]) {
        for (int j = 0; j < arr.length; j++) {
            if (pick[j] == true)
                System.out.print(arr[j]);

        }
        System.out.println();
    }

    void search(boolean pick[], int arr[], int sum, int pos, int d) {
        if (pos >= arr.length)
            return;
        if ((arr[pos] + sum) < d) {
            pick[pos] = true;
            search(pick, arr, sum + arr[pos], pos + 1, d);

        } else if ((arr[pos] + sum) == d) {
            pick[pos] = true;
            print(arr, pick);
        }
        pick[pos] = false;
        search(pick, arr, sum, pos + 1, d);


    }
}

TOWER OF HANOI

import java.util.Scanner;
public class tower_of_hanoi {
    public static void main(String args[])
    {
        Scanner sc=new Scanner(System.in);
        System.out.println("enter the number of blocks");
        int blocks=sc.nextInt();
        char start='a',temp='b',finish='c';
        tower_of_hanoi ob=new tower_of_hanoi();
        ob.function(blocks,start,temp,finish);

    }
    void function(int blocks,char start,char temp,char finish)
    {
        if(blocks ==1)
            System.out.println("move rod"+"1 "+start+" to "+finish);
        else
        {
            function(blocks-1,start,finish,temp);
            System.out.println("move rod"+blocks+" "+start+"to"+finish);
            function(blocks-1,temp,start,finish);
        }

    }

}

TRAVERSAL TREE ITERATION

import java.util.Scanner;

public class traversal {
    public static void main(String args[]) {
        traversal ob = new traversal();
        node1 n = new node1(1);

        n.left = new node1(2);

        n.right = new node1(3);

        n.left.left = new node1(4);
        n.left.right = new node1(5);
        System.out.println("postorder traversal of the array");
        ob.postorder(n);
        System.out.println();
        System.out.println("inorder traversal of the array");
        ob.inorder(n);
        System.out.println();

        System.out.println("preorder traversal of the array");
        ob.preorder(n);
    }

    void postorder(node1 n) {
        if (n == null)
            return;
        postorder(n.left);
        postorder(n.right);
        System.out.print(n.data + "");
    }

    void inorder(node1 n) {
        if (n == null)
            return;
        inorder(n.left);
        System.out.print(n.data + "");
        inorder(n.right);


    }

    void preorder(node1 n) {
        if (n == null)
            return;
        System.out.print(n.data + "");
        preorder(n.left);
        preorder(n.right);

    }
}


TREE OPERATIONS

import java.util.Scanner;

public class tree_ops {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        tree_ops ob = new tree_ops();
        node n = null;
        n = ob.insert(n);
        ob.inorder(n);
        System.out.println("enter the item to be searched");
        int val = sc.nextInt();

        boolean t=ob.search(n, val);
System.out.println(t);

    }

    node insert(node n) {
        Scanner sc = new Scanner(System.in);
        if (n == null) {
            System.out.println("enter the value to be inserted ");
            node temp = new node();
            temp.data = sc.nextInt();
            if (temp.data == -1)
                return null;
            n = temp;

        }
        n.left = insert(n.left);
        n.right = insert(n.right);
        return n;
    }

    void inorder(node n) {
        if (n == null)
            return;
        inorder(n.left);
        System.out.print(n.data + "");
        inorder(n.right);


    }

    boolean search(node n, int val) {

        if (n == null)
            return false;

        if (n.data == val)
            return true;

        // then recur on left sutree /
        boolean res1 = search(n.left, val);
        if (res1) return true; // node found, no need to look further

        // node is not found in left, so recur on right subtree /
        boolean res2 = search(n.right, val);

        return res2;
    }

}

UNION LINKED LIST

import java.util.Scanner;
import java.util.HashMap;
import java.util.Map;

public class union {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        union ob = new union();
        node n1 = null;
        node n2 = null;
        int choice = 1;
        while (choice > 0) {
            System.out.println();
            System.out.println("enter your choice 1 to insert 2 to delete");
            choice = sc.nextInt();
            switch (choice) {
                case 1: {
                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n1 = ob.insert(n1, val);
                    ob.display(n1);
                    break;
                }
                case 2: {

                    System.out.println("enter the value to be inserted");
                    int val = sc.nextInt();
                    n2 = ob.insert(n2, val);
                    ob.display(n2);
                    break;
                }
            }
        }
        ob.function(n1, n2);
    }

    node insert(node n, int val) {
        node temp = new node();
        temp.data = val;
        temp.next = null;
        node head = n;
        if (n == null) {
            n = temp;
            return n;
        } else {
            while (n.next != null)
                n = n.next;
            n.next = temp;
        }
        return head;
    }

    void display(node n) {
        if (n != null) {
            node head = n;
            if (head.next == null)
                System.out.println(head.data);
            else {
                while (head.next != null) {
                    System.out.print(head.data + "->");
                    head = head.next;
                }
                System.out.print(head.data);
            }
        }
    }

    void function(node n1, node n2) {
        node result = n1;
        node head = n1;
        HashMap<Integer, Integer> map = new HashMap<>();
        while (n1.next != null) {
            map.put(n1.data, -1);
            n1 = n1.next;
        }
        map.put(n1.data, -1);
        while (n2.next != null) {
            if (!(map.containsKey(n2.data))) {
                while (result.next != null)
                    result = result.next;
                node temp = new node();
                temp.data = n2.data;
                temp.next = null;
                result.next = temp;
                result = result.next;
            }
            n2 = n2.next;
        }
        display(head);
    }
}












