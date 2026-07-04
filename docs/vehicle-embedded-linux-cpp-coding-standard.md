# 车载嵌入式 Linux C++ 开发规范

版本：2026-07-04

适用范围：车载嵌入式 Linux 平台上的 C++ 产品代码，包括 Adaptive
Application、中间件、系统服务、诊断、通信、存储、升级、日志、健康监控、
硬件抽象层适配代码，以及相关测试代码。

参考来源见
[vehicle-embedded-linux-cpp-standards-sources.md](./references/vehicle-embedded-linux-cpp-standards-sources.md)。
来源覆盖矩阵、离线资料索引和 Obsidian 图谱见
[vehicle-embedded-linux-cpp-standards-traceability.md](./references/vehicle-embedded-linux-cpp-standards-traceability.md)。
AI vibe coding 的紧凑检查规则和 Token 节省策略见
[ai-vibe-coding-cpp-check-rules.md](./ai-vibe-coding-cpp-check-rules.md)。
本文档以 AUTOSAR C++14、MISRA C++、SEI CERT C++、C++ Core Guidelines 和
Google C++ Style Guide 的公开信息和工程意图为基础，形成项目内部可执行规则。
不要把本文档视为 AUTOSAR 或 MISRA 原文替代品；安全审计和客户准入仍以授权标准
原文、静态分析报告和偏离记录为准。

## 1. 核心原则

### CPP-GEN-001 产品代码优先

所有 C++ 代码必须按产品级代码生成和评审，而不是 demo、脚本或实验代码。代码必须
可测试、可诊断、可维护、可静态分析，并能在资源受限和长期运行的车载环境中稳定运行。

### CPP-GEN-002 安全子集优先

默认使用受限 C++14 子集。只有当目标工具链、静态分析器、代码评审和运行时约束都
支持时，才允许按模块批准使用 C++17 特性。禁止 AI 自动引入新语言版本、第三方库、
运行时机制或构建选项。

### CPP-GEN-003 确定性优先

安全相关、实时相关、启动链路、健康监控、通信链路和持久化链路必须避免不可预测的
资源使用、隐式阻塞、无界等待、隐藏异常传播和未受控动态分配。

### CPP-GEN-004 可证明优先

每项规则偏离都必须有记录。记录必须说明偏离规则、原因、风险、替代方案、验证方式、
责任人和有效范围。AI 生成代码不得自行创建偏离；偏离必须由人类 reviewer 批准。

## 2. 规范优先级

规则冲突时按以下顺序执行：

1. 法规、客户要求、功能安全、信息安全、项目安全计划。
2. AUTOSAR C++14、MISRA C++、SEI CERT C++ 的适用要求。
3. 本项目规范。
4. 模块级设计文档和接口规范。
5. 通用 C++ Core Guidelines、Google C++ Style Guide。
6. 局部既有代码风格。

如果既有代码违反本规范，新增代码不得扩大问题。修复既有问题应单独提交，除非它是
当前修改的必要前置条件。

## 3. 合规语言

- `必须`：无条件执行；违反即为缺陷。
- `禁止`：无条件不允许；如需例外必须走偏离流程。
- `应当`：默认执行；如果不执行必须在评审中说明原因。
- `可以`：允许但不鼓励扩散；必须保持局部、可测试、可维护。

## 4. 构建和工具链基线

### CPP-BUILD-001 语言标准

默认编译标准为 `-std=c++14`。如果项目批准 C++17，必须在构建系统、静态分析配置、
CI 和本文档扩展表中明确允许的特性列表。

### CPP-BUILD-002 编译告警

产品代码必须在 CI 中启用严格告警，并将项目认可的告警作为错误处理。推荐基线：

```text
-Wall -Wextra -Werror -Wconversion -Wsign-conversion -Wshadow
-Wnon-virtual-dtor -Wold-style-cast -Woverloaded-virtual
-Wnull-dereference -Wdouble-promotion -Wformat=2
```

具体选项可按编译器裁剪，但不得通过关闭告警掩盖缺陷。新增抑制必须写明原因和范围。

### CPP-BUILD-003 静态分析

