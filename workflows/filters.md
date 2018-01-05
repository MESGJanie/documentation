# Filters

Filters are a powerfull way to select only some kind of events. Let's imagine that your application wants to execute a command every time an event occurs but **only** when the data in the event are matching your needs.

In order to do that you can define some data matching between the data from the event and the filter you want

Filters will be defined with an event's attribute and it's predicate(s). The filter will be valid when all the predicates from all the attributes are true. If the attribute doesn't exists or cannot match the predicate, this predicate will be ignored.

## Data attribute predicates

For every types of data that the event will send you have the possibility to apply a predicate according to the type of data

#### String

- **equals_to**: the event's data attribute exactly matches the given value
- **not_equals_to**: the event's data attribute is different than the given value
- **contains**: the event's data attribute contains the given value
- **starts_with**: the event's data attribute starts with the given value
- **ends_with**: the event's data attribute ends with the given value
- **matches**: the event's data attribute match the given regexp value

#### Number

- **equals_to**: the event's data attribute exactly matches the given value
- **not_equals_to**: the event's data attribute is different than the given value
- **greater_than**: the event's data attribute is greater than the given value
- **lower_than**: the event's data attribute is lower than the given value

#### Date

All dates values must be set in ISO 8601 format (eg: `"2018-01-05T10:30:00.000Z"`)

- **equals_to**: the event's data attribute exactly matches the given value
- **not_equals_to**: the event's data attribute is different than the given value
- **greater_than**: the event's data attribute is greater than the given value
- **lower_than**: the event's data attribute is lower than the given value
