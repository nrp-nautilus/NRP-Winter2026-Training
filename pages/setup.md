---
title: Setup
---

## Prerequisites for NRP Training

Before attending the NRP training sessions, please ensure you have completed the following setup steps.

### 1. NRP Access Requirements

**Institutional Account Access**
- You must have an institutional account with NRP access via Authentik
- NRP access is available to users from US academic institutions or those collaborating with US institutions
- If you don't have access, see [Getting Started with NRP](https://nrp.ai/documentation/userdocs/start/getting-started/)

**Namespace Membership**
- You must be part of at least one namespace to participate in the training
- Check your namespaces at: [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/)
- **Students**: Contact your research supervisor to be added to their namespace
- **Faculty/Researchers**: Request namespace admin status in [Matrix](https://nrp.ai/contact/)

### 2. Install kubectl

The Kubernetes command-line tool, `kubectl`, is required for the training exercises.

#### Linux

Download and install:

~~~bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
~~~

#### macOS

Using Homebrew:

~~~bash
brew install kubectl
~~~

Or download directly:

~~~bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
~~~

#### Windows

Download from: [https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)

### 3. Install kubelogin Plugin

:::danger[Required]
You **must** install the `kubelogin` plugin, or your kubeconfig file **will not work**.
:::

#### macOS

~~~bash
brew install kubelogin
~~~

#### Linux/Windows

Download from: [https://github.com/int128/kubelogin?tab=readme-ov-file#setup](https://github.com/int128/kubelogin?tab=readme-ov-file#setup)

### 4. Download Kubernetes Config File

1. Download the config file from: [https://nrp.ai/config](https://nrp.ai/config)
2. Save it as `config` (without any extension) in your `~/.kube` folder
3. If the folder doesn't exist, create it:

~~~bash
mkdir ~/.kube
~~~

4. The final path should be: `~/.kube/config`

### 5. Cross-Platform kubelogin Fixes

If you run into authentication issues with `kubelogin`, try these fixes:

**Keyring errors** (e.g., `/run/user/1000/bus` not found):
- Add `--token-cache-storage=disk` to store tokens on disk instead of Linux keyring

**Browser issues** (won't launch or opens incorrectly):
- Add `--browser-command="/mnt/c/Program Files/Google/Chrome/Application/chrome.exe"` (Windows WSL: points to Windows Chrome)

**Port binding errors** (`port 8000 already in use`):
- Add `--listen-port=18000` (change to any unused port)

**No local browser available** (remote console):
- Add `--grant-type=device-code --skip-open-browser`

Example config snippet:

~~~yaml
args:
  - oidc-login
  - get-token
  - --browser-command="/mnt/c/Program Files/Google/Chrome/Application/chrome.exe"
  - --listen-port=18000
  - --token-cache-storage=disk
~~~

### 6. Verify Installation

1. **Check kubectl context**:

~~~bash
kubectl config get-contexts
~~~

You should see `nautilus` in the list. If you have multiple contexts, set it:

~~~bash
kubectl config use-context nautilus
~~~

2. **Test authentication**:

~~~bash
kubectl get nodes
~~~

This will open a browser window for authentication via CiLogon.

3. **Verify namespace access**:

~~~bash
kubectl get pods -n <YOUR_NAMESPACE>
~~~

If you see "No resources found", that's okay - it means you have access but there are no pods yet.

4. **Set default namespace** (optional):

~~~bash
kubectl config set-context nautilus --namespace <YOUR_NAMESPACE>
~~~

### 7. Training Environment

**For Live Training:**
- The training will be conducted using NRP's JupyterHub: `jupyterhub-west.nrp-nautilus.io`
- You'll use this as your local environment during the session
- All commands can be copy-pasted from the training notebooks

**For Self-Paced Learning:**
- Use your local environment with `kubectl` configured as above
- Follow the training notebooks: `Beginner_Track_Training.ipynb` and `Intermediate_Track_Training.ipynb`

## Getting Help

If you encounter issues during setup:

- **Matrix**: [Join NRP's Matrix](https://nrp.ai/contact/) for community support
- **Email**: usersupport@nrp-nautilus.io
- **Documentation**: [NRP Getting Started Guide](https://nrp.ai/documentation/userdocs/start/getting-started/)

## Additional Resources

- [NRP Portal](https://nrp.ai)
- [NRP Documentation](https://nrp.ai/documentation/)
- [Namespace Management](https://nrp.ai/namespaces/) 


## Setup for local rendering of the lessons (optional)

Though not essential, it is desirable to be able to preview changes on your own machine
before pushing them to GitHub.

In order to preview changes locally, you must install the software described below.

If you don't install bundler as indicated in this section, you will not be able to preview the
lessons locally (in other words, you won't be able to run `make serve` or `make site`).
However, you can still edit the files that make up the lessons. You will only be able to see the
changes once your edits have been merged in the main repository.

### Windows

For Windows, there are two main ways to setup your system to be able to render the lessons.

- Option 1 relies on the Windows Subsystem for Linux (WSL). WSL allows you to run a Linux
  environment directly from Windows.
- Option 2 relies on using Windows built-in applications.

> ## Option 1 - Using the Windows Subsystem for Linux (WSL)
>
> If your version of Windows supports it, using the WSL will make the installation of the tools
> needed easier. Instructions to install Linux distributions from Windows 10/Windows Server are
> available from the [Microsoft website](https://docs.microsoft.com/en-us/windows/wsl/about).
>
> Once you have installed a Linux distribution, you can follow the installation instructions for
> [Linux](#linux-ubuntu) listed below. If you install a distribution other than Ubuntu, you will
> need to adjust the commands that install the packages.
{: .solution}

> ## Option 2 - Using Windows built-in applications
>
> With the instructions below, you'll be able to preview websites for non-R based lessons. You'll be
> able to do so from the Git Bash terminal or from the "Command Prompt with Ruby" that comes with
> the Ruby installation. You won't be able to use the `make` commands as Make isn't available from
> the Git Bash terminal (see the optional section below for more info).
>
> 1. **Ruby**, go to <https://rubyinstaller.org/> choose Ruby+DevKit for your
>   system (typically x64), and the most recent version available. During the
>   installation process, choose to install the MSYS2 toolchain. On the last step
>   of the installation screen, make sure that "Run 'ridk install'" is checked.
>   This will bring a terminal window with 3 options, press "Enter" (for the
>   default installation). After this step completes, you'll be prompted again,
>   and press "Enter" again (for the default option). Once the installation is
>   complete, restart your system.
>
>2. Navigate to the folder that contains the lesson, and you can now use `bundle
>   exec jekyll serve` from your Git Bash terminal to preview the lessons.
>
> ### Optional (but recommended)
>
> With these instructions, you'll be able to use the `make` commands and render all lessons
> including those that use R. However, you won't be able to do so from the Git Bash terminal, but
> from the other terminals (Windows Powershell, cmd.exe, or the Command Prompt with Ruby).
>
>1. In the File Explorer, right-click on "This PC" icon, and click on
>   "Properties". Click on "Advanced System Settings", and click on the button
>   "Environment Variables". Click on the variable "Path", and then the "Edit"
>   button. Click on the "New" button and add `C:\Ruby26-x64\msys64\usr\bin` and click "OK".
>   Click "New" again and add `C:\Ruby26-x64\bin` and click "OK" again. (use
>   the File Explorer to make sure this is the correct path for your Ruby and
>   MSYS2 installation). If you're working on R-based lessons and R isn't already
>   listed there, you need to add it by adding `C:\Program Files\R\R-3.x.x\bin`
>   (using the correct path and R version number for your installation).
>
>2. Open the "Command Prompt with Ruby" (if it was open before you edited the
>   Path, close it and re-open it).
{: .solution}

Regardless of the option you chose, go to the section [For Everyone](#for-everyone).

### macOS

1. First make sure you have Homebrew installed

    To install Homebrew, open the [Homebrew website](https://brew.sh/) and copy/paste the first command 
    on that page into your Terminal. You may also want to check the requirements first. The command for
    installing Homebrew will look something like:

      ~~~
      /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
      ~~~
      {: .language-bash}

      If you're not sure whether you have brew installed, type

      ~~~
      brew help
      ~~~
      {: .language-bash}

      If you have Homebrew installed, you should see something like:

      ~~~
      Example usage:
      brew search [TEXT|/REGEX/]
      brew info [FORMULA...]
      brew install FORMULA...
      brew update
      brew upgrade [FORMULA...]
      brew uninstall FORMULA...
      brew list [FORMULA...]

    Troubleshooting:
      brew config
      brew doctor
      brew install --verbose --debug FORMULA

    Contributing:
      brew create [URL [--no-fetch]]
      brew edit [FORMULA...]

    Further help:
      brew commands
      brew help [COMMAND]
      man brew
      https://docs.brew.sh
      ~~~
      {: .output}


2. **Ruby** -- `brew install ruby`

3. **bundler** -- `gem install bundler --user-install`

4. Go to the section [For Everyone](#for-everyone)

### Linux (Ubuntu)

Currently, GitHub pages uses Jekyll 3.9.0 which is not compatible with version of Ruby >= 3.
Previously, the version of Ruby that was provided by your system package manager were older than
Ruby 3 and could be used. As more distributions are upgrading the version of Ruby they provide, it
becomes more difficult to rely on the version of Ruby they provide. Instead, we recommend that you
use [rbenv](https://github.com/rbenv/rbenv) (an utility that allows you to have and use different
version of Ruby on your system) installed with [homebrew](https://brew.sh/). Using homebrew instead
of compiling ruby from source with rbenv allievates complications with dependencies between your
operating system and specific versions of Ruby.

1. Install Homebrew:
   
   ~~~
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ~~~
   {: .language-bash}

2. Install rbenv and ruby-build using homebrew:

   ~~~
   brew install rbenv ruby-build
   ~~~
   {: .language-bash}
   
3. Set up rbenv:

   ~~~
   rbenv init
   ~~~
   {: .language-bash}
   
4. Install the Ruby version needed for the lessons:

   ~~~
   rbenv install 2.7.3
   ~~~
   {: .language-bash}
   
5. Inside the directory for the lesson, check that you are using Ruby 2.7.3 by typing `rbenv
   version`, if this commands does not return 2.7.3, type:

   ~~~
   rbenv local 2.7.3
   rbenv shell 2.7.3
   ~~~
   {: .language-bash}

6. Install bundler with:

   ~~~
   gem install bundler
   ~~~
   {: .language-bash}
   
   You should not need to use `sudo` to install gems as they should get installed within your home
   directory. To check where the gems will be installed type:
   
   ~~~
   gem env home
   ~~~
   {: .language-bash}
   
   This command should show you a path to your home directory.
   
   `gem` is the package management framework for Ruby. It was installed as part
    of Ruby in the step above. `bundler` is also a package manager but at the
    scale of a project instead of being system-wide. It makes it easier to
    manage dependencies.

7. Clean up. To make sure that the gems installed are compatible with Ruby 2.7.3 and to avoid issues
   with other versions that you might have used in the past, run:

   ~~~
   make clean
   ~~~
   {: .language-bash}
   

### For Everyone

1. **The GitHub Pages Ruby Gem**

    Make sure there is a `Gemfile` at the root of your lesson repository. The content of this file
    should match the [`Gemfile`](https://github.com/carpentries/styles/blob/gh-pages/Gemfile) found
    in the styles repository. 
        
2. **Generate the lesson**

    Now you are ready to run jekyll to build your website and run a local server. To do this run:

    ~~~
    make serve
    ~~~
    {: .language-bash}

    There will be several lines of output after this. If there were errors or warnings you can use
    them to fix your lesson and run again if it failed. If it was successful the last few lines
    will include `Server address: http://127.0.0.1:4000` and `Server running... press ctrl-c to
    stop`. You can see your rendered site by pointing your browser to the address shown.

### For R-based lessons

You will need a recent version of R (>= 3.5.0). Installation instructions are available from the
[CRAN website](https://cran.r-project.org).

We use the [knitr][cran-knitr], and [remotes](https://cran.r-project.org/package=remotes) to format
lessons written in R Markdown and figure out needed packages to execute the code in the lesson. You
will need to install the `remotes` package to build R lessons (and this example lesson), the other
packages will be installed automatically during the rendering of the lesson. You can install this
package by opening an R terminal and typing:

~~~
install.packages('remotes', repos = 'https://cran.rstudio.com')
~~~
{: .language-r}

### Troubleshooting

1. Check your version of Ruby:

   ~~~
   ruby -v
   ~~~
   {: .language-bash}

   You need Ruby 2.1.0 or later (currently GitHub pages uses Ruby 2.7.1). If you
   have an older version of Ruby, if possible upgrade your operating system to a
   more recent version. If it's not possible, consider using 
   [rbenv](https://github.com/rbenv/rbenv).

    ~~~
    rbenv install 2.7.1
    ~~~
    {: .language-bash}

    And then instructing `rbenv` to use it in your lesson development process by
    executing the following command from your lesson directory:

    ~~~
    rbenv local 2.7.1
    ~~~
    {: .language-bash}

2.  **[RubyGems][rubygems]**
    is a tool which manages Ruby packages. It should have been installed along with Ruby and you can
    test your installation by running

    ~~~
    gem --version
    ~~~
    {: .language-bash}

3. If you want to run `bin/lesson_check.py` (which is invoked by `make lesson-check`)
you will need the [PyYAML][pyyaml] module for Python 3.


## Creating a New Lesson

We will assume that your user ID is `timtomch` and the name of your
new lesson is `data-cleanup`.

We recommend using the GitHub Importer whenever possible but sometimes users
experience problems with the Importer failing to correctly import repositories.
On previous occasions, the issue has been solved quite quickly so we recommend
that you wait 24 hours before trying to import the repository again. If you
can't wait and are familiar with using Git on the command line, [follow the
alternative instructions below](#creating-a-new-lesson-using-git-commands) to set up your new
repository. If using command line git is also not an option for you, please reach out to
[team@carpentries.org](mailto:team@carpentries.org) for help.

If you are looking to create a lesson intended for [The Carpentries
Incubator](https://github.com/carpentries-incubator), there is a dedicated template and instructions
included in the [repository](https://github.com/carpentries-incubator/template).


1.  We'll use the [GitHub's importer][importer] to make a copy of this repo in your own GitHub
    account. (Note: This is like a GitHub Fork, but not connected to the upstream changes)

2.  Put the URL of **[the styles repository][styles]**, that is
    **https://github.com/carpentries/styles** in the "Your old repository’s clone URL" box.
    Do not use the URL of this repository,
    as that will bring in a lot of example files you don't actually want.

3.  Select the owner for your new repository.
    In our example this is `timtomch`,
    but it may instead be an organization you belong to.

4.  Choose a name for your lesson repository.
    In our example, this is `data-cleanup`.

5.  Make sure the repository is public.

6.  At this point, you should have a page like this:

    ![Screenshot of web form that says "Import your project to GitHub" showing two fields with "Your old repositoy's clone URL" set to "carpentries/styles" and "Your new repository details" set to "timtomch" for the owner and "data-cleanup" for the name]({{ page.root }}/fig/using-github-import.png)

    You can now click "Begin Import". When the process is done, you can click
    "Continue to repository" to visit your newly-created repository.

7.  If you want to work on the lesson from your local machine, you can
    now clone your newly-created repository to your computer:

    ~~~
    $ git clone -b gh-pages https://github.com/timtomch/data-cleanup.git
    ~~~
    {: .language-bash}

    Note that the URL for your lesson will have your username and chosen repository name.

8.  Go into that directory using:

    ~~~
    $ cd data-cleanup
    ~~~
    {: .language-bash}

    Note that the name of your directory should be what you named your lesson
    on the example this is `data-cleanup`.

9. To be able to pull upstream style changes, you should manually add the
     styles repository as a remote called `template`:

    ~~~
    $ git remote add template https://github.com/carpentries/styles.git
    ~~~
    {: .language-bash}

    This will allow you to pull in changes made to the template,
    such as improvements to our CSS style files.
    (Note that the user name above is `carpentries`, *not* `timtomch`,
    since you are adding the master copy of the template as a remote.)

10. Configure the `template` remote to not download tags:

    ~~~
    $ git config --local remote.template.tagOpt --no-tags
    ~~~
    {: .language-bash}

10. Make sure you are using the `gh-pages` branch of the lesson template:

    ~~~
    $ git checkout gh-pages
    ~~~
    {: .language-bash}

	This will ensure that you are using the most "stable" version of the
	template repository. Since it's being actively maintained by the
	Software Carpentry community, you could end up using a development branch
	that contains experimental (and potentially not working) features without
	necessarily realising it. Switching to the `gh-branch` ensures you are
	using the "stable" version of the template.

11. Run `bin/lesson_initialize.py` to create all of the boilerplate files
    that cannot be put into the styles repository
    (because they would trigger repeated merge conflicts).

12. Create and edit files as explained further in
    [the episodes of this lesson]({{ page.root }}{% link index.md %}#schedule).

13. (requires Jekyll Setup from below) Preview the HTML pages for your lesson:

    ~~~
    $ make serve
    ~~~
    {: .language-bash}

    Alternatively, you can try using Docker:

    ~~~
    $ make docker-serve
    ~~~
    {: .language-bash}

14. Commit your changes and push to the `gh-pages` branch of your
    repository:

    ~~~
    $ cd data-cleanup
    $ git add changed-file.md
    $ git commit -m "Explanatory message"
    $ git push origin gh-pages
    ~~~
    {: .language-bash}


> ## Creating a New Lesson using Git commands
>
> If the GitHub Importer is failing, you can instead follow the instructions
> below that use Git commands to achieve the same result. We'll use Git to make
> a copy of this repo in your own GitHub account. The effect is like a GitHub
> fork, but not connected to the upstream changes.
>
> 1.  Create an empty repo on GitHub, using [GitHub's new repository tool](new)
>     to go to the  the "Create new repository" page.
>
> 2.  Select the owner for your new repository.
>     In our example this is `timtomch`,
>     but it may instead be an organization you belong to.
>
> 3.  Choose a name for your lesson repository.
>     In our example, this is `data-cleanup`.
>
> 5.  Make sure the repository is public. Don't initialize any files,
>     i.e. leave boxes unchecked at the bottom of that page.
>     Then, click on the green "Create repository" button to create the repository.
>
> 6.  Clone the **[the styles repository][styles]**, that is
>     **https://github.com/carpentries/styles** to a local directory called `data-cleanup`,
>     using a bash shell to run the command:
>     ~~~
>     $ git clone -b gh-pages https://github.com/carpentries/styles.git data-cleanup
>     ~~~
>     {: .language-bash}
>     Do not use the URL of *this* repository (https://github.com/carpentries/lesson-example),
>     as that will bring in a lot of example files you don't actually want.
>
> 7.  Go into your new local directory using:
>
>     ~~~
>     $ cd data-cleanup
>     ~~~
>     {: .language-bash}
>
>     Note that in this and the previous step, the name of your directory should
>     be what you named your lesson; on the example this is `data-cleanup`.
>
> 8.  Link the local directory you just created with the new Github repository you created,
>
>     ~~~
>     $ git remote set-url origin https://github.com/timtomch/data-cleanup.git
>     $ git push origin gh-pages
>     ~~~
>     {: .language-bash}
>
>     Again, the name of the remote should be `<username>/<mylesson>`, your username
>     followed by what you named your lesson; on the example this is
>     `timtomch/data-cleanup`. You can check that this has worked by refreshing the webpage from
      > step 5, e.g. `https://github.com/timtomch/data-cleanup`.
>
> 9. To be able to pull upstream style changes, you should manually add the
>      styles repository as a remote called `template`:
>
>     ~~~
>     $ git remote add template https://github.com/carpentries/styles.git
>     ~~~
>     {: .language-bash}
>
>     This will allow you to pull in changes made to the template,
>     such as improvements to our CSS style files.
>     (Note that the user name above is `carpentries`, *not* `timtomch`,
>     since you are adding the master copy of the template as a remote.)
>
> 10. Configure the `template` remote to not download tags:
>
>     ~~~
>     $ git config --local remote.template.tagOpt --no-tags
>     ~~~
>     {: .language-bash}
>
> 10. Make sure you are using the `gh-pages` branch of the lesson template:
>
>     ~~~
>     $ git checkout gh-pages
>     ~~~
>     {: .language-bash}
>
>     This will ensure that you are using the most "stable" version of the
>     template repository. Since it's being actively maintained by The
>     Carpentries community, you could end up using a development branch
>     that contains experimental (and potentially not working) features without
>     necessarily realising it. Switching to the `gh-pages` branch ensures you are
>     using the "stable" version of the template.
>
> 11. Create all of the boilerplate files that cannot be put into the styles  X
>     repository
>     (because they would trigger repeated merge conflicts), by running:
>     ~~~
>     $ python bin/lesson_initialize.py
>     ~~~
>     {: .language-bash}
>
>     Edit `_config.yml` and `index.md` for the lesson you are creating.
>
> 12. Create and edit files as explained further in
>     [the episodes of this lesson]({{ page.root }}{% link index.md %}#schedule).
>
> 13. (requires Jekyll Setup from above) Preview the HTML pages for your lesson:
>
>     ~~~
>     $ make serve
>     ~~~
>     {: .language-bash}
>
>     Alternatively, you can try using Docker:
>
>     ~~~
>     $ make docker-serve
>     ~~~
>     {: .language-bash}
>
> 14. Commit your changes and push to the `gh-pages` branch of your
>     repository:
>
>     ~~~
>     $ cd data-cleanup
>     $ git add changed-file.md
>     $ git commit -m "Explanatory message"
>     $ git push origin gh-pages
>     ~~~
>     {: .language-bash}
{: .solution}

## Notes

1.  SSH cloning (rather than the HTTPS cloning used above)
    will also work for those who have set up SSH keys with GitHub.

2.  Once a lesson has been created, please submit changes
    for review as pull requests that contain Markdown files only.

3.  Some people have had intermittent errors during the import process,
    possibly because of the network timing out.
    If you experience a problem, please re-try;
    if the problem persists,
    use the instructions above to
    [import the repository     manually](#creating-a-new-lesson-using-git-commands) using Git
    commands, otherwise, please [get in touch][email].


## Setup Instructions for a specific existing lesson

1.  Installation instructions for core lessons are included in
    the [workshop template's home page][workshop-repo],
    so that they are all in one place.
    The `setup.md` files of core lessons link to
    the appropriate sections of the [workshop template page][workshop-repo].

2.  Other lessons' `setup.md` include full installation instructions organized by OS
    (following the model of the workshop template home page).



{% include links.md %}
