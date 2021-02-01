"""Implementing the Binary Search Tree data structure"""


class Node:
    """
    Class to create the node that stores the given data.

    Attributes:
                data - the data to be stored
                left_child - reference to the left child to the node
                right_child - reference to the right_child to the node
                parent - reference to the parent node of the given node(default = None)
    """
    def __init__(self, data, parent=None):
        self.data = data
        self.left_child = None
        self.right_child = None
        self.parent = parent


class BinarySearchTree:
    """
    Class to create the BST

    Attributes:
                __root - a pointer referring to the start node of the tree.

    Methods:
            insert() - inserts a new data node
            traverse() - prints the contents of the tree in "IN-ORDER" manner.
            remove() - removes a given data node
            max() - finds and returns the largest data in the tree
            min() - finds and returns the smallest data in the tree
    """
    def __init__(self):
        self.__root = None

    def insert(self, data):
        if not self.__root:
            self.__root = Node(data)
        elif self.__root:
            self.__insert_data(data, self.__root)

    def __insert_data(self, data, node):
        # if data is smaller than the root node's data
        # check the left tree
        if data < node.data:
            if node.left_child:
                self.__insert_data(data, node.left_child)
            else:
                node.left_child = Node(data, node)  # node is the current parent
        # if the data is greater than the root node's data
        # check the right tree
        else:
            if node.right_child:
                self.__insert_data(data, node.right_child)
            else:
                node.right_child = Node(data, node)  # node is the current parent

    def traverse(self):
        if not self.__root:
            print("Empty tree!")
        else:
            self.__traverse_in_place(self.__root)

    def __traverse_in_place(self, node):
        if node.left_child:
            self.__traverse_in_place(node.left_child)

        print(node.data)

        if node.right_child:
            self.__traverse_in_place(node.right_child)

    def remove(self, data):
        if not self.__root:
            print("No item to remove!")
        else:
            self.__root = self.__remove_data(data, self.__root)

    def __remove_data(self, data, node):
        if not Node:
            return
        # search the given data in the tree
        if data < node.data:
            node.left_child = self.__remove_data(data, node.left_child)
        elif data > node.data:
            node.right_child = self.__remove_data(data, node.right_child)
        # if the data is found
        else:
            # check various cases
            # if the target node is a leaf node
            if not node.right_child and not node.left_child:
                del node
                return None  # notifying that the leaf node is deleted
            # if the node has one child node
            # 1) if the right child doesn't exist
            # we store the value of left child, delete the node and return the value of left child
            # this value is updated in the "node.left_child" variable in line 61
            elif not node.right_child:
                temp = node.left_child
                del node
                return temp
            # 2) if the left child node doesn't exist
            # we store the value of right child, delete the node and return the value of right child
            # this value is updated in the "node.right_child" variable in line 63
            elif not node.left_child:
                temp = node.right_child
                del node
                return temp
            # if the target node has two children
            # we first swap the value of either the successor or predecessor(will be a leaf)
            # when the target node stres the new value
            # we can now delete the successor or predecessor value as it is now a duplicate leaf
            else:
                # in this case we are going for the successor value(smallest in the right tree)
                successor = self.__find_successor(node.right_child)  # finding in the right subtree to the node
                node.data = successor.data

                # now deleting the successor leaf which is in the right tree to the node
                node.right_child = self.__remove_data(successor.data, node.right_child)
        return node

    def __find_successor(self, node):
        if node.left_child:
            return self.__find_successor(node.left_child)
        return node

    def min(self):
        if self.__root:
            return self.__find_min(self.__root)

    def __find_min(self, node):
        if node.left_child:
            return self.__find_min(node.left_child)
        return node.data

    def max(self):
        if self.__root:
            return self.__find_max(self.__root)

    def __find_max(self, node):
        if node.right_child:
            return self.__find_max(node.right_child)
        return node.data


if __name__ == '__main__':

    bst = BinarySearchTree()

    bst.insert(33)
    bst.insert(21)
    bst.insert(56)
    bst.insert(-12.98)
    bst.insert(-34)
    bst.insert(-9.66)
    bst.insert(67)
    bst.insert(50)

    bst.remove(21)

    bst.traverse()

    print("min:", bst.min())
    print("max:", bst.max())

