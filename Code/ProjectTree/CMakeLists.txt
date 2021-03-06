CMAKE_MINIMUM_REQUIRED(VERSION 3.18.1 FATAL_ERROR)
SET(PROJECTNAME ProjectTree)
PROJECT(${PROJECTNAME})
ADD_DEFINITIONS("-DPROJECTTREE_API")

#Qt
IF(NOT DEFINED Qt5_DIR)
	SET(Qt5_DIR "Qt5_DIR-NOTFOUND" CACHE PATH "Qt5_DIR")
	MESSAGE(SEND_ERROR "Qt5_DIR is null, please find Qt5_config.cmake file position!~")
ENDIF()
#VTK
IF(NOT DEFINED FASTCAE_VTK_INCLUDE)
	SET(FASTCAE_VTK_INCLUDE "FASTCAE_VTK_INCLUDE-NOTFOUND" CACHE PATH "FASTCAE_VTK_INCLUDE")
	MESSAGE(SEND_ERROR "FASTCAE_VTK_INCLUDE is null, please specified VTK include directory!~")
ENDIF()
IF(NOT DEFINED FASTCAE_VTK_LIBDIR)
	SET(FASTCAE_VTK_LIBDIR "FASTCAE_VTK_LIBDIR-NOTFOUND" CACHE PATH "FASTCAE_VTK_LIBDIR")
	MESSAGE(SEND_ERROR "FASTCAE_VTK_LIBDIR is null, please specified VTK release library directory!~")
ENDIF()
IF(NOT DEFINED FASTCAE_VTK_LIBDDIR)
	SET(FASTCAE_VTK_LIBDDIR "FASTCAE_VTK_LIBDDIR-NOTFOUND" CACHE PATH "FASTCAE_VTK_LIBDDIR")
	MESSAGE(SEND_ERROR "FASTCAE_VTK_LIBDDIR is null, please specified VTK debug library directory!~")
ENDIF()
IF(NOT DEFINED FASTCAE_VTK_BINDIR)
	SET(FASTCAE_VTK_BINDIR "FASTCAE_VTK_BINDIR-NOTFOUND" CACHE PATH "FASTCAE_VTK_BINDIR")
	MESSAGE(SEND_ERROR "FASTCAE_VTK_BINDIR is null, please specified VTK release dll directory!~")
ENDIF()
IF(NOT DEFINED FASTCAE_VTK_BINDDIR)
	SET(FASTCAE_VTK_BINDDIR "FASTCAE_VTK_BINDDIR-NOTFOUND" CACHE PATH "FASTCAE_VTK_BINDDIR")
	MESSAGE(FATAL_ERROR "FASTCAE_VTK_BINDDIR is null, please specified VTK release dll directory!~")
ENDIF()

IF(NOT DEFINED FASTCAE_VTK_LIBDS)
	FILE(GLOB FASTCAE_VTK_LIBDS "${FASTCAE_VTK_LIBDDIR}/*.lib")
ENDIF()
IF(NOT DEFINED FASTCAE_VTK_LIBS)
	FILE(GLOB FASTCAE_VTK_LIBS "${FASTCAE_VTK_LIBDIR}/*.lib")
ENDIF()

#FILE(GLOB ProjectTree_TS_FILE "${TOP_PROJECT_SOURCE_DIR}/qrc/translations/ProjectTree_zh_CN.ts")
FILE(GLOB SRC_UI_FILES "*.ui")
FILE(GLOB SRC_H_FILES "*.h")
FILE(GLOB SRC_CPP_FILES "*.cpp")

#SET_SOURCE_FILES_PROPERTIES(${ProjectTree_TS_FILE} PROPERTIES OUTPUT_LOCATION "${TOP_PROJECT_SOURCE_DIR}/qrc/translations")
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${TOP_PROJECT_SOURCE_DIR}/../output/bin_d)    
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${TOP_PROJECT_SOURCE_DIR}/../output/bin) 
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${TOP_PROJECT_SOURCE_DIR}/../output/bin_d)   
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${TOP_PROJECT_SOURCE_DIR}/../output/bin) 

