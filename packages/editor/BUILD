# load("@aspect_rules_js//npm/private:npm_package.bzl", "npm_package")
# load("@aspect_bazel_lib//lib:copy_to_bin.bzl", "copy_to_bin")

# # load("//tools:jest.bzl", "jest_test")
# load("@npm//http-server:package_json.bzl", "http_server")
load("//tools:index.bzl", "check_format")

# exports_files(["package.json", "tsconfig.json"])

# PACKAGE_NAME = "editor"

# npm_package(
#     name = PACKAGE_NAME,
#     package = "@formatjs/%s" % PACKAGE_NAME,
#     srcs = [
#         "LICENSE.md",
#         "README.md",
#         ":dist",
#         ":dist-esm",
#     ],
# )

# alias(
#     name = "publish",
#     actual = "%s.publish" % PACKAGE_NAME,
# )

# alias(
#     name = "pack",
#     actual = "%s.pack" % PACKAGE_NAME,
# )

# SRCS = glob(["*.ts*"])

# SRC_DEPS = [
#     "//:node_modules/@material-ui/core",
#     "//:node_modules/@material-ui/icons",
#     "//:node_modules/@material-ui/lab",
#     "//:node_modules/react",
#     "//:node_modules/@types/react",
#     "//:node_modules/react-dom",
#     "//:node_modules/@types/react-dom",
#     "//packages/react-intl:dist",
#     "//packages/icu-messageformat-parser:dist",
# ]

# ts_compile(
#     name = "dist",
#     package = "@formatjs/%s" % PACKAGE_NAME,
#     srcs = SRCS,
#     skip_esm = False,
#     deps = SRC_DEPS,
# )

# # jest_test(
# #     name = "unit",
# #     srcs = [
# #         "package.json",
# #         "tests/extract/unit.test.ts",
# #         "tests/extract/vue_extractor.test.ts",
# #         "tests/extract/__snapshots__/unit.test.ts.snap",
# #     ] + SRCS,
# #     deps = [
# #         "//packages/icu-messageformat-parser:types",
# #         "//packages/ts-transformer:types",
# #         "//:node_modules/rimraf",
# #         "//:node_modules/@types/rimraf",
# #     ] + SRC_DEPS,
# # )

check_format(
    name = "prettier",
    srcs = glob(
        [
            "**/*",
        ],
        exclude = [
            "CHANGELOG.md",
        ],
    ),
)

# esbuild(
#     name = "app",
#     define = {
#         "process.env.NODE_ENV": "development",
#     },
#     entry_point = "lib/main.js",
#     format = "iife",
#     deps = [":dist-esm"] + SRC_DEPS + [
#     ],
# )

# copy_to_bin(
#     name = "index",
#     srcs = ["index.html"],
# )

# http_server(
#     name = "server",
#     data = [
#         ":index",
#         ":app",
#     ] + glob(["fixtures/*.json"]),
#     args = [
#         "packages/editor",
#     ],
# )

# # package_json_test(
# #     name="package_json_test",
# #     deps=SRC_DEPS
# # )
