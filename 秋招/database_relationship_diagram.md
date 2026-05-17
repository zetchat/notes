# 社区商城商品数据库关系图

## 数据库表关系图

```mermaid
erDiagram
    %% 商品分类体系
    category {
        bigint id PK
        char name
        varchar img_url
        bigint parent_id
        tinyint status
        int sort
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 属性体系
    attr_group {
        bigint id PK
        char name
        int sort
        varchar remark
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    attr {
        bigint id PK
        char name
        int input_type
        varchar select_list
        bigint attr_group_id FK
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 商品核心信息
    sku_info {
        bigint id PK
        bigint category_id FK
        bigint attr_group_id FK
        tinyint sku_type
        varchar sku_name
        varchar img_url
        int per_limit
        tinyint publish_status
        tinyint check_status
        tinyint is_new_person
        int sort
        varchar sku_code
        decimal price
        decimal market_price
        int stock
        int lock_stock
        int low_stock
        int sale
        bigint ware_id FK
        bigint version
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 商品展示相关
    sku_image {
        bigint id PK
        bigint sku_id FK
        varchar img_name
        varchar img_url
        int sort
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    sku_detail {
        bigint id PK
        bigint sku_id FK
        text detail_html
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    sku_poster {
        bigint id PK
        bigint sku_id FK
        varchar img_name
        varchar img_url
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    sku_attr_value {
        bigint id PK
        bigint sku_id FK
        bigint attr_id FK
        varchar attr_name
        varchar attr_value
        int sort
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 库存和仓库管理
    ware {
        bigint id PK
        varchar name
        varchar province
        varchar city
        varchar district
        varchar detail_address
        decimal longitude
        decimal latitude
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    region_ware {
        bigint id PK
        varchar region
        varchar region_name
        bigint ware_id FK
        varchar ware_name
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    sku_stock_history {
        bigint id PK
        bigint sku_id FK
        decimal price
        int stock
        int sale
        date sale_date
        bigint ware_id FK
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 评价系统
    comment {
        bigint id PK
        bigint sku_id FK
        varchar sku_name
        varchar nick_name
        varchar icon
        tinyint star
        varchar ip
        tinyint status
        int follow_count
        int reply_count
        varchar resources
        text content
        tinyint type
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    comment_replay {
        bigint id PK
        bigint comment_id FK
        varchar nick_name
        varchar icon
        varchar content
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 分类品牌关联
    base_category_trademark {
        bigint id PK
        bigint category3_id FK
        bigint trademark_id FK
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 系统工具
    mq_repeat_record {
        bigint id PK
        varchar business_no
        timestamp create_time
        timestamp update_time
        tinyint is_deleted
    }
    
    %% 关系定义
    category ||--o{ sku_info : "分类包含商品"
    category ||--o{ base_category_trademark : "分类关联品牌"
    
    attr_group ||--o{ attr : "分组包含属性"
    attr_group ||--o{ sku_info : "分组关联商品"
    
    attr ||--o{ sku_attr_value : "属性关联值"
    
    sku_info ||--o{ sku_image : "商品包含图片"
    sku_info ||--o{ sku_detail : "商品包含详情"
    sku_info ||--o{ sku_poster : "商品包含海报"
    sku_info ||--o{ sku_attr_value : "商品包含属性值"
    sku_info ||--o{ sku_stock_history : "商品库存历史"
    sku_info ||--o{ comment : "商品包含评价"
    
    ware ||--o{ sku_info : "仓库存储商品"
    ware ||--o{ region_ware : "仓库关联区域"
    ware ||--o{ sku_stock_history : "仓库库存历史"
    
    comment ||--o{ comment_replay : "评价包含回复"
```

## 业务流程图

```mermaid
flowchart TD
    A[商品分类管理] --> B[属性分组管理]
    B --> C[属性管理]
    C --> D[商品信息录入]
    D --> E[商品图片上传]
    D --> F[商品详情编辑]
    D --> G[商品海报制作]
    D --> H[商品属性值设置]
    
    I[仓库管理] --> J[区域仓库配置]
    J --> K[库存管理]
    K --> L[库存历史记录]
    
    D --> M[商品上架]
    M --> N[用户购买]
    N --> O[库存扣减]
    O --> L
    
    N --> P[用户评价]
    P --> Q[评价回复]
    
    R[品牌管理] --> S[分类品牌关联]
    
    style A fill:#e1f5fe
    style D fill:#f3e5f5
    style I fill:#e8f5e8
    style M fill:#fff3e0
    style P fill:#fce4ec
```

## 表关系说明

### 1. 核心业务关系
- **商品分类** → **商品信息**：每个商品必须属于一个分类
- **属性分组** → **属性** → **商品属性值**：商品通过属性体系描述规格
- **商品信息** → **商品展示**：图片、详情、海报等展示信息

### 2. 库存管理关系
- **仓库** → **区域仓库**：仓库与销售区域的关联
- **商品** → **库存历史**：记录商品库存变化

### 3. 用户交互关系
- **商品** → **评价** → **评价回复**：完整的用户评价体系

### 4. 品牌管理关系
- **分类** → **品牌关联**：管理分类与品牌的关联关系

## 数据库设计特点

1. **模块化设计**：每个业务模块独立管理
2. **软删除**：所有表都支持软删除（is_deleted字段）
3. **审计字段**：统一的时间戳和版本控制
4. **灵活扩展**：属性体系支持动态扩展
5. **数据完整性**：通过外键保证数据一致性
