---
title: Restricting Pinject to load only appropriate modules
layout: post
---

I use [Pinject](https://github.com/google/pinject) extensively for dependency injection in my Python code. 
By default Pinject loads all modules that it can lay its hands on. While this generally works, it can lead to trouble when 
code starts to get complicated. The following are the issues that I faced.

1. Pinject forces me to include gdbm and Tkinter under certain cases where Python's Six package is loaded. The issue has also
been raised [here](https://github.com/google/pinject/issues/11)

2. By default Pinject loads all modules that it can lay its hands on, when resolving classes it does not seem to care for the 
package/module from which the class were referenced. As an example consider the following class

		import pinject
		from mycode.authentication import Authentication
		
		class TestClass(object):
		
		  @pinject.copy_args_to_public_fields
		  def __init__(self, authentication):
		    pass 
				
 When Pinject tries to create an object of TestClass, it doesn't automatically identify that authentication field needs to
 be injected with Authentication object defined in mycode.authentication module in cases where it detects Authentication class
 else where lets say in httplib2.Authentication. 
 
Below is my solution to the problem. In the application root, I replace __get_explicit_or_default_modules with a 
function that I define. This function filters the modules that Pinject has to inspect to inject objects into my classes.

	  def my_get_explicit_or_default_modules(modules):
	  """
	  This function modified pinject 
	
	  """
	  if modules is pinject.finding.ALL_IMPORTED_MODULES:
	      return [md for md in sys.modules.values() if md is not None and
	              not str(md.__name__).startswith('six.')
	              and str(md.__name__).startswith('mycode')]
	  elif modules is None:
	      return []
	  else:
	      return modules
				
and then in the root I write the following to replace pinject's function with mine.

    finding._get_explicit_or_default_modules = my_get_explicit_or_default_modules
		
This gets me around issues 1 and 2.
