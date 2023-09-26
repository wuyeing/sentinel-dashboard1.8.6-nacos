# sentinel-dashboard1.8.6-nacos
 
#### 介绍
* 本项目用于将sentinel控制台的策略配置信息保存到nacos中，使用json格式存储
* 本项目来自[sentinel-dashboard1.8.6-nacos](https://github.com/max-holo/sentinel-dashboard1.8.6-nacos)和[sentinel-dashboard-nacos](https://github.com/modongning/sentinel-dashboard-nacos)，源码改自前者，主要是修复了一些BUG。
* 简单使用就当作官方的控制台使用方式即可（除了要注意nacos的相关配置，后文会讲），如果想要深入可参考[sentinel-dashboard-nacos](https://github.com/modongning/sentinel-dashboard-nacos)。


#### 使用教程及其注意事项

1. 配置nacos相关参数，下面以properties的格式说明
   ```properties
    # 需要指定nacos的地址和配置文件所保存的命名空间id
    nacos.addr=127.0.0.1:8848
    nacos.namespace=634f0e00-37b5-42ef-b028-cb1fe5bfc628
   ```

2. 已经通过硬编码写死的配置（源码可见com.alibaba.csp.sentinel.dashboard.config.NacosConfigUtil），需要格外注意
   ```java
    public final class NacosConfigUtil {

        // group-id已经写死为SENTINEL_GROUP了，需要注意
        public static final String GROUP_ID = "SENTINEL_GROUP";

        // 关于配置的data-id（配置名称）对应着固定的命名规则，项目名+规则后缀，比如限流规则的命名规则为“项目名-flow-rules”，即${spring.application.name}-flow-rules
        // 假如，被监控的项目名称（spring.application.name）为app，则其在nacos保存的限流规则的data-id为app-flow-rules，熔断规则的data-id为app-degrade-rules，以此类推
        // 想要深入可见com.alibaba.csp.sentinel.dashboard.rule.nacos包下关于Provider和Publisher的具体实现

        // 下面是通过硬编码写死的规则后缀
        public static final String FLOW_DATA_ID_POSTFIX = "-flow-rules";
        public static final String DEGRADE_DATA_ID_POSTFIX = "-degrade-rules";
        public static final String SYSTEM_FULE_DATA_ID_POSTFIX = "-system-rules";
        public static final String AUTHORITY_DATA_ID_POSTFIX = "-authority-rules";
        public static final String GATEWAY_FLOW_DATA_ID_POSTFIX = "-gateway-flow-rules";
        public static final String GATEWAY_API_DATA_ID_POSTFIX = "-gateway-api-rules";
        public static final String DASHBOARD_POSTFIX = "-dashboard";

        public static final String PARAM_FLOW_DATA_ID_POSTFIX = "-param-rules";
        public static final String CLUSTER_MAP_DATA_ID_POSTFIX = "-cluster-map";


        /**
        * cc for `cluster-client`
        */
        public static final String CLIENT_CONFIG_DATA_ID_POSTFIX = "-cc-config";
        /**
        * cs for `cluster-server`
        */
        public static final String SERVER_TRANSPORT_CONFIG_DATA_ID_POSTFIX = "-cs-transport-config";
        public static final String SERVER_FLOW_CONFIG_DATA_ID_POSTFIX = "-cs-flow-config";
        public static final String SERVER_NAMESPACE_SET_DATA_ID_POSTFIX = "-cs-namespace-set";

        private NacosConfigUtil() {
        }
    }
   ```   
