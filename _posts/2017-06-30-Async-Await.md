---
layout: post
title: "读《C#并发编程经典实例》网摘笔记"
date: 2017-06-30 14:06:05
categories: C#
tags: 并发 Await Async
excerpt: 一直以来都有一种观点是实现底层架构，编写驱动和引擎，或者是框架和工具开发的才是高级开发人员，做上层应用的人仅仅是“码农”，其实能够利用好平台提供的相关类库，而不是全部采用底层技术自己实现，开发出高质量，稳定的应用程序，对技术能力的考验并不低于开发底层库，如TPL，async，await等。
author: fatso
---

* content
{:toc}


## 并发编程概述
+ 并发:
同时做多件事
+ 多线程:
并发的一种形式，它采用多个线程来执行程序
+ 并行处理:
把正在执行的大量的任务分割成小块，分配给多个同时运行的线程
并行处理是多线程的一种，而多线程是并发的一种处理形式
+ 异步编程:
并发的一种形式，它采用future模式或者callback机制，以避免产生不必要的线程
异步编程的核心理念是异步操作：启动了的操作会在一段时间后完成。这个操作正在执行时，不会阻塞原来的线程。启动了这个操作的线程，可以继续执行其他任务。当操作完成后，会通知它的future，或者调用回调函数，以便让程序知道操作已经结束
+ await关键字的作用:
启动一个将会被执行的Task（该Task将在新线程中运行），并立即返回，所以await所在的函数不会被阻塞。当Task完成后，继续执行await后面的代码
+ 响应式编程:
并发的一种基于声明的编程方式，程序在该模式中对事件作出反应
不要用 void 作为 async 方法的返回类型！ async 方法可以返回 void，但是这仅限于编写事件处理程序。一个普通的 async 方法如果没有返回值，要返回 Task，而不是 void
async 方法在开始时以同步方式执行。在 async 方法内部，await 关键字对它的参数执行一个异步等待。它首先检查操作是否已经完成，如果完成了，就继续运行 （同步方式）。否则，它会暂停 async 方法，并返回，留下一个未完成的 task。一段时间后， 操作完成，async
方法就恢复运行。
await代码中抛出异常后，异常会沿着Task方向前进到引用处
你一旦在代码中使用了异步，最好一直使用。调用 异步方法时，应该（在调用结束时）用 await 等待它返回的 task 对象。一定要避免使用 Task.Wait 或 Task .Result 方法，因为它们会导致死锁
线程是一个独立的运行单元，每个进程内部有多个线程，每个线程可以各自同时执行指令。 每个线程有自己独立的栈，但是与进程内的其他线程共享内存
每个.NET应用程序都维护着一个线程池，这种情况下，应用程序几乎不需要自行创建新的线程。你若要为 COM interop 程序创建 SAT 线程，就得 创建线程，这是唯一需要线程的情况
线程是低级别的抽象，线程池是稍微高级一点的抽象
并发编程用到的集合有两类：并发变成+不可变集合
大多数并发编程技术都有一个类似点：它们本质上都是函数式的。这里的函数式是作为一种基于函数组合的编程模式。函数式的一个编程原则是简洁（避免副作用），另一个是不变性（指一段数据不能被修改)
.NET 4.0 引入了并行任务库（TPL），完全支持数据并行和任务并行。但是一些资源较少的 平台（例如手机），通常不支持 TPL。TPL 是 .NET 框架自带的

## 异步编程基础
指数退避是一种重试策略，重试的延迟时间会逐 次增加。在访问 Web 服务时，最好的方式就是采用指数退避，它可以防止服务器被太多的重试阻塞
``` C#
static async Task<string> DownloadStringWithRetries(string uri)
{
    using (var client = new HttpClient())
    {
        // 第 1 次重试前等 1 秒，第 2 次等 2 秒，第 3 次等 4 秒。
        var nextDelay = TimeSpan.FromSeconds(1);
        for (int i = 0; i != 3; ++i)
        {
            try
            {
                return await client.GetStringAsync(uri);
            }
            catch
            { }

            await Task.Delay(nextDelay);
            nextDelay = nextDelay + nextDelay;
        }
        
        // 最后重试一次，以便让调用者知道出错信息。
        return await client.GetStringAsync(uri);
    }
}
```
Task.Delay 适合用于对异步代码进行单元测试或者实现重试逻辑。要实现超时功能的话， 最好使用 CancellationToken
如何实现一个具有异步签名的同步方法。如果从异步接口或基类继承代码，但希望用同步的方法来实现它，就会出现这种情况。解决办法是可以使用 Task.FromResult 方法创建并返回一个新的 Task 对象，这个 Task 对象是已经 完成的，并有指定的值
使用 IProgress 和 Progress 类型。编写的 async 方法需要有 IProgress 参数，其 中 T 是需要报告的进度类型，可以展示操作的进度
Task.WhenALl可以等待所有任务完成，而当每个Task抛出异常时，可以选择性捕获异常
Task.WhenAny可以等待任一任务完成，使用它虽然可以完成超时任务（其中一个Task设为Task.Delay），但是显然用专门的带有取消标志的超时函数处理比较好
第一章提到async和上下文的问题：在默认情况下，一个 async 方法在被 await 调用后恢复运行时，会在原来的上下文中运行。而加上扩展方法ConfigureAwait(false)后，则会在await之后丢弃上下文

