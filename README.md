```go
package main

import (
	"fmt"
	"net"
	"sync"
	"time"
)

// 포트 스캔 함수
func scanPort(ip string, port int, wg *sync.WaitGroup) {
	defer wg.Done()

	address := fmt.Sprintf("%s:%d", ip, port)
	conn, err := net.DialTimeout("tcp", address, 1*time.Second)
	if err != nil {
		return // 포트가 닫혀있음
	}
	conn.Close()
	fmt.Printf("열려있는 포트 발견: %s\n", address)
}

func main() {
	var ip string
	var startPort, endPort int

	// 사용자 입력 받기
	fmt.Print("스캔할 IP 주소를 입력하세요: ")
	fmt.Scanln(&ip)
	fmt.Print("시작 포트를 입력하세요: ")
	fmt.Scanln(&startPort)
	fmt.Print("끝 포트를 입력하세요: ")
	fmt.Scanln(&endPort)

	var wg sync.WaitGroup

	// 포트 범위 스캔
	for port := startPort; port <= endPort; port++ {
		wg.Add(1)
		go scanPort(ip, port, &wg)
	}

	wg.Wait()
	fmt.Println("스캔 완료!")
}
```
