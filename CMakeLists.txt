# Copyright: (C) 2014 iCub Facility - Istituto Italiano di Tecnologia
# Authors: Ugo Pattacini
# CopyPolicy: Released under the terms of the GNU GPL v2.0.

cmake_minimum_required(VERSION 3.5)
project(speech)

find_package(YARP REQUIRED)
find_package(ICUBcontrib REQUIRED)
list(APPEND CMAKE_MODULE_PATH ${ICUBCONTRIB_MODULE_PATH}
                              ${CMAKE_CURRENT_SOURCE_DIR})

include(ICUBcontribHelpers)
include(ICUBcontribOptions)
icubcontrib_set_default_prefix()

add_subdirectory(iSpeak)
add_subdirectory(speech-interpreters)

option(BUILD_SPEECHSDK_MODULES "enable the build of Speech SDK related modules on Windows" OFF)
if(WIN32)
  find_package(SpeechSDK)
  if(SpeechSDK_FOUND AND BUILD_SPEECHSDK_MODULES)
    add_subdirectory(speechRecognizer)
    add_subdirectory(windows-tts)
  endif()
endif()

option(BUILD_ACAPELA_TTS "enable the build of acapela wrapper" OFF)
if(BUILD_ACAPELA_TTS)
  find_package(AcapelaSDK REQUIRED)
  add_subdirectory(acapela-tts)
endif()

option(BUILD_SVOX_SPEECH "enable the build of svox-speech" OFF)
if(BUILD_SVOX_SPEECH)
  add_subdirectory(svox-speech)
endif()

option(BUILD_GOOGLE_SPEECH "enable the build of google-speech" OFF)
if(BUILD_GOOGLE_SPEECH)
  add_subdirectory(speechInteraction)
endif()

icubcontrib_add_uninstall_target()
