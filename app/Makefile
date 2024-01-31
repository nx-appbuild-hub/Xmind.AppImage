# Copyright 2020 Alex Woroschilow (alex.woroschilow@gmail.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
PWD:=$(shell pwd)

.PHONY: all

all: clean init
	mkdir --parents $(PWD)/build/Boilerplate.AppDir/xmind
	$(PWD)/bin/Apprepo.AppImage --destination=$(PWD)/build appdir boilerplate \
		libgdk-pixbuf2.0-0 libatk1.0-0 libatk-bridge2.0-0 libgtk2.0-0 \
		libreadline8 at-spi2-core libpango-1.0-0 libselinux1 \
		libtinfo6 libncurses6 libtinfo5 libgtk-3-0 libwayland-cursor0

	wget --output-document="$(PWD)/build/build.deb" https://dl3.xmind.net/XMind-for-Linux-amd64bit-12.0.3-202206261857.deb
	dpkg -x $(PWD)/build/build.deb $(PWD)/build

	cp --force --recursive $(PWD)/build/opt/XMind/* 	$(PWD)/build/Boilerplate.AppDir/xmind
	cp --force --recursive $(PWD)/AppDir/XMind.ini      $(PWD)/build/Boilerplate.AppDir/xmind

	chmod 4755 $(PWD)/build/Boilerplate.AppDir/xmind/xmind

	echo "LD_LIBRARY_PATH=\$${LD_LIBRARY_PATH}:\$${APPDIR}/xmind" 	>> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo "export LD_LIBRARY_PATH=\$${LD_LIBRARY_PATH}" 				>> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo "exec \$${APPDIR}/xmind/xmind \"\$${@}\"" 		            >> $(PWD)/build/Boilerplate.AppDir/AppRun

	rm -f $(PWD)/build/Boilerplate.AppDir/*.desktop 		|| true
	rm -f $(PWD)/build/Boilerplate.AppDir/*.png 		  	|| true
	rm -f $(PWD)/build/Boilerplate.AppDir/*.svg 		  	|| true

	cp --force $(PWD)/AppDir/*.desktop 			$(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.png 				$(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.svg 				$(PWD)/build/Boilerplate.AppDir/ || true

	 export ARCH=x86_64 && $(PWD)/bin/appimagetool.AppImage $(PWD)/build/Boilerplate.AppDir $(PWD)/XMind.AppImage
	 chmod +x $(PWD)/XMind.AppImage

init:
	mkdir --parents $(PWD)/bin

	wget https://github.com/area-of-dev/apprepo.AppImage/releases/download/latest/apprepo.AppImage \
		--output-document=$(PWD)/bin/Apprepo.AppImage
	chmod +x $(PWD)/bin/Apprepo.AppImage

	wget https://github.com/AppImage/AppImageKit/releases/download/13/appimagetool-x86_64.AppImage \
		--output-document=$(PWD)/bin/appimagetool.AppImage
	chmod +x $(PWD)/bin/appimagetool.AppImage

clean:
	rm -rf $(PWD)/*.AppImage
	rm -rf $(PWD)/bin/*
	rm -rf $(PWD)/build
