# ER 図（物理設計）

```mermaid
erDiagram
    room_types {
        room_type_id int PK "部屋タイプID(NN)"
        name varchar_20 "名称(NN)"
    }

    rooms {
        room_id int PK "部屋ID(NN)"
        name varchar_50 "部屋名(NN)"
        room_type_id int FK "部屋タイプID(NN)"
        capacity tinyint "定員数(NN)"
        area smallint "面積(㎡)(NN)"
    }

    facilities {
        facility_id int PK "設備ID(NN)"
        name varchar_50 "設備名(NN)"
    }

    room_facilities {
        room_id int PK,FK "部屋ID(NN)"
        facility_id int PK,FK "設備ID(NN)"
    }

    plans {
        plan_id int PK "プランID(NN)"
        name varchar_100 "名称(NN)"
        description text "説明"
        base_price int "基本価格(NN)"
        weekend_rate decimal_5_2 "土日割増率(NN)"
        min_guests tinyint "最少人数(NN)"
        max_guests tinyint "最大人数(NN)"
        max_nights tinyint "最長泊数(NN)"
        room_id int FK "部屋ID(NN)"
    }

    member_types {
        member_type_id int PK "会員タイプID(NN)"
        name varchar_20 "名称(NN)"
    }

    plan_member_types {
        plan_id int PK,FK "プランID(NN)"
        member_type_id int PK,FK "会員タイプID(NN)"
    }

    featured_plans {
        featured_id int PK "おすすめID(NN)"
        plan_id int FK "プランID(NN)"
        start_date date "表示開始日(NN)"
        end_date date "表示終了日(NN)"
        description text "説明"
    }

    members {
        member_id int PK "会員ID(NN)"
        name varchar_50 "氏名(NN)"
        email varchar_100 "メールアドレス(NN)"
        password varchar_255 "パスワード(NN)"
        member_type_id int FK "会員タイプID(NN)"
        address varchar_200 "住所"
        phone varchar_15 "電話番号"
        gender char_1 "性別"
        birth_date date "生年月日"
        news_subscription boolean "お知らせ受信(NN)"
    }

    icon_settings {
        member_id int PK,FK "会員ID(NN)"
        image_path varchar_255 "画像パス(NN)"
        scale decimal_3_2 "拡大縮小(NN)"
        border_color varchar_7 "枠線色(NN)"
    }

    reservations {
        reservation_id int PK "予約ID(NN)"
        member_id int FK "会員ID(NN)"
        plan_id int FK "プランID(NN)"
        room_id int FK "部屋ID(NN)"
        check_in_date date "宿泊日(NN)"
        nights tinyint "宿泊数(NN)"
        guests tinyint "人数(NN)"
        guest_name varchar_50 "氏名(NN)"
        contact_method varchar_20 "確認連絡方法(NN)"
        requests text "要望"
        total_amount int "合計金額(NN)"
    }

    addon_plans {
        addon_plan_id int PK "追加プランID(NN)"
        name varchar_50 "名称(NN)"
        price int "価格(NN)"
    }

    plan_addon_plans {
        plan_id int PK,FK "プランID(NN)"
        addon_plan_id int PK,FK "追加プランID(NN)"
    }

    reservation_addon_plans {
        reservation_id int PK,FK "予約ID(NN)"
        addon_plan_id int PK,FK "追加プランID(NN)"
        quantity tinyint "数量(NN)"
    }

    %% リレーション
    room_types ||--o{ rooms : "部屋タイプの部屋"
    rooms ||--o{ room_facilities : "部屋の設備"
    facilities ||--o{ room_facilities : "設備の部屋"
    rooms ||--o{ plans : "使用部屋"
    plans ||--o{ plan_member_types : "プランの対象会員タイプ"
    member_types ||--o{ plan_member_types : "会員タイプのプラン"
    plans ||--o{ featured_plans : "対象プラン"
    member_types ||--o{ members : "種類"
    members ||--|| icon_settings : "会員の拡張設定"
    members ||--o{ reservations : "予約者"
    plans ||--o{ reservations : "プラン指定"
    rooms ||--o{ reservations : "部屋指定"
    plans ||--o{ plan_addon_plans : "プランの追加プラン"
    addon_plans ||--o{ plan_addon_plans : "追加プランのプラン"
    reservations ||--o{ reservation_addon_plans : "予約の追加プラン"
    addon_plans ||--o{ reservation_addon_plans : "追加プランの予約"
```
