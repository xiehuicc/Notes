### 1.简介

> 

![QQ截图20210420143302.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5d0ede01df104b248f55e6b3a34ac5d4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



### 2.入口【日志收集】

> 所谓入口，**其实就是收集日志信息**，如何有效，分门别类的收集所需信息，是我们记录日志文件的第一步

log4js提供了**等级分类**和**别名分类**



#### 等级（Level）

> 就是日志的分级，日志有了分级，才能更好的为我们展示日志（不同级别的日志在控制台采用不同的颜色，比如error通常是红色）

```js
{
  ALL: new Level(Number.MIN_VALUE, "ALL"),
  TRACE: new Level(5000, "TRACE"),
  DEBUG: new Level(10000, "DEBUG"),
  INFO: new Level(20000, "INFO"),
  WARN: new Level(30000, "WARN"),
  ERROR: new Level(40000, "ERROR"),
  FATAL: new Level(50000, "FATAL"),
  MARK: new Level(9007199254740992, "MARK"), // 2^53
  OFF: new Level(Number.MAX_VALUE, "OFF")
}

```

![QQ截图20210420150155.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff789f078d45411d8b1a8aa139e1021f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



#### 类型（category）

> 实例化log4js时，可以指定一个别名，然后再日志中可以方便区分哪个文件

```js
const log4js = require('log4js');

// 实例化时，唯一可以传的一个参数category
const logger = log4js.getLogger('日志1');

logger.level = "all"

logger.trace('this is trace');
logger.debug('this is debug');
logger.info('this is info');
logger.warn('this is warn');
logger.error('this is error');
logger.fatal('this is fatal');
logger.mark('this is mark');

```

![QQ截图20210420150733.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9cb0a4a120a84967b981b358cea506be~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



### 3. 出口【日志输出】

#### Appender (输出源)

> 如何保存再文件中，就时候就需要appender上场了，它决定日志输出到哪里。可以将输出日志信息写入文件，发送电子邮件，通过网络发送数据到其它地方（开发时一般输出到控制台，上线后一般输出到文件里）

![QQ截图20210420151221.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1be4803724c34cc795f12b36c7ca87d2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

#### file

```js
 // 将日志文件保存到文件中
log4js.configure({
   appenders: { koa2learn: { type: 'file', filename: './log/koa2learn.log' } },
   categories: { default: { appenders: ['koa2learn'], level: 'info' } }
 }); 
```



#### datefile





#### stdout

> 该方法用于，输出日志到标注输入输出流

```js
const log4js = require('log4js');
// 对 category 和 appenders 进行配置
log4js.configure({
    appenders: { 'out': { type: 'stdout' } },
    categories: { default: { appenders: ['out'], level: 'info' } }
});


let logger = log4js.getLogger();
logger.trace('this is trace');
logger.debug('this is debug');
logger.info('this is info');
logger.warn('this is warn');
logger.error('this is error');
logger.fatal('this is fatal');
logger.mark('this is mark');

```

控制台打印：

![QQ截图20210420161642.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a6de40fccd0c41a58cce1a952c7fdf12~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)