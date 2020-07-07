## What is DockerCoins
- generate a few random bytes
- hash these bytes
- increment a counter to keep track of speed
- repeat forever

## DockerCoins' Five Services
- rng: web service generating random bytes
- hasher: web service computing hash of POSTed data
- worker: background process calling rng and hasher
- webui: web interface to watch process
- redis: data store (holds a counter updated by worker)

## How Five Services work
- worker invokes rng to generate random bytes
- worker invokes hasher to hash these bytes
- worker in an infinite loop
- every second, worker updates redis to indicate how many loops were done
- webui queries redis, computes hashing speed and expose it on browser

Worker  -GET -> rng
        -POST-> hasher
        -TCP -> redis    <-TCP- webui <-GET- user

## How does each service find out the address of the other ones?
- we don't have hard-coded IP address / FQDNs
- we just connect to a service name - dynamic, embedded DNS server does the rest
- For example, 

redis = Redis("**redis**")
def get_random_bytes():
    r = requests.get("http://**rng**/32")
    return r.content
def hash_bytes(data):
    r = requests.post("http://**hasher**/",
                      data=data,
                      headers={"Content-Type": "application/octet-stream"})