INCLUDE_DIRECTORIES(${TOP_PROJECT_SOURCE_DIR} 
					${FASTCAE_VTK_INCLUDE})

#FIND_PACKAGE(Qt5 COMPONENTS Core Widgets Xml LinguistTools REQUIRED)
FIND_PACKAGE(Qt5 COMPONENTS Core Widgets Xml REQUIRED)
#QT5_CREATE_TRANSLATION(ProjectTree_QM_FILE ${PROJECT_SOURCE_DIR} ${ProjectTree_TS_FILE} OPTIONS -noobsolete)
QT5_ADD_RESOURCES(QF_Generated_QRC_FILE ${QF_QRC_FILE})
QT5_WRAP_UI(SRC_Generated_UI_FILES ${SRC_UI_FILES})
SET(CMAKE_AUTOMOC ON)

#SOURCE_GROUP("Translation" FILES ${ProjectTree_QM_FILE} ${ProjectTree_TS_FILE})
SOURCE_GROUP("rc" FILES ${QF_Generated_QRC_FILE} ${QF_QRC_FILE})
SOURCE_GROUP("Ui" FILES ${SRC_Generated_UI_FILES} ${SRC_UI_FILES})
SOURCE_GROUP("Generated Files" FILES "${TOP_PROJECT_BINARY_DIR}/${PROJECTNAME}/${PROJECTNAME}_autogen/mocs_compilation.cpp")

ADD_LIBRARY(${PROJECTNAME} SHARED #${ProjectTree_TS_FILE}
								  #${ProjectTree_QM_FILE}
								  ${QF_Generated_QRC_FILE}
								  ${SRC_Generated_UI_FILES}								   
								  ${SRC_H_FILES}
								  ${SRC_CPP_FILES})
								   
TARGET_LINK_LIBRARIES(${PROJECTNAME} Qt5::Widgets Qt5::Xml)
TARGET_LINK_LIBRARIES(${PROJECTNAME} debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/Settings${CMAKE_DEBUG_POSTFIX}.lib"         optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/Settings${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/PythonModule${CMAKE_DEBUG_POSTFIX}.lib"     optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/PythonModule${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/DataProperty${CMAKE_DEBUG_POSTFIX}.lib"     optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/DataProperty${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/MeshData${CMAKE_DEBUG_POSTFIX}.lib"         optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/MeshData${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/Geometry${CMAKE_DEBUG_POSTFIX}.lib"		    optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/Geometry${CMAKE_RELEASE_POSTFIX}.lib"			
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/SelfDefObject${CMAKE_DEBUG_POSTFIX}.lib"	optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/SelfDefObject${CMAKE_RELEASE_POSTFIX}.lib"					 
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/Material${CMAKE_DEBUG_POSTFIX}.lib"         optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/Material${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/BCBase${CMAKE_DEBUG_POSTFIX}.lib"           optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/BCBase${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/ConfigOptions${CMAKE_DEBUG_POSTFIX}.lib"    optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/ConfigOptions${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/ParaClassFactory${CMAKE_DEBUG_POSTFIX}.lib" optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/ParaClassFactory${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/ModelData${CMAKE_DEBUG_POSTFIX}.lib"        optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/ModelData${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/ModuleBase${CMAKE_DEBUG_POSTFIX}.lib"       optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/ModuleBase${CMAKE_RELEASE_POSTFIX}.lib"
									 debug "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG}/PostWidgets${CMAKE_DEBUG_POSTFIX}.lib"      optimized "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE}/PostWidgets${CMAKE_RELEASE_POSTFIX}.lib")				 
ADD_DEPENDENCIES(${PROJECTNAME} GmshModule)
									 
FOREACH(FASTCAE_VTK_LIBD ${FASTCAE_VTK_LIBDS})
	TARGET_LINK_LIBRARIES(${PROJECTNAME} debug ${FASTCAE_VTK_LIBD})
ENDFOREACH()	
FOREACH(FASTCAE_VTK_LIB ${FASTCAE_VTK_LIBS})
	TARGET_LINK_LIBRARIES(${PROJECTNAME} optimized ${FASTCAE_VTK_LIB})
ENDFOREACH()									 