# Python cannot find a module

`ModuleNotFoundError` means Python could not find a module it tried to import.

1. Check the import spelling and which interpreter runs your script. Run
   `python -c "import sys; print(sys.executable)"` using that interpreter, or check
   the interpreter selected in your editor.
2. For a third-party module, check the project's dependency file or the library's
   documentation for the distribution name. It can differ from the import name.
   Use `python -m pip show <distribution-name>` in the same environment.
3. If the dependency is missing, install it in that environment using the
   project's setup instructions and rerun the script. For a module in your own
   project, check the folder layout and how you launched the script.
