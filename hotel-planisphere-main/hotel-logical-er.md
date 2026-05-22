# ER 図（論理設計）- シンプル版

```mermaid
erDiagram
    room_types {
        room_type_id "部屋タイプID"
        name "名称"
    }

    rooms {
        room_id "部屋ID"
        name "部屋名"
        room_type_id "部屋タイプID"
        capacity "定員数"
        area "面積"
    }

    facilities {
        facility_id "設備ID"
        name "設備名"
    }

    room_facilities {
        room_id "部屋ID"
        facility_id "設備ID"
    }

    plans {
        plan_id "プランID"
        name "プラン名"
        description "説明"
        base_price "基本価格"
        weekend_rate "土日割増率"
        min_guests "最少人数"
        max_guests "最大人数"
        max_nights "最長泊数"
        room_id "部屋ID"
    }

    member_types {
        member_type_id "会員タイプID"
        name "名称"
    }

    plan_member_types {
        plan_id "プランID"
        member_type_id "会員タイプID"
    }

    featured_plans {
        featured_id "おすすめID"
        plan_id "プランID"
        start_date "表示開始日"
        end_date "表示終了日"
        description "説明"
    }

    members {
        member_id "会員ID"
        name "氏名"
        email "メールアドレス"
        password "パスワード"
        member_type_id "会員タイプID"
        address "住所"
        phone "電話番号"
        gender "性別"
        birth_date "生年月日"
        news_subscription "お知らせ受信"
    }

    icon_settings {
        member_id "会員ID"
        image_path "画像パス"
        scale "拡大縮小"
        border_color "枠線色"
    }

    reservations {
        reservation_id "予約ID"
        member_id "会員ID"
        plan_id "プランID"
        room_id "部屋ID"
        check_in_date "宿泊日"
        nights "宿泊数"
        guests "人数"
        guest_name "氏名"
        contact_method "確認連絡方法"
        requests "要望"
        total_amount "合計金額"
    }

    addon_plans {
        addon_plan_id "追加プランID"
        name "名称"
        price "価格"
    }

    plan_addon_plans {
        plan_id "プランID"
        addon_plan_id "追加プランID"
    }

    reservation_addon_plans {
        reservation_id "予約ID"
        addon_plan_id "追加プランID"
        quantity "数量"
    }

    %% リレーション
    room_types ||--o{ rooms : "部屋タイプ"
    rooms ||--o{ room_facilities : "部屋設備"
    facilities ||--o{ room_facilities : "設備"
    rooms ||--o{ plans : "対象部屋"
    plans ||--o{ plan_member_types : "対象会員"
    member_types ||--o{ plan_member_types : "利用可能プラン"
    plans ||--o{ featured_plans : "おすすめプラン"
    member_types ||--o{ members : "会員種別"
    members ||--|| icon_settings : "アイコン設定"
    members ||--o{ reservations : "予約"
    plans ||--o{ reservations : "選択プラン"
    rooms ||--o{ reservations : "予約部屋"
    plans ||--o{ plan_addon_plans : "利用可能追加プラン"
    addon_plans ||--o{ plan_addon_plans : "対象プラン"
    reservations ||--o{ reservation_addon_plans : "予約追加プラン"
    addon_plans ||--o{ reservation_addon_plans : "追加プラン"
```
