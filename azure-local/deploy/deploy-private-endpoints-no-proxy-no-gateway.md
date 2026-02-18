---
title: Use Private Endpoints with Azure Local for No Proxy No Gateway
description: Review how Azure Private Endpoints can be used when deploying Azure Local, without an enterprise proxy and without an Arc gateway.
author: alkohli
ms.author: alkohli
ms.reviewer: alkohli
ms.date: 02/18/2026
ms.topic: concept-article
---

# Use private endpoints with Azure Local for no proxy and no Arc gateway scenario

This article provides an overview of how you can integrate both existing and new Azure private endpoints with Azure Local. A private endpoint for Azure Local is a network interface that uses a private IP address from the virtual network associated with your Azure Local.

Currently, Azure Local offers the following distinct methods for outbound connectivity:

- Deploy Azure Local without an enterprise proxy and without an Arc gateway.

- Deploy Azure Local with an enterprise proxy but without an Arc gateway.

- Deploy Azure Local without an enterprise proxy but with an Arc gateway.

- Deploy Azure Local with both an enterprise proxy and an Arc gateway.

Each of these scenarios is described in the subsequent sections of this article.


## Scenario 1: No Proxy, No Arc Gateway

**Description:** Azure Local infrastructure sends HTTP and HTTPS traffic directly via the default route without a proxy or Arc gateway. Enterprise firewall or router can redirect this traffic to various subnets using the public internet or Azure ExpressRoute. Customers must define permitted endpoints on their firewall according to destination needs.

:::image type="content" source="media/deploy-private-endpoints-no-proxy-no-gateway/image1.png" alt-text="Scenario with no proxy and no Arc gateway.":::Outbound Connectivity for Azure Local hosts:

**Diagram legend**:

- LNET = Logical Network

- 10.0.0.0/16 is just an example of a private network where you can configure the private endpoint.

<!-- -->

- HTTP/HTTPS outbound traffic uses the management network's default route.

- Public endpoints go through the enterprise firewall to the internet.

- Private endpoints are routed by the enterprise firewall via Azure ExpressRoute or S2S VPN.

:::image type="content" source="media/deploy-private-endpoints-no-proxy-no-gateway/image2.png" alt-text="A blue and purple rectangle with text AI-generated content may be incorrect.":::Outbound Connectivity for Arc Resource Bridge VM:

**Diagram legend**:

- LNET = Logical Network

- 10.0.0.0/16 is just an example of a private network where you can configure the private endpoint.

<!-- -->

- HTTP/HTTPS outbound traffic uses the management network's default route.

- Public endpoints go through the enterprise firewall or router to the internet.

- Private endpoints are routed by the enterprise firewall via Azure ExpressRoute or S2S VPN.

:::image type="content" source="media/deploy-private-endpoints-no-proxy-no-gateway/image3.png" alt-text="A blue and purple text and a white rectangle AI-generated content may be incorrect.":::Outbound Connectivity for AKS clusters control plane and worker VMs:

**Diagram legend**:

- LNET = Logical Network

- 10.0.0.0/16 is just an example of a private network where you can configure the private endpoint.

<!-- -->

- When the AKS cluster LNET shares the management network, HTTP and HTTPS outbound traffic follow the default route of the management network.

- If the AKS cluster LNET is separate from the management network, HTTP and HTTPS outbound traffic use the default route of the AKS subnet.

- The enterprise firewall or router directs public endpoint traffic over the internet.

- For private endpoints, the enterprise firewall sends traffic through Azure ExpressRoute or an S2S VPN.

:::image type="content" source="media/deploy-private-endpoints-no-proxy-no-gateway/image4.png" alt-text="A blue and purple line with words AI-generated content may be incorrect.":::Outbound Connectivity for Azure Local VMs:

**Diagram legend**:

- LNET = Logical Network

- 10.0.0.0/16 is just an example of a private network where you can configure the private endpoint.

<!-- -->

- Azure Local VMs can have independent proxy and Arc gateway settings that don't relate to the host configuration.

- VMs on Azure Local hosts can run with or without their own proxy setup.

- Without proxy settings, VM traffic uses the VM's LNET default gateway.

- The proxy details you set during VM creation are applied as environment variables. If you need to configure any WinInet or WinHTTP proxy settings for Windows, you must do so within the VM. For Linux VMs, you might also need to provide additional proxy settings.

### Private endpoints considerations when deploying Azure Local without proxy and without Arc gateway

*Key Vault private endpoints (vault.azure.net):*

Azure Local needs a Key Vault for deployment, and it can use a private endpoint. Keep public access enabled until deployment finishes, as the Azure portal and HCI RP configure the Key Vault secrets.

*Storage account private endpoints (blob.core.windows.net):*

Azure Local with two nodes requires a Storage Account for deployment. You can use a private endpoint for this Storage Account, but you must allow public access until the initial deployment is complete. Azure portal and HCI RP need to configure the cloud witness during deployment. Once deployment is completed, you can restrict Storage Account access to only allow private networks.

