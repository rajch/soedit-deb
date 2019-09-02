SOURCE_TARBALL=soedit-1.0.tar.gz
SOURCE_DIRECTORY=soedit-1.0
SIGNING_EMAIL=rajch@hotmail.com
CHANGES_FILE=soedit_1.0-1_amd64.changes

usage:
	@echo
	@echo Environment:
	@echo   SOURCE_TARBALL: $(SOURCE_TARBALL)
	@echo   SOURCE_DIRECTORY: $(SOURCE_DIRECTORY)
	@echo   SIGNING_EMAIL: $(SIGNING_EMAIL)
	@echo   CHANGES_FILE: $(CHANGES_FILE)
	@echo
	@echo Usage:
	@echo   Available targets are prebuild-deb, build-deb, prebuild-repo, build-repo
	@echo   The first time, they should be invoked in the order above
	@echo   You can also invoke the targets clean-repo and clean-deb
	@echo

.PHONY: prebuild-deb build-deb clean-deb prebuild-repo build-repo clean-repo

../build:
	mkdir --p ../build

prebuild-deb: ../build
	@[ -f ../build/$(SOURCE_TARBALL) ] && \
	( \
	  cd ../build && \
	    tar -xzvf $(SOURCE_TARBALL) && \
	    cd $(SOURCE_DIRECTORY) && \
	      dh_make --single --email $(SIGNING_EMAIL) --yes --createorig && \
	      cp -r ../../templates/build/$(SOURCE_DIRECTORY)/debian/* debian/ && \
		  echo control, copyright and soedit.manpages files have been copied. && \
		  echo Delete whatever else you do not need. && \
		  echo Then, invoke build-deb \
	) || \
	  echo Please download $(SOURCE_TARBALL) into build directory, and re-invoke prebuild-deb 

build-deb: 
	cd ../build && \
	  cd $(SOURCE_DIRECTORY) && \
	  dpkg-buildpackage --build=full --root-command=fakeroot --sign-key=$(SIGNING_EMAIL)

clean-deb:
	cd ../build && rm -rf *


../docs:
	mkdir -p ../docs

prebuild-repo: ../docs
	cd ../docs && \
	  cp -r ../templates/repo/* . && \
	  sed -i "s/\%SIGNING_EMAIL\%/$(SIGNING_EMAIL)/g" conf/distributions

build-repo:
	cd ../docs && \
	  reprepro --ignore=wrongdistribution include stretch ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include jessie ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include bionic ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include xenial ../build/$(CHANGES_FILE)

clean-repo:
	cd ../docs && \
	  rm -rf *
