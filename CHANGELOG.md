# Changelog

## Unreleased

* ...


## 2.0.2      2021-04-02

* Update to libpg_query 13-2.0.3
  - Normalize: Fix handling of two subsequent DefElem elements
* Parser CFLAGS: Build with -std=gnu99 to ensure CentOS compatibility


## 2.0.1      2021-03-30

* Update to libpg_query 13-2.0.2
  - Fix ARM builds: Avoid dependency on cpuid.h header
  - Simplify deparser of TableLikeClause
  - Fix asprintf warnings by ensuring _GNU_SOURCE is set early enough
* Add FingerprintToUInt64 method for callers that prefer to handle uint64s
  - This prevents a caller from having to do a hex string to uint64 conversion
    by simply returning the uint64 version of the fingerprint instead, which
    is already always provided by libpg_query.
* Add HashXXH3_64 helper method for generating XXH3 64-bit hash values
  - This can be useful when trying to fit other values into a data structure
    sized for the fingerprint, such as when encountering a parsing error
    during fingerprinting, and wanting to encode that fact uniquely into
    the fingerprint value.


## 2.0.0      2021-03-18

* Update libpg_query to 13-2.0.0
* Switch to use Protobuf generated nodes instead of custom logic
  - WARNING: This is breaking API change!
  - This is a breaking change in the API, but necessary in order to
    significantly improve the performance of parsing a query into Go structs,
    as well as allowing future bi-directional passing of parse trees between
    Go and C, such as for a future addition of a deparser.
* Rename pg_query.FastFingerprint to pg_query.Fingerprint
  - WARNING: This is breaking API change!
  - We no longer have direct support for running the fingerprint in the Go
    library, as its unnecessarily complex to support, and we can instead
    rely on the C fingerprinting method.
* Add support for deparsing parse trees back into a SQL statement
  - Call the new `pg_query.Deparse` method to get back the SQL text for
    a particular parse tree structure
  - This relies on the new deparser in libpg_query to transform a given parse
    tree back into a SQL statement. Note that this is currently considered
    experimental, and should not be used on unsanitized input, due to the risk
    of crashes in the C code with unexpected conditions.
* Update import path to `github.com/pganalyze/pg_query_go`


## 1.0.2      2021-02-18

* Update libpg_query to 10-1.0.5
  - This resolves memory leak problems, adds PPC architecture support,
    and refreshes the Postgres minor version to 10.16.


## 1.0.1      2020-11-07

* Update libpg_query to 10-1.0.3
  - This fixes ARM builds, and refreshes the Postgres patch version. This
    commit does not change the Postgres major version yet (still at PG 10).


## 1.0.0      2019-01-11

* Initial release with a tagged version
  - Note that 1.X releases will reflect Postgres 10 parser based releases from
    now on. When the Postgres 11 parser is released, that will be a new major
    version.
