class Node:
    def __init__(self, key=None):
        self.key = key
        self.left = None
        self.right = None
        self.parent = None


class AVLTree:

    def __init__(self):
        self.root = None

    def run(self) -> None:
        while True:
            command = input("Digite 'insert <chave>' para inserir um valor ou 'exit' para sair: ")
            if command == 'exit':
                break
            elif command.startswith('insert'):
                _, key = command.split()
                self.insert(int(key))
                self.print_tree(self.root)
            elif command.startswith('delete'):
                _, key = command.split()
                self.delete(int(key))
                self.print_tree(self.root)
            print("")

    def insert(self, key: int) -> None:
        if self.root is None:
            self.root = Node(key)
        else:
            self._insert(key, self.root)

    def _insert(self, key: int, node: Node) -> None:
        if key < node.key:
            if node.left is None:
                node.left = Node(key)
                node.left.parent = node
                self.check_balance()
            else:
                self._insert(key, node.left)
        elif key > node.key:
            if node.right is None:
                node.right = Node(key)
                node.right.parent = node
                self.check_balance()
            else:
                self._insert(key, node.right)
        else:
            print("Chave já existe na árvore!")

    def print_tree(self, node: Node, height: int = 1) -> None:
        if node is not None:
            self.print_tree(node.right, height+1)
            print(f'{" ".join([""]*height*3)} {node.key}, h={height}')
            self.print_tree(node.left, height+1)

    def max_height(self, node: Node, cur_height: int) -> int:
        if node is None:
            return cur_height
        left_height = self.max_height(node.left, cur_height+1)
        right_height = self.max_height(node.right, cur_height+1)
        return max(left_height, right_height)

    def get_balance(self, node: Node) -> int:
        if node is None:
            return 0
        left_max = self.max_height(node.left, 0)
        right_max = self.max_height(node.right, 0)
        return left_max - right_max

    def check_balance(self) -> None:
        if self.root is not None:
            self._check_balance(self.root)

    def _check_balance(self, node: Node) -> None:
        if node is None:
            return

        self._check_balance(node.left)
        self._check_balance(node.right)

        primary_balance = self.get_balance(node)
        secondary_left_balance = self.get_balance(node.left)
        secondary_right_balance = self.get_balance(node.right)

        if primary_balance > 1:
            if secondary_left_balance < 0:
                print(f"Rotação Dupla: \n Nó primário({node.key}) com balanço {primary_balance} e nó secundario a esquerda({node.left.key}) com balanço {secondary_left_balance}")
                self.left_rotation(node.left)
                self.right_rotation(node)
            else:
                print(f"Rotação Simples \n Nó primário({node.key}) com balanço {primary_balance}")
                self.right_rotation(node)
        elif primary_balance < -1:
            if secondary_right_balance > 0:
                print(f"Rotação Dupla: \n Nó primário({node.key}) com balanço {primary_balance} e nó secundario a direita({node.right.key}) com balanço {secondary_right_balance}")
                self.right_rotation(node.right)
                self.left_rotation(node)
            else:
                print(f"Rotação Simples \n Nó primário({node.key}) com balanço {primary_balance}")
                self.left_rotation(node)

    def right_rotation(self, node: Node) -> None:
        """
            if node == Z:
                          (Z)
                         /
                     (Y)         --->        (Y)
                    /                       /   \
                 (x)                     (X)    (Z)

            if node == Y:
                (Z)                (Z)
                   \                  \
                    (Y)    --->        (X)
                   /                      \
                (X)                        (Y)
        """
        node_parent = node.parent
        node_left = node.left
        right_of_the_node_left = node_left.right
        node_left.right = node
        node.parent = node_left
        node.left = right_of_the_node_left
        if right_of_the_node_left is not None:
            right_of_the_node_left.parent = node
        node_left.parent = node_parent
        if node_left.parent is None:
            self.root = node_left
        else:
            if node_left.parent.left == node:
                node_left.parent.left = node_left
            else:
                node_left.parent.right = node_left

    def left_rotation(self, node: Node) -> None:
        """
            if node == Z:
                (Z)
                   \
                    (Y)         --->          (Y)
                       \                     /   \
                       (x)                (X)    (Z)

            if node == Y:
                    (Z)                     (Z)
                   /                       /
                (Y)          --->       (X)
                   \                   /
                    (X)             (Y)
        """
        node_parent = node.parent
        node_right = node.right
        left_of_the_node_right = node_right.left
        node_right.left = node
        node.parent = node_right
        node.right = left_of_the_node_right
        if left_of_the_node_right is not None:
            left_of_the_node_right.parent = node
        node_right.parent = node_parent
        if node_right.parent is None:
            self.root = node_right
        else:
            if node_right.parent.left == node:
                node_right.parent.left = node_right
            else:
                node_right.parent.right = node_right

    def delete(self, key):
        return self._delete(self.find(key))

    def find(self, key):
        if self.root is not None:
            return self._find(key, self.root)
        else:
            return None

    def _find(self, key, node):
        if key == node.key:
            return node
        elif key < node.key and node.left is not None:
            return self._find(key, node.left)
        elif key > node.key and node.right is not None:
            return self._find(key, node.right)
        else:
            return None

    def _delete(self, node):
        if node is None:
            print("Chave não existe na árvore!")

        node_parent = node.parent
        if node.left is None and node.right is None:
            if node_parent is None:
                self.root = None
            else:
                if node_parent.left == node:
                    node_parent.left = None
                else:
                    node_parent.right = None
        elif node.left is not None and node.right is not None:
            current = node.right
            while current.left is not None:
                current = current.left
            node.key = current.key
            self._delete(current)
        elif node.left is not None or node.right is not None:
            child = node.left if node.left is not None else node.right
            if node_parent is None:
                self.root = child
            else:
                if node_parent.left == node:
                    node_parent.left = child
                else:
                    node_parent.right = child
            child.parent = node_parent
        self.check_balance()
