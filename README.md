# q-mode

## Features

q-mode is a major mode for editing q (the language written by [Kx Systems](http://www.kx.com)) in [Emacs](https://www.gnu.org/software/emacs/).

Some of its major features include:
- interaction with inferior q instance using

There is another q-mode on GitHub that is more fully-featured
[psaris/q-mode]. I think I may have branched from that a long time ago
and changed the implementation substantially.

I only describe the minimal set of features I've used since then. I
lot of the original features are present but unimplemented.

- syntax highlighting
- q documentation on info
- remote operation with gnuclient 
  

## Installation

To load `q-mode` on-demand, instead of at startup, add this to your
initialization file

```elisp
(autoload 'q-mode "q-mode")
```
The add the following to your initialization file to open all .k
and .q files with q-mode as major mode automatically:

```elisp
(add-to-list 'auto-mode-alist '("\\.[kq]\\'" . q-mode))
```

If you load ess-mode, it will attempt to associate the .q extension
with S-mode.  To stop this, add the following lines to your
initialization file.

```elisp
(defun remove-ess-q-extn ()
 (when (assoc "\\.[qsS]\\'" auto-mode-alist)
  (setq auto-mode-alist
        (remassoc "\\.[qsS]\\'" auto-mode-alist))))
(add-hook 'ess-mode-hook 'remove-ess-q-extn)
(add-hook 'inferior-ess-mode-hook 'remove-ess-q-extn)
```

## Usage

Q mode defined in `q-mode.el':
Major mode for editing Q scripts.

Provides the `q-run-script' (C-c C-l) command to run the interpreter
on the script in the current buffer. It will be verified that the buffer has a
file associated with it, and you will be prompted to save edited buffers when
invoking this command. Special commands to quickly locate the main script and
the input line of the Q eval buffer, and to visit the source lines shown in
compiler/debugger messages are provided as well (see `q-eval-mode').

These operations can be selected from the Q mode menu (accessible from
the menu bar), which also provides commands for reading the online
help and customizing the Q/Q-Eval mode setup.

Command list:

key             binding
---             -------

C-c		Prefix Command
TAB		q-indent-line
ESC		Prefix Command
( .. )		q-electric-delim
;		q-electric-delim
=		q-electric-delim
[		q-electric-delim
]		q-electric-delim
|		q-electric-delim

C-M-i		q-move-to-indent-column
C-M-q		q-indent-current-rule

C-c C-a		q-first-msg
C-c C-b		q-do-buf
C-c C-c		q-do-cmd
C-c C-f		q-find-script
C-c C-h		q-help
C-c C-l		q-run-script
C-c RET		q-run-main
C-c C-n		q-next-msg
C-c C-p		q-prev-msg
C-c C-u		q-current-msg
C-c C-v		q-goto-input-line
C-c C-x		q-quit-eval
C-c C-z		q-last-msg


Entry to this mode calls the value of q-mode-hook if that value is
non-nil.

## Typical use

I noted above, I don't really use that many of these features.

Typically, I would load a .q script into Emacs. Put in a comment
break somewhere in it, that's a forward slash, like this.

/ 

Run the script with `C-c C-l` - say "yes" to the prompt to revert the buffer. It
will execute code up to the comment. Go back to the edit buffer, find
the code after the comment and then step through it with `C-c C-c`.
  
## Customization

`M-x customize-group` can be used to customize the `q` group.

Key to these are the q-prog-name, q-prog-opts and q-prog-args.

There are a great many other options, that are not in use in this
implementation of q-mode.

The font-lock-mode with syntax highlighting doesn't work
anymore. There is a `kdbp-mode' that does that.

The q-gnuclient and q-info methods don't work.

## Controlling Execution

You can also use the local varialbes block to specify how to run the Q interpreter.

```q
/  Local Variables: 
/  mode:q
/  q-prog-args: "last d -p 5016 -t 1000"
/  fill-column: 75
/  comment-column:50
/  comment-start: "/  "
/  comment-end: ""
/  End:
```

## About functions

Although this mode is useful, the step mode `C-c C-c` doesn't support multi-line
function definitions. The q interpreter doesn't have a "paste" mode like Python or
Scala, so if evaluates each line, even when defining a function.

### So

Either make the function a single line,
  
Or put any multi-line function into a separate script and source them with \l.

It is mentioned in the other q-mode by psaris that there is support for that.

# Postamble

I do still use this mode, so I may be able to fix it up if you need a feature.

## History

I mentioned above that there is another q-mode by psaris.

https://github.com/psaris/q-mode

I haven't used this directly. There is another q-mode by little-arhat

https://github.com/little-arhat/q-mode

And I've followed arhat's work, but added some features from psaris. 

arhat used a basic q-minor-mode to interact with the q interpreter and
put the syntax highlighting into kdbp-mode.

I promoted arhat's q-minor-mode to be a major-mode, so there is no
syntax-highlighting.

I used some of psaris' q interpreter interaction in this q-mode.

If you want syntax highlighting, you can still use the kdbp-mode, but
the syntax is out-of-date now.

I've now separated the kdbp-mode I use with this q-mode so that this
q-mode will work on its own.

So the kdbp-mode I use is this one.

https://github.com/eepgwde/kdbp-mode

The notes I made there say that I took it from Alvin Shih, but it
looks very similar to arhat.
