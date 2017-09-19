# Redigo Codis

Redigo Codis is a go client for codis based on Redigo.

## Features

- Use a round robin policy to balance load to multiple codis proxies.
- Detect proxy online and offline automatically.

## How to use

```go
import (
    "time"

    "github.com/garyburd/redigo/redis"
    "github.com/tranch-xiao/redigo-codis"
)

p := &codis.Pool{
    ZkServers: []string{"locallhost:8021"},
    ZkTimeout: time.Second * 60,
    ZkPath: "/codis/proxy",
    Dail: func(network, address string) (redis.Conn, error) {
        conn, err := redis.Dial(network, address)
        if err == nil {
            conn.Send("AUTH", "PASSWORD")
        }
        return conn, err
    },
}

r := p.Get()
r.Do("SET", "key", "value")
```
