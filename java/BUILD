package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//container:container.bzl", "container_image")
load("@io_bazel_rules_docker//contrib:test.bzl", "container_test")
load("@io_bazel_rules_docker//java:image.bzl", "java_image")
load("@package_bundle//file:packages.bzl", "packages", "versions")
load("@package_bundle_debian10//file:packages.bzl", packages_debian10 = "packages", versions_debian10 = "versions")
load("//cacerts:java.bzl", "cacerts_java")
load("//java:jre_ver.bzl", "jre_ver")
load("//java:jre_ver.bzl", "zulu_ver")

DISTRO_SUFFIXES = ("_debian9", "_debian10")

DISTRO_PACKAGES = {
    "_debian9": packages,
    "_debian10": packages_debian10,
}

DISTRO_VERSIONS = {
    "_debian9": versions,
    "_debian10": versions_debian10,
}

cacerts_java(
    name = "cacerts_java",
)

[container_image(
    name = rule_name + distro_suffix,
    base = ("//cc:cc" if (not ("debug" in rule_name)) else "//cc:debug") + distro_suffix,
    debs = [
        DISTRO_PACKAGES[distro_suffix]["zlib1g"],
        DISTRO_PACKAGES[distro_suffix]["libjpeg62-turbo"],
        DISTRO_PACKAGES[distro_suffix]["libpng16-16"],
        DISTRO_PACKAGES[distro_suffix]["liblcms2-2"],
        DISTRO_PACKAGES[distro_suffix]["libfreetype6"],
        DISTRO_PACKAGES[distro_suffix]["fonts-dejavu-core"],
        DISTRO_PACKAGES[distro_suffix]["fontconfig-config"],
        DISTRO_PACKAGES[distro_suffix]["libexpat1"],
        DISTRO_PACKAGES[distro_suffix]["libfontconfig1"],
    ] + [DISTRO_PACKAGES[distro_suffix][deb] for deb in java_debs],
    # We expect users to use:
    # cmd = ["/path/to/deploy.jar", "--option1", ...]
    entrypoint = [
        "/usr/bin/java",
        "-jar",
    ],
    env = {
        "JAVA_VERSION": jre_ver(DISTRO_VERSIONS[distro_suffix][java_debs[0]]),
    },
    symlinks = {
        "/usr/bin/java": java_executable_path,
    },
    tars = [":cacerts_java"],
) for (distro_suffix, rule_name, java_debs, java_executable_path) in [
    (
        "_debian9",
        "java8",
        ["openjdk-8-jre-headless"],
        "/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java",
    ),
    (
        "_debian9",
        "java8_debug",
        [
            "openjdk-8-jre-headless",
            "openjdk-8-jdk-headless",
        ],
        "/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java",
    ),
    (
        "_debian9",
        "java11",
        ["openjdk-11-jre-headless"],
        "/usr/lib/jvm/java-11-openjdk-amd64/bin/java",
    ),
    (
        "_debian9",
        "java11_debug",
        [
            "openjdk-11-jre-headless",
            "openjdk-11-jdk-headless",
        ],
        "/usr/lib/jvm/java-11-openjdk-amd64/bin/java",
    ),
    (
        "_debian10",
        "java11",
        ["openjdk-11-jre-headless"],
        "/usr/lib/jvm/java-11-openjdk-amd64/bin/java",
    ),
    (
        "_debian10",
        "java11_debug",
        [
            "openjdk-11-jre-headless",
            "openjdk-11-jdk-headless",
        ],
        "/usr/lib/jvm/java-11-openjdk-amd64/bin/java",
    ),
]]

