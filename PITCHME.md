# The Problem
	Development and automated tests rely on a system which is no longer maintained. Orchestration has effectively been abandoned and is currently kept running
	part time by Kevin Secrist and myself (mostly Kevin).
	
# The Proposal
	We begin to transition some of our test environments to a docker based setup
	
# Product Benefits
	+ Potential reduction in Rackspace Cloud Spend as we reduce the load on Orchestration
	+ Multiple options available for automated tests, reduces impact of a failure with either system
	+ 

# Technical Benefits
	+ RSE team has had success using docker for dev test environments. I already spoke with Jon Armstrong before he left and ran this by him. He provided some helpful guidance.
	+ Cloud Office Ops Team (aka Jimmy et co) believes containers are the future path for our production environment, this gives us a start in that direction and lets 
	us get familiar with containers. They are also an additional source of help & guidance.
	+ Containers appear to be simpler to maintain than the current orchestration system. Hopefully this will encourage more of the devs to help maintain and improve the images
	+ Once the images are built instantiating running containers is very fast (seconds). This should help address a common dev complaint that the smoke tests take to long to run.
	
# Project Plan
	- Build a simple windows container with IIS
	- Run Notifications API in that container
	- Build (or download) additional windows container with SqlServer
	- Build (or download) additional windows (or linux?) container with Mongo
	- Build additional windows (or linux) container with mocktopus
	- Compose an environment with SqlServer, Mongo, mocktopus, and IIS
	- Host Audit Service API in that environment
	- Test Audit Service API in the container environment
		- Run playtypus against the service API to execute the smoke tests
		- Figure out how to capture & compare artifacts (Datapult/DataPillar)
	
# The Result
	Once complete we have an environment that can host any of our service APIs for dev testing 
	and have a pattern to replace the current smoke tests.
	
# Next steps
	- Figure out configuration transformations needed to take containers into production
	- Move Usage API tests into containers
	- Move Apps-Billing tests into containers
	
# Open Questions
	- Can we run both linux & windows containers on a windows host? (it appears we can, but this needs confirmation)
	- How do we inject our code into the image?
		+ Need to be able to update and rerun
		+ RSE handles this by mounting the git directory in the image, this won't work for us (compiled code vs interpreted) but we may be able to do something similar
	- How do we handle configuration
		+ Docker compose supports DNS so that helps with some pieces