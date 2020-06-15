# Command Line Interface (CLI)

```bash
dprint <SUBCOMMAND> [OPTIONS] [--] [files]...
```

## Installing and Setup/Initialization

See [Install](install) and [Setup](setup).

## Help

The information outlined here will only be for the latest version, so `dprint help` or `dprint --help` will output information on how to use the CLI.

## Formatting Files

After [setting up a configuration file](setup), run the `fmt` command:

```bash
dprint fmt
```

Or to override the configuration file's `includes` and `excludes`, you may specify the file paths to format or not format here:

```bash
dprint fmt **/*.js --excludes **/data
```

## Checking What Files Aren't Formatted

Instead of formatting files, you can get a report of any files that aren't formatted by running:

```bash
dprint check
```

Example output:

![Example of dprint check output.](/images/check-example.png "Example of dprint check output.")

## Using a Custom Config File Path or URL

Instead of the default path of *dprint.config.json* or *config/dprint.config.json*, you can specify a path to a configuration file via the `--config` or `-c` flag.

```bash
dprint fmt --config path/to/my/config.json
```

## Diagnostic Commands and Flags

### Outputting file paths

Sometimes you may not be sure what files dprint is picking up and formatting. To check, use the `output-file-paths` subcommand to see all the resolved file paths for the current plugins based on the CLI arguments and configuration.

```bash
dprint output-file-paths
```

Example output:

```bash
C:\dev\my-project\scripts\build-homepage.js
C:\dev\my-project\scripts\build-schemas.js
C:\dev\my-project\website\playground\config-overrides.js
C:\dev\my-project\website\playground\src\components\ExternalLink.tsx
C:\dev\my-project\website\playground\src\components\index.ts
C:\dev\my-project\website\playground\src\components\Spinner.tsx
...etc...
```

### Outputting resolved configuration

When diagnosing configuration issues it might be useful to find out what the internal lower level configuration used by the plugins is. To see that, use the following command:

```bash
dprint output-resolved-config
```

Example output:

```text
typescript: {
  "arguments.preferHanging": true,
  "arguments.preferSingleLine": false,
  "arguments.trailingCommas": "onlyMultiLine",
  "arrayExpression.preferHanging": true,
  "arrayExpression.preferSingleLine": false,
  "arrayExpression.trailingCommas": "onlyMultiLine",
  "arrayPattern.preferHanging": true,
  // ...etc...
  "whileStatement.singleBodyPosition": "nextLine",
  "whileStatement.spaceAfterWhileKeyword": true,
  "whileStatement.useBraces": "preferNone"
}
json: {
  "commentLine.forceSpaceAfterSlashes": true,
  "indentWidth": 2,
  "lineWidth": 160,
  "newLineKind": "lf",
  "useTabs": false
}
```

### Verbose

It is sometimes useful to see what's going on under the hood. For those cases, run dprint with the `--verbose` flag.

For example:

```bash
dprint check --verbose
```

Example output:

```text
[VERBOSE]: Getting cache directory.
[VERBOSE]: Reading file: C:\Users\user\AppData\Local\Dprint\Dprint\cache\cache-manifest.json
[VERBOSE]: Checking path exists: ./dprint.config.json
[VERBOSE]: Reading file: V:\dev\my-project\dprint.config.json
[VERBOSE]: Globbing: ["**/*.{ts,tsx,js,jsx,json}", "!website/playground/build", "!scripts/build-website", "!**/dist", "!**/target", "!**/wasm", "!**/*-lock.json", "!**/node_modules"]
[VERBOSE]: Finished globbing in 12ms
[VERBOSE]: Reading file: C:\Users\user\AppData\Local\Dprint\Dprint\cache\typescript-0.19.2.compiled_wasm
[VERBOSE]: Reading file: C:\Users\user\AppData\Local\Dprint\Dprint\cache\json-0.4.1.compiled_wasm
[VERBOSE]: Creating instance of dprint-plugin-typescript
[VERBOSE]: Creating instance of dprint-plugin-jsonc
[VERBOSE]: Created instance of dprint-plugin-jsonc in 9ms
[VERBOSE]: Reading file: V:\dev\my-project\website\playground\tsconfig.json
[VERBOSE]: Reading file: V:\dev\my-project\website\assets\schemas\v0.json
[VERBOSE]: Reading file: V:\dev\my-project\dprint.config.json
[VERBOSE]: Formatted file: V:\dev\my-project\website\assets\schemas\v0.json in 2ms
[VERBOSE]: Formatted file: V:\dev\my-project\dprint.config.json in 0ms
[VERBOSE]: Formatted file: V:\dev\my-project\website\playground\tsconfig.json in 0ms
[VERBOSE]: Created instance of dprint-plugin-typescript in 35ms
[VERBOSE]: Reading file: V:\dev\my-project\website\playground\public\formatter.worker.js
[VERBOSE]: Reading file: V:\dev\my-project\website\assets\formatter\v1.js
[VERBOSE]: Reading file: V:\dev\my-project\website\playground\src\plugins\getPluginInfo.ts
[VERBOSE]: Formatted file: V:\dev\my-project\website\playground\public\formatter.worker.js in 22ms
[VERBOSE]: Formatted file: V:\dev\my-project\website\assets\formatter\v1.js in 6ms
[VERBOSE]: Formatted file: V:\dev\my-project\website\playground\src\plugins\getPluginInfo.ts in 4ms
...etc....
```

This may be useful for finding files that are taking a long time to format and maybe should be excluded from formatting.

### Clearing Cache

Internally, a cache is used to avoid re-downloading files. It may be useful in some scenarios to clear this cache by running:

```bash
dprint clear-cache
```