
kill all the ports from 3000 till ...

```
max_port=3004
max_port_int=${max_port: -1}
for ((port=3000; port<=3000+max_port_int; port++)); do
    pid=$(lsof -t -i:"$port")
    if [ -n "$pid" ]; then
        echo "Killing process $pid on port $port"
        kill -9 $pid
    fi
done
```


