---
title: 通道工厂和缓存
ms.date: 03/30/2017
ms.assetid: 954f030e-091c-4c0e-a7a2-10f9a6b1f529
ms.openlocfilehash: 055c9d1412338bb444ca33556f3c94b1ffc4c6a7
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54745317"
---
# <a name="channel-factory-and-caching"></a>通道工厂和缓存
WCF 客户端应用程序使用 <xref:System.ServiceModel.ChannelFactory%601> 类来创建 WCF 服务的通信通道。  创建 <xref:System.ServiceModel.ChannelFactory%601> 实例会带来一定的开销，因为这涉及以下操作：  
  
-   构造 <xref:System.ServiceModel.Description.ContractDescription> 树  
  
-   反映所有所需的 CLR 类型  
  
-   构造通道堆栈  
  
-   资源的释放  
  
 为了尽量减少这种开销，WCF 可以缓存使用 WCF 客户端代理时的通道工厂。  
  
> [!TIP]
>  当直接使用 <xref:System.ServiceModel.ChannelFactory%601> 类时，您可以直接控制通道工厂创建。  
  
 使用生成的 WCF 客户端代理[ServiceModel Metadata Utility Tool (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)派生自<xref:System.ServiceModel.ClientBase%601>。 <xref:System.ServiceModel.ClientBase%601> 定义一个静态 <xref:System.ServiceModel.ClientBase%601.CacheSetting%2A> 属性，该属性定义通道工厂缓存行为。 为特定类型设定缓存设置。 例如，设置`ClientBase<ITest>.CacheSettings`到一个如下定义的值会影响仅那些代理 /clientbase 类型的`ITest`。 特定 <xref:System.ServiceModel.ClientBase%601> 的缓存设置在创建第一个代理/ClientBase 实例后就不可改变。  
  
## <a name="specifying-caching-behavior"></a>指定缓存行为  
 缓存行为通过将 <xref:System.ServiceModel.ClientBase%601.CacheSetting> 属性设置为下列值之一指定。  
  
|缓存设置值|描述|  
|-------------------------|-----------------|  
|<xref:System.ServiceModel.CacheSetting.AlwaysOn>|应用程序域内的 <xref:System.ServiceModel.ClientBase%601> 的所有实例都可以参与缓存。 开发人员已经确定对缓存没有不利的安全性影响。 缓存将不会关闭即使"安全性敏感"属性<xref:System.ServiceModel.ClientBase%601>进行访问。 "安全性敏感"属性<xref:System.ServiceModel.ClientBase%601>都<xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A>，<xref:System.ServiceModel.ClientBase%601.Endpoint%2A>和<xref:System.ServiceModel.ClientBase%601.ChannelFactory%2A>。|  
|<xref:System.ServiceModel.CacheSetting.Default>|只有从在配置文件中定义的终结点创建的 <xref:System.ServiceModel.ClientBase%601> 的实例才参与应用程序域内的缓存。 以编程方式在应用程序域内创建的 <xref:System.ServiceModel.ClientBase%601> 的任何实例都将不参与缓存。 此外，缓存将被禁用的实例<xref:System.ServiceModel.ClientBase%601>后访问任何其"安全性敏感"属性。|  
|<xref:System.ServiceModel.CacheSetting.AlwaysOff>|在相关应用程序域内，已对特定类型的 <xref:System.ServiceModel.ClientBase%601> 的所有实例关闭缓存。|  
  
 下面的代码段演示如何使用 <xref:System.ServiceModel.ClientBase%601.CacheSetting%2A> 属性。  
  
```csharp  
class Program   
{   
   static void Main(string[] args)   
   {   
      ClientBase<ITest>.CacheSettings = CacheSettings.AlwaysOn;   
      foreach (string msg in messages)   
      {   
         using (TestClient proxy = new TestClient (new BasicHttpBinding(), new EndpointAddress(address)))   
         {   
            // ...  
            proxy.Test(msg);   
            // ...  
         }   
      }   
   }   
}  
// Generated by SvcUtil.exe     
public partial class TestClient : System.ServiceModel.ClientBase, ITest { }  
```  
  
 在上述代码中，`TestClient` 的所有实例都将使用相同的通道工厂。  
  
```csharp  
class Program   
{   
   static void Main(string[] args)   
   {   
      ClientBase.CacheSettings = CacheSettings.Default;   
      int i = 1;   
      foreach (string msg in messages)   
      {   
         using (TestClient proxy = new TestClient ("MyEndpoint", new EndpointAddress(address)))   
         {   
            if (i == 4)   
            {   
               ServiceEndpoint endpoint = proxy.Endpoint;   
               ... // use endpoint in some way   
            }   
            proxy.Test(msg);   
         }   
         i++;   
   }   
}   
  
// Generated by SvcUtil.exe     
public partial class TestClient : System.ServiceModel.ClientBase, ITest {}  
```  
  
 在上述示例中，`TestClient` 的所有实例都将使用相同的通道工厂，实例 #4 除外。 实例 # 4 将使用专门供其使用而创建的通道工厂。 此设置适用于此类方案：特定终结点需要的安全设置不同于属于相同通道工厂类型（在此情况下为 `ITest`）的其他终结点。  
  
```csharp  
class Program   
{   
   static void Main(string[] args)   
   {   
      ClientBase.CacheSettings = CacheSettings.AlwaysOff;   
      foreach (string msg in messages)   
      {   
         using (TestClient proxy = new TestClient ("MyEndpoint", new EndpointAddress(address)))   
         {   
            proxy.Test(msg);   
         }           
      }   
   }  
}  
  
// Generated by SvcUtil.exe   
public partial class TestClient : System.ServiceModel.ClientBase, ITest {}  
```  
  
 在上面的示例中，`TestClient` 的所有实例将使用不同的通道工厂。 当每个终结点具有不同的安全需求并且对缓存没有意义时，这非常有用。  
  
## <a name="see-also"></a>请参阅
- <xref:System.ServiceModel.ClientBase%601>
- [生成客户端](../../../../docs/framework/wcf/building-clients.md)
- [客户端](../../../../docs/framework/wcf/feature-details/clients.md)
- [使用 WCF 客户端访问服务](../../../../docs/framework/wcf/accessing-services-using-a-wcf-client.md)
- [如何：考虑使用 ChannelFactory](../../../../docs/framework/wcf/feature-details/how-to-use-the-channelfactory.md)
