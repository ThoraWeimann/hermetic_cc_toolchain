load("@hermetic_cc_toolchain//rules:platform.bzl", "platform_binary")

cc_binary(
    name = "which_libc",
    srcs = ["main.c"],
    target_compatible_with = [
        "@platforms//os:linux",
    ],
)

[
    (
        platform_binary(
            name = "which_libc_{}".format(name),
            src = "which_libc",
            platform = platform,
        ),
        sh_test(
            name = "test_libc_{}".format(name),
            srcs = ["test.sh"],
            data = ["which_libc_{}".format(name)],
            env = {
                "WANT": want,
                "BINARY": "$(location which_libc_{})".format(name),
            },
        ),
    )
    for name, platform, want in [
        ("linux_amd64_musl", "//libc_aware/platform:linux_amd64_musl", "non-glibc"),
        ("linux_amd64_gnu.2.28", "//libc_aware/platform:linux_amd64_gnu.2.28", "glibc_2.28"),
        ("linux_amd64", "//platform:linux_amd64", "glibc_2.28"),
    ]
]
