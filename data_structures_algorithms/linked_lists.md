# Linked Lists
Linked list is a sequence of elements (nodes) where one links to another one (one-way for singly linked list & two-way for doubly linked list). <br>
Just like an array, it can store any types of data, and they can be sorted/unsorted or duplicated. And its nodes will contain data and location information
of the next node (and previous node if doubly linked list).

The key difference of the linked list from the array are:
- Linked list has no index - hence, inserting/removing/getting an element requires iteration approach from the head to the location - O(n). 
But when the location is at its head, it's fast - O(1).
- Unlike the array, the linked list doesn't require pre-allocated memory and data can be stored at different locations. This makes data insertion easier as
no new memory location is needed when the capacity is reached. 

## Python Implementation
Two classes are defined for a node and a linked list. 

    class Node:  
        """
        Node's data and pointer to the next node are defined.
        """
        def __init__(self, data=None, next=None):
            self.data = data
            self.next = next
            
    class LinkedList:
        """
        Linked list's functions are defined.
        """
        def __init__(self):
            """
            Initially, head node is None. (no head node)
            """
            self.head = None
            
        def insert_at_beginning(self, data):
            """
            New head node is defined and it points to the previous head.
            """
            node = Node(data, self.head) 
            self.head = node # define the new head node
            
        def print(self):
            if self.head is None:
                print("Linked list is empty")
                return
            
            itr = self.head
            llstr = '' # placeholder for iterating node's data
            while itr:
                llstr += str(itr.data) + '-->'
                itr = itr.next # move to the next node (itr = iterator)
                
            print(llstr)
            
        def insert_at_end(self, data):
            """
            New node is defined at the end of the linked list.
            """
            # if there is no head, new node is defined as head
            if self.head is None:  
                self.head = Node(data, None)
                return
            
            # when there is head, iteration is done until the end node is reached
            itr = self.head
            while itr.next: 
                itr = itr.next
            # when the end node is reached, its next node is defined as the new end node
            itr.next = node(data, None)
            
        def insert_values(self, data_list):
            """
            A new linked list is created using a list of data.
            """
            # remove head node
            self.head = None 
            # append data to the end
            for data in data_list:
                self.insert_at_end(data)
                
        def get_length(self):
            """
            Returns length of the linked list.
            """
            count = 0
            itr = self.head
            while itr:
                itr = itr.next
            return(count)
            
        def remove_at(self, index):
            """
            Removes node at the given index.
            """
            if (index < 0) or (index >= self.get_length()):
                raise Exception("Invalid index")
                
            if index == 0:
                self.head = self.head.next
                return
                
            count = 0
            itr = self.head
            while itr:
                # right before reaching the index, the current node's next next node 
                # will become the current node's next node
                if index == count + 1:
                    itr.next = itr.next.next
                    break
                    
                count += 1
                itr = itr.next
                
        def insert_at(self, data, index):
            """
            New data is stored on node at the given index.
            """
            if (index < 0) or (index >= self.get_length()):
                raise Exception("Invalid index")
                
            if index == 0:
                self.insert_at_beginning(data)
                return
                
            count = 0 
            itr = self.head
            while itr:
                if count == index - 1:
                    node = Node(data, self.next)
                    itr.next = node
                    break
                
                count += 1
                itr = itr.next
            
    if __name__ == '__main__':
        ll = LinkedList()
        ll.insert_at_beginning(5)
        ll.insert_at_beginning(89)
        ll.insert_at_end(79)
        ll.print()
        
    $ 89-->5-->79
      
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
