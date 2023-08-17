cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

# ===----------------------------------------------------------------------=== #

option(BUILD_AU "Build Audio Unit artifacts" ON)
option(BUILD_VST "Build VST3 artifacts" ON)
option(BUILD_STANDALONE "Build standalone app artifacts" OFF)

set(PLUGIN_NAME "JUCEMake")
set(PLUGIN_VERSION "0.1.0")
set(PLUGIN_AUTHOR "JUCE")
set(PLUGIN_BUNDLE_ID "com.juce.JUCEMake")
set(PLUGIN_AUTHOR_CODE "JUCE")
set(PLUGIN_CODE "JCMK")

if(BUILD_AU)
  list(APPEND PLUGIN_FORMATS "AU")
endif()
if(BUILD_VST)
  list(APPEND PLUGIN_FORMATS "VST3")
endif()
if(BUILD_STANDALONE)
  list(APPEND PLUGIN_FORMATS "Standalone")
endif()

# ===----------------------------------------------------------------------=== #

project(${PLUGIN_NAME} VERSION ${PLUGIN_VERSION})

find_package(JUCE CONFIG REQUIRED)

juce_add_plugin(${PLUGIN_NAME}
  COMPANY_NAME ${PLUGIN_AUTHOR}
  BUNDLE_ID ${PLUGIN_BUNDLE_ID}

  PLUGIN_MANUFACTURER_CODE ${PLUGIN_AUTHOR_CODE}
  PLUGIN_CODE ${PLUGIN_CODE}

  FORMATS ${PLUGIN_FORMATS}
  COPY_PLUGIN_AFTER_BUILD TRUE)

file(GLOB PLUGIN_SOURCE src/*.h src/*.cpp)
target_sources(${PLUGIN_NAME} PRIVATE ${PLUGIN_SOURCE})
target_compile_features(${PLUGIN_NAME} PRIVATE cxx_std_17)
target_compile_definitions(${PLUGIN_NAME}
  PUBLIC 
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0 
    JUCE_VST3_CAN_REPLACE_VST2=0)
target_link_libraries(${PLUGIN_NAME}
  PRIVATE 
    juce::juce_audio_utils 
    juce::juce_audio_processors
  PUBLIC
    juce::juce_recommended_config_flags
    juce::juce_recommended_lto_flags
    juce::juce_recommended_warning_flags)