合入前必须至少通过项目配置的 `clang-tidy`、`cppcheck` 或商业静态分析工具。安全
相关模块应映射到 AUTOSAR/MISRA/CERT 检查集。工具误报必须以局部抑制加说明处理，
禁止全局关闭规则。

### CPP-BUILD-004 Sanitizer

主机侧测试应定期运行 ASan、UBSan、TSan。目标环境无法运行 sanitizer 时，必须保留
主机等价测试或集成测试证据。

## 5. 架构和模块边界

### CPP-ARCH-001 分层依赖

推荐依赖方向：

```text
application -> domain service -> platform abstraction -> Linux/POSIX/driver
```

上层不得直接散落调用 `open`、`ioctl`、`pthread_*`、`socket`、`systemd`、D-Bus、
SOME/IP、DDS 等平台接口。平台能力必须通过可测试 wrapper 或 adapter 暴露。

### CPP-ARCH-002 接口先于实现

公共接口必须表达所有权、生命周期、线程安全、阻塞行为、错误语义和时间单位。接口
不清楚时，AI 不得猜测实现；必须先补充接口约束或测试。

### CPP-ARCH-003 小模块

单个类应只有一个主要职责。新增类通常不应同时处理协议解析、状态机、线程管理、I/O
和日志。超过约 300 行的源文件或超过约 150 行的类应在评审中说明为什么仍然清晰。

### CPP-ARCH-004 依赖注入

涉及时间、文件系统、网络、硬件、随机数、进程环境的代码必须通过接口注入或 wrapper
隔离，确保单元测试可替换。禁止在业务逻辑中直接读取全局环境作为隐藏输入。

## 6. 允许、限制和禁止的 C++ 特性

### CPP-FEAT-001 默认允许

在遵守本规范的前提下，以下特性默认允许：

- RAII。
- `enum class`。
- `constexpr` 简单常量和纯函数。
- `noexcept`，用于不会失败或失败不通过异常表达的函数。
- `std::array`、`std::vector`、`std::string`，但实时路径必须控制容量。
- `std::unique_ptr` 表达唯一所有权。
- `std::lock_guard`、`std::unique_lock`、`std::atomic`。
- 范围 `for`，前提是不产生隐藏拷贝。
- `auto`，仅当类型在右侧清晰可见或能提升安全性时使用。

### CPP-FEAT-002 默认限制

以下特性必须谨慎使用，并在代码评审中能解释必要性：

- 模板：只用于类型安全、零成本抽象或容器适配；禁止模板元编程炫技。
- Lambda：只用于局部回调；捕获必须显式，禁止默认捕获 `[&]` 和 `[=]`。
- 继承：优先组合；允许纯接口和稳定多态边界。
- 虚函数：允许用于明确接口；析构函数必须为 `virtual` 或 `protected non-virtual`。
- `std::shared_ptr`：必须说明共享所有权原因；禁止用它掩盖生命周期设计问题。
- 运算符重载：仅允许值类型中语义自然、无副作用的重载。
- 类型擦除、回调链、观察者模式：必须有生命周期和线程安全说明。

### CPP-FEAT-003 默认禁止

产品代码默认禁止：

- 裸 `new`、裸 `delete`、`malloc`、`free` 出现在业务逻辑中。
- C 风格 cast：`(T)x`。
- `reinterpret_cast`，除非硬件寄存器、协议字节流或 ABI 边界已批准。
- `const_cast` 修改原本为 `const` 的对象。
- `dynamic_cast` 和 RTTI 依赖。
- 异常作为错误处理机制。
- `goto`、`setjmp`、`longjmp`。
- 可变参数 C 函数封装新接口。
- 非局部宏生成代码。
- 全局可变状态。
- `using namespace` 出现在头文件或全局作用域。
- `system`、`popen`、shell 拼接命令。
- 未封装的信号处理、线程创建、文件描述符管理和 socket 生命周期。

## 7. 所有权、生命周期和内存

### CPP-MEM-001 所有权必须显式

函数接口必须明确对象所有权：

- 非拥有只读引用：`const T&`。
- 非拥有可修改引用：`T&`。
- 可为空非拥有指针：`T*`，必须说明可为空条件。
- 唯一所有权转移：`std::unique_ptr<T>`。
- 共享所有权：`std::shared_ptr<T>`，必须有设计说明。

