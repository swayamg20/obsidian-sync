Normal — C++ map syntax is verbose. Here's a cheat sheet to bookmark:

---

**Declaration**

```cpp
unordered_map<int, int> mp;           // basic
unordered_map<string, vector<int>> mp; // value is a vector
unordered_map<string, vector<string>> mp; // both string
```

**Insert / Update**

```cpp
mp[key] = val;          // insert or overwrite
mp[key]++;              // increment (auto-creates with 0 if not exists)
mp[key].push_back(x);   // if value is vector, auto-creates empty vector
mp.insert({key, val});  // only inserts if key doesn't exist
```

**Access**

```cpp
mp[key];                // get value (creates default if not exists ⚠️)
mp.at(key);             // get value (throws if not exists, safer)
```

**Check existence**

```cpp
mp.count(key);          // 1 or 0 — cleanest
mp.find(key) != mp.end(); // when you need iterator too
mp.contains(key);       // C++20 only
```

**Delete**

```cpp
mp.erase(key);
```

**Iterate**

```cpp
for (auto& [key, val] : mp) { }   // structured binding — use this always
for (auto& p : mp) { p.first; p.second; } // older style
```

**Size / Empty**

```cpp
mp.size();
mp.empty();
```

---

**Mental model — just remember these 4 things:**

1. `mp[key]` — access/insert
2. `mp.count(key)` — check exists
3. `mp.erase(key)` — delete
4. `auto& [k, v]` — iterate

Everything else is a variation of these.