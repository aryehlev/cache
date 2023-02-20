# LRU

This is a Least Recently Used cache backed by a generic doubly linked list with O(1) time complexity.

# When to use
You would typically use an LRU cache when:

- Capacity of cache will hold nearly all data.
- Entries being used are being used on a consistent frequency.

Both above will prevent large amounts of data flapping in and out of the cache.
If your cache can only hold a fraction of values being stored or data seen on a cadence but high frequency, check out using the LFU cache instead.

## Usage
```go
package main

import (
	"fmt"
	"github.com/go-playground/cache/lru"
	"time"
)

func main() {
	cache := lru.New[string, string](100).MaxAge(time.Hour).HitFn(func(key string, value string) {
		fmt.Printf("Hit Key: %s Value %s\n", key, value)
	}).Build()
	cache.Set("a", "b")
	cache.Set("c", "d")

	option := cache.Get("a")
	if option.IsNone() {
		return
	}
	fmt.Println("result:", option.Unwrap())
}
```