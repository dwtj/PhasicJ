load("@io_bazel_rules_rust//rust:rust.bzl", "rust_library")
load("@dwtj_rules_java//java:defs.bzl", "dwtj_java_binary")
load("//bazel/util/rust:rename_cdylib.bzl", "rename_cdylib")

rust_library(
    name = "ptagent",
    srcs = ["lib.rs"],
    crate_type = "cdylib",
    edition = "2018",
    deps = ["@openjdk_linux_x64//rust/jvmti"],
)

dwtj_java_binary(
    name = "MyApp",
    main_class = "pt.agent.MyApp",
    srcs = ["MyApp.java"],
    output_jar = "MyApp.jar",
)

rename_cdylib(
  name = "rename",
  src = ":ptagent",
  out = "libptagent.so",
)

sh_test(
    name = "test",
    srcs = ["test.sh"],
    data = [
        ":libptagent.so",
        ":MyApp.jar",
    ],
    args = [
        "$(location :libptagent.so)",
        "$(location :MyApp.jar)",
    ],
)