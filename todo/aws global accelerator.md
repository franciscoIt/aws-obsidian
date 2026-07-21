![[Pasted image 20260720164116.png]]
The correct answer is **B — AWS Global Accelerator with UDP listeners and endpoint groups in each Region.**

The selected answer (D) is incorrect. Here's why:

**Why B is correct:**

- Global Accelerator uses the AWS global network backbone (not the public internet) to route traffic, which reduces latency and jitter/packet loss significantly.
- It natively supports **UDP** listeners, which is exactly the protocol this game uses.
- It automatically routes each player to the closest/healthiest endpoint group (Region) using **anycast IPs**, and performs health checks with automatic failover — ideal for real-time multiplayer gaming.
- This is AWS's specifically recommended pattern for latency-sensitive, non-HTTP (UDP/TCP) gaming and VoIP workloads.

**Why the others are wrong:**

- **A (Transit Gateway mesh):** Transit Gateway is for connecting VPCs/networks together (routing), not for directing end users to the nearest Region. It doesn't reduce internet-leg latency for players and adds unnecessary complexity/cost as a full mesh.
- **C (CloudFront):** CloudFront is a content delivery network built for HTTP/HTTPS caching of content — it does **not support UDP** at all, so this fails the core protocol requirement.
- **D (VPC peering mesh):** VPC peering just connects VPCs privately; it has nothing to do with routing _end users_ to the optimal Region, and does not reduce client-to-server latency or packet loss. It solves a different problem (backend-to-backend connectivity), not the user-facing latency/loss issue described.