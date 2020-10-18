cmake_policy(VERSION 3.9)
cmake_minimum_required(VERSION 3.9)
project( 07_ChessDemo )
#Cv34QRibbonCmkClsWithDocCuda8Vs15
if(1)
    set (PclVision_VERSION_MAJOR 2)
    set (PclVision_VERSION_MINOR 202006261423)
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
SET(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++11")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")
message(${CMAKE_CXX_FLAGS})

#------------------------------------------qt5 must -----------------------------------
#注意，在一个大工程里，这几个开关一定要尽量放在前面打开，否则有可能会报无法生成ui文件的错误
set(CMAKE_AUTOMOC ON)#自动翻译信号与槽
set(CMAKE_AUTORCC ON)#自动翻译资源文件为cpp文件
set(CMAKE_AUTOUIC ON)#自动翻译界面文件为cpp文件
set(CMAKE_INCLUDE_CURRENT_DIR ON)

#-----------------------------------------qt5-------------------------------
message( "(0)(lk)________ Qt5 _______:  ")
FIND_PACKAGE (QT5 REQUIRED)
INCLUDE_DIRECTORIES (${CMAKE_CURRENT_BINARY_DIR})
ADD_DEFINITIONS (${QT_DEFINITIONS})

find_package(Qt5Widgets)
set(CMAKE_PREFIX_PATH $ENV{QTDIR594})
find_package(Qt5 COMPONENTS Core Gui  Widgets Qml Quick REQUIRED)
find_package(Qt5Core)
find_package(Qt5Widgets)
find_package(Qt5Gui)
find_package(Qt5OpenGL)
find_package(Qt5Xml)
message( "  find   qt5   finish.\n\n")

#----------------------------------------header soruces -------------------------------
#set(HEADERS##    mainwindow.h    )
set(SOURCES    main.cpp    )
set(RESOURCES    image.qrc    )
#set(QMLS    main.qml )
qt5_wrap_ui(ui_FILES *.ui   )
file(GLOB_RECURSE FORMS "./*.ui"   "./UISet/*.ui"   )

# 搜集指定目录 . 和./MyClass 下的源文件，将输出结果列表储存到变量ALL_SOURCE中   MainApp
aux_source_directory(. vision_SOURCE)
aux_source_directory(${PROJECT_SOURCE_DIR}/QRibbon     vision_SOURCE)
aux_source_directory(${PROJECT_SOURCE_DIR}/Classify     vision_SOURCE)
aux_source_directory(${PROJECT_SOURCE_DIR}/CoreFunctions     vision_SOURCE)
aux_source_directory(${PROJECT_SOURCE_DIR}/UISet     vision_SOURCE)
message( "\n\n\n __________________ vision_SOURCE ____________________" )
message(  ${vision_SOURCE}  )
message( "\n\n\n" )

INCLUDE_DIRECTORIES( ${PROJECT_SOURCE_DIR}
    "${PROJECT_SOURCE_DIR}/UISet"
    "${PROJECT_SOURCE_DIR}/CoreFunctions"
    "${PROJECT_SOURCE_DIR}/Classify"
    )
# 包含进目录 gen_pcd/
include_directories("${PROJECT_BINARY_DIR}")
#ui_mainwindow.h
link_directories("${PROJECT_BINARY_DIR}/lib")
include_directories("${PROJECT_SOURCE_DIR}/VisionUI")
message( "\n\n\n __________________PROJECT_SOURCE_DIR____________________" )
message(  ${PROJECT_SOURCE_DIR}  )
message( "\n\n\n")
message( "\n\n\n _________________ PROJECT_BINARY_DIR  __________________" )
message(  ${PROJECT_BINARY_DIR}  )
message("\n\n\n" )


set( Lib_dep_root    "D:/caffeBundle/libraries_v140_x64_py35_1.1.0/libraries")

