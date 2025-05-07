# Working on LCSB Head Node Deployment

The [[LCSB]] needs deploying using the [[LCSB Bootstrap]] VPC and state storage.

Changes
- Make an `lcsb-deploy` repo to separate the code from the deployment
- Introduce a [[Devcontainer]] into the deployment code
- Copy `terraform` and `scripts` to deployment repo under `legacy_code`
- Create a `.envrc` for [[direnv]]
- Stripped out all the conditional logic for system creation if not present.
    - If you want to deploy the sandbox, you have to run the bootstrap and use the bootstrapped state in your sandbox deployment.  It's not hard.  It's just that it's two things instead of one thing.
- 