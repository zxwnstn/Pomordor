cmake_minimum_required(VERSION 3.16.2 FATAL_ERROR)

project(PomordorUI LANGUAGES CXX)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

ADD_DEFINITIONS(-DUNICODE)
ADD_DEFINITIONS(-D_UNICODE)

if(MSVC)
# Instruct CMake where to find cmake related stuffs
set (CMAKE_PREFIX_PATH "..\\vendor\\Qt5.14.2\\msvc2017")

# Set QTDIR to Qt5 SDK path
set (QTDIR "../Qt5.14.2/msvc2017/")
set (RUN_ENV "PATH=${QTDIR}/bin")
MESSAGE ("-- RUN_ENV: ${RUN_ENV}")
endif(MSVC)


find_package(Qt5 REQUIRED COMPONENTS Core Widgets Gui)

set(project_ui
	src/TimerPage/TimerPage.ui
	src/TimerPage/ScheduleDialog.ui
)

set(project_headers
	src/pompch.h
	src/TimerPage/Timer.h
	src/TimerPage/TimerPage.h
	src/TimerPage/ScheduleDialog.h
	src/TimerPage/TrayIcon.h
	src/TimerPage/Titlebar.h
)

set(project_sources 
    src/main.cpp
	src/pompch.cpp
	src/TimerPage/Timer.cpp
	src/TimerPage/TimerPage.cpp
	src/TimerPage/ScheduleDialog.cpp
	src/TimerPage/TrayIcon.cpp
	src/TimerPage/Titlebar.cpp
)

qt5_wrap_ui(project_headers_wrapped ${project_ui})
qt5_wrap_cpp(project_sources_moc ${project_headers} )

add_executable(
	${PROJECT_NAME} 
	${project_headers} 
	${project_sources} 
	${project_sources_moc} 
	${project_headers_wrapped} 
	assets/files.qrc
)

target_precompile_headers(
	${PROJECT_NAME} 
	PUBLIC 
	"${CMAKE_CURRENT_SOURCE_DIR}/src/pompch.h"
)

target_include_directories(
	${PROJECT_NAME} 
	PUBLIC 
	${CMAKE_SOURCE_DIR}/PomordorEngine/src
	${CMAKE_CURRENT_SOURCE_DIR}/src
)

target_link_libraries(${PROJECT_NAME}
    PUBLIC
    Qt5::Core
    Qt5::Gui
    Qt5::Widgets
    PomordorEngine
)