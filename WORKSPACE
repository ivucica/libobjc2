workspace(name = "libobjc2")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

local_repository(
    name = "gnustep_make",
    path = "../tools-make-bazel",
)

http_archive(
    name = "robin_map",
    build_file_content = """
load("@rules_cc//cc:defs.bzl", "cc_library")

cc_library(
    name = "robin_map",
    hdrs = glob(["include/tsl/*.h"]),
    includes = ["include"],
    visibility = ["//visibility:public"],
)
""",
    sha256 = "0abd2a272947d1d403ce7467e75aae5bdcfe839f4fc8d513ba5bfe170d5f2057",
    strip_prefix = "robin-map-757de829927489bee55ab02147484850c687b620",
    urls = ["https://github.com/Tessil/robin-map/archive/757de82.tar.gz"],
)
