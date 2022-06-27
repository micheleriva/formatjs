load("@aspect_bazel_lib//lib:write_source_files.bzl", "write_source_files")
load("@aspect_rules_js//npm/private:npm_package.bzl", "npm_package")
load("//tools:index.bzl", "check_format", "generate_src_file", "package_json_test", "ts_compile")
load("//tools:jest.bzl", "jest_test")

exports_files(["package.json", "tsconfig.json"])

PACKAGE_NAME = "icu-skeleton-parser"

npm_package(
    name = PACKAGE_NAME,
    package = "@formatjs/%s" % PACKAGE_NAME,
    srcs = [
        "LICENSE.md",
        "README.md",
        "package.json",
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

SRCS = glob(["*.ts"])

SRC_DEPS = [
    "//packages/ecma402-abstract:dist",
]

ts_compile(
    name = "dist",
    package = "@formatjs/%s" % PACKAGE_NAME,
    srcs = SRCS,
    skip_esm = False,
    deps = SRC_DEPS,
)

jest_test(
    name = "unit",
    srcs = SRCS + glob([
        "tests/**/*.ts",
        "tests/**/*.tsx",
        "tests/**/*.snap",
    ]),
    deps = [
        "//packages/ecma402-abstract:types",
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
            "**/*.generated.ts",
            "CHANGELOG.md",
            "tests/__snapshots__/*",
        ],
    ),
)

generate_src_file(
    name = "regex",
    src = "regex.generated.ts",
    data = [
        "scripts/global.ts",
        "//:node_modules/@unicode/unicode-13.0.0",
        "//:node_modules/regenerate",
    ],
    entry_point = "scripts/regex-gen.ts",
)

package_json_test(
    name = "package_json_test",
    deps = SRC_DEPS,
)
