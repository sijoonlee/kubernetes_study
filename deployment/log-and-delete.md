## kubectl logs <pod name | type/name>

## kubectl logs deploy/pingpong
```
64 bytes from 1.1.1.1: seq=653 ttl=127 time=34.362 ms
64 bytes from 1.1.1.1: seq=654 ttl=127 time=23.144 ms
64 bytes from 1.1.1.1: seq=655 ttl=127 time=24.301 ms
64 bytes from 1.1.1.1: seq=656 ttl=127 time=25.765 ms
64 bytes from 1.1.1.1: seq=657 ttl=127 time=22.494 ms
64 bytes from 1.1.1.1: seq=658 ttl=127 time=22.178 ms
64 bytes from 1.1.1.1: seq=659 ttl=127 time=24.537 ms
64 bytes from 1.1.1.1: seq=660 ttl=127 time=22.987 ms
64 bytes from 1.1.1.1: seq=661 ttl=127 time=23.873 ms
64 bytes from 1.1.1.1: seq=662 ttl=127 time=31.450 ms
64 bytes from 1.1.1.1: seq=663 ttl=127 time=23.151 ms
```

## options
- -f/--follow : stream logs in real time
- --tail : indicate how many lines you want to see
- --since : to get logs only after a given timestamp

## kubectl logs deploy/pingpong --tail 1 --follow AND kubectl delete pod/pingpong-c849c8bbc-rsjnq
```
Found 3 pods, using pod/pingpong-c849c8bbc-rsjnq        // logs command can see only one pod at a time
64 bytes from 1.1.1.1: seq=1056 ttl=127 time=24.609 ms
64 bytes from 1.1.1.1: seq=1057 ttl=127 time=24.991 ms
64 bytes from 1.1.1.1: seq=1058 ttl=127 time=24.276 ms
64 bytes from 1.1.1.1: seq=1059 ttl=127 time=23.709 ms
64 bytes from 1.1.1.1: seq=1060 ttl=127 time=24.868 ms
64 bytes from 1.1.1.1: seq=1061 ttl=127 time=24.000 ms
....
64 bytes from 1.1.1.1: seq=1331 ttl=127 time=24.047 ms
64 bytes from 1.1.1.1: seq=1332 ttl=127 time=24.385 ms
rpc error: code = Unknown desc = an error occurred when try to find container // kubectl delete pod/pingpong-c849c8bbc-rsjnq
"ba27c69e3f43f35fdb25a04b1f637d4dc0215db1b283711a47e288092583d8a8": does not exist 
```

## kubectl get pods
```
NAME                       READY   STATUS        RESTARTS   AGE
pingpong-c849c8bbc-g9hfc   1/1     Running       0          9m15s
pingpong-c849c8bbc-rsjnq   1/1     Terminating   0          22m
pingpong-c849c8bbc-tgr7t   1/1     Running       0          9m15s
pingpong-c849c8bbc-zmvl9   1/1     Running       0          24s     // replica set will create new one
```

## kubectl delete pod
- it terminates the pod gracefully
    - sending TERM signal and wait
- as soon as the pod enters in "terminating" state, replica set replaces it
- After 30 seconds later (by default), the pod is killed