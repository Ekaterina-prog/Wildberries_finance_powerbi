# DAX Measures — Wildberries Finance Dashboard

Ниже приведены основные DAX-меры, используемые в финансовом дашборде Wildberries.

---

# Выручка WB

```DAX
Выручка WB = 
CALCULATE(
    SUM('public v_report_realization'[retail_amount]),
    'public v_report_realization'[doc_type_name] = "Продажа"
)
-
CALCULATE(
    SUM('public v_report_realization'[retail_amount]),
    'public v_report_realization'[doc_type_name] = "Возврат"
)
```

---

# Заказы, шт.

```DAX
Заказов, шт. = 
CALCULATE(
    SUM('public v_report_realization'[quantity]),
    'public v_report_realization'[doc_type_name] = "Возврат"
)
```

---

# Комиссия WB

```DAX
Комиссия WB = 
SUM('public v_report_realization'[ppvz_sales_commission])
```

---

# Комиссия WB %

```DAX
Комиссия WB % = 
DIVIDE(
    [Комиссия WB],
    [Выручка WB]
)
```

---

# Логистика

```DAX
Логистика = 
SUM('public v_report_realization'[delivery_rub])
```

---

# Себестоимость

```DAX
Cost Total = 
SUMX(
    'public v_report_realization',
    COALESCE(
        LOOKUPVALUE(
            'Себестоимость'[cost],
            'Себестоимость'[barcode],
            'public v_report_realization'[barcode]
        ),
        0
    )
    *
    'public v_report_realization'[quantity]
)
```

---

# Валовая прибыль

```DAX
Валовая прибыль = 
[Выручка WB]
- [Комиссия WB]
- [Логистика]
- SUM('public v_report_realization'[ppvz_reward])
- SUM('public v_report_realization'[ppvz_vw])
- SUM('public v_report_realization'[ppvz_vw_nds])
- [Cost Total]
```

---

# Рентабельность

```DAX
Рентабельность = 
DIVIDE(
    [Валовая прибыль],
    [Выручка WB]
)
```

---

# ROI

```DAX
ROI = 
DIVIDE(
    [Валовая прибыль],
    [Cost Total]
)
```

---

# Выручка предыдущего периода

```DAX
Revenue Prev Period = 
CALCULATE(
    [Выручка WB],
    PREVIOUSMONTH(Calendar[Date])
)
```

---

# Прирост выручки

```DAX
Revenue Growth = 
[Выручка WB] - [Revenue Prev Period]
```

---

# Лидеры роста

Фильтр визуала:

```
Revenue Growth > 0
```

Сортировка:

```
по убыванию
```

---

# Лидеры падения

Фильтр визуала:

```
Revenue Growth < 0
```

Сортировка:

```
по возрастанию
```