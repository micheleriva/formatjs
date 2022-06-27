load("@aspect_bazel_lib//lib:write_source_files.bzl", "write_source_files")
load("@aspect_rules_esbuild//esbuild:defs.bzl", "esbuild")
load("@aspect_rules_js//npm/private:npm_package.bzl", "npm_package")
load("@npm//ts-node:package_json.bzl", ts_node_bin = "bin")
load("//tools:index.bzl", "check_format", "package_json_test", "ts_compile")
load("//tools:jest.bzl", "jest_test")

exports_files(["package.json", "tsconfig.json"])

PACKAGE_NAME = "intl-messageformat"

npm_package(
    name = PACKAGE_NAME,
    package = PACKAGE_NAME,
    srcs = [
        "LICENSE.md",
        "README.md",
        "package.json",
        "%s.esm.js" % PACKAGE_NAME,
        "%s.iife.js" % PACKAGE_NAME,
        ":dist",
    ],
)

alias(
    name = "publish",
    actual = "%s.publish" % PACKAGE_NAME,
)

alias(
    name = "pack",
    actual = "%s.pack" % PACKAGE_NAME,
)

SRCS = glob(["src/*.ts"]) + ["index.ts"]

SRC_DEPS = [
    "//packages/icu-messageformat-parser:dist",
    "//packages/fast-memoize:dist",
    "//packages/ecma402-abstract:dist",
]

TESTS = glob([
    "tests/*.test.ts",
])

TEST_DEPS = SRC_DEPS + [
    "//:node_modules/jasmine-expect",
]

ts_compile(
    name = "dist",
    package = PACKAGE_NAME,
    srcs = SRCS,
    skip_esm = False,
    skip_esm_esnext = False,
    deps = SRC_DEPS,
)

jest_test(
    name = "unit",
    srcs = SRCS + TESTS,
    deps = TEST_DEPS + [
        "//packages/icu-messageformat-parser:types",
        "//packages/fast-memoize:types",
        "//packages/ecma402-abstract:types",
    ],
)

esbuild(
    name = "%s.iife" % PACKAGE_NAME,
    config = {
        "globalName": "IntlMessageFormat",
    },
    entry_point = "lib/index.js",
    target = "es5",
    deps = [
        ":dist-esm",
    ] + SRC_DEPS,
)

esbuild(
    name = "%s.esm" % PACKAGE_NAME,
    config = {
        "globalName": "IntlMessageFormat",
    },
    entry_point = "lib_esnext/index.js",
    format = "esm",
    target = "esnext",
    deps = [
        ":dist-esm-esnext",
    ] + SRC_DEPS,
)

write_source_files(
    name = "tsconfig_json",
    files = {"tsconfig.json": "//tools:tsconfig.golden.json"},
)

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

ts_node_bin.ts_node(
    name = "benchmark",
    args = [
        "--transpile-only",
        "$(execpath tests/benchmark.ts)",
    ],
    srcs = SRCS + SRC_DEPS + [
        "tests/benchmark.ts",
        "//:tsconfig.json",
        "//:node_modules/tslib",
        "//packages/intl-pluralrules",
        "//packages/intl-pluralrules:locale-data",
        "//:tsconfig.node.json",
    ],
)

package_json_test(
    name = "package_json_test",
    deps = SRC_DEPS,
)
