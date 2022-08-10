Maxwell Usage Notes
========================
Setup:
1. `brew install re2c`
2. `brew install json-c`
3. `./install`
4. clone https://github.com/phalcon/cphalcon, switch to branch `4.1.x` which is what Titan currently uses
5. In cphalcon, find and replace all instances of `final class` with `class`. The converter will break on converting these classes without doing so.
4. `./bin/zeptophp <path to cphalcon directory containing .zep files>`

Conversion Notes:
1. Ensure generated file whitespace is correct and remove extraneous "/*" comment lines
2. Remove `// EMITTER VERSION [20151126]` comment for each file
3. Ensure imports are actually used and convert any existing import to use the maxwell versions if they have already been converted.
4. Convert `zephir_typeof()` calls to `is_<type>()`. Applies to ints, strings, objects, arrays, resources.
5. Convert `zephir_isempty()` calls to `empty()`
6. Convert `zephir_isset_array($array, $index)` calls to `isset($array[$index])`
7. Convert `if (zephir_fetch_array($result, $array, $index))` calls to `if (isset($array[$index])) { $result = $array[$index]; }`. Note: The zephir fetch array sets the result variable's value if the array item is set. Pay special attention to not change behavior.
7. Convert `starts_with` calls to `ZephirFuncs::startsWith()`.
8. Convert `memstr($haystack, $needle)` calls to `strpos($haystack, $needle) !== false`
9. Converter does not bring over method param or return types, some interfaces may fail to be met because of this. Fix these on a case-by-case basis.

ZEPHIR to PHP Translator
========================

This project implements a [ZEP](http://zephir-lang.com/) to [PHP](https://www.php.net/) translator.

The initial goal of this project, was to be able to create a PHP Version of [PHALCON](https://phalconphp.com), so that I could debug and discover, in depth, the workings of PHALCON.

This project, combined with the [PHALCON Debug Extension](https://github.com/test-to-com/phalcondbg) project has allowed me to finally Xdebug PHALCON.

Build Instructions
------------------

**DISCLAIMER:** I use Ubuntu 14.04 as my development OS. I have not tried building this on any other Linux Distribution or Windows.

**NOTE:** use git clone **--recursive**, to clone the *zep-to-php* repository, as this will also bring in clones of the the other repositories (dependencies), that contain files, I use to build zeptophp.

### Linux/Unix/Mac

#### Requirements
You will need, working:

* [ZEPHIR](http://zephir-lang.com/) last version tested was 0.8.x

**NOTE:** You actually don't need ZEPHIR working, you need the _json-c_ library to be installed (the ZEPHIR Parser uses it to build it's intermediate files). You can use the install-json from the ZEPHIR Project to build this library.

This should basically work on any Linux Distribution:

```bash
./install
```

or if you want to run _tests_ agains the ZEP files included with ZEPHIR:

```bash
./install -t
```

** NOTE:** This adds a symlink _zeptophp_ to your user's bin directory _~/bin_, if it exists. Otherwise you can use _./bin/zeptophp_ to run the translator.

How to use
----------

If you have:

1. A _bin_ in the root of your user's home directory _~/bin_ , and
2. It's part of your _PATH_

then running the translator is as simple as 

```
zeptophp ...path to extension's zep files...
```

If not, then use the _zeptophp_ under the _bin_ directory from where you ran ./install.
