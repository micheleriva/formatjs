load("@aspect_bazel_lib//lib:copy_to_bin.bzl", "copy_to_bin")
load("@npm//@docusaurus/core:package_json.bzl", docusaurus_bin = "bin")
load("@npm//http-server:package_json.bzl", http_server_bin = "bin")
load("//tools:index.bzl", "check_format")

check_format(
    name = "prettier",
    srcs = glob(
        [
            "docs/**/*",
            "lang/*",
            "src/**/*",
            "*.js*",
        ],
    ) + [
        "BUILD",
    ],
    config = ".prettierrc.json",
)

DEPS = [
    ":copy",
    "//packages/intl-locale:dist",
    "//packages/intl-displaynames:dist",
    "//packages/intl-displaynames:locale-data",
    "//packages/intl-listformat:dist",
    "//packages/intl-listformat:locale-data",
    "//packages/intl-messageformat:dist",
    "//packages/icu-messageformat-parser:dist",
    "//packages/react-intl:dist",
    "@npm//@docusaurus/plugin-google-analytics",
    "@npm//@docusaurus/preset-classic",
    "@npm//@docusaurus/theme-live-codeblock",
    "@npm//classnames",
    "@npm//react",
    "@npm//react-dom",
]

copy_to_bin(
    name = "copy",
    srcs = glob([
        "docs/**/*",
        "lang/*",
        "src/**/*",
        "static/**/*",
        "*.js",
    ]),
)

docusaurus_bin.docusaurus(
    name = "build",
    args = [
        "build",
        "--out-dir",
        "$(@D)",
        "$(BINDIR)/website",
    ],
    srcs = DEPS,
    output_dir = True,
)

docusaurus_bin.docusaurus(
    name = "deploy",
    # configuration_env_vars = [
    #     "USE_SSH",
    #     "GIT_USER",
    #     "DEPLOYMENT_BRANCH",
    #     "NO_UPDATE_NOTIFIER",
    # ],
    srcs = DEPS + [
        ":build",
        "docusaurus.config.js",
    ],
    args = [
        "deploy",
        "--skip-build",
        "--out-dir",
        "$$(rlocation $(location :build))",
        "website",
    ],
)

http_server_bin.http_server(
    name = "serve",
    srcs = [
        ":build",
    ],
    args = [
        "$$(rlocation $(location :build))",
    ],
)
