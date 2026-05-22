# Concept ER Diagram

```mermaid
erDiagram

    宿泊プラン ||--o{ 予約 : "予約される"
    部屋 ||--o{ 宿泊プラン : "利用される"
    会員 ||--o{ 予約 : "行う"
```