[container_image(
    name = rule_name + distro_suffix,
    base = ("//cc:cc" if (not ("debug" in rule_name)) else "//cc:debug") + distro_suffix,
    debs = [
        DISTRO_PACKAGES[distro_suffix]["zlib1g"],
        DISTRO_PACKAGES[distro_suffix]["fonts-dejavu-core"],
        DISTRO_PACKAGES[distro_suffix]["fontconfig-config"],
        DISTRO_PACKAGES[distro_suffix]["libexpat1"],
        DISTRO_PACKAGES[distro_suffix]["libfontconfig1"],
    ] + [DISTRO_PACKAGES[distro_suffix][deb] for deb in java_debs],
    env = {
        "JAVA_VERSION": zulu_ver(DISTRO_VERSIONS[distro_suffix][java_debs[0]]),
    },
    symlinks = {
        "/usr/bin/java": java_executable_path,
    },
) for (distro_suffix, rule_name, java_debs, java_executable_path) in [
    (
        "_debian9",
        "zulu8",
        [
            "zre-8",
        ],
        "/usr/lib/jvm/zre-8-amd64/bin/java",
    ),
    (
        "_debian9",
        "zulu8_debug",
        [
            "zulu-8",
        ],
        "/usr/lib/jvm/zulu-8-amd64/bin/java",
    ),
    (
        "_debian9",
        "zulu11",
        [
            "zre-11",
        ],
        "/usr/lib/jvm/zre-11-amd64/bin/java",
    ),
    (
        "_debian9",
        "zulu11_debug",
        [
            "zulu-11",
        ],
        "/usr/lib/jvm/zulu-11-amd64/bin/java",
    ),
    (
        "_debian10",
        "zulu11",
        [
            "zre-11",
        ],
        "/usr/lib/jvm/zre-11-amd64/bin/java",
    ),
    (
        "_debian10",
        "zulu11_debug",
        [
            "zulu-11",
        ],
        "/usr/lib/jvm/zulu-11-amd64/bin/java",
    ),
    (
        "_debian9",
        "zulu13_debug",
        [
            "zulu-13",
        ],
        "/usr/lib/jvm/zulu-13-amd64/bin/java",
    ),
    (
        "_debian10",
        "zulu13_debug",
        [
            "zulu-13",
        ],
        "/usr/lib/jvm/zulu-13-amd64/bin/java",
    ),
]]

# Provide aliases so the default images use debian9
alias(
    name = "java8",
    actual = ":java8_debian9",
)

alias(
    name = "java8_debug",
    actual = ":java8_debug_debian9",
)

alias(
    name = "java11",
    actual = ":java11_debian9",
)

alias(
    name = "java11_debug",
    actual = ":java11_debug_debian9",
)

alias(
    name = "zulu8",
    actual = ":zulu8_debian9",
)

alias(
    name = "zulu8_debug",
    actual = ":zulu8_debug_debian9",
)

alias(
    name = "zulu11",
    actual = ":zulu11_debian9",
)

alias(
    name = "zulu11_debug",
    actual = ":zulu11_debug_debian9",
)

alias(
    name = "zulu13_debug",
    actual = ":zulu13_debug_debian9",
)

container_test(
    name = "java8_debian9_test",
    configs = ["testdata/java8.yaml"],
    image = ":java8_debian9",
)

container_test(
    name = "java8_debug_debian9_test",
    configs = ["testdata/java8_debug.yaml"],
    image = ":java8_debug_debian9",
)

[container_test(
    name = "java11" + distro_suffix + "_test",
    configs = ["testdata/java11.yaml"],
    image = ":java11" + distro_suffix,
) for distro_suffix in DISTRO_SUFFIXES]

[container_test(
    name = "java11_debug" + distro_suffix + "_test",
    configs = ["testdata/java11_debug.yaml"],
    image = ":java11_debug" + distro_suffix,
) for distro_suffix in DISTRO_SUFFIXES]

container_test(
    name = "zulu8_debian9_test",
    configs = ["testdata/zulu8.yaml"],
    image = ":zulu8_debian9",
)

container_test(
    name = "zulu8_debug_debian9_test",
    configs = ["testdata/zulu8_debug.yaml"],
    image = ":zulu8_debug_debian9",
)

[container_test(
    name = "zulu11" + distro_suffix + "_test",
    configs = ["testdata/zulu11.yaml"],
    image = ":zulu11" + distro_suffix,
) for distro_suffix in DISTRO_SUFFIXES]

[container_test(
    name = "zulu11_debug" + distro_suffix + "_test",
    configs = ["testdata/zulu11_debug.yaml"],
    image = ":zulu11_debug" + distro_suffix,
) for distro_suffix in DISTRO_SUFFIXES]

[container_test(
    name = "zulu13_debug" + distro_suffix + "_test",
    configs = ["testdata/zulu13_debug.yaml"],
    image = ":zulu13_debug" + distro_suffix,
) for distro_suffix in DISTRO_SUFFIXES]

java_image(
    name = "check_certs_java8_debian9",
    srcs = ["testdata/CheckCerts.java"],
    base = "//java:java8_debian9",
    main_class = "testdata.CheckCerts",
)

[java_image(
    name = "check_certs_java11" + distro_suffix,
    srcs = ["testdata/CheckCerts.java"],
    base = "//java:java11" + distro_suffix,
    main_class = "testdata.CheckCerts",
) for distro_suffix in DISTRO_SUFFIXES]

container_test(
    name = "check_certs_java8_debian9_test",
    configs = ["testdata/java8_certs.yaml"],
    image = ":check_certs_java8_debian9",
)

[container_test(
    name = "check_certs_java11" + distro_suffix + "_test",
    configs = ["testdata/java11_certs" + distro_suffix + ".yaml"],
    image = ":check_certs_java11" + distro_suffix,
) for distro_suffix in DISTRO_SUFFIXES]
