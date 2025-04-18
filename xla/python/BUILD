load("//xla:py_strict.bzl", "py_strict_test")
load("//xla:pytype.bzl", "pytype_strict_library")
load(
    "//xla:xla.default.bzl",
    "xla_cc_test",
    "xla_py_test_deps",
)
load(
    "//xla/tsl:tsl.bzl",
    "if_google",
    "internal_visibility",
)
load("//xla/tsl:tsl.default.bzl", "get_compatible_with_portable", "tsl_pybind_extension")
load("//xla/tsl/platform:build_config.bzl", "tf_proto_library")
load("//xla/tsl/platform:rules_cc.bzl", "cc_library")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = internal_visibility([":jax"]),
    licenses = ["notice"],
)

package_group(
    name = "friends",
    includes = [
        "//xla:friends",
        "//xla:internal",
    ],
)

package_group(
    name = "jax",
    packages = [
        "//third_party/py/jax/...",
    ],
)

pytype_strict_library(
    name = "xla_client",
    srcs = ["xla_client.py"],
    visibility = ["//visibility:public"],
    deps = if_google(["//third_party/py/jax/jaxlib/xla:xla_client"]),
)

pytype_strict_library(
    name = "xla_extension",
    srcs = ["xla_extension.py"],
    visibility = ["//visibility:public"],
    deps = if_google(["//third_party/py/jax/jaxlib/xla:xla_extension"]),
)

tsl_pybind_extension(
    name = "status_casters_ext",
    testonly = 1,
    srcs = ["status_casters_ext.cc"],
    deps = [
        "//xla/pjrt:exceptions",
        "//xla/pjrt:status_casters",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@nanobind",
    ],
)

py_strict_test(
    name = "status_casters_test",
    srcs = ["status_casters_test.py"],
    main = "status_casters_test.py",
    tags = ["no_oss"],
    deps = [
        ":status_casters_ext",
        "@absl_py//absl/testing:absltest",
    ] + xla_py_test_deps(),
)

cc_library(
    name = "types",
    srcs = ["types.cc"],
    hdrs = ["types.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        ":nb_numpy",
        "//xla:literal",
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/pjrt:exceptions",
        "//xla/python/ifrt",
        "//xla/python/pjrt_ifrt:pjrt_dtype",
        "//xla/tsl/platform:logging",
        "//xla/tsl/platform:statusor",
        "//xla/tsl/python/lib/core:numpy",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "@nanobind",
    ],
)

cc_library(
    name = "python_ref_manager",
    srcs = ["python_ref_manager.cc"],
    hdrs = ["python_ref_manager.h"],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:span",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "@nanobind",
    ],
)

cc_library(
    name = "traceback",
    srcs = ["traceback.cc"],
    hdrs = ["traceback.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        ":nb_class_ptr",
        # placeholder for index annotation deps
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/hash",
        "@com_google_absl//absl/log:check",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@nanobind",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "//xla/pjrt:exceptions",
        "//xla/tsl/platform:logging",
        "@tsl//tsl/platform",
    ],
)

cc_library(
    name = "pprof_profile_builder",
    srcs = ["pprof_profile_builder.cc"],
    hdrs = ["pprof_profile_builder.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        "//xla:util",
        "//xla/tsl/platform:logging",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status:statusor",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "@nanobind",
        "@tsl//tsl/platform:protobuf",
        "@tsl//tsl/profiler/protobuf:profile_proto_cc",
    ],
)

