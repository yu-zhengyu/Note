---
title: 总结(VII) Design review --- 设计题目总结
date: 2016-01-07 09:22:30
tags:
- leetcode
- 刷题
- 设计
categories:
- 总结
---
基本上看到哪里就记到哪里吧，看到高频的就记一下。

### 洗牌算法
这个有意思了，以前重来没想过这个问题。有些公司会问你怎么去设计ipod，然后重点问你怎么去设计随机播放。尽可能有少的资源，而且不会有重复的。当然，简单地调用ramdon估计就是回被提出去的了。然后在网上看到一个简单的解法，先记下，有空再去深究：

####code[java]
```
public class Shuffle {
    public static void main(String arg[]){  
        String[] musicUrl={"/music/1.mp3",  
                "/music/2.mp3",  
                "/music/3.mp3",  
                "/music/4.mp3",  
                "/music/5.mp3",  
                "/music/6.mp3",  
                "/music/7.mp3",  
                "/music/8.mp3",  
                "/music/9.mp3",  
                "/music/10.mp3"};  
          
        shuffle(musicUrl);  
        for(String music:musicUrl){  
            System.out.println(music);  
        }  
    }  
    public static void shuffle(String[] musicUrl){  
        int shuffle_key;  
        String temp;  
        Random rand = new Random();  
          
        for(int i = 0; i < musicUrl.length; i++){  
            shuffle_key = rand.nextInt(musicUrl.length-1);  
            temp = musicUrl[i];  
            musicUrl[i] = musicUrl[shuffle_key];  
            musicUrl[shuffle_key] = temp;  
        }  
    }  
}

```

***

### Tri 字典树和回溯法结合的题目 -- leetcode 211. Add and Search Word - Data structure design

这题过了，基本上可以说自己应该稍微掌握了字典树和回溯法了。当然，其实这两个并不难，只要我们仔细想好每一个步骤就好。其实回溯法和暴力比较像，不过要注意边角条件就是了。

字典树还是比较好理解的，如果有一些单词有了共同前缀，我们就省略一些空间来存储这些数据，而是去主要存储后缀就好。而字典树的内部可以用map或者数组都可以，看大家自己喜欢，我的字典树是用26位数组存的，稍微浪费了一点空间。

下面是字典树的具体实现：

####code[java]
```
class TrieNode {
    // Initialize your data structure here.
    boolean isWord;
    char val;
    TrieNode[] ChildNode = new TrieNode[26];
    public TrieNode() {
    }
    public TrieNode(char c) {
        TrieNode n = new TrieNode();
        n.val = c;
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
        root.val = ' ';
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        TrieNode p = root;
        for(int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if(p.ChildNode[c - 'a'] == null) {
                p.ChildNode[c - 'a'] = new TrieNode(c);
            }
            p = p.ChildNode[c - 'a'];
        }
        p.isWord = true;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        TrieNode p = root;
        for(int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if(p.ChildNode[c - 'a'] == null)
                return false;
            p = p.ChildNode[c - 'a'];
        }
        return p.isWord;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        TrieNode p = root;
        for(int i = 0; i < prefix.length(); i++) {
            char c = prefix.charAt(i);
            if(p.ChildNode[c - 'a'] == null)
                return false;
            p = p.ChildNode[c - 'a'];
        }
        return true;
    }
}

```

在搜索是否存在一个单词的时候，有点像链表的便利。注意我们的字典树中存储了一个boolean变量，其实这个变量就是为了判断这个字母结尾的时候，是不是构成一个单词。

按照上面的例子，我自己又实现了一下leetcode的211题目，然后搜索的时候用了回溯法，一次通过了。下面给出自己的解法。其实还是按照回溯法之前的套路，用一个help函数来帮组我们。如果遇到了".", 我们就把26个字母都要扫一遍，只要有一个遍历是跑通的，我们就说明存在这个函数。


####code[java]
```
public class WordDictionary {
    public class Node {
        public Node[] carray;
        public boolean iscorrent;
        public char c;
        public Node(char c) {
            this.c = c;
            carray = new Node[26];
            iscorrent = false;
        }
    }
    
    public Node root;
    
    public WordDictionary() {
        root = new Node(' '); 
    }
    
    // Adds a word into the data structure.
    public void addWord(String word) {
        Node current = root;
        for(int i = 0; i < word.length(); i++) {
            if(current.carray[word.charAt(i) - 'a'] == null) {
                current.carray[word.charAt(i) - 'a'] = new Node(word.charAt(i));
            }
            current = current.carray[word.charAt(i) - 'a'];
        }
        current.iscorrent = true;
    }
    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        Node current = root;
        int index = 0;
        boolean result = help(current, word, index);
        return result;
    }
    
    public boolean help(Node temp, String word, int index) {
        if(temp == null)
            return false;
        if(word.length() == index)
            return temp.iscorrent;
        char c = word.charAt(index);
        if(c == '.') {
            boolean tempresult = false;
            for(int i = 0; i < 26; i++) {
                tempresult |= help(temp.carray[i], word, index + 1);
            }
            return tempresult;
        }
        else {
            if(temp.carray[c - 'a'] == null)
                return false;
            else {
                return help(temp.carray[c - 'a'], word, index + 1);
            }
        }
    }
    
    /**
     * @param args
     */
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        WordDictionary wordDictionary = new WordDictionary();
        wordDictionary.addWord("word");
        if(wordDictionary.search("patter"))
            System.out.println("OK");
        else {
            System.out.println("ERROR");
        }
    }

}

```

***

### Suppose you have a logger class that is used to log error and warning messages. How can you implement this class while using the Singleton design pattern?

这题也是考察基本的Singleton方法。既然我们只能产生一个实例，那么，只能够将构造函数设为privete. 换句话说,外部是没办法通过构造函数创建实例.所以,我们只会产生一个实例在类的内部当中.

####code[java]
```
// Implements a simple logging class using a singleton.
public class Logging {

    // this creates the actual Singleton instance
    private static final Logging singletonInstance = new Logging();

    /*
     * Private constructor prevents others from instantiating this class:
     */
    private Logging() {
    }

    // this method returns the singleton instance
    public static Logging getSingleton() {
        return singletonInstance;
    }

    /*
     * This will print a message to the screen: sample call:
     * Logging.getSingleton().log("testing message");
     */

    public void log(String message) {
        System.out.println(System.currentTimeMillis() + ": " + message);
    }
}

```

