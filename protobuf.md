<!--
 * @Author: yangshaoqing yangshaoqing@haomo.ai
 * @Date: 2023-04-18 20:16:57
 * @LastEditors: yangshaoqing yangshaoqing@haomo.ai
 * @LastEditTime: 2023-04-19 14:43:14
 * @FilePath: /cpp_tech_stack/protobuf.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# Protobuf使用介绍
## protobuf的数据格式
``` 
message Example1 {
    // 字段规则 类型 名字 = 字段编号;
    optional string stringVal = 1;
    optional bytes bytesVal = 2;
    message EmbeddedMessage {
        int32 int32Val = 1;
        string stringVal = 2;
    }
    optional EmbeddedMessage embeddedExample1 = 3;
    repeated int32 repeatedInt32Val = 4;
    repeated string repeatedStringVal = 5;
}
```
### 字段类型
1. required: 必须提供该字段的值，否则该消息将被视为“未初始化”
2. optional: 可以设置也可以不设置该字段。如果未设置可选字段值，则使用默认值。对于简单类型，你可以指定自己的默认值，就像我们在示例中为电话号码类型所做的那样。否则，使用系统默认值：数字类型为 0，字符串为空字符串，bools 为 false
3. repeated: 该字段可以重复任意次数（包括零次），重复顺序会被保留，可以将 repeated 字段视为动态大小的数组
4. enum: 具有枚举类型的字段只能将一组指定的常量作为其值
5. oneof: 可选字段，除了 oneof 共享内存中的所有字段，并且最多只能同时设置一个字段。设置 oneof 的任何成员会自动清除所有其他成员

## proto使用接口
```
 // required name
  inline bool has_name() const;
  inline void clear_name();
  inline const ::std::string& name() const;
  inline void set_name(const ::std::string& value);
  inline void set_name(const char* value);
  inline ::std::string* mutable_name();

  // required id
  inline bool has_id() const;
  inline void clear_id();
  inline int32_t id() const;
  inline void set_id(int32_t value);

  // optional email
  inline bool has_email() const;
  inline void clear_email();
  inline const ::std::string& email() const;
  inline void set_email(const ::std::string& value);
  inline void set_email(const char* value);
  inline ::std::string* mutable_email();

  // repeated phones
  inline int phones_size() const;
  inline void clear_phones();
  inline const ::google::protobuf::RepeatedPtrField< ::tutorial::Person_PhoneNumber >& phones() const;
  inline ::google::protobuf::RepeatedPtrField< ::tutorial::Person_PhoneNumber >* mutable_phones();
  inline const ::tutorial::Person_PhoneNumber& phones(int index) const;
  inline ::tutorial::Person_PhoneNumber* mutable_phones(int index);
  inline ::tutorial::Person_PhoneNumber* add_phones();
```
