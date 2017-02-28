---
title:  "Vim Setup for Developing React JS"
date:   2017-02-28
categories: Vim React JavaScript Syntastic ESLint
---

# Vim Setup for Developing React JS

I am loving [React](https://facebook.github.io/react/) and I want you to give it a try too.

There was an initial hurdle I had to overcome before I could proficiently work with and not against React. There was the not so small detail of setting up Vim, my code editor of choice. I explored many paths and followed advice from a few different sources, but unfortunately there were a few duds. It's understandable considering the JavaScript landscape can change so frequently these days. Good news! I will tell you the successful path I've found for getting React setup in Vim along with linting via the Syntastic plugin combined with ESLint and JSX syntax highlighting via the vim-jsx plugin. Let's begin.


## Step 1 - Install Syntastic

There is actually a pretty good chance if you're reading this then you already have Syntastic installed. Go ahead and skip to Step 2 then. Otherwise, read on.
Syntastic is a syntax checker for Vim. Any code you have open in Vim can be checked for errors when you save your source file. Better yet, many languages are not just
supported for syntax, they are also supported for checking compile time errors and static analysis. Essentially, if you're used to seeing red squigglies in your commercial, name-brand IDEs, then this is the plugin that enables the same behavior.
Installing it is easy if you have [Pathogen](https://github.com/tpope/vim-pathogen) or [Vundle](https://github.com/VundleVim/Vundle.vim) already setup. For your reference, the complete instructions to install with Pathogen are
[here](https://github.com/vim-syntastic/syntastic#222-step-2-install-syntastic-as-a-pathogen-bundle),  but the steps are really quite simple to follow along below.

<aside class="notice">
If you don't have Pathogen or Vundle, I'll recommend you to use Pathogen. Please note that Pathogen and Vundle are very different tools and it's completely normal and useful to have both. For installing plugins however, either one is capable of assisting you throughout this guide.
</aside>

First, `cd` into your bundle path, like so.

    $ cd ~/.vim/bundle

And initiate a git clone to pull down the syntastic repository and files.

	$ git clone --depth=1 https://github.com/vim-syntastic/syntastic.git

Don't forget to generate the help tags by typing this command in Vim:

    :Helptags

This Vim command effectively installs the features you've recently downloaded via Pathogen. When that is complete, you can then move onto Step 2 to install the ESLint and React plugins for Syntastic to use.


## Step 2 - Install ESLint

<aside class="notice">
Note: The following projects are deprecated so you DO NOT want to use them. I kept finding articles and repos referencing these which were a major source of confusion
in this step.

    1. [JSXHint](https://github.com/strml/jsxhint)
    2. [syntastic-react](https://github.com/jaxbot/syntastic-react)
</aside>

The linter is what let's Syntastic identify compiler issues and highlights additional potential concerns the compiler might not identify. Syntastic doesn't know the
latest EcmaScript features by default, let alone React's JSX syntax that enables the inline XML syntactic sugar. That's why we have to install two libraries
that will be leveraged by Syntastic. Note, these are now NodeJS packages, not Vim plugins, so you will use the `npm` command instead of Vim's Pathogen or Vundle to install the software throughout this step.

The key here is that you want Vim to have linting enabled for any and all of your React projects. Therefore, you want to globally install these packages. If you
also want your build scripts for a specific project to be able to lint your source, say during a CI build, that's fine. You will then want to locally install the
packages for that too. If you're googling and reading online and are not sure whether you want global or local installation methods, then that is the difference here. Globally is what Vim needs, locally is what your build agent needs. You can have both.

All that being said, let's first install `eslint`.

    $ sudo npm -g install eslint

That should install just fine. At the time of this writing, it will be version 3.16.0. (Note: version 1.x.x has different options for the configurations, so always check the version you are installing here).

Now, globally install `eslint-plugin-react`.

    $ sudo npm -g install eslint-plugin-react

Note: on Mac this gave me a `Peer Dependency Unmet` error message. In my experiences, this is a fatal error that shouldn't be ignored. However, apparently it's a "false" warning this time and the recommendation is to ignore it. I don't like it either.


Next, you have to configure the linter manually, by setting up a `.eslintrc` file. Don't worry, I have one all ready for you to use. These rules were
used by jaxbot at his blog [here](https://jaxbot.me/articles/setting-up-vim-for-react-js-jsx-02-03-2015) so send praise his way.

Put the following JSON into `~/.eslintrc` (Note, this is in your home directory because we are configuring the global install).

    {
        "parser": "eslint",
        "env": {
            "browser": true,
            "node": true
        },
        "settings": {
            "ecmascript": 6,
            "jsx": true
        },
        "plugins": [
            "react"
        ],
        "rules": {
            "strict": 0,
            "quotes": 0,
            "no-unused-vars": 0,
            "camelcase": 0,
            "no-underscore-dangle": 0
        }
    }

	
Optionally, you can also globally install `babel-eslint` if you are using edge features of Babel. At the time of this writing, those would specifically be class properties, decorators, and types. ESLint supports ES6 through 8 and JSX by default. If you do use `babel-eslint`, you will have to replace the line `"parser": "eslint"` to `"parser": "babel-eslint"`. Check the official github repo [here](https://github.com/babel/babel-eslint) for more info.


There is one more step that you need to do. Vim, or specifically Syntastic, won't know to use the npm ESLint package yet. In order to set that up, it's
one easy line that goes into anywhere into your `~/.vimrc` file.

    let g:syntastic_javascript_checkers = ['eslint']


Now you can exit and re-open Vim to get "re-source" the .vimrc file. Better yet, here is a trick. You can re-source `.vimrc` by the following command in Vim, without having to leave the editor.

    :so ~/.vimrc

	
That is it for Step 2. You will now have ESLint properly complaining when your .js or .jsx files are hopelessly inadequate. Remember, you can configure the rules at anytime in the `~/.eslintrc` file. The documentation for that is [here](https://github.com/yannickcr/eslint-plugin-react#list-of-supported-rules).


## Step 3 - Install vim-jsx

We have linting executing on filesave now. This is great, but everybody I know loves their editor's colour theme. Black and white is just not going to cut it and heaven help you if you use a high-contrast theme these days, but I digress. This step should seem comparatively easy now that you are an expert at installing Vim plugins. I'll show you the commands for installation using Pathogen again, but feel free to use Vundle if your muse compels you.

What we want from this step ultimately is [vim-jsx](https://github.com/mxw/vim-jsx). However there is a dependency on [vim-javascript](https://github.com/pangloss/vim-javascript), so let's get that to begin with.


First you need to `cd` into your bundle path.

    $ cd ~/.vim/bundle

Then simply `git clone` the repository into your bundles like so.

    $ git clone https://github.com/pangloss/vim-javascript.git


You could do the `:Helptags` now, but let's just sneak in one more package - the much anticipated `vim-jsx`.

    $ git clone https://github.com/mxw/vim-jsx.git


Now, I give you permission to go to town on the helptags. In Vim, type the following command.

    :Helptags


That should configure any .jsx files to show up with pretty colours. That is wonderful, but there is a catch! Many React applications use .js as the de facto file extension now, even if it is JSX syntax. This is ok, because I have the solution.

Please insert the following line anywhere into your `~/.vimrc` file.

    let g:jsx_ext_required = 0


Don't forget to use that neat trick I showed you.

    :so ~/.vimrc


That should cover all your bases. You will now have syntax highlighting and linting enabled in Vim for React projects. 
Please ask me any questions. I'm always happy to help.

