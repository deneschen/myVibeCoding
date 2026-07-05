# SEI CERT C++ 安全编码聚合规范

版本：2026-07-05

来源目录：`secure-coding-standards/content/5.sei-cert-cpp-coding-standard`

适用范围：本文件只聚合 SEI CERT C++ Coding Standard 中与 C++ 直接相关的高质量规范内容，用于 C++ 安全设计、代码评审、静态分析配置、缺陷整改和 AI 生成代码约束。本文不是源站逐字镜像，不复制长篇示例、完整 analyzer 表格、附件或站点管理内容。

本仓库车载嵌入式 Linux C++ 产品代码的强制主规范仍是
[vehicle-embedded-linux-cpp-coding-standard.md](../vehicle-embedded-linux-cpp-coding-standard.md)。
本文是 CERT C++ 来源聚合参考，用于补充来源覆盖和安全规则族。

## 1. 使用定位

### CERT-CPP-AGG-001 来源和语言基线

CERT C++ 来源以 C++14 为主要基线，也可指导 C++11 旧代码整改。本文按 C++14 安全子集表达规则意图，并与本仓库默认 C++14、禁用异常作为错误处理、禁用 RTTI、禁止裸资源管理的项目策略对齐。

### CERT-CPP-AGG-002 与 CERT C 的关系

CERT C++ 标准引用并依赖 CERT C 的大量安全原则。C++ 代码涉及 C 库、POSIX、信号、C 字符串、C API 或 ABI 边界时，还应参考
[sei-cert-c-secure-coding-aggregate.md](./sei-cert-c-secure-coding-aggregate.md)。

### CERT-CPP-AGG-003 规则和偏离

CERT C++ 当前源目录只暴露 rules，不暴露独立 recommendations。违反规则必须修复或形成批准偏离。偏离必须说明不会引入安全、可靠性或可移植性缺陷。

偏离记录模板：

```text
Deviation ID:
CERT C++ family or rule:
File or module:
Reason:
Security and safety risk:
Alternatives considered:
Mitigation:
Verification:
Owner:
Approval:
Review date:
```

## 2. 总体 C++ 安全原则

### CERT-CPP-GEN-001 使用受限 C++14

产品 C++ 代码默认使用受限 C++14 子集。新语言特性、第三方库、编译选项和 ABI 变化必须经模块批准。避免依赖编译器扩展、未定义行为、对象布局假设和实现定义行为。

### CERT-CPP-GEN-002 RAII 优先

内存、文件、socket、锁、线程、临时文件、映射内存、设备句柄、事务和权限提升状态必须由 RAII 类型或专用 owner 管理。禁止手写多出口清理逻辑。

### CERT-CPP-GEN-003 接口表达契约

接口必须明确所有权、生命周期、可空性、错误语义、线程安全、阻塞行为、异常边界和时间单位。裸指针不得表示所有权。

### CERT-CPP-GEN-004 安全边界显式化

跨进程、网络、文件、C ABI、插件、回调、线程和权限边界的数据必须验证、复制或封装。不得让内部对象布局、未初始化字节或敏感字段越过信任边界。

## 3. 声明和初始化

### CERT-CPP-DCL-001 避免危险声明

不得定义 C 风格可变参数函数、保留标识符、语法歧义声明、头文件匿名命名空间、标准命名空间修改或 ODR 违反。公共头文件必须自包含并避免全局副作用。

### CERT-CPP-DCL-002 初始化顺序必须可控

对象必须完整初始化。构造函数成员初始化顺序必须与声明顺序一致。避免静态对象初始化循环；安全相关初始化不得依赖跨翻译单元顺序。

### CERT-CPP-DCL-003 析构和释放函数不得抛出

析构函数、释放函数和 RAII 清理路径不得抛出异常。失败必须通过显式 `close()`、状态返回、受控日志或上层生命周期协议处理。

### CERT-CPP-DCL-004 信任边界防泄漏

