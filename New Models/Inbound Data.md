# Inbound Data

Inbound data is a named representation of some block of data.

All inbound data types are abstractly represented, and are considered a [[dependency]] of a model execution run.

Instances of Inbound Data have a type, and any given type has a different method for resolution.  However, a resolution produces some outcome and such an outcome is stored into some [[Datastore]]