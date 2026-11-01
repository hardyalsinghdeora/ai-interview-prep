# Networking and Web Cheatsheet

Core terms for how data moves across networks and the web, from protocol layers up to load balancers and browser security rules.

> Related: [system-design.md](system-design.md) · [distributed-systems.md](distributed-systems.md) · [security-and-auth.md](security-and-auth.md) · [api-design.md](api-design.md) · [CS fundamentals topic](../topics/13-cs-fundamentals/README.md) · [Debugging topic](../topics/12-debugging-troubleshooting/README.md) · [API design skill](../skills/api-design-and-microservices.md)

## Layers and addressing

| Term | What it means |
|---|---|
| **OSI model** (Open Systems Interconnection) | Seven-layer reference model for networking. Gives a shared vocabulary for locating problems, though real stacks rarely map to it exactly. |
| **TCP/IP model** (Transmission Control Protocol/Internet Protocol) | Four-layer model the internet actually uses: link, internet, transport, application. Simpler and closer to real implementations than OSI. |
| **Encapsulation** | Each layer wraps the data from the layer above with its own header. Explains frames, packets and segments, and protocol overhead. |
| **IP** (Internet Protocol) | Routes packets between networks by address, with best-effort delivery. Offers no delivery or ordering guarantees, so higher layers add them. |
| **IPv4 and IPv6** (Internet Protocol versions 4 and 6) | 32-bit and 128-bit address formats. IPv4 exhaustion drove widespread address sharing through NAT and the gradual move to IPv6. |
| **CIDR** (Classless Inter-Domain Routing) | Notation such as `10.0.0.0/16`, where the suffix gives the network prefix length. Used to size subnets and write firewall rules. |
| **NAT** (Network Address Translation) | Rewrites addresses at a router so many private hosts share one public address. Saves addresses but complicates inbound and peer-to-peer connections. |
| **Port** | 16-bit number identifying a service on a host, such as 443 for HTTPS. Lets one address serve many applications. |
| **Socket** | Operating system endpoint for network communication. A TCP connection is identified by source and destination addresses and ports, known as the 4-tuple. |
| **MTU** (Maximum Transmission Unit) | Largest packet a link can carry, commonly 1500 bytes on Ethernet. Mismatches cause fragmentation or mysterious stalls on large transfers. |
| **RTT** (Round-Trip Time) | Time for a signal to reach a destination and return. Dominates web latency, because every handshake costs at least one round trip. |
| **ICMP** (Internet Control Message Protocol) | Carries network diagnostics and errors. Powers `ping` and `traceroute`; blocking it can break path MTU discovery. |
| **ARP** (Address Resolution Protocol) | Maps an IPv4 address to a hardware address on the local network. Required before a host can send a frame on its link. |
| **BGP** (Border Gateway Protocol) | Routing protocol that exchanges reachability between networks on the internet. Misconfigurations can take large services offline. |

| OSI layer | TCP/IP layer | Examples |
|---|---|---|
| 7 Application | Application | HTTP, DNS, SMTP |
| 6 Presentation | Application | Encoding, compression, roughly TLS |
| 5 Session | Application | Session handling, roughly TLS |
| 4 Transport | Transport | TCP, UDP, QUIC (runs over UDP) |
| 3 Network | Internet | IP, ICMP |
| 2 Data link | Link | Ethernet, Wi-Fi, ARP |
| 1 Physical | Link | Cable, fibre, radio |

## TCP and UDP

