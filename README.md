# skeleton directory for Linux user homes

the command line programs `useradd(8)` and `homectl(1)` with their argument `--skel`

---

The steps below do not work. I leave them here for the future.

1. archive repository

   compared to a checkout, this method gets rid of the directory `.git`, although it still leaves behind the `.gitmodules` and the unnecessary `.gitignore` files

   ```Shell
   git archive --format=tar --output=skel.tar -- HEAD
   ```

   ```Shell
   git submodule foreach --quiet --recursive 'echo ${displaypath}' | tar --append --file=skel.tar --verbatim-files-from --files-from=-
   ```

   Must pass `--quiet`, because `git submodule foreach` sends its diagnostic "Entering ..." messages to stdout.

   problem: submodules do not have a proper `.git` directory; instead, `.git` is a file that points to the `.git` directory of the top repository.
   I could do the opposite of `git submodule absorbgitdirs` by hand, but that seems fragile and too involved.

1. check out

   either:

   - clone

     ```Shell
     git clone -- github.com:kalrish/skel.git
     ```

     but authenticity must be proven!

   - alternatively bundle

     ```Shell
     git bundle create -- skel.bundle
     ```

     ```Shell
     git clone -- skel.bundle
     ```

1. Create a temporary directory.

   This does not have to be persisted. With `mktemp(1)`:

   ```Shell
   skel="$(mktemp --directory)"
   ```

2. d
