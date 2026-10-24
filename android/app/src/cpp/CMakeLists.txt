set(SRCS
  android_controller_interface.cpp
  android_controller_interface.h
  android_host_interface.cpp
  android_host_interface.h
  android_http_downloader.cpp
  android_http_downloader.h
  android_progress_callback.cpp
  android_progress_callback.h
  android_settings_interface.cpp
  android_settings_interface.h
)

add_library(polygonia-native SHARED ${SRCS})
target_link_libraries(polygonia-native PRIVATE android frontend-common core common glad imgui)

find_package(OpenSLES)
if(OPENSLES_FOUND)
  message("Enabling OpenSL ES audio stream")
  target_sources(polygonia-native PRIVATE
    opensles_audio_stream.cpp
    opensles_audio_stream.h)
  target_link_libraries(polygonia-native PRIVATE OpenSLES::OpenSLES)
  target_compile_definitions(polygonia-native PRIVATE "-DUSE_OPENSLES=1")
endif()