cc_library(
    name = "py_client",
    srcs = [
        "py_array.cc",
        "py_client.cc",
        "py_compile_only_client.cc",
        "py_device.cc",
        "py_device_list.cc",
        "py_executable.cc",
        "py_memory_space.cc",
        "py_program.cc",
        "py_values.cc",
        "sharding.cc",
        "to_ifrt_sharding.cc",
    ],
    hdrs = [
        "py_array.h",
        "py_client.h",
        "py_compile_only_client.h",
        "py_device.h",
        "py_device_list.h",
        "py_executable.h",
        "py_memory_space.h",
        "py_program.h",
        "py_values.h",
        "sharded_device_array.h",
        "sharding.h",
        "to_ifrt_sharding.h",
    ],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        ":aggregate_profile",
        ":callback",
        ":guard_lib",
        ":nb_absl_span",
        ":nb_class_ptr",
        ":nb_helpers",
        ":nb_numpy",
        ":pprof_profile_builder",
        ":py_client_cpu",
        ":py_host_callback",
        ":py_host_callback_proto_cc",
        ":python_ref_manager",
        ":traceback",
        ":types",
        ":util",
        ":xplane_to_profile_instructions",
        # placeholder for index annotation deps
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/cleanup",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/functional:any_invocable",
        "@com_google_absl//absl/hash",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/log:check",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:cord",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
        "@com_google_absl//absl/types:variant",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@nanobind",
        "@shardy//shardy/dialect/sdy/ir:dialect",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "//xla:comparison_util",
        "//xla:literal",
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:types",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/builder:xla_builder",
        "//xla/hlo/builder:xla_computation",
        "//xla/hlo/builder/lib:arithmetic",
        "//xla/hlo/ir:hlo",
        "//xla/pjrt:exceptions",
        "//xla/pjrt:host_callback",
        "//xla/pjrt:host_memory_spaces",
        "//xla/pjrt:lru_cache",
        "//xla/pjrt:mlir_to_hlo",
        "//xla/pjrt:pjrt_client",
        "//xla/pjrt:pjrt_common",
        "//xla/pjrt:pjrt_compiler",
        "//xla/pjrt:pjrt_device_description",
        "//xla/pjrt:pjrt_executable",
        "//xla/pjrt:pjrt_future",
        "//xla/pjrt:pjrt_layout",
        "//xla/pjrt:status_casters",
        "//xla/pjrt:transpose",
        "//xla/pjrt/distributed",
        "//xla/pjrt/distributed:client",
        "//xla/python/compile_only_ifrt:client",
        "//xla/python/ifrt",
        "//xla/python/ifrt:attribute_map",
        "//xla/python/ifrt:custom_call_program",
        "//xla/python/ifrt:plugin_program",
        "//xla/python/ifrt:plugin_program_serdes",
        "//xla/python/ifrt:user_context",
        "//xla/python/ifrt/hlo:hlo_program",
        "//xla/python/pjrt_ifrt",
        "//xla/python/pjrt_ifrt:pjrt_attribute_map_util",
        "//xla/python/pjrt_ifrt:pjrt_dtype",
        "//xla/python/pjrt_ifrt:xla_host_callback_proto_cc",
        "//xla/python/pjrt_ifrt:xla_ifrt",
        "//xla/service:computation_placer_hdr",
        "//xla/service:custom_call_status",
        "//xla/service:custom_call_target_registry",
        "//xla/service:platform_util",
        "//xla/service/spmd/shardy:constants",
        "//xla/service/spmd/shardy:utils",
        "//xla/service/spmd/shardy/sdy_round_trip:pipelines",
        "//xla/tsl/concurrency:ref_count",
        "//xla/tsl/framework:allocator",
        "//xla/tsl/framework/mlir:status_scoped_diagnostic_handler",
        "//xla/tsl/platform:env",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:logging",
        "//xla/tsl/platform:status",
        "//xla/tsl/platform:statusor",
        "//xla/tsl/python/lib/core:numpy",
        "@tsl//tsl/platform:casts",
        "@tsl//tsl/platform:fingerprint",
        "@tsl//tsl/platform:ml_dtypes",
        "@tsl//tsl/profiler/lib:profiler_session",
        "@tsl//tsl/profiler/lib:traceme",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc",
    ] + if_google(["@com_google_protobuf//:any_cc_proto"]),
)

cc_library(
    name = "py_host_callback",
    srcs = ["py_host_callback.cc"],
    hdrs = ["py_host_callback.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        ":callback",
        ":py_host_callback_proto_cc",
        ":python_ref_manager",
        ":types",
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/pjrt:host_callback",
        "//xla/pjrt:pjrt_compiler",
        "//xla/python/ifrt",
        "//xla/python/pjrt_ifrt",
        "//xla/python/pjrt_ifrt:xla_host_callback_proto_cc",
        "//xla/tsl/concurrency:ref_count",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/log:check",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@llvm-project//llvm:Support",
        "@nanobind",
    ] + if_google([
        "@com_google_protobuf//:any_cc_proto",
    ]),
)

