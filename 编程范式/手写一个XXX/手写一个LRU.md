```
#include <unordered_map>
#include <list>

class LRUCache {
public:
    LRUCache(int capacity) : cap(capacity) {}

    int get(int key) {
        auto it = cache.find(key);
        if (it == cache.end()) return -1;
        touch(it);
        return it->second.first;
    }

    void put(int key, int value) {
        auto it = cache.find(key);
        if (it != cache.end()) touch(it);
        else {
            if (cache.size() == cap) {
                cache.erase(used.front());
                used.pop_front();
            }
            used.push_back(key);
        }
        cache[key] = {value, used.end()--};
    }

private:
    typedef std::pair<int, std::list<int>::iterator> pair_t;
    typedef std::unordered_map<int, pair_t> cache_t;

    void touch(cache_t::iterator it) {
        int key = it->first;
        used.erase(it->second.second);
        used.push_back(key);
        it->second.second = used.end()--;
    }

    cache_t cache;
    std::list<int> used;
    int cap;
};

```

对于list来说，只有你直接操作的那个元素的迭代器会失效，其它的迭代器并不会失效。这是因为list的元素是存储在非连续的内存空间中，插入和删除元素时，不会影响到其它元素。
注意，这个行为和vector是不同的。对于vector，如果在中间插入或删除元素，那么指向插入或删除位置以后的所有元素的迭代器都会失效。因为vector的元素是存储在连续的内存空间
中，插入和删除元素会导致其它元素的位置发生改变。
