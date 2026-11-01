# Linux & Networking

Most production incidents eventually reach the operating system or the network, so interviewers use Linux and networking questions to separate engineers who can diagnose a live problem from those who restart it and hope. Senior backend, SRE-adjacent and Forward Deployed Engineer roles ask most, because FDEs often debug inside customer environments with unfamiliar firewalls, proxies and DNS, and with only a shell to work from.

> Related: [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [Kubernetes](kubernetes.md) · [Observability & reliability](observability-and-reliability.md)

---

## A. Fundamentals — must answer crisply
- [ ] Process vs. thread: what do they share, what do they not, and what do `fork` and `exec` each do?
- [ ] SIGTERM vs. SIGKILL vs. SIGHUP — which can be caught, and what should a well-behaved service do on each?
- [ ] What is a file descriptor, and why does "everything is a file" matter for sockets, pipes and limits?
- [ ] Virtual vs. resident memory: what do VSZ and RSS mean, and why is "free memory" on Linux usually misleading?
- [ ] What do load average and CPU utilisation each measure, and how can load be high while the CPU is idle?
- [ ] Walk through the TCP three-way handshake and the four-way close. What does each side's state look like?
- [ ] Walk through a DNS lookup from an application call to an authoritative answer, including caches and TTLs.
- [ ] What does TLS give you, and what happens during a TLS 1.3 handshake? *(verify)*
- [ ] L4 vs. L7 load balancing: what can each see, and what can each do as a result?

## B. Working depth
- [ ] What is a zombie process vs. an orphan, and why does PID 1 in a container need to reap children?
- [ ] Namespaces vs. cgroups: which isolates what a process can see, and which limits what it can use?
- [ ] How does the OOM killer choose a victim, and how do you confirm it killed your process?
- [ ] What is `TIME_WAIT` for, which side holds it, and why is a large count usually harmless on a server?
- [ ] A server shows thousands of connections in `CLOSE_WAIT`. Whose bug is it, and what does it tell you about the application?
- [ ] What limits how many connections a service can hold: `ulimit -n`, ephemeral ports, the accept backlog, conntrack? How do you check each?
- [ ] systemd basics: unit files, `Restart=` policies, dependencies, and reading logs with `journalctl`.
- [ ] Source NAT vs. destination NAT: how does NAT change what a server sees, and what is NAT gateway port exhaustion? *(verify)*
- [ ] What is MTU, how does path MTU discovery work, and why might small requests succeed while large ones hang?
- [ ] DNS record types (A, AAAA, CNAME, MX, TXT, NS) and why low TTLs do not guarantee fast cutover.
- [ ] How does TLS certificate validation work: the chain, SNI, expiry, and what mutual TLS adds?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Which cgroup v2 controls apply when you set container CPU and memory limits, and how does CFS throttling hurt tail latency? *(verify)*
- [ ] Nagle's algorithm, delayed ACKs and `TCP_NODELAY` — when do they interact badly, and when would you change the defaults?
- [ ] TCP keepalive vs. HTTP keep-alive vs. idle timeouts on a load balancer: how do mismatched timeouts cause sporadic 502s?
- [ ] How does connection pooling interact with load balancing, and why can long-lived HTTP/2 or gRPC connections skew traffic across backends?
- [ ] Round robin vs. least connections vs. consistent hashing: how does each behave during a slow backend or a scale-out?
- [ ] What can go wrong with conntrack tables under high connection churn, and how would you detect and mitigate it?
- [ ] What are the costs of TLS termination at the edge vs. re-encryption to the backend, and when does compliance decide it for you?
- [ ] How do you preserve the client IP through proxies and NAT (`X-Forwarded-For`, PROXY protocol), and what are the spoofing risks?
- [ ] What failure modes do DNS caches and negative caching cause, and how do you make a service resilient to a resolver outage?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A service suddenly logs "too many open files". Walk through how you confirm the cause and tell a leak from a limit that is simply too low.
- [ ] An HTTPS call from a customer's network hangs after the TCP connect, but `curl` to a small endpoint works. How do you narrow it down (MTU, proxy, TLS inspection, firewall)?
- [ ] Requests between two services see intermittent five-second delays. What do you suspect, and how do you prove it?
- [ ] A host is slow but CPU looks fine. Work through memory pressure, disk I/O, run queue and network with the tools you would use.
- [ ] Outbound calls start failing at peak with connection timeouts, while the destination reports healthy. Design the investigation and the fix.
- [ ] A process is stuck and its logs have stopped. Without restarting it, how do you find out what it is doing?
- [ ] Plan a zero-downtime DNS and load balancer cutover to a new region, including rollback and clients that ignore TTLs.

## E. Hands-on checks (things to be able to DO)
- [ ] Use `top` or `htop`, `vmstat` and `iostat` to classify a slowdown as CPU, memory or I/O.
- [ ] Attach `strace` to a running process and identify the slow or failing system call.
- [ ] Use `ss` and `lsof` to list listening sockets, connection states and the descriptors owned by a process.
- [ ] Capture traffic with `tcpdump` using a filter, and find a handshake, a retransmission and a reset in the capture.
- [ ] Use `dig` to trace a lookup from the root, query a specific resolver and read the TTLs.
- [ ] Write a systemd unit with a restart policy, resource limits and a non-root user, then debug a failed start.
- [ ] Inspect a certificate chain and negotiated protocol with `openssl s_client`, and test timing phases with `curl -w`.

---

**Good answers show:**
- A method rather than a guess: forming a hypothesis, choosing the tool that tests it, and narrowing layer by layer.
- Reasoning from kernel and protocol behaviour, such as which side holds a socket state and why.
- Awareness that limits are everywhere: descriptors, ports, conntrack, backlog, cgroups, and timeouts between hops.
- Careful handling of production: read-only checks first, and knowing that tools like `strace` add overhead.
- Connecting low-level causes to user-visible symptoms such as tail latency, 502s and sporadic hangs.
- Calm troubleshooting in environments you do not control, working with what the network team can and cannot change.

**Prove it:** Build a small lab with two containers and a reverse proxy, then cause five failures on purpose: descriptor exhaustion, a `CLOSE_WAIT` leak, a low MTU on one link, a mismatched idle timeout that produces 502s, and CPU throttling from a cgroup limit. For each, write a short runbook with the symptom, the commands you ran, the evidence from `ss`, `strace` or `tcpdump`, and the fix.
