cmake_minimum_required (VERSION 2.8.0)

project (PoromechanicDeformation)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Location to search files or libraries
set(SEARCH_DIRS ENV PATH /usr/local/opt/ /usr/include /usr/local/include /usr/lib /usr/lib64 /usr/local/lib .. )

#enabling NEOPZ library
find_path(NEOPZ_INCLUDE TPZGeoElement.h PATHS ${SEARCH_DIRS})
if(NEOPZ_INCLUDE-NOTFOUND)
	set (NEOPZ_INCLUDE "" CACHE PATH "Directory where NEOPZ header files can be found")
else()
	include_directories(${NEOPZ_INCLUDE})
endif()

find_library(NEOPZ_LIB NAMES libpz.a PATHS ${SEARCH_DIRS})
if(NEOPZ_LIB-NOTFOUND)
    set (NEOPZ_LIB "" CACHE PATH "Directory where the NEOPZ library can be found")
endif()

#enabling pthread library
find_path(PTHREAD_INCLUDE NAMES PThread.h pthread.h PATHS ${SEARCH_DIRS} ${PROJECT_SOURCE_DIR}/../externallibs/pthread ${PROJECT_SOURCE_DIR}/../externallibs/pthread/include ${PROJECT_SOURCE_DIR}/../pthread )
find_library(PTHREAD_LIB NAMES libpthread.dylib pthread.lib pthreadVC2.lib libPThread.so libPThread.a libpthread.so libpthread.a PATHS ${SEARCH_DIRS} ${PROJECT_SOURCE_DIR}/../externallibs/pthread/lib  ${PROJECT_SOURCE_DIR}/../pthread/lib)
if(PTHREAD_INCLUDE-NOTFOUND)
	set (PTHREAD_INCLUDE "" CACHE PATH "Directory where pthread.h can be found")
endif()
if(PTHREAD_LIB-NOTFOUND)
	set (PTHREAD_LIB "" CACHE PATH "Directory where the pthread library can be found")
endif()
include_directories(${PTHREAD_INCLUDE})


# Adding projec files

file(GLOB header *.h)
file(GLOB source *.cpp)
add_executable(PoromechanicDeformation ${source} ${header})
target_link_libraries(PoromechanicDeformation ${NEOPZ_LIB} ${PTHREAD_LIB})




