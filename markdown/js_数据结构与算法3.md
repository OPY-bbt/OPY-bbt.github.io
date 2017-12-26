# 树

## 二叉树搜索树（BST）
- 只能有两个子节点
- 左侧存比父节点小的值        
```js
function BinarySearchTree() {
    var Node = function(key) {
        this.key = key;
        this.left = null;
        this.right = null;
    }
    
    var root = null;
    this.insert = function(key) {
        var insertNode = function(node, newNode) {
            if (newNode.key < node.key) {
                if (node.left == null) {
                    node.left = newNode;
                } else {
                    insertNode(node.left, newNode);
                }
            } else {
                if (node.right == null) {
                    node.right = newNode;
                } else {
                    insertNode(node.right, newNode);
                }
            }
        }
        var newNode = new Node(key);
        if (root === null) {
            root = newNode;
        } else {
            insertNode(root, newNode);
        }
    }
    this.search = function(key) {
        const searchNode = (node, key) => {
            if (node === null) {
                return false;
            }
            if (key === node.key) {
                return true;
            }
            if (key < node.key) {
                return searchNode(node.left, key);
            }
            if (key > node.key) {
                return searchNode(node.right, key);
            }
        }
        searchNode(root, key);
    }
    this.inOrderTraverse = function(callback) {
        // 中序遍历
        const inOrderTraverseNode = (node, callback) => {
            if (node !== null) {
                inOrderTraverseNode(node.left, callback);
                callback(node.key);
                inOrderTraverseNode(node.right, callback);
            }
        }
        inOrderTraverseNode(root, callback);
    }
    this.preOrderTraverse = function() {
        // 先序遍历
        const preOrderTraverseNode = (node, callback) => {
            if (node !== null) {
                callback(node.key);
                preOrderTraverseNode(node.left);
                preOrderTraverseNode(node.right);
            }
        }
        preOrderTraverseNode()
    }
    this.postOrderTraverse = function() {
        // 后序遍历
        const postOrderTraverseNode = (node, callback) => {
            if (node !== null) {
                postOrderTraverseNode(node.left, callback);
                postOrderTraverseNode(node.right, callback);
                callback(node.key);
            }
        }
        postOrderTraverseNode(root, callback);
    }
    this.min = function() {
        const getMin = (node) => {
            if (node) {
                let leftest = node;
                while(leftest && leftest.left !== null) {
                    leftest = leftest.left;
                }
                return leftest.key;
            }
            return null;
        }
        return getMin(root);
    }
    this.max = function() {
        const getMax = (node) => {
            if (node) {
                let rightest = node;
                while(rightest && rightest.right !== null) {
                    rightest = rightest.right;
                }
                return rightest.key;
            }
            return null;
        }
        return getMax(root);
    }
    this.remove = function(key) {
        const findMinNode = (node) => {
            if (node) {
                let leftest = node;
                while(leftest && leftest.left !== null) {
                    leftest = leftest.left;
                }
                return leftest;
            }
            return null;
        }
        const removeNode = (node, key) => {
            if (node === null) {
                return null;
            }
            if (key < node.key) {
                node.left = removeNode(node.left, key);
                return node;
            } else if (key > node.key) {
                node.right = removeNode(node.right, key);
                return node;
            } else {
                if (node.left === null && node.right === null) {
                    node = null;
                    return node;
                }

                if (node.left === null) {
                    node = node.right;
                    return node;
                }

                if (node.right === null) {
                    node = node.left;
                    return node;
                }

                const dest = findMinNode(node.right);
                node.key = dest.key;
                node.right = removeNode(node.right, dest.key);
                return node;
            }
        }
    }
}
```