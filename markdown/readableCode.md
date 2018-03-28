# The Art of Readable Code

## 代码应该易于理解
- 现在你可能会想，谁会关心是不是有人能理解它？ 我是唯一使用这段代码的人！，就算你从事只有一个人的项目，这个目标也是值得的， 那个其他人可能就是6个月的你自己。（--o00）

## 将信息装到名字里
- 少用tmp, retval（除非这个变量的生命周期只有几行，临时变量）
- 索引的第一个字母应该与数据的第一个匹配（club[ci] + users[ui] + members[mi]）
- 不要因为一个名字含糊婉转而需要选择它
- 带单位的值（start_ms）
- 表示包含 min-max，first-last；表示包含／排除 begin-end
- is, has, can, should -> bool
- 标记 TODO(我还没有处理的事情),FIXME(已知的问题),HACK(对一个问题不得不采用比较粗糙的解决方案),XXX(危险！这里有重要的问题)

## 简化循环和逻辑
- 条件语句中的参数顺序（左侧倾向于不断变化，右侧常量 eg: lengtg>10）
- 条件语句中 先处理正逻辑，简单的情况。
- 只在简单情况下使用？：，可读性放在第一位，而不是行数
- 避免使用do／while循环
- 从函数中提前返回（if-else中通过提早返回减少嵌套， 循环中通过continue）

## 拆分超长的表达式
- 用作解释的变量（引入一个临时变量，表示小一点的子表达式）
- 德摩根定理 分别取反，转换与／或

## 变量与可读性
- 减少变量
- 减小每个变量的作用域
- 只写一次的变量

## 重新组织代码


## 一次只做一件事

## 少写代码
- 熟悉身边的库， 隔一段时间，花15分钟阅读标准库中的所有函数

## 设计并改进 （分钟／小时计数器）
```c++
class MinuteHourCounter {
  public:
    // add a new Data point( count >= 0).
    // for the next minute, MinuteCount() will be larger by +count
    // for the next hour, HourCount() will be larget by +count
    void Add(int count);

    //return the accumulated count over the past 60 seconds
    int MinuteCount();

    //return the accumulated count over the past 3600 seconds
    int HourCount();
};
```

// first
```c++
class MinuteHourCounter {
  struct Event {
    Event(int count, time_t time): count(count), time(time) {}
    int count;
    time_t time;
  };
  list<Event> events;
  public: 
    void Add(int count) {
      events.push_back(Event(count, time()));
    }
    int MinuteCount() {
      int count = 0;
      const time_t now_secs = time();
      for (list<Event>::reverse_iterator i = events.rbegin();
        i != events.rend() && i -> time > now_secs - 60; ++i) {
          count += i ->count;
        }
      return count;
    }
    int HourCount() {
      int count = 0;
      const time_t now_secs = time();
      for (list<Event>::reverse_iterator i = events.rbegin();
        i != events.rend() && i->time > now_secs - 3600; ++i) {
          count += i->count;
        }
      return count;
    }
}
```

// first版本 重复代码太多
// second
```c++
class MinuteHourCounter {
  list<Event> events;

  int CountSince(time_t cutoff) {
    int count = 0;
    for (list<Event>::reverse_iterator rit = events.rbegin();
      rit != events.rend(); ++rit) {
        if (rit->time <= cutoff) {
          break;
        }
        count += rit->count;
      }
  }

  public:
    void Add(int count) {
      event.push_back(Event(count, time()));
    }
    int MinuteCount() {
      return CountSince(time() - 60);
    }
    int HourCount() {
      return CountSince(time() - 3600);
    }
};
```

//second 保存着所有的值，虽然有些值已经没用了。速度太慢O(n)
