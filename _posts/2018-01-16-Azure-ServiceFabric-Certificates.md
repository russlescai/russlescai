---
layout: post
title: Service Fabric On-Premise Secure Cluster
description: Secure your Service Fabric cluster and ensure the cluster is in a healthy state
imgurl: images/blog-banner.jpg
tags: [service-fabric, certificates]
---

Microsoft recently [released](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-anywhere) Azure Service Fabric which can be configured on-premise, as well as on Azure. This provides a staged transition of services to Azure in the future, without having to re-architect or migrate.

Apps deployed on-premise can be deployed to Azure easily.

Setting up a local Service Fabric platform is reasonably straight forward, but it is important to consider minimum resource requirements (e.g. Microsoft recommends [3 nodes](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-common-questions#what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller) for a non-production cluster, and in production, minimum of [5 nodes](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-capacity)). 

Service Fabric can be setup using Windows security, but I would advise using a X.509 server certificate [secure configuration](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) (which is provided as part of the sample confiugration files).

One assumption is that the cluster nodes will have Internet access. Without Internet access, the certificate verification processes fail (or more specifically, time out). 

Otherwise, Service Fabric will report a warning, that the security CertGetCertificateChain function is above the threshold of 15 seconds. Essentially the certificate chain validation times out.

The reason for this, is that even for domain certificates (or where the root certificate is installed locally), the [CertGetCertificateChain](https://msdn.microsoft.com/en-us/library/windows/desktop/aa376078(v=vs.85).aspx) still makes an Internet call for an updated Certificate Revocation List (CRL).

It makes a call to the following location (note it is intentionally HTTP):
http://www.download.windowsupdate.com/msdownload/update/v3/static/trustedr/en/authrootstl.cab

The default timeout for a revocation URL check is 15 seconds. Without Internet access, the download will timeout, and Service Fabric will display a cluster health warning.

