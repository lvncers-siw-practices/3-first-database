# ドローンスクール管理システム ER 図

この ER 図は `drone-school-dump-fixed.sql` に基づいて作成されています。

## エンティティ関連図

```mermaid
erDiagram
    %% メインエンティティ
    SCHOOLS {
        int id PK
        varchar(200) headline
        text overview
        int prefecture_id FK
        int graduate_num
        year begin_year
        int job_title_id FK
        varchar(200) location
    }

    COURSES {
        int id PK
        varchar(100) name
        text detail
        int attendance_days
        int fee
    }

    INSTRUCTORS {
        int id PK
        varchar(100) name
        text detail
        int operation_company_id FK
    }

    LICENSES {
        int id PK
        varchar(45) series
        varchar(200) conditions
    }

    %% マスタデータ
    PREFECTURES {
        int id PK
        varchar(45) name
    }

    JOB_TITLES {
        int id PK
        varchar(45) title
    }

    ORGANIZATIONS {
        int id PK
        varchar(100) name
    }

    BUSINESS {
        int id PK
        varchar(100) name
    }

    OPERATION_COMPANIES {
        int id PK
        varchar(100) name
    }

    %% 関連テーブル（多対多）
    SCHOOL_LICENSES {
        int id PK
        int school_id FK
        int license_id FK
        int status
    }

    SCHOOL_INSTRUCTORS {
        int id PK
        int school_id FK
        int instructor_id FK
    }

    COURSE_LICENSES {
        int id PK
        int course_id FK
        int license_id FK
    }

    SCHOOL_ORGANIZATIONS {
        int id PK
        int school_id FK
        int organization_id FK
    }

    SCHOOL_BUSINESSES {
        int id PK
        int school_id FK
        int business_id FK
    }

    SCHOOL_OPERATION_COMPANIES {
        int id PK
        int school_id FK
        int operation_company_id FK
    }

    %% 直接的な関連（一対多）
    PREFECTURES ||--o{ SCHOOLS : "belongs_to"
    JOB_TITLES ||--o{ SCHOOLS : "categorizes"
    OPERATION_COMPANIES ||--o{ INSTRUCTORS : "employs"

    %% 多対多の関連
    SCHOOLS ||--o{ SCHOOL_LICENSES : ""
    LICENSES ||--o{ SCHOOL_LICENSES : ""

    SCHOOLS ||--o{ SCHOOL_INSTRUCTORS : ""
    INSTRUCTORS ||--o{ SCHOOL_INSTRUCTORS : ""

    COURSES ||--o{ COURSE_LICENSES : ""
    LICENSES ||--o{ COURSE_LICENSES : ""

    SCHOOLS ||--o{ SCHOOL_ORGANIZATIONS : ""
    ORGANIZATIONS ||--o{ SCHOOL_ORGANIZATIONS : ""

    SCHOOLS ||--o{ SCHOOL_BUSINESSES : ""
    BUSINESS ||--o{ SCHOOL_BUSINESSES : ""

    SCHOOLS ||--o{ SCHOOL_OPERATION_COMPANIES : ""
    OPERATION_COMPANIES ||--o{ SCHOOL_OPERATION_COMPANIES : ""
```

## システム概要

このドローンスクール管理システムは以下の要素で構成されています：

### 🏫 メインエンティティ

- **SCHOOLS**: ドローンスクールの基本情報
- **COURSES**: 提供されるコース情報
- **INSTRUCTORS**: 講師情報
- **LICENSES**: 取得可能なライセンス情報

### 📍 マスタデータ

- **PREFECTURES**: 都道府県情報
- **JOB_TITLES**: 職種分類
- **ORGANIZATIONS**: 関連組織
- **BUSINESS**: 業種分類
- **OPERATION_COMPANIES**: 運営会社

### 🔗 関係性の詳細

#### 直接的な関連（一対多）

- 各スクールは 1 つの都道府県に属する
- 各スクールは 1 つの職種カテゴリを持つ
- 各講師は 1 つの運営会社に所属する

#### 多対多の関連

- **スクール ↔ ライセンス**: スクールで取得可能なライセンス
- **スクール ↔ 講師**: スクールに所属する講師
- **コース ↔ ライセンス**: コースで取得可能なライセンス
- **スクール ↔ 組織**: スクールが関連する組織
- **スクール ↔ 業種**: スクールが対応する業種
- **スクール ↔ 運営会社**: スクールを運営する会社

### 💡 設計のポイント

1. **正規化された設計**: データの重複を避け、整合性を保つ
2. **柔軟な関係性**: 多対多の関係を適切に表現
3. **拡張性**: 新しい属性や関係を追加しやすい構造
4. **マスタデータ管理**: 都道府県、職種等の基本データを分離

### 🚀 使用例

```sql
-- スクールとその取得可能ライセンスを取得
SELECT s.headline, l.series
FROM schools s
JOIN school_licenses sl ON s.id = sl.school_id
JOIN licenses l ON sl.license_id = l.id
WHERE s.prefecture_id = 1;

-- 講師とその所属スクールを取得
SELECT i.name, s.headline
FROM instructors i
JOIN school_instructors si ON i.id = si.instructor_id
JOIN schools s ON si.school_id = s.id;
```
