# 命名
管理名称是最重要的一致性规则。命名风格即刻提醒着我们命名实体是什么种类的东西：类型、变量、函数、常量、宏等等，而不需要我们搜寻这个实体的声明。我们大脑中的模式匹配引擎很大程度上依赖于这种命名规则。

命名规则十分“武断”，但是我们认为在这个领域一致性比个人偏好重要得多，因此不管你是否觉得合理，规则就是规则。

## 一般命名规则
使用即使在不同团队中的人都看得懂的名称来优化可读性。

1. 使用描述对象意图的名称。不要担心节省屏幕的水平空间，因为让新读者立即理解你的代码要重要得多。
2. 尽量少用缩写——那些对于你项目之外的人不了解的缩写，尤其是首字母缩写。
3. 不要通过删除单词内的字母来缩写。
4. 根据经验，在维基百科上列出来的缩写一般是可以的。
5. 一般来说，描述性应该与名称的可见范围成正比。例如，n在5行函数中可能是一个好的变量名，但在类的范围内，它可能太模糊了。

        class MyClass {
        public:
            int CountFooErrors(const std::vector<Foo>& foos) {
                int n = 0; // 在有限的范围和上下文中，含义清晰
                for (const auto& foo : foos) {
                    ...
                    ++n;
                }
                return n;
            }
            void DoSomethingImportant() {
                std::string fqdn = ...; // 完全合格的域名的知名缩写
            }
        private:
            const int kMaxAllowedConnections = ...; //在上下文中有明确的含义
        };

        class MyClass {
        public:
            int CountFooErrors(const std::vector<Foo>& foos) {
                int total_number_of_foo_errors = 0; // 在有限的范围和上下文中过于冗余
                for (int foo_index = 0; foo_index < foos.size(); ++foo_index) { // 过于冗余，可使用惯用命名`i`
                    ...
                    ++total_number_of_foo_errors;
                }
                return total_number_of_foo_errors;
            }
            void DoSomethingImportant() {
                int cstmr_id = ...; // 删掉了单词内部的字母
            }
        private:
            const int kNum = ...; // 在大范围下，含义不明确
        };

需要注意特定的常见缩写是可以的，比如i作为迭代变量名、T作为模板参数名等等。

“word”是指内部没有空格的用英语写的任何单词。这包含了缩写，比如首字母缩写。

模板参数应该遵循对应类别的命名风格：类模板参数应该遵循类命名规则，非类模板参数应该遵循变量命名规则。

## 文件名
1. 文件名应该全小写且能包含下划线“`_`”和破折号“`-`”。
2. 遵循项目使用的约定。如果没有一致的本地模式可遵循，则首选“`_`”。

可接受的文件名例子：
- my_useful_class.cc
- my-useful-class.cc
- myusefulclass.cc
- myusefulclass_test.cc

cpp文件应该以`.cc`后缀结尾，头文件应该以`.h`后缀结尾。依赖于被文本包含在特定位置的文件应该以`.inc`后缀结尾（还请参阅自包含头文件一节）。

不要使用已经存在于`/usr/include`中的文件名，比如`db.h`。

总之，让你的文件名非常具体。比如，使用`http_server_logs.h`好过`logs.h`。一个非常常见的例子就是有一对文件命名为`foo_bar.h`和`foo_bar.cc`共同定义了`FooBar`类。

## 数据类型名
数据类型名以大写字母开始且对于每一个新的单词首字母大写，没有下划线`_`：`MyExcitingClass`,`MyExcitingEnum`。

所有数据类型名——类名、结构体、类型别名、联合体以及类模板参数，都是一样的约定。数据类型名以大写字母开始且对于每一个新的单词首字母大写，没有下划线`_`。比如：

        // classes and structs
        class UrlTable {...
        class UrlTableTester {...
        struct UrlTableProperties {...

        // typedefs
        typedef hash_map<UrlTableProperties *, std::string> PropertiesMap;

        // using aliases
        using PropertiesMap = hash_map<UrlTableProperties *, std::string>;

        // enums
        enum class UrlTableError {...

## 变量名
变量名（包括函数参数）和成员变量都是小写，单词间以下划线分隔。类数据成员（非结构体）还有尾随下划线。比如：`a_local_variable`，`a_struct_data_member`，`a_class_data_member_`。

### 常见变量名
例如：

        std::string table_name; // 可以，带下划线的小写字母
        std::string tableName;  // 不可以，混合形式

### 类数据成员
类数据成员，无论静态、非静态，的命名都类似于普通非成员变量，但是有尾随下划线`_`。

        class TableInfo {
            ...
        private:
            std::string table_name_;        // 可以，以下划线结尾
            static Pool<TableInfo>* pool_;  // 可以
        };

### 结构体数据成员
结构体数据成员，无论静态、非静态，的命名都类似于普通非成员变量。它们不需要尾随下划线`_`，但是类数据成员需要。

        struct UrlTableProperties {
            std::string name;
            int num_entries;
            static Pool<UrlTableProperties>* pool;
        };

## 常量名
声明为`constexpr`或者`const`以及在编程期间值为固定的变量