| Term | What it means |
|---|---|
| **TCP** (Transmission Control Protocol) | Connection-oriented transport giving reliable, ordered byte streams. The default choice when losing data is unacceptable. |
| **UDP** (User Datagram Protocol) | Connectionless transport sending independent datagrams with no delivery or ordering guarantee. Low overhead suits real-time media, DNS and QUIC. |
| **Three-way handshake** | SYN, SYN-ACK, ACK exchange that opens a TCP connection and agrees sequence numbers. Adds one round trip before data flows. |
| **Connection close** (FIN, Finish) | Each side sends FIN and receives an acknowledgement, so each direction closes independently. An RST (Reset) aborts the connection immediately instead. |
| **`TIME_WAIT`** | State held by the side that closes first, so late packets cannot corrupt a new connection. Many short connections can exhaust ephemeral ports. |
| **Retransmission** | TCP resends data that was not acknowledged, after a timeout or duplicate acknowledgements. Provides reliability but causes latency spikes under packet loss. |
| **Flow control** | Receiver advertises a window limiting unacknowledged data. Stops a fast sender overwhelming a slow receiver. |
| **Congestion control** | Sender adjusts its rate to protect the network, using algorithms such as CUBIC and BBR. Determines throughput on lossy or long-distance links. |
| **Slow start** | New connections begin with a small congestion window that grows quickly. Reusing connections avoids paying this ramp-up cost again. |
| **Nagle's algorithm** | Batches small TCP writes to reduce tiny packets. Can add latency, so interactive systems often disable it with `TCP_NODELAY`. |
| **Head-of-line blocking** | One lost or slow item holds up everything queued behind it. Motivated HTTP/2 multiplexing and then HTTP/3 over QUIC. |
| **SYN flood** (Synchronise) | Attack sending many handshake openers to exhaust half-open connection state. SYN cookies mitigate it by storing no state until completion. |

| TCP vs UDP | TCP | UDP |
|---|---|---|
| Connection | Handshake required | None |
| Reliability | Retransmits and acknowledges | None built in |
| Ordering | Guaranteed | Not guaranteed |
| Data model | Byte stream | Discrete datagrams |
| Rate control | Flow and congestion control | None built in |
| Typical use | Web, databases, file transfer | Voice, video, DNS, QUIC |

## HTTP

| Term | What it means |
|---|---|
| **HTTP** (Hypertext Transfer Protocol) | Stateless request and response protocol of the web. Statelessness simplifies scaling, with cookies or tokens carrying any session context. |
| **HTTPS** (Hypertext Transfer Protocol Secure) | HTTP protected by TLS. Provides encryption, integrity and server authentication, and is the expected default for public sites. |
| **QUIC** (a name; originally Quick UDP Internet Connections) | Encrypted, multiplexed transport over UDP. Avoids TCP head-of-line blocking and supports connection migration when a client changes network. |
| **Multiplexing** | Carrying many concurrent request streams over one connection. Removes the need for many parallel connections per host. |
| **Header compression** | HTTP/2 uses HPACK and HTTP/3 uses QPACK to shrink repetitive headers. Saves bandwidth on chatty APIs and cookie-heavy sites. |
| **Safe method** | A method that should not change server state, such as `GET`, `HEAD` or `OPTIONS`. Lets caches and crawlers call it freely. |
| **Idempotent method** | Repeating the request has the same effect as sending it once: `GET`, `PUT`, `DELETE`. Makes retries safe; `POST` is not idempotent by default. |
| **Cookie** | Small value the server sets and the browser returns automatically. Flags such as `HttpOnly`, `Secure` and `SameSite` limit theft and misuse. |
| **`Cache-Control`** | Header stating who may cache a response and for how long, for example `max-age` or `no-store`. Controls browser and CDN behaviour. |
| **ETag** (Entity Tag) | Version identifier for a response. Clients revalidate with `If-None-Match` and may receive `304`, saving a full download. |

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Transport | TCP | TCP | QUIC over UDP |
| Format | Text | Binary frames | Binary frames |
| Concurrency | One request at a time per connection | Many streams per connection | Many independent streams |
| Head-of-line blocking | Request and TCP level | TCP level only | Affects only the stream with loss |
| Header compression | None | HPACK | QPACK |
| Encryption | Optional | Optional in spec, required by browsers | Always, with TLS 1.3 |