类对象跨信任边界前必须避免暴露填充字节、未初始化成员、内部指针、敏感状态和对象布局细节。应转换为明确的序列化结构。

## 4. 表达式、生命周期和类型

### CERT-CPP-EXP-001 不依赖求值顺序和隐藏副作用

表达式不得依赖未指定求值顺序、副作用交错或 unevaluated operand 中的副作用。复杂表达式应拆解为清晰步骤。

### CERT-CPP-EXP-002 生命周期必须有效

不得访问生命周期外对象、悬空引用、悬空指针、失效迭代器或被移动后语义未定义的对象。lambda 不得让引用捕获对象超出生命周期。

### CERT-CPP-EXP-003 类型和 ABI 边界必须匹配

不得用错误 linkage 调用函数，不得跨执行边界传递非标准布局对象，不得对不完整类型错误 cast 或 delete，不得错误使用 `offsetof`、`va_start` 或对象表示位。

### CERT-CPP-INT-001 枚举和整数必须范围安全

禁止把整数转换为枚举范围外值。整数到枚举、符号转换、窄化转换和协议字段转换必须检查范围。新增枚举优先使用 `enum class`。

## 5. 容器、字符串和迭代器

### CERT-CPP-CTR-001 容器访问必须在有效范围内

容器索引、迭代器范围、指针偏移和库函数输入必须经过范围验证。不得用溢出的迭代器运算或不属于同一容器的迭代器差值。

### CERT-CPP-CTR-002 引用、指针和迭代器失效必须可见

修改容器、字符串或分配器后，必须重新评估引用、指针和迭代器有效性。接口不得长期保存不受所有权保护的内部元素引用。

### CERT-CPP-CTR-003 排序谓词必须有效

排序、关联容器和查找 predicate 必须满足严格弱序，不得依赖可变谓词对象、隐藏状态或不稳定比较。

### CERT-CPP-STR-001 字符串构造和访问必须安全

不得从空指针构造 `std::string`。字符串存储必须容纳字符和终止符。`basic_string` 元素引用和范围访问必须有效；需要范围检查时使用显式检查或安全 API。

## 6. 内存管理

### CERT-CPP-MEM-001 不访问已释放资源

释放后不得访问对象、数组、容器元素、文件对象或平台句柄。所有权转移后原 owner 必须进入明确、可测试状态。

### CERT-CPP-MEM-002 动态资源必须正确释放

动态分配资源必须由 `std::unique_ptr`、容器、RAII wrapper 或项目 owner 类型管理。禁止裸 `new/delete` 出现在业务逻辑。

### CERT-CPP-MEM-003 分配失败必须处理

即使项目禁用异常，分配、容器增长和资源创建失败仍必须有错误返回、容量预留、降级或初始化期失败策略。

### CERT-CPP-MEM-004 手动生命周期必须隔离

placement new、显式析构、过对齐类型、自定义 allocator、替换 `operator new/delete` 和未初始化存储必须只在底层封装中使用，并有边界测试。

### CERT-CPP-MEM-005 智能指针不得掩盖所有权错误

不得把已被其他 owner 持有的裸指针放进无关智能指针。`shared_ptr` 必须有共享所有权设计理由，避免循环引用和跨线程生命周期不清。

## 7. 输入输出和 C/POSIX 边界

### CERT-CPP-FIO-001 文件流必须正确定位和关闭

交替读写文件流必须按标准要求进行 flush、seek 或定位。文件对象、fd 和 stream 必须及时关闭并处理失败。

### CERT-CPP-FIO-002 C++ I/O 错误不得丢失

流状态、异常配置、短 I/O、编码转换和关闭失败必须转换为项目错误域。不得只依赖析构隐式关闭来表达业务成功。

### CERT-CPP-FIO-003 C API 必须封装

C 标准库、POSIX、系统调用、信号和 ABI 边界必须通过 RAII wrapper 或小型 adapter 暴露给 C++ 业务逻辑。错误码、`errno`、句柄生命周期和线程语义必须集中处理。

