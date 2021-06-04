# Basic

All resources should present, at a minimum, the following pieces of information about a cloud resource

-   `Purpose` : Human description for the purpose of the resource
-   `EOLDate` : Simple date (`YYYY-MM-DD`) after which the resource may be destroyed.  If you need a very long-term durable resource use a date far in the future (like `2031-12-31`). Note that `EOLDate` is _inclusive_, so it is the _last day that resource will be valid_. So for `2031-12-22`, the resource can be deleted starting on `2031-12-23`
-   `Owner` : An entity, by local account identifier, that owns the resource.  This should be some notification target
-   `CostCenter` : A [[Cost Center]] designation
-   `Source` : The artifact that was used to create this resource (e.g. `org.infrastructurebuilder:OrangeAde:1.2.3` )
-   `Environment` : The identifying name of an [[environment]] that this resource belongs to. (probably `io.rialtic:OrangeAde:1.2.3:zip:prod` based on above)