if( 0 )
    message( "\n___________BOOST__________________")
    ### ---[ 3rd party libraries
    macro(find_boost)
        if(PCL_ALL_IN_ONE_INSTALLER)
            set(BOOST_ROOT "${Lib_dep_root}")
        elseif(NOT BOOST_INCLUDEDIR)
            set(BOOST_INCLUDEDIR "${Lib_dep_root}/include/boost-1_61")
        endif(PCL_ALL_IN_ONE_INSTALLER)
        set(BOOST_ROOT "${Lib_dep_root}")
        set(BOOST_INCLUDEDIR "${Lib_dep_root}/include/boost-1_61")
        # use static Boost in Windows
        if(WIN32)
            set(Boost_USE_STATIC_LIBS ON)
            set(Boost_USE_STATIC ON)
            set(Boost_USE_MULTITHREAD )
        endif(WIN32)
        if(${CMAKE_VERSION} VERSION_LESS 2.8.5)
            set(Boost_ADDITIONAL_VERSIONS
                "1.47.0" "1.47" "1.46.1"
                "1.46.0" "1.46" "1.45.0" "1.45" "1.44.0" "1.44" "1.43.0" "1.43")
        else(${CMAKE_VERSION} VERSION_LESS 2.8.5)
            set(Boost_ADDITIONAL_VERSIONS
                "1.68.0" "1.68"
                "1.68.0" "1.68" "1.67.0" "1.67" "1.66.0" "1.66" "1.65.1" "1.65.0" "1.65"
                "1.64.0" "1.64" "1.63.0" "1.63" "1.62.0" "1.62" "1.61.0" "1.61" "1.60.0" "1.60"
                "1.59.0" "1.59" "1.58.0" "1.58" "1.57.0" "1.57" "1.56.0" "1.56" "1.55.0" "1.55"
                "1.54.0" "1.54" "1.53.0" "1.53" "1.52.0" "1.52" "1.51.0" "1.51"
                "1.50.0" "1.50" "1.49.0" "1.49" "1.48.0" "1.48" "1.47.0" "1.47")
        endif(${CMAKE_VERSION} VERSION_LESS 2.8.5)
        # Disable the config mode of find_package(Boost) iostreams serialization
        set(Boost_NO_BOOST_CMAKE ON)
        find_package(Boost 1.61  ${QUIET_} COMPONENTS system filesystem thread date_time chrono   )

        set(BOOST_FOUND ${Boost_FOUND})
        set(BOOST_INCLUDE_DIRS "${Boost_INCLUDE_DIR}")
        set(BOOST_LIBRARY_DIRS "${Boost_LIBRARY_DIRS}")
        set(BOOST_LIBRARIES ${Boost_LIBRARIES})
        INCLUDE_DIRECTORIES( "BOOST_LIBRARIES"  )
        INCLUDE_DIRECTORIES( ${BOOST_LIBRARIES}  )
        if(WIN32 AND NOT MINGW)
            set(BOOST_DEFINITIONS ${BOOST_DEFINITIONS} -DBOOST_ALL_NO_LIB)
        endif(WIN32 AND NOT MINGW)
    endmacro(find_boost)
    find_boost()
    INCLUDE_DIRECTORIES( "find_boost() finish."  )

    ##boost
    set(BOOST_DIR "${Lib_dep_root}")
    INCLUDE_DIRECTORIES( "${Lib_dep_root}/include/boost-1_61"  )
    LINK_DIRECTORIES("${Lib_dep_root}/lib")

    INCLUDE_DIRECTORIES( "BOOST_INCLUDEDIR:"  )
    INCLUDE_DIRECTORIES( ${BOOST_INCLUDEDIR}  )
    INCLUDE_DIRECTORIES( "BOOST_LIBRARIES:"  )
    INCLUDE_DIRECTORIES( ${BOOST_LIBRARIES}  )
    LINK_DIRECTORIES( "D:/caffeBundle/libraries_v140_x64_py35_1.1.0/libraries/lib")



    # //cv
    if(0)
        set(OpenCV_DIR "${Lib_dep_root}/cv3411CL8Vs15NoSfm/x64/lib")
        find_package( OpenCV EXACT 3.4.11 REQUIRED PATHS "${Lib_dep_root}/cv3411CL8Vs15NoSfm" )
    endif()
    if( 0 )
        set(OpenCV_DIR "${Lib_dep_root}/cv3411Cuda8Vs15NoCudaimgproc/x64/lib")
        find_package( OpenCV EXACT 3.4.11 REQUIRED PATHS "${Lib_dep_root}/cv3411Cuda8Vs15NoCudaimgproc" )
    endif()
    if(0)
        set(OpenCV_DIR "${Lib_dep_root}/cv310Vs15Cuda8installAllFull/x64/lib")
        find_package( OpenCV EXACT 3.1.0 REQUIRED PATHS "${Lib_dep_root}/cv310Vs15Cuda8installAllFull" )
    endif()
    if(1)
        set(OpenCV_DIR "${Lib_dep_root}/x64/lib")
        find_package( OpenCV EXACT 3.1.0 REQUIRED PATHS "${Lib_dep_root}" )
    endif()
    #cv440Official
    if(0)
        set(OpenCV_DIR "${Lib_dep_root}/cv440Official/x64/lib")
        find_package( OpenCV EXACT 4.4.0 REQUIRED PATHS "${Lib_dep_root}/cv440Official" )
    endif()
    message( "________________________________________")
    message( "OpenCV_VERSION: ")
    message( ${OpenCV_VERSION})
    message( "OpenCV_INCLUDE_DIRS: ")
    message( ${OpenCV_INCLUDE_DIRS})
    message( "OpenCV_COMPUTE_CAPABILITIES: ")
    #message( ${OpenCV_COMPUTE_CAPABILITIES})
    #message( ${OpenCV_CUDA})
    message( "OpenCV_ARCH: ")
    message( ${OpenCV_ARCH})
    message( "OpenCV_LIBS: ")
    message( ${OpenCV_LIBS})
    foreach( var ${OpenCV_LIBS})
        message( ${var})
    endforeach()



    set(DepAppendPath   "D:/caffeBundle/depAppend/jsoncpp-master/x64")
    #------------------- jsoncpp ----------------------------------
    message( "(5.01)(lk)________ jsoncpp _______:  ")
    set( jsoncpp_INCLUDE_DIRS    "${DepAppendPath}/include" )
    link_directories(   "${DepAppendPath}/lib"  )
    include_directories(${jsoncpp_INCLUDE_DIRS})
    message( "jsoncpp info(lk):  ")
    message( ${jsoncpp_INCLUDE_DIRS})
    message( "jsoncpp find finish.\n\n")

    #-------------------------------------------- gflags ----------------------------------------------------
    link_directories("${PROJECT_BINARY_DIR}/lib")
    set(gflags_DIR  "${Lib_dep_root}/cmake" )
    find_package(gflags  REQUIRED  PATHS  "${Lib_dep_root}" )
    include_directories(${gflags_include_directory})
    link_directories(${gflags_library_directory} )
    include_directories(SYSTEM ${gflags_INCLUDE_DIRS})
    list(APPEND LINKER_LIBS ${gflags_LIBRARIES})
    include_directories(  "${Lib_dep_root}/include"  )
    link_directories(  "${Lib_dep_root}/lib"  )
    set(gflags_LIB gflagsd)



    #-------------------------------------------- glog ----------------------------------------------------
    link_directories("${PROJECT_BINARY_DIR}/lib")
    set(glog_DIR  "${Lib_dep_root}/lib/cmake/glog" )
    find_package(glog  REQUIRED  PATHS  "${Lib_dep_root}" )
    include_directories(${glog_include_directory})
    link_directories(${glog_library_directory} )
    include_directories(SYSTEM ${GLOG_INCLUDE_DIRS})
    list(APPEND LINKER_LIBS ${GLOG_LIBRARIES})
    include_directories(  "${Lib_dep_root}/include"  )
    link_directories(  "${Lib_dep_root}/lib"  )
    set(GLOG_LIB glogd)