## 并行开发的基础
Parallel 类有一个简单的成员 Invoke，可用于需要并行调用一批方法，并且这些方法（大部分）是互相独立的
``` C#
static void ProcessArray(double[] array)
{
    Parallel.Invoke(
    () => ProcessPartialArray(array, 0, array.Length / 2),
    () => ProcessPartialArray(array, array.Length / 2,array.Length));
}

static void ProcessPartialArray(double[] array, int begin, int end)
{
    // 计算密集型的处理过程 ...  
}
```
在并发编程中，Task类有两个作用：作为并行任务，或作为异步任务。并行任务可以使用 阻塞的成员函数，例如 Task.Wait、Task.Result、Task.WaitAll 和 Task.WaitAny。并行任务通常也使用 AttachedToParent 来建立任务之间的“父 / 子”关系。并行任务的创建需要 用 Task.Run 或者 Task.Factory.StartNew。
相反的，异步任务应该避免使用阻塞的成员函数，而应该使用 await、Task.WhenAll 和 Task. WhenAny。异步任务不使用 AttachedToParent，但可以通过 await 另一个任务，建立一种隐 式的“父 / 子”关系。

## 测试技巧
MSTest从Visual Studio2012 版本开始支持 async Task 类型的单元测试
如果单元测试框架不支持 async Task 类型的单元测试，就需要做一些额外的修改才能等待异步操作。其中一种做法是使用 Task.Wait，并在有错误时拆开 AggregateException 对象。我的建议是使用 NuGet 包 Nito.AsyncEx 中的 AsyncContext 类
这里附上一个ABP中实现的可操作AsyncHelper类，就是基于AsyncContext实现
``` C#

    /// <summary>
    /// Provides some helper methods to work with async methods.
    /// </summary>
    public static class AsyncHelper
    {
        /// <summary>
        /// Checks if given method is an async method.
        /// </summary>
        /// <param name="method">A method to check</param>
        public static bool IsAsyncMethod(MethodInfo method)
        {
            return (
                method.ReturnType == typeof(Task) ||
                (method.ReturnType.IsGenericType && method.ReturnType.GetGenericTypeDefinition() == typeof(Task<>))
                );
        }

        /// <summary>
        /// Runs a async method synchronously.
        /// </summary>
        /// <param name="func">A function that returns a result</param>
        /// <typeparam name="TResult">Result type</typeparam>
        /// <returns>Result of the async operation</returns>
        public static TResult RunSync<TResult>(Func<Task<TResult>> func)
        {
            return AsyncContext.Run(func);
        }

        /// <summary>
        /// Runs a async method synchronously.
        /// </summary>
        /// <param name="action">An async action</param>
        public static void RunSync(Func<Task> action)
        {
            AsyncContext.Run(action);
        }
    }
```
在 async 代码中，关键准则之一就是避免使用 async void。我非常建议大家在对 async void 方法做单元测试时进行代码重构，而不是使用 AsyncContext。

## 集合
线程安全集合是可同时被多个线程修改的可变集合。线程安全集合混合使用了细粒度锁定和无锁技术，以确保线程被阻塞的时间最短（通常情况下是根本不阻塞）。对很多线程安全集合进行枚举操作时，内部创建了该集合的一个快照（snapshot），并对这个快照进行枚举操作。线程安全集合的主要优点是多个线程可以安全地对其进行访问，而代码只会被阻塞很短的时间，或根本不阻塞

ConcurrentDictionary 是数据结构中的精品，它是线程安全的，混合使用了细粒度锁定和无锁技术，以确保绝大多数情况下能进行快速访问.

