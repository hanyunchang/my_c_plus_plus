C++标准库中的 std::this_thread 命名空间包含以下函数：

std::this_thread::get_id: 返回当前线程的线程ID。

std::this_thread::yield: 提醒调度器当前线程愿意放弃其对处理器的使用，允许其他线程运行。

std::this_thread::sleep_for: 使当前线程阻塞（即处于睡眠状态）指定的时间段。

std::this_thread::sleep_until: 使当前线程阻塞（即处于睡眠状态）直到指定的时间点。

std::this_thread::hardware_concurrency: 返回能并发执行的线程数量的提示。如果无法计算或得出结果，则返回0。

以上函数都定义在 <thread> 头文件中。