# Filters

Filters are a powerfull way to select only some kind of events. Let's imagine that your application wants to execute a task every time an event occurs but **only** when the data in the event are matching your needs.

In order to do that you can define some data matching between the data from the event and the filter you want

Filters will be defined with an event's attribute and it's predicate(s). The filter will be valid when all the predicates from all the attributes are true. If the attribute doesn't exists or cannot match the predicate, this predicate will be ignored.

The filter will need to be a triplet like follow :
`attribute <predicate> value`

## List of predicates

#### eq
> The `eq` predicate accept the event, when the event's data attribute is equal to the value
>
> **Opposite: `neq`**

#### neq
> The `neq` predicate accept the event, when the event's data attribute is different than the value
>
> **Opposite: `eq`**

#### gt
> The `gt` predicate accept the event, when the event's data attribute is greater than the value
>
> **Opposite: `lteq`**

#### gteq
> The `gteq` predicate accept the event, when the event's data attribute is greater than or equals to the value
>
> **Opposite: `lt`**

#### lt
> The `lt` predicate accept the event, when the event's data attribute is less than the value
>
> **Opposite: `gteq`**

#### lteq
> The `lteq` predicate accept the event, when the event's data attribute is less than or equals to the value
>
> **Opposite: `gt`**

#### matches
> The `matches` predicate accept the event, when the event's data attribute matches the regexp given as the value
>
> **Opposite: `does_not_match`**

#### does_not_match
> The `does_not_matches` predicate accept the event, when the event's data attribute does not match the regexp given as the value
>
> **Opposite: `matches`**

#### start
> The `start` predicate accept the event, when the event's data attribute start with the value
>
> **Opposite: `not_start`**

#### not_start
> The `not_start` predicate accept the event, when the event's data attribute does not start with the value
>
> **Opposite: `start`**

#### end
> The `end` predicate accept the event, when the event's data attribute end with the value
>
> **Opposite: `not_end`**

#### not_end
> The `end` predicate accept the event, when the event's data attribute does not end with the value
>
> **Opposite: `end`**

#### in
> The `in` predicate accept the event, when the event's data attribute is included in the list of values
>
> **Opposite: `not_in`**

#### not_in
> The `not_in` predicate accept the event, when the event's data attribute is not included in the list of values
>
> **Opposite: `in`**

#### cont
> The `cont` predicate accept the event, when the event's data attribute contains the value
> 
> **Opposite: `not_cont`**

#### not_cont
> The `not_cont` predicate accept the event, when the event's data attribute does not contains the value
> 
> **Opposite: `cont`**

## Available predicates per attribute type

| Attribute type | Available predicates |
| --- | --- |
| String | [eq](#eq)<br/>[neq](#neq)<br/>[matches](#matches)<br/>[does_not_match](#does_not_match)<br/>[start](#start)<br/>[not_start](#not_start)<br/>[end](#end)<br/>[not_end](#not_end)<br/>[in](#in)<br/>[not_in](#not_in)<br/>[cont](#cont)<br/>[not_cont](#not_cont) |
| Number | [eq](#eq)<br/>[neq](#neq)<br/>[gt](#gt)<br/>[gteq](#gteq)<br/>[lt](#lt)<br/>[lteq](#lteq)<br/>[in](#in)<br/>[not_in](#not_in) |
| DateTime (_in ISO 8601 format_) | [eq](#eq)<br/>[neq](#neq)<br/>[gt](#gt)<br/>[gteq](#gteq)<br/>[lt](#lt)<br/>[lteq](#lteq)<br/>[in](#in)<br/>[not_in](#not_in) |
| Boolean | [eq](#eq)<br/>[neq](#neq) |