endif()

######################################生成exe####################################################### # ${RESOURCES}
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
add_executable(${PROJECT_NAME}    MACOSX_BUNDLE ${SOURCES}   ${vision_SOURCE}   ${FORMS}   ${RESOURCES}  )
target_link_libraries(   ${PROJECT_NAME}  PUBLIC     Qt5::Core Qt5::Widgets Qt5::Gui Qt5::Qml Qt5::Quick
    ${BOOST_LIBRARIES} ${OpenCV_LIBRARIES} ${GLOG_LIB} ${gflags_LIB}
#    jsoncppd
    )
message( "(6)(lk)__________ project ___________:  "  )
message(  "project info(lk):  "  )
message(   ${PROJECT_NAME}  )
#message(   ${HEADERS}  )
message(   ${SOURCES}   )
message(   ${vision_SOURCE}  )
#message(   ${FORMS}   )
include_directories(${PROJECT_BINARY_DIR} ${OpenCL_INCLUDE_DIRS} )
set(CMAKE_INSTALL_PREFIX  "${PROJECT_BINARY_DIR}/install")
## Binaries
install (TARGETS ${PROJECT_NAME} DESTINATION bin)

#######################################################
#Add some multithreaded build support
MARK_AS_ADVANCED(MULTITHREADED_BUILD)
set(MULTITHREADED_BUILD 8 CACHE STRING "How many threads are used to build the project")
if(MULTITHREADED_BUILD)
    if(${CMAKE_GENERATOR} MATCHES "Unix Makefiles")
        message(STATUS ${CMAKE_BUILD_TOOL})
        set(CMAKE_MAKE_PROGRAM  "${CMAKE_MAKE_PROGRAM} -j${MULTITHREADED_BUILD}")
        message(STATUS "Added arguments to CMAKE_BUILD_TOOL: ${CMAKE_MAKE_PROGRAM}")
    elseif(MSVC)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}/MP")
        message(STATUS "Added parallel build arguments to CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")
    endif()
endif()
#######################################################


