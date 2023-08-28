```
class Config {
    // ... 其他配置信息
public:
    Config() {
        // 从文件中加载配置
    }

    void reload() {
        // 重新从文件中加载配置
    }

    // ... 其他配置相关的操作
};

#include <atomic>
#include <memory>

template<typename T>
class DoubleBuffer {
private:
    std::atomic<std::shared_ptr<T>> readBuffer;
    std::atomic<std::shared_ptr<T>> writeBuffer;

public:
    DoubleBuffer() {
        readBuffer = std::make_shared<T>();
        writeBuffer = std::make_shared<T>();
    }

    std::shared_ptr<T> getReadBuffer() {
        return readBuffer.load(std::memory_order_acquire);
    }

    std::shared_ptr<T> getWriteBuffer() {
        return writeBuffer.load(std::memory_order_acquire);
    }

    void swapBuffers() {
        auto temp = readBuffer.load(std::memory_order_acquire);//极端情况下，reload时可能还有线程在读这个config，需要保证其可以读到过时数据但是不能读到脏数据
        readBuffer.store(writeBuffer.exchange(temp, std::memory_order_acq_rel), std::memory_order_release);
    }
};


DoubleBuffer<Config> configBuffers;

void updateConfig() {
    // 更新配置
    configBuffers.getWriteBuffer()->reload();
    // 切换buffer
    configBuffers.swapBuffers();
}

void readConfig() {
    // 读取配置
    auto config = configBuffers.getReadBuffer();
    // 使用config
}
```