| Status | Name | Use it to mean |
|---|---|---|
| `101` | Switching Protocols | Upgrade accepted, for example to WebSocket |
| `200` | OK | Success with a body |
| `201` | Created | Resource created |
| `202` | Accepted | Queued for asynchronous processing |
| `204` | No Content | Success with no body |
| `206` | Partial Content | Range request, for resumable downloads |
| `301` / `308` | Moved Permanently / Permanent Redirect | Permanent move; `308` keeps the method |
| `302` / `307` | Found / Temporary Redirect | Temporary move; `307` keeps the method |
| `304` | Not Modified | Cached copy is still valid |
| `400` | Bad Request | Malformed request |
| `401` | Unauthorized | Missing or invalid authentication |
| `403` | Forbidden | Authenticated identity is not allowed |
| `404` | Not Found | No such resource |
| `409` | Conflict | Clashes with current state |
| `412` | Precondition Failed | `If-Match` or similar check failed |
| `422` | Unprocessable Content | Well-formed but semantically invalid |
| `429` | Too Many Requests | Rate limited; often sent with `Retry-After` |
| `500` | Internal Server Error | Unhandled server fault |
| `502` | Bad Gateway | Proxy received a bad upstream response |
| `503` | Service Unavailable | Overloaded or down for maintenance |
| `504` | Gateway Timeout | Proxy timed out waiting for upstream |

## TLS

| Term | What it means |
|---|---|
| **TLS** (Transport Layer Security) | Protocol providing encryption, integrity and authentication over a transport. Successor to SSL (Secure Sockets Layer), whose name still lingers. |
| **TLS handshake** (Transport Layer Security) | Negotiates versions and ciphers, authenticates the server and derives session keys. TLS 1.3 takes one round trip; TLS 1.2 takes two. |
| **0-RTT** (Zero Round-Trip Time) | TLS 1.3 resumption that sends data in the first flight. Faster, but replayable, so limit it to idempotent requests. |
| **Symmetric and asymmetric encryption** | Asymmetric key pairs authenticate and establish secrets; fast symmetric keys then encrypt the traffic. TLS uses both for security and speed. |
| **Certificate** | Signed document binding a public key to a domain name. Lets clients verify they reached the real server. |
| **CA** (Certificate Authority) | Trusted organisation that signs certificates. Clients trust a site by following its chain up to a root CA in their trust store. |
| **Certificate chain** | Leaf certificate, intermediates, then root. Servers must send the intermediates, or some clients fail validation. |
| **mTLS** (Mutual Transport Layer Security) | Client and server both present certificates. Common for service-to-service identity inside zero-trust networks and service meshes. |
| **SNI** (Server Name Indication) | Client names the target host during the handshake. Lets one address serve many certificates, but is visible unless encrypted. |
| **ECH** (Encrypted Client Hello) | Encrypts sensitive handshake fields, including the server name, from on-path observers. Deployment is still partial *(verify)*. |
| **ALPN** (Application-Layer Protocol Negotiation) | TLS extension where client and server agree the application protocol, such as `h2`. Avoids an extra round trip. |
| **Forward secrecy** | Ephemeral key exchange means a later private-key leak cannot decrypt recorded traffic. Mandatory in TLS 1.3. |
| **HSTS** (Hypertext Transfer Protocol Strict Transport Security) | Response header telling browsers to use only HTTPS for a domain. Blocks downgrade attacks after the first visit. |
| **TLS termination** (Transport Layer Security) | Decrypting traffic at a proxy or load balancer. Centralises certificates and enables layer 7 routing, but backend traffic needs separate protection. |

## DNS