禁止用裸指针表达所有权。

### CPP-MEM-002 RAII 管理资源

文件描述符、socket、mutex、线程、mmap、timer、epoll fd、设备句柄、临时文件和锁
必须由 RAII 类型管理。禁止在多出口函数中手写成对清理逻辑。

### CPP-MEM-003 动态分配边界

启动初始化阶段可以按配置进行动态分配。进入实时路径、周期任务、通信热路径或安全监控
循环后，禁止无界动态分配。容器必须预留容量或使用固定容量结构。

### CPP-MEM-004 生命周期不得隐藏

禁止返回悬空引用、悬空指针、指向局部变量的 `string_view` 或 lambda 捕获局部引用
后跨作用域使用。异步任务不得捕获 `this`，除非对象生命周期由任务调度器明确持有。

### CPP-MEM-005 初始化必须完整

所有变量必须在定义处初始化。类成员必须使用成员初始化列表。禁止依赖默认未初始化内存。

## 8. 错误处理

### CPP-ERR-001 默认不使用异常

项目产品代码默认禁止抛出异常、跨模块传播异常或依赖异常恢复。第三方库可能抛异常时，
必须在边界捕获并转换为项目错误类型。构建若禁用异常，则依赖库也必须匹配。

### CPP-ERR-002 使用显式错误类型

可失败函数必须返回 `Result<T, Error>`、`Status`、错误码枚举，或项目已有等价类型。
禁止用 `bool` 表达多种失败原因。错误类型必须能映射到日志、诊断和测试断言。

### CPP-ERR-003 不吞错

禁止忽略返回值。确实可忽略时必须写成显式形式，例如 `static_cast<void>(...)` 并附简短
说明。析构函数不得抛出异常，失败只能记录受控日志或通过显式 `close()` 暴露。

### CPP-ERR-004 Linux 错误边界

POSIX 调用必须处理 `errno`、`EINTR`、短读短写、非阻塞返回、权限错误和资源耗尽。
wrapper 必须把平台错误转换为项目错误域，避免上层散落解析 `errno`。

## 9. 并发和实时行为

### CPP-CON-001 线程生命周期必须可控

禁止 detached thread。每个线程必须有所有者、名称、启动点、停止协议、join 路径和故障
上报路径。后台线程不得在析构期间静默遗留。

### CPP-CON-002 禁止数据竞争

共享可变数据必须用 mutex、atomic、消息队列或单线程执行器保护。`volatile` 不是线程
同步机制。atomic 必须说明内存序；默认使用顺序一致，除非有充分理由优化。

### CPP-CON-003 锁必须简单

禁止在持锁期间执行阻塞 I/O、回调外部代码、调用未知耗时函数或写持久化存储。多个锁
必须有固定顺序。禁止递归 mutex，除非旧接口迁移期有偏离记录。

### CPP-CON-004 等待必须有边界

等待条件必须有超时、取消或健康监控路径。禁止用固定 `sleep` 轮询作为同步机制。测试
也不得依赖脆弱 sleep。

### CPP-CON-005 回调线程语义必须明确

注册回调的接口必须说明回调在哪个线程执行、是否可重入、是否允许阻塞、是否允许调用
回注册方。AI 生成回调代码时必须先检查这些约束。

## 10. 时间和调度

### CPP-TIME-001 使用单调时钟

超时、周期任务、deadline、重试退避必须使用单调时钟，例如 `std::chrono::steady_clock`
或平台等价接口。禁止使用系统墙钟计算超时。

### CPP-TIME-002 时间单位必须显式

接口使用 `std::chrono` 类型表达时间。禁止裸 `int timeout`、`uint32_t ms` 之类模糊参数，
除非协议字段本身如此定义，并且名称带单位。

### CPP-TIME-003 周期任务必须抗漂移

周期任务应基于下一次计划时间调度，而不是循环末尾简单 sleep 固定时长。超时恢复必须
避免无限补偿或持续抢占系统。

## 11. 数值、类型和位操作

### CPP-TYPE-001 使用固定宽度整数

