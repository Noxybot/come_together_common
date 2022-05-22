set(PROTO_FILES
    come_together.proto
)
file( GLOB component_SOURCES ${CMAKE_CURRENT_BINARY_DIR}/*.cc )
# Add Library target with protobuf sources
add_library(protocol ${PROTO_FILES} ${component_SOURCES})
target_link_libraries(protocol
    PUBLIC
        protobuf::libprotobuf
        gRPC::grpc
        gRPC::grpc++
)
get_target_property(grpc_cpp_plugin_location gRPC::grpc_cpp_plugin LOCATION)
target_include_directories(protocol PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
set_target_properties(protocol PROPERTIES LINKER_LANGUAGE CXX)

protobuf_generate(TARGET protocol LANGUAGE cpp)
protobuf_generate(TARGET protocol LANGUAGE grpc GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc PLUGIN "protoc-gen-grpc=${grpc_cpp_plugin_location}")
