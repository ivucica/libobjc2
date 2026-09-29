load(
    "@gnustep_make//bazel:objc.bzl",
    "eh_trampoline_asm",
    "gnustep_expand_template",
    "gnustep_platform_linkopts",
    "objc_binary",
    "objc_library",
    "objc_test",
)

package(
    default_visibility = ["//visibility:public"],
)

gnustep_expand_template(
    name = "objc_config_h",
    out = ".bazel/include/objc/objc-config.h",
    substitutions = {
        "#cmakedefine STRICT_APPLE_COMPATIBILITY @STRICT_APPLE_COMPATIBILITY@": "/* #undef STRICT_APPLE_COMPATIBILITY */",
    },
    template = "objc/objc-config.h.in",
)

eh_trampoline_asm(
    name = "eh_trampoline_s",
    out = ".bazel/eh_trampoline.S",
    src = "eh_trampoline.cc",
)

objc_library(
    name = "objc",
    srcs = [
        "block_trampolines.S",
        "objc_msgSend.S",
        ":eh_trampoline_s",
        "abi_version.c",
        "alias_table.c",
        "block_to_imp.c",
        "builtin_classes.c",
        "caps.c",
        "category_loader.c",
        "class_table.c",
        "dtable.c",
        "eh_personality.c",
        "encoding2.c",
        "gc_none.c",
        "hooks.c",
        "ivar.c",
        "legacy.c",
        "loader.c",
        "protocol.c",
        "runtime.c",
        "sarray2.c",
        "sendmsg2.c",
        "statics_loader.c",
        "NSBlocks.m",
        "blocks_runtime.m",
        "blocks_runtime_np.m",
        "fast_paths.m",
        "mutation.m",
        "arc.mm",
        "associate.mm",
        "properties.mm",
        "objcxx_eh.cc",
        "selector_table.cc",
        "alias.h",
        "asmconstants.h",
        "blocks_runtime.h",
        "buffer.h",
        "category.h",
        "class.h",
        "constant_string.h",
        "dtable.h",
        "dwarf_eh.h",
        "gc_ops.h",
        "hash_table.h",
        "helpers.hh",
        "ivar.h",
        "legacy.h",
        "loader.h",
        "lock.h",
        "method.h",
        "module.h",
        "nsobject.h",
        "objcxx_eh.h",
        "objcxx_eh_private.h",
        "pool.hh",
        "properties.h",
        "protocol.h",
        "safewindows.h",
        "sarray2.h",
        "selector.h",
        "spinlock.h",
        "string_hash.h",
        "type_encoding_cases.h",
        "unwind-arm.h",
        "unwind-itanium.h",
        "unwind.h",
        "visibility.h",
    ],
    hdrs = [
        "Block.h",
        "Block_private.h",
        "common.S",
        "objc/Availability.h",
        "objc/Object.h",
        "objc/Protocol.h",
        "objc/blocks_private.h",
        "objc/blocks_runtime.h",
        "objc/capabilities.h",
        "objc/developer.h",
        "objc/encoding.h",
        "objc/hooks.h",
        "objc/message.h",
        "objc/objc-api.h",
        "objc/objc-arc.h",
        "objc/objc-auto.h",
        "objc/objc-class.h",
        "objc/objc-exception.h",
        "objc/objc-runtime.h",
        "objc/objc-visibility.h",
        "objc/objc.h",
        "objc/runtime-deprecated.h",
        "objc/runtime.h",
        "objc/slot.h",
        ":objc_config_h",
    ] + select({
        "@gnustep_make//bazel:cpu_arm64": ["objc_msgSend.aarch64.S"],
        "//conditions:default": ["objc_msgSend.x86-64.S"],
    }),
    conlyopts = [
        "-Xclang",
        "-fexceptions",
        "-Wno-gnu-folding-constant",
    ],
    cxxopts = [
        "-std=c++20",
        "-fexceptions",
    ],
    includes = [
        ".",
        "objc",
        ".bazel/include",
        ".bazel/include/objc",
    ],
    linkopts = gnustep_platform_linkopts() + [
    ],
    local_defines = [
        "GNUSTEP",
        "__OBJC_RUNTIME_INTERNAL__=1",
        "__OBJC_BOOL",
        "TYPE_DEPENDENT_DISPATCH",
        "OLDABI_COMPAT=1",
        "NO_LEGACY",
        "EMBEDDED_BLOCKS_RUNTIME",
        "CXA_ALLOCATE_EXCEPTION_SPECIFIER=noexcept",
    ],
    deps = [
        "@robin_map//:robin_map",
    ],
)

alias(
    name = "gs_libobjc2",
    actual = ":objc",
)

objc_library(
    name = "test_runtime",
    testonly = True,
    srcs = ["Test/Test.m"],
    hdrs = ["Test/Test.h"],
    includes = ["Test"],
    objc_runtime = "gnustep-2.0",
    deps = [":objc"],
)

objc_binary(
    name = "allocate_pair_bin",
    testonly = True,
    srcs = ["Test/AllocatePair.m"],
    copts = [
        "-UNDEBUG",
        "-DGS_RUNTIME_V2",
    ],
    objc_runtime = "gnustep-2.2",
    deps = [
        ":objc",
        ":test_runtime",
    ],
)

objc_test(
    name = "AllocatePair_test",
    size = "small",
    srcs = ["Test/AllocatePair.m"],
    copts = [
        "-UNDEBUG",
        "-DGS_RUNTIME_V2",
    ],
    objc_runtime = "gnustep-2.2",
    deps = [
        ":objc",
        ":test_runtime",
    ],
)

objc_test(
    name = "BlockTest_arc_test",
    size = "small",
    arc_srcs = ["Test/BlockTest_arc.m"],
    copts = [
        "-UNDEBUG",
        "-DGS_RUNTIME_V2",
    ],
    objc_runtime = "gnustep-2.2",
    deps = [
        ":objc",
        ":test_runtime",
    ],
)

objc_test(
    name = "PropertyIntrospectionTest2_arc_test",
    size = "small",
    arc_srcs = ["Test/PropertyIntrospectionTest2_arc.m"],
    copts = [
        "-UNDEBUG",
        "-DGS_RUNTIME_V2",
    ],
    objc_runtime = "gnustep-2.2",
    deps = [
        ":objc",
        ":test_runtime",
    ],
)