| Term | What it means |
|---|---|
| **DNS** (Domain Name System) | Distributed hierarchical database mapping names to records such as addresses. A frequent cause of outages because almost everything depends on it. |
| **Recursive resolver** | Server that does the lookup work for clients and caches answers. Usually run by an ISP (Internet Service Provider), cloud or public DNS provider. |
| **Authoritative name server** | Server holding the definitive records for a zone. Where changes are made, though caches may serve old answers. |
| **Root and TLD servers** (Top-Level Domain) | Upper levels of the hierarchy. Root points to TLD servers such as `.com`, which point to a domain's authoritative servers. |
| **TTL** (Time To Live) | How long a record may be cached. Short values speed failover and migration but increase query load. |
| **A record** (Address) | Maps a name to an IPv4 address. The most basic record for reaching a host. |
| **AAAA record** (Internet Protocol version 6 address) | Maps a name to an IPv6 address, the 128-bit equivalent of an A record. |
| **CNAME record** (Canonical Name) | Aliases one name to another. Cannot coexist with other records at the same name, so plain CNAMEs do not work at the zone apex. |
| **MX record** (Mail Exchange) | Names the mail servers for a domain, with priorities. Wrong values silently break inbound email. |
| **TXT record** (Text) | Holds free-form text, used for domain ownership checks and email authentication such as SPF and DKIM. |
| **NS record** (Name Server) | Delegates a zone to its authoritative servers. Changed when moving DNS providers. |
| **Anycast** | The same address is announced from many locations, and routing picks a nearby one. Used by DNS and CDNs for latency and resilience. |
| **DoH and DoT** (Domain Name System over Hypertext Transfer Protocol Secure, and over Transport Layer Security) | Encrypt DNS queries between client and resolver. Improve privacy, though they can bypass network-level filtering. |

```text
Lookup: browser/OS cache -> recursive resolver -> root -> TLD -> authoritative -> answer cached for TTL
```

## CDN

| Term | What it means |
|---|---|
| **CDN** (Content Delivery Network) | Geographically distributed caches that serve content near users. Cuts latency and origin load, and absorbs traffic spikes and some attacks. |
| **PoP** (Point of Presence) | A CDN edge location. More well-connected locations generally mean lower latency for nearby users. |
| **Origin server** | The authoritative server behind the CDN. Receives requests on a cache miss, so it must survive miss storms. |
| **Origin shield** | Extra cache layer between the edge and origin. Collapses misses from many locations, protecting the origin during purges. |
| **Cache key** | The request attributes that identify a cached object, typically URL plus selected headers. Too broad cuts hit ratio; too narrow risks serving wrong content. |
| **Cache hit ratio** | Share of requests served from cache. The main CDN health metric, because small drops can multiply origin load. |
| **Purge** | Removing objects from CDN caches before expiry. Versioned file names often avoid needing it for static assets. |
| **`stale-while-revalidate`** | `Cache-Control` directive that serves a stale copy while refreshing in the background. Keeps latency low during revalidation. |

## Load balancing

| Term | What it means |
|---|---|
| **Layer 4 load balancer** (L4) | Routes on addresses and ports without reading the payload. Fast and protocol-agnostic, but cannot route by path or header. |
| **Layer 7 load balancer** (L7) | Understands application protocols such as HTTP. Enables path routing, retries and header logic, at higher processing cost. |
| **Round robin** | Sends each request to the next server in turn. Simple and effective when servers and requests are similar. |
| **Least connections** | Picks the server with the fewest active connections. Better than round robin when request durations vary widely. |
| **Consistent hashing** | Places keys and servers on a ring, so adding a server moves only a fraction of keys. Preserves cache affinity. |
| **Health check** | Periodic probe that removes failing backends from rotation. Shallow checks miss broken dependencies; deep checks can spread failures. |
| **Sticky session** | Keeps sending a client to the same backend, by cookie or hash. Eases stateful apps but skews load and complicates failover. |
| **Connection draining** | Stops new traffic to a backend while in-flight requests finish. Essential for zero-downtime deployments. |
| **DSR** (Direct Server Return) | Backends reply straight to clients, bypassing the balancer on the way back. Saves capacity when responses are much larger than requests. |
| **GSLB** (Global Server Load Balancing) | Steers users between regions, usually through DNS or anycast. Supports latency-based routing and regional failover. |

