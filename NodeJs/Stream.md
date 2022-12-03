### 1.文件读写的Stream

- 事实上Node中很多对象都是基于流实现的
  - http模块的Request和Response对象
  - process.stdout对象
- 官方：另外所有的流都是EventEmitter的实例
- Node中有四种基本流类型
  - Writable：可向其写入数据的流；例如fs.createWriteStream()
  - Readable：可从中读取数据流；例如fs.createReadStream()
  - Duplex：同时为Writable和Readable的流；例如net.Socket
  - Transform：Duplex可以再读取和写入数据时，修改或转换数据的流；例如zlib.createDeflate() 