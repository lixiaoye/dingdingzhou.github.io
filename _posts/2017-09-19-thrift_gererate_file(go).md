---
layout:     post
title:      "Thrift 生成文件简析"
subtitle:   "Golang版"
date:       2017-09-19 12:00:00
author:     "Dingding"
header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - thrift
    - golang
---

## thrift文件内容
```
struct mydata {
  1: i32 myparam1,
  2: string myparam2
}

service myser {
  mydata myif1(1: i32 funparam1,2: string funparam2),
  i32 myif2(1: i32 funparam1)
}

```

## ttypes.go文件内容

### 结构体 Mydata 
* 函数 NewMydata()
* 函数 Write()
    * WriteStructBegin()
        * 需要参数:结构体名称
    * writeField1(),writeField2(),  ...
        * 需要参数:参数名称,参数类型,参数指示符
    * WriteFieldStop()
    * WriteStructEnd()
* 内部函数 writeField1()
* 内部函数 writeField2()
* 函数 Read()
    * ReadStructBegin()
    * readField1(),readField2(), ...
    * ReadFieldStop()
    * ReadStructEnd()
* 内部函数 writeField1()
* 内部函数 writeField2()
* 函数 String()

## myser.go文件内容

### 接口 Myser
* Myif1(funparam1 int32, funparam2 string) (r *Mydata, err error)
* Myif2(funparam1 int32) (r int32, err error)

### 结构体 MyserClient
* 域
    * Transport
    * ProtocolFactory
    * InputProtocol
    * OutputProtocol
    * SeqId
* 操作函数(<span style="color:red">这是客户端唯一需要关心的函数</span>)
    * func NewMyserClientFactory(t thrift.TTransport, f thrift.TProtocolFactory) *MyserClient  
      用于生成结构体 MyserClient
    * 函数 func NewMyserClientProtocol(t thrift.TTransport, iprot thrift.TProtocol, oprot thrift.TProtocol) *MyserClient  
      用于生成结构体 MyserClient,与上一个意义一致,只是由于go不支持重载,才多了一个
* 函数成员  
    * func (p *MyserClient) Myif1(funparam1 int32, funparam2 string) (r *Mydata, err error)  
      封装了如下两个方法的调用关系:
        * func (p *MyserClient) sendMyif1(funparam1 int32, funparam2 string) (err error)  
          将输入参数封装成结构体,并发送
        * func (p *MyserClient) recvMyif1() (value *Mydata, err error)  
          读取调用结果,并封装成结构体
    * func (p *MyserClient) Myif2(funparam1 int32) (r int32, err error) 
        * func (p *MyserClient) sendMyif2(funparam1 int32) (err error)
        * func (p *MyserClient) recvMyif2() (value int32, err error) 

### 结构体 MyserProcessor
* 域
    * processorMap map[string]thrift.TProcessorFunction
    * handler      Myser
* 操作函数(<span style="color:red">这是服务端唯一需要关心的函数</span>)
     * func NewMyserProcessor(handler Myser) *MyserProcessor  
       用于生成结构体 MyserProcessor,初始化时,将接口函数全部加入到 processorMap 中
* 函数成员
    * func (p *MyserProcessor) AddToProcessorMap(key string, processor thrift.TProcessorFunction)  
    用于操作 processorMap ,添加
    * func (p *MyserProcessor) GetProcessorFunction(key string) (processor thrift.TProcessorFunction, ok bool)  
    用于操作 processorMap ,获取map值(其实是函数指针)
    * func (p *MyserProcessor) ProcessorMap() map[string]thrift.TProcessorFunction  
    用于操作 processorMap ,获取成员 processorMap
    * func (p *MyserProcessor) Process(iprot, oprot thrift.TProtocol) (success bool, err thrift.TException) 
        * 按传输协议读取接口名称
        * 从 processorMap 中查出接口名称对应的接口地址
        * 根据接口地址调用接口(实际调用的是`myserProcessorMyif1.Process || myserProcessorMyif.Process`)

### 结构体 myserProcessorMyif1
* handler Myser
* func (p *myserProcessorMyif1) Process(seqId int32, iprot, oprot thrift.TProtocol) (success bool, err thrift.TException)
    * 有异常要把异常抛到客户端
    * 调用 handler 内接口,将结果返回客户端

### 结构体 myserProcessorMyif2
* handler Myser
* func (p *myserProcessorMyif2) Process(seqId int32, iprot, oprot thrift.TProtocol) (success bool, err thrift.TException)
    * 有异常要把异常抛到客户端
    * 调用 handler 内接口,将结果返回客户端

### 辅助函数和辅助结构体
* 将每一个接口的参数封装成结构体
* 将每一个接口的返回值封装成结构体
