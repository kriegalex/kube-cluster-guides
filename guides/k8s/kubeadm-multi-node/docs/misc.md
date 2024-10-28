# BIOS/UEFI Settings

Check your BIOS/UEFI settings to enable features that can improve performance:

- **Main Graphics Adapter**: Set to external if using a dedicated GPU.
- **SR-IOV**: Enable for Single Root I/O Virtualization.
- **Resizable BAR**: Enable for improved GPU performance.

Consult your motherboard's documentation for details.

# Kernel Parameters Explained

## net.bridge.bridge-nf-call-ip6tables

This parameter enables the bridge network driver to pass IPv6 traffic to `ip6tables` for processing. It allows `ip6tables` to filter bridged IPv6 traffic.

## net.bridge.bridge-nf-call-iptables

This parameter enables the bridge network driver to pass IPv4 traffic to `iptables` for processing, allowing `iptables` to filter bridged IPv4 traffic.

### Importance in Kubernetes

When set to `1`, `iptables` can apply Kubernetes-defined rules to bridged traffic, ensuring proper network communication between pods and services.

## net.ipv4.ip_forward

This parameter controls whether the Linux kernel forwards IPv4 packets. Setting it to `1` allows the machine to forward packets, acting as a router.

# Home Networking

## Static Routing

Configure your router to route traffic to the MetalLB IP range via your Kubernetes nodes.

For example, if MetalLB is assigning IPs in the `10.0.1.0/24` range, add a static route on your router:

- **Destination Network**: `10.0.1.0/24`
- **Gateway**: IP address of your Kubernetes node (e.g., `10.0.0.8`)

## Port Forwarding

If you need external access to services (e.g., a service exposed on port `32400`), set up port forwarding on your router:

- **External Port**: `32400`
- **Internal IP**: MetalLB assigned IP (e.g., `10.0.1.2`)
- **Internal Port**: `32400`

Ensure that your firewall rules allow the traffic.