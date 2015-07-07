************
Introduction
************

THIS PACKAGE IS DEPRECATED

PLEASE USE THE NEW PACKAGE WHICH WILL BE DEVELOPED HERE:
https://github.com/armstrtw/Rawscli


:Date: October 5, 2011
:Authors: Whit Armstrong
:Contact: armstrong.whit@gmail.com
:Web site: http://github.com/armstrtw/AWS.tools
:License: GPL-3


Purpose
=======

AWS.tools is an R package for Amazon Web Services EC2/S3.


Features
========

* AWS.tools can fire and terminate EC2 instances.

* AWS.tools can upload and pull back S3 objects from R.

* Look for more features shortly.


Usage
=====

A minimal example of borrowed from JD Long::

	library(AWS.tools)
	library(rzmq)
	
	
	cl <- startCluster(ami="ami-a531fccc",key="my-key",instance.count=4,instance.type="m1.large")
	master.node <- get.master(cl)
	
	estimatePi <- function(seed) {
	    set.seed(seed)
	    numDraws <- 1e6
	
	    r <- .5
	    x <- runif(numDraws, min=-r, max=r)
	    y <- runif(numDraws, min=-r, max=r)
	    inCircle <- ifelse( (x^2 + y^2)^.5 < r , 1, 0)
	    
	    sum(inCircle) / length(inCircle) * 4
	}
	
	print(system.time(ans <- zmq.lapply(as.list(1:1e4),
	                                    estimatePi,
	                                    execution.server=paste("tcp://",master.node$PublicDNS,":6000",sep=""),
	                                    sink.server=paste("tcp://",master.node$PublicDNS,":6001",sep=""))))
	
	res <- terminateCluster(cl)
	
