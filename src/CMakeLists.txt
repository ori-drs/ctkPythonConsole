

find_package(Qt5 REQUIRED COMPONENTS Core Gui Widgets)

find_package(PythonLibs REQUIRED)
find_package(catkin  REQUIRED COMPONENTS python_qt)


include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${PYTHON_INCLUDE_DIRS})
include_directories(${Qt5Core_INCLUDE_DIRS})
include_directories(${Qt5Widgets_INCLUDE_DIRS})
include_directories(${catkin_INCLUDE_DIRS})

set(srcs
  ctkConsole.cpp
  ctkPythonConsole.cpp
  ctkAbstractPythonManager.cpp
)

qt5_wrap_cpp(moc_srcs
  ctkConsole.h
  ctkConsole_p.h
  ctkPythonConsole.h
  ctkAbstractPythonManager.h
  )

add_library(${PROJECT_NAME} ${srcs} ${moc_srcs})
target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Widgets ${PYTHON_LIBRARIES} ${catkin_LIBRARIES})
set_target_properties(${PROJECT_NAME} PROPERTIES SOVERSION 1.0 VERSION 1.0)

add_executable(console main.cpp)
target_link_libraries(console ${PROJECT_NAME})

install(FILES
  ctkAbstractPythonManager.h
  ctkConsole.h
  ctkPythonConsole.h
  ctkPythonConsoleExport.h
  DESTINATION include/ctkPythonConsole)

install(TARGETS ${PROJECT_NAME}
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
