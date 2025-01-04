```cpp
class Node {
public:
    int key;
    int value;
    Node* prev;
    Node* next;
    Node(int k = 0, int v = 0) : key(k), value(v) {}
};
class LRUCache {

private:

    int capacity;
    //哨兵节点方便插入节点到头节点，同时也方便访问最后一个节点
    Node* dummy;
    //使用哈希表简化查找节点
    unordered_map<int,Node*> keyToNode;
public:
    LRUCache(int capacity) {
        this->capacity = capacity;
        dummy = new Node();
        //双向循环链表需要指向自己
        dummy->prev = dummy;
        dummy->next = dummy;
    }

    int get(int key) {
        auto iter = keyToNode.find(key);
        if(iter == keyToNode.end()){
            return -1;
        }
            //若存在，将其移到链表头
            auto node = iter->second;
            //删除原表位置
            node->prev->next = node->next;
            node->next->prev = node->prev;
            //放在链表头
            node->prev = dummy;
            node->next = dummy->next;
            dummy->next = node;
            node->next->prev = node;        
        return (*iter).second->value;

    }

    void put(int key, int value) {
        auto iter = keyToNode.find(key);
        //判断是否存在
        //若不存在,创建节点，放在链表头
        if(iter == keyToNode.end()){
            Node* node = new Node(key,value);
            keyToNode[key] = node;
            node->next = dummy->next;
            node->prev = dummy;
            dummy->next = node;
            node->next->prev = node;
            //判断size是否超出大小,超出则删除最后一个node
            if(keyToNode.size() > capacity){
                Node* backNode = dummy->prev;
                backNode->prev->next = dummy;
                dummy->prev = backNode->prev;
                keyToNode.erase(backNode->key);
                delete backNode;
            }
        }else{
            //若存在，将其移到链表头
            auto node = iter->second;
            node->value = value;
            //删除原表位置
            node->prev->next = node->next;
            node->next->prev = node->prev;
            //放在链表头
            node->prev = dummy;
            node->next = dummy->next;
            dummy->next = node;
            node->next->prev = node;
        }

    }

};
```