## 8. 异常和错误处理

### CERT-CPP-ERR-001 项目默认不以异常作为错误处理

CERT C++ 覆盖异常安全、异常边界和异常对象规则。本仓库车载 C++ 产品代码更严格：默认禁止抛出异常、跨模块传播异常或依赖异常恢复。第三方库异常必须在边界捕获并转换为显式错误。

### CERT-CPP-ERR-002 不得突然终止程序

业务错误不得通过 `abort`、`terminate`、未处理异常或进程退出隐藏。安全关键服务必须返回错误、上报诊断或进入受控降级。

### CERT-CPP-ERR-003 禁止 `setjmp` 和 `longjmp`

不得使用 `setjmp`、`longjmp` 或等价非局部跳转绕过 C++ 对象析构和 RAII 清理。

### CERT-CPP-ERR-004 字符串转数值必须检测失败

字符串到整数、浮点、枚举或配置值的转换必须检测范围、尾随字符、空输入、编码和转换失败。

## 9. 面向对象规则

### CERT-CPP-OOP-001 构造和析构期间不调用虚函数

构造和析构期间不得调用依赖派生类行为的虚函数。初始化应通过非虚私有函数、工厂或显式启动步骤完成。

### CERT-CPP-OOP-002 多态基类析构必须正确

通过基类指针删除派生对象时，基类必须有虚析构函数。否则应禁止通过基类删除，或使用 protected non-virtual 析构表达接口约束。

### CERT-CPP-OOP-003 防止对象切片

不应按值传递或存储多态对象。接口使用引用、指针、owner 类型或值语义明确的非多态类型。

### CERT-CPP-OOP-004 特殊成员函数必须语义一致

复制、移动、赋值、析构和比较操作必须保持对象不变量。自赋值不得破坏对象；复制操作不得修改源对象。

### CERT-CPP-OOP-005 重载和特殊函数优先类型安全

值类型可以使用语义自然的运算符重载。不得用 C 标准库函数绕过 C++ 类型系统、构造/析构规则或对象不变量。

## 10. 并发

### CERT-CPP-CON-001 mutex 生命周期必须有效

不得销毁已锁定 mutex，不得在异常、早返回或错误路径泄漏锁。锁必须由 RAII guard 管理。

### CERT-CPP-CON-002 共享数据必须避免数据竞争

共享对象、bit-field、容器、字符串和状态变量必须通过 mutex、atomic、消息队列或单线程执行器保护。`volatile` 不是同步机制。

### CERT-CPP-CON-003 锁顺序必须固定

多个锁必须有全局或模块级顺序，避免死锁。不得 speculative lock 已由当前线程拥有的 non-recursive mutex。

### CERT-CPP-CON-004 条件变量必须带谓词循环

等待条件变量必须用谓词循环处理虚假唤醒。等待必须有取消、超时或停止协议；不得用裸 `sleep` 代替同步。

### CERT-CPP-CON-005 线程安全和活性同等重要

并发代码必须同时证明不会数据竞争、不会死锁、不会无限等待、不会销毁仍被线程访问的对象。

## 11. Misc、安全随机和信号

### CERT-CPP-MSC-001 不使用 `std::rand`

不得使用 `std::rand()` 或弱随机数生成安全材料。安全用途必须使用项目批准的密码学随机源。普通伪随机数也必须正确播种并说明用途。

### CERT-CPP-MSC-002 `[[noreturn]]` 和返回路径必须一致

值返回函数所有路径必须返回值。声明为 `[[noreturn]]` 的函数不得返回。

### CERT-CPP-MSC-003 信号处理函数必须简单

C++ 信号 handler 必须是普通函数，并遵守 C/POSIX 异步信号安全要求。复杂处理转发到事件循环或专用安全机制。

## 12. C++ 代码评审清单