协议、持久化、跨进程、跨平台和硬件相关字段必须使用 `std::uint*_t` 或 `std::int*_t`。
普通计数和索引可使用 `std::size_t`。禁止假设 `int`、`long` 宽度。

### CPP-TYPE-002 禁止隐式窄化

窄化转换、符号转换、浮点到整数转换必须显式，并在转换前检查范围。禁止依赖溢出、截断
或实现定义行为。

### CPP-TYPE-003 枚举必须强类型

新增枚举必须使用 `enum class`。跨接口传输时必须定义底层类型和合法值。禁止将枚举值
与整数混用。

### CPP-TYPE-004 位操作必须局部封装

位掩码、移位、端序转换、对齐处理必须封装在小函数中，并有边界测试。禁止对有符号数
执行移位。协议解析必须明确大小端。

### CPP-TYPE-005 浮点使用需批准

安全相关判断、状态机和诊断阈值应避免直接浮点相等比较。浮点算法必须定义误差范围、
单位、输入边界和异常值处理。

## 12. 字符串、容器和算法

### CPP-CONT-001 容器容量必须受控

实时路径、通信热路径和常驻服务不得使用无限增长容器。`vector`、`string`、map 类容器
必须有容量策略、清理策略和资源耗尽处理。

### CPP-CONT-002 字符串边界必须明确

外部输入字符串必须验证长度、编码、终止符、路径字符和日志安全。禁止把外部输入直接
拼接为路径、命令、SQL、JSON 或诊断消息。

### CPP-CONT-003 优先标准算法

简单循环可以保留；复杂查找、复制、转换、排序应优先使用标准算法，以减少边界错误。
使用算法时不得降低可读性。

### CPP-CONT-004 避免隐藏拷贝

范围循环读取大对象时使用 `const auto&`。需要拷贝时必须语义明确。AI 不得为了简短而
引入昂贵拷贝。

## 13. Linux/POSIX 边界

### CPP-LNX-001 系统调用必须封装

业务代码不得直接散落调用 POSIX API。应使用 `FileDescriptor`、`Socket`、`TimerFd`、
`Epoll`、`Process`、`SharedMemory` 等 RAII wrapper 或项目已有封装。

### CPP-LNX-002 文件描述符安全

打开 fd 必须设置 `CLOEXEC`，明确阻塞模式和权限。关闭失败、dup、fork/exec 边界必须
有设计说明。禁止 fd 泄漏到子进程。

### CPP-LNX-003 路径和权限

文件路径必须来自受信配置或经过规范化校验。禁止跟随不受信符号链接写敏感文件。新建
文件必须显式权限，避免宽权限默认值。

### CPP-LNX-004 进程和 shell

禁止 `system` 和 `popen`。确需启动进程时，必须使用参数数组形式、固定可执行路径、
最小环境变量、超时、退出码检查和日志审计。

### CPP-LNX-005 信号处理

信号 handler 内只能调用异步信号安全操作。复杂处理必须转发到 eventfd、pipe 或事件
循环。禁止在 signal handler 中使用锁、日志库、分配内存或 C++ 对象复杂操作。

## 14. 安全编码

### CPP-SEC-001 输入默认不可信

来自网络、IPC、诊断、持久化、OTA、配置、环境变量、命令行、设备节点和传感器的数据
都必须视为不可信。解析代码必须有长度、范围、格式、状态机和资源限制。

### CPP-SEC-002 认证、授权和加密

禁止自行实现加密算法、随机数算法、证书校验、密钥派生和安全协议。必须使用项目批准的
安全库和平台服务。密钥、token 和证书材料不得写入日志、core dump 或普通配置文件。

### CPP-SEC-003 日志不泄密

日志不得包含密钥、token、完整证书、个人数据、车辆敏感标识或未脱敏诊断载荷。错误
日志必须对运维有用，但不得暴露攻击面。

### CPP-SEC-004 防止拒绝服务

解析器、队列、缓存、连接、重试和日志都必须有限额。外部输入不得触发无限循环、无限
分配、无限递归或持续高优先级 CPU 占用。

## 15. 诊断、日志和可观测性

### CPP-OBS-001 日志必须结构化

产品日志应包含模块、错误码、关键上下文和限频策略。禁止在高频循环中无节制日志。

