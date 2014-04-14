damp.ekeko.releng
=================

This repository serves the continuous integration of all [Ekeko](https://github.com/cderoove/damp.ekeko)-related projects.
Whenever this repository is updated, the [travis-ci](https://travis-ci.org/) cloud service automatically retrieves, builds and tests the latest code of the following projects:

- [Ekeko](https://github.com/cderoove/damp.ekeko) (aka ``damp.ekeko``) - a Clojure library for applicative logic meta-programming against Eclipse projects.
- [Ekeko/X](https://github.com/cderoove/damp.ekeko.snippets) (aka  ``damp.ekeko.snippets``) - an Ekeko extension for template-based program querying and transformation of Java projects.
- [GASR](https://github.com/cderoove/damp.ekeko.aspectj) (aka ``damp.ekeko.aspectj``) - an Ekeko extension for querying AspectJ projects. 


The status of the last continuous integration build is: [![Build Status](https://travis-ci.org/cderoove/damp.ekeko.releng.svg?branch=master)](https://travis-ci.org/cderoove/damp.ekeko.releng). <br/>
See 
[https://travis-ci.org/cderoove/damp.ekeko.releng](https://travis-ci.org/cderoove/damp.ekeko.releng) for the full log.


###  Adding a new Ekeko-related project to the build

The following instructions assume you are working on a local working copy of this repository:

- Ensure the new project can be built using the [eclipse-tycho](https://www.eclipse.org/tycho/) plugin for [maven](http://maven.apache.org/). The Vogella [tutorial](http://www.vogella.com/tutorials/EclipseTycho/article.html) can be consulted if needed. See the ``pom.xml`` files in [Ekeko/X](https://github.com/cderoove/damp.ekeko.snippets) and each subdirectory for a concrete example. 

- Add a new git submodule to this repository, making sure to track the project's master branch and to use a publicly accessible url:<br/>
``git submodule add -b master git://github.com/cderoove/damp.ekeko.snippets.git``

- Run the included script to update all submodules to their latest commit:<br/>
``./update_root_and_submodules``

- Update the ``pom.xml`` in this repository such that its ``modules`` section references the newly added submodule. 

- Run ``mvn clean verify`` to build and test all projects. 

- Committing and pushing your changes will trigger a new build at [https://travis-ci.org/cderoove/damp.ekeko.releng](https://travis-ci.org/cderoove/damp.ekeko.releng),

- If desired, the ``deploy`` script can be run to deploy all resulting Eclipse plugins to the Eclipse update-site: [http://soft.vub.ac.be/~cderoove/eclipse](http://soft.vub.ac.be/~cderoove/eclipse)

- If desired, update ``.travis.cl`` to be notified by e-mail about the status future builds (see ) 

###  Known issues

- A daily rebuild of this project has been scheduled at [http://traviscron.pythonanywhere.com/](http://traviscron.pythonanywhere.com/). It would be nice  if such a build could be triggered as soon as a subproject was updated. This would require some post-build hook into a ``.travis.cl`` of each subproject. Patches are welcomed.

- The ``pom.xml`` files for the [Ekeko/X](https://github.com/cderoove/damp.ekeko.snippets) and [GASR](https://github.com/cderoove/damp.ekeko.aspectj) subprojects assume that a snapshot build of [Ekeko](https://github.com/cderoove/damp.ekeko) has been installed in the local maven repository. This happens automatically when this project is built, but not when a subproject is built on its own. Running ``mvn -Pbuild-individual-bundles clean verify`` in the subproject will cause maven to look for the Ekeko snapshot in the public update-site, but such a snapshot cannot always be found  as its identifier carries a timestamp. All ``pom.xml`` files configure the ``tycho-buildtimestamp-jgit`` plugin to obtain [reproducible version qualifiers](https://wiki.eclipse.org/Tycho/Reproducible_Version_Qualifiers), but this does not seem to work. Again, patches are welcomed. For now, the only workaround is to build all subprojects together from this repository rather than individually. 
