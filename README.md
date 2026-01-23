# Rate Limiter (Go)

![Status](https://img.shields.io/badge/status-In%20Progress-orange)  
![Language](https://img.shields.io/badge/language-Go-blue)

## Описание
Rate Limiter — это middleware для HTTP API, написанное на Go, которое ограничивает количество запросов от одного клиента за определённый промежуток времени.  
Цель проекта — изучить работу с **concurrency**, **middleware** и инфраструктурными сервисами в Go.

---

## Функциональность 
- Ограничение количества запросов по IP или по пользователю  
- Возврат HTTP 429 Too Many Requests при превышении лимита  
- Поддержка Redis для распределённого использования на нескольких инстансах  
- Настройка лимитов через конфигурацию (Requests per Time Window)  
- Unit-тесты для проверки корректной работы
- Поддержка хранения состояния лимитов в PostgreSQL (с возможностью будущего перехода на Redis)

---

## Стек технологий
- **Язык:** Go  
- **Сервер:** net/http  
- **Хранилище:** PostgreSQL (для учебного проекта; в будущем можно подключить Redis для высокой нагрузки) 
- **Тестирование:** Go testing package  

---

## План развития
1. Создать базовый middleware для Go HTTP-сервера  
2. Добавить поддержку лимитов по IP и по пользователю  
3. Настроить хранение состояния лимитов в **PostgreSQL** (для учебного проекта)  
4. Добавить unit-тесты и документацию  
5. В будущем: интегрировать **Redis** для распределённых сервисов и высокой нагрузки  
6. В будущем: добавить поддержку различных стратегий ограничения (fixed window, sliding window)

---

## Пример использования (планируемый)
```go
package main

import (
    "net/http"
    "time"
    "github.com/TheDarvion/rate-limiter-go/limiter"
)

func main() {
    // 10 запросов в минуту
    rl := limiter.NewRateLimiter(10, 1*time.Minute)

    http.Handle("/", rl.Middleware(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Hello, world!"))
    })))

    http.ListenAndServe(":8080", nil)
}
