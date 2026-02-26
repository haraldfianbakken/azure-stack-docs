---
title: About Private Endpoints with Azure Local
description: Review how Azure Private Endpoints can be used when deploying Azure Local, with and without Arc gateway, and with and without Proxy.
author: alkohli
ms.author: alkohli
ms.reviewer: alkohli
ms.date: 02/26/2026
ms.topic: concept-article
---

# About Azure private endpoints on Azure Local

This article provides an overview of Azure private endpoints on Azure Local and the supported scenarios. 

## About private endpoints for Azure Local

A private endpoint for Azure Local is a network interface that uses a private IP address from the virtual network associated with your Azure Local.

## Supported private endpoints for Azure Local

While Azure Local infrastructure (nodes and Arc resource bridge VM) supports a variety of [Private endpoint types](/azure/private-link/private-endpoint-overview), Azure Arc Private Link isn't supported. For more information, see [Use Azure Private Link to Connect Servers to Azure Arc by Using a private endpoint](/azure/azure-arc/servers/private-link-security). As a result, registering Azure Local with Arc always uses Arc public endpoints. For more information, see Troubleshoot Azure Arc resource bridge issues.

If you're already using Azure Arc private link scope for Arc for servers and your corporate DNS is already resolving Arc private endpoints, you must use a different DNS server for Azure Local infrastructure to ensure it always resolves public Arc endpoints.

For example, if name resolution for these two endpoints (**gbl.his.arc.azure.com** and **agentserviceapi.guestconfiguration.azure.com**) returns a private IP address from any of these ranges (10.x.x.x, 192.168.x.x, or 172.16.x.x) on the Azure Local nodes or the enterprise proxy, it means Azure Arc Private Link DNS resolution is enabled and isn't supported.

If you intend to use private endpoints for services such as Key Vaults, Storage Accounts, SQL, Azure Container Registry, or other PaaS offerings alongside Azure Local, make sure that your DNS infrastructure resolves the PaaS FQDN to an internal IP address, and your network correctly routes traffic. Traffic is directed either to the public internet for public endpoints or through Azure ExpressRoute/Site-to-Site (S2S) VPN for private endpoints, according to its destination.

## Support for Azure Local VMs and Arc Private Link

Although Azure Local nodes and Arc Resource Bridge VM don't support Arc private link, you can use Arc Private Link inside Azure Local VMs if the DNS servers in use aren't the same as those used for the Azure Local infrastructure (Azure Local infrastructure DNS servers must not resolve Arc private endpoints). 

- To enable Arc Private Link on existing Azure Local VMs, follow [Use Azure Private Link to Connect Servers to Azure Arc by Using a private endpoint](/azure/azure-arc/servers/private-link-security#configure-an-existing-azure-arc-enabled-server).
- For new Azure Local VMs, you can't enable Arc Private Link during deployment yet. 

## Arc resource bridge and AKS reserved network subnets coexistence with private endpoints

By default, when deploying Arc Resource Bridge VM in Azure Local, the following IP ranges are reserved for Kubernetes pods and services. Ensure that your PaaS services private endpoint IPs on the Azure VNET subnet used by AKS workloads don't overlap with any of reserved Kubernetes subnets.

For example, if your private endpoint has an IP from an Azure subnet 10.244.1.0/24, AKS understands such IP as reserved and the request doesn't leave the AKS virtual networks and never reaches the private endpoint in the Azure VNET. However, if the private endpoint IP is on an Azure subnet 10.245.0.0/24, AKS resolves the endpoint as external and routes the traffic to reach the private endpoint.

| **Service** | **Designated IP range** |
|----|----|
| Arc resource bridge Kubernetes pods | 10.244.0.0/16 (from 10.244.0.1 to 10.244.255.254) |
| Arc resource bridge Kubernetes services | 10.96.0.0/12 (from 10.96.0.1 to 10.111.255.54) |

The following table summarizes key points for using supported private endpoints with Azure Local.

> [!NOTE]
> Azure Local doesn't support Azure Arc Private Link for all the tabulated scenarios, and Azure Local infrastructure (nodes and Arc resource bridge VM) must use public Arc endpoints.

| Scenario | Private endpoint outbound path | Supported private endpoint types | Key requirements or limitations |
|----|----|:--:|:--:|----|
| **1. No proxy, no Arc gateway** | Direct. <br> Routed via Express Route or S2S VPN | Storage, SQL, Key Vault, Azure Container Registry and other PaaS services supporting Private Link | Configure routing and DNS for private endpoints; no proxy bypass needed |
| **2. With proxy, no Arc gateway** | Proxy bypassed. <br> Routed via Express Route or S2S VPN | Storage, SQL, Key Vault, Azure Container Registry and other PaaS services supporting Private Link | Configure routing, DNS and proxy bypass list for private endpoint FQDNs |
| **3. No proxy, with Arc gateway** | AKS cluster IP proxy bypassed. <br> Routed via Express Route or S2S VPN | Storage, SQL, Key Vault, Azure Container Registry and other PaaS services supporting Private Link | Configure routing, DNS and environment variables for AKS/Arc resource bridge after Arc registration to bypass private endpoint FQDNs |
| **4. With proxy, with Arc gateway** | Proxy bypassed. <br> Routed via Express Route or S2S VPN | Storage, SQL, Key Vault, Azure Container Registry and other PaaS services supporting Private Link | Configure routing, DNS and proxy bypass for private endpoint FQDNs |

## Related steps

Learn how to deploy Azure private endpoints for Azure Local in the following scenarios:
- [Deploy Azure Local with private endpoints, and without proxy, without Arc gateway](deploy-about-private-endpoints-no-proxy.md)
- [Deploy Azure Local with private endpoints, and with proxy, without Arc gateway](deploy-about-private-endpoints-proxy.md)
- [Deploy Azure Local with private endpoints and without proxy, with Arc gateway](deploy-about-private-endpoints-arc-gateway.md)
- [Deploy Azure Local with private endpoints, with proxy, with Arc gateway](deploy-about-private-endpoints-proxy-arc-gateway.md)