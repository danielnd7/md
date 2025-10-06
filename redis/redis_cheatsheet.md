# Redis cheat sheet — commands, syntax, brief explanation & return values
*(Single Markdown file — ready to copy)*

---

## Table of contents
1. [Key operations & basic strings](#key-operations--basic-strings)  
2. [Atomic multi-key ops](#atomic-multi-key-ops)  
3. [Keys listing / deletion / expiration](#keys-listing--deletion--expiration)  
4. [Counters (INCR / DECR family)](#counters-incr--decr-family)  
5. [Lists (LPUSH / RPUSH / LPOP ...)](#lists-lpush--rpush--lpop--)  
6. [Hashes (HSET / HGET / HINCRBY ...)](#hashes-hset--hget--hincrby--)  
7. [Sets (SADD / SMEMBERS / SINTER ...)](#sets-sadd--smembers--sinter--)  
8. [Sorted sets (ZSET) — ZADD flags & commands](#sorted-sets-zset----zadd-flags--commands)  
9. [DB-level commands (FLUSHDB / FLUSHALL)](#db-level-commands-flushdb--flushall)  
10. [General notes & gotchas (ranges, negative indexes, patterns, performance)](#general-notes--gotchas-ranges-negative-indexes-patterns-performance)

---

## Key operations & basic strings

### `SET key value`
```
SET mykey "hello"
```
- **What it does:** Store `value` at `key`.
- **Returns:** `OK`.
- **Notes:** (Not shown here) `SET` supports options like `EX seconds`, `PX milliseconds`, `NX`, `XX` — but `SET` basic usage returns `OK`.

### `GET key`
```
GET mykey
```
- **What it does:** Retrieve the string value of `key`.
- **Returns:** The string value, or `nil` if key does not exist.

### `SETNX key value`
```
SETNX mykey "value"
```
- **What it does:** Set `key` to `value` *only if* key does **not** exist.
- **Returns:** `1` if the key was set, `0` if it was not (because it already existed).

### `GETSET key value`
```
GETSET mykey "newvalue"
```
- **What it does:** Atomically set `key` to `newvalue` and return the old value.
- **Returns:** Old value, or `nil` if key did not exist.

---

## Atomic multi-key ops

### `MSET key1 value1 [key2 value2 ...]`
```
MSET k1 "v1" k2 "v2"
```
- **What it does:** Set multiple keys in one atomic operation.
- **Returns:** `OK`.

### `MGET key1 [key2 ...]`
```
MGET k1 k2 k3
```
- **What it does:** Get values for multiple keys in one call.
- **Returns:** Array of values (for missing keys, `nil` placeholders are returned in the corresponding positions).

---

## Keys listing / deletion / expiration

### `KEYS pattern`
```
KEYS user:*
```
- **What it does:** Return all keys matching `pattern`.
- **Returns:** Array of matching keys (strings).
- **Pattern examples:**
  - `h?llo` → matches `hello`, `hallo`, `hxllo` (exactly one char)
  - `h*llo` → matches `hllo`, `heeeello` (0..n chars)
  - `h[ae]llo` → matches `hello`, `hallo` (one of the bracket chars)
  - `h[^e]llo` → matches `hallo`, `hbllo`, but **not** `hello`
  - `h[a-b]llo` → matches `hallo`, `hbllo` (range)
- **Warning:** `KEYS` is **O(n)** over the keyspace — avoid in production. Use `SCAN` for incremental, non-blocking iteration.

### `DEL key [key2 ...]`
```
DEL k1 k2
```
- **What it does:** Delete given keys.
- **Returns:** Integer — number of keys removed.

### `EXPIRE key seconds`
```
EXPIRE mykey 3600
set mykey "val" EX 120
```
- **What it does:** Set a timeout (in seconds) on `key`.
- **Returns:** `1` if timeout set successfully, `0` if key does not exist.

### `TTL key`
```
TTL mykey
```
- **What it does:** Get remaining time to live (in seconds) for `key`.
- **Returns:**  
  - `>0` : seconds left  
  - `-1` : key exists but has **no** associated expire  
  - `-2` : key does **not** exist

---

## Counters (INCR / DECR family)

### `INCR key`
```
INCR counter
```
- **What it does:** Increment integer value stored at `key` by 1 (key is set to `0` before increment if it did not exist).
- **Returns:** New value as integer.

### `INCRBY key increment`
```
INCRBY counter 10
```
- **What it does:** Increment by `increment`.
- **Returns:** New integer value.

### `INCRBYFLOAT key increment`
```
INCRBYFLOAT price 1.5
```
- **What it does:** Increment floating value; useful for non-integers.
- **Returns:** New value as string representation of the float.

### `DECR` / `DECRBY`
- **Analogous** to `INCR` / `INCRBY` but decrement.

### `EXISTS key [key2 ...]`
```
EXISTS k1 k2
```
- **What it does:** Check whether keys exist.
- **Returns:** Integer — number of keys that exist (for a single key, `1` or `0`).

---

## Lists (LPUSH / RPUSH / LPOP ...)

### `LPUSH key element [element ...]`
```
LPUSH mylist "a" "b"
```
- **What it does:** Push elements to **head** of the list (left).
- **Returns:** Integer — the length of the list after push.

### `RPUSH key element [element ...]`
```
RPUSH mylist "x"
```
- **What it does:** Push elements to **tail** (right).
- **Returns:** New length.

### `LLEN key`
```
LLEN mylist
```
- **What it does:** Get length of the list.
- **Returns:** Integer length.

### `LINDEX key index`
```
LINDEX mylist 0
LINDEX mylist -1
```
- **What it does:** Get element by index.
- **Indexes:** `0` is head, increasing positive to tail; negative indexes allowed (`-1` last element, `-2` second-to-last, ...).
- **Returns:** Element or `nil` if index out of range.

### `LRANGE key start stop`
```
LRANGE mylist 0 -1
```
- **What it does:** Return sublist from `start` to `stop` **inclusive**.
- **Common:** `0 -1` returns the whole list.
- **Returns:** Array of elements.

### `LPOP` / `RPOP`
```
LPOP mylist
RPOP mylist
```
- **What it does:** Pop and return element from head or tail.
- **Returns:** The popped element, or `nil` if list is empty.

---

## Hashes (HSET / HGET / HGETALL ...)

### `HSET key field value`
```
HSET user:1 name "Alice"
```
- **What it does:** Set field in hash stored at key.
- **Returns:** `1` if field is new and value set, `0` if field existed and value was updated (modern Redis `HSET` may return integer count of fields added).

### `HGET key field`
```
HGET user:1 name
```
- **What it does:** Get value of a hash field.
- **Returns:** Value or `nil` if field/key does not exist.

### `HGETALL key`
```
HGETALL user:1
```
- **What it does:** Return all fields and values of the hash.
- **Returns:** Array (field1, value1, field2, value2, ...). Many client libraries will map this to a dictionary/object.

### `HDEL key field [field ...]`
- **What it does:** Delete one or more fields.
- **Returns:** Integer — number of fields removed.

### `HINCRBY key field increment`
```
HINCRBY user:1 visits 1
```
- **What it does:** Increment integer field value by `increment`.
- **Returns:** New value as integer.

### `HINCRBYFLOAT` — analogous but for floats.

### `HEXISTS key field` / `HKEYS key` / `HVALS key`
- `HEXISTS` → `1` if field exists, else `0`.  
- `HKEYS` → list of fields.  
- `HVALS` → list of values.

---

## Sets (unordered)

### `SADD key member [member ...]`
```
SADD myset "a" "b"
```
- **What it does:** Add one or more members to set.
- **Returns:** Integer — number of elements added (not counting already existing members).

### `SMEMBERS key`
- **What it does:** Return all members of the set.
- **Returns:** Array (unordered).

### `SISMEMBER key member`
- **What it does:** Check membership.
- **Returns:** `1` if member exists, otherwise `0`.

### `SREM key member [member ...]`
- **What it does:** Remove members.
- **Returns:** Integer — number of members removed.

### Set operations
- `SINTER key1 [key2 ...]` — intersection, returns set of members present in **all** sets.  
- `SUNION key1 [key2 ...]` — union.  
- `SDIFF key1 [key2 ...]` — difference (members in key1 not in others).  
- **Returns:** Arrays of members.

### `SRANDMEMBER key [count]`
```
SRANDMEMBER myset
SRANDMEMBER myset 3
SRANDMEMBER myset -5
```
- **What it does:** Return one or more random members.
- **Behavior of `count`:**
  - If omitted: returns a single random member (or `nil` if set empty).
  - If `count > 0`: return up to `count` **unique** elements (no repetitions). If `count` > set size, you get the whole set (unique).
  - If `count < 0`: return `abs(count)` elements *allowing duplicates* (members can repeat).

---

## Sorted Sets (ZSET) — commands & ZADD flags explained

Sorted sets store members with an associated floating-point score and keep members ordered by score.

### `ZADD key [NX|XX] [GT|LT] [CH] [INCR] score member [score member ...]`
```
ZADD leaderboard 100 "alice" 95 "bob"
```
**Flags explained:**
- `NX` — Only add new elements. Do not update existing members. (Skip members that already exist.)
- `XX` — Only update existing elements. Do not add new members.
- `GT` — Only update existing elements if the new score is **greater** than the current one.
- `LT` — Only update existing elements if the new score is **less** than the current one.
- `CH` — Modify return: with `CH` Redis returns the number of elements **that were changed** (added or updated). Without `CH` it returns the number of **new** elements added.
- `INCR` — When specified, increment the score of `member` by `score` and **return the new score**. With `INCR` you can only operate on a single `score member` pair. (Return value: new score as string/number.)

**Return values (typical):**
- Normally `ZADD` returns integer — number of elements **added** (not updated).  
- With `CH`, it returns number of elements **changed** (added + updated).  
- With `INCR`, returns the new score (as number).

---

### `ZCOUNT key min max`
```
ZCOUNT leaderboard 50 100
ZCOUNT leaderboard -inf +inf
```
- **What it does:** Count members with score between `min` and `max`.
- **Notes:** `min`/`max` can be `-inf` / `+inf`. To make a bound exclusive, prefix with `(`, e.g. `ZCOUNT z (5 10` counts scores `>5` and `<=10`.
- **Returns:** Integer — count.

### `ZRANGE key start stop [WITHSCORES] [LIMIT offset count]`
```
ZRANGE leaderboard 0 -1
ZRANGE leaderboard 0 9 WITHSCORES
ZRANGE leaderboard 0 -1 WITHSCORES LIMIT 0 10
```
- **What it does:** Return members in index range `[start, stop]` (scores ascending).
- **Indexes:** Negative indexes allowed (`-1` = last). `start` and `stop` are **inclusive**.
- **WITHSCORES:** return member-score pairs.
- **LIMIT offset count:** paginate the results (applies counting after selecting range).

### `ZREVRANGE key start stop [WITHSCORES]`
- Same as `ZRANGE` but in **descending** order (highest score first).

### `ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`
```
ZRANGEBYSCORE leaderboard 50 100 WITHSCORES LIMIT 0 10
ZRANGEBYSCORE leaderboard -inf +inf
ZRANGEBYSCORE leaderboard (50 100   # >50 and <=100
```
- **What it does:** Return members with scores between `min` and `max` (inclusive unless `(` used to make exclusive).
- **WITHSCORES, LIMIT** behave same as above.
- **Returns:** Array of members (or member-score pairs with `WITHSCORES`).

### `ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]`
- Same as `ZRANGEBYSCORE` but ordered high→low.

### `ZREM key member [member ...]`
- **What it does:** Remove one or more members from zset.
- **Returns:** Integer — number of members removed.

---

## DB-level commands

### `FLUSHDB`
```
FLUSHDB
```
- **What it does:** Remove all keys from the currently selected database.
- **Returns:** `OK`.
- **Be careful:** destructive; in production often requires confirmation or is disabled.

### `FLUSHALL`
```
FLUSHALL
```
- **What it does:** Remove all keys from **all** databases on the Redis server.
- **Returns:** `OK`.
- **Warning:** Highly destructive.

---

## General notes & gotchas (ranges, negative indexes, patterns, performance)

- **Ranges & extremes (`-inf`, `+inf`):**  
  - Sorted-set score ranges accept `-inf` and `+inf`.  
  - To make bounds **exclusive**, prefix with `(`, e.g. `ZRANGEBYSCORE z (5 10` means `5 < score <= 10`.
- **Negative indexes:**  
  - For list and `ZRANGE` index-based commands, negative numbers count from the end: `-1` = last element, `-2` = second-to-last, etc. For `LRANGE 0 -1` you get the entire list.
- **Return types:**  
  - Many commands return integers (counts), strings (values), arrays (lists of elements), or `nil` when things don’t exist. CLI prints `nil` for missing values.
- **Atomicity:**  
  - Single Redis commands are atomic. `MSET` is atomic across multiple keys.
- **Performance warning:**  
  - `KEYS` is O(N) and blocks; don't run on large production datasets. Use `SCAN` for incremental scanning.
- **`EXISTS` accepts multiple keys:** modern `EXISTS k1 k2` returns the count of keys that exist (not strictly 0/1 if you pass many keys).
- **`ZADD` flags interactions:** `NX` and `XX` are mutually exclusive. `GT` and `LT` only make sense for updates (they won't add new members when there is no existing score unless behavior with `XX`/`NX` modifies that; typically `GT`/`LT` guard update conditions).
- **`SRANDMEMBER` `count` semantics:** positive = unique up to set size; negative = allow repeats and return exactly `abs(count)` items.
- **Expiry precision:** `EXPIRE` is in seconds; `PEXPIRE` exists for milliseconds. `TTL` returns seconds; `PTTL` returns milliseconds.

---

## Small examples

```redis
# Strings
SET greeting "hello"          # -> OK
GET greeting                 # -> "hello"

# Counters
INCR visits                  # -> (integer) 1
INCRBY visits 5              # -> (integer) 6

# Lists
LPUSH q "task1" "task2"      # -> length after push
LRANGE q 0 -1                # -> ["task2","task1"]

# Hashes
HSET user:1 name "Alice"     # -> 1 (field added)
HGETALL user:1               # -> ["name","Alice"]

# Sets
SADD tags "redis" "db"       # -> number added
SRANDMEMBER tags 2           # -> up to 2 unique tags

# Sorted sets
ZADD leaderboard 150 "alice" 140 "bob"
ZRANGE leaderboard 0 -1 WITHSCORES
ZCOUNT leaderboard (100 +inf  # -> count scores >100

# Key management
EXPIRE session:1 3600        # -> 1
TTL session:1                # -> 3599 (or -1/-2)
```

---

If you want, I can:
- Convert this into a printer-friendly one-page PDF.  
- Add a small quick-reference table with return types and complexity (O(1), O(N), etc.).  
Tell me which extra format or additions you'd like and I’ll include them straight into the same single document.

