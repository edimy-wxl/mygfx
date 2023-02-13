#THIS FILE IS AUTO GENERATED FROM THE TEMPLATE! DO NOT CHANGE!
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_VERSION 1)
cmake_minimum_required(VERSION 3.17)

# specify cross compilers and tools
set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)
set(CMAKE_ASM_COMPILER  arm-none-eabi-gcc)
set(CMAKE_AR arm-none-eabi-ar)
set(CMAKE_OBJCOPY arm-none-eabi-objcopy)
set(CMAKE_OBJDUMP arm-none-eabi-objdump)
set(SIZE arm-none-eabi-size)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)
set(CMAKE_SYSTEM_PROCESSOR arm)
set(CMAKE_EXE_LINKER_FLAGS "--specs=nosys.specs" CACHE INTERNAL "")

# project settings
project(mygfx C CXX ASM)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_C_STANDARD 11)

add_compile_options(-mcpu=cortex-m0plus -g3)
add_compile_options(-Os -ffunction-sections -fdata-sections -Wall -fstack-usage -mfloat-abi=soft -mthumb)
if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -fno-rtti -fno-use-cxa-atexit -femit-class-debug-always")
endif(CMAKE_COMPILER_IS_GNUCXX)


include_directories(
        Core/Inc
        Drivers/STM32G0xx_HAL_Driver/Inc
        Drivers/STM32G0xx_HAL_Driver/Inc/Legacy
        Drivers/CMSIS/Device/ST/STM32G0xx/Include
        Drivers/CMSIS/Include
        Middlewares/ST/touchgfx/framework/include
        TouchGFX/App
        TouchGFX/target/generated
        TouchGFX/target
        TouchGFX/generated/fonts/include
        TouchGFX/generated/gui_generated/include
        TouchGFX/generated/images/include
        TouchGFX/generated/texts/include
        TouchGFX/generated/videos/include
        TouchGFX/gui/include
)

add_definitions(-DDEBUG -DUSE_HAL_DRIVER -DSTM32G071xx)

file(GLOB_RECURSE SOURCES
        "Drivers/STM32G0xx_HAL_Driver/Src/*.*"
        "Core/Src/*.*"
        "Core/Startup/*.*"
        "TouchGFX/gui/src/*.*"
        "TouchGFX/generated/fonts/src/*.*"
        "TouchGFX/generated/images/src/*.*"
        "TouchGFX/generated/texts/src/*.*"
        "TouchGFX/generated/gui_generated/src/*.*"
        "TouchGFX/target/*.cpp"
        "TouchGFX/target/generated/*.cpp"
        "TouchGFX/App/*.*"
        )


set(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/STM32G071RBTX_FLASH.ld)
set(ST_TOUCHGFX ${CMAKE_SOURCE_DIR}/Middlewares/ST/touchgfx/lib/core/cortex_m0/gcc)

add_link_options(-Wl,-gc-sections,--print-memory-usage -Wl,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map)
add_link_options(-mcpu=cortex-m0plus -mthumb -mthumb-interwork -mfloat-abi=soft )
add_link_options(-T ${LINKER_SCRIPT})


add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT})
target_link_libraries(${PROJECT_NAME}.elf ${CMAKE_SOURCE_DIR}/Middlewares/ST/touchgfx/lib/core/cortex_m0/gcc/libtouchgfx.a)


set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)

add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMENT "Building ${HEX_FILE}
Building ${BIN_FILE}")
