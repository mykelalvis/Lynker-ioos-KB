# Config of a Model for Running Models
A [[Model Execution Configuration]] defines 
- [[Inbound Data]] - What do we need and where to run the model
	- [[Data Source]] - An origianl source of data
- [[Required Image]] - What executable is required for the model
- Set of [[Execution Dependency]] elements, 
	- Things needed for the [[Required Image]] to run
	- Including the [[Execution Hardware]] (a specialized form of an [[Execution Dependency]])
- The commands necessary to invoke the [[Required Image]] and start the execution/ "model run"
	- This could be implicit, according to the [[Required Image]]
- The [[Outbound Data]] that should result.  Specifically, where will it go.
- The [[Monitoring Information]] that allow us to know that the model is still executing
	- Logs or [[Outbound Data]] files being written?
	- Execution/CPU utilization?
	

# Mock Example