### CPP-OBS-002 错误码可追踪

对外暴露的失败必须能映射到诊断码、内部错误域或明确的恢复动作。禁止只输出自然语言
错误字符串后丢失机器可处理信息。

### CPP-OBS-003 健康监控

常驻服务必须暴露启动状态、运行状态、关键依赖状态和自检结果。线程异常退出、队列阻塞、
周期任务超时、持久化失败必须可观测。

## 16. 头文件、命名和格式

### CPP-STYLE-001 头文件自包含

每个头文件必须能独立编译。头文件只包含必要依赖，优先前置声明。禁止头文件产生全局
副作用、定义非 `inline` 变量或引入 `using namespace`。

### CPP-STYLE-002 include 顺序

推荐顺序：

1. 对应头文件。
2. C++ 标准库。
3. C 标准库兼容头。
4. 第三方库。
5. 项目头文件。

每组之间空一行。禁止依赖 include 顺序隐藏缺失依赖。

### CPP-STYLE-003 命名约定

- 文件名：`lower_snake_case.h`、`lower_snake_case.cpp`。
- 命名空间：`lower_snake_case`。
- 类型、类、结构体：`UpperCamelCase`。
- 函数、成员函数：`lowerCamelCase`。
- 变量、成员变量：`lower_snake_case`，成员变量后缀 `_`。
- 常量：`kUpperCamelCase`。
- 枚举值：`kUpperCamelCase`。
- 宏：`PROJECT_MODULE_NAME`，仅限 include guard、编译配置和平台适配。

### CPP-STYLE-004 格式化

使用项目 `.clang-format`。未配置时采用：

- 缩进 4 空格。
- 最大行宽 100。
- 大括号遵循现有模块风格；新模块使用 K&R。
- 指针和引用贴近类型：`const T& value`、`T* ptr`。
- 每个 `case` 必须显式 `break`、`return` 或标注 fallthrough。

### CPP-STYLE-005 注释

注释解释意图、约束、单位、线程安全和非显然权衡。禁止注释重复代码字面含义。公共接口
必须写明失败模式和阻塞行为。

## 17. 测试和验证

### CPP-TEST-001 新代码必须可测试

新增业务逻辑必须有单元测试。难以测试通常说明设计边界不清。AI 生成代码时必须同时
生成或更新测试，除非用户明确只要草稿。

### CPP-TEST-002 边界测试

必须覆盖：

- 空输入、最大输入、非法输入。
- 整数边界、符号转换、溢出风险。
- 资源耗尽、超时、重试、取消。
- POSIX 短读短写、`EINTR`、权限失败。
- 并发停止、重复启动、析构时仍有任务。

### CPP-TEST-003 测试必须确定

测试不得依赖真实时间 sleep、外部网络、固定执行顺序或机器负载。时间、I/O、随机数和
系统调用必须可替换。

### CPP-TEST-004 合入门禁

合入前必须通过格式化、编译、单元测试、静态分析。安全相关模块还应通过 sanitizer、
模糊测试或协议解析压力测试。

## 18. AI 生成代码规则

### CPP-AI-001 生成前必须读取规范

Copilot、Codex、Claude 或其他 AI 工具生成 C++ 前，必须读取：

1. [ai-vibe-coding-cpp-check-rules.md](./ai-vibe-coding-cpp-check-rules.md)。
2. 本文档。
3. 当前模块设计文档、接口文件和测试。
4. `.clang-format`、`.clang-tidy` 和当前模块构建/测试配置。

涉及规范重构、来源覆盖、审计映射、规则冲突或标准解释时，必须额外读取
[vehicle-embedded-linux-cpp-standards-sources.md](./references/vehicle-embedded-linux-cpp-standards-sources.md)
和
[vehicle-embedded-linux-cpp-standards-traceability.md](./references/vehicle-embedded-linux-cpp-standards-traceability.md)。
禁止为了普通代码生成任务直接加载完整离线大文件。

### CPP-AI-002 AI 不得擅自扩大范围

AI 只能修改与任务直接相关的文件。禁止顺手重构、统一格式化无关文件、替换框架、引入
新库、改变构建系统、改变语言标准或调整 ABI，除非任务明确要求。

