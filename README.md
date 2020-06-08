[![Build Status](https://travis-ci.com/STaRiCHDED/Lab06.svg?branch=master)](https://travis-ci.com/STaRiCHDED/Lab06)
## Laboratory work VI

<a href="https://yandex.ru/efir/?stream_id=vGWlFO3of-Rg"><img src="https://raw.githubusercontent.com/tp-labs/lab06/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```sh
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
```sh
$ export GITHUB_USERNAME=STaRiCHDED
$ export GITHUB_EMAIL=nik179804@gmail.com
$ sudo apt install vim
$ alias edit=vim
$ alias gsed=sed
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ git clone https://github.com/${GITHUB_USERNAME}/Lab05 projects/Lab06
$ cd projects/
$ cd projects/Lab06
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/Lab06
$ gsed -i '/project(print)/a\
> set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
> ' CMakeLists.txt
$ gsed -i '/project(print)/a\
> set(PRINT_VERSION\
>   \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
> ' CMakeLists.txt
$ gsed -i '/project(print)/a\
> set(PRINT_VERSION_TWEAK 0)
> ' CMakeLists.txt
$ gsed -i '/project(print)/a\
> set(PRINT_VERSION_PATCH 0)
> ' CMakeLists.txt
$ gsed -i '/project(print)/a\
> set(PRINT_VERSION_MINOR 1)
> ' CMakeLists.txt
$ gsed -i '/project(print)/a\
> set(PRINT_VERSION_MAJOR 0)
> ' CMakeLists.txt
$  touch DESCRIPTION && edit DESCRIPTION
$ touch ChangeLog.md
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`"
 cat > ChangeLog.md <<EOF
> * ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
> - Initial RPM release
> EOF
$ cat > CPackConfig.cmake <<EOF
> include(InstallRequiredSystemLibraries)
> EOF
$ cat >> CPackConfig.cmake <<EOF
> set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
> set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
> set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
> set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
> set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
> set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
> set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
> set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
> EOF
$ cat >> CPackConfig.cmake <<EOF
> 
> set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
> set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
> EOF
$ cat >> CPackConfig.cmake <<EOF
> 
> set(CPACK_RPM_PACKAGE_NAME "print-devel")
> set(CPACK_RPM_PACKAGE_LICENSE "MIT")
> set(CPACK_RPM_PACKAGE_GROUP "print")
> set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
> set(CPACK_RPM_PACKAGE_RELEASE 1)
> EOF
$ cat >> CPackConfig.cmake <<EOF
> 
> set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
> set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
> set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
> EOF
$ cat >> CPackConfig.cmake <<EOF
> 
> include(CPack)
> EOF
$ cat >> CMakeLists.txt <<EOF
> 
> include(CPackConfig.cmake)
> EOF
$ gsed -i 's/Lab05/Lab06/g' README.md
$ git add .
$  git commit -m"added cpack config"
[master 9c2cf40] added cpack config
 5 files changed, 40 insertions(+), 5 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION
$ git tag v0.1.0.0
$ git push origin master --tags
$ travis login --auto
Successfully logged in as STaRiCHDED!
$ travis enable
Detected repository as STaRiCHDED/Lab06, is this correct? |yes| y
STaRiCHDED/Lab06: enabled :)
$ cmake -H. -B_build
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/nikitaklimov/STaRiCHDED/workspace/projects/Lab06/_build
$ cmake --build _build
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
$ cd _build
$ cpack -G "TGZ"
$ cd ..
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/nikitaklimov/STaRiCHDED/workspace/projects/Lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
$ mv _build/*.tar.gz artifacts
$ tree artifacts
artifacts
├── print-0.1.0.0-Linux.tar.gz
└── screenshot.png

0 directories, 2 files
$ git add .
$ git commit -m"uploadlab6"
$ git push origin master
```

## Report

```sh
$ popd
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
