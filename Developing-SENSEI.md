## Contents

- [Coding Standard](#coding-standard)
- [Monitoring CI Build](#monitoring-ci-build)
- [Adding Unit Tests to SENSEI](#adding-unit-tests-to-sensei)
- [Adding Directories and Files to SENSEI](#adding-directories-and-files-to-sensei) 

## Coding Standard

- Everyone should read Seeley’s ‘How Not to Write Fortran in Any Language’: 
    - http://queue.acm.org/detail.cfm?id=1039535 
- NO TABS! Indent with spaces only as different editors view tabs as different number of spaces, i.e., what looks good on your computer may look terrible on another.
- No trailing white space, it adds clutter to diffs! Vi & Emacs can be set up to remove it for you automatically.
- Indent 2 spaces for readability.  It keeps lines from getting too long and yet still provides visual order.  You can set Vi and Emacs to automatically do this for you, see the examples on the wiki.
- Use an 80 column line limit (for readability).  
    - See the following: http://www.emacswiki.org/emacs/EightyColumnRule
    - NB: If you push a commit that has lines longer than 80 characters or that have trailing whitespace, the commit will be rejected and you will have to rewrite your history to get your commits pushed. This is a huge pain and you will regret it!
    - To avoid committing files with lines over 80 characters, save the following code to a file named 'pre-commit' and put it in 'SENSEI/.git/hooks/':

```js
#!/bin/sh
# Pre-commit hook for git which removes trailing whitespace, converts tabs to spaces, and enforces a max line length.
#Based on https://gist.github.com/torsten/3706787

if git-rev-parse --verify HEAD >/dev/null 2>&1 ; then
    against=HEAD
else
# Initial commit: diff against an empty tree object
    against=9e517e78330d20d2064fde149c4f0fdddf2fdf1d
fi

files_with_whitespace=`git diff-index --check --cached $against | # Find all changed files
sed '/^[+-]/d' | # Remove lines which start with + or -
sed -E 's/:[0-9]+:.*//' | # Remove end of lines which contains numbers, etc.
sed '/Generated/d' | # Ignore generated files
sed '/Libraries/d' | # Ignore libraries
sed '/.\(f90|f95|f03|f08|F90|F95|F03|F08\)\$/!d' | # Process Fortran files
uniq` # Remove duplicate files

# Change field separator to newline so that for correctly iterates over lines
IFS=$'\n'

# Find files with trailing whitespace
for FILE in $files_with_whitespace ; do
    echo "Fixing whitespace in $FILE" >&2
# Replace tabs with four spaces
    sed -i "" $'s/\t/ /g' "$FILE"
# Strip trailing whitespace
    sed -i '' -E 's/[[:space:]]*$//' "$FILE"
    git add "$FILE"
done

# Detect too long lines in all Fortran files:
changed_source_files=`git diff-index --cached $against --numstat | cut -f3 | egrep '\.f90|.f95|.f03|.f08|.F90|.F95|.F03|.F08$'`

if [[ -n "$changed_source_files" ]]; then
    found_offenses=''

    for file in $changed_source_files ; do
	too_long=`git diff-index --cached -p $against -- "$file" | egrep '^\+.{81,}' | sed -E 's/^\+//'`
	if [[ -n "$too_long" ]]; then
	    found_offenses=YES
	    printf "\n$file:\n%s\n" "$too_long" >&2
	fi
    done

    if [[ -n $found_offenses ]]; then
	echo -e "\nAborting commit because you added lines longer than 80 chars." >&2
	exit 1
    fi
fi
```

- All code should be in modules except for included functions in the function library.  This way, all subroutines or functions in the module will have automatically defined interfaces.
- Name all files with an `*.f90` extension to indicate free format files
    - The only exception to this rule is if you need the preprocessor to run then they should be named `*.F90`
    - See the last post of this thread: https://software.intel.com/en-us/forums/topic/328744 
- File names must match module/function names to make searching easier
- Always use `intent` attributes to help others understand the intended data flow
- Use `use, only` statements to aid in tracking data movement
- Declare `implicit none` once per file.  Implicit variables are confusing, and declaring `implicit none` in the module header covers all the contained routines to follow.
- Avoid using the `save` statement.  It can easily cause you to not be thread safe.
- Use self documenting routine/variable names to avoid confusion and useless comments.  For example, it is fairly obvious that a routine named `compute_residual_norm` computes a residual norm, and that if it contains a variable called `l2_norm` there’s no need for a comment that ‘l2_norm is the l2 norm of the residual’
- Use a space to separate keywords, i.e., `end do` instead of `eddo` as this is preferred in modern Fortran
- Use modern Fortran constructs such as `[   ]` for array constructs instead of `(/   /)` as this is preferred in modern Fortran
- Use modern Fortran relational operators such as `<=` rather than `.le.` as this is preferred in modern Fortran
    - Note: logicals require `.eqv.` and `.neqv.`
- Remember to push to and pull from the repository often as this keeps you from getting out of sync with everyone else.  Get into the habit of doing a `git pull` every morning to catch up on what has happened.
- Use commit message that explain why you’re committing code, not simply stating that you are committing code.  Think about the difference between a message that says ‘to enable super-awesome feature by adding a new module’ and one that simply states ‘to add a file.’  Which one gives you more meaningful information at a glance?
- Align like parts of code, such as variable declarations, variable assignment statements (“=” signs), multiple function/subroutine calls, etc., for readability & ease of debugging.
Don't do this:

```js
var1 = func_call( arg1, arg2 )
var23 = func_call( input1, input2 )
```

Instead, do this:

```js
var1  = func_call( arg1,   arg2   )
var23 = func_call( input1, input2 )
```

- Subroutine comment headers begin in Column 1
- Subroutine name/input/output declaration begin in Column 3
- Subroutine comment headers should have the following form, or at minimum the !==80 box to mark the beginning of a new subroutine
- Use `continue` statements after variable declarations
- For the ending of a subroutine/module/function give the subroutine/module/function name you are ending

```js
!=============================== subroutine_name =============================80
!>
!! Comments
!<
!=============================================================================80
  subroutine subroutine_name ( input, output )

    ( Declaration Section )
    ...

    continue

    ( Execution Section )
    ...

  end subroutine subroutine_name
```

## Monitoring CI Build

SENSEI uses Jenkins ( a fork of Hudson ) as its Continuous Integration Server.
To view the current status of SENSEI, open a web browser and go to:

columbia.aoe.vt.edu:8080

NB: You must ask Steve for access to Columbia, and it will only be from within the CFD Lab!

Currently, Jenkins clones SENSEI and does a complete clean build from scratch.
It then builds the documentation and places a time stamped copy of the reference manual into
roycfd.
As long as these tasks complete succesfully, it runs the unit tests.

In the future, further tests will be added, such as order of accuracy tests and regression tests.

## Adding Unit Tests to SENSEI
TODO: write this section


## Adding Directories and Files to SENSEI
### New CMake System
The CMake system makes it much easier to add files and folders with the use of macros.

The crux of the CMake system are `CMakeLists.txt` and there should be one in each directory.
The main one appears in the top level of the SENSEI directory.
This sets up the settings and macros used for the rest of the build.
It also creates the targets (executables) for the build project.
Then it kicks off the build process by saying what directories it need to go into with the `add_subdirectory( <dir> )` command.
Then each subdirectory also needs a `CMakeLists.txt` file.
In these subdirectory files there are 3 main commands/macros that will be used:
* `add_subdirectory( <dir> )`: descend into a child folder
* `add_libsources( <filelist> )`: macro that adds files into the sensei library, can also be a list of files (most common)
* `add_exesources( <exename> <filelist> )`: macro that adds files as a source for the given executable (not very common)

The order of the files is unimportant as long as all of the files needed are listed, as CMake will read the files and determine which files depend on which and compile them in order.
It is also important that if a folder contains subfolders but no files itself it must still issue the command `add_libsources()` to pass this list on to its parent folder.

If you have added additional unit tests you can add them to their respective `CMakeLists.txt` with the macro `add_test_sources( <testname> <filelist> )`.
This adds the list of files to the list of files to be preprocessed into a test with the given name.
Then you can add the test target to make the test executable with the command `add_test_target( <testname> unit ${<testname>_sources} )`

Now you can rebuild your code with only the make command in your build dir because it will rerun cmake for you.  Make sure that you commit the new and modified `CMakeLists.txt` files so that other people can build your new stuff.

The macros are located in `/path_to_SENSEI/cmake/macros` if you are interested but these should not need to be modified.

Take a look through the source code to figure out how to do this if you get confused but it is much easier than autotools.

### Old Automake System
Since SENSEI uses Autotools to build, you need to be familiar with the configure.ac file and Makefile.am files.

The configure.ac file is at the top level of the SENSEI directory and tells Autotools what directories exist that contain a Makefile.am file.  Take a look at SENSEI's configure.ac file to see how this works.

A Makefile.am file contains information on what source files need to be built and if they are going to be turned into a library or a program.  Once again, take some time to look at SENSEI's Makefile.am files.

When you're ready to add a new directory, create a Makefile.am for it.  For the most part, it's safe (and probably easiest) to copy a Makefile.am file from one directory to a new directory and modify it to suit your needs.  Then, edit the Makefile.am which is at the same level as your directory and add the directory name to the SUBDIRS line so that Make can properly make its build tree.  Finally, go to the configure.ac file and add the correct path to your new directory.

Now you can reconfigure and rebuild your code.  Make sure that you commit the new and modified Makefile.am's and the modified configure.ac so that other people can build your new stuff.

Remember, take a look through the source code to figure out how to do this if you get confused, you should be able to figure it out with a little trial and error.