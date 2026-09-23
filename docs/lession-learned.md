# Lessons Learned

The most valuable parts of operating this platform have often come from problems rather than initial deployments.

---

## Lesson: Simplicity Over Complexity

### Mesh VPN Evaluation

I initially wanted to use **NetBird** as a mesh VPN solution because it offered more flexibility than a basic VPN setup.

My goals included:

- connecting multiple trusted devices through a mesh network
- using my own exit node
- having more control over network access
- integrating centralized authentication through Keycloak

NetBird uses WireGuard as its underlying VPN technology and worked correctly from a connectivity perspective.

However, I experienced noticeable latency and responsiveness issues on my iPhone and iPad clients which was extremely annoying.

For my environment, I therefore decided to simplify the setup and move to a direct **WireGuard** configuration instead.

The direct WireGuard setup provided the private connectivity I actually needed with fewer components and more predictable behavior.

The main lesson was that additional features and abstraction are only useful when they provide a clear operational benefit.

---

## Lesson: Networking Problems Are Usually Layered

Routing, VPNs, UFW, iptables, MTU settings, Kubernetes networking and ingress will influence each other.

A connectivity problem that initially looks like an application issue may actually originate several layers below it.

Testing each layer independently made troubleshooting significantly more effective.

AI couldnt help much here because it went circles so i had to painfully solve it algorithmically.

---

## Lesson: Comnplexer Authentication Increased Infrastructure Complexity Immensely.

I experimented with **Keycloak** for centralized authentication especially because of netbird.

The pure suffering and pain caused by endlessly troubleshooting problems consumed immense time which was needed elsewhere.

Simplicity over Complexity once again wouldve been better here for this smaller project. (It is no enterprise model)

Therefore: For small self-hosted environments, the operational complexity of a component should be justified by a real requirement rather than added only because the technology is available.

---

## GitOps Is Pure Gold

Using Argo CD moved application deployment away from repeated manual Kubernetes changes.

Keeping the desired state in Git makes it easier to:

- understand what should be running
- track changes
- reproduce deployments
- identify configuration drift
- recover from incorrect changes

---

## Kubernetes Encourages Clear Separation

Running the dashboard and Python automation services as Kubernetes workloads helps separate application code from the underlying Debian host.

Applications can be managed through deployments, services and GitOps rather than being manually started and maintained directly on the server.

---

## Biggest Takeaway

Be aware of overengineering and focus on Simplicity over Complexity in order to aquire biggest ROI.

Simple things at scale tend to become complex and complex architecture at scale tends to become impossible especially if you do it yourself or only in a small team.
