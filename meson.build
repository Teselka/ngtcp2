project(
    'ngtcp2',
    'c',
    default_options: [
        'b_ndebug=true',
    ],
    version: '1.22.1',
    meson_version: '>=1.1.0'
)

project_version = meson.project_version()
cc = meson.get_compiler('c')

sources = files(    
    'lib/ngtcp2_pkt.c',
    'lib/ngtcp2_conv.c',
    'lib/ngtcp2_str.c',
    'lib/ngtcp2_vec.c',
    'lib/ngtcp2_buf.c',
    'lib/ngtcp2_conn.c',
    'lib/ngtcp2_mem.c',
    'lib/ngtcp2_pq.c',
    'lib/ngtcp2_map.c',
    'lib/ngtcp2_rob.c',
    'lib/ngtcp2_ppe.c',
    'lib/ngtcp2_crypto.c',
    'lib/ngtcp2_err.c',
    'lib/ngtcp2_range.c',
    'lib/ngtcp2_acktr.c',
    'lib/ngtcp2_rtb.c',
    'lib/ngtcp2_frame_chain.c',
    'lib/ngtcp2_strm.c',
    'lib/ngtcp2_idtr.c',
    'lib/ngtcp2_gaptr.c',
    'lib/ngtcp2_ringbuf.c',
    'lib/ngtcp2_cid.c',
    'lib/ngtcp2_ksl.c',
    'lib/ngtcp2_cc.c',
    'lib/ngtcp2_bbr.c',
    'lib/ngtcp2_addr.c',
    'lib/ngtcp2_path.c',
    'lib/ngtcp2_pv.c',
    'lib/ngtcp2_pmtud.c',
    'lib/ngtcp2_version.c',
    'lib/ngtcp2_rst.c',
    'lib/ngtcp2_window_filter.c',
    'lib/ngtcp2_opl.c',
    'lib/ngtcp2_balloc.c',
    'lib/ngtcp2_objalloc.c',
    'lib/ngtcp2_transport_params.c',
    'lib/ngtcp2_settings.c',
    'lib/ngtcp2_callbacks.c',
    'lib/ngtcp2_dcidtr.c',
    'lib/ngtcp2_pcg.c',
    'lib/ngtcp2_ratelim.c',
    'lib/ngtcp2_conn_info.c',
    'lib/ngtcp2_fmt.c',
)

includes = include_directories('lib/includes/')

cdata = configuration_data()
cdata.set('PACKAGE_VERSION', project_version)

version_parts = project_version.split('.')

version_major = version_parts[0].to_int()
version_minor = version_parts[1].to_int()
version_patch = version_parts[2].to_int()

version_num = version_major * 256 * 256 + version_minor * 256 + version_patch

cdata.set('PACKAGE_VERSION_NUM', '@0@'.format(version_num))

version_h = configure_file(
    # build_subdir: 'lib/includes/ngtcp2',
    configuration: cdata,
    input: 'lib/includes/ngtcp2/version.h.in',
    output: 'version.h'
)

sources += version_h

defs = []

foreach header : [
    'arpa/inet.h',
    'netinet/in.h',
    'netinet/ip.h',
    'unistd.h',
    'sys/endian.h',
    'endian.h',
    'byteswap.h',
    'asm/types.h',
    'linux/netlink.h',
    'linux/rtnetlink.h',

]
    if cc.has_header(header)
        defs += '-DHAVE_@0@'.format(header.underscorify().to_upper())
    endif
endforeach

if get_option('disable_logging')
    defs += '-DNGTCP2_DISABLE_LOGGING'
else
    sources += files(
        'lib/ngtcp2_log.c',
    )
endif

if get_option('disable_strerror')
    defs += '-DNGTCP2_DISABLE_STRERROR'
endif

if get_option('disable_qlog')
    defs += '-DNGTCP2_DISABLE_QLOG'
else
    sources += files(
        'lib/ngtcp2_qlog.c',
    )
endif

if get_option('use_builtin_unreachable')
    defs += '-DNGTCP2_USE_BUILTIN_UNREACHABLE'
else
    sources += files(
        'lib/ngtcp2_unreachable.c'
    )
endif

ngtcp2_lib = library(
    'ngtcp2',
    sources,
    include_directories: includes,
    c_args: defs,
)

ngtcp2_dep = declare_dependency(
    link_with: ngtcp2_lib,
    include_directories: includes,
)

