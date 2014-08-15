# add the talk here


Introduction
What is a monolith
Why is it good?
Why is it bad?
What is a micro service
Rewrite?

What did we do?
	E-conomic have two products
		Two monoliths, two git repositories

	Identified some easy wins
		Image conversion
		Sending of emails

	First service, image conversion in a monolith
		Each conversion spawned graphics magick
		Unstable conversion gave an unstable app
		We wanted to sandbox the image conversion

		Containment (image converter)
			Created new repo for image converter
			Same structure, same language, same modules (small steps)
			Seperate deployment, same deployment cycle, two week sprints

		Learnings (image converter)
			Pros
				Isolation
				Seperate scaling
				Seperate memory and CPU usage
			Cons
				Had to add new ELB, pingdom
				Developers on several platforms, should work on all platforms

			First service should give a great experience
			Happy "bosses" means more services
			Small steps. There's a time for everything. Developers are conservative.

	Second service, send emails
		Spearheaded by people excited about the first service
		What sending email used to entail
			Gets data from database
			Uses templates with data
			Send email with some module
		What sending email was afterwards
			Gets data from database
			Uses templates with data
			Send email with some module
		This was not good...

		Learnings
			Cons
				Coupled to the database
				Coupled to templates
				Database was now a global dependency
				Communication through webhooks
			Pros
				Less code base
				Easier deployment
				Seperate memory and CPU usage
				
			Basically, the code was just taken out directly out
			Should have a queue, but wasn't the time
				Sort of implemented our own queue using the database

	The next level, added a few more services
		Culture changed
		New feature -> "Why don't we do that as a service?"

		Lack of infrastructure
			Hardcoded paths/hostnames/load balancers
			Queueing became more of an issue
			Deployment was only done by a few people
			Got mandate to change infrastructure
			Logging? Monitoring?

		Event bus
			It was now time to go deeper in the stack
			Instead of hackish web hooks we looked into events/queues
			Settled with Redis, easy to set up
			First introduction, "wait wut - we want events in everything!"

		Routing
			Addd ETCD as service registry
			Needed to scale in number of services
			Access (internal/external services)

		Deployment
			We set up continuos deployment on one of the main products
			Uses internal/external tools to make most people able to deploy

		Logging
			Set up standardized logging

		Monitoring
			Able to see status of each service, call up people when Mathias

	Services everywhere
		This is where we are now
		Most new features are somehow based on a service
		







How to change into micro services
	Identify some easy wins
		Stateless
		No/little business logic
		Generic problems
		Image conversion
		Sending of emails

	Service communication
		Keep it simple, use http
		Forget about "so webscale"
		In big co's focus on fewer technologies, conservative
		Transparency in infrastructure

	Environments
		Make sure you have different environments (production, staging, playground, development)
		Local development contains way more moving parts

	Deployment
		Completely automatic is the goal
		All levels of developers need to use it
		Should encourage easy deployments

	Routing
		Non-trivial component, pick one to keep
		Educate people, make sure they don't hardcode uri's

	Company communication
		Make sure others understand micro services

