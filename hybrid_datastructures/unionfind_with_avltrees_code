class TreeNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.parent = None
        self.height = 1

class AVLTree:
    def __init__(self, value=None):
        self.root = value

    def insert(self, key):
        self.root = self._insert(self.root, key)

    def _insert(self, node, key):
        if node is None:
            return TreeNode(key)

        if key < node.key:
            node.left = self._insert(node.left, key)
            node.left.parent = node
        else:
            node.right = self._insert(node.right, key)
            node.right.parent = node

        node.height = 1 + max(self._height(node.left), self._height(node.right))
        balance = self._balance_factor(node)

        if balance > 1:
            if key < node.left.key:
                return self._rotate_right(node)
            else:
                node.left = self._rotate_left(node.left)
                return self._rotate_right(node)

        if balance < -1:
            if key > node.right.key:
                return self._rotate_left(node)
            else:
                node.right = self._rotate_right(node.right)
                return self._rotate_left(node)

        return node

    def delete(self, key):
        self.root = self._delete(self.root, key)

    def _delete(self, node, key):
        if node is None:
            return node

        if key < node.key:
            node.left = self._delete(node.left, key)
        elif key > node.key:
            node.right = self._delete(node.right, key)
        else:
            if node.left is None:
                temp = node.right
                if temp:
                    temp.parent = node.parent
                node = None
                return temp
            elif node.right is None:
                temp = node.left
                if temp:
                    temp.parent = node.parent
                node = None
                return temp

            temp = self._get_min_value_node(node.right)
            node.key = temp.key
            node.right = self._delete(node.right, temp.key)

        if node is None:
            return node

        node.height = 1 + max(self._height(node.left), self._height(node.right))
        balance = self._balance_factor(node)

        if balance > 1 and self._balance_factor(node.left) >= 0:
            return self._rotate_right(node)

        if balance < -1 and self._balance_factor(node.right) <= 0:
            return self._rotate_left(node)

        if balance > 1 and self._balance_factor(node.left) < 0:
            node.left = self._rotate_left(node.left)
            return self._rotate_right(node)

        if balance < -1 and self._balance_factor(node.right) > 0:
            node.right = self._rotate_right(node.right)
            return self._rotate_left(node)

        return node

    def _get_min_value_node(self, node):
        if node is None or node.left is None:
            return node

        return self._get_min_value_node(node.left)

    def _height(self, node):
        if node is None:
            return 0
        return node.height

    def _balance_factor(self, node):
        return self._height(node.left) - self._height(node.right)

    def _rotate_left(self, z):
        y = z.right
        T2 = y.left

        y.left = z
        z.right = T2

        z.height = 1 + max(self._height(z.left), self._height(z.right))
        y.height = 1 + max(self._height(y.left), self._height(y.right))

        y.parent = z.parent
        z.parent = y
        if T2:
            T2.parent = z

        return y

    def _rotate_right(self, y):
        x = y.left
        T2 = x.right

        x.right = y
        y.left = T2

        y.height = 1 +(self._height(y.left), self._height(y.right))
        x.height = 1 + max(self._height(x.left), self._height(x.right))

        x.parent = y.parent
        y.parent = x
        if T2:
            T2.parent = y

        return x

    def inorder_traversal(self):
        result = []
        self._inorder_traversal(self.root, result)
        return result

    def _inorder_traversal(self, node, result):
        if node:
            self._inorder_traversal(node.left, result)
            result.append(node.key)
            self._inorder_traversal(node.right, result)

    def get_absolute_parent(self, key):
        node = self._search_recursive(self.root, key)
        if node:
            while node.parent:
                node = node.parent
            return node.key
        return None

    def find(self, key):
        return self._find(self.root, key)

    def _find(self, node, key):
        if node is None:
            return None

        if key < node.key:
            return self._find(node.left, key)
        elif key > node.key:
            return self._find(node.right, key)
        else:
            return node

class UnionFind:
    def __init__(self):
        self.sets = {}

    def make_set(self, key):
        if key not in self.sets:
            self.sets[key] = AVLTree()
            self.sets[key].insert(key)

    def add_item(self, key, item):
        if key not in self.sets:
            self.make_set(key)
        self.sets[key].insert(item)


    def find(self, item):
        for key, tree in self.sets.items():
            if tree.find(item):
                return key
        return None

    def union(self, item1, item2,checkcyclicity=False):
        key1 = self.find(item1)
        key2 = self.find(item2)
        if key1 is None or key2 is None:
            return None

        if key1 == key2:
            if checkcyclicity==True:
                print("The graph is cyclic")
            else:
                print("they belong to same set")
            return

        set1 = self.sets[key1]
        set2 = self.sets[key2]

        if set1.root.height < set2.root.height:
            smaller_key, smaller_set = key1, set1
            larger_key, larger_set = key2, set2
        else:
            smaller_key, smaller_set = key2, set2
            larger_key, larger_set = key1, set1

        elements = smaller_set.inorder_traversal()
        for element in elements:
            if not larger_set.find(element):
                larger_set.insert(element)

        del self.sets[smaller_key]

        return larger_set

    def print_sets(self):
        for key, tree in self.sets.items():
            print(f"{key} : {tree.inorder_traversal()}")

    def remove_item(self,key):
        if key not in self.sets.keys():
            parent=self.find(key)
            self.sets[parent].delete(key)
        else:
            del self.sets[key]




def print_menu():
    print("1. Make Set")
    print("2. Add Item")
    print("3. Find")
    print("4. Union")
    print("5. Remove Item")
    print("6. Print Sets")
    print("7. Exit")

uf = UnionFind()

while True:
    print_menu()
    choice = input("Enter your choice (1-7): ")

    if choice == "1":
        key = input("Enter the key for the set: ")
        uf.make_set(key)
        print("Set created.")

    elif choice == "2":
        key = input("Enter the key of the set: ")
        item = input("Enter the item to add: ")
        uf.add_item(key, item)
        print("Item added.")

    elif choice == "3":
        item = input("Enter the item to find: ")
        key = uf.find(item)
        if key is not None:
            print(f"Item found in set with key {key}")
        else:
            print("Item not found.")

    elif choice == "4":
        item1 = input("Enter the first item: ")
        item2 = input("Enter the second item: ")
        check_cyclicity = input("Do you want to check cyclicity? (y/n): ").lower() == "y"
        uf.union(item1, item2, check_cyclicity)
        print("Union performed.")

    elif choice == "5":
        key = input("Enter the key or item to remove: ")
        uf.remove_item(key)
        print("Item removed.")

    elif choice == "6":
        uf.print_sets()

    elif choice == "7":
        print("Exiting...")
        break

    else:
        print("Invalid choice. Please try again.")