- 接口是否表达所有权、生命周期、线程安全、错误、阻塞行为和时间单位。
- 是否存在裸 `new/delete`、手动多出口清理、悬空引用或失效迭代器。
- 是否存在异常传播、`setjmp/longjmp`、RTTI 依赖或跨 ABI 对象泄漏。
- 是否存在未初始化读取、错误求值顺序、错误 cast、错误 linkage 或枚举越界。
- 容器和字符串访问是否有边界，predicate 是否满足严格弱序。
- 多态类型是否有正确析构策略，是否避免构造/析构虚调用和对象切片。
- 并发代码是否使用 RAII 锁、固定锁顺序、条件变量谓词循环和明确停止协议。
- C/POSIX/API 边界是否封装错误码、资源生命周期、权限和信任边界。
- 是否通过格式化、编译、单元测试、静态分析和相关动态分析。

## 13. AI 生成 C++ 代码规则

AI 生成或修改 C++ 代码时不得输出：

- 裸 `new/delete`、裸 owning pointer、未封装 fd/socket/线程/锁。
- 异常作为常规错误处理、跨模块异常传播、`setjmp/longjmp`。
- RTTI 依赖、默认捕获 lambda、悬空引用捕获。
- 无边界容器增长、无界等待、无界重试或无容量策略缓存。
- 未检查返回值、吞错、泄密日志或只返回 `bool` 的复杂失败。
- 直接散落 POSIX/C API 调用而没有 RAII wrapper。

AI 必须同步说明相关 CERT C++ 规则族、修改范围、测试方式和未能验证的检查。普通 C++ 编码任务优先读取
[ai-vibe-coding-cpp-check-rules.md](../ai-vibe-coding-cpp-check-rules.md)
和主 C++ 规范；只有更新标准、解决规则冲突或做来源覆盖审计时，才需要读取本文件和来源追踪文件。

## 14. 来源覆盖审计

### 14.1 文件数量

| 来源分组 | 文件数 | 本文覆盖 |
| :--- | ---: | :--- |
| 顶层索引和 errata | 2 | 来源定位、版本和维护说明。 |
| `2.front-matter` | 16 | C++14 范围、适用性、风格边界和标准组织方式。 |
| `3.rules` | 95 | 第 3 至 11 节的 C++ 规则族。 |
| `4.back-matter` | 55 | 分析器、风险评估、术语、相关指南和参考文献。 |
| `5.admin` | 2 | 管理和 TODO 含义，不作为产品代码规则。 |
| 合计 | 170 | 全部纳入本文件覆盖范围。 |

### 14.2 规则族数量

| CERT C++ 规则族 | Rules 文件数 |
| :--- | ---: |
| Rules root index | 1 |
| Declarations and Initialization (DCL) | 12 |
| Expressions (EXP) | 15 |
| Integers (INT) | 2 |
| Containers (CTR) | 10 |
| Characters and Strings (STR) | 5 |
| Memory Management (MEM) | 9 |
| Input Output (FIO) | 3 |
| Exceptions and Error Handling (ERR) | 14 |
| Object-Oriented Programming (OOP) | 10 |
| Concurrency (CON) | 8 |
| Miscellaneous (MSC) | 6 |

### 14.3 排除项

以下内容已审计但不进入规范主体：图片附件、站点导航样式、Confluence 页面装饰、完整 analyzer 明细表、源站 TODO 管理列表和源站构建工程。删除 `secure-coding-standards` 前，如果不需要逐条原始示例和完整工具表，本文件已经覆盖 C++ 相关规范意图。

## 15. 维护规则

- 更新本文时必须重新统计 `5.sei-cert-cpp-coding-standard` 下的 Markdown 数量。
- 不复制长篇源规则、示例和 analyzer 表格。
- 新增 C++ 项目规则时，应同步考虑主 C++ 规范是否需要更新。
- C 语言/POSIX 底层规则优先写入 C 聚合文档；项目级 C++ 产品规则优先写入主 C++ 规范。
