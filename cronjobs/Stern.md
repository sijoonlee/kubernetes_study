## github
https://github.com/wercker/stern

## stern pingpong
```
pingpong-c849c8bbc-tgr7t pingpong 64 bytes from 1.1.1.1: seq=6386 ttl=127 time=22.533 ms
pingpong-c849c8bbc-8g8bp pingpong 64 bytes from 1.1.1.1: seq=3652 ttl=127 time=23.219 ms
pingpong-c849c8bbc-m4g8l pingpong 64 bytes from 1.1.1.1: seq=3651 ttl=127 time=31.029 ms
pingpong-c849c8bbc-zmvl9 pingpong 64 bytes from 1.1.1.1: seq=5856 ttl=127 time=26.371 ms
pingpong-c849c8bbc-g9hfc pingpong 64 bytes from 1.1.1.1: seq=6386 ttl=127 time=23.672 ms
pingpong-c849c8bbc-ph7xc pingpong-c849c8bbc-sv857pingpong  64 bytes from 1.1.1.1: seq=3652 ttl=127 time=104.235 ms
pingpong 64 bytes from 1.1.1.1: seq=3651 ttl=127 time=111.214 ms
pingpong-c849c8bbc-z7bvk pingpong 64 bytes from 1.1.1.1: seq=3653 ttl=127 time=93.118 ms
pingpong-c849c8bbc-tgr7t pingpong 64 bytes from 1.1.1.1: seq=6387 ttl=127 time=24.834 ms
pingpong-c849c8bbc-8g8bp pingpong 64 bytes from 1.1.1.1: seq=3653 ttl=127 time=25.733 ms
pingpong-c849c8bbc-m4g8l pingpong 64 bytes from 1.1.1.1: seq=3652 ttl=127 time=26.959 ms
```

## stern --tail 1 --timestamps
```
pingpong-c849c8bbc-m4g8l pingpong 2020-07-02T09:04:37.488442781-07:00 64 bytes from 1.1.1.1: seq=3731 ttl=127 time=27.258 ms
pingpong-c849c8bbc-zmvl9 pingpong 2020-07-02T09:04:37.717389784-07:00 64 bytes from 1.1.1.1: seq=5936 ttl=127 time=27.558 ms
pingpong-c849c8bbc-tgr7t pingpong 2020-07-02T09:04:38.093312142-07:00 64 bytes from 1.1.1.1: seq=6467 ttl=127 time=22.895 ms
pingpong-c849c8bbc-z7bvk pingpong 2020-07-02T09:04:37.98086868-07:00 64 bytes from 1.1.1.1: seq=3733 ttl=127 time=28.671 ms
pingpong-c849c8bbc-sv857 pingpong 2020-07-02T09:04:37.957322996-07:00 64 bytes from 1.1.1.1: seq=3731 ttl=127 time=25.102 ms
pingpong-c849c8bbc-8g8bp pingpong 2020-07-02T09:04:38.472019411-07:00 64 bytes from 1.1.1.1: seq=3733 ttl=127 time=31.975 ms
pingpong-c849c8bbc-m4g8l pingpong 2020-07-02T09:04:38.486829622-07:00 64 bytes from 1.1.1.1: seq=3732 ttl=127 time=25.108 ms
pingpong-c849c8bbc-zmvl9 pingpong 2020-07-02T09:04:38.714182067-07:00 64 bytes from 1.1.1.1: seq=5937 ttl=127 time=23.992 ms
pingpong-c849c8bbc-g9hfc pingpong 2020-07-02T09:04:38.807001628-07:00 64 bytes from 1.1.1.1: seq=6467 ttl=127 time=24.155 ms
```