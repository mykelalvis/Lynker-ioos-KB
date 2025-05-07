# What Does It Take?

*The Build Environment*

One of the greater hurdles when I arrived on this project was understanding how to build and execute a hydrologic “model”.   Coming from a background of industrial engineering practices, there appears to be a great deal of what I will refer to as “artisanal” work that goes into the execution of a model.  While a lot of this is probably exactly what needs to be done, if the model will ever be run more than once (or more important, in scientific terms, by someone else to validate its output), the ability to reliably transport the execution of the model to another platform is essential.  

In order to do that transport, it is necessary to understand how it is built in the first place.  Building a hydrologic model (or anything, really) requires that we know some basic elements of the build.  Foremost of these is the [[build environment]].

First are some assumptions:

1. The model will only be run on a linux box.
	1. Specifically, a RedHat-compatible Linux Box
    1. Even more specifically, Alma linux to keep it accessible.
    2. For current purposes, this will only be done on AWS.
       However, that assumption could easily need to change for a wide variety of reasons.
2. The model will be run on some form of virtualized hardware, such as an EC2 instance on AWS or as an ECS task or inside Docker or Kubernetes.
3. At [[model runtime environment|runtime]]: 
    1. The model will likely have some data that it needs to access at some known location.
    	1. This needs to be configurable at some level.  
    	2. Configuration could mean that the input lives below some compiled code’s location.
    2. The model will need storage for delivering its output.
    	1. Same as inputs, this needs to be configurable.
    	2. Same as inputs, the configuration might be faulty per the model’s requirements.

I think the elements of a build are (kinda in-order):

1. The source code.  This can be in any format that anyone wants, but for purposes of our discussion, we’ll say it needs to be an archive (zip or tarball)
	1. This is something that is easy to fetch from github.  Cloning a repo is less-easy, and doesn’t actually accomplish what I want to do.
	2. This would be expanded to an arbitrary location on a filesystem.  If it has to be at a certain location, that’s  a Bad Thing™.
	3. Afterwards, dos2unix would be run against the tree to ensure that line-endings.  This is important and a little scary, because it could really mess up certain kinds of data.
2. Install “dependencies”
	1. A dependency is (literally) anything that needs to be done before pre-build configuration can be attempted.
		1. In a broad sense, the installation of the source code from above is just another dependency, but as the ***primary*** source artifact it needs to have at least slightly higher billing.
	2. Any arbitrary dependency is itself dependent on the attributes of the underlying running environment that is being configured to build the code.
	> For example, as we have determined that all code will be run on Alma linux, then the dependencies that are installed must necessarily be the ones that work on whatever version of the OS and whatever processor architectures are to be selected for execution.  Elements such as “number of available cores” and “amount of available memory” and “available networks” are generally more the focus of the [[model runtime environment]].
	4.   
3. Prebuild Configuration
4. Executed generation of executable code
5. Collection of executable code into an archive

Once the final step of “the build” is performed, there should exist a single artifact that contains all the code needed to execute the model.**