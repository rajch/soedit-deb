ORIGIN_URL=https://github.com/rajch/soedit/releases/download/v1.0/soedit-1.0.tar.gz
SOURCE_TARBALL=soedit-1.0.tar.gz
SOURCE_DIRECTORY=soedit-1.0
SIGNING_EMAIL=PLEASE_SET_THIS
SIGNING_KEY=PLEASE_SET_THIS
CHANGES_FILE=soedit_1.0-1_amd64.changes

usage:
	@echo
	@echo Environment:
	@echo ------------
	@echo ORIGIN_URL: $(ORIGIN_URL)
	@echo SOURCE_TARBALL: $(SOURCE_TARBALL)
	@echo SOURCE_DIRECTORY: $(SOURCE_DIRECTORY)
	@echo SIGNING_EMAIL: $(SIGNING_EMAIL)
	@echo SIGNING_EMAIL: $(SIGNING_KEY)
	@echo CHANGES_FILE: $(CHANGES_FILE)
	@echo
	@echo Usage:
	@echo ------
	@echo Available targets are:
	@echo fetch-origin, prebuild-deb, build-deb, prebuild-repo, build-repo
	@echo
	@echo The first time, they should be invoked in the order above.
	@echo You can also invoke the targets clean-repo and clean-deb for cleaning up
	@echo

.PHONY: prebuild-deb build-deb clean-deb prebuild-repo build-repo clean-repo

../build:
	mkdir --p ../build

fetch-origin: ../build
	wget -O ../build/$(SOURCE_TARBALL) $(ORIGIN_URL)

prebuild-deb:
	@[ -f ../build/$(SOURCE_TARBALL) ] && (\
	  [ "$(SIGNING_EMAIL)" != "PLEASE_SET_THIS" ] && (\
	    cd ../build && \
		echo Extracting source tarball... && \
	    tar -xzvf $(SOURCE_TARBALL) && \
	    cd $(SOURCE_DIRECTORY) && \
		echo Calling dh_make... && \
	    dh_make --single --email $(SIGNING_EMAIL) --yes --createorig && \
		echo Copying template build files to debian subdirectory... && \
	    cp -r ../../templates/build/$(SOURCE_DIRECTORY)/debian/* debian/ && \
	  	echo control, copyright and soedit.manpages files have been copied. && \
	  	echo Delete whatever else you do not need. && \
	  	echo Then, invoke build-deb. \
	  ) || \
	    echo Please set SIGNING_EMAIL to the email-address used in the signing key. \
	) || \
	  echo Please download $(SOURCE_TARBALL) into build directory, and re-invoke prebuild-deb.

build-deb: 
	@[ "$(SIGNING_KEY)" != "PLEASE_SET_THIS" ] && (\
	  cd ../build && \
	    cd $(SOURCE_DIRECTORY) && \
	    dpkg-buildpackage --build=full --root-command=fakeroot --sign-key=$(SIGNING_KEY) \
	) || \
	  echo Please set SIGNING_KEY

clean-deb:
	cd ../build && rm -rf *

../docs:
	mkdir -p ../docs

prebuild-repo: ../docs
	@[ "$(SIGNING_KEY)" != "PLEASE_SET_THIS" ] && (\
	  cd ../docs && \
	    echo Copying repo template files... && \
	    cp -r ../templates/repo/* . && \
	    echo Filling signing key $(SIGNING_KEY) in distributions file... && \
	    sed -i "s/\%SIGNING_KEY\%/$(SIGNING_KEY)/g" conf/distributions && \
	    echo Copying ASCII GPG public key to docs/gpg && \
	    gpg --export --armor --output=../docs/gpg $(SIGNING_KEY) \
	) || \
	  echo Please set SIGNING_KEY

build-repo:
	@cd ../docs && \
	  echo Building repo for buster, stretch, jessie, disco, bionic and xenial... && \
	  reprepro --ignore=wrongdistribution include buster ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include stretch ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include jessie ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include disco ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include bionic ../build/$(CHANGES_FILE) && \
	  reprepro --ignore=wrongdistribution include xenial ../build/$(CHANGES_FILE)

clean-repo:
	cd ../docs && \
	  rm -rf *
