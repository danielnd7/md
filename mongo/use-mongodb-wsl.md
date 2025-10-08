Great — here’s exactly how to **start, check, and stop MongoDB** correctly in **WSL (Ubuntu)** 👇

---

## 🟢 **1️⃣ Start MongoDB**

```bash
sudo service mongod start
```

If you want to see startup messages:

```bash
sudo tail -f /var/log/mongodb/mongod.log
```

You should see a line like:

```
[initandlisten] waiting for connections on port 27017
```

That means MongoDB is running ✅

---

## 🟡 **2️⃣ Check MongoDB status**

```bash
sudo service mongod status
```

Possible outputs:

* `active (running)` → ✅ working
* `inactive (dead)` or `failed` → ❌ stopped or error (check the log with the command above)

You can also verify by connecting:

```bash
mongosh
```

If it connects to `mongodb://127.0.0.1:27017/`, your server is active.

---

## 🔴 **3️⃣ Stop MongoDB**

```bash
sudo service mongod stop
```

To confirm it stopped:

```bash
sudo service mongod status
```

Now it should show `inactive (dead)`.

---

## ⚙️ **4️⃣ (Optional) Restart**

```bash
sudo service mongod restart
```

---

## 💡 **5️⃣ Bonus: check if it’s listening on port 27017**

```bash
ss -tlnp | grep 27017
```

or

```bash
netstat -tlnp | grep 27017
```

You’ll see something like:

```
LISTEN 0 128 127.0.0.1:27017 ...
```

---

✅ **Summary**

| Action          | Command                                    |
| --------------- | ------------------------------------------ |
| Start MongoDB   | `sudo service mongod start`                |
| Check status    | `sudo service mongod status`               |
| Stop MongoDB    | `sudo service mongod stop`                 |
| Restart MongoDB | `sudo service mongod restart`              |
| View logs       | `sudo tail -f /var/log/mongodb/mongod.log` |

