# go-zk-fifo
a distributed fifo queue using golang and based on zookeeper

## Note
This project is based on zookeeper. Check out the zookeeper configuration <a href="http://zookeeper.apache.org/doc/r3.4.6/zookeeperStarted.html">here</a>.

## Installation
```
go get github.com/samuel/go-zookeeper
go get github.com/nladuo/go-zk-fifo   
```
## Example
### Basic
```go
package main

import (
    "fmt"
    "github.com/nladuo/go-zk-fifo/fifo"
)

var (
    hosts    []string = []string{"127.0.0.1:2181"} // the zk server list
    basePath string   = "/fifo"                    // the application znode, you can create it by your self
    fifoData []byte   = []byte("the fifo data")    // the data of application's znode
    prefix   string   = "seq-"                     // the fifo prefix
)

func main() {
    // create zk connection
    err := fifo.EstablishZkConn(hosts)
    if err != nil {
        panic(err)

    }
    //create the distributed fifo
    myfifo := fifo.NewFifo(basePath, fifoData, prefix)
    //put one data into fifo
    myfifo.Put([]byte("go-zk-fifo"))
    //get one data from fifo
    data := myfifo.Poll()
    fmt.Println(string(data))
}


```
### Producer And Consumer
<b>start the producer</b>
```shell
go run $GOPATH/src/github.com/nladuo/go-zk-fifo/test/producer.go
```
<b>start the consumer</b>
```shell
go run $GOPATH/src/github.com/nladuo/go-zk-fifo/test/consumer.go
```