<!--
 * @Author: yangshaoqing yangshaoqing@haomo.ai
 * @Date: 2023-04-05 17:51:01
 * @LastEditors: yangshaoqing yangshaoqing@haomo.ai
 * @LastEditTime: 2023-04-05 18:18:37
 * @FilePath: /cpp_tech_stack/shared_ptr.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# 三种智能指针
## std::unique_ptr
离开unique_ptr指针的作用域时自动释放内存，不需要手动释放  
```
// 使用裸指针需要手动释放内存
{
    int* p = new int(100);
    
    delete p; // 需要记得内存释放
}

{
    std::unique_ptr<int> p = std::make_unique<int>(100);

    // 离开p的作用域自动释放

    std::unique_ptr<int> p_tmp = std::move(p);
    // unique_ptr只可以move，不可以拷贝
}

```


## std::shared_ptr
与std::unique_ptr不同的是，会对指针的引用个数进行计数，当计数为0时才会自动释放。用于多线程同一块内存占用  
```
{
    std::shared_ptr<int> sptr = std::make_shared<int>(200);
    assert(sptr.use_count() == 1);  // 此时引用计数为 1
    {   
        std::shared_ptr<int> sptr1 = sptr;
        assert(sptr.get() == sptr1.get());
        assert(sptr.use_count() == 2);   // sptr和sptr1共享资源，引用计数为 2
    }   
    assert(sptr.use_count() == 1);   // sptr1 已经释放
}
// use_count 为 0 时自动释放内存
```


## std::weak_ptr
std::weak_ptr 要与 std::shared_ptr 一起使用。 一个 std::weak_ptr 对象看做是 std::shared_ptr 对象管理的资源的观察者，它不影响共享资源的生命周期  