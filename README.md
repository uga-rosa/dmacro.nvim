# dmacro.nvim

> [!CAUTION]
> You need to use the nightly build of neovim (3-31-2024 or later).<br>
> <s>This plugin depends on the pull request https://github.com/neovim/neovim/pull/28098 .
> You need to build neovim, which is available at https://github.com/zeertzjq/neovim/tree/on-key-typed . </s>

## Related works

- [dmacro.el](https://github.com/emacs-jp/dmacro)
- [Dynamic Macro for Visual Studio Code](https://github.com/tshino/vscode-dynamic-macro)

## Example



https://github.com/tani/dmacro.nvim/assets/5019902/e008d47d-9094-447f-abff-7aa7e4b18259



This plugin dynamically defines a macro.
**You do not need to make any markers for the macro.**
To define a macro, this plugin detects the reputation as follows:

- If you enter the same key sequence twice (e.g. `abcabc`), this plugin will define a macro with the key sequence (e.g. `abc`).
  ```mermaid
  graph LR
    start(( )) --> a1((a))
    subgraph 1st
    a1 --> b1((b))
    b1 --> c1((c))
    end
    subgraph 2nd
    c1 --> a2((a))
    a2 --> b2((b))
    b2 --> c2((c))
    end
    subgraph 3rd
    c2 --> dmacro(dmacro)
    dmacro -.- a3((a))
    subgraph MacroExecution
    a3 -.- b3((b))
    b3 -.- c3((c))
    end
    end
    c3 --> quit(( ))
  ```
  

- If you type the subsequence (e.g. `a`) of the previous key sequence (e.g. `abc`), this plugin will define a macro with the rest of the previous sequence (e.g. `bc`). After expanding the macro, the whole sequence (e.g. `abc`) will be the macro.
  ```mermaid
  graph LR
    start(( )) --> a1((a))
    subgraph 1st
    a1 --> b1((b))
    b1 --> c1((c))
    end
    subgraph 2nd
    c1 --> a2((a))
    a2 --> dmacro1(dmacro)
    dmacro1 -.- b2((b))
    subgraph MacroExecution_1
    b2 -.- c2((c))
    end
    end
    subgraph 3rd
    c2 --> dmacro2(dmacro)
    dmacro2 -.- a3((a))
    subgraph MacroExecution_2
    a3 -.- b3((b))
    b3 -.- c3((c))
    end
    end
    c3 --> quit(( ))
  ```

## Usage

Before loading buffers, call the `setup()` function.

```lua
require('dmacro').setup({
    dmacro_key = '<C-t>' --  you need to set the dmacro_key
})
```

## Licence

This software is released under the MIT licence.
