cmake_minimum_required(VERSION 3.10)
project(Voxcad)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(VOXELYZE_DIR ../MCC_Voxelyze)
set(QTUTILS_DIR ./QTUtils)

include_directories(${VOXELYZE_DIR})

# Locate the Voxelyze static library
find_library(LIB_VOXELYZE
        NAMES Voxelyze
        PATHS ${VOXELYZE_DIR}/cmake-build-debug)

# Set OpenGL preprocessor flag
add_definitions(-DUSE_OPEN_GL)

set(SOURCE
        ./main.cpp
        ./VoxCad.cpp
        ./QVX_Edit.cpp
        ./QVX_Interface.cpp
        ./Dlg_3DBrush.cpp
        ./Dlg_Palette.cpp
        ./Dlg_Workspace.cpp
        ./Dlg_VoxInfo.cpp
        ./Dlg_BCs.cpp
        ./Dlg_FEAInfo.cpp
        ./Dlg_Physics.cpp
        ./Dlg_Tensile.cpp
        ./Dlg_VideoCapture.cpp
        ./Dlg_EditPrim.cpp
        ./Dlg_StructureEditor.cpp
        ./QVX_TensileTest.cpp

        ${VOXELYZE_DIR}/VX_Object.cpp
        ${VOXELYZE_DIR}/VX_FEA.cpp
        ${VOXELYZE_DIR}/VX_Environment.cpp
        ${VOXELYZE_DIR}/VX_FRegion.cpp
        ${VOXELYZE_DIR}/VXS_SimGLView.cpp
        ${VOXELYZE_DIR}/Utils/Mesh.cpp
        ${VOXELYZE_DIR}/Utils/GL_Utils.cpp
        ${VOXELYZE_DIR}/VX_Sim.cpp
        ${VOXELYZE_DIR}/VX_Bond.cpp
        ${VOXELYZE_DIR}/VX_Voxel.cpp
        ${VOXELYZE_DIR}/VX_MeshUtil.cpp
        ${VOXELYZE_DIR}/VXS_Bond.cpp
        ${VOXELYZE_DIR}/VXS_BondCollision.cpp
        ${VOXELYZE_DIR}/VXS_BondInternal.cpp
        ${VOXELYZE_DIR}/VXS_Voxel.cpp
        ${VOXELYZE_DIR}/Utils/XML_Rip.cpp
        ${VOXELYZE_DIR}/Utils/Array3D.cpp
        ${VOXELYZE_DIR}/Utils/MarchCube.cpp
        ${VOXELYZE_DIR}/Utils/tinyxml.cpp
        ${VOXELYZE_DIR}/Utils/tinyxmlparser.cpp
        ${VOXELYZE_DIR}/Utils/tinyxmlerror.cpp

        ${QTUTILS_DIR}/QOpenGL.cpp
        ${QTUTILS_DIR}/QThreadWrap.cpp
        ${QTUTILS_DIR}/QSimplePlot.cpp)

set(INCLUDES
        ./QVX_Interfaces.h)

set (UIS
        ./VoxCad.ui
        ./vPalette.ui
        ./vStructureEditor.ui
        ./vWorkSpace.ui
        ./vVoxInfo.ui
        ./vBCs.ui
        ./vPrimEdit.ui
        ./vFEAInfo.ui
        ./vPhysics.ui
        ./vTensile.ui
        ./vVideoGen.ui)

SET (RESOURCES
        ./VoxCad.qrc)

# Add requirements for building Qt projects
find_package(Qt4 COMPONENTS QtCore QtGui QtOpenGL REQUIRED)
include(${QT_USE_FILE})
add_definitions(${QT_DEFINITIONS})
include_directories(${CMAKE_BINARY_DIR})

# Directives for Qt-metacompiler
QT4_ADD_RESOURCES(RES_SOURCES ${RESOURCES})
QT4_WRAP_UI(UI_HEADERS ${UIS})
QT4_WRAP_CPP(MOC_SRCS ${INCLUDES})

# Reference OpenGL library
find_package(OpenGL REQUIRED)

add_executable(Voxcad
        ${SOURCE}
        ${MOC_SRCS}
        ${RES_SOURCES}
        ${UI_HEADERS})

target_link_libraries(Voxcad
        ${LIB_VOXELYZE}
        ${QT_LIBRARIES}
        ${OPENGL_LIBRARIES})