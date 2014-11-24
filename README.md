# `configure`: functions module for shellfire

This module provides a simple framework for managing configuration of a [`shellfire`] application. It is deliberately separate to the [`core`] module, which provides automatic loading of configuration for the command line at run time. Instead, it lets users define configuration in both files and directories of snippets, which can be overridden, defaulted, validated and restored.

An example user is the [`swaddle`] packaging tool.

## Overview

Configuration is organised into namespaces which are _private_ to your program; specifying the same namespace in different programs will give different results.

To get going, first we need to register some settings:-

```bash
configure_register Value Boolean swaddle_web use_index_name_in_directory_links yes
configure_register Array Any swaddle_web pandoc_options
configure_register Value NotEmpty swaddle_web index_name 'index.html'
```

Note that there's no need to register the namespace; this happens automatically. The framework supports `Value`s and `Array`s. A `Value` may have a default (for example, `index_name` above defaults to `index.html`). Arrays can not have defaults.



## Importing

To import this module, add a git submodule to your repository. From the root of your git repository in the terminal, type:-

```bash
mkdir -p lib/shellfire
cd lib/shellfire
git submodule add "https://github.com/shellfire-dev/configure.git"
cd -
git submodule init --update
```

You may need to change the url `https://github.com/shellfire-dev/configure.git"` above if using a fork.

You will also need to add paths - include the module [`paths.d`].


## Namespace `configure`

### To use in code

If calling from another [`shellfire`] module, add to your shell code the line

```bash
core_usesIn configure
```

in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn configure
	…
}
```

### Functions


#### `configure_register`

|Parameter|Value|Optional|
|---------|-----|--------|
|`namespace`|Namespace used to differentiate configuration setting from any other|_No_|
|`configurationSettingName`|Name of configuration setting|_No_|

#### `configure_getConfigurationSetting`

|Parameter|Value|Optional|
|---------|-----|--------|
|`namespace`|Namespace used to differentiate configuration setting from any other|_No_|
|`configurationSettingName`|Name of configuration setting|_No_|


Use this function to obtain the value of a configuration setting. For example, to find the `repositoryName`:-

```bash
…
local repositoryName="$(configure_getConfigurationSetting 'swaddle_github' 'repository_name')"
…
```

By convention, namespaces are organised in a hierarchy using underscores, `_`, and names use lower case and underscores `_`. This makes them easy to set in programs.

[shellfire]: https://github.com/shellfire-dev "shellfire homepage"
[core]: https://github.com/shellfire-dev/core "shellfire core module homepage"
[swaddle]: https://github.com/raphaelcohn/swaddle "Swaddle homepage"
[paths.d]: https://github.com/shellfire-dev/paths.d "paths.d shellfire module homepage"