cc_library(
    name = "callback",
    srcs = [
        "callback.cc",
    ],
    hdrs = [
        "callback.h",
    ],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        ":nb_numpy",
        ":python_ref_manager",
        "//xla:comparison_util",
        "//xla:xla_data_proto_cc",
        "//xla/pjrt:host_callback",
        "//xla/pjrt:transpose",
        "//xla/service:custom_call_status",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:span",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "@nanobind",
    ],
)

cc_library(
    name = "py_client_cpu",
    srcs = ["py_client_cpu.cc"],
    hdrs = ["py_client_cpu.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        ":nb_numpy",
        ":types",
        "//xla:shape_util",
        "//xla/ffi:ffi_api",
        "//xla/ffi/api:ffi",
        "//xla/pjrt:host_callback",
        "//xla/pjrt:transpose",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_absl//absl/types:span",
        "@local_config_python//:python_headers",  # buildcleaner: keep
        "@nanobind",
    ],
    alwayslink = 1,
)

cc_library(
    name = "inspect_sharding",
    srcs = ["inspect_sharding.cc"],
    hdrs = ["inspect_sharding.h"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "//xla:xla_data_proto_cc",
        "//xla/hlo/ir:hlo",
        "//xla/service:custom_call_sharding_helper",
        "//xla/service/spmd:spmd_partitioner",
        "@com_google_absl//absl/status",
    ],
    # Always register 'InspectSharding' custom partitioning handler.
    alwayslink = 1,
)

