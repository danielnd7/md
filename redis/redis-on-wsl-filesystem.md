When you install **Redis** with `sudo apt install redis-server` inside WSL (Ubuntu/Debian), several files and directories are placed in your Linux filesystem. Here’s the breakdown:

---

## 📂 Main Redis components installed

### 1. **Binaries (executables)**

Located in:

```
/usr/bin/
```

* `redis-server` → the main Redis server process
* `redis-cli` → command-line client to interact with Redis
* `redis-benchmark` → benchmarking tool
* `redis-check-aof`, `redis-check-rdb` → utilities to check/repair persistence files

---

### 2. **Configuration files**

Located in:

```
/etc/redis/
```

* `redis.conf` → the main configuration file you edit (bind address, ports, persistence, etc.)

---

### 3. **Systemd / Service files**

Located in:

```
/lib/systemd/system/redis-server.service
```

or sometimes symlinked to:

```
/etc/systemd/system/redis.service
```

These define how `systemctl` or `service redis-server start` controls the daemon.

---

### 4. **Data files**

Default directory:

```
/var/lib/redis/
```

* Where Redis stores database snapshots (`dump.rdb`) or append-only files (`appendonly.aof`) if persistence is enabled.

---

### 5. **Logs**

Default location:

```
/var/log/redis/redis-server.log
```

---

### 6. **Runtime / PID files**

Temporary files for the running process:

```
/var/run/redis/
```

* Example: `redis-server.pid` (stores the process ID).

---

✅ So in short, installing Redis drops files into:

* **/usr/bin/** → executables
* **/etc/redis/** → configs
* **/var/lib/redis/** → persistent data
* **/var/log/redis/** → logs
* **/var/run/redis/** → PID/runtime info
* **/lib/systemd/system/** → service definition

---