| L4 vs L7 | L4 | L7 |
|---|---|---|
| Sees | Addresses, ports | Paths, headers, cookies |
| Speed | Faster | More processing per request |
| TLS | Usually passes through | Usually terminates |
| Use for | Any TCP or UDP traffic, very high throughput | HTTP routing, canaries, authentication at the edge |

## Proxies

| Term | What it means |
|---|---|
| **Forward proxy** | Acts for clients when reaching the internet. Used for egress control, filtering and hiding client addresses. |
| **Reverse proxy** | Sits in front of servers and receives client traffic. Provides routing, TLS termination, caching and a shield for backends. |
| **API gateway** (Application Programming Interface) | Reverse proxy for APIs that centralises authentication, rate limiting and routing, so individual services need not duplicate them. |
| **Sidecar proxy** | Proxy deployed beside each service instance. Moves retries, mTLS and telemetry out of application code. |
| **`X-Forwarded-For`** | Header listing the client and proxy chain. Clients can spoof it, so trust only entries added by your own proxies. |

## WebSockets and real-time

| Term | What it means |
|---|---|
| **WebSocket** | Persistent, full-duplex connection over a single TCP connection. Suits chat, collaboration and live data needing low-latency traffic both ways. |
| **Upgrade handshake** | WebSocket starts as an HTTP request with `Upgrade: websocket` and the server replies `101`. This helps it pass through existing web infrastructure. |
| **Ping and pong frames** | Control frames that confirm a WebSocket is alive. Detect dead peers and stop idle timeouts on proxies. |
| **SSE** (Server-Sent Events) | One-way server-to-browser stream over ordinary HTTP. Includes automatic reconnection; simpler than WebSockets when clients rarely send. |
| **Long polling** | Client request stays open until data arrives, then the client immediately asks again. A fallback that works almost anywhere, with higher overhead. |

| Option | Direction | Connection | Best for |
|---|---|---|---|
| Short polling | Client pull | Repeated requests | Infrequent updates, simplicity |
| Long polling | Client pull, held open | Repeated long requests | Legacy-friendly near-real-time |
| SSE | Server to client | One long-lived HTTP stream | Feeds, progress, streamed tokens |
| WebSocket | Both ways | One upgraded connection | Chat, games, collaboration |

## CORS

| Term | What it means |
|---|---|
| **Same-origin policy** | Browser rule stopping scripts reading responses from another origin. Protects logged-in data from malicious sites. |
| **Origin** | The combination of scheme, host and port. A difference in any one makes it a different origin. |
| **CORS** (Cross-Origin Resource Sharing) | Headers letting a server relax the same-origin policy for chosen origins. Enforced by browsers, so it does not protect against non-browser clients. |
| **Simple request** | A `GET`, `HEAD` or `POST` with only safelisted headers and content types. Sent without preflight, so it can still reach the server. |
| **Preflight request** | `OPTIONS` request the browser sends first for other cross-origin requests, checking permission. Caching it with `Access-Control-Max-Age` removes repeated round trips. |
| **`Access-Control-Allow-Origin`** | Response header naming the permitted origin, or `*` for any. Without a match, the browser hides the response from scripts. |
| **Credentialed request** | Cross-origin request that sends cookies or similar. Requires `Access-Control-Allow-Credentials: true` and an explicit origin, not `*`. |
| **WebSocket origin check** | CORS does not apply to WebSockets. Servers must validate the `Origin` header themselves, or risk cross-site hijacking. |

```http
OPTIONS /orders HTTP/1.1
Origin: https://app.example.com
Access-Control-Request-Method: PUT

HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, PUT
Access-Control-Max-Age: 600
```
