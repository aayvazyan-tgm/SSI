cmake_minimum_required(VERSION 2.8.4)
project(SSI)

set(TIVAWARE_PATH "/opt/tivaware")

set(MCU "/root/ClionProjects/SSI/TM4C123GH6PM")
set(LD_SCRIPT "${MCU}.ld")

set(CFLAGS " -g -mthumb -mcpu=cortex-m4 -mfpu=fpv4-sp-d16 -mfloat-abi=softfp -Os -ffunction-sections -fdata-sections -MD -std=c99 -Wall -pedantic -DPART_$(MCU) -c -I${TIVAWARE_PATH} -DTARGET_IS_BLIZZARD_RA1")
set(LDFLAGS " -T ${LD_SCRIPT} --entry ResetISR -nostartfiles")
#set(LDFLAGS "${LDFLAGS} --gc-sections") #does not work!!?

# end of user configuration
set(CMAKE_SHARED_LIBRARY_LINK_C_FLAGS "")
set(CMAKE_SHARED_LIBRARY_LINK_CXX_FLAGS "")

set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_PROCESSOR TM4C123G)

include(CMakeForceCompiler)

# Compiler & Linker Settings
CMAKE_FORCE_C_COMPILER(arm-none-eabi-gcc none)
CMAKE_FORCE_CXX_COMPILER(arm-none-eabi-g++ none)

set(CMAKE_CXX_FLAGS "${CFLAGS}")
set(CMAKE_CXX_LINK_FLAGS "${LDFLAGS}")
# Use GCC for linking executables to avoid linking to stdlibc++ _BUT_ get all the math libraries etc.
set(CMAKE_C_LINK_EXECUTABLE "arm-none-eabi-ld")
set(CMAKE_CXX_LINK_EXECUTABLE "arm-none-eabi-ld")

set(CMAKE_C_LINK_EXECUTABLE "<CMAKE_C_COMPILER> <FLAGS> <CMAKE_CXX_LINK_FLAGS> <LINK_FLAGS> <OBJECTS> -o <TARGET> ${CMAKE_GNULD_IMAGE_VERSION} <LINK_LIBRARIES>")
set(CMAKE_CXX_LINK_EXECUTABLE "<CMAKE_C_COMPILER> <FLAGS> <CMAKE_CXX_LINK_FLAGS> <LINK_FLAGS> <OBJECTS> -o <TARGET> ${CMAKE_GNULD_IMAGE_VERSION} <LINK_LIBRARIES>")
set(CMAKE_EXE_LINKER_FLAGS "${LDFLAGS}")
set(CMAKE_OBJCOPY arm-none-eabi-objcopy)


INCLUDE_DIRECTORIES(${TIVAWARE_PATH})


add_executable(main src/main.c)