cc_library(
    name = "custom_partition_callback",
    srcs = ["custom_partition_callback.cc"],
    hdrs = ["custom_partition_callback.h"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "//xla:debug_options_flags",
        "//xla:util",
        "//xla/hlo/builder:xla_computation",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/pass:hlo_pass_pipeline",
        "//xla/pjrt:mlir_to_hlo",
        "//xla/pjrt/c:pjrt_c_api_custom_partitioner_extension_hdrs",
        "//xla/pjrt/c:pjrt_c_api_hdrs",
        "//xla/pjrt/c:pjrt_c_api_helpers",
        "//xla/service:call_inliner",
        "//xla/service:custom_call_sharding_helper",
        "//xla/service/spmd:spmd_partitioner",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:status",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/cleanup",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "custom_call_batch_partitioner",
    srcs = ["custom_call_batch_partitioner.cc"],
    hdrs = ["custom_call_batch_partitioner.h"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "//xla:shape_util",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/utils:hlo_sharding_util",
        "//xla/service:custom_call_sharding_helper",
        "//xla/service/spmd:spmd_partitioner",
        "@com_google_absl//absl/log:check",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "ops",
    srcs = ["ops.cc"],
    hdrs = ["ops.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        ":nb_absl_span",
        ":nb_helpers",
        ":types",
        # placeholder for index annotation deps
        "@com_google_absl//absl/types:span",
        "@nanobind",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/builder:xla_builder",
        "//xla/hlo/builder:xla_computation",
        "//xla/hlo/builder/lib:approx_topk",
        "//xla/hlo/builder/lib:approx_topk_shape",
        "//xla/hlo/builder/lib:comparators",
        "//xla/hlo/builder/lib:lu_decomposition",
        "//xla/hlo/builder/lib:math",
        "//xla/hlo/builder/lib:qr",
        "//xla/hlo/builder/lib:self_adjoint_eig",
        "//xla/hlo/builder/lib:sorting",
        "//xla/hlo/builder/lib:svd",
        "//xla/pjrt:status_casters",
        "//xla/service:hlo_proto_cc",
    ],
)

cc_library(
    name = "refine_polymorphic_shapes",
    srcs = ["refine_polymorphic_shapes.cc"],
    hdrs = ["refine_polymorphic_shapes.h"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "//xla/mlir/utils:error_util",
        "//xla/mlir_hlo:mhlo_passes",
        "//xla/mlir_hlo:stablehlo_extension_passes",
        "//xla/service/spmd/shardy/round_trip_common:import_constants",
        "//xla/service/spmd/shardy/sdy_round_trip:pipelines",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:logging",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:BytecodeWriter",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:FuncExtensions",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Parser",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:Transforms",
        "@shardy//shardy/dialect/sdy/ir:dialect",
        "@stablehlo//:base",
        "@stablehlo//:chlo_ops",
        "@stablehlo//:stablehlo_ops",
    ],
)

cc_library(
    name = "profiler_utils",
    srcs = ["profiler_utils.cc"],
    hdrs = ["profiler_utils.h"],
    compatible_with = get_compatible_with_portable(),
    visibility = internal_visibility([":friends"]),
    deps = [
        "//xla/backends/profiler:profiler_backends",
        "//xla/backends/profiler/plugin:plugin_tracer",
        "//xla/backends/profiler/plugin:profiler_c_api_hdrs",
        "//xla/pjrt/c:pjrt_c_api_hdrs",
        "//xla/pjrt/c:pjrt_c_api_helpers",
        "//xla/pjrt/c:pjrt_c_api_profiler_extension_hdrs",
        "@tsl//tsl/profiler/lib:profiler_factory",
        "@tsl//tsl/profiler/lib:profiler_interface",
        "@tsl//tsl/profiler/protobuf:profiler_options_proto_cc",
    ],
)

cc_library(
    name = "profiler",
    srcs = ["profiler.cc"],
    hdrs = ["profiler.h"],
    # TODO(b/172353882): figure out why compatible_with is needed to avoid some internal errors.
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        ":aggregate_profile",
        ":profiler_utils",
        ":xplane_to_profile_instructions",
        # placeholder for index annotation deps
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@nanobind",
        "//xla/backends/profiler:profiler_backends",
        "//xla/backends/profiler/cpu:python_tracer",
        "//xla/backends/profiler/plugin:plugin_tracer",
        "//xla/backends/profiler/plugin:profiler_c_api_hdrs",
        "//xla/pjrt:exceptions",
        "//xla/pjrt:status_casters",
        "//xla/pjrt/c:pjrt_c_api_hdrs",
        "//xla/pjrt/c:pjrt_c_api_profiler_extension_hdrs",
        "//xla/python/profiler:profile_data_lib",
        "//xla/tsl/platform:macros",
        "//xla/tsl/profiler/rpc:profiler_server_impl",
        "//xla/tsl/profiler/rpc/client:capture_profile",
        "//xla/tsl/profiler/rpc/client:profiler_client_impl",
        "@tsl//tsl/platform:protobuf",
        "@tsl//tsl/profiler/lib:profiler_factory",
        "@tsl//tsl/profiler/lib:profiler_interface",
        "@tsl//tsl/profiler/lib:profiler_session",
        "@tsl//tsl/profiler/lib:traceme",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc",
        "@tsl//tsl/profiler/protobuf:profiler_options_proto_cc",
        "@tsl//tsl/profiler/protobuf:xplane_proto_cc",
    ],
)

cc_library(
    name = "guard_lib",
    srcs = ["guard_lib.cc"],
    hdrs = ["guard_lib.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        # placeholder for index annotation deps
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/functional:function_ref",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@nanobind",
        "//xla:util",
    ],
)

cc_library(
    name = "util",
    srcs = ["util.cc"],
    hdrs = ["util.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        "//xla:util",
        "//xla/python/ifrt",
        "//xla/tsl/concurrency:ref_count",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/types:span",
    ],
)

tf_proto_library(
    name = "py_host_callback_proto",
    srcs = ["py_host_callback.proto"],
    visibility = internal_visibility([":friends"]),
)

cc_library(
    name = "logging",
    srcs = ["logging.cc"],
    hdrs = ["logging.h"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/log:initialize",
    ],
)

cc_library(
    name = "xplane_to_profile_instructions",
    srcs = ["xplane_to_profile_instructions.cc"],
    hdrs = ["xplane_to_profile_instructions.h"],
    deps = [
        "//xla:xla_proto_cc",
        "//xla/hlo/ir:hlo",
        "//xla/service:hlo_proto_cc",
        "//xla/tsl/platform:env",
        "//xla/tsl/platform:types",
        "//xla/tsl/profiler/convert:xla_op_utils",
        "//xla/tsl/profiler/utils:file_system_utils",
        "//xla/tsl/profiler/utils:tf_xplane_visitor",
        "//xla/tsl/profiler/utils:xplane_schema",
        "//xla/tsl/profiler/utils:xplane_utils",
        "//xla/tsl/profiler/utils:xplane_visitor",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc",
        "@tsl//tsl/profiler/protobuf:xplane_proto_cc",
    ],
)

xla_cc_test(
    name = "xplane_to_profile_instructions_test",
    srcs = ["xplane_to_profile_instructions_test.cc"],
    deps = [
        ":xplane_to_profile_instructions",
        "//xla/hlo/testlib:verified_hlo_module",
        "//xla/service:hlo_proto_cc",
        "//xla/tsl/platform:test",
        "//xla/tsl/platform:test_main",
        "//xla/tsl/profiler/convert:xla_op_utils",
        "//xla/tsl/profiler/rpc/client:save_profile",
        "//xla/tsl/profiler/utils:file_system_utils",
        "//xla/tsl/profiler/utils:xplane_builder",
        "//xla/tsl/profiler/utils:xplane_schema",
        "@com_google_googletest//:gtest",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc_impl",
        "@tsl//tsl/profiler/protobuf:xplane_proto_cc",
    ],
)

cc_library(
    name = "nb_class_ptr",
    hdrs = ["nb_class_ptr.h"],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = ["@nanobind"],
)

cc_library(
    name = "nb_helpers",
    hdrs = ["nb_helpers.h"],
    compatible_with = [],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    deps = [
        "@com_google_absl//absl/strings:str_format",
        "@local_config_python//:python_headers",
        "@nanobind",
    ],
)

cc_library(
    name = "nb_numpy",
    srcs = ["nb_numpy.cc"],
    hdrs = ["nb_numpy.h"],
    compatible_with = [],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/strings:string_view",
        "@com_google_absl//absl/types:span",
        "@nanobind",
        # copybara:uncomment "//third_party/py/numpy:multiarray",
        "@local_config_python//:python_headers",
        "//xla/tsl/python/lib/core:numpy",
    ],
)

