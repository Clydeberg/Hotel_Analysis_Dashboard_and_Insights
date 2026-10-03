# DAX Measures

This catalog contains all **25 measures** extracted from the `key_measures` table in `hotel.pbix`. Names and DAX expressions are preserved from the model, including original spelling and capitalization.

## Core measures

### Revenue

```DAX
Revenue =
Sum(fact_bookings[revenue_realized])
```

### Total_Bookings

```DAX
Total_Bookings =
COUNT(fact_bookings[booking_id])
```

### Total_Capacity

```DAX
Total_Capacity =
sum(fact_aggregated_booking[capacity])
```

### Total_Successful_Booking

```DAX
Total_Successful_Booking =
CALCULATE(
    [Total_Bookings],
    fact_bookings[booking_status] = "Checked Out"
)
```

### Occupancy%

```DAX
Occupancy% =
DIVIDE([Total_Successful_Booking], [Total_Capacity], 0)
```

### Average_rating

```DAX
Average_rating =
AVERAGE(fact_bookings[ratings_given])
```

### no_of_days

```DAX
no_of_days =
DATEDIFF(MIN(dim_date[date]), MAX(dim_date[date]), DAY) + 1
```

### Total_Cancelled_Bookings

```DAX
Total_Cancelled_Bookings =
CALCULATE(
    [Total_Bookings],
    fact_bookings[booking_status] = "Cancelled"
)
```

### Cancellation%

```DAX
Cancellation% =
DIVIDE([Total_Cancelled_Bookings], [Total_Bookings])
```

### Total_no_show_bookngs

```DAX
Total_no_show_bookngs =
CALCULATE(
    [Total_Bookings],
    fact_bookings[booking_status] = "No Show"
)
```

### No_Show%

```DAX
No_Show% =
DIVIDE([Total_no_show_bookngs], [Total_Bookings])
```

### Booking%_by_platform

```DAX
Booking%_by_platform =
DIVIDE(
    [Total_Bookings],
    CALCULATE(
        [Total_Bookings],
        ALL(fact_bookings[booking_platform])
    ),
    0
)
```

### Booking%_by_room

```DAX
Booking%_by_room =
DIVIDE(
    [Total_Bookings],
    CALCULATE(
        [Total_Bookings],
        ALL(dim_rooms[room_class])
    ),
    0
)
```

### ADR

```DAX
ADR =
DIVIDE([Revenue], [Total_Bookings], 0)
```

### Realisation%

```DAX
Realisation% =
1 - ([Cancellation%] + [No_Show%])
```

### RevPAR

```DAX
RevPAR =
DIVIDE([Revenue], [Total_Capacity], 0)
```

### DBRN

```DAX
DBRN =
DIVIDE([Total_Bookings], [no_of_days], 0)
```

### DSRN

```DAX
DSRN =
DIVIDE([Total_Capacity], [no_of_days], 0)
```

### DURN

```DAX
DURN =
DIVIDE([Total_Successful_Booking], [no_of_days], 0)
```

## Week-over-week measures

### Revenue_WoW_change%

```DAX
Revenue_WoW_change% =
VAR selv =
    IF(
        HASONEFILTER(dim_date[week_num]),
        SELECTEDVALUE(dim_date[week_num]),
        MAX(dim_date[week_num])
    )
VAR revcw =
    CALCULATE([Revenue], dim_date[week_num] = selv)
VAR revpw =
    CALCULATE(
        [Revenue],
        FILTER(
            ALL(dim_date[week_num]),
            dim_date[week_num] = selv - 1
        )
    )
RETURN
    DIVIDE(revcw, revpw, 0) - 1
```

### Occupancy_WoW_change%

```DAX
Occupancy_WoW_change% =
VAR selv =
    IF(
        HASONEFILTER(dim_date[week_num]),
        SELECTEDVALUE(dim_date[week_num]),
        MAX(dim_date[week_num])
    )
VAR revcw =
    CALCULATE([Occupancy%], dim_date[week_num] = selv)
VAR revpw =
    CALCULATE(
        [Occupancy%],
        FILTER(
            ALL(dim_date),
            dim_date[week_num] = selv - 1
        )
    )
RETURN
    DIVIDE(revcw, revpw, 0) - 1
```

### ADR_WoW_change%

```DAX
ADR_WoW_change% =
VAR selv =
    IF(
        HASONEFILTER(dim_date[week_num]),
        SELECTEDVALUE(dim_date[week_num]),
        MAX(dim_date[week_num])
    )
VAR revcw =
    CALCULATE([ADR], dim_date[week_num] = selv)
VAR revpw =
    CALCULATE(
        [ADR],
        FILTER(
            ALL(dim_date),
            dim_date[week_num] = selv - 1
        )
    )
RETURN
    DIVIDE(revcw, revpw, 0) - 1
```

### Realisation_WoW_change%

```DAX
Realisation_WoW_change% =
VAR selv =
    IF(
        HASONEFILTER(dim_date[week_num]),
        SELECTEDVALUE(dim_date[week_num]),
        MAX(dim_date[week_num])
    )
VAR revcw =
    CALCULATE([Realisation%], dim_date[week_num] = selv)
VAR revpw =
    CALCULATE(
        [Realisation%],
        FILTER(
            ALL(dim_date),
            dim_date[week_num] = selv - 1
        )
    )
RETURN
    DIVIDE(revcw, revpw, 0) - 1
```

### DSRN_WoW_change%

```DAX
DSRN_WoW_change% =
VAR selv =
    IF(
        HASONEFILTER(dim_date[week_num]),
        SELECTEDVALUE(dim_date[week_num]),
        MAX(dim_date[week_num])
    )
VAR revcw =
    CALCULATE([DSRN], dim_date[week_num] = selv)
VAR revpw =
    CALCULATE(
        [DSRN],
        FILTER(
            ALL(dim_date),
            dim_date[week_num] = selv - 1
        )
    )
RETURN
    DIVIDE(revcw, revpw, 0) - 1
```

### RevPAR_WoW_change%

```DAX
RevPAR_WoW_change% =
VAR selv =
    IF(
        HASONEFILTER(dim_date[week_num]),
        SELECTEDVALUE(dim_date[week_num]),
        MAX(dim_date[week_num])
    )
VAR revcw =
    CALCULATE([RevPAR], dim_date[week_num] = selv)
VAR revpw =
    CALCULATE(
        [RevPAR],
        FILTER(
            ALL(dim_date),
            dim_date[week_num] = selv - 1
        )
    )
RETURN
    DIVIDE(revcw, revpw, 0) - 1
```

## Metric glossary

- **ADR (Average Daily Rate):** realized revenue per booking in this model.
- **RevPAR (Revenue per Available Room):** realized revenue divided by available room capacity.
- **DBRN:** Daily Booked Room Nights.
- **DSRN:** Daily Sellable Room Nights.
- **DURN:** Daily Utilized Room Nights.
- **Realisation %:** share of bookings that are neither cancelled nor no-shows; equivalently, the checked-out share for the status values present in this model.

> Note: the measure name `Total_no_show_bookngs` contains the original model's spelling. It has not been renamed here so the documentation matches the PBIX exactly.
