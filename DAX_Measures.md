# DAX Measures Used in Power BI Dashboard

This file contains the key DAX measures used in the Retail Analytics Power BI Dashboard.

---

## Core KPI Measures

### Revenue

```DAX
Revenue =
SUMX(
    Orders,
    Orders[Quantity] * RELATED(Stock[Retail_Price])
)
```

### COGs

```DAX
COGs =
SUMX(
    Orders,
    Orders[Quantity] * RELATED(Stock[Total_Cost])
)
```

### Profit

```DAX
Profit =
[Revenue] - [COGs]
```

### Quantity

```DAX
Quantity =
SUM(Orders[Quantity])
```

---

## Average & Inventory Measures

### Average COGS

```DAX
Avg Cogs =
AVERAGE(Stock[Total_Cost])
```

### Average Inventory Turnover

```DAX
Avg Inventory turnover =
AVERAGE(Stock[inventory_turnover])
```

### Average Inventory Value

```DAX
Average inventory value =
1
```

---

## Year-Based Measures

### Quantity 2020

```DAX
Quantity 2020 =
SUM(Stock[2020_units_sold])
```

### Quantity 2021

```DAX
Quantity 2021 =
CALCULATE(
    [Quantity],
    dimDate[Year] = 2021
)
```

### Quantity 2022

```DAX
Quantity 2022 =
CALCULATE(
    [Quantity],
    dimDate[Year] = 2022
)
```

### Revenue 2020

```DAX
$ Revenue 2020 =
SUMX(
    Stock,
    Stock[2020_units_sold] * Stock[Retail_Price]
)
```

### Revenue 2021

```DAX
$ Revenue 2021 =
CALCULATE(
    [Revenue],
    dimDate[Year] = 2021
)
```

### Profit 2020

```DAX
Profit 2020 =
SUMX(
    Stock,
    (Stock[2020_units_sold] * Stock[Retail_Price]) -
    (Stock[2020_units_sold] * Stock[Total_Cost])
)
```

### Profit 2021

```DAX
Profit 2021 =
SUMX(
    Stock,
    (Stock[Quantity 2021] * Stock[Retail_Price]) -
    (Stock[Quantity 2021] * Stock[Total_Cost])
)
```

### Profit Margin 2021

```DAX
Profit Margin 2021 =
[Profit 2021] / [$ Revenue 2021]
```

---

## Revenue Percentage Measures

### % Revenue 2021

```DAX
% Revenue 2021 =
CALCULATE(
    [Revenue],
    dimDate[Year] = 2021
) /
CALCULATE(
    [Revenue],
    dimDate[Year] = 2021,
    ALL(Stock)
)
```

### % Revenue 2020

```DAX
% Revenue 2020 =
CALCULATE(
    SUMX(
        Stock,
        Stock[2020_units_sold] * Stock[Retail_Price]
    ),
    dimDate[Year] = 2022
) /
CALCULATE(
    SUMX(
        Stock,
        Stock[2020_units_sold] * Stock[Retail_Price]
    ),
    dimDate[Year] = 2022,
    ALL(Stock)
)
```

---

## Product & Cost Measures

### Total COGS 2021

```DAX
total_COGS_2021 =
SUM(Stock[total_COGS_2021])
```

### COGS Revenue Ratio

```DAX
COGS_Revenue_ratio =
SUM(Stock[COGS_Revenue_ratio])
```

### Number of Products

```DAX
# NumOfProducts =
COUNTROWS(Stock)
```

### Retail Price

```DAX
Retail Price =
AVERAGE(Stock[Retail_Price])
```

---

## Placeholder Measures

### Quantity 2021 Placeholder

```DAX
Quantity 2021 Placeholder =
[Quantity 2021]
```

### Profit 2021 Placeholder

```DAX
Profit 2021 Placeholder =
[Profit 2021]
```

### Placeholder Formatting

```DAX
Placeholder/Quantity-Margin-Formatting =
SWITCH(
    SELECTEDVALUE(Placeholder[Placeholder_Order]),
    1, "#,##0,.0K",
    2, "0.0%",
    3, "0.0"
)
```

### Placeholder Quantity, Margin, Avg Turnover

```DAX
Placeholder/Quantity-Margin-AvgTurnOver =
SWITCH(
    SELECTEDVALUE(Placeholder[Placeholder_Order]),
    1, [Quantity 2021],
    2, [Profit Margin 2021],
    3, [Avg Inventory turnover]
)
```

---

## Conditional Formatting Measures

### Placeholder Conditional Formatting

```DAX
Placeholder/Quantity-Margin-AvgTurnOver CF =
VAR _Quantity_Max =
    MAXX(
        ALL(Stock[Category]),
        [Quantity 2021]
    )

VAR _Quantity_Min =
    MINX(
        ALL(Stock[Category]),
        [Quantity 2021]
    )

VAR _Margin_Max =
    MAXX(
        ALL(Stock[Category]),
        [Profit Margin 2021]
    )

VAR _Margin_Min =
    MINX(
        ALL(Stock[Category]),
        [Profit Margin 2021]
    )

VAR _TurnOver_Max =
    MAXX(
        ALL(Stock[Category]),
        [Avg Inventory turnover]
    )

VAR _TurnOver_Min =
    MINX(
        ALL(Stock[Category]),
        [Avg Inventory turnover]
    )

RETURN
SWITCH(
    TRUE(),

    [Quantity 2021] = _Quantity_Max &&
    SELECTEDVALUE(Placeholder[Placeholder_Order]) = 1, "#00aeef",

    [Quantity 2021] = _Quantity_Min &&
    SELECTEDVALUE(Placeholder[Placeholder_Order]) = 1, "#ed1c24",

    [Profit Margin 2021] = _Margin_Max &&
    SELECTEDVALUE(Placeholder[Placeholder_Order]) = 2, "#00aeef",

    [Profit Margin 2021] = _Margin_Min &&
    SELECTEDVALUE(Placeholder[Placeholder_Order]) = 2, "#ed1c24",

    [Avg Inventory turnover] = _TurnOver_Max &&
    SELECTEDVALUE(Placeholder[Placeholder_Order]) = 3, "#00aeef",

    [Avg Inventory turnover] = _TurnOver_Min &&
    SELECTEDVALUE(Placeholder[Placeholder_Order]) = 3, "#ed1c24",

    "#bcbec0"
)
```

### ABC Cards Conditional Formatting

```DAX
ABC Cards CF =
SWITCH(
    SELECTEDVALUE(Stock[ABC_class]),
    "A [High Value]", "#6dcff6",
    "B [Medium Value]", "#9ddcf9",
    "C [Low Value]", "#d4effc"
)
```

---

## Other Measures

### Date Text

```DAX
Date Text =
"1/1/2021 - 12/31/2021"
```

### Active Products 2020 Dummy

```DAX
Active Products _ 2020 _ Dummy =
[# NumOfProducts] *
( RANDBETWEEN(-1, 1) * ( RAND() + 1 ) ) *
( 1 - RAND() )
```