if get_option('tests')
    munit_dep = dependency('munit', required: false)
    if not munit_dep.found()
        munit_dep = declare_dependency(
            link_with: static_library(
                'munit', 
                files('tests/munit/munit.c'),
                include_directories: include_directories('tests/munit/')
            ),
            include_directories: include_directories('tests/munit/')
        )
    endif

    test_sources = files(
        'tests/main.c',
        'tests/ngtcp2_pkt_test.c',
        'tests/ngtcp2_range_test.c',
        'tests/ngtcp2_rob_test.c',
        'tests/ngtcp2_acktr_test.c',
        'tests/ngtcp2_map_test.c',
        'tests/ngtcp2_transport_params_test.c',
        'tests/ngtcp2_rtb_test.c',
        'tests/ngtcp2_idtr_test.c',
        'tests/ngtcp2_conn_test.c',
        'tests/ngtcp2_ringbuf_test.c',
        'tests/ngtcp2_conv_test.c',
        'tests/ngtcp2_ksl_test.c',
        'tests/ngtcp2_gaptr_test.c',
        'tests/ngtcp2_vec_test.c',
        'tests/ngtcp2_strm_test.c',
        'tests/ngtcp2_pv_test.c',
        'tests/ngtcp2_pmtud_test.c',
        'tests/ngtcp2_str_test.c',
        'tests/ngtcp2_tstamp_test.c',
        'tests/ngtcp2_cc_test.c',
        'tests/ngtcp2_window_filter_test.c',
        'tests/ngtcp2_settings_test.c',
        'tests/ngtcp2_callbacks_test.c',
        'tests/ngtcp2_ppe_test.c',
        'tests/ngtcp2_dcidtr_test.c',
        'tests/ngtcp2_addr_test.c',
        'tests/ngtcp2_pcg_test.c',
        'tests/ngtcp2_ratelim_test.c',
        'tests/ngtcp2_conn_info_test.c',
        'tests/ngtcp2_cid_test.c',
        'tests/ngtcp2_log_test.c',
        'tests/ngtcp2_fmt_test.c',
        'tests/ngtcp2_macro_test.c',
        'tests/ngtcp2_test_helper.c',
    )

    if not get_option('disable_qlog')
        test_sources += files(
            'tests/ngtcp2_qlog_test.c',
        )
    endif

    ngtcp2_test = executable(
        'ngtcp2_test',
        test_sources,
        dependencies: [ ngtcp2_dep, munit_dep ],
        include_directories: include_directories('lib/'),
        c_args: defs
    )

    test('ngtcp2_test', ngtcp2_test)
endif

openssl_opt = get_option('openssl')
wolfssl_opt = get_option('wolfssl')

if get_option('crypto')
    crypto_sources = files(
        'crypto/shared.c',
    )

    crypto_includes = include_directories(
        'lib/',
        'lib/includes/',
        'crypto/',
        'crypto/includes/',
    )

    if openssl_opt.allowed()
        openssl_dep = dependency('openssl', required: openssl_opt.enabled())
        if openssl_dep.found()
            ossl_crypto_sources = crypto_sources + files(
                'crypto/ossl/ossl.c',    
            )

            ngtcp2_crypto_ossl_lib = library(
                'ngtcp2_crypto_ossl',
                ossl_crypto_sources,
                dependencies: [ ngtcp2_dep, openssl_dep ],
                include_directories: crypto_includes,
            )

            ngtcp2_crypto_ossl_dep = declare_dependency(
                link_with: ngtcp2_crypto_ossl_lib,
                dependencies: openssl_dep,
                include_directories: crypto_includes,
            )

            openssl_opt = openssl_opt.enable_auto_if(true)
        endif
    endif

    if wolfssl_opt.allowed()
        wolfssl_dep = dependency('wolfssl', required: wolfssl_opt.enabled())
        if wolfssl_dep.found()
            if cc.compiles('''
                #include <wolfssl/ssl.h>
                #include <wolfssl/quic.h>
                #ifndef WOLFSSL_QUIC
                #error ""
                #endif
            ''')
                wolfssl_crypto_sources = crypto_sources + files(
                    'crypto/wolfssl/wolfssl.c',    
                )

                ngtcp2_crypto_wolfssl_lib = library(
                    'ngtcp2_crypto_wolfssl',
                    wolfssl_crypto_sources,
                    dependencies: [ ngtcp2_dep, wolfssl_dep ],
                    include_directories: crypto_includes,
                )

                ngtcp2_crypto_wolfssl_dep = declare_dependency(
                    link_with: ngtcp2_crypto_wolfssl_lib,
                    dependencies: wolfssl_dep,
                    include_directories: crypto_includes,
                )

                wolfssl_opt = wolfssl_opt.enable_auto_if(true)
            else
                if wolfssl_opt.enabled()
                    error('wolfSSL support was explicitly enabled but libary wasn\'t built with QUIC support')
                else
                    warning('wolfSSL was found but libary wasn\'t built with QUIC support')
                endif
            endif
        endif
    endif
