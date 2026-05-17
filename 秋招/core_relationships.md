# 社区商城核心业务关系图

## 核心实体关系图

```mermaid
graph TB
    %% 核心实体
    Category[商品分类<br/>category]
    SkuInfo[商品信息<br/>sku_info]
    AttrGroup[属性分组<br/>attr_group]
    Attr[属性<br/>attr]
    Ware[仓库<br/>ware]
    
    %% 商品展示相关
    SkuImage[商品图片<br/>sku_image]
    SkuDetail[商品详情<br/>sku_detail]
    SkuPoster[商品海报<br/>sku_poster]
    SkuAttrValue[商品属性值<br/>sku_attr_value]
    
    %% 库存和评价
    StockHistory[库存历史<br/>sku_stock_history]
    Comment[商品评价<br/>comment]
    CommentReply[评价回复<br/>comment_replay]
    
    %% 关联关系
    Category -->|1:N| SkuInfo
    AttrGroup -->|1:N| Attr
    AttrGroup -->|1:N| SkuInfo
    Attr -->|1:N| SkuAttrValue
    Ware -->|1:N| SkuInfo
    Ware -->|1:N| StockHistory
    
    SkuInfo -->|1:N| SkuImage
    SkuInfo -->|1:1| SkuDetail
    SkuInfo -->|1:N| SkuPoster
    SkuInfo -->|1:N| SkuAttrValue
    SkuInfo -->|1:N| StockHistory
    SkuInfo -->|1:N| Comment
    
    Comment -->|1:N| CommentReply
    
    %% 样式
    classDef core fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef display fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef business fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    
    class Category,AttrGroup,Attr,Ware core
    class SkuImage,SkuDetail,SkuPoster,SkuAttrValue display
    class SkuInfo,StockHistory,Comment,CommentReply business
```

## 业务模块划分

```mermaid
mindmap
  root((社区商城))
    商品管理
      分类管理
        三级分类
        分类图片
      属性管理
        属性分组
        属性定义
      商品信息
        基本信息
        价格库存
        上架状态
      商品展示
        商品图片
        商品详情
        商品海报
    库存管理
      仓库管理
        仓库信息
        区域配置
      库存控制
        库存扣减
        库存预警
        历史记录
    用户交互
      商品评价
        评分星级
        评价内容
        图片视频
      评价回复
        商家回复
        用户互动
    系统工具
      品牌关联
      消息去重
```

## 数据流向图

```mermaid
flowchart LR
    subgraph "商品录入流程"
        A[创建分类] --> B[设置属性]
        B --> C[录入商品]
        C --> D[上传图片]
        D --> E[编辑详情]
        E --> F[设置属性值]
        F --> G[商品上架]
    end
    
    subgraph "库存管理流程"
        H[创建仓库] --> I[配置区域]
        I --> J[设置库存]
        J --> K[库存监控]
        K --> L[库存扣减]
    end
    
    subgraph "用户购买流程"
        M[浏览商品] --> N[选择商品]
        N --> O[下单购买]
        O --> P[库存扣减]
        P --> Q[用户评价]
    end
    
    G --> M
    L --> P
```

## 关键业务规则

### 1. 商品管理规则
- 每个商品必须属于一个分类
- 每个商品必须关联一个属性分组
- 商品属性值必须基于预定义的属性
- 商品上架前必须通过审核

### 2. 库存管理规则
- 商品库存不能为负数
- 锁定库存用于已下单但未支付的订单
- 库存低于预警值时需要补货提醒
- 库存变化需要记录历史

### 3. 评价管理规则
- 只有购买过的用户才能评价
- 评价可以包含图片和视频
- 商家可以回复用户评价
- 评价支持点赞和回复功能

### 4. 数据完整性规则
- 所有表都支持软删除
- 关键操作需要记录时间戳
- 库存操作需要版本控制防止并发问题
- 外键关系保证数据一致性
