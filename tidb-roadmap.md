---
title: TiDB 路线图
summary: 了解 TiDB 未来的发展方向，包括新特性和改进提升。
---

# TiDB 路线图

TiDB 路线图展示了 TiDB 未来的计划。随着我们发布长期稳定版本 (LTS)，这个路线图将会持续更新。通过路线图，你可以预先了解 TiDB 的未来规划，以便你关注进度，了解关键里程碑，并对开发工作提出反馈。

在开发过程中，路线图可能会根据用户需求和反馈进行调整，请不要根据路线图的内容制定上线计划。如果你有功能需求，或者想提高某个特性的优先级，请在 [GitHub](https://github.com/pingcap/tidb/issues) 上提交 issue。

## TiDB 重要特性规划

<table>
  <thead>
    <tr>
      <th>类别</th>
      <th>2024 年底 LTS 版本</th>
      <th>2025 年中 LTS 版本</th>
      <th>未来版本</th>
    </tr>
  </thead>
  <tbody valign="top">
    <tr>
      <td>
        <b>可扩展性与性能</b><br />增强性能
      </td>
      <td>
        <ul>
          <li>
             <b>TiKV 数据缓存</b><br />
            TiKV 在内存中维护数据的最近版本，以减少对多版本数据的重复扫描，进而提升性能。
          </li>
          <br />
          <li>
             <b>分区表全局索引</b><br />
          </li>
          <br />
          <li>
             <b>自动配置统计信息收集的并行度</b><br />
            TiDB 根据部署的节点数量和硬件规格自动配置统计信息收集任务的并行度和扫描并发度，提升收集速度。
          </li>
          <br />
          <li>
             <b>加速数据库恢复</b><br />
            缩短全量数据库恢复和 Point-in-time recovery (PITR) 所需的时间。
          </li>
          <br />
          <li>
             <b>支持不限大小的事务</b><br />
            未提交事务所处理的数据量不再受限于 TiDB 节点可用内存大小，提高事务和批量任务的成功率。
          </li>
          <br />
          <li>
             <b>TiProxy 根据负载转发流量</b><br />
            TiProxy 依据目标 TiDB 节点的负载情况对流量进行转发，以充分利用硬件资源。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>PD 的 heartbeat 微服务化</b><br />
            PD 的 heartbeat 可以独立部署和单独扩展，避免 PD 成为集群的资源瓶颈。
          </li>
          <br />
          <li>
            <b>减少统计信息收集时的 I/O 消耗</b><br />
            在进行统计信息收集时，可以选择在 TiKV 上仅扫描部分数据样本，以减少统计信息收集所消耗的时间和资源。
          </li>
          <br />
          <li>
            <b>移除将 Limit 算子下推到 TiKV 的已知限制</b><br />
          </li>
          <br />
          <li>
            <b>Cascades 优化器框架</b><br />
            引入更成熟强大的优化器框架，扩展当前优化器的基础能力。
          </li>
          <br />
          <li>
            <b>单个 DM 任务在全量迁移时达到每秒 150 MiB</b><br />
          </li>
          <br />
          <li>
            <b>增强 DDL 执行框架</b><br />
            提供可扩展的并行 DDL 执行框架，提升 DDL 的性能和稳定性。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>表级别的负载均衡</b><br />
            PD 根据每个表上各 Region 的负载情况决定数据的调度策略。
          </li>
          <li>
            <b>提升大数据量系统表的处理性能</b><br />
            当系统表中存在大量数据时，提升查询系统表的性能，降低查询开销。
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <b>稳定性与高可用</b>
        <br />提升可靠性
      </td>
      <td>
        <ul>
          <li>
            <b>限制备份任务的内存消耗</b><br />
          </li>
          <br />
          <li>
            <b>限制统计信息收集的内存消耗</b><br />
          </li>
          <br />
          <li>
            <b>管理大量的 SQL Binding</b><br />
              提升 SQL Binding 的使用体验，方便用户创建和管理大量的执行计划，以稳定数据库性能。
          </li>
          <br />
          <li>
            <b>资源组增强对复杂 SQL 的控制</b><br />
              在复杂 SQL 执行完成前，定期评估其 Request Unit (RU) 消耗，防止其在执行期间对整个系统产生过大的影响。
          </li>
          <br />
          <li>
            <b>自动为资源消耗超出预期的查询切换资源组</b><br />
              当一个查询被识别为 Runaway Query，用户可以选择将其调整至特定资源组，并设置资源消耗的上限。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>限制表元信息的内存消耗</b><br />
            提升大规模集群的稳定性。
          </li>
          <br />
          <li>
            <b>分布式统计信息收集</b><br />
            统计信息收集支持在多个 TiDB 节点上并行进行，提升收集效率。
          </li>
          <br />
          <li>
            <b>多版本统计信息</b><br />
            当统计信息被更新后，用户可以查看统计信息的历史版本，并能够选择恢复到过去某个版本的统计信息。
          </li>
          <br />
          <li>
            <b>更可靠的数据备份</b><br />
            减少数据备份过程中可能出现的内存不足等问题，并确保备份数据的可用性。
          </li>
          <br />
          <li>
            <b>常用算子均可落盘</b><br />
            HashAgg、Sort、TopN、HashJoin、WindowFunction、IndexJoin 和 IndexHashJoin 等常用算子均可落盘，进一步降低 OOM 风险。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>自适应资源组</b><br />
            资源组根据过往的运行情况自动调整资源组的 RU 设定。
          </li>
          <br />
          <li>
            <b>强化内存保护</b><br />
            TiDB 主动监控所有模块的内存使用情况，阻止可能影响系统稳定性的内存操作。 
          </li>
          <br />
          <li>
            <b>实例级执行计划缓存</b><br />
            同一个 TiDB 实例的所有会话可以共享执行计划缓存，提升内存利用率。
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <b>数据库管理与可观测性</b>
        <br />增强数据库可管理性及其生态系统
      </td>
      <td>
        <ul>
          <li>
            <b>可靠的查询终止</b><br />
            正在运行中的 SQL 语句能够被立即终止，并从 TiDB 和 TiKV 中释放相应的资源。
          </li>
          <br />
          <li>
            <b>切换资源组的权限控制</b>
            <br />只有被授予特定权限的用户才能切换自身的资源组，防止资源被滥用。
          </li>
          <br />
          <li>
            <b>支持查看表或 SQL 与热点 Region 的关系</b>
          </li>
          <br />
          <li>
            <b><code>IMPORT INTO</code> 支持逻辑导入</b>
          </li>
          <br />
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>细粒度定制统计信息收集策略</b>
            <br />用户可以修改特定表的统计信息收集策略，例如健康度和并行度。
          </li>
          <br />
          <li>
            <b>Workload Repository</b>
            <br />TiDB 持久化内存中记录的负载信息，包括累计统计数据和实时统计数据，有助于故障排查和分析。
          </li>
          <br />
          <li>
            <b>自动索引推荐</b>
            <br />TiDB 自动分析可以优化的 SQL 语句，并建议创建新索引或删除已有索引。
          </li>
          <br />
          <li>
            <b>支持修改分区表的列类型</b>
            <br />用户可以修改分区中列的类型，无论该列是否为分区键。
          </li>
          <br />
          <li>
            <b>设置 <code>IMPORT INTO</code> 的冲突策略</b>
            <br />用户可以为导入数据时出现的冲突设置解决策略，例如报错退出、忽略或替换。
          </li>
          <br />
          <li>
            <b>全链路监控</b>
            <br />跟踪单条 SQL 语句在整个生命周期中的时间消耗，包括 TiDB、TiKV、PD 和 TiFlash。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>负载分析</b>
            <br />分析 Workload Repository 中的过往负载数据，根据分析结果提出优化建议，例如 SQL 调优和统计信息收集策略调整。
          </li>
          <br />
          <li>
            <b>支持修改主键</b>
          </li>
          <br />
          <li>
            <b>支持将数据导出为 SQL 语句</b>
          </li>
          <br />
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <b>安全</b>
        <br />增强数据安全与隐私保护
      </td>
      <td>
        <ul>
          <li>
            <b>Google Cloud KMS</b>
            <br />完善静态加密基于 Google Cloud KMS 的密钥管理机制，使其成为正式功能。
          </li>
          <br />
          <li>
            <b>完善动态权限</b>
            <br />完善动态权限设计，限制 Super 权限的实现。
          </li>
          <br />
          <li>
            <b>基于标记的日志脱敏</b>
            <br />支持在集群日志中标记敏感数据，然后根据使用场景选择是否对这些敏感信息进行脱敏。
          </li>
          <br />
          <li>
            <b>FIPS</b>
            <br />加密场景符合 FIPS 标准。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>支持 AWS IAM 认证</b>
            <br />TiDB 作为 AWS 第三方 ARN，用于 AWS IAM 访问。
          </li>
          <br />
          <li>
            <b>Kerberos</b>
            <br />支持基于 Kerberos 的身份认证。
          </li>
          <li>
            <b>MFA</b>
            <br />支持多因素身份认证机制。
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <b>基于标签的访问控制</b>
            <br />支持通过配置标签的方式，以标签形式对数据进行访问控制。
          </li>
          <br />
          <li>
            <b>增强客户端加密</b>
            <br />支持客户端对关键字段进行加密，增强数据安全性。
          </li>
          <br />
          <li>
            <b>业务数据动态脱敏</b>
            <br />基于不同数据应用场景的数据脱敏，保证重要领域的数据安全。
          </li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

> **注意：**
>
> 上述表格中并未列出所有计划发布的内容。另外，不同的服务订阅版本中的功能可能有所不同。