cc_library(
    name = "nb_absl_inlined_vector",
    hdrs = ["nb_absl_inlined_vector.h"],
    compatible_with = [],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/container:inlined_vector",
        "@nanobind",
    ],
)

cc_library(
    name = "nb_absl_span",
    hdrs = ["nb_absl_span.h"],
    compatible_with = [],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/types:span",
        "@nanobind",
    ],
)

cc_library(
    name = "nb_absl_flat_hash_map",
    hdrs = ["nb_absl_flat_hash_map.h"],
    compatible_with = [],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/container:flat_hash_map",
        "@nanobind",
    ],
)

cc_library(
    name = "nb_absl_flat_hash_set",
    hdrs = ["nb_absl_flat_hash_set.h"],
    compatible_with = [],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    visibility = internal_visibility([":friends"]),
    deps = [
        "@com_google_absl//absl/container:flat_hash_set",
        "@nanobind",
    ],
)

cc_library(
    name = "aggregate_profile",
    srcs = ["aggregate_profile.cc"],
    hdrs = ["aggregate_profile.h"],
    deps = [
        ":xplane_to_profile_instructions",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/types:span",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc",
    ],
)

xla_cc_test(
    name = "aggregate_profile_test",
    srcs = ["aggregate_profile_test.cc"],
    deps = [
        ":aggregate_profile",
        "//xla/tsl/platform:test",
        "//xla/tsl/platform:test_main",
        "@com_google_absl//absl/types:span",
        "@com_google_googletest//:gtest",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc_impl",
    ],
)

py_strict_test(
    name = "xla_compiler_test",
    srcs = ["xla_compiler_test.py"],
    main = "xla_compiler_test.py",
    tags = ["no_oss"],
    deps = [
        ":xla_extension",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:absltest",
        "@absl_py//absl/testing:parameterized",
    ] + xla_py_test_deps(),
)
