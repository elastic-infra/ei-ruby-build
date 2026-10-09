# ei-ruby-build
Install Ruby versions for Amazon Linux 2023

## Builds with known issues

Some prebuilt binaries have OpenSSL-related issues. Their definitions add a warning step
(the same mechanism as upstream ruby-build's `warn_eol`), so `rbenv install` prints a `WARNING`
to stderr. The installation itself still succeeds.

| Version | Architecture | Warning step | Issue |
|---|---|---|---|
| 3.3.7 | x86_64, aarch64 | `warn_bundled_openssl` | Bundles OpenSSL 3.0.x. Libraries linked against the system OpenSSL (e.g. libcurl) fail to load in the same process. |
| 3.4.9 | x86_64 | `warn_bundled_openssl` | Same as above. |
| 3.3.6 | x86_64, aarch64 | `warn_requires_newer_openssl` | Built against OpenSSL 3.4 or later. `require "openssl"` fails on systems with an older OpenSSL. |
| 3.4.2 | x86_64, aarch64 | `warn_requires_newer_openssl` | Same as above. |
| 2.6.6, 2.7.4, 3.0.3 | x86_64, aarch64 | `warn_eol` | Ruby itself is EOL. These bundle OpenSSL 1.1.1 because Ruby < 3.1 does not support OpenSSL 3. It uses a different SONAME (`libssl.so.1.1`), so it does not conflict with the system OpenSSL 3, but it no longer receives security updates. |

Other versions link against the system OpenSSL and require only `OPENSSL_3.0.0` symbols.

When a version listed above is rebuilt, remove its warning step from the definition and the row
from this table in the same pull request.
