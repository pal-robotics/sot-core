# Copyright 2010, François Bleibel, Olivier Stasse, JRL, CNRS/AIST
#
# This file is part of sot-core.
# sot-core is free software: you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public License
# as published by the Free Software Foundation, either version 3 of
# the License, or (at your option) any later version.
#
# sot-core is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Lesser Public License for more details.  You should have
# received a copy of the GNU Lesser General Public License along with
# sot-core. If not, see <http://www.gnu.org/licenses/>.

CMAKE_MINIMUM_REQUIRED(VERSION 2.8.3)

find_package(catkin REQUIRED COMPONENTS sot_cmake dynamic-graph-python jrl-mal)

INCLUDE(${SOT_CMAKE_DIR}/base.cmake)
INCLUDE(${SOT_CMAKE_DIR}/boost.cmake)
INCLUDE(${SOT_CMAKE_DIR}/lapack.cmake)
INCLUDE(${SOT_CMAKE_DIR}/cpack.cmake)

SET(PROJECT_NAME sot-core)
SET(PROJECT_DESCRIPTION "Hierarchical task solver plug-in for dynamic-graph.")
SET(PROJECT_URL "http://github.com/jrl-umi3218/sot-core")

include_directories(${catkin_INCLUDE_DIRS})

#This project will create many plugins as shared libraries, listed here
SET(plugins
  sot/sot-qr
  sot/weighted-sot
  sot/sot-h
  sot/sot

  math/op-point-modifier

  matrix/operator
  matrix/derivator
  matrix/fir-filter
  matrix/integrator-abstract
  matrix/integrator-euler
  matrix/matrix-constant
  matrix/vector-constant
  matrix/vector-to-rotation

  task/gain-adaptive
  task/task-pd
  task/constraint
  task/gain-hyperbolic
  task/task
  task/task-conti
  task/task-unilateral

  feature/feature-point6d
  feature/feature-vector3
  feature/feature-generic
  feature/feature-joint-limits
  feature/feature-1d
  feature/feature-point6d-relative
  feature/feature-visual-point
  feature/feature-task
  feature/feature-line-distance
  feature/feature-posture
  feature/visual-point-projecter

  traces/reader

  tools/time-stamp
  tools/timer
  tools/seq-play
  tools/sequencer
  tools/robot-simu
  tools/motion-period
  tools/neck-limitation
  tools/mailbox-vector
  tools/kalman
  tools/joint-limitator
  tools/gripper-control
  tools/com-freezer
  tools/clamp-workspace
  tools/binary-int-to-uint
  tools/periodic-call-entity
  tools/joint-trajectory-entity

  control/control-gr
  control/control-pd
  )

foreach(plugin ${plugins})
  get_filename_component(l ${plugin} NAME)
  list(APPEND export_libs ${l})
endforeach()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
 # LIBRARIES ${export_libs}
  CATKIN_DEPENDS dynamic-graph
)

SET(CUSTOM_HEADER_DIR "sot/core")

# Disable -Werror on Unix for now.
SET(CXX_DISABLE_WERROR True)

SET(PKG_CONFIG_ADDITIONAL_VARIABLES
  ${PKG_CONFIG_ADDITIONAL_VARIABLES}
  plugindirname
  plugindir
  )

SETUP_PROJECT()

PKG_CONFIG_APPEND_LIBS("sot-core")

# Search for dependencies.
# Boost
SET(BOOST_COMPONENTS thread filesystem program_options unit_test_framework system regex )
SEARCH_FOR_BOOST()

ADD_SUBDIRECTORY(include)
ADD_SUBDIRECTORY(src)
#ADD_SUBDIRECTORY(unitTesting)
#ADD_SUBDIRECTORY(doc)

SETUP_PROJECT_FINALIZE()
SETUP_PROJECT_CPACK()
