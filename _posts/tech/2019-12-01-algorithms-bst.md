---
layout: post
title: Algorithms筆記-Binary Search Tree 二叉查找樹
category: tech
tags: [algorithms]
---

## Definition

![](http://www.hauchenglee.com/assets/images/tech/bst.png)

A *binary search tree* (BST) is a binary tree where each node has a `Comparable` key 
(and an associated value) and satisfies the restriction that the key in any node is larger than 
the keys in all nodes in that node's left subtree and smaller than the keys in all nodes in that 
node's right subtree.

![](http://www.hauchenglee.com/assets/images/tech/binary-tree-anatomy.png)

![](http://www.hauchenglee.com/assets/images/tech/bst-anatomy.png)

## Basic implementation

```java
package algs4;

import edu.princeton.cs.algs4.StdIn;
import edu.princeton.cs.algs4.StdOut;

import java.util.NoSuchElementException;

public class BST<Key extends Comparable<Key>, Value> {
    private Node root; // root of BST

    private class Node {
        private Key key;           // sorted by key
        private Value val;         // associated data
        private Node left, right;  // left and right subtrees
        private int size;          // number of nodes in subtree

        public Node(Key key, Value val, int size) {
            this.key = key;
            this.val = val;
            this.size = size;
        }
    }

    /**
     * Initialized an empty symbol table
     */
    public BST() {
    }

    /**
     * Returns true is this symbol table is empty.
     *
     * @return {@code true} if this symbol table is empty; {@code false} otherwise
     */
    public boolean isEmpty() {
        return size() == 0;
    }

    /**
     * Returns the number of key-value pairs in this symbol table.
     *
     * @return the number of key-value pairs in this symbol table
     */
    public int size() {
        return size(root);
    }

    // return number of key-value pairs in BST rooted at x
    private int size(Node x) {
        if (x == null) return 0;
        return x.size;
    }

    /**
     * Does this symbol table contain the given key?
     *
     * @param key the key
     * @return {@code true} if this symbol table contains {@code key} and
     * {@code false} otherwise
     * @throws IllegalArgumentException if {@code key} is {@code null}
     */
    public boolean contains(Key key) {
        if (key == null) throw new IllegalArgumentException("argument to contains() is null");
        return get(key) != null;
    }

    /**
     * Returns the value associated with the given key.
     *
     * @param key the key
     * @return the value associated with the given key if the key is in the symbol table
     * and {@code null} if the key is not in the symbol table
     * @throws IllegalArgumentException if {@code key} is {@code null}
     */
    public Value get(Key key) {
        return get(root, key);
    }

    private Value get(Node x, Key key) {
        if (key == null) throw new IllegalArgumentException("calls get() with a null key");
        if (x == null) return null;
        int cmp = key.compareTo(x.key);
        if (cmp < 0) return get(x.left, key);
        else if (cmp > 0) return get(x.right, key);
        else return x.val;
    }

    /**
     * Inserts the specified key-value pair into the symbol table, overwriting the old
     * value with the new value if the symbol table already contains the specified key.
     * Deletes the specified key (and its associated value) from this symbol table
     * if the specified value is {@code null}
     *
     * @param key the key
     * @param val the value
     * @throws IllegalArgumentException if {@code key} is {@code null}
     */
    public void put(Key key, Value val) {
        if (key == null) throw new IllegalArgumentException("calls put() with a null key");
        if (val == null) {
            delete(key);
            return;
        }
        root = put(root, key, val);
        assert check();
    }

    private Node put(Node x, Key key, Value val) {
        if (x == null) return new Node(key, val, 1);
        int cmp = key.compareTo(x.key);
        if (cmp < 0) x.left = put(x.left, key, val);
        else if (cmp > 0) x.right = put(x.right, key, val);
        x.size = 1 + size(x.left) + size(x.right);
        return x;
    }

    /**
     * Removes the smallest key and associated value from the symbol table.
     *
     * @throws NoSuchElementException if the symbol table is empty
     */
    public void deleteMin() {
        if (isEmpty()) throw new NoSuchElementException("Symbol table underflow");
        root = deleteMin(root);
        assert check();
    }

    private Node deleteMin(Node x) {
        if (x.left == null) return x.right;
        x.left = deleteMin(x.left);
        x.size = size(x.left) + size(x.right) + 1;
        return x;
    }

    /**
     * Removes the largest key and associated value from the symbol table.
     *
     * @throws NoSuchElementException if the symbol table is empty
     */
    public void deleteMax() {
        if (isEmpty()) throw new NoSuchElementException("Symbol table underflow");
        root = deleteMax(root);
        assert check();
    }

    private Node deleteMax(Node x) {
        if (x.right == null) return x.left;
        x.right = deleteMax(x.right);
        x.size = size(x.left) + size(x.right) + 1;
        return x;
    }

    /**
     * Remove teh specified key and its associated value from this symbol table
     * (if the key is in this symbol table).
     *
     * @param key the key
     * @throws IllegalArgumentException if {@code key} if {@code null}
     */
    public void delete(Key key) {
        if (key == null) throw new IllegalArgumentException("calls delete() with a null key");
        root = delete(root, key);
        assert check();
    }

    private Node delete(Node x, Key key) {
        if (x == null) return null;

        int cmp = key.compareTo(x.key);
        if (cmp < 0) x.left = delete(x.left, key);
        else if (cmp > 0) x.right = delete(x.right, key);
        else {
            if (x.right == null) return x.left;
            if (x.left == null) return x.right;
            Node t = x;
            x = min(t.right);
            x.right = deleteMin(t.right);
            x.left = t.left;
        }
        x.size = size(x.left) + size(x.right) + 1;
        return x;
    }

    /**
     * Returns the smallest key in the symbol table
     *
     * @return the smallest key in the symbol table
     * @throws NoSuchElementException if the symbol table is empty
     */
    public Key min() {
        if (isEmpty()) throw new NoSuchElementException("calls min() with empty symbol table");
        return min(root).key;
    }

    private Node min(Node x) {
        if (x.left == null) return x;
        else return min(x.left);
    }

    /**
     * Returns the largest key in the symbol table
     *
     * @return the largest key in the symbol table
     * @throws NoSuchElementException if the symbol table is empty
     */
    public Key max() {
        if (isEmpty()) throw new NoSuchElementException("calls max() with empty symbol table");
        return max(root).key;
    }

    private Node max(Node x) {
        if (x.right == null) return x;
        else return max(x.right);
    }

    /**
     * Returns the largest key in the symbol table less than or equal to {@code key}.
     *
     * @param key the key
     * @return the largest key in the symbol table less than or equal to {@code key}
     * @throws NoSuchElementException   if there is no such key
     * @throws IllegalArgumentException if {@code key} is {@code null}
     */
    public Key floor(Key key) {
        if (key == null) throw new IllegalArgumentException("argument to floor() is null");
        if (isEmpty()) throw new NoSuchElementException("calls floor() with empty symbol table");
        Node x = floor(root, key);
        if (x == null) return null;
        else return x.key;
    }

    private Node floor(Node x, Key key) {
        if (x == null) return null;
        int cmp = key.compareTo(x.key);
        if (cmp == 0) return x;
        if (cmp < 0) return floor(x.left, key);
        Node t = floor(x.right, key);
        if (t != null) return t;
        else return x;
    }

    public Key floor2(Key key) {
        return floor2(root, key, null);
    }

    private Key floor2(Node x, Key key, Key best) {
        if (x == null) return best;
        int cmp = key.compareTo(x.key);
        if (cmp < 0) return floor2(x.left, key, best);
        else if (cmp > 0) return floor2(x.right, key, x.key);
        else return x.key;
    }

    /**
     * Returns the smallest key in the symbol table greater than or equal to {@code key}
     *
     * @param key the key
     * @return the smallest key in the symbol table greater than or equal to {@code key}
     * @throws NoSuchElementException   if there is no such key
     * @throws IllegalArgumentException if {@code key} if {@code null}
     */
    public Key ceiling(Key key) {
        if (key == null) throw new IllegalArgumentException("argument to ceiling() is null");
        if (isEmpty()) throw new NoSuchElementException("calls ceiling() with empty symbol table");
        Node x = ceiling(root, key);
        if (x == null) return null;
        else return x.key;
    }

    private Node ceiling(Node x, Key key) {
        if (x == null) return null;
        int cmp = key.compareTo(x.key);
        if (cmp == 0) return x;
        if (cmp < 0) {
            Node t = ceiling(x.left, key);
            if (t != null) return t;
            else return x;
        }
        return ceiling(x.right, key);
    }

    /**
     * Return the key in the symbol table whose rank is {@code k}.
     * This is the (k+1)st smallest key in the symbol table.
     *
     * @param k the order statistic
     * @return the key in the symbol table of rank {@code k}
     * @throws IllegalArgumentException unless {@code k} is between 0 and <em>n</em>-1
     */
    public Key select(int k) {
        if (k < 0 || k >= size()) {
            throw new IllegalArgumentException("argument to select() is invalid: " + k);
        }
        Node x = select(root, k);
        return x.key;
    }

    // Return key of rank k
    private Node select(Node x, int k) {
        if (x == null) return null;
        int t = size(x.left);
        if (t > k) return select(x.left, k);
        else if (t < k) return select(x.right, k - t - 1);
        else return x;
    }

    /**
     * Return the number of keys in the symbol table strictly less than {@code key}
     *
     * @param key the key
     * @return the number of keys in the symbol table strictly less than {@code key}
     * @throws IllegalArgumentException if {@code key} is {@code null}
     */
    public int rank(Key key) {
        if (key == null) throw new IllegalArgumentException("argument to rank() is null");
        return rank(key, root);
    }

    // Number of keys in the subtree less than key.
    private int rank(Key key, Node x) {
        if (x == null) return 0;
        int cmp = key.compareTo(x.key);
        if (cmp < 0) return rank(key, x.left);
        else if (cmp > 0) return 1 + size(x.left) + rank(key, x.right);
        else return size(x.left);
    }

    /**
     * Returns all keys in the symbol table as an {@code Iterable}.
     * To iterate over all of the keys in the symbol table named {@code st},
     * use the foreach notation: {@code for (Key key : st.keys())}.
     *
     * @return all keys in the symbol table
     */
    public Iterable<Key> keys() {
        if (isEmpty()) return new Queue<Key>();
        return keys(min(), max());
    }

    /**
     * Returns all keys in the symbol table in the given range,
     * as an {@code Iterable}.
     *
     * @param lo lo minimum endpoint
     * @param hi hi maximum endpoint
     * @return all keys in the symbol table between {@code lo}
     * (inclusive) and {@code hi} (inclusive)
     * @throws IllegalArgumentException if either {@code lo} or {@code hi}
     *                                  is {@code null}
     */
    public Iterable<Key> keys(Key lo, Key hi) {
        if (lo == null) throw new IllegalArgumentException("first argument to keys() is null");
        if (hi == null) throw new IllegalArgumentException("second argument to keys() is null");

        Queue<Key> queue = new Queue<>();
        keys(root, queue, lo, hi);
        return queue;
    }

    private void keys(Node x, Queue<Key> queue, Key lo, Key hi) {
        if (x == null) return;
        int cmplo = lo.compareTo(x.key);
        int cmphi = hi.compareTo(x.key);
        if (cmplo < 0) keys(x.left, queue, lo, hi);
        if (cmplo <= 0 && cmphi >= 0) queue.enqueue(x.key);
        if (cmphi > 0) keys(x.right, queue, lo, hi);
    }

    /**
     * Returns the number of keys in the symbol table in the given range.
     *
     * @param lo lo minimum endpoint
     * @param hi hi maximum endpoint
     * @return the number of keys in the symbol table between {@code lo}
     * (inclusive) and {@code hi} (inclusive)
     * @throws IllegalArgumentException if either {@code lo} or {@code hi}
     *                                  is {@code null}
     */
    public int size(Key lo, Key hi) {
        if (lo == null) throw new IllegalArgumentException("first argument to size() is null");
        if (hi == null) throw new IllegalArgumentException("second argument to size() is null");

        if (lo.compareTo(hi) > 0) return 0;
        if (contains(hi)) return rank(hi) - rank(lo) + 1;
        else return rank(hi) - rank(lo);
    }

    /**
     * Returns the height of the BST (for debugging).
     *
     * @return the height of the BST (a 1-node tree has height 0)
     */
    public int height() {
        return height(root);
    }

    private int height(Node x) {
        if (x == null) return -1;
        return 1 + Math.max(height(x.left), height(x.right));
    }

    /**
     * Returns the keys in the BST in level (for debugging).
     *
     * @return the keys in the BST in level order traversal
     */
    public Iterable<Key> levelOrder() {
        Queue<Key> keys = new Queue<>();
        Queue<Node> queue = new Queue<>();
        queue.enqueue(root);
        while (!queue.isEmpty()) {
            Node x = queue.dequeue();
            if (x == null) continue;
            keys.enqueue(x.key);
            queue.enqueue(x.left);
            queue.enqueue(x.right);
        }
        return keys;
    }

    /*************************************************************************
     *  Check integrity of BST data structure.
     ***************************************************************************/

    private boolean check() {
        if (!isBST()) StdOut.println("Not in symmetric order");
        if (!isSizeConsistent()) StdOut.println("Subtree counts not consistent");
        if (!isRankConsistent()) StdOut.println("Ranks not consistent");
        return isBST() && isSizeConsistent() && isRankConsistent();
    }

    // does this binary tree satisfy symmetric order?
    // Note: this test also ensures that data structure is a binary tree since order is strict
    private boolean isBST() {
        return isBST(root, null, null);
    }

    // is the tree rooted at x a BST with all keys strictly between min and max
    // (if min or max is null, treat as empty constraint)
    // Credit: Bob Dondero's elegant solution
    private boolean isBST(Node x, Key min, Key max) {
        if (x == null) return true;
        if (min != null && x.key.compareTo(min) <= 0) return false;
        if (max != null && x.key.compareTo(max) >= 0) return false;
        return isBST(x.left, min, x.key) && isBST(x.right, x.key, max);
    }

    // are the size fields correct?
    private boolean isSizeConsistent() {
        return isSizeConsistent(root);
    }

    private boolean isSizeConsistent(Node x) {
        if (x == null) return true;
        if (x.size != size(x.left) + size(x.right) + 1) return false;
        return isSizeConsistent(x.left) && isSizeConsistent(x.right);
    }

    private boolean isRankConsistent() {
        for (int i = 0; i < size(); i++)
            if (i != rank(select(i))) return false;
        for (Key key : keys())
            if (key.compareTo(select(rank(key))) != 0) return false;
        return true;
    }

    /**
     * Unit tests the {@code BST} data type.
     *
     * @param args the command-line arguments
     */
    public static void main(String[] args) {
        BST<String, Integer> st = new BST<String, Integer>();
        for (int i = 0; !StdIn.isEmpty(); i++) {
            String key = StdIn.readString();
            if ((st.size() > 1) && (st.floor(key) != st.floor2(key)))
                throw new RuntimeException("floor() function inconsistent");
            st.put(key, i);
        }

        for (String s : st.levelOrder())
            StdOut.println(s + " " + st.get(s));

        StdOut.println();

        for (String s : st.keys())
            StdOut.println(s + " " + st.get(s));
    }
}

```

Source: [BST.java](https://algs4.cs.princeton.edu/32bst/BST.java.html){:target="_blank"}

### search

> A recursive algorithm to search for a key in BST follow immediately from the 
> recursive structure: If the tree is empty, we have a search miss; if the search key is equal 
> to the key at the root, we have a search hit. Otherwise, we search (recursively) in the 
> appropriate subtree. The recursive `get()` method implements this algorithm directly. It 
> takes a node (root of a subtree) as first argument and a key as second argument, 
> starting with the root of the tree and the search key.

![](http://www.hauchenglee.com/assets/images/tech/bst-search.png)

<br>

思路：
- 將當下parent node與tree root node比值，得出`-1`（小於）、`1`（大於）或是`0`（等於）
- 如果找到，返回，否則繼續使用遞歸算法，持續求解

```
/**
 * Returns the value associated with the given key.
 *
 * @param key the key
 * @return the value associated with the given key if the key is in the symbol table
 * and {@code null} if the key is not in the symbol table
 * @throws IllegalArgumentException if {@code key} is {@code null}
 */
public Value get(Key key) {
    return get(root, key);
}

private Value get(Node x, Key key) {
    if (key == null) throw new IllegalArgumentException("calls get() with a null key");
    if (x == null) return null;
    int cmp = key.compareTo(x.key);
    if      (cmp < 0) return get(x.left, key);
    else if (cmp > 0) return get(x.right, key);
    else              return x.val;
}
```

### insert

> Insert is not much more difficult to implement than search. Indeed, a search for a 
> key not in the tree ends at a null link, and all that we need to do is replace that link with a 
> new node containing the key. The recursive `put()` method accomplishes this task using 
> logic similar to that we used for the recursive search: 
> - If the tree is empty, we return a new node containing the key and value; 
> - if the search key is less than the key at the root, we set the left link to the result of inserting the key into the left subtree; 
> - otherwise, we set the right link to the result of inserting the key into the right subtree.

![](http://www.hauchenglee.com/assets/images/tech/bst-insert.png)

思路：
 
不斷的遞歸搜索左右孩子，知道找到一個`null`節點，就把`<key, value>`插入。
如果`key`本身在樹中，則更新值。

插入操作可以用遞歸來做。（凡是涉及鏈表的插入操作，都可以考慮用遞歸，如果用非遞歸相對來說麻煩點，
因為插入操作是在父節點上插入一個左右各半的孩子節點，如果root為空還得單獨判斷）

```
/**
 * Inserts the specified key-value pair into the symbol table, overwriting the old 
 * value with the new value if the symbol table already contains the specified key.
 * Deletes the specified key (and its associated value) from this symbol table
 * if the specified value is {@code null}.
 *
 * @param  key the key
 * @param  val the value
 * @throws IllegalArgumentException if {@code key} is {@code null}
 */
public void put(Key key, Value val) {
    if (key == null) throw new IllegalArgumentException("calls put() with a null key");
    if (val == null) {
        delete(key);
        return;
    }
    root = put(root, key, val);
    assert check();
}

private Node put(Node x, Key key, Value val) {
    if (x == null) return new Node(key, val, 1);
    int cmp = key.compareTo(x.key);
    if      (cmp < 0) x.left  = put(x.left,  key, val);
    else if (cmp > 0) x.right = put(x.right, key, val);
    else              x.val   = val;
    x.size = 1 + size(x.left) + size(x.right);
    return x;
}
```

<br>

動畫演示：

![](http://www.hauchenglee.com/assets/images/tech/bst-insert-animation.gif)

## Analysis

根據不同的插入順序，二叉樹會呈現出不同的形狀，最好的狀態當然是完全平衡的狀態，這個時候搜索效率最高，
最壞的形式就是插入的序列有序，這個時候二叉樹的搜索效率就是N了。

![](http://www.hauchenglee.com/assets/images/tech/bst-case.png)

<br>

這個隨機插入的效果：

![](http://www.hauchenglee.com/assets/images/tech/bst-insert-random.gif)

<br>

研究表明，如果插入是隨機的情況下，二叉樹的平均深度都不深。所以通常二叉樹的效率還是很高的。

![](http://www.hauchenglee.com/assets/images/tech/bst-research.png)

![](http://www.hauchenglee.com/assets/images/tech/bst-time.png)

## Order-based methods and deletion

### minimum and maximum

這個很簡單，最小的話直接**搜索到**最左的孩子，最大的話直接**搜索到**最右的孩子。

p.s. 搜索到意思就代表用遞歸算法來實現

![](http://www.hauchenglee.com/assets/images/tech/bst-min-max.png)

### floor and ceiling

`floor`：找到比key小的最大key<br>
`ceiling`：找到比key大的最小key

![](http://www.hauchenglee.com/assets/images/tech/bst-floor-ceiling.png)

### selection

> Suppose that we seek the key of rank *k* (the key such that precisely *k* other 
> keys in the BST are smaller). If the number of keys *t* in the left subtree is larger than *k*,
> we look (recursively) for the key of rank `k` in the left subtree; if *t* is equal to *k*, we return 
> the key at the root; and if *t* is smaller than *k*, we look (recursively) for the key of rank *k - t - 1* 
> in the right subtree.

假設我們尋找等級為k 的密鑰（該密鑰使得BST中的其他k個密鑰恰好較小）。
如果左子樹中的鍵數t大於k，我們將（遞歸）尋找左子樹中等級k的鍵；如果t等於k，則返回根的鍵；
如果t小於k，我們（遞歸）在右邊的子樹中尋找等級k-t-1的關鍵字。

![](http://www.hauchenglee.com/assets/images/tech/bst-select.png)

### rank

返回鍵小於key的個數。有三種情況：
- root == key：左孩子`count`（因為只有左孩子小於它）
- root < key：左孩子`count` + 1（自己）+ 右孩子繼續找
- root > key：左孩子繼續找（給定key還沒找到比自己小的）

![](http://www.hauchenglee.com/assets/images/tech/bst-rank.png)

### delete



### range search

根據二叉樹的結構，很簡單了：
- 遍歷左子樹
- 把自己key插入列隊
- 遍歷右子樹

```
public Iterable<Key> keys() {
    Queue<Key> q = new Queue<Key>();
    inorder(root, q);
    return q;
}

private void inorder(Node x, Queue<Key> q) {
    if (x == null) return;
    inorder(x.left, q);
    q.enqueue(x.key);
    inorder(x.right, q);
}
```

## Operations summary

用二叉樹實現的符號表操作，其時間複雜度要遠遠小於其他方式。

![](http://www.hauchenglee.com/assets/images/tech/bst-operations-summary.png)

## Reference

- [Binary Search Trees](https://algs4.cs.princeton.edu/32bst/){:target="_blank"}
- [《 常见算法与数据结构》符号表ST（3）——二叉查找树 （附动画） - Voidsky - CSDN博客](https://blog.csdn.net/hk2291976/article/details/51407287){:target="_blank"}
- [《 常见算法与数据结构》符号表ST（4）——二叉查找树删除 （附动画） - Voidsky - CSDN博客](https://blog.csdn.net/hk2291976/article/details/51407569){:target="_blank"}

---