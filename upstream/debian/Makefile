PREP = prep
GIT_ROOT = $(shell git rev-parse --show-toplevel)

$(PREP): 
	#First time run requires update to install pkgs
	sudo apt-get update
	#Install pbuilder and copy the conf file
	if [ -d "/var/cache/pbuilder" ]; then \
		@echo "Folder found"; \
		cp utils/.pbuilderrc ~/; \
	else \
		@echo "Folder not found"; \
		sudo apt-get install pbuilder debootstrap devscripts; \
		sudo cp utils/.pbuilderrc ~/; \
		sudo pbuilder create; \
	fi; \
	sudo pbuilder update
	mkdir -p $(GIT_ROOT)/../build

kazoo:$(PREP)
	#Copy the kazoo to build dir and prepare a .orig file
	cp -R kazoo/kazoo-1.3.1 $(GIT_ROOT)/../build/
	(cd $(GIT_ROOT)/../build ; rm -rf kazoo-1.3.1/debian/)
	(cd $(GIT_ROOT)/../build; tar -zcvf kazoo_1.3.1.orig.tar.gz kazoo-1.3.1)
	cp -Rf kazoo/kazoo-1.3.1 $(GIT_ROOT)/../build/
	
	#Build the kazoo from the BUILD dir	
	(cd $(GIT_ROOT)/../build/kazoo-1.3.1; sudo pdebuild --use-pdebuild-internal --debbuildopts -tc)

all:$(PREP) \
	kazoo