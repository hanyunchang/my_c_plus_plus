```
#include <atomic>

template<typename T>
class share_pointer{
public:
    share_pointer(T* data) {
        _data = data;
        _count = new atomic<int>(1);
    }

    share_pointer(const share_pointer& other) {
        _data = other._data;
        _count = other._count;
        _count->fetch_add(1);
    }
    
    ~share_pointer() {
        _count->fetch_sub(1);
        if(_count->load(memory_order_acquire)) {
            delete _data;
            _data = nullptr;
            delete _count;
        }
    }

    share_pointer& operator=(const share_pointer& other) {
        if(&other != this) { //&other代表other的地址, 判断引用的对象和指针指向的地址是否相等, 可以这样做
            return *this;
        }

        if(_count->load(memory_order_acquire) == 1) { //自己之前的引用计数如果自减后为0，那么需要释放资源
            delete _data;
            delete _count;
        }

        _data = other._data;
        _count = other._count;
        _count->fetch_add(1); 

        return *this;
    }

    T* operator->() {
        return _data;
    }

    T& operator*() {
        return *_data;
    }
private:
    T* _data = nullptr;
    atomic<int>* _count = 0;
};
```
