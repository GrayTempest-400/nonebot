这是一个极简教程，如有不正确请在issue中指出,<font color='red'>欢迎大佬指教</font>，用Markdown编辑器和阅读器以获得最佳体验

参考文档声明:[MRSlouzk/Nonebot-plugintutorials： Nonebot2的插件编写教程，有些地方可能讲的不好，欢迎大佬们指教！ (github.com)](https://github.com/MRSlouzk/Nonebot-plugintutorials)

[nonebot/nonebot2： 跨平台 Python 异步聊天机器人框架 / 用 Python 编写的异步多平台聊天机器人框架 (github.com)](https://github.com/nonebot/nonebot2)

[Well404 - 知乎 (zhihu.com)](https://www.zhihu.com/people/well404)

基于 Python asyncio 编写

### 1.关键词应答，示例代码

from nonebot import<font color='red'> on_keyword</font>

from nonebot.adapters.onebot.v11 import Message

 

<font color='cornflowerblue'>word</font>=<font color='red'>on_keyword</font>({"x"})

 

@<font color='cornflowerblue'>word</font>.handle()

async def _():

await <font color='cornflowerblue'>word</font>.finish(Message("y"))

```python
from nonebot import on_keyword

from nonebot.adapters.onebot.v11 import Message

 

word=on_keyword({"x"})

 

word.handle()

async def _():

await word.finish(Message("y"))
```

 这段示例代码是当bot接收到"x"时，返回"y"

<font color='red'>红色</font>的是响应器的函数可替换为：

1. **on**: 创建任何类型的事件响应器。
2. **on_metaevent**: 创建元事件响应器。
3. **on_message**: 创建消息事件响应器。
4. **on_request**: 创建请求事件响应器。
5. **on_notice**: 创建通知事件响应器。
6. **on_startswith**: 创建消息开头匹配事件响应器。
7. **on_endswith**: 创建消息结尾匹配事件响应器。
8. **on_fullmatch**: 创建消息完全匹配事件响应器。
9. **on_keyword**: 创建消息关键词匹配事件响应器。
10. **on_command**: 创建命令消息事件响应器。
11. **on_shell_command**: 创建     shell 命令消息事件响应器。
12. **on_regex**: 创建正则表达式匹配事件响应器。
13. **CommandGroup**: 创建具有共同命令名称前缀的命令组。
14. **MatcherGroup**: 创建具有共同参数的响应器组。

响应器用于接收关键词如**on_message**是接收所有消息，**on_command** 接收的消息是需要带”/”的

 蓝色的在写代码时可自定义，但每个都要相同如:

<font color='cornflowerblue'>a</font>=<font color='red'>on_keyword</font>({"x"})

 @<font color='cornflowerblue'>a</font>.handle()

async def _():

await <font color='cornflowerblue'>a</font>.finish(Message("y"))



### rule的用法

```python
from nonebot import on_keyword
from nonebot.rule import to_me

word=on_keyword({"x"},rule=to_me())
```

rule是判断信息类型如

rule=to_me 是检测信息是否at bot

<font color='orange'>checker</font>

```python
from nonebot import on_keyword
from nonebot.adapters.onebot.v11 import Event

def _checker(event: Event) -> bool :
    return event.message_type=="group"

word=on_keyword({"你是谁"},rule=_checker)
```

<font color='orange'>事件响应器的匹配规则是一个 `Rule` 对象，它是一系列 `checker` 的集合，当所有的 `checker` 都返回 `True` 时，才会触发该响应器。</font>

### 优先级 `priority`

事件响应器的优先级代表事件响应器的执行顺序

同一优先级的事件响应器会**同时执行**，优先级数字**越小**越先响应！优先级请从 `1` 开始排序！

```python
word=on_keyword({"你是谁"},priority=5)
```

阻断 `block`

当有任意事件响应器发出了阻止事件传递信号时，该事件将不再会传递给下一优先级，直接结束处理。

NoneBot 内置的事件响应器中，所有非 `command` 规则的 `message` 类型的事件响应器都会阻断事件传递，其他则不会。

在部分情况中，可以使用 `matcher.stop_propagation()` 方法动态阻止事件传播，该方法需要 `handler` 在参数中获取 `matcher` 实例后调用方法。

```python
foo = on_request()
@foo.handle()async def handle(matcher: Matcher):    matcher.stop_propagation()
```



### 有效期 `temp`/`expire_time`

`temp` 属性：配置事件响应器在下一次响应之后移除。

`expire_time` 属性：配置事件响应器在指定时间之后移除。

### 阻断 `block`

当有任意事件响应器发出了阻止事件传递信号时，该事件将不再会传递给下一优先级，直接结束处理。

NoneBot 内置的事件响应器中，所有非 `command` 规则的 `message` 类型的事件响应器都会阻断事件传递，其他则不会。

在部分情况中，可以使用 `matcher.stop_propagation()` 方法动态阻止事件传播，该方法需要 `handler` 在参数中获取 `matcher` 实例后调用方法。

```python
foo = on_request()
@foo.handle()async def handle(matcher: Matcher):    matcher.stop_propagation()
```

### CQ码[][CQ 码 / CQ Code | go-cqhttp 帮助中心][#]([CQ 码 / CQ Code | go-cqhttp 帮助中心](https://docs.go-cqhttp.org/cqcode/))和MessageSegment

CQ 码是指 CQ 中特殊消息类型的文本格式, 这是它的基本语法:

```python
[CQ:类型,参数=值,参数=值]
```

1

在 QQ 中, 一个消息由多个部分构成, 例如一段文本, 一个图片, at 某人的一个部分. CQ 中定义了与这些消息相符的 CQ 码, 以方便用户使用.

例如, 下面是由一个 at 部分和一个文本部分构成的合法 CQ 消息串

```python
[CQ:at,qq=114514]早上好啊
```

1

例如qq号为114514的人昵称为"小明", 那么上述消息串在QQ中的渲染是这样的:

```python
@小明 早上好啊
```

CQ码有多种功能如：发送语音，戳一戳等

<font color='red'>但容易被风控，不建议使用</font>

### MessageSegment

MessageSegment的使用方法实际上非常简单，基本的用法就是 `MessageSegment.xxx()` ，并且正如上文所说，MessageSegment本质上是Message类，所以我们不需要用 `Message()` 对其进行转义。 首先我们先输入 `MessageSegment.` 然后查看编辑器的提示或转到源码，就能看到MessageSegment提供的方法了:

![img](https://pic1.zhimg.com/80/v2-fa64324e79cd01aea9a35a76f283169c_720w.webp)

我们这里用image进行举例:

```python
from nonebot.adapters.onebot.v11 import MessageSegment
setu001 = MessageSegment.image('file:///D:/learning_materials/setu/001.jpg')
```

这样setu001就是一个Message类的涩图了，理论上和 `Message('[CQ:image,file=file:///D:/learning_materials/setu/001.jpg]')` 是等效的。

MessageSegment由于功能很多，因此不在此一一演示了，想要用好MessageSegment，需要需要多加利用编辑器的自动补全和语法提示，这样就不必翻源码即可使用了。

### Call _api [][CQ 码 / CQ Code | go-cqhttp 帮助中心][#](https://docs.go-cqhttp.org/api/)

### 使用方法

首先我们需要知道我们对接的客户端有什么样的api，这样我们才能对其发起请求。我们使用的[gocqhttp的官方文档](https://link.zhihu.com/?target=https%3A//docs.go-cqhttp.org/api)中对API有详细的介绍。

call_api的写法目前有两种，`bot.call_api('xxx', **{key:value})`和`bot.xxx(key=value)`两种仅写法不同，实质并无影响，可根据自身喜好进行调整。

这里我们以“获取群信息”为例进行写法一的演示。

![img](https://pic2.zhimg.com/80/v2-d42e248459b2124283ccac7a9a58ebed_720w.webp)

我们需要提供两个数据，其中no_cache为选填字段（是否为选填字段需要自行结合描述和实践进行判断），group_id为必填字段。

```python
from nonebot.adapters.onebot.v11 import Bot
from nonebot import on_message

test = on_command('test')
@test.handle()
async def _(bot: Bot):
    # call_api的写法一
    data = await bot.call_api('get_group_info',**{
        'group_id' : 123456
    })
```

这样我们就获得了data这个json格式的返回值，然后进行简单的转义就可以读取了。 这里我们再演示一下和“发送消息”，并使用第二种方法进行演示。

```python
from nonebot.adapters.onebot.v11 import Bot, Event
from nonebot import on_message
import ast

test = on_command('test')
@test.handle()
async def _(bot: Bot, event: Event):
    # call_api的写法一
    data = await bot.call_api('get_group_info',**{
        'group_id' : 123456
    })
    # 对json进行转义
    data = ast.literal_eval(str(data))
    msg = f"群号  ：{data['group_id']}\
          \n群名称：{data['group_name']}\
          \n成员数：{data['member_count']}"
    # call_api的写法二
    await bot.send(
        event   = event,
        message = msg
    )
    # 不过，这里更推荐直接用响应器的send方法
    # await test.send(msg)
```

