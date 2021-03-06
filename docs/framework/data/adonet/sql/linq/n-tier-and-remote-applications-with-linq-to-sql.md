---
title: 使用 LINQ to SQL 的 N 层和远程应用程序
ms.date: 03/30/2017
ms.assetid: 854a1cdd-53cb-45f5-83ca-63962a9b3598
ms.openlocfilehash: 32035720992a679457f11414ab282f949b450082
ms.sourcegitcommit: d2ccb199ae6bc5787b4762e9ea6d3f6fe88677af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2019
ms.locfileid: "56094069"
---
# <a name="n-tier-and-remote-applications-with-linq-to-sql"></a>使用 LINQ to SQL 的 N 层和远程应用程序
可以创建使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 的 n 层或多层应用程序。 通常情况下，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]数据上下文、 实体类和查询构造逻辑位于中间层上作为数据访问层 (DAL)。 业务逻辑和任何非持久性数据都可以在实体的分部类和分部方法中以及数据上下文中完整实现，也可以在单独的类中实现。

 客户端或表示层调用中间层的远程接口上的方法，而该层上的 DAL 会执行映射到 <xref:System.Data.Linq.DataContext> 方法的查询或存储过程。 中间层通常以实体或代理对象的 XML 表示形式向客户端返回数据。

 在中间层上，实体由数据上下文创建，数据上下文会跟踪实体的状态，并管理从数据库延迟加载实体以及将更改提交给数据库的过程。 这些实体被“附加”到 `DataContext`。 但是，在实体通过序列化发送到其他层后，这些实体会分离出来，这意味着 `DataContext` 不再跟踪它们的状态。 必须在将客户端发送回来进行更新的实体重新附加到数据上下文之后，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 才能将更改提交给数据库。 如果原始值和/或时间戳是开放式并发检查所必需的，则客户端负责将它们返回给中间层。

 在 ASP.NET 应用程序中，<xref:System.Web.UI.WebControls.LinqDataSource> 管理这一复杂过程的大部分工作。 有关详细信息，请参阅[LinqDataSource Web 服务器控件概述](https://docs.microsoft.com/previous-versions/aspnet/bb547113(v=vs.100))。

## <a name="additional-resources"></a>其他资源
 有关如何实现使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 的 n 层应用程序的更多信息，请参见以下主题：

-   [使用 ASP.NET 的 LINQ to SQL N 层](../../../../../../docs/framework/data/adonet/sql/linq/linq-to-sql-n-tier-with-aspnet.md)

-   [使用 Web 服务 的 LINQ to SQL N 层](../../../../../../docs/framework/data/adonet/sql/linq/linq-to-sql-n-tier-with-web-services.md) 

-   [实现 N 层业务逻辑](../../../../../../docs/framework/data/adonet/sql/linq/implementing-business-logic-linq-to-sql.md)

-   [N 层应用程序中的数据检索和 CUD 操作 (LINQ to SQL)](../../../../../../docs/framework/data/adonet/sql/linq/data-retrieval-and-cud-operations-in-n-tier-applications.md)

 有关使用 ADO.NET 数据集的 n 层应用程序的详细信息，请参阅[使用 n 层应用程序中的数据集](/visualstudio/data-tools/work-with-datasets-in-n-tier-applications)。

## <a name="see-also"></a>请参阅
- [背景信息](../../../../../../docs/framework/data/adonet/sql/linq/background-information.md)