endif

if get_option('examples')
    libev_dep = declare_dependency(
        include_directories: include_directories('/usr/include'),
        dependencies: cc.find_library('ev', dirs: [ '/usr/lib' ])
    )

    nghttp3_dep = dependency('nghttp3', fallback: [ 'nghttp3', 'nghttp3_dep' ])

    add_languages('cpp', native: false)

    examples_client_sources = files(
        'examples/client.cc',
        'examples/client_base.cc',
        'examples/http3_client_proto_codec.cc',
        'examples/debug.cc',
        'examples/util.cc',
        'examples/shared.cc',
    )

    examples_server_sources = files(
        'examples/server.cc',
        'examples/server_base.cc',
        'examples/http3_server_proto_codec.cc',
        'examples/debug.cc',
        'examples/util.cc',
        'examples/http.cc',
        'examples/shared.cc',
        'examples/siphash.cc',
    )

    if openssl_opt.enabled()
        osslclient_sources = examples_client_sources + files(
            'examples/tls_client_context_ossl.cc',
            'examples/tls_client_session_ossl.cc',
            'examples/tls_session_base_ossl.cc',
            'examples/util_openssl.cc',

            'third-party/urlparse/urlparse.c',
        )

        osslserver_sources = examples_server_sources + files(
            'examples/tls_server_context_ossl.cc',
            'examples/tls_server_session_ossl.cc',
            'examples/tls_session_base_ossl.cc',
            'examples/util_openssl.cc',

            'third-party/urlparse/urlparse.c',
        )

        ossl_includes = include_directories(
            'lib/',
            'third-party/',
            'third-party/urlparse/',
            'crypto/includes/',
        )

        ossl_deps = [ ngtcp2_dep, libev_dep, nghttp3_dep, ngtcp2_crypto_ossl_dep ]

        executable(
            'osslclient',
            osslclient_sources,
            cpp_args: [ defs, '-DWITH_EXAMPLE_OSSL', '-DWITH_EXAMPLE_HTTP3_PROTO_CODEC' ],
            override_options: [ 'cpp_std=c++23,c++latest' ],
            dependencies: ossl_deps,
            include_directories: ossl_includes,
        )

        executable(
            'osslserver',
            osslserver_sources,
            cpp_args: [ defs, '-DWITH_EXAMPLE_OSSL', '-DWITH_EXAMPLE_HTTP3_PROTO_CODEC' ],
            override_options: [ 'cpp_std=c++23,c++latest' ],
            dependencies: ossl_deps,
            include_directories: ossl_includes,
        )
    endif

    if wolfssl_opt.enabled()
        wolfsslclient_sources = examples_client_sources + files(
            'examples/tls_client_context_wolfssl.cc',
            'examples/tls_client_session_wolfssl.cc',
            'examples/tls_session_base_wolfssl.cc',
            'examples/util_openssl.cc',

            'third-party/urlparse/urlparse.c',
        )

        wolfsslserver_sources = examples_server_sources + files(
            'examples/tls_server_context_wolfssl.cc',
            'examples/tls_server_session_wolfssl.cc',
            'examples/tls_session_base_wolfssl.cc',
            'examples/util_openssl.cc',

            'third-party/urlparse/urlparse.c',
        )

        wolfssl_includes = include_directories(
            'lib/',
            'third-party/',
            'third-party/urlparse/',
            'crypto/includes/',
        )

        wolfssl_deps = [ ngtcp2_dep, libev_dep, nghttp3_dep, ngtcp2_crypto_wolfssl_dep ]

        executable(
            'wolfsslclient',
            wolfsslclient_sources,
            cpp_args: [ defs, '-DWITH_EXAMPLE_WOLFSSL', '-DWITH_EXAMPLE_HTTP3_PROTO_CODEC' ],
            override_options: [ 'cpp_std=c++23,c++latest' ],
            dependencies: wolfssl_deps,
            include_directories: wolfssl_includes,
        )

        executable(
            'wolfsslserver',
            wolfsslserver_sources,
            cpp_args: [ defs, '-DWITH_EXAMPLE_WOLFSSL', '-DWITH_EXAMPLE_HTTP3_PROTO_CODEC' ],
            override_options: [ 'cpp_std=c++23,c++latest' ],
            dependencies: wolfssl_deps,
            include_directories: wolfssl_includes,
        )
    endif
endif
