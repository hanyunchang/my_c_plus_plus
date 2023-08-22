```
#include <list>
#include <unordered_map>

using namespace std;

class LRU{
public:
    LRU(int capacity) : _capacity(capacity) { }

    bool get(string& key, string* value) {
        if(_hash.count(key)) {
            auto it = _hash[key];
            *value = it->second;
            _lru.splice(_lru.begin(), _lru, it);
            return true;
        }
        return false;
    }

    void put(string& key, string& value) {
        if(_hash.count(key)) {
            auto it = _hash[key]; 
            it->second = value;
            _lru.splice(_lru.begin(), _lru, it); 
            return ;
        }
        
        if(_lru.size() == _capacity) {
            auto last_key = _lru.back().second;
            _hash.erase(last_key);
            _lru.pop_back();
        }
        _lru.emplace_front(key, value);
        _hash[key] = _lru.begin();
    }
private:
    list<pair<string, string>> _lru;
    unordered_map<string, list<pair<string, string>>::iterator> _hash;
    int _capacity;
};
```


