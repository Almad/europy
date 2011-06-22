
Paver: the build tool you missed
=================================

![Alt text](paver_banner.jpg)

Lukas Linhart
-----------------------

Centrum Holdings
-----------------------

-------------------------------------

About Me
=====================================

* Regular pythonista since '05
* Testing and automation obsession
* Centrum Holdings, mostly Django-related work
  * Lots of smallish projects

-------------------------------------


Stumbling upon Paver
=====================================

* "Setuptools no more"

* Kevin Dangoor To The Rescue

-------------------------------------

Survey
=====================================

Who would use a build tool...

-------------------------------------


Build tool in an interpreted world
=====================================

* Generated Content In Repository (TM)
* Moving (files) around

-------------------------------------


...and why in Python?
=====================================

* Batteries included (tm)
* Integration with pythonic tools

-------------------------------------


...and why Paver?
=====================================

* Simple things stay simple
* Complex is possible (or integrated)

-------------------------------------

Single Point of Entry
=====================================

As in "Console API"

	!sh
	paver bootstrap
	paver prepare
	paver run


-------------------------------------

Getting started (First Task!)
=====================================

`pavement.py`:
	
	!python
	from paver.easy import *

	@task
	def install_dependencies():
		sh('pip install --upgrade -r requirements.txt')


-------------------------------------


Embrace distutils/setuptools
=====================================


	!python
	from paver.easy import *
	from paver.setuputils import setup

	setup(**same_args_as_in_setup)

-------------------------------------


setup.py compatibility
=====================================

	paver minilib
	
	paver generate_setup
	
	(pip install -e worky)

-------------------------------------


Dependencies
=====================================

	!python
	@task
	@needs('install_dependencies')
	def prepare():
		""" Prepare complete environment """
		sh("python setup.py develop")

-------------------------------------



Overwriting distutils commands
=====================================

	!python
	@task
	@needs('html', "minilib", "generate_setup", "distutils.command.sdist")
	def sdist():
		pass


-------------------------------------

Positional arguments
=====================================

	!python
	@task
	@consume_args
	def unit(args):
		import nose
		nose.run_exit(
			argv = ["nosetests"] + args
		)


-------------------------------------

CMD options, GNU style
=====================================

	!python
	@task
	@cmdopts([
		('domain-username=', 'd', 'Domain username'),
		('upload-url=', 'u', 'URL to upload to')
	])
	@needs('download_diff_packages')
	def upload_packages(options):
		# censored

-------------------------------------

Oh, options
=====================================

	!python
	options(
		minilib=Bunch(
			extra_files=['doctools', 'virtual']
		)
	)
	
-------------------------------------


Options (cont.)
=====================================

	!python
	# inside minilib task
	options.get('extra_files', [])


-------------------------------------

Namespace search
=====================================
	
	!python
	options(setup=Bunch(version="1.1"))
	options.version
	'1.1'
	options.order('minilib')
	options.version
	AttributeError: version


-------------------------------------

sh
=====================================

	!python
	myval = sh("cat /etc/fstab", capture=True)

-------------------------------------

dry
=====================================

	!python
	# prepare
	dry("Modify", do_fs_mumbo_jumbo)

-------------------------------------


path.py 
=====================================

	!python
	@task
	def publish_docs():
		builtdocs = path("docs") / options.sphinx.builddir / "html"
		destdir = options.docroot
		destdir.rmtree()
		builtdocs.move(destdir)

-------------------------------------

Documentation (sphinx)
=====================================

	!python
	options(
		sphinx=Bunch(
			builddir="build",
			sourcedir="source"
		)
	)

And run

	!sh
	paver html



-------------------------------------

Documentation (cog)
=====================================

	!python
	#<== include('started/oldway/setup.py')==>
	#<==end==>

configured with

	!python
	options(
	    cog=Bunch(
		includedir="docs/samples",
		beginspec="<==",
		endspec="==>",
		endoutput="<==end==>"
	    )
	)




-------------------------------------


Virtualenv
=====================================


	!python
	options(
		virtualenv=Bunch(
			packages_to_install=["nose", "virtualenv"],
			install_paver=True,
			script_name='bootstrap.py',
			paver_command_line=None,
			dest_dir="virtualenv"
		)
	)

-------------------------------------

Discovery
=====================================


-------------------------------------

Setting up django discovery
=====================================
	
	!python
	from paver.discovery import discover_django

	options(
		discovery = Bunch(
			django = Bunch(
				settings_path = "subdir"
			)
		)
	)

	discover_django(options)

-------------------------------------

Happy Django command panda
=====================================

	paver django.validate

-------------------------------------

Future
=====================================

* virtualenv improvements
  * VIRTUAL_ENV context autodetection
* more integration (first-class fabric, ...)
* what would you like?


-------------------------------------

Q & (some) A
=====================================

Fork us on github
------------------

http://github.com/paver/paver/
-------------------------------

paver@googlegroups.com
-------------------------------

