include(ExternalProject)
include(VariableUtils)
include(Validate)

validate(
  HAS_VALUE
    BOOTSTRAP_BIN_DIR
  EXISTS
    CACHE_DIR
    ROOT_DIR
    INSTALL_DIR
    LLVM_SOURCE_DIR)

get_filename_component(srcDir "${LLVM_SOURCE_DIR}/../compiler-rt" ABSOLUTE)
if (NOT IS_DIRECTORY "${srcDir}")
  message(FATAL_ERROR "Compiler-RT source directory not found: ${srcDir}")
endif()

ExternalProject_Add(compiler-rt
  SOURCE_DIR ${srcDir}
  CMAKE_GENERATOR Ninja
  CMAKE_ARGS
    -DROOT_DIR=${ROOT_DIR}
    -DINSTALL_DIR=${INSTALL_DIR}
    -DLLVM_SOURCE_DIR=${LLVM_SOURCE_DIR}
    -DBOOTSTRAP_BIN_DIR=${BOOTSTRAP_BIN_DIR}
    -C${CACHE_DIR}/compiler-rt-cache.cmake
  DOWNLOAD_COMMAND ""
  TEST_COMMAND ""
  DEPENDS llvm-bootstrap)