ConcurrentDictionary 内置了AddOrUpdate, TryRemove, TryGetValue等方法。如果多个线程读写一个共享集合，使用ConcurrentDictionary 是最合适的，如果不会频繁修改，那就更适合使用ImmutableDictionary 。而如果是一些线程只添加元素，一些线程只移除元素，最好使用生产者/消费者集合

## 函数式OOP
异步编程是函数式的（functional），.NET 引入的async让开发者进行异步编程的时候也能用过程式编程的思维来进行思考，但是在内部实现上，异步编程仍然是函数式的

伟人说过，世界既是过程式的，也是函数式的，但是终究是函数式的

可以用await等待的是一个类（如Task 对象），而不是一个方法。可以用await等待某个方法返回的Task，无论它是不是async方法。

类的构造函数里是不能进行异步操作的，一般可以使用如下方法。相应的，我们可以通过
``` C#
var instance=new Program.CreateAsync();

    class Program
    {
        private Program()
        {

        }

        private async Task<Program> InitializeAsync()
        {
            await Task.Delay(TimeSpan.FromSeconds(1));

            return this;
        }

        public static Task<Program> CreateAsync()
        {
            var result = new Program();

            return result.InitializeAsync();
        }

    }
```
在编写异步事件处理器时，事件参数类最好是线程安全的。要做到这点，最简单的办法就 是让它成为不可变的（即把所有的属性都设为只读）

## 同步
同步的类型主要有两种：通信和数据保护

如果下面三个条件都满足，就需要用同步来保护共享的数据
多段代码正在并发运行
这几段代码在访问（读或写）同一个数据
至少有一段代码在修改（写）数据
观察以下代码，确定其同步和运行状态
``` C#
class SharedData
{
    public int Value { get; set; }
}

async Task ModifyValueAsync(SharedData data)
{
    await Task.Delay(TimeSpan.FromSeconds(1));
    data.Value = data.Value + 1;
}

// 警告：可能需要同步，见下面的讨论。
async Task<int> ModifyValueConcurrentlyAsync()
{
    var data = new SharedData();

    // 启动三个并发的修改过程。
    var task1 = ModifyValueAsync(data);
    var task2 = ModifyValueAsync(data);
    var task3 = ModifyValueAsync(data);

    await Task.WhenAll(task1, task2, task3);

    return data.Value;
}
```
本例中，启动了三个并发运行的修改过程。需要同步吗？答案是“看情况”。如果能确定 这个方法是在 GUI 或 ASP.NET 上下文中调用的（或同一时间内只允许一段代码运行的任 何其他上下文），那就不需要同步，因为这三个修改数据过程的运行时间是互不相同的。 例如，如果它在 GUI 上下文中运行，就只有一个 UI 线程可以运行这些数据修改过程，因 此一段时间内只能运行一个过程。因此，如果能够确定是“同一时间只运行一段代码”的 上下文，那就不需要同步。但是如果从线程池线程（如 Task.Run）调用这个方法，就需要同步了。在那种情况下，这三个数据修改过程会在独立的线程池线程中运行，并且同时修改 data.Value，因此必须同步地访问 data.Value。

不可变类型本身就是线程安全的，修改一个不可变集合是不可能的，即便使用多个Task.Run向集合中添加数据，也并不需要同步操作

线程安全集合（例如 ConcurrentDictionary）就完全不同了。与不可变集合不同，线程安 全集合是可以修改的。线程安全集合本身就包含了所有的同步功能

关于锁的使用，有四条重要的准则
限制锁的作用范围（例如把lock语句使用的对象设为私有成员）
文档中写清锁的作用内容
锁范围内的代码尽量少（锁定时不要进行阻塞操作）
在控制锁的时候绝不运行随意的代码（不要在语句中调用事件处理，调用虚拟方法，调用委托）
如果需要异步锁，请尝试 SemaphoreSlim

不要在 ASP. NET 中使用 Task.Run，这是因为在 ASP.NET 中，处理请求的代码本来就是在线程池线程中运行的，强行把它放到另一个线程池线程通常会适得其反

## 实用技巧

程序的多个部分共享了一个资源，现在要在第一次访问该资源时对它初始化
``` C#
static int _simpleValue;
static readonly Lazy<Task<int>> MySharedAsyncInteger = 
    new Lazy<Task<int>>(() => 
    Task.Run(async () =>
        {
            await Task.Delay(TimeSpan.FromSeconds(2));

            return _simpleValue++;
        }));

async Task GetSharedIntegerAsync()
{
    int sharedValue = await MySharedAsyncInteger.Value;
}
```