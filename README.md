Fast int64 -> float64 hash in golang.

slightly modified from brenpt/intintmap to have float64 values instead

# intfloatmap

    import "github.com/glacialspring/intfloatmap"

Package intintmap is a fast int64 key -> float64 value map.

It interleaves keys and values in the same underlying array to improve locality.

It is 2-4X faster than the builtin map:
```
BenchmarkIntFloatMapFill                      10	 142930930 ns/op
BenchmarkStdMapFill                    	       5	 297418730 ns/op
BenchmarkIntFloatMapGet10PercentHitRate	    5000	    124437 ns/op
BenchmarkStdMapGet10PercentHitRate     	    5000	    143553 ns/op
BenchmarkIntFloatMapGet100PercentHitRate     500	   3843636 ns/op
BenchmarkStdMapGet100PercentHitRate    	     100	  13164908 ns/op
```

## Usage

```go
m := intfloatmap.New(32768, 0.6)
m.Put(int64(1234), float64(-222))
m.Put(int64(123), float64(33))

v, ok := m.Get(int64(222))
v, ok := m.Get(int64(333))

m.Del(int64(222))
m.Del(int64(333))

fmt.Println(m.Size())

for k := range m.Keys() {
    fmt.Printf("key: %d\n", k)
}

for kv := range m.Items() {
    fmt.Printf("key: %d, value: %g\n", kv[0], kv[1])
}
```

#### type Map

```go
type Map struct {
}
```

Map is a map-like data-structure for int64s

#### func  New

```go
func New(size int, fillFactor float64) *Map
```
New returns a map initialized with n spaces and uses the stated fillFactor. The
map will grow as needed.

#### func (*Map) Get

```go
func (m *Map) Get(key int64) (float64, bool)
```
Get returns the value if the key is found.

#### func (*Map) Put

```go
func (m *Map) Put(key int64, val float64)
```
Put adds or updates key with value val.

#### func (*Map) Del

```go
func (m *Map) Del(key int64)
```
Del deletes a key and its value.

#### func (*Map) Keys

```go
func (m *Map) Keys() chan int64
```
Keys returns a channel for iterating all keys.

#### func (*Map) Items

```go
func (m *Map) Items() chan KeyValuePair
```
Items returns a channel for iterating all key-value pairs.


#### func (*Map) Size

```go
func (m *Map) Size() int
```
Size returns size of the map.
