# Python 3.13 Release Notes Digest

A handout digest of the official **What's New in Python 3.13** release notes
(https://docs.python.org/3/whatsnew/3.13.html), distilled for a study group's
Python 3.13 unit. This file covers the major sections of the release notes;
remaining sections are listed in the follow-up issue
[Python 3.13 digest follow-ups](https://github.com/mcpmark-eval-liuhezi/python313-digest/issues).

## Highlights

- **PEP 703 — Free-threaded CPython (experimental):** CPython 3.13 has experimental
  support for running with the GIL disabled (`python3.13t`, `--disable-gil`,
  `PYTHON_GIL`/`-X gil=1`). Expect some bugs and a single-threaded performance hit;
  free-threaded builds may optionally run with the GIL enabled at runtime.
- **PEP 744 — Experimental JIT compiler:** A basic JIT is added via
  `--enable-experimental-jit` (values `no`, `yes`, `yes-off`, `interpreter`);
  translation uses.copy-and-patch with an LLVM build-time dependency. Performance
  gains are modest and expected to improve over future releases.
- **PEP 667 — Defined `locals()` semantics:** `locals()` now has defined
  mutation semantics; optimized scopes return independent snapshots and
  `FrameType.f_locals` returns a write-through proxy. `exec()`/`eval()` also
  accept `globals`/`locals` as keywords.
- **PEP 696 — Type parameters support defaults:** `typing.TypeVar`,
  `typing.ParamSpec` and `typing.TypeVarTuple` now support default values.
- **PEP 702 — `warnings.deprecated()` decorator:** New decorator to mark
  deprecations in the type system and emit `DeprecationWarning` at runtime.
- **PEP 705 / PEP 742 — `typing.ReadOnly` and `typing.TypeIs`:** `ReadOnly`
  marks a `TypedDict` item read-only for type checkers; `TypeIs` provides more
  intuitive type narrowing as an alternative to `TypeGuard`.
- **PEP 669 — Low-impact monitoring C API:** New suite of functions for
  generating monitoring events in the C API; runtime overhead ~4.5%.
- **PEP 730 / PEP 738 — Mobile platform support:** iOS is now a PEP 11 supported
  platform at tier 3 (`arm64-apple-ios`, `arm64-apple-ios-simulator`); Android
  (`aarch64-linux-android`, `x86_64-linux-android`) is tier 3; `wasm32-wasi` is
  tier 2 and `wasm32-emscripten` is no longer supported.
- **PEP 594 — Removed 19 "dead batteries":** `aifc`, `audioop`, `cgi`, `cgitb`,
  `chunk`, `crypt`, `imghdr`, `mailcap`, `msilib`, `nis`, `nntplib`, `ossaudiodev`,
  `pipes`, `sndhdr`, `spwd`, `sunau`, `telnetlib`, `uu` and `xdrlib` are removed.
- **New interactive interpreter:** A new REPL based on PyPy code (multiline
  editing, F1 help, F2 history, F3 paste mode, color prompts/tracebacks controlled
  by `PYTHON_COLORS`/`NO_COLOR`/`FORCE_COLOR`). Tracebacks are colored by default.
- **Other library highlights:** `dbm.sqlite3` is the default `dbm` backend;
  `base64.z85encode()`/`z85decode()`; `copy.replace()`; `random` CLI;
  `statistics.kde()`/`kde_random()`; `math.fma()`; `os.process_cpu_count()`;
  Linux `timerfd_*` interface; `pathlib.Path.from_uri()` and `PurePath.full_match()`;
  `typing.NoDefault`, `get_protocol_members()`, `is_protocol()`;
  `argparse` `_deprecated=` parameter; `email` headers quoting safety;
  `ssl.create_default_context()` now sets `VERIFY_X509_PARTIAL_CHAIN` and
  `VERIFY_X509_STRICT` by default.

## Notable deprecations

- `array` : the `'u'` (wchar_t) format code is deprecated at runtime (since 3.13),
  use `'w'` (Py_UCS4); removal planned for 3.16.
- `ctypes` : undocumented `SetPointerType()` deprecated (removal 3.15); `ARRAY()`
  soft-deprecated in favour of `type * length`.
- `decimal` : non-standard format specifier `'N'` deprecated (removal 3.18).
- `dis` : `HAVE_ARGUMENT` separator deprecated; check membership in `hasarg`.
- `gettext` : non-integer plural-form arguments deprecated.
- `glob` : undocumented `glob0()`/`glob1()` deprecated; use `glob(root_dir=...)`.
- `http.server` : `CGIHTTPRequestHandler` and the `--cgi` flag deprecated
  (removal 3.15).
- `mimetypes` : positional path arguments to `guess_type()` soft-deprecated;
  use `guess_file_type()`.
- `re` : positional `maxsplit`/`count`/`flags` in `split()`/`sub()`/`subn()`
  deprecated; these become keyword-only.
- `pathlib` : `PurePath.is_reserved()` deprecated (removal 3.15); use
  `os.path.isreserved()`.
- `platform` : `platform.java_ver()` deprecated (removal 3.15).
- `pydoc` : undocumented `ispackage()` deprecated.
- `sqlite3` : multiple positional args to `connect()`/constructor, keyword args
  for `create_function()`/`create_aggregate()`, and keyword callback for
  `set_authorizer()`/`set_progress_handler()`/`set_trace_callback()` deprecated
  (positional-only in 3.15).
- `sys` : `_enablelegacywindowsfsencoding()` deprecated (removal 3.16); use the
  `PYTHONLEGACYWINDOWSFSENCODING` environment variable.
- `tarfile` : undocumented `TarFile.tarfile` attribute deprecated (removal 3.16).
- `traceback` : `TracebackException.exc_type` deprecated; use `exc_type_str`.
- `typing` : functional `NamedTuple` keyword syntax deprecated (3.15); omitting
  or passing `None` for `fields_` in `NamedTuple`/`TypedDict` deprecated (3.15);
  `@typing.no_type_check_decorator` deprecated (3.15); `typing.AnyStr` deprecated
  (removed from `typing.__all__` in 3.16, removed entirely in 3.18).
- `wave` : `getmark()`/`setmark()`/`getmarkers()` deprecated (removal 3.15).
- `zipimport` : `zipimporter.load_module()` deprecated; use `exec_module()`.
- Import system: setting `__cached__` without `__spec__.cached`, or `__package__`
  without `__spec__.parent`, deprecated (3.15/3.16).
- `threading.RLock()` taking arguments deprecated (3.15).
- `types.CodeType.co_lnotab` deprecated (PEP 626; may be removed 3.15).
- `functools.reduce()` keyword `function`/`sequence` deprecated (3.14).
- `logging` : custom handler `strm` argument deprecated (removal 3.16).
- `mimetypes` : untdotted extensions deprecated (ValueError in 3.16).
- `shutil.ExecError` deprecated (3.14); `symtable.Class.get_methods` deprecated
  (3.14); `sysconfig.expand_makefile_vars()` deprecated (3.14);
  `pkgutil.find_loader()`/`get_loader()` deprecated, use `importlib.util.find_spec()`;
  `pty.master_open()`/`slave_open()` → `pty.openpty()`;
  `urllib.parse.Quoter` deprecated; `itertools` copy/deepcopy/pickle removed in 3.14;
  `multiprocessing` default start method to change on Linux/BSD/others;
  `pathlib.is_relative_to()`/`relative_to()` additional args deprecated;
  `struct.Struct.__new__`/`__init__` without format deprecated (3.20);
  `__version__`/`version`/`VERSION` attributes in many modules deprecated (3.20);
  PEP 829: `name.pth` import lines silently ignored (warnings from 3.20, must be
  utf-8-sig); `datetime.strptime("%e")` without year deprecated (3.17);
  `collections.abc.ByteString` removal scheduled for 3.17 (PEP 688);
  `encodings.normalize_encoding()` non-ascii names deprecated (3.17);
  `profile` module deprecated, use `profiling.tracing` (PEP 799, 3.17);
  `tkinter.Variable.trace_*` deprecated (3.17);
  `typing._UnionGenericAlias` and `typing.ByteString` removal scheduled for 3.17;
  bitwise inversion on booleans (`~True`/`~False`) deprecated since 3.12.

## Source

Source: https://docs.python.org/3/whatsnew/3.13.html — "What's New In Python 3.13"
(Python 3.13 release notes; Python 3.13 released October 7, 2024; editors Adam
Turner and Thomas Wouters). Read and distilled on the `python-313-digest` branch.
