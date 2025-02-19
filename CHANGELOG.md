# Changelog

## 2021-09-25 libunftp 0.18.2

_tag: libunftp-0.18.2_

- [#386](https://github.com/bolcom/libunftp/issues/386) Implemented graceful shutdown through the Server.shutdown_indicator method.
- Upgraded to rustls v0.20.0
- Upgraded other minor dependency versions
- Testing improvements

## 2021-09-25 libunftp 0.18.1

_tag: libunftp-0.18.1_

- Replace futures with futures-util and use Tokio's mpsc channels
- [#371](https://github.com/bolcom/libunftp/pull/371), [#377](https://github.com/bolcom/libunftp/pull/377) Fixed an 
  issue where rclone reported all file sizes as 0. The fix was to include the number of links to a file in the output 
  to the client.
- Fixed a unit tests
- Upgraded dependencies
- [#379](https://github.com/bolcom/libunftp/pull/379) Fixed an issue where the `Permissions` struct could not be used 
  even though it was public.
- [#380](https://github.com/bolcom/libunftp/pull/380), [#381](https://github.com/bolcom/libunftp/pull/381) Return STAT 
  response as a multi-line in accordance with RFC 959 in order to fix an issue with the Cyberduck client.

## 2021-07-13 Release of all crates

### libunftp 0.18.0

_tag: libunftp-0.18.0_

- [#356](https://github.com/bolcom/libunftp/pull/356) Authenticators can now also take the connection source IP, and 
  the client certificate chain into account in addition to the password when performing authentication.
- [#356](https://github.com/bolcom/libunftp/pull/356/files) **Breaking**: The `Authenticator::authenticate` method now 
  takes a `Credentials` structure reference instead of a `str` reference for the second parameter.
- [#373](https://github.com/bolcom/libunftp/pull/373) **Breaking**: The `StorageBackend` methods were all changed to 
  take a reference of a user (`&User`) instead of an optional reference to it (`&Option<User>`).
- Dependency upgrades and cleanups
- Fixed an issue where OPTS UTF8 returned the wrong FTP reply code
- [#361](https://github.com/bolcom/libunftp/issues/361) Don't allow consecutive PASS commands
- Added support for TLS client certificates
- [#358](https://github.com/bolcom/libunftp/pull/358/files) Added the ability for authenticators to do password-less 
  authentication when the user presents a valid client certificate. See the `Authenticator.cert_auth_sufficient` method. 
  
### unftp-auth-jsonfile v0.2.0

_tag: unftp-auth-jsonfile-0.2.0_

- Added support for per-user IP allow lists
- [#369](https://github.com/bolcom/libunftp/issues/369) Added support for per-user client certificate CN matching
- [#355](https://github.com/bolcom/libunftp/pull/355) Created a new Docker image that generates PBKDF2 keys for the
  authenticator.

### unftp-auth-* v0.2.0

- compiled unftp-auth-pam against libunftp v0.18.0
- compiled unftp-auth-rest against libunftp v0.18.0

### unftp-sbe-* v0.2.0

- compiled unftp-sbe-fs against libunftp v0.18.0
- compiled unftp-sbe-gcs against libunftp v0.18.0

## 2021-05-22 unftp-sbe-gcs v0.1.1

_tag: unftp-sbe-gcs-0.1.1_

- Added an extension trait that adds a `Server::with_gcs` constructor.
- Added support for the `SITE MD5` FTP command. Also see [Server::sitemd5](https://docs.rs/libunftp/0.17.4/libunftp/struct.Server.html#method.sitemd5) in libunftp. 

## 2021-05-22 libunftp 0.17.4

_tag: libunftp-0.17.4_

- Added a new `SITE MD5` command that allows FTP clients to obtain the MD5 checksum of a remote file. The feature is 
  disabled for anonymous users by default. See [Server::sitemd5](https://docs.rs/libunftp/0.17.4/libunftp/struct.Server.html#method.sitemd5).

## 2021-05-02 libunftp v0.17.3

_tag: libunftp-0.17.3_

- Added Mutual TLS support.

## 2021-05-01 unftp-auth-jsonfile v0.1.1

_tag: unftp-auth-jsonfile-0.1.1_

- Added support for PBKDF2 encoded passwords

## 2021-04-25 libunftp v0.17.2

_tag: libunftp-0.17.2_

- Fixed output formatting of the FEAT command.
- Fixed the SIZE command that wrongly took the REST restart position into account and also caused number overflows 
  because of that.
- Removed panics that could happen when failing to load the TLS certificate or key, these errors are now propagated via 
  the `Server::listen` method.
- Implemented TLS session resumption with server side session IDs.
- Implemented TLS session resumption with [tickets](https://tools.ietf.org/html/rfc5077).  
- Added the `Server::ftps_tls_flags` method to allow switching TLS features on or off.

## 2021-04-18 libunftp v0.17.1

_tag: libunftp-0.17.1_

Changes in this release:

- [#327](https://github.com/bolcom/libunftp/issues/327) Allow PROT and PBSZ without requiring authentication.
- [#330](https://github.com/bolcom/libunftp/pull/330) Load TLS certificates only once at startup instead of on every connect.

## 2021-03-26 Newly splitted auth and storage back-ends

- Released [unftp-sbe-gcs](https://crates.io/crates/unftp-sbe-gcs)
- Released [unftp-sbe-fs](https://crates.io/crates/unftp-sbe-fs)
- Released [unftp-auth-jsonfile](https://crates.io/crates/unftp-auth-jsonfile)
- Released [unftp-auth-pam](https://crates.io/crates/unftp-auth-pam)
- Released [unftp-auth-rest](https://crates.io/crates/unftp-auth-rest)

## 2021-03-26 libunftp v0.17.0

_tag: libunftp-0.17.0_


The main focus of this release was the removal of contained authentication and storage back-ends from the libunftp crate 
and into their own crates. As you can imagine this brings about breaking changes.

Source code for these crates can still be found in this repository under the `crates` directory.

Breaking Changes:

- Split the GCS back-end into crate [unftp-sbe-gcs](https://crates.io/crates/unftp-sbe-gcs)
- Split the Filesystem back-end into crate [unftp-sbe-fs](https://crates.io/crates/unftp-sbe-fs)
- Split the JSON file authenticator into crate [unftp-auth-jsonfile](https://crates.io/crates/unftp-auth-jsonfile)
- Split the PAM authenticator into crate [unftp-auth-pam](https://crates.io/crates/unftp-auth-pam)
- Split the REST authenticator into crate [unftp-auth-rest](https://crates.io/crates/unftp-auth-rest)
- Changed some public API names to adhere to Rust naming conventions:
  - PAMAuthenticator became PamAuthenticator
  - PassiveHost::IP became PassiveHost::Ip
  - PassiveHost::DNS became PassiveHost::Dns
  - RestError::HTTPStatusError became RestError::HttpStatusError
  - RestError::JSONDeserializationError became RestError::JsonDeserializationError
  - RestError::JSONSerializationError became RestError::JsonSerializationError
- The `Server::with_fs` method moved into the `ServerExt` extension trait of `unftp-sbe-fs`
- The `Server::with_fs_and_auth` method was removed. Use the `Server::with_authenticator` method instead.


Other changes:

- Upgraded outdated dependencies
