load("@aspect_bazel_lib//lib:write_source_files.bzl", "write_source_files")
load("@aspect_rules_js//npm/private:npm_package.bzl", "npm_package")
load("//tools:index.bzl", "check_format", "package_json_test", "ts_compile")
load("//tools:jest.bzl", "jest_test")

exports_files(["package.json", "tsconfig.json"])

PACKAGE_NAME = "eslint-plugin-formatjs"

npm_package(
    name = PACKAGE_NAME,
    package = PACKAGE_NAME,
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

SRCS = glob(["rules/*.ts"]) + [
    "index.ts",
    "util.ts",
]

SRC_DEPS = [
    "//:node_modules/@types/eslint",
    "//:node_modules/@typescript-eslint/typescript-estree",
    "//:node_modules/emoji-regex",
    "//:node_modules/eslint",
    "//:node_modules/typescript",
    "//packages/icu-messageformat-parser:dist",
    "//packages/ts-transformer:dist",
]

ts_compile(
    name = "dist",
    package = PACKAGE_NAME,
    srcs = SRCS,
    deps = SRC_DEPS,
)

TESTS_BASE_SRCS = SRCS + glob(
    [
        "tests/*.ts",
    ],
    exclude = ["tests/*.test.ts"],
)

TEST_FILES = glob([
    "tests/*.test.ts",
])

[jest_test(
    name = "unit-%s" % f[6:f.index(".test.ts")],
    srcs = TESTS_BASE_SRCS + [f],
    deps = [
        "//:node_modules/@typescript-eslint/parser",
        "//:node_modules/vue-eslint-parser",
        "//packages/icu-messageformat-parser:types",
        "//packages/ts-transformer:types",
    ] + SRC_DEPS,
) for f in TEST_FILES]

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

package_json_test(
    name = "package_json_test",
    deps = SRC_DEPS,
)
