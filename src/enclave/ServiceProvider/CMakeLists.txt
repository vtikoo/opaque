cmake_minimum_required(VERSION 2.8)

project(OpaqueKeyGen)

find_package(OpenSSL REQUIRED)

set(SOURCES
  KeyGen.cpp
  ServiceProvider.cpp
  ServiceProviderJNI.cpp
  agent_wget.cpp
  base64.c
  crypto.cpp
  ecp.cpp
  iasrequest.cpp
  sp_crypto.cpp
  tls_client.cpp
	verify_callback.cpp
  verify_signer_openssl.cpp)
  
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC -Wno-attributes")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} ${CMAKE_CXX_FLAGS}")

add_library(ra_jni SHARED ${SOURCES})

find_library(CRYPTO_LIB crypto)
find_library(SSL_LIB ssl)
target_link_libraries(ra_jni "${CRYPTO_LIB}" "${SSL_LIB}")

target_link_libraries(ra_jni openenclave::oehostapp OpenSSL::SSL)

add_executable(keygen ${SOURCES})
target_link_libraries(keygen "${CRYPTO_LIB}" "${SSL_LIB}")

install(TARGETS ra_jni DESTINATION lib)
