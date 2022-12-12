# Advent of Code 2022 in Common Lisp

I thought going through the Advent of Code 2022 in Common Lisp would be a fun way to give the language a shot. 

# Computer Setup

Setting up a M1 Macbook Air

1. install Steel Bank Common Lisp from homebrew
```bash
brew install sbcl
```
2. install quicklisp followign instructions [here](https://www.quicklisp.org/beta/)

```bash
$ curl -O https://beta.quicklisp.org/quicklisp.lisp
$ sbcl --load quicklisp.lisp
$ sbcl --load quicklisp.lisp
* (quicklisp-quickstart:install)
* (ql:add-to-init-file)

```

3. install `common-lisp-jupyter`

Following instructions 
Building `common-lisp-jupyter` needs ZMQ headers but `clang` will not find the zmq header without this symlink
```
$ brew install zeromq
$ sudo ln -s /opt/homebrew/include /usr/local/include
```

Installation instruction recommend "Unless there is are specific requirements that dictate the type of kernel installation a user “non-image” kernel is recomended"

```
* (ql:quickload :common-lisp-jupyter)
* (cl-jupyter:install)
```

Jupyter kernel fails to start with the message in the log

```
fatal error encountered in SBCL pid 43529 pthread 0x102a54580:
Can't find sbcl.core
```

If we look at the installed kernel spec

```
(base) colmryan@Colms-MacBook-Air ~ % cat /Users/colmryan/Library/Jupyter/kernels/common-lisp/kernel.json
{
  "argv": [
    "/opt/homebrew/Cellar/sbcl/2.2.11/libexec/bin/sbcl",
    "--eval",
    "(ql:quickload :common-lisp-jupyter)",
    "--eval",
    "(jupyter:run-kernel 'jupyter/common-lisp:kernel)",
    "{connection_file}"
  ],
  "display_name": "Common Lisp",
  "language": "common-lisp",
  "interrupt_mode": "message",
  "metadata": {
    "debugger": true
  }
}          
```

we can see that trying to pass the full path does not work

```
(base) colmryan@Colms-MacBook-Air ~ % /opt/homebrew/Cellar/sbcl/2.2.11/libexec/bin/sbcl
fatal error encountered in SBCL pid 43572 pthread 0x10292c580:
Can't find sbcl.core
```

Maybe it is pointing to `sbcl.core` with a relative path? In any case we can fix the kernelspec by using the linked file in `/opt/homebrew/bin` instead.

```
(base) colmryan@Colms-MacBook-Air ~ % which sbcl
/opt/homebrew/bin/sbcl
```

# Lisp Learning Links
1. [Learn Common Lisp](https://lisp-lang.org/learn/) very limited and basic introduction. 
2. [The Common Lisp Cookbook]https://lispcookbook.github.io/cl-cookbook/ heaps of content in a detailed cookbook. 
3. [Practical Common Lisp](https://gigamonkeys.com/book/)