### CPP-AI-003 AI 默认禁止的输出

AI 不得生成：

- 裸 `new/delete`、异常、RTTI、`dynamic_cast`、`reinterpret_cast`。
- `system`、`popen` 或 shell 拼接。
- 无测试的复杂逻辑。
- 未说明线程生命周期的后台线程。
- 默认捕获 lambda。
- 忽略返回值的代码。
- 无容量上限的缓存、队列或重试循环。
- 随机日志、无错误码日志、泄密日志。

### CPP-AI-004 AI 必须输出验证证据

AI 修改代码后必须说明：

- 修改了哪些文件。
- 哪些规则影响了设计。
- 运行了哪些测试、构建、静态分析。
- 未能验证的项目和原因。

### CPP-AI-005 AI 生成代码提示模板

在 VS Code 插件或命令行 AI 中使用以下提示：

```text
请按 docs/vehicle-embedded-linux-cpp-coding-standard.md 生成产品级车载嵌入式
Linux C++ 代码。默认 C++14，遵守 AUTOSAR/MISRA/CERT 意图。禁止异常、RTTI、
裸 new/delete、全局可变状态、system/popen、默认捕获 lambda 和未封装 POSIX 调用。
使用 RAII、显式所有权、Result/Status 错误返回、受控容器容量、单调时钟和可测试设计。
同时更新或生成测试，并说明验证命令。
```

## 19. Code Review 清单

评审新增或修改 C++ 代码时必须检查：

- 接口是否表达所有权、生命周期、线程安全、错误和时间单位。
- 是否存在未初始化变量、悬空引用、隐藏拷贝、隐式窄化、符号转换。
- 是否存在裸资源、fd 泄漏、锁泄漏、线程泄漏。
- 是否存在异常、RTTI、C 风格 cast、宏滥用、全局可变状态。
- 是否处理 POSIX `errno`、`EINTR`、短读短写和资源耗尽。
- 是否有无界容器、无界重试、无界等待或高频日志。
- 是否有输入验证、权限控制、敏感信息脱敏。
- 是否有确定性测试、边界测试、失败路径测试。
- 是否通过格式化、编译、静态分析和相关测试。
- 如果规则偏离，是否有批准记录和风险验证。

## 20. 偏离记录模板

```text
Deviation ID:
Rule:
File/Function:
Reason:
Risk:
Alternatives considered:
Mitigation:
Verification:
Owner:
Approval:
Expiry or review date:
```

## 21. 推荐工具配置

最低推荐：

- `clang-format`：统一格式。
- `clang-tidy`：启用 bugprone、cert、cppcoreguidelines、hicpp、modernize 中经项目筛选
  的规则。
- `cppcheck`：补充静态分析。
- `cmake --build` 或项目原生构建系统。
- `ctest`、GoogleTest、Catch2 或项目既有测试框架。
- ASan、UBSan、TSan 主机侧验证。
- 覆盖率工具：用于识别未测失败路径，不作为唯一质量指标。

商业项目应使用客户认可的 MISRA/AUTOSAR 覆盖工具，并保留报告。

## 22. 来源覆盖和重构索引

本文档只保留可执行项目规则，不内嵌离线资料全文、授权标准原文、OCR 原始内容或
长篇示例。常规 AI coding 的紧凑检查规则维护在
[ai-vibe-coding-cpp-check-rules.md](./ai-vibe-coding-cpp-check-rules.md)。
来源到规则章节的覆盖关系、Obsidian wikilinks 和 Mermaid 图谱维护在
[vehicle-embedded-linux-cpp-standards-traceability.md](./references/vehicle-embedded-linux-cpp-standards-traceability.md)。

更新本文档时必须先检查该索引，确认：

- 新来源是否为官方来源、授权来源或已归档的高质量本地资料。
- 现有章节是否已经覆盖该来源的工程意图，避免重复规则。
- 新增内容是否能转化为可执行约束、评审检查项、工具门禁或 AI 生成限制。
- 哪些内容因授权、OCR 质量、低信号网页或项目不适用性不进入正文。

如果新增或删除离线资料，必须同步更新 source map、offline manifest 和 traceability
index，保持主规范、来源索引和离线归